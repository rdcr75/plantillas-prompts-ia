# Prompt: Asistente Arquitecto de Software

## 🎯 Propósito

Obtener recomendaciones arquitectónicas detalladas para sistemas complejos usando IA.

## 📋 Plantilla

Copia todo lo siguiente, rellena los [CORCHETES], pega en ChatGPT/Claude:

---

Eres un arquitecto de software experto con más de 15 años de experiencia diseñando sistemas escalables. Tu tarea es revisar y mejorar una arquitectura de sistema.

**Nombre del Sistema:** [NOMBRE_TU_SISTEMA]
**Escala Actual:** [USUARIOS_ACTUALES/TRANSACCIONES_POR_SEGUNDO]
**Escala Objetivo:** [ESCALA_PROYECTADA_EN_12_MESES]

**Arquitectura Actual:**
[Describe tu sistema actual: ¿Monolito? ¿Microservicios? ¿Stack tecnológico?]

**Restricciones:**

- Presupuesto: [RESTRICCIONES_PRESUPUESTO]
- Timeline: [TIMELINE]
- Tamaño del equipo: [NUMERO_DE_INGENIEROS]
- Requisitos críticos: [LISTA_REQUISITOS]

**Puntos Débiles:**

- [PROBLEMA_ACTUAL_1]
- [PROBLEMA_ACTUAL_2]
- [PROBLEMA_ACTUAL_3]

**Preguntas que necesito responder:**

1. ¿Qué patrón arquitectónico resolvería mejor nuestros problemas?
2. ¿Cómo deberíamos migrar de la arquitectura actual a la propuesta?
3. ¿Cuáles son los riesgos y cómo los mitigamos?
4. ¿Cuál es un timeline realista?
5. ¿Qué habilidades necesitamos contratar?

Por favor proporciona:

- Recomendación de arquitectura detallada (con diagrama ASCII/texto)
- Roadmap de migración (fases, checkpoints)
- Elecciones tecnológicas (por qué cada una)
- Evaluación de riesgos
- Requisitos de recursos
- Métricas de éxito

---

## 📝 Ejemplo Real

**Sistema:** Plataforma de E-commerce (tipo Amazon)
**Escala Actual:** 100k usuarios, 1k/seg en pico
**Escala Objetivo:** 1M usuarios, 10k/seg en pico

[Inserta tus detalles arriba, luego ejecuta]

## 🎯 Salida Esperada

Obtendrás:
✅ Arquitectura recomendada (microservicios, event-driven, etc.)
✅ Sugerencias de stack tecnológico (bases de datos, colas de mensajes, caché)
✅ Plan de escalabilidad
✅ Roadmap de migración
✅ Evaluación de riesgos

## 💡 Consejos

- **Sé específico** en restricciones y puntos débiles
- **Incluye stack actual** (influye en recomendaciones)
- **Menciona restricciones no técnicas** (presupuesto, timeline, equipo)
- **Haz preguntas de seguimiento** basadas en la salida
