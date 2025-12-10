# Workflows n8n

Este directorio contiene todos los workflows de n8n exportados en formato JSON.

## Estructura

Los workflows están organizados por categoría:

```
workflows/
├── facturacion/          # Workflows relacionados con facturación
├── notificaciones/       # Workflows de notificaciones
├── reportes/            # Workflows de generación de reportes
├── integraciones/       # Workflows de integración con sistemas externos
└── utilidades/          # Workflows de utilidad general
```

## Convención de Nombres

Los archivos de workflow siguen el siguiente formato:
```
[Categoría]-[ID]-[Nombre-Descriptivo].json
```

Ejemplos:
- `FAC-001-Generar-Factura-Electronica.json`
- `NOT-001-Enviar-Notificacion-Email.json`
- `REP-001-Reporte-Ventas-Mensual.json`

## Importar Workflows

### Método 1: Desde la UI de n8n

1. Acceder a n8n en http://localhost:5678
2. Click en "Add workflow" o abrir un workflow existente
3. Click en el menú (⋮) > "Import from File"
4. Seleccionar el archivo JSON del workflow
5. Guardar el workflow

### Método 2: Usando CLI de n8n

```bash
# Importar un workflow específico
n8n import:workflow --input=src/workflows/facturacion/FAC-001-Generar-Factura.json

# Importar todos los workflows de una carpeta
n8n import:workflow --input=src/workflows/facturacion/ --separate

# Importar con reemplazo de IDs
n8n import:workflow --input=src/workflows/facturacion/FAC-001-Generar-Factura.json --replace
```

## Exportar Workflows

### Exportar desde n8n

```bash
# Exportar un workflow específico
n8n export:workflow --id=[WORKFLOW_ID] --output=src/workflows/[categoria]/[nombre].json

# Exportar todos los workflows
n8n export:workflow --all --output=backups/all-workflows.json

# Exportar workflows por tag
n8n export:workflow --tag=facturacion --output=src/workflows/facturacion/
```

### Desde la UI

1. Abrir el workflow en n8n
2. Click en el menú (⋮) > "Download"
3. Guardar el archivo en la carpeta correspondiente
4. Renombrar según convención

## Estructura de un Workflow

Un archivo de workflow JSON contiene:

```json
{
  "name": "Nombre del Workflow",
  "nodes": [
    {
      "parameters": {},
      "name": "Start",
      "type": "n8n-nodes-base.start",
      "position": [240, 300]
    }
  ],
  "connections": {},
  "settings": {
    "executionOrder": "v1"
  },
  "staticData": null,
  "tags": [],
  "triggerCount": 0
}
```

## Configuración de Workflows

### Variables de Entorno

Usar variables de entorno para configuraciones específicas de ambiente:

```javascript
// En un Function Node
const apiUrl = $env.API_URL || 'https://api.default.com';
const apiKey = $env.API_KEY;
```

### Credenciales

No incluir credenciales en los archivos JSON. Las credenciales deben configurarse en n8n:

1. Credentials > Add Credential
2. Seleccionar tipo de credencial
3. Completar información
4. Guardar con nombre descriptivo

### Settings Recomendados

- **Execution Order**: v1 (recomendado para flujos lineales)
- **Timezone**: America/Bogota
- **Error Workflow**: Configurar workflow de manejo de errores
- **Timeout**: 300 segundos (ajustar según necesidad)

## Testing de Workflows

### Pruebas Manuales

1. Activar el workflow en modo de prueba
2. Ejecutar manualmente con datos de prueba
3. Verificar los outputs en cada nodo
4. Revisar los logs de ejecución

### Pruebas Automatizadas

Ver `tests/integration/workflows/` para tests automatizados.

## Versionado

- Mantener el workflow JSON en Git
- Usar branches para desarrollo de features
- Crear tags para releases estables
- Documentar cambios en el commit message

## Documentación Relacionada

- **Especificaciones Funcionales**: `docs/especificaciones/`
- **Documentación Técnica**: `docs/tecnicas/`
- **Guía de Despliegue**: `docs/manuales/guia_despliegue.md`

## Mejores Prácticas

### Organización de Nodos

- Usar nombres descriptivos para nodos
- Agrupar nodos relacionados visualmente
- Usar notas (sticky notes) para documentar secciones
- Mantener el flujo de izquierda a derecha

### Manejo de Errores

- Implementar error handling en workflows críticos
- Usar "Error Trigger" node para workflows de error
- Logging apropiado de errores
- Notificaciones de errores críticos

### Performance

- Evitar loops innecesarios
- Usar batch processing cuando sea posible
- Implementar caching donde aplique
- Monitorear tiempo de ejecución

### Seguridad

- No hardcodear credenciales
- Validar inputs
- Sanitizar datos sensibles en logs
- Usar HTTPS para comunicaciones

## Troubleshooting

### Workflow no se importa

- Verificar formato JSON válido
- Verificar compatibilidad de versión de n8n
- Revisar que no falten dependencias (custom nodes)

### Workflow falla al ejecutar

- Verificar credenciales configuradas
- Revisar logs de ejecución
- Verificar conectividad a servicios externos
- Validar datos de entrada

## Contacto

Para dudas sobre workflows específicos, contactar al owner del componente (ver `gestion/componentes/registro.md`).
