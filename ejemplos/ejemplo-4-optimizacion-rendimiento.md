# Ejemplo 4: Optimización de Rendimiento

## 📋 Contexto

**Sistema:** Plataforma de Analytics Real-Time
**Problema:** Dashboards tarden 30 segundos en cargar
**Meta:** <2 segundos en p95

## 🎯 Desafío

Los dashboards:

- Ejecutan 50+ queries
- N+1 queries severo
- Sin caché
- Sin índices apropiados
- Base de datos normalizada (muchos JOINs)

## 📝 Prompt Usado

Eres experto en optimización de rendimiento. Tu tarea es ayudar a
optimizar un sistema lento.

Stack:

Backend: Node.js/Express
Base de datos: PostgreSQL 13
Cache: Redis (instalado pero no usado)
Frontend: React

Problema:

Dashboards tardan 30 segundos en cargar
p95 latencia: 30seg
p99 latencia: 60seg
Servidor 8 CPU, 32GB RAM

Queries típicas:
[Ejemplos de queries lentas]

Restricciones:

No puedo cambiar schema (datos críticos)
No puedo reescribir completo
Necesito mejoras rápidas

Por favor proporciona:

Análisis de bottlenecks
Optimizaciones quick wins
Plan de optimización
Métricas de éxito

## 💡 Estrategia de Optimización

### Fase 1: Diagnóstico (Día 1)

**Herramientas:**

```bash
# PostgreSQL query analysis
EXPLAIN ANALYZE SELECT * FROM orders WHERE user_id = 1;

# Node.js profiling
node --prof app.js
node --prof-process isolate-*.log | head -20

# Redis monitoring
redis-cli MONITOR

# Load testing
wrk -t4 -c100 -d30s http://localhost:3000/api/dashboard
```

**Resultados:**

Problema 1: Query de 50 operaciones secuenciales
Estimado tiempo: 50 * 50ms = 2.5 segundos (sólo queries)

Problema 2: N+1 queries severo
Consulta usuarios: 1 query
Consulta órdenes por usuario: 1000 queries
Total: 1001 queries en lugar de 1

Problema 3: Sin índices en tablas grandes
Escanea 10M filas en lugar de 10k

Problema 4: Sin caché
Cada usuario request ejecuta todas las queries


### Fase 2: Quick Wins (Semana 1)

**#1: Agregar Índices**

```sql
-- Encontrar queries lentas
SELECT query, calls, mean_time 
FROM pg_stat_statements 
ORDER BY mean_time DESC 
LIMIT 10;

-- Crear índices
CREATE INDEX idx_orders_user_id ON orders(user_id);
CREATE INDEX idx_orders_created_at ON orders(created_at DESC);
CREATE INDEX idx_users_email ON users(email);

-- Verificar uso de índices
EXPLAIN ANALYZE SELECT * FROM orders WHERE user_id = 123;
-- Debe mostrar "Index Scan" en lugar de "Seq Scan"

Resultado: -80% en tiempo de queries
```

**#2: Batch Queries con JOIN**

```javascript
// Antes (N+1):
const users = await db.query('SELECT * FROM users LIMIT 100');
const orders = await Promise.all(
  users.map(user => db.query('SELECT * FROM orders WHERE user_id = ?', [user.id]))
);
// 101 queries total

// Después (batch):
const users = await db.query('SELECT * FROM users LIMIT 100');
const userIds = users.map(u => u.id);
const orders = await db.query(
  'SELECT * FROM orders WHERE user_id = ANY(?)',
  [userIds]
);
// 2 queries total

Resultado: -95% en número de queries
```

**#3: Agregar Caché Redis**

```javascript
// Caché de 5 minutos para dashboard
const dashboardKey = `dashboard:${userId}`;
const cached = await redis.get(dashboardKey);

if (cached) {
  return JSON.parse(cached);
}

// Si no está en caché, calcular
const data = await calculateDashboard(userId);

// Guardar en caché
await redis.setex(dashboardKey, 300, JSON.stringify(data));

return data;

Resultado: -90% en tiempo para usuarios repetidos
```

**#4: Select Solo Columnas Necesarias**

```javascript
// Antes:
const data = await db.query('SELECT * FROM orders WHERE user_id = ?', [123]);

// Después:
const data = await db.query(
  'SELECT id, date, amount, status FROM orders WHERE user_id = ?',
  [123]
);
// Menos datos traídos del disco, menos deserialization

Resultado: -20% en tiempo de transferencia
```

**Resultados después de Fase 1:**

