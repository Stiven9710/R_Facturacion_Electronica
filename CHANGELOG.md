# Changelog

Todos los cambios notables en este proyecto serán documentados en este archivo.

El formato está basado en [Keep a Changelog](https://keepachangelog.com/es-ES/1.0.0/),
y este proyecto adhiere a [Semantic Versioning](https://semver.org/lang/es/).

## [Unreleased]

### Agregado
- Estructura inicial del proyecto
- Documentación de especificaciones funcionales
- Documentación técnica
- Guías de despliegue
- Sistema de gestión de componentes
- Templates de configuración
- Estructura de directorios para workflows, tests y despliegue

### Cambiado
- N/A

### Deprecated
- N/A

### Removido
- N/A

### Corregido
- N/A

### Seguridad
- N/A

---

## [1.0.0] - YYYY-MM-DD

### Agregado
- Primera versión del sistema de facturación electrónica
- Workflow de generación de facturas
- Integración con API DIAN
- Sistema de notificaciones
- Dashboard de reportes

---

## Tipos de Cambios

- `Agregado` - para funcionalidades nuevas
- `Cambiado` - para cambios en funcionalidades existentes
- `Deprecated` - para funcionalidades que serán removidas
- `Removido` - para funcionalidades removidas
- `Corregido` - para corrección de bugs
- `Seguridad` - para vulnerabilidades de seguridad

## Formato de Entradas

Cada entrada debe incluir:
- **Descripción clara** del cambio
- **ID de Issue/PR** si aplica
- **Autor** si es relevante
- **Breaking changes** marcados claramente

### Ejemplo:

```markdown
## [1.2.0] - 2024-01-15

### Agregado
- Nuevo workflow de facturación recurrente (#123) @usuario
- Soporte para múltiples monedas (#124)

### Cambiado
- **BREAKING**: Formato de API de facturas actualizado (#125)
- Mejorado performance de generación de reportes (#126)

### Corregido
- Error en cálculo de impuestos (#127)
- Bug en validación de NIT (#128)

### Seguridad
- Actualizado dependencia con vulnerabilidad crítica (#129)
```

## Versionado

El proyecto sigue [Semantic Versioning](https://semver.org/):

- **MAJOR** (X.0.0): Cambios incompatibles con versiones anteriores
- **MINOR** (0.X.0): Nueva funcionalidad compatible con versiones anteriores
- **PATCH** (0.0.X): Correcciones de bugs compatibles

---

[Unreleased]: https://github.com/Stiven9710/R_Facturacion_Electronica/compare/v1.0.0...HEAD
[1.0.0]: https://github.com/Stiven9710/R_Facturacion_Electronica/releases/tag/v1.0.0
