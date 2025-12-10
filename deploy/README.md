# Configuraciones de Despliegue

Este directorio contiene las configuraciones específicas para cada ambiente de despliegue.

## Estructura

```
deploy/
├── development/       # Ambiente de desarrollo
├── staging/          # Ambiente de staging/QA
├── production/       # Ambiente de producción
└── README.md         # Este archivo
```

## Ambientes

### Development

**Propósito**: Desarrollo activo y pruebas locales

**Características**:
- Base de datos local o compartida de desarrollo
- Logs en modo debug
- Hot reload habilitado
- Credenciales de prueba
- APIs mock cuando sea posible

**Acceso**: Desarrolladores

### Staging

**Propósito**: Pruebas de integración y validación antes de producción

**Características**:
- Réplica de ambiente de producción
- Datos de prueba realistas
- Logs en modo info
- Credenciales de staging de proveedores
- APIs reales en modo sandbox

**Acceso**: Desarrolladores, QA, Stakeholders

### Production

**Propósito**: Ambiente productivo

**Características**:
- Alta disponibilidad
- Logs en modo warn/error
- Monitoreo completo
- Backups automáticos
- Credenciales productivas
- APIs productivas

**Acceso**: Limitado (DevOps, Tech Lead)

## Archivos de Configuración

Cada ambiente contiene:

```
environment/
├── docker-compose.yml    # Configuración Docker
├── .env.example         # Template de variables de entorno
├── nginx.conf           # Configuración de proxy
└── README.md            # Documentación específica del ambiente
```

## Variables de Entorno por Ambiente

### Development

```bash
NODE_ENV=development
N8N_HOST=localhost
N8N_PORT=5678
N8N_PROTOCOL=http
DB_TYPE=sqlite
N8N_LOG_LEVEL=debug
N8N_METRICS=false
```

### Staging

```bash
NODE_ENV=staging
N8N_HOST=staging.facturacion.example.com
N8N_PORT=443
N8N_PROTOCOL=https
DB_TYPE=postgresdb
N8N_LOG_LEVEL=info
N8N_METRICS=true
N8N_BASIC_AUTH_ACTIVE=true
```

### Production

```bash
NODE_ENV=production
N8N_HOST=facturacion.example.com
N8N_PORT=443
N8N_PROTOCOL=https
DB_TYPE=postgresdb
N8N_LOG_LEVEL=warn
N8N_METRICS=true
N8N_BASIC_AUTH_ACTIVE=true
EXECUTIONS_DATA_PRUNE=true
N8N_CONCURRENCY_PRODUCTION_LIMIT=10
```

## Despliegue

### Docker Compose

Cada ambiente tiene su propio `docker-compose.yml`:

```yaml
version: '3.8'

services:
  n8n:
    image: n8nio/n8n:latest
    container_name: n8n-${ENV}
    restart: unless-stopped
    ports:
      - "${N8N_PORT}:5678"
    environment:
      - NODE_ENV=${NODE_ENV}
      - N8N_BASIC_AUTH_ACTIVE=${N8N_BASIC_AUTH_ACTIVE}
      - N8N_BASIC_AUTH_USER=${N8N_BASIC_AUTH_USER}
      - N8N_BASIC_AUTH_PASSWORD=${N8N_BASIC_AUTH_PASSWORD}
      - DB_TYPE=${DB_TYPE}
      - DB_POSTGRESDB_HOST=postgres
      - DB_POSTGRESDB_PORT=5432
      - DB_POSTGRESDB_DATABASE=${DB_POSTGRESDB_DATABASE}
      - DB_POSTGRESDB_USER=${DB_POSTGRESDB_USER}
      - DB_POSTGRESDB_PASSWORD=${DB_POSTGRESDB_PASSWORD}
    volumes:
      - n8n_data:/home/node/.n8n
      - ./workflows:/workflows:ro
    depends_on:
      - postgres
    networks:
      - n8n-network

  postgres:
    image: postgres:14
    container_name: postgres-${ENV}
    restart: unless-stopped
    environment:
      - POSTGRES_DB=${DB_POSTGRESDB_DATABASE}
      - POSTGRES_USER=${DB_POSTGRESDB_USER}
      - POSTGRES_PASSWORD=${DB_POSTGRESDB_PASSWORD}
    volumes:
      - postgres_data:/var/lib/postgresql/data
    networks:
      - n8n-network

volumes:
  n8n_data:
  postgres_data:

networks:
  n8n-network:
    driver: bridge
```

