# Prompt: Asistente de Depuración

## 🎯 Propósito

Ayuda estructurada para debuggear problemas complejos en código.

## 📋 Plantilla

Copia todo lo siguiente, rellena los [CORCHETES], pega en ChatGPT/Claude:

---

Eres un experto en debugging. Tu tarea es ayudar a identificar y solucionar un problema técnico complejo.

**Lenguaje/Stack:** [LENGUAJE, FRAMEWORK, VERSIONES]

**Descripción del Problema:**
[Describe qué está fallando, cuándo ocurre, síntomas]

**Error/Mensaje que Recibe:**
[PEGA EL ERROR EXACTO]

**Stack Trace (si aplica):**

[PEGA EL STACK TRACE COMPLETO]

**Contexto:**

- Sistema operativo: [WINDOWS/MAC/LINUX]
- Versiones: [VERSIONES_RELEVANTES]
- Ambiente: [DESARROLLO/STAGING/PRODUCCIÓN]
- Volumen de datos: [SMALL/MEDIUM/LARGE]

**Código Relevante:**

[PEGA EL CÓDIGO QUE CREES QUE ESTÁ FALLANDO]

**Qué Intentaste:**

- [INTENTO_1]
- [INTENTO_2]
- [INTENTO_3]

**Logs Recientes:**
[PEGA LOGS RELEVANTES]

Por favor:

- Identifica la causa raíz probable
- Proporciona paso-a-paso para reproducir
- Sugiere posibles soluciones
- Ayuda a diagnosticar si hay datos corruptos
- Proporciona comandos de debugging
- Sugiere monitoreo/alertas para prevenir

---

## 📝 Ejemplo Real

**Stack:** Node.js + Express + PostgreSQL
**Error:** Timeout en query de base de datos

[Inserta detalles arriba, luego ejecuta]

## 🎯 Salida Esperada

✅ Causa raíz identificada
✅ Pasos de reproducción
✅ Soluciones con ejemplos de código
✅ Prevención futura
✅ Monitoreo recomendado

## 💡 Consejos

- Proporciona TODOS los detalles de error
- Incluye logs sin filtrar (censura solo datos sensibles)
- Menciona cuándo comenzó el problema
- Describe cualquier cambio reciente
- Proporciona datos sobre escala/volumen
