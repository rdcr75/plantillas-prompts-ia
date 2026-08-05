# Prompt: Estratega de Despliegue

## 🎯 Propósito

Planificación de estrategias de CI/CD y despliegue seguro.

## 📋 Plantilla

Copia todo lo siguiente, rellena los [CORCHETES], pega en ChatGPT/Claude:

---

Eres experto en CI/CD y deployments. Tu tarea es diseñar una estrategia de despliegue segura.

**Proyecto:** [NOMBRE_PROYECTO]
**Stack:** [LENGUAJE/FRAMEWORK/STACK]
**Ambiente Actual:** [AMBIENTE_ACTUAL]

**Ambientes:**

- Desarrollo: [DESCRIPCIÓN]
- Staging: [DESCRIPCIÓN]
- Producción: [DESCRIPCIÓN]

**Herramientas Disponibles:**

- VCS: [Git/etc]
- CI/CD: [Jenkins/GitHub Actions/GitLab CI/etc]
- Containerización: [Docker/etc]
- Orchestración: [Kubernetes/ECS/etc]
- IaaS: [AWS/GCP/Azure]

**Requisitos:**

- Downtime máximo permitido: [DOWNTIME]
- Rollback time máximo: [TIEMPO]
- Testing requerido: [UNIT/INTEGRATION/E2E]
- Aprobación requerida: [MANUAL/AUTOMÁTICA]

**Desafíos Actuales:**

- [DESAFÍO_1]
- [DESAFÍO_2]
- [DESAFÍO_3]

**Cambios Frecuentes:**

- Frecuencia de deploys: [VECES_POR_SEMANA]
- Tamaño de cambios: [PEQUEÑO/MEDIO/GRANDE]
- Riesgo de cambios: [BAJO/MEDIO/ALTO]

Por favor proporciona:

- Pipeline CI/CD detallada (etapas, triggers)
- Estrategia de despliegue (blue-green/canary/rolling)
- Rollback plan
- Monitoring y alertas
- Seguridad (scanning, permisos, etc.)
- Documentación de proceso
- Checklist pre-deploy

---

## 📝 Ejemplo Real

**Proyecto:** API de Pagos
**Stack:** Java/Spring Boot, PostgreSQL, Kubernetes, AWS
**Ambiente:** Staging y Producción

[Inserta detalles arriba, luego ejecuta]

## 🎯 Salida Esperada

✅ Pipeline CI/CD detallada
✅ Estrategia de deployment
✅ Rollback procedures
✅ Monitoring setup
✅ Runbook de deploy

## 💡 Consejos

- Sé específico sobre herramientas disponibles
- Menciona volumen de usuarios/tráfico
- Describe cambios típicos
- Pregunta sobre compliance requerido