### Comandos de Despliegue

```bash
# Development
cd deploy/development
docker-compose up -d

# Staging
cd deploy/staging
docker-compose up -d

# Production
cd deploy/production
docker-compose up -d
```

## Proceso de Despliegue

### 1. Pre-Despliegue

```bash
# Verificar tests
npm test

# Backup de producción (solo para prod)
./src/scripts/backup.sh

# Revisar cambios
git log --oneline -10
```

### 2. Despliegue

```bash
# Pull últimos cambios
git pull origin [branch]

# Navegar al ambiente
cd deploy/[environment]

# Cargar variables de entorno
source .env

# Deploy
docker-compose up -d --build

# Verificar logs
docker-compose logs -f n8n
```

### 3. Post-Despliegue

```bash
# Verificar salud
curl http://localhost:5678/healthz

# Verificar workflows
docker exec n8n n8n list:workflow

# Verificar logs
docker-compose logs --tail=100 n8n
```

## Rollback

```bash
# Detener servicios
docker-compose down

# Revertir código
git checkout [commit-anterior]

# Restaurar backup (si es necesario)
./src/scripts/restore.sh [backup-file]

# Redesplegar
docker-compose up -d
```

## Monitoreo

### Logs

```bash
# Ver logs en tiempo real
docker-compose logs -f n8n

# Logs de últimas 100 líneas
docker-compose logs --tail=100 n8n

# Logs con timestamp
docker-compose logs -t n8n
```

### Health Checks

```bash
# Health check manual
curl http://localhost:5678/healthz

# Automatizado (agregar a cron)
*/5 * * * * curl -f http://localhost:5678/healthz || alert-script.sh
```

### Métricas

- **n8n metrics**: Habilitadas en staging/production
- **Prometheus**: Endpoint `/metrics`
- **Grafana**: Dashboards de monitoreo

## Backups

### Automático

```bash
# Configurar cron job
0 2 * * * /path/to/backup.sh production
```

### Manual

```bash
# Backup completo
./src/scripts/backup.sh deploy/production/backups/

# Backup solo workflows
docker exec n8n n8n export:workflow --all --output=/backups/workflows.json
```

## Seguridad

### Checklist de Seguridad

- [ ] Variables de entorno no comiteadas
- [ ] Credenciales en secrets manager
- [ ] HTTPS habilitado en staging/production
- [ ] Basic auth habilitado
- [ ] Firewall configurado
- [ ] Backups encriptados
- [ ] Logs no contienen datos sensibles
- [ ] Rate limiting configurado

### SSL/TLS

```bash
# Usar Let's Encrypt con nginx
certbot --nginx -d facturacion.example.com
```

## Troubleshooting

### Contenedor no inicia

```bash
# Ver logs
docker-compose logs n8n

# Verificar configuración
docker-compose config

# Verificar recursos
docker stats
```

### Base de datos no conecta

```bash
# Verificar conectividad
docker exec n8n ping postgres

# Verificar credenciales
docker exec postgres psql -U $DB_USER -d $DB_NAME -c "SELECT 1"
```

### Performance Issues

```bash
# Verificar recursos
docker stats

# Escalar recursos
# Editar docker-compose.yml
resources:
  limits:
    cpus: '2'
    memory: 4G
```

## Documentación Adicional

- **Guía de Despliegue**: `docs/manuales/guia_despliegue.md`
- **Arquitectura**: `docs/tecnicas/arquitectura_general.md`
- **Runbooks**: `docs/manuales/runbooks/`

## Contacto

Para acceso a ambientes o problemas de despliegue, contactar:
- **DevOps**: [email]
- **Tech Lead**: [email]
- **On-call**: [número]
