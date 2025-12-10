# Configuraciones

Este directorio contiene archivos de configuración para diferentes ambientes y componentes del sistema.

## Estructura

```
config/
├── development/         # Configuraciones de desarrollo
├── staging/            # Configuraciones de staging
├── production/         # Configuraciones de producción
├── templates/          # Templates de configuración
└── README.md           # Este archivo
```

## Archivos de Configuración

### config.json

Configuración principal del sistema:

```json
{
  "environment": "development",
  "n8n": {
    "host": "localhost",
    "port": 5678,
    "protocol": "http"
  },
  "database": {
    "type": "postgres",
    "host": "localhost",
    "port": 5432,
    "database": "n8n"
  },
  "apis": {
    "facturacion": {
      "baseUrl": "https://api.facturacion.com",
      "timeout": 30000
    }
  },
  "features": {
    "enableNotifications": true,
    "enableReports": true
  }
}
```

### credentials.json

⚠️ **NO COMMITEAR ESTE ARCHIVO**

Template para credenciales (usar variables de entorno en producción):

```json
{
  "facturacionApi": {
    "type": "apiKey",
    "apiKey": "${FACTURACION_API_KEY}"
  },
  "database": {
    "username": "${DB_USERNAME}",
    "password": "${DB_PASSWORD}"
  }
}
```

### environment.json

Variables de entorno específicas:

```json
{
  "NODE_ENV": "development",
  "LOG_LEVEL": "debug",
  "API_BASE_URL": "http://localhost:3000",
  "WEBHOOK_BASE_URL": "http://localhost:5678/webhook"
}
```

## Uso por Ambiente

### Development

```bash
# Copiar template
cp config/templates/config.development.template.json config/development/config.json

# Editar con valores locales
nano config/development/config.json
```

### Staging

```bash
# Usar variables de entorno
export NODE_ENV=staging
export DB_HOST=staging-db.example.com
export API_KEY=staging-key

# La aplicación cargará config/staging/config.json
```

### Production

```bash
# Todas las configuraciones deben venir de variables de entorno
# No usar archivos de configuración con valores hardcodeados
```

## Variables de Entorno

### Variables Requeridas

| Variable | Descripción | Ejemplo |
|----------|-------------|---------|
| `NODE_ENV` | Ambiente de ejecución | `production` |
| `N8N_HOST` | Host de n8n | `0.0.0.0` |
| `N8N_PORT` | Puerto de n8n | `5678` |
| `DB_TYPE` | Tipo de base de datos | `postgresdb` |
| `DB_HOST` | Host de base de datos | `localhost` |
| `DB_PORT` | Puerto de base de datos | `5432` |
| `DB_NAME` | Nombre de base de datos | `n8n` |
| `DB_USER` | Usuario de base de datos | `n8n_user` |
| `DB_PASSWORD` | Password de base de datos | `secret` |

### Variables Opcionales

| Variable | Descripción | Valor por Defecto |
|----------|-------------|-------------------|
| `N8N_BASIC_AUTH_ACTIVE` | Activar autenticación básica | `true` |
| `N8N_BASIC_AUTH_USER` | Usuario para auth básica | `admin` |
| `GENERIC_TIMEZONE` | Zona horaria | `America/Bogota` |
| `N8N_LOG_LEVEL` | Nivel de logging | `info` |

## Archivo .env

Crear archivo `.env` en la raíz del proyecto:

```bash
# n8n Configuration
N8N_HOST=0.0.0.0
N8N_PORT=5678
N8N_PROTOCOL=http
N8N_BASIC_AUTH_ACTIVE=true
N8N_BASIC_AUTH_USER=admin
N8N_BASIC_AUTH_PASSWORD=changeme

# Database
DB_TYPE=postgresdb
DB_POSTGRESDB_HOST=localhost
DB_POSTGRESDB_PORT=5432
DB_POSTGRESDB_DATABASE=n8n
DB_POSTGRESDB_USER=n8n_user
DB_POSTGRESDB_PASSWORD=changeme

# External APIs
FACTURACION_API_URL=https://api.facturacion.com
FACTURACION_API_KEY=your-api-key-here

# Timezone
GENERIC_TIMEZONE=America/Bogota

# Logging
N8N_LOG_LEVEL=info
N8N_LOG_OUTPUT=console,file
```

## Seguridad

### ⚠️ IMPORTANTE

1. **NUNCA** commitear credenciales reales
2. Usar `.gitignore` para excluir archivos sensibles:
   ```
   config/development/config.json
   config/staging/config.json
   config/production/config.json
   .env
   .env.local
   credentials.json
   ```

3. Usar variables de entorno en producción
4. Rotar credenciales regularmente
5. Usar servicios de gestión de secretos (AWS Secrets Manager, Azure Key Vault, etc.)

## Validación de Configuración

Script para validar configuración:

```javascript
// validate-config.js
const config = require('./config.json');

function validateConfig(config) {
  const required = ['environment', 'n8n', 'database'];
  
  for (const field of required) {
    if (!config[field]) {
      throw new Error(`Missing required config field: ${field}`);
    }
  }
  
  console.log('Configuration is valid ✓');
}

validateConfig(config);
```

## Templates

Ver `config/templates/` para templates de configuración que pueden copiarse y adaptarse:

- `config.development.template.json`
- `config.staging.template.json`
- `config.production.template.json`
- `.env.template`

## Documentación Relacionada

- **Guía de Despliegue**: `docs/manuales/guia_despliegue.md`
- **Documentación Técnica**: `docs/tecnicas/`
- **Variables de n8n**: [n8n Environment Variables](https://docs.n8n.io/hosting/environment-variables/)

## Troubleshooting

### Configuración no se carga

1. Verificar que el archivo existe
2. Verificar formato JSON válido
3. Verificar permisos de lectura
4. Revisar logs de inicio de aplicación

### Variables de entorno no se leen

1. Verificar que archivo `.env` está en la raíz
2. Verificar sintaxis del archivo `.env`
3. Reiniciar la aplicación después de cambios
4. Verificar que la aplicación carga dotenv

## Contacto

Para configuraciones específicas de ambiente, contactar al equipo de DevOps.
