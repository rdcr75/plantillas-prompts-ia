# Categoría: Depuración

Prompts y técnicas para debuggear problemas complejos.

## Prompts en Esta Categoría

### 1. Asistente de Depuración

**Archivo:** `../prompts/asistente-depuracion.md`
**Uso:** Debugging estructurado
**Complejidad:** Intermedio
**Tiempo:** 15-30 minutos
**Resultado:** Causa raíz, soluciones

## Metodología de Debugging

### 1. Reproducción

- Entiende cuándo ocurre
- Documenta pasos exactos
- Reproduce en ambiente controlado
- Identifica frecuencia

### 2. Aislamiento

- Reduce variables
- Prueba componentes aislados
- Usa git bisect si es necesario
- Identifica última versión buena

### 3. Diagnóstico

- Revisa logs
- Usa herramientas de profiling
- Inspecciona estado
- Proporciona hipótesis

### 4. Solución

- Prueba arreglo
- Valida que soluciona
- Verifica no introduce bugs
- Documenta solución

### 5. Prevención

- Agrega tests
- Mejora logging
- Documenta (para otros)
- Configura alertas

## Herramientas Útiles

### Logging

- Nivel de log apropiado
- Información contextual
- Timestamps
- Stack traces completo

### Profiling

- CPU profiler
- Memory profiler
- Network profiler
- Database query analyzer

### Debugging

- Debugger del IDE
- Breakpoints
- Watch variables
- Remote debugging

### Monitoring

- Application Performance Monitoring (APM)
- Error tracking
- Uptime monitoring
- Custom metrics

## Errores Comunes

### No Reproducible

- Registra contexto exacto
- Intenta en diferentes ambientes
- Valida dependencias
- Revisa versiones

### Intermitente

- Aumenta logging
- Usa herramientas de race condition
- Valida sincronización
- Revisa timing

### En Producción

- No apagues el servidor
- Recolecta datos primeiro
- Escala si hay urgencia
- Pide contexto al usuario

---

**Última Actualización:** 2026
