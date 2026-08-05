# Ejemplo 2: Diseño de API REST Profesional

## 📋 Contexto

**Sistema:** API de Pagos para Marketplace
**Audiencia:** Aplicaciones web y mobile
**Escala:** 1k requests/segundo en pico
**Seguridad:** PCI-DSS compliance requerido

## 🎯 Problema

Necesitas diseñar una API que:

- Sea segura (manejo de datos de tarjeta de crédito)
- Sea escalable (1k req/seg)
- Sea fácil de usar (buena DX)
- Tenga versionado claro
- Permita rate limiting sin afectar usuarios legítimos

## 📝 Prompt Usado

Eres experto en diseño de APIs. Tu tarea es ayudar a diseñar una API
robusta y escalable.

Tipo de API: REST
Propósito: Procesar pagos en marketplace
Clientes: Web app (React), Mobile app (iOS/Android), Backend partners

Recursos Principales:

Payments (transacciones)
Refunds (devoluciones)
Webhooks (notificaciones en tiempo real)
Merchants (información de vendedor)

Operaciones Principales:

Crear pago
Obtener estado de pago
Procesar reembolso
Listar transacciones
Configurar webhooks

Requisitos:

Autenticación: JWT tokens
Rate Limiting: Sí (100 req/min por usuario)
Versionado: URL (/v1/, /v2/)
Formato: JSON
Paginación: Cursor-based

Casos de Uso:

Checkout rápido (checkout page incrustada)
Pagos recurring (suscripciones)
Webhooks para confirmación (en tiempo real)
Análisis de transacciones

Restricciones:

PCI-DSS compliance
Latencia máxima: 500ms
Disponibilidad: 99.99%
Clientes esperados: 500+ merchants

Por favor proporciona:

Estructura de endpoints
Esquemas de request/response
Códigos de error
Rate limiting strategy
Seguridad (authentication, encryption)
Ejemplos de curl

## 💡 Salida Esperada

### Endpoints de API

PAGOS:
POST /v1/payments → Crear pago
GET /v1/payments/{id} → Obtener detalles
GET /v1/payments → Listar pagos (paginado)
PUT /v1/payments/{id} → Actualizar pago

REEMBOLSOS:
POST /v1/payments/{id}/refunds → Crear reembolso
GET /v1/refunds/{id} → Obtener reembolso

WEBHOOKS:
POST /v1/webhooks → Registrar webhook
GET /v1/webhooks → Listar webhooks
DELETE /v1/webhooks/{id} → Eliminar webhook

### Esquemas de Request/Response

**Crear Pago (POST /v1/payments):**

```json
REQUEST:
{
  "amount": 9999,              // En centavos (99.99 USD)
  "currency": "USD",
  "description": "Compra en marketplace",
  "payment_method": {
    "type": "card",
    "card": {
      "number": "4111111111111111",
      "exp_month": 12,
      "exp_year": 2025,
      "cvc": "123"
    }
  },
  "metadata": {
    "order_id": "ord_123456",
    "user_id": "usr_789"
  }
}

RESPONSE (200):
{
  "id": "pay_1234567890",
  "status": "succeeded",
  "amount": 9999,
  "currency": "USD",
  "description": "Compra en marketplace",
  "created": "2026-01-15T10:30:00Z",
  "metadata": {
    "order_id": "ord_123456",
    "user_id": "usr_789"
  }
}

RESPONSE (400 - Error):
{
  "error": {
    "code": "card_declined",
    "message": "La tarjeta fue rechazada",
    "param": "payment_method.card"
  }
}
```

**Obtener Pago (GET /v1/payments/pay_1234567890):**

```json
RESPONSE (200):
{
  "id": "pay_1234567890",
  "status": "succeeded",
  "amount": 9999,
  "currency": "USD",
  "description": "Compra en marketplace",
  "payment_method": {
    "type": "card",
    "card": {
      "last4": "1111",
      "brand": "visa",
      "exp_month": 12,
      "exp_year": 2025
    }
  },
  "created": "2026-01-15T10:30:00Z",
  "updated": "2026-01-15T10:30:00Z"
}
```

**Listar Pagos (GET /v1/payments?limit=10&starting_after=pay_1234567890):**

