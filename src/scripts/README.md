# Scripts de Utilidad

Este directorio contiene scripts auxiliares para automatización de tareas comunes.

## Categorías de Scripts

### Despliegue
Scripts para automatizar procesos de despliegue:
- `deploy.sh` - Script maestro de despliegue
- `backup.sh` - Backup de workflows y configuración
- `restore.sh` - Restaurar desde backup

### Migración
Scripts para migrar datos y configuraciones:
- `migrate-workflows.js` - Migrar workflows entre ambientes
- `migrate-credentials.js` - Migrar credenciales

### Utilidades
Scripts de uso general:
- `export-all-workflows.sh` - Exportar todos los workflows
- `import-workflows.sh` - Importar workflows en batch
- `validate-config.js` - Validar archivos de configuración
- `check-health.sh` - Verificar salud del sistema

### Desarrollo
Scripts para desarrollo:
- `setup-dev.sh` - Configurar ambiente de desarrollo
- `start-dev.sh` - Iniciar n8n en modo desarrollo
- `test.sh` - Ejecutar tests

## Uso de Scripts

### Permisos

Dar permisos de ejecución:
```bash
chmod +x src/scripts/*.sh
```

### Ejecutar Scripts

```bash
# Desde la raíz del proyecto
./src/scripts/deploy.sh

# O desde el directorio de scripts
cd src/scripts
./deploy.sh
```

## Scripts Principales

### deploy.sh

Despliega el sistema a un ambiente específico.

**Uso:**
```bash
./src/scripts/deploy.sh [environment]

# Ejemplos:
./src/scripts/deploy.sh development
./src/scripts/deploy.sh staging
./src/scripts/deploy.sh production
```

**Opciones:**
- `--skip-backup` - Saltar backup pre-despliegue
- `--force` - Forzar despliegue sin confirmación

### backup.sh

Crea backup de workflows, credenciales y configuración.

**Uso:**
```bash
./src/scripts/backup.sh [output-directory]

# Ejemplo:
./src/scripts/backup.sh backups/$(date +%Y%m%d)
```

### export-all-workflows.sh

Exporta todos los workflows activos.

**Uso:**
```bash
./src/scripts/export-all-workflows.sh

# Los workflows se exportan a src/workflows/
```

### validate-config.js

Valida archivos de configuración.

**Uso:**
```bash
node src/scripts/validate-config.js [config-file]

# Ejemplo:
node src/scripts/validate-config.js src/config/development/config.json
```

## Desarrollo de Scripts

### Template para Scripts Bash

```bash
#!/bin/bash

# Script: nombre-script.sh
# Descripción: Descripción del script
# Autor: Nombre
# Fecha: DD/MM/YYYY

set -e  # Exit on error
set -u  # Exit on undefined variable

# Variables
SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
PROJECT_ROOT="$(cd "$SCRIPT_DIR/../.." && pwd)"

# Funciones
function log_info() {
    echo "[INFO] $1"
}

function log_error() {
    echo "[ERROR] $1" >&2
}

function log_success() {
    echo "[SUCCESS] $1"
}

# Main
function main() {
    log_info "Iniciando script..."
    
    # Lógica del script aquí
    
    log_success "Script completado exitosamente"
}

# Ejecutar main
main "$@"
```

### Template para Scripts Node.js

```javascript
#!/usr/bin/env node

/**
 * Script: nombre-script.js
 * Descripción: Descripción del script
 * Autor: Nombre
 * Fecha: DD/MM/YYYY
 */

const fs = require('fs');
const path = require('path');

// Configuración
const CONFIG = {
  projectRoot: path.resolve(__dirname, '../..'),
  // Otras configuraciones
};

// Funciones auxiliares
function logInfo(message) {
  console.log(`[INFO] ${message}`);
}

function logError(message) {
  console.error(`[ERROR] ${message}`);
}

function logSuccess(message) {
  console.log(`[SUCCESS] ${message}`);
}

// Función principal
async function main() {
  try {
    logInfo('Iniciando script...');
    
    // Lógica del script aquí
    
    logSuccess('Script completado exitosamente');
  } catch (error) {
    logError(`Error: ${error.message}`);
    process.exit(1);
  }
}

// Ejecutar
if (require.main === module) {
  main();
}

module.exports = { main };
```

## Convenciones

### Nombres de Archivos
- Scripts bash: `kebab-case.sh`
- Scripts Node.js: `kebab-case.js`
- Scripts Python: `snake_case.py`

### Código
- Comentar adecuadamente
- Validar inputs
- Manejo de errores robusto
- Logging informativo
- Exit codes apropiados

### Exit Codes
- `0` - Éxito
- `1` - Error general
- `2` - Error de uso/parámetros
- Otros códigos según necesidad

## Testing de Scripts

### Bash Scripts

```bash
# Usar shellcheck para validar sintaxis
shellcheck src/scripts/deploy.sh

# Probar en ambiente seguro
./src/scripts/deploy.sh --dry-run
```

### Node.js Scripts

```javascript
// test/scripts/script-name.test.js
const { main } = require('../../src/scripts/script-name');

describe('script-name', () => {
  test('should execute successfully', async () => {
    await expect(main()).resolves.not.toThrow();
  });
});
```

## Seguridad

⚠️ **Consideraciones de Seguridad:**

1. No incluir credenciales hardcodeadas
2. Validar todos los inputs
3. Usar variables de entorno para secretos
4. Sanitizar rutas de archivos
5. Verificar permisos antes de ejecutar

## Documentación

Cada script debe tener:
- Comentarios de encabezado con descripción
- Uso y ejemplos
- Lista de dependencias
- Documentación de parámetros

## Lista de Scripts

| Script | Tipo | Descripción | Autor | Última Actualización |
|--------|------|-------------|-------|---------------------|
| - | - | - | - | - |

## Recursos

- [Bash Best Practices](https://bertvv.github.io/cheat-sheets/Bash.html)
- [Node.js Scripts](https://nodejs.org/en/knowledge/command-line/how-to-parse-command-line-arguments/)

## Troubleshooting

### Script no ejecuta

1. Verificar permisos: `ls -la script.sh`
2. Dar permisos: `chmod +x script.sh`
3. Verificar shebang: Primera línea debe ser `#!/bin/bash` o `#!/usr/bin/env node`

### Error de dependencias

1. Verificar Node.js instalado: `node --version`
2. Instalar dependencias: `npm install`
3. Verificar PATH

## Contacto

Para scripts específicos, contactar al autor indicado en el encabezado del script.
