# Categoría: Calidad de Código

Prompts para mejorar calidad, seguridad y mantenibilidad del código.

## Prompts en Esta Categoría

### 1. Experto en Revisión de Código

**Archivo:** `../prompts/experto-revision-codigo.md`
**Uso:** Análisis profundo de código
**Complejidad:** Intermedio
**Tiempo:** 20-30 minutos
**Resultado:** Problemas identificados, mejoras

### 2. Asistente de Depuración

**Archivo:** `../prompts/asistente-depuracion.md`
**Uso:** Debugging de problemas complejos
**Complejidad:** Intermedio
**Tiempo:** 15-30 minutos
**Resultado:** Causa raíz, soluciones

## Principios de Calidad

### SOLID

- **S**ingle Responsibility
- **O**pen/Closed
- **L**iskov Substitution
- **I**nterface Segregation
- **D**ependency Inversion

### Clean Code

- Nombres significativos
- Funciones pequeñas (una responsabilidad)
- Comentarios solo cuando sea necesario
- Manejo de errores explícito
- Testing

### Seguridad

- Input validation
- Prevención de inyección
- Autenticación/Autorización
- Encriptación
- Logging de seguridad

## Checklist de Revisión de Código

### Seguridad

- [ ] Valida todos los inputs
- [ ] Usa parameterized queries
- [ ] Maneja errores correctamente
- [ ] No expone información sensible en logs
- [ ] Autentica y autoriza

### Rendimiento

- [ ] Evita N+1 queries
- [ ] Cachea cuando sea necesario
- [ ] Algoritmos eficientes
- [ ] No hace trabajo innecesario

### Mantenibilidad

- [ ] Nombres claros
- [ ] Funciones pequeñas
- [ ] Sin código duplicado
- [ ] Testing adecuado
- [ ] Documentación

---

**Última Actualización:** 2026