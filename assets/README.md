# Assets del Proyecto

Este directorio contiene recursos estáticos y plantillas reutilizables.

## Estructura

```
assets/
├── images/           # Imágenes y diagramas
│   ├── arquitectura/
│   ├── diagramas/
│   ├── screenshots/
│   └── logos/
└── templates/        # Plantillas reutilizables
    ├── workflows/
    ├── documentos/
    └── emails/
```

## Images

### Arquitectura
Diagramas de arquitectura del sistema:
- Arquitectura general
- Arquitectura de componentes
- Diagramas de despliegue

**Formatos**: PNG, SVG (preferido para diagramas)

### Diagramas
Diagramas de flujo y procesos:
- Diagramas de flujo de workflows
- Diagramas de secuencia
- Diagramas de casos de uso

**Herramientas sugeridas**:
- draw.io / diagrams.net
- Lucidchart
- Mermaid (para diagramas en Markdown)

### Screenshots
Capturas de pantalla para documentación:
- UI de n8n
- Configuraciones
- Resultados de ejecución
- Ejemplos de uso

### Logos
Logos del proyecto y marcas relacionadas:
- Logo principal del proyecto
- Logos de servicios integrados
- Iconos

**Formatos**: PNG (con transparencia), SVG

## Templates

### Workflows
Templates de workflows n8n reutilizables:
- Template de manejo de errores
- Template de logging
- Template de notificaciones
- Template de validación

### Documentos
Plantillas de documentos:
- Plantilla de especificación funcional
- Plantilla de documentación técnica
- Plantilla de reportes

### Emails
Plantillas de emails HTML:
- Notificación de factura generada
- Alerta de error
- Reporte diario/semanal

## Convenciones de Nombres

### Imágenes
```
[categoria]-[descripcion]-[version].[ext]

Ejemplos:
arquitectura-general-v1.png
diagrama-flujo-facturacion-v2.svg
screenshot-workflow-config.png
```

### Templates
```
template-[tipo]-[nombre].[ext]

Ejemplos:
template-workflow-error-handler.json
template-email-factura.html
template-doc-especificacion.md
```

## Uso de Assets

### En Documentación

Markdown:
```markdown
![Arquitectura General](../assets/images/arquitectura/arquitectura-general-v1.png)
```

HTML:
```html
<img src="../assets/images/arquitectura/arquitectura-general-v1.png" 
     alt="Arquitectura General" 
     width="600">
```

### En Workflows

Para usar templates:
1. Copiar template desde `assets/templates/workflows/`
2. Importar en n8n
3. Personalizar según necesidad

## Versionado

Las imágenes de arquitectura y diagramas importantes deben versionarse:
- Mantener versión anterior al actualizar
- Documentar cambios en nombre o metadata
- Ejemplo: `arquitectura-v1.png`, `arquitectura-v2.png`

## Optimización

### Imágenes
- Comprimir PNGs (usar herramientas como TinyPNG)
- Usar SVG para diagramas cuando sea posible
- Tamaño recomendado: < 500KB por imagen

### Templates
- Minimizar templates HTML
- Documentar variables/placeholders
- Incluir comentarios explicativos

## Licencias

Asegurar que todos los assets tienen licencia apropiada:
- Assets creados internamente: Licencia del proyecto
- Assets de terceros: Verificar licencia y dar crédito

## .gitattributes

Para manejo correcto de archivos binarios:
```
*.png binary
*.jpg binary
*.jpeg binary
*.gif binary
*.svg text
*.json text
*.html text
```

## Herramientas Recomendadas

### Diagramas
- [draw.io](https://app.diagrams.net/)
- [Mermaid](https://mermaid.js.org/)
- [PlantUML](https://plantuml.com/)

### Edición de Imágenes
- GIMP (gratuito)
- Photoshop
- Figma

### Optimización
- [TinyPNG](https://tinypng.com/)
- [SVGO](https://github.com/svg/svgo)
- [ImageOptim](https://imageoptim.com/)

## Contribución

Al agregar assets:
1. Usar nombres descriptivos
2. Optimizar antes de commitear
3. Documentar en README si es necesario
4. Actualizar referencias en documentación

## Contacto

Para dudas sobre assets, contactar al equipo de documentación o diseño.
