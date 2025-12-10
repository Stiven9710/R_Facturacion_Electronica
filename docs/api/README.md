# Documentación de APIs

Esta carpeta contiene la documentación de todas las APIs utilizadas e implementadas en el sistema de facturación electrónica.

## Contenido

### APIs Externas
Documentación de APIs de terceros que consume el sistema:
- APIs de facturación electrónica (DIAN, proveedores)
- APIs de servicios de pago
- APIs de notificaciones
- Otras integraciones

### APIs Internas
Documentación de endpoints expuestos por nuestros workflows:
- Webhooks de n8n
- APIs REST implementadas
- Callbacks y notificaciones

## Estructura de Documentación

Cada API debe documentarse siguiendo el formato OpenAPI/Swagger cuando sea posible.

### Plantilla de Documentación

Para cada API, incluir:
- **Descripción general**: Propósito y alcance
- **Autenticación**: Método y credenciales necesarias
- **Endpoints**: Lista completa de endpoints
- **Request/Response**: Ejemplos de peticiones y respuestas
- **Códigos de error**: Listado de errores posibles
- **Rate limiting**: Límites de uso
- **Ejemplos**: Casos de uso comunes

## Lista de APIs

### APIs Externas

#### API de Facturación DIAN
- **Archivo**: `api_dian.md`
- **Versión**: 2.1
- **Estado**: Activo
- **Proveedor**: DIAN

#### API de Servicios de Pago
- **Archivo**: `api_pagos.md`
- **Versión**: 1.0
- **Estado**: Activo

### APIs Internas (Webhooks)

#### Webhook de Generación de Facturas
- **Archivo**: `webhook_generar_factura.md`
- **URL**: `/webhook/generar-factura`
- **Método**: POST

#### Webhook de Notificaciones
- **Archivo**: `webhook_notificaciones.md`
- **URL**: `/webhook/notificacion`
- **Método**: POST

## Convenciones

- Usar formato Markdown para documentación legible
- Incluir archivos OpenAPI/Swagger (.yaml o .json) cuando sea posible
- Proporcionar colecciones de Postman/Insomnia para testing
- Mantener ejemplos actualizados

## Testing de APIs

### Colecciones
- `collections/postman/` - Colecciones de Postman
- `collections/insomnia/` - Colecciones de Insomnia

### Ambientes
- Development: `environments/dev.json`
- Staging: `environments/staging.json`
- Production: `environments/production.json`

## Seguridad

⚠️ **IMPORTANTE**: 
- No incluir credenciales reales en la documentación
- Usar placeholders: `{{API_KEY}}`, `{{SECRET}}`
- Referir a documentación de configuración para obtener credenciales

## Actualización

La documentación de APIs debe actualizarse cuando:
- Se agrega un nuevo endpoint
- Cambia el formato de request/response
- Se actualiza la versión de la API externa
- Cambian los requisitos de autenticación
