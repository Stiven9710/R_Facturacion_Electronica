# Guía de Despliegue - Sistema de Facturación Electrónica

## Tabla de Contenidos
1. [Requisitos Previos](#requisitos-previos)
2. [Configuración de Ambientes](#configuración-de-ambientes)
3. [Despliegue en Desarrollo](#despliegue-en-desarrollo)
4. [Despliegue en Staging](#despliegue-en-staging)
5. [Despliegue en Producción](#despliegue-en-producción)
6. [Verificación Post-Despliegue](#verificación-post-despliegue)
7. [Rollback](#rollback)
8. [Troubleshooting](#troubleshooting)

## Requisitos Previos

### Software Requerido
- **n8n**: Versión 1.0.0 o superior
- **Node.js**: Versión 18.x o superior
- **npm**: Versión 9.x o superior
- **Git**: Para control de versiones
- **Docker** (opcional): Para despliegue en contenedores

### Accesos Necesarios
- [ ] Acceso al repositorio Git
- [ ] Credenciales de n8n
- [ ] Acceso a APIs externas
- [ ] Credenciales de base de datos
- [ ] Acceso a servicios de facturación

## Configuración de Ambientes

### Variables de Entorno

Crear archivo `.env` basado en `.env.example`:

```bash
# n8n Configuration
N8N_HOST=0.0.0.0
N8N_PORT=5678
N8N_PROTOCOL=https
N8N_BASIC_AUTH_ACTIVE=true
N8N_BASIC_AUTH_USER=admin
N8N_BASIC_AUTH_PASSWORD=

# Database
DB_TYPE=postgresdb
DB_POSTGRESDB_HOST=localhost
DB_POSTGRESDB_PORT=5432
DB_POSTGRESDB_DATABASE=n8n
DB_POSTGRESDB_USER=
DB_POSTGRESDB_PASSWORD=

# External Services
FACTURACION_API_URL=
FACTURACION_API_KEY=

# Timezone
GENERIC_TIMEZONE=America/Bogota
```

### Configuración por Ambiente

#### Development
```bash
NODE_ENV=development
N8N_LOG_LEVEL=debug
```

#### Staging
```bash
NODE_ENV=staging
N8N_LOG_LEVEL=info
```

#### Production
```bash
NODE_ENV=production
N8N_LOG_LEVEL=warn
N8N_METRICS=true
```

## Despliegue en Desarrollo

### 1. Instalación Local

```bash
# Clonar repositorio
git clone https://github.com/Stiven9710/R_Facturacion_Electronica.git
cd R_Facturacion_Electronica

# Instalar n8n
npm install -g n8n

# Configurar variables de entorno
cp deploy/development/.env.example .env
# Editar .env con tus configuraciones
```

### 2. Iniciar n8n

```bash
# Iniciar n8n
n8n start

# Acceder a la interfaz
# http://localhost:5678
```

### 3. Importar Workflows

```bash
# Opción 1: Desde la UI de n8n
# Settings > Import from File > Seleccionar archivo de src/workflows/

# Opción 2: Usando CLI de n8n
n8n import:workflow --input=src/workflows/workflow-name.json
```

### 4. Configurar Credenciales

1. Acceder a n8n UI
2. Ir a Credentials
3. Agregar las credenciales necesarias según `docs/tecnicas/`

## Despliegue en Staging

### Usando Docker

```bash
# Navegar al directorio de staging
cd deploy/staging

# Construir imagen
docker build -t facturacion-n8n:staging .

# Ejecutar contenedor
docker-compose up -d

# Verificar logs
docker-compose logs -f n8n
```

### Configuración

```bash
# Importar workflows
docker exec -it n8n n8n import:workflow --input=/data/workflows/

# Verificar estado
docker exec -it n8n n8n list:workflow
```

## Despliegue en Producción

### Pre-Despliegue Checklist

- [ ] Backup de workflows existentes
- [ ] Backup de base de datos
- [ ] Verificar configuraciones de producción
- [ ] Pruebas en staging completadas
- [ ] Documentación actualizada
- [ ] Plan de rollback preparado

### Pasos de Despliegue

#### 1. Backup

```bash
# Backup de workflows
n8n export:workflow --all --output=backups/workflows-$(date +%Y%m%d).json

# Backup de credenciales (sin valores sensibles)
n8n export:credentials --all --output=backups/credentials-$(date +%Y%m%d).json
```

#### 2. Detener Servicio

```bash
# Si usando systemd
sudo systemctl stop n8n

# Si usando Docker
docker-compose down
```

#### 3. Actualizar Código

```bash
# Hacer pull de cambios
git fetch origin
git checkout production
git pull origin production

# Verificar versión
git log -1
```

#### 4. Actualizar Workflows

```bash
# Importar workflows actualizados
n8n import:workflow --input=src/workflows/ --replace

# Verificar workflows
n8n list:workflow
```

#### 5. Iniciar Servicio

```bash
# Si usando systemd
sudo systemctl start n8n
sudo systemctl status n8n

# Si usando Docker
docker-compose up -d
docker-compose ps
```

## Verificación Post-Despliegue

### Verificaciones Automáticas

```bash
# Verificar conectividad
curl -f http://localhost:5678/healthz || exit 1

# Verificar workflows activos
n8n list:workflow --active

# Verificar logs
tail -f ~/.n8n/logs/n8n.log
```

### Verificaciones Manuales

1. **Acceso a UI**
   - [ ] Login funciona correctamente
   - [ ] Workflows visibles

2. **Workflows Críticos**
   - [ ] Workflow de generación de facturas
   - [ ] Workflow de notificaciones
   - [ ] Workflow de sincronización

3. **Integraciones**
   - [ ] Conexión a API de facturación
   - [ ] Conexión a base de datos
   - [ ] Webhooks funcionando

4. **Monitoreo**
   - [ ] Métricas disponibles
   - [ ] Alertas configuradas
   - [ ] Logs generándose

### Pruebas de Humo

```bash
# Ejecutar workflow de prueba
n8n execute --id=[WORKFLOW_ID]

# Verificar resultado
# Check logs y output
```

## Rollback

### Procedimiento de Rollback

#### 1. Identificar Versión Anterior

```bash
# Ver commits recientes
git log --oneline -10

# Identificar commit previo estable
git show [COMMIT_HASH]
```

#### 2. Revertir Código

```bash
# Revertir a commit anterior
git revert [COMMIT_HASH]
# o
git reset --hard [COMMIT_HASH]
git push origin production --force
```

#### 3. Restaurar Workflows

```bash
# Detener n8n
sudo systemctl stop n8n

# Restaurar workflows desde backup
n8n import:workflow --input=backups/workflows-YYYYMMDD.json --replace

# Reiniciar n8n
sudo systemctl start n8n
```

#### 4. Verificar Rollback

```bash
# Verificar versión
git log -1

# Verificar workflows
n8n list:workflow

# Pruebas de humo
n8n execute --id=[WORKFLOW_ID]
```

## Troubleshooting

### Problema: n8n no inicia

**Síntomas**: Servicio no se levanta

**Solución**:
```bash
# Verificar logs
journalctl -u n8n -n 50

# Verificar puerto
netstat -tulpn | grep 5678

# Verificar permisos
ls -la ~/.n8n/
```

### Problema: Workflow falla al ejecutar

**Síntomas**: Error en ejecución de workflow

**Solución**:
```bash
# Verificar logs del workflow
# En UI: Executions > Ver detalles

# Verificar credenciales
n8n list:credentials

# Probar conexión a APIs
curl -X POST [API_URL] -H "Authorization: Bearer [TOKEN]"
```

### Problema: Performance degradado

**Síntomas**: Respuestas lentas

**Solución**:
```bash
# Verificar recursos
top
df -h

# Verificar conexiones DB
# [Comando según DB utilizada]

# Revisar logs
tail -f ~/.n8n/logs/n8n.log | grep "ERROR"
```

### Problema: Webhooks no funcionan

**Síntomas**: No se reciben eventos de webhook

**Solución**:
```bash
# Verificar URL de webhook
n8n list:workflow --active

# Verificar configuración de red
curl -X POST [WEBHOOK_URL] -d '{"test": "data"}'

# Verificar logs de entrada
tail -f ~/.n8n/logs/n8n.log | grep "webhook"
```

## Contacto y Soporte

Para problemas durante el despliegue:
- **Equipo Técnico**: [email]
- **Documentación**: `docs/tecnicas/`
- **Issues**: [GitHub Issues URL]

## Historial de Despliegues

| Fecha | Versión | Ambiente | Responsable | Notas |
|-------|---------|----------|-------------|-------|
| DD/MM/YYYY | 1.0.0 | Production | [Nombre] | Despliegue inicial |