```json
RESPONSE (200):
{
  "object": "list",
  "data": [
    {
      "id": "pay_2234567890",
      "status": "succeeded",
      "amount": 5000,
      "currency": "USD",
      "created": "2026-01-15T11:00:00Z"
    },
    {
      "id": "pay_1234567890",
      "status": "succeeded",
      "amount": 9999,
      "currency": "USD",
      "created": "2026-01-15T10:30:00Z"
    }
  ],
  "has_more": true,
  "url": "/v1/payments"
}
```

### Códigos de Error

200 OK → Operación exitosa
400 Bad Request → Parámetros inválidos
401 Unauthorized → Token inválido/faltante
402 Payment Required → Tarjeta rechazada
403 Forbidden → Permisos insuficientes
404 Not Found → Recurso no existe
409 Conflict → Estado inválido para operación
429 Too Many Requests → Rate limit excedido
500 Server Error → Error interno

### Autenticación

**Bearer Token (JWT):**

Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...

**API Key (alternativa simple):**

Authorization: Bearer sk_live_1234567890abcdef

### Rate Limiting

Headers de respuesta:
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 47
X-RateLimit-Reset: 1642262400

Límites:

100 requests/minuto por token
1000 requests/minuto por merchant
10000 requests/minuto total

### Seguridad

**HTTPS obligatorio:**

Todo tráfico debe ser HTTPS con TLS 1.2+

**Encriptación de datos sensibles:**
Números de tarjeta: nunca logs ni storage directo
CVV: procesar solo con proveedores certificados
Datos en tránsito: encriptados
Datos en reposo: encriptados

**Validación de webhook:**

Cada webhook incluye:
X-Webhook-Signature: sha256=hmac_signature

Verificar:
signature = hmac_sha256(webhook_body, webhook_secret)

### Ejemplo de Uso (cURL)

**Crear Pago:**

```bash
curl -X POST https://api.payments.com/v1/payments \
  -H "Authorization: Bearer sk_live_1234567890" \
  -H "Content-Type: application/json" \
  -d '{
    "amount": 9999,
    "currency": "USD",
    "description": "Compra en marketplace",
    "payment_method": {
      "type": "card",
      "card": {
        "number": "4111111111111111",
        "exp_month": 12,
        "exp_year": 2025,
        "cvc": "123"
      }
    }
  }'
```

**Obtener Pago:**

```bash
curl https://api.payments.com/v1/payments/pay_1234567890 \
  -H "Authorization: Bearer sk_live_1234567890"
```

**Procesar Reembolso:**

```bash
curl -X POST https://api.payments.com/v1/payments/pay_1234567890/refunds \
  -H "Authorization: Bearer sk_live_1234567890" \
  -H "Content-Type: application/json" \
  -d '{
    "amount": 5000,
    "reason": "customer_request"
  }'
```

### Webhooks Soportados

payment.succeeded
→ Enviado cuando pago es procesado exitosamente
→ Usa para actualizar inventario, enviar confirmación

payment.failed
→ Enviado cuando pago falla
→ Usa para notificar al usuario

refund.created
→ Enviado cuando se inicia un reembolso
→ Usa para auditoría, ajustes contables

payment.disputed
→ Enviado cuando hay disputa (chargeback)
→ Usa para investigación, contacto con cliente


### Versionado

URL Versioning:
/v1/payments ← API versión 1
/v2/payments ← API versión 2 (cambios no backward-compatible)

Política de deprecación:

Anuncia deprecation 6 meses antes
Soporta ambas versiones durante transición
Migramos gradualmente a clientes

## 📊 Lecciones Aprendidas

### Lo Que Funcionó Bien

✅ Versionado claro desde el inicio
✅ Rate limiting flexible (no afecta usuarios legítimos)
✅ Webhooks para notificaciones real-time
✅ Errores claros con códigos específicos

### Lo Que Fue Difícil

❌ Idempotencia (pago duplicado por retry)
❌ Manejo de reembolsos parciales
❌ Sincronización de estado en caso de error
❌ Documentación mantener al día

## 🔗 Recursos

- [Stripe API Design](https://stripe.com/docs/api)
- [Adyen API](https://docs.adyen.com/)
- [RESTful API Guidelines](https://google.aip.dev/111)

---

**Aplicable a:** Cualquier API financiera o crítica
**Complejidad:** Intermedio-Avanzado
**Timeline típico:** 4-6 semanas diseño, 8-12 semanas implementación
