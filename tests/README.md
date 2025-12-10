# Tests

Este directorio contiene todos los tests del sistema de facturación electrónica.

## Estructura

```
tests/
├── unit/              # Pruebas unitarias
├── integration/       # Pruebas de integración
├── e2e/              # Pruebas end-to-end
├── fixtures/         # Datos de prueba
├── helpers/          # Utilidades para testing
└── README.md         # Este archivo
```

## Tipos de Tests

### Unit Tests (`unit/`)
Pruebas de funciones y componentes individuales de forma aislada.

**Ejemplos:**
- Validación de funciones de transformación
- Lógica de negocio
- Utilidades y helpers

### Integration Tests (`integration/`)
Pruebas de integración entre componentes y sistemas externos.

**Ejemplos:**
- Workflows completos de n8n
- Integración con APIs externas
- Flujo de datos entre nodos

### End-to-End Tests (`e2e/`)
Pruebas de escenarios completos de usuario.

**Ejemplos:**
- Proceso completo de facturación
- Flujo de notificaciones
- Generación de reportes

## Stack de Testing

### Herramientas
- **Jest**: Framework de testing principal
- **Supertest**: Testing de APIs HTTP
- **nock**: Mock de llamadas HTTP
- **n8n Test Helpers**: Utilidades para testing de workflows

### Configuración

```javascript
// jest.config.js
module.exports = {
  testEnvironment: 'node',
  coverageDirectory: 'coverage',
  collectCoverageFrom: [
    'src/**/*.{js,ts}',
    '!src/**/*.test.{js,ts}',
  ],
  testMatch: [
    '**/tests/**/*.test.{js,ts}',
  ],
  coverageThreshold: {
    global: {
      branches: 80,
      functions: 80,
      lines: 80,
      statements: 80,
    },
  },
};
```

## Ejecutar Tests

### Todos los Tests

```bash
# Ejecutar todos los tests
npm test

# Con coverage
npm test -- --coverage

# En modo watch
npm test -- --watch
```

### Tests Específicos

```bash
# Unit tests
npm test -- tests/unit

# Integration tests
npm test -- tests/integration

# E2E tests
npm test -- tests/e2e

# Test específico
npm test -- tests/unit/validators.test.js
```

### Por Patrón

```bash
# Tests que contengan "factura"
npm test -- --testNamePattern="factura"

# Tests en archivos que contengan "workflow"
npm test -- --testPathPattern="workflow"
```

## Escribir Tests

### Unit Test Ejemplo

```javascript
// tests/unit/validators.test.js
const { validateNIT } = require('../../src/utils/validators');

describe('validateNIT', () => {
  test('should validate correct NIT', () => {
    const nit = '900123456-1';
    expect(validateNIT(nit)).toBe(true);
  });

  test('should reject invalid NIT', () => {
    const nit = 'invalid';
    expect(validateNIT(nit)).toBe(false);
  });

  test('should handle null input', () => {
    expect(validateNIT(null)).toBe(false);
  });
});
```

### Integration Test Ejemplo

```javascript
// tests/integration/workflows/facturacion.test.js
const { executeWorkflow } = require('../../helpers/n8n-helper');

describe('Workflow: Generar Factura', () => {
  test('should generate invoice successfully', async () => {
    const input = {
      cliente: {
        nit: '900123456-1',
        nombre: 'Cliente Test'
      },
      items: [
        { descripcion: 'Producto 1', valor: 1000 }
      ]
    };

    const result = await executeWorkflow('FAC-001-Generar-Factura', input);

    expect(result.status).toBe('success');
    expect(result.data).toHaveProperty('numeroFactura');
  });

  test('should handle invalid NIT', async () => {
    const input = {
      cliente: {
        nit: 'invalid',
        nombre: 'Cliente Test'
      }
    };

    await expect(
      executeWorkflow('FAC-001-Generar-Factura', input)
    ).rejects.toThrow('NIT inválido');
  });
});
```

### E2E Test Ejemplo

```javascript
// tests/e2e/facturacion-completa.test.js
const request = require('supertest');

describe('E2E: Proceso Completo de Facturación', () => {
  test('should complete full billing process', async () => {
    // 1. Crear cliente
    const cliente = await request(API_URL)
      .post('/clientes')
      .send({ nit: '900123456-1', nombre: 'Cliente Test' });

    expect(cliente.status).toBe(201);

    // 2. Generar factura
    const factura = await request(API_URL)
      .post('/facturas')
      .send({
        clienteId: cliente.body.id,
        items: [{ descripcion: 'Producto', valor: 1000 }]
      });

    expect(factura.status).toBe(201);
    expect(factura.body).toHaveProperty('numeroFactura');

    // 3. Verificar notificación enviada
    // ... más aserciones
  });
});
```

