# Plantillas de Prompts de IA para Desarrollo de Software

**Prompts optimizados para ChatGPT, Claude y Gemini** — diseñados para asistir 
a arquitectos de software, desarrolladores y líderes técnicos en la toma de 
decisiones y resolución de problemas.

## 🎯 Propósito

Este repositorio contiene plantillas de prompts probadas en producción para 
escenarios comunes de desarrollo de software. Cada prompt está optimizado para 
claridad, contexto y salida accionable desde LLMs.

**Casos de uso:**
- Diseño de sistemas y revisión arquitectónica
- Asistencia en revisión de código y optimización
- Diseño de APIs y documentación
- Toma de decisiones técnicas
- Refactorización de sistemas legacy
- Estrategias de optimización de rendimiento

## 📂 Estructura del Repositorio

### `prompts/`
Plantillas de prompts principales, un archivo por escenario.

### `ejemplos/`
Ejemplos del mundo real mostrando entrada → salida de LLM → uso práctico.

### `categorias/`
Organizadas por dominio (Arquitectura, Calidad de Código, Documentación, Depuración).

## 🚀 Inicio Rápido

1. **Selecciona un prompt** de `prompts/` según tu caso de uso
2. **Copia el prompt completo** en tu LLM (ChatGPT, Claude, etc.)
3. **Personaliza las secciones [CONTEXTO]** con tus detalles específicos
4. **Ejecuta** e itera sobre el resultado

### Ejemplo: Arquitectura de Software

[Copia el prompt desde prompts/arquitecto-software-asistente.md]

[Reemplaza: [NOMBRE_TU_SISTEMA] con "Sistema de Procesamiento de Pagos"]
[Reemplaza: [RESTRICCIONES] con "Debe manejar 10k transacciones/segundo"]

→ Pega en ChatGPT/Claude
→ Obtén recomendaciones detalladas de arquitectura

## 📋 Prompts Disponibles

| Prompt | Caso de Uso | Complejidad |
| --- | --- | --- |
| arquitecto-software-asistente | Diseño de sistemas, decisiones arquitectónicas | Avanzado |
| experto-revision-codigo | Análisis de calidad de código, optimización | Intermedio |
| generador-documentacion-tecnica | Auto-generar docs desde código | Intermedio |
| asistente-depuracion | Solucionar problemas complejos | Intermedio |
| ayudante-diseno-sistemas | Lluvia de ideas rápida de diseño | Principiante |
| experto-diseno-api | Diseño de APIs REST/GraphQL | Intermedio |
| estratega-despliegue | Planificación CI/CD y despliegue | Avanzado |

## 💡 Consejos para Mejores Resultados

1. **Sé específico:** Cuanto más contexto proporciones, mejor el resultado
2. **Itera:** Ejecuta el prompt, revisa, refina y vuelve a ejecutar
3. **Combina prompts:** Usa múltiples prompts secuencialmente para tareas complejas
4. **Personaliza:** Adapta los prompts a tu stack tecnológico y restricciones
5. **Versión en git:** Guarda outputs en git para comparar evoluciones

## 📚 Flujos de Trabajo Ejemplo

### Flujo 1: Nuevo Diseño de Sistema
1. Comienza con `ayudante-diseno-sistemas` (lluvia de ideas)
2. Expande con `arquitecto-software-asistente` (diseño detallado)
3. Continúa con `experto-diseno-api` (contratos de API)
4. Finaliza con `generador-documentacion-tecnica` (docs)

### Flujo 2: Refactorización de Código
1. Usa `experto-revision-codigo` (identifica problemas)
2. Aclara con `asistente-depuracion` (entiende raíz del problema)
3. Planifica con `estratega-despliegue` (estrategia de rollout)

## 🤝 Contribuir

¿Encontraste un mejor prompt? ¿Tienes un nuevo caso de uso? ¡Contribuciones bienvenidas!

Ver [CONTRIBUIR.md](CONTRIBUIR.md)

## 📄 Licencia

Licencia MIT — Libre de usar, modificar y compartir.

---

## 🔗 Recursos Relacionados

- [Guía Prompt Engineering de OpenAI](https://platform.openai.com/docs/guides/prompt-engineering)
- [Guía Prompt Engineering de Anthropic](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview)
- [Mi GitHub](https://github.com/yourusername)
- [Mi Perfil Toptal](https://www.toptal.com/profile/ruben-cabrera)

---

**Autor:** Rubén Cabrera  
**Última Actualización:** 2026  
**Estado:** Activo y Mantenido ✓
