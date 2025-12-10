# Registro de Componentes

Este documento mantiene un registro de todos los componentes del sistema de facturación electrónica.

## Tabla de Componentes

### Workflows n8n

| ID | Nombre | Descripción | Versión | Estado | Owner | Fecha Creación | Última Actualización |
|----|--------|-------------|---------|--------|-------|----------------|---------------------|
| WF-001 | - | - | - | Backlog | - | - | - |

**Estados**:
- 🔵 Backlog
- 🟡 En Desarrollo
- 🟢 Activo
- 🔴 Deprecated
- ⚫ Archivado

### Custom Nodes

| ID | Nombre | Descripción | Versión | Estado | Owner | Fecha Creación | Última Actualización |
|----|--------|-------------|---------|--------|-------|----------------|---------------------|
| CN-001 | - | - | - | Backlog | - | - | - |

### Scripts

| ID | Nombre | Descripción | Lenguaje | Versión | Estado | Owner | Fecha Creación | Última Actualización |
|----|--------|-------------|----------|---------|--------|-------|----------------|---------------------|
| SC-001 | - | - | - | - | Backlog | - | - | - |

### Integraciones

| ID | Sistema Externo | Tipo | Descripción | Versión API | Estado | Owner | Fecha Creación | Última Actualización |
|----|-----------------|------|-------------|-------------|--------|-------|----------------|---------------------|
| INT-001 | - | - | - | - | Backlog | - | - | - |

## Dependencias entre Componentes

### Matriz de Dependencias

| Componente | Depende de | Es requerido por |
|------------|------------|------------------|
| WF-001 | - | - |

## Componentes por Categoría

### Facturación
- WF-001: [Nombre]
- WF-002: [Nombre]

### Notificaciones
- WF-010: [Nombre]

### Reportes
- WF-020: [Nombre]

### Integraciones
- INT-001: [Nombre]

## Roadmap de Componentes

### Q1 2025
- [ ] WF-001: Generación de facturas
- [ ] WF-002: Validación de documentos
- [ ] INT-001: Integración con DIAN

### Q2 2025
- [ ] WF-010: Sistema de notificaciones
- [ ] CN-001: Nodo de validación NIT

### Q3 2025
- [ ] WF-020: Dashboard de reportes
- [ ] INT-002: Integración con sistema de pagos

### Q4 2025
- [ ] Optimizaciones y mejoras

## Componentes Deprecated

| ID | Nombre | Fecha Deprecación | Reemplazado por | Motivo |
|----|--------|-------------------|-----------------|--------|
| - | - | - | - | - |

## Notas

- Los componentes deben tener documentación completa antes de pasar a estado "Activo"
- Cada componente debe tener al menos un owner responsable
- Las versiones siguen Semantic Versioning (MAJOR.MINOR.PATCH)
- Los componentes deprecated deben mantenerse 6 meses antes de archivarse

## Proceso de Registro

1. Asignar ID único según categoría (WF-, CN-, SC-, INT-)
2. Completar información básica en tabla correspondiente
3. Crear documentación en `docs/especificaciones/` y `docs/tecnicas/`
4. Actualizar matriz de dependencias
5. Actualizar roadmap si aplica

## Última Actualización

**Fecha**: DD/MM/YYYY  
**Por**: [Nombre]  
**Cambios**: Creación del documento