Antes: 30,000 ms (30 segundos)
Después: 2,500 ms (2.5 segundos)
Mejora: 92% reducción


### Fase 3: Optimizaciones Medianas (Semana 2-3)

**#5: Materialized Views**

```sql
-- Vista materializada para agregaciones pesadas
CREATE MATERIALIZED VIEW daily_sales_summary AS
SELECT 
  date_trunc('day', created_at)::date as day,
  product_id,
  SUM(quantity) as total_qty,
  SUM(amount) as total_amount,
  COUNT(*) as transaction_count
FROM orders
GROUP BY date_trunc('day', created_at), product_id;

-- Crear índice en la vista
CREATE INDEX idx_daily_sales_product ON daily_sales_summary(product_id);

-- Refresh periódicamente
REFRESH MATERIALIZED VIEW CONCURRENTLY daily_sales_summary;

-- Usar en queries
SELECT * FROM daily_sales_summary WHERE day = '2026-01-15';

Resultado: -95% en tiempo de aggregaciones
```

**#6: Database Connection Pooling**

```javascript
// Antes: Crear conexión por request (lento)
const connection = await postgres.connect();

// Después: Usar pool de conexiones reutilizables
const pool = new Pool({
  max: 20,  // máximo de conexiones
  idleTimeoutMillis: 30000,
  connectionTimeoutMillis: 2000,
});

const result = await pool.query('SELECT * FROM users WHERE id = $1', [123]);

Resultado: -40% latencia, mejor throughput
```

**#7: Read Replicas (PostgreSQL)**

```javascript
// Setup:
// Servidor primario: writes
// 2 servidores replica: reads

// En código:
const writePool = new Pool({host: 'primary.db', ...});
const readPool = new Pool({host: 'replica1.db', ...});

// Dashboards usan read replica
const dashboardData = await readPool.query('SELECT ...');

// Writes en primario
await writePool.query('INSERT INTO orders ...');

Resultado: -70% latencia para lectura, escalabilidad
```

### Fase 4: Arquitectura (Mes 2)

**#8: ElasticSearch para Búsqueda**

```javascript
// Antes: SQL LIKE query (lento)
const results = await db.query(
  `SELECT * FROM products WHERE name ILIKE ?`,
  ['%laptop%']
);
// 2 segundos para 1M productos

// Después: ElasticSearch (rápido)
const results = await elasticsearch.search({
  index: 'products',
  body: {
    query: {
      match: { name: 'laptop' }
    }
  }
});
// 50ms

Resultado: -97% en tiempo de búsqueda
```

**#9: Message Queue para Procesamiento Asincrónico**

```javascript
// Antes: Procesar de forma sincrónica (bloquea)
app.post('/api/orders', async (req, res) => {
  const order = await Order.create(req.body);
  
  // Esto bloquea:
  await sendEmail(order);
  await updateAnalytics(order);
  await notifyWarehouse(order);
  
  res.json(order); // Tarda 5 segundos en responder
});

// Después: Queue (respuesta rápida)
app.post('/api/orders', async (req, res) => {
  const order = await Order.create(req.body);
  
  // Encolar para procesamiento asincrónico
  await queue.enqueue('send-email', {orderId: order.id});
  await queue.enqueue('update-analytics', {orderId: order.id});
  await queue.enqueue('notify-warehouse', {orderId: order.id});
  
  res.json(order); // Responde en 100ms
});

// Worker procesa en background
queue.process('send-email', async (job) => {
  const order = await Order.find(job.data.orderId);
  await sendEmail(order);
});

Resultado: -95% latencia en API, mejor UX
```

### Resultados Finales

Métrica Antes Después Mejora
─────────────────────────────────────────
p50 latencia 15seg 500ms 97%
p95 latencia 30seg 1.5seg 95%
p99 latencia 60seg 3seg 95%
Queries/request 51 3 94%
Cache hit rate 0% 85% ∞
DB connections 500 50 90%
CPU usage 90% 30% 67%
RAM usage 28GB 8GB 71%

User Experience:

Dashboard loads instantly (feels snappy)
No spinning loaders
Smooth interactions
Happy customers ✓

## 🔗 Recursos

- [Use The Index, Luke!](https://use-the-index-luke.com/) - SQL performance
- [PostgreSQL Performance Tuning](https://www.postgresql.org/docs/current/performance-tips.html)
- [Node.js Performance Best Practices](https://nodejs.org/en/docs/guides/nodejs-performance/)

---

**Aplicable a:** Cualquier sistema lento
**Complejidad:** Intermedio
**Timeline típico:** 4-6 semanas para mejoras principales
