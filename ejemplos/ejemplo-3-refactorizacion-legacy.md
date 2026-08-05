# Ejemplo 3: Refactorización de Sistema Legacy

## 📋 Contexto

**Sistema:** Plataforma de Gestión de Inventario (15 años)
**Lenguaje:** PHP 5.6 (¡muy antiguo!)
**Usuarios:** 100+ tiendas minoristas
**Presión:** Rendimiento pobre, difícil mantener

## 🎯 Problema

El código legacy:

- PHP 5.6 (soporte terminado hace años)
- No tiene tests
- Base de datos con relaciones circulares
- Variables globales por todo el código
- Slow (genera reportes en 30 minutos)
- Acoplamiento severo
- Difícil agregar features

## 📝 Prompt Usado

Eres experto en revisión de código. Tu tarea es revisar código legacy
y proporcionar feedback constructivo.

Lenguaje: PHP
Framework: Sin framework (código procedural)
Patrón Arquitectónico: Ninguno (spaghetti code)

Enfoque de Revisión:

Seguridad
Rendimiento
Mantenibilidad
Testing

Contexto del Código:
Plataforma de inventario de 15 años. Gestiona 100+ tiendas.
Genera reportes, maneja transacciones, integra con proveedores.

Código a Revisar:
[Ejemplos de código legacy con problemas comunes]

Preguntas Específicas:

¿Cuál es el plan de refactorización menos riesgoso?
¿Deberíamos reescribir o refactorizar gradualmente?
¿Cómo mantenemos servicios durante transición?
¿Qué debe ser prioritario?

Por favor proporciona:

Problemas principales (severidad)
Roadmap de refactorización
Estrategia de testing
Herramientas recomendadas
Pasos concretos

## 💡 Salida Esperada (Estrategia)

### Problemas Identificados

| Problema | Severidad | Impacto | Urgencia |
| --- | --- | --- | --- |
| PHP 5.6 EOL | Crítica | Seguridad, soporte | Inmediato |
| Sin tests | Alta | Regressions, mantenimiento | Alta |
| Globals | Alta | Bugs, threading issues | Alta |
| N+1 queries | Alta | Rendimiento pobre | Alta |
| Variables circulares | Media | Complejidad cognitiva | Media |
| Sin logging | Media | Debugging difícil | Media |

### Roadmap de Refactorización (12 meses)

**Fase 1 (Mes 1-3): Seguridad + Monitoring**

Semana 1-2: Setup

Upgrade a PHP 7.4 (versión LTS)
Instalar herramienta estática (PHPStan, Psalm)
Agregar logging (Monolog)

Semana 3-4: Tests Base

Escribir tests para funcionalidad crítica (pagos, inventario)
Target: 30% cobertura

Semana 5-8: Refactor Crítico

Eliminar globals
Reescribir queries N+1 más severas
Agregar validación de inputs
Fixes de seguridad

Semana 9-12: Validar

Testing exhaustivo
Load testing
Deploy a staging
Rollback plan listo

**Fase 2 (Mes 4-6): Modernización**

Semana 13-16: Framework Moderno

Migrar a PHP 8.1 + Laravel/Symfony
Establecer patrón arquitectónico (MVC limpio)
Crear estructura base

Semana 17-20: Modularización

Identificar "bounded contexts"
Migrar módulo por módulo (ej: Inventario, Pagos, Reportes)
Mantener API legacy para compatibilidad

Semana 21-24: Testing

Tests unitarios para nueva arquitectura
Tests de integración
Tests end-to-end

**Fase 3 (Mes 7-9): Validación**

Semana 25-32: Parallel Run

Sistema nuevo corre en paralelo
Compara resultados con sistema legacy
Identifica discrepancias
Usuarios beta testing

**Fase 4 (Mes 10-12): Cutover**

Semana 33-36: Gradual Migration

Move 10% de usuarios a sistema nuevo
Monitor de cerca
Valida datos
Fix issues

Semana 37-40: Ramp Up

Move 50% usuarios
Mantén fallback a legacy

Semana 41-44: Final

Move 100% usuarios
Decomission sistema legacy (después de período de estabilidad)

