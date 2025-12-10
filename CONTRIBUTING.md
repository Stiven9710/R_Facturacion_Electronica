# Guía de Contribución

¡Gracias por tu interés en contribuir al Sistema de Facturación Electrónica! Este documento proporciona las pautas para contribuir al proyecto.

## Tabla de Contenidos

1. [Código de Conducta](#código-de-conducta)
2. [Cómo Empezar](#cómo-empezar)
3. [Proceso de Desarrollo](#proceso-de-desarrollo)
4. [Estándares de Código](#estándares-de-código)
5. [Proceso de Pull Request](#proceso-de-pull-request)
6. [Reporte de Bugs](#reporte-de-bugs)
7. [Sugerencias de Features](#sugerencias-de-features)

## Código de Conducta

Este proyecto y todos los participantes están regidos por un código de conducta profesional. Se espera que todos los colaboradores:

- Sean respetuosos y profesionales
- Acepten críticas constructivas
- Se enfoquen en lo mejor para la comunidad
- Muestren empatía hacia otros miembros

## Cómo Empezar

### 1. Fork y Clone

```bash
# Fork el repositorio en GitHub
# Luego clona tu fork
git clone https://github.com/TU-USUARIO/R_Facturacion_Electronica.git
cd R_Facturacion_Electronica

# Agregar upstream remote
git remote add upstream https://github.com/Stiven9710/R_Facturacion_Electronica.git
```

### 2. Configurar Ambiente

```bash
# Instalar dependencias
npm install

# Instalar n8n
npm install -g n8n

# Copiar configuración
cp src/config/.env.template .env
# Editar .env con tus configuraciones
```

### 3. Crear Branch

```bash
# Actualizar desde upstream
git fetch upstream
git checkout develop
git merge upstream/develop

# Crear branch para tu feature/fix
git checkout -b feature/nombre-descriptivo
# o
git checkout -b fix/nombre-del-bug
```

## Proceso de Desarrollo

### 1. Análisis

- Revisar la issue o requirement
- Leer documentación relacionada
- Entender el contexto del cambio

### 2. Diseño

- Planificar la solución
- Considerar impacto en componentes existentes
- Documentar decisiones de diseño si es necesario

### 3. Implementación

- Escribir código siguiendo estándares
- Agregar comentarios cuando sea necesario
- Mantener commits pequeños y atómicos

### 4. Testing

- Escribir tests unitarios
- Agregar tests de integración si aplica
- Asegurar que todos los tests pasen
- Verificar coverage

### 5. Documentación

- Actualizar README si aplica
- Actualizar documentación técnica
- Documentar APIs o cambios significativos
- Agregar comentarios en código complejo

## Estándares de Código

### Convenciones de Nombres

#### Workflows n8n
```
[Categoría]-[ID]-[Nombre-Descriptivo]
Ejemplo: FAC-001-Generar-Factura-Electronica
```

#### Variables JavaScript
```javascript
// camelCase para variables y funciones
const numeroFactura = 123;
function calcularTotal() {}

// PascalCase para clases
class FacturaService {}

// UPPER_SNAKE_CASE para constantes
const MAX_INTENTOS = 3;
```

#### Archivos
```
# kebab-case para archivos
nombre-archivo.js
componente-facturacion.md

# PascalCase para componentes/clases
FacturaService.js
```

### Estilo de Código JavaScript

```javascript
// Usar const/let, no var
const valor = 10;
let contador = 0;

// Usar arrow functions
const sumar = (a, b) => a + b;

// Destructuring
const { nombre, email } = usuario;

// Template literals
const mensaje = `Hola ${nombre}`;

// Async/await sobre callbacks
async function obtenerDatos() {
  const resultado = await api.getData();
  return resultado;
}

// Manejo de errores
try {
  await operacionRiesgosa();
} catch (error) {
  console.error('Error:', error.message);
  throw error;
}
```

### Comentarios

```javascript
// ✅ Comentar el "por qué", no el "qué"
// Usamos timeout de 5s porque el servicio externo es lento
const TIMEOUT = 5000;

// ❌ Evitar comentarios obvios
// Incrementar contador
contador++;

// ✅ Documentar funciones complejas
/**
 * Calcula el total de una factura incluyendo impuestos
 * @param {Array} items - Items de la factura
 * @param {number} tasaImpuesto - Tasa de impuesto (0-1)
 * @returns {number} Total calculado
 */
function calcularTotal(items, tasaImpuesto) {
  // implementación
}
```

### Git Commits

Seguir [Conventional Commits](https://www.conventionalcommits.org/):

```bash
# Formato
<type>(<scope>): <subject>

<body>

<footer>

# Types
feat:     Nueva funcionalidad
fix:      Bug fix
docs:     Cambios en documentación
style:    Formato, punto y coma faltantes, etc.
refactor: Refactorización de código
test:     Agregar tests
chore:    Actualizar tareas de build, configs, etc.

# Ejemplos
feat(facturacion): agregar validación de NIT

Implementa validación de formato de NIT según normativa DIAN
antes de generar factura electrónica.

Closes #123

fix(notificaciones): corregir envío de emails

El servicio SMTP no estaba usando TLS correctamente.

Fixes #456
```

## Proceso de Pull Request

### 1. Antes de Crear el PR

```bash
# Actualizar desde upstream
git fetch upstream
git rebase upstream/develop

# Ejecutar tests
npm test

# Verificar lint (si aplica)
npm run lint

# Verificar que no hay conflictos
git status
```

### 2. Crear el PR

1. Push a tu fork:
   ```bash
   git push origin feature/nombre-descriptivo
   ```

2. Ir a GitHub y crear Pull Request

3. Completar template de PR:
   ```markdown
   ## Descripción
   Breve descripción de los cambios

   ## Tipo de Cambio
   - [ ] Bug fix
   - [ ] Nueva funcionalidad
   - [ ] Breaking change
   - [ ] Documentación

   ## Testing
   - [ ] Tests unitarios agregados/actualizados
   - [ ] Tests de integración agregados/actualizados
   - [ ] Tests pasan localmente

   ## Checklist
   - [ ] Código sigue estándares del proyecto
   - [ ] Comentarios agregados en código complejo
   - [ ] Documentación actualizada
   - [ ] Sin warnings en consola
   - [ ] Tests pasan
   ```

### 3. Durante la Revisión

- Responder a comentarios promptamente
- Hacer cambios solicitados
- Mantener conversación profesional
- Agregar commits adicionales según feedback

### 4. Después de Aprobación

- Hacer squash de commits si es necesario
- Asegurar que branch está actualizado
- El maintainer hará merge

## Reporte de Bugs

### Template de Bug Report

```markdown
**Descripción del Bug**
Descripción clara y concisa del bug.

**Pasos para Reproducir**
1. Ir a '...'
2. Ejecutar workflow '...'
3. Ver error

**Comportamiento Esperado**
Descripción de lo que esperabas que sucediera.

**Comportamiento Actual**
Descripción de lo que realmente sucede.

**Screenshots**
Si aplica, agregar screenshots.

**Ambiente**
- OS: [e.g. Ubuntu 20.04]
- n8n version: [e.g. 1.0.0]
- Node.js version: [e.g. 18.0.0]

**Logs**
```
Agregar logs relevantes aquí
```

**Contexto Adicional**
Cualquier otra información relevante.
```

## Sugerencias de Features

### Template de Feature Request

```markdown
**¿Es tu feature request relacionada a un problema?**
Descripción clara del problema. Ej: Siempre me frustro cuando [...]

**Solución Propuesta**
Descripción clara de lo que quieres que suceda.

**Alternativas Consideradas**
Descripción de soluciones alternativas que consideraste.

**Contexto Adicional**
Cualquier otra información o screenshots sobre la feature.

**Beneficio**
¿Cómo beneficiará esto al proyecto?
```

## Proceso de Review

### Para Reviewers

- Ser constructivo y respetuoso
- Explicar el "por qué" de los comentarios
- Distinguir entre "must-have" y "nice-to-have"
- Aprobar cuando el código cumple estándares

### Para Contributors

- No tomar comentarios como personales
- Pedir clarificación si no entiendes
- Agradecer el tiempo del reviewer
- Implementar feedback o explicar por qué no

## Recursos

- [Documentación del Proyecto](docs/)
- [Guía de n8n](https://docs.n8n.io/)
- [JavaScript Style Guide](https://github.com/airbnb/javascript)
- [Conventional Commits](https://www.conventionalcommits.org/)

## Preguntas

Si tienes preguntas:
1. Revisa la documentación
2. Busca en issues existentes
3. Crea una nueva issue con label "question"
4. Contacta a maintainers

## Reconocimientos

Todos los contribuidores son reconocidos a través de:
- GitHub Contributors page
- Commits en el historial del proyecto
- Menciones en releases notes

¡Gracias por contribuir! 🎉
