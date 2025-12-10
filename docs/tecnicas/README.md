# Documentación Técnica

Este directorio contiene la documentación técnica detallada de todos los componentes del sistema.

## Contenido

La documentación técnica incluye:
- Arquitectura de componentes
- Detalles de implementación en n8n
- Configuración de workflows y nodos
- APIs y endpoints utilizados
- Manejo de errores y logging
- Consideraciones de seguridad y performance
- Guías de despliegue y mantenimiento

## Estructura de Documentos

Cada componente técnico debe documentarse siguiendo la plantilla `TEMPLATE_documentacion_tecnica.md`.

### Convención de Nombres
- Workflows: `DT-WF-[ID]-[NombreWorkflow].md`
- Custom Nodes: `DT-CN-[ID]-[NombreNodo].md`
- Scripts: `DT-SC-[ID]-[NombreScript].md`
- APIs: `DT-API-[ID]-[NombreAPI].md`

## Índice de Componentes

### Workflows
| ID | Nombre | Versión | Documento | Estado |
|----|--------|---------|-----------|--------|
| WF-001 | [Nombre] | 1.0.0 | [Link] | Activo |

### Custom Nodes
| ID | Nombre | Versión | Documento | Estado |
|----|--------|---------|-----------|--------|
| CN-001 | [Nombre] | 1.0.0 | [Link] | Activo |

### Scripts
| ID | Nombre | Versión | Documento | Estado |
|----|--------|---------|-----------|--------|
| SC-001 | [Nombre] | 1.0.0 | [Link] | Activo |

## Guías Técnicas

- **[Guía de n8n](guia_n8n.md)**: Mejores prácticas para desarrollo en n8n
- **[Guía de APIs](guia_apis.md)**: Estándares para integración de APIs
- **[Guía de Seguridad](guia_seguridad.md)**: Consideraciones de seguridad

## Arquitectura del Sistema

Ver `arquitectura_general.md` para una visión completa de la arquitectura del sistema.

## Actualización de Documentación

La documentación técnica debe actualizarse:
- Después de cada cambio significativo en el código
- Al agregar nuevos componentes
- Al actualizar dependencias críticas
- Durante el proceso de code review