### Estrategia de Testing

**Nivel 1: Unit Tests (nuevos módulos)**

```php
// Antes (legacy):
function calcularInventario($tienda_id) {
  // 200 líneas de lógica sin estructura
}

// Después (moderno con tests):
class InventoryCalculator {
  public function calculate(StoreId $storeId): Inventory {
    // 20 líneas claras de lógica
  }
}

// Test:
public function testCalculateInventoryForStore() {
  $calculator = new InventoryCalculator();
  $inventory = $calculator->calculate(StoreId::fromString('123'));
  
  $this->assertEquals(100, $inventory->getTotalItems());
}
```

**Nivel 2: Integration Tests**

```php
// Test flujo completo: 
// recibir orden → actualizar inventario → generar factura
public function testOrderToInvoiceFlow() {
  $order = Order::create(['items' => [...]]);
  $inventory = $this->inventoryService->updateFromOrder($order);
  $invoice = $this->invoiceService->generateFromOrder($order);
  
  $this->assertTrue($invoice->isValid());
}
```

**Nivel 3: Regression Tests**

```php
// Comparar resultados legacy vs nuevo
public function testNewSystemMatchesLegacyResults() {
  $legacyResult = $this->legacyInventoryCalculator->calculate($storeId);
  $newResult = $this->newInventoryCalculator->calculate($storeId);
  
  $this->assertEquals($legacyResult, $newResult);
}
```

### Herramientas Recomendadas

Análisis Estático:

PHPStan (detecta errores sin ejecutar)
Psalm (seguridad, types)
PHP-CS-Fixer (estilo)

Testing:

PHPUnit (tests unitarios)
Codeception (tests integración)
Behat (tests comportamiento)

Debugging:

Xdebug (debugger)
Blackfire (profiling)
Monolog (logging)

Refactoring:

PHPStorm IDE (refactoring integrado)
Rector (refactoring automático)
Git (revert si falla)

CI/CD:

GitHub Actions / GitLab CI
Automated tests en cada commit
Staging automático

### Pasos Concretos (Semana 1)

```bash
# 1. Upgrade a PHP 7.4
composer config platform.php 7.4.0
composer update

# 2. Instalar herramientas
composer require --dev phpstan/phpstan
composer require --dev squizlabs/php_codesniffer
composer require monolog/monolog

# 3. Ejecutar análisis
./vendor/bin/phpstan analyse src/

# 4. Crear primer test
mkdir -p tests/Unit
# Crear test para función crítica

# 5. CI/CD
# Crear .github/workflows/tests.yml
# Run tests en cada push
```

### Riesgos y Mitigación

| Riesgo | Impacto | Mitigación |
| --- | --- | --- |
| Datos corruptos en migración | Crítico | Parallel run, sync validation |
| Downtime durante cutover | Alto | Blue-green deployment |
| Usuarios confundidos | Medio | Comunicación clara, rollback |
| Costos de desarrollo | Alto | Priorizar critical path |

## 📊 Resultados Esperados

### Antes (Legacy)

PHP: 5.6 (EOL)
Tiempo de deploy: 2 horas (manual, riesgoso)
Reportes: 30 minutos
Cobertura tests: 0%
Bugs/mes: 15-20
Nuevas features/mes: 1-2

### Después (Moderno)

PHP: 8.1
Tiempo de deploy: 5 minutos (automático, seguro)
Reportes: 2 minutos (10x más rápido)
Cobertura tests: 80%
Bugs/mes: 2-3 (90% menos)
Nuevas features/mes: 8-10 (5x más)

## 🔗 Recursos

- [Refactoring: Improving the Design of Existing Code - Martin Fowler](https://refactoring.com/)
- [Working Effectively with Legacy Code - Michael Feathers](https://www.oreilly.com/library/view/working-effectively-with/0131177052/)
- [Modernizing PHP Applications - Cal Evans](https://www.oreilly.com/library/view/modernizing-php-applications/9781491929057/)

---

**Aplicable a:** Cualquier codebase legacy
**Complejidad:** Avanzado
**Timeline típico:** 12-18 meses para migración completa