## Fixtures y Mocks

### Fixtures

```javascript
// tests/fixtures/clientes.js
module.exports = {
  clienteValido: {
    nit: '900123456-1',
    nombre: 'Cliente Test',
    email: 'test@example.com'
  },
  clienteInvalido: {
    nit: 'invalid',
    nombre: '',
    email: 'not-an-email'
  }
};
```

### Mocks

```javascript
// tests/helpers/mocks.js
const nock = require('nock');

function mockDianAPI() {
  nock('https://api.dian.gov.co')
    .post('/validar-factura')
    .reply(200, {
      status: 'approved',
      cude: 'ABC123'
    });
}

module.exports = { mockDianAPI };
```

## Coverage

### Generar Reporte

```bash
# Generar coverage
npm test -- --coverage

# Ver reporte HTML
open coverage/lcov-report/index.html
```

### Umbrales de Coverage

- **Global**: 80%
- **Código crítico** (validaciones, transformaciones): 90%
- **Código de infraestructura**: 70%

## CI/CD Integration

### GitHub Actions

```yaml
# .github/workflows/test.yml
name: Tests

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v2
      - run: npm install
      - run: npm test -- --coverage
      - uses: codecov/codecov-action@v2
```

## Mejores Prácticas

### 1. Nombres Descriptivos

```javascript
// ✅ Bueno
test('should reject invoice with invalid NIT format', () => {});

// ❌ Malo
test('test1', () => {});
```

### 2. Arrange-Act-Assert

```javascript
test('should calculate total correctly', () => {
  // Arrange
  const items = [
    { precio: 100, cantidad: 2 },
    { precio: 50, cantidad: 1 }
  ];

  // Act
  const total = calculateTotal(items);

  // Assert
  expect(total).toBe(250);
});
```

### 3. Tests Independientes

```javascript
// ✅ Cada test es independiente
describe('User Service', () => {
  beforeEach(() => {
    // Setup fresh state for each test
  });

  test('test 1', () => {});
  test('test 2', () => {});
});
```

### 4. Mock Dependencias Externas

```javascript
// Mock de API externa
jest.mock('../../src/api/dian-client');
```

### 5. Tests Rápidos

- Unit tests deben ejecutar en < 100ms
- Integration tests en < 5s
- E2E tests en < 30s

## Debugging Tests

### Ejecutar en Debug Mode

```bash
# Node.js inspector
node --inspect-brk node_modules/.bin/jest --runInBand

# VS Code launch.json
{
  "type": "node",
  "request": "launch",
  "name": "Jest Debug",
  "program": "${workspaceFolder}/node_modules/.bin/jest",
  "args": ["--runInBand"],
  "console": "integratedTerminal"
}
```

### Logging en Tests

```javascript
// Usar console.log temporalmente
test('debug test', () => {
  const result = processData(input);
  console.log('Result:', JSON.stringify(result, null, 2));
  expect(result).toBeDefined();
});
```

## Troubleshooting

### Tests Fallan Intermitentemente

1. Verificar dependencias de tiempo
2. Asegurar independencia de tests
3. Revisar mocks y fixtures
4. Verificar estado compartido

### Tests Lentos

1. Optimizar setup/teardown
2. Usar mocks en lugar de servicios reales
3. Ejecutar tests en paralelo
4. Revisar queries a BD

### Coverage Bajo

1. Identificar código sin cubrir
2. Agregar tests para casos edge
3. Revisar branches no testeadas
4. Considerar complejidad del código

## Recursos

- [Jest Documentation](https://jestjs.io/)
- [Testing Best Practices](https://github.com/goldbergyoni/javascript-testing-best-practices)
- [n8n Testing](https://docs.n8n.io/hosting/testing/)

## Checklist Pre-Commit

Antes de hacer commit, verificar:

- [ ] Todos los tests pasan
- [ ] Coverage cumple umbrales
- [ ] No hay tests skipped sin razón
- [ ] Tests nuevos para código nuevo
- [ ] Tests descriptivos y bien organizados

## Contacto

Para dudas sobre testing, contactar al equipo de QA o Tech Lead.
