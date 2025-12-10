# Custom Nodes n8n

Este directorio contiene nodos personalizados desarrollados para extender la funcionalidad de n8n.

## ¿Qué son los Custom Nodes?

Los custom nodes son extensiones que agregan nueva funcionalidad a n8n que no está disponible en los nodos predeterminados.

## Estructura

```
custom-nodes/
├── node-name/
│   ├── node-name.node.ts         # Código principal del nodo
│   ├── node-name.node.json       # Metadata del nodo
│   ├── GenericFunctions.ts       # Funciones auxiliares
│   ├── package.json              # Dependencias del nodo
│   └── README.md                 # Documentación del nodo
└── README.md                     # Este archivo
```

## Desarrollo de Custom Nodes

### Requisitos

- Node.js 18+
- n8n instalado globalmente
- TypeScript

### Crear un Nuevo Nodo

```bash
# Usar el CLI de n8n
n8n-node-dev new

# O clonar un template
cp -r custom-nodes/template custom-nodes/my-new-node
cd custom-nodes/my-new-node
npm install
```

### Estructura Básica

```typescript
// my-node.node.ts
import {
  IExecuteFunctions,
  INodeType,
  INodeTypeDescription,
  NodeOperationError,
} from 'n8n-workflow';

export class MyCustomNode implements INodeType {
  description: INodeTypeDescription = {
    displayName: 'My Custom Node',
    name: 'myCustomNode',
    group: ['transform'],
    version: 1,
    description: 'Descripción del nodo',
    defaults: {
      name: 'My Custom Node',
    },
    inputs: ['main'],
    outputs: ['main'],
    properties: [
      {
        displayName: 'Operation',
        name: 'operation',
        type: 'options',
        options: [
          {
            name: 'Process',
            value: 'process',
          },
        ],
        default: 'process',
      },
    ],
  };

  async execute(this: IExecuteFunctions) {
    const items = this.getInputData();
    const returnData = [];

    for (let i = 0; i < items.length; i++) {
      try {
        // Lógica del nodo
        const item = items[i];
        
        returnData.push({
          json: {
            // Datos de salida
          },
        });
      } catch (error) {
        if (this.continueOnFail()) {
          returnData.push({
            json: {
              error: error.message,
            },
          });
          continue;
        }
        throw error;
      }
    }

    return [returnData];
  }
}
```

### Compilar el Nodo

```bash
cd custom-nodes/my-node
npm run build

# El nodo compilado se genera en dist/
```

### Instalar el Nodo en n8n

```bash
# Opción 1: Link local (desarrollo)
cd ~/.n8n/custom
npm link /path/to/custom-nodes/my-node

# Opción 2: Instalar desde directorio
cd ~/.n8n/custom
npm install /path/to/custom-nodes/my-node

# Reiniciar n8n
n8n start
```

## Testing de Custom Nodes

### Unit Tests

```typescript
// my-node.test.ts
import { MyCustomNode } from './my-node.node';

describe('MyCustomNode', () => {
  let node: MyCustomNode;

  beforeEach(() => {
    node = new MyCustomNode();
  });

  test('should process data correctly', async () => {
    // Test implementation
  });
});
```

### Integration Tests

Ver `tests/integration/custom-nodes/` para ejemplos de tests de integración.

## Mejores Prácticas

### 1. Manejo de Errores

```typescript
try {
  // Código del nodo
} catch (error) {
  if (this.continueOnFail()) {
    return [{
      json: { error: error.message }
    }];
  }
  throw new NodeOperationError(this.getNode(), error);
}
```

### 2. Validación de Inputs

```typescript
const operation = this.getNodeParameter('operation', i) as string;

if (!operation) {
  throw new NodeOperationError(
    this.getNode(),
    'Operation parameter is required'
  );
}
```

### 3. Credenciales

```typescript
// Definir credenciales en el nodo
credentials: [
  {
    name: 'myApi',
    required: true,
  },
],

// Usar credenciales
const credentials = await this.getCredentials('myApi');
const apiKey = credentials.apiKey as string;
```

### 4. Logging

```typescript
// Para debugging
console.log('Processing item:', item);

// Para errores
console.error('Error processing:', error);
```

### 5. Performance

- Procesar items en batch cuando sea posible
- Usar async/await apropiadamente
- Implementar timeouts
- Cachear cuando sea necesario

## Publicación

### npm Package

```json
// package.json
{
  "name": "n8n-nodes-my-custom-node",
  "version": "1.0.0",
  "description": "Custom node for n8n",
  "main": "dist/index.js",
  "types": "dist/index.d.ts",
  "n8n": {
    "n8nNodesApiVersion": 1,
    "nodes": [
      "dist/nodes/MyCustomNode/MyCustomNode.node.js"
    ]
  }
}
```

```bash
# Publicar a npm
npm publish
```

## Lista de Custom Nodes

| Nombre | Versión | Descripción | Estado | Documentación |
|--------|---------|-------------|--------|---------------|
| - | - | - | - | - |

## Recursos

- [n8n Node Development](https://docs.n8n.io/integrations/creating-nodes/)
- [n8n API Reference](https://docs.n8n.io/integrations/creating-nodes/code/)
- [Community Nodes](https://docs.n8n.io/integrations/community-nodes/)

## Troubleshooting

### El nodo no aparece en n8n

1. Verificar que está instalado en `~/.n8n/custom/`
2. Reiniciar n8n
3. Verificar logs de error: `~/.n8n/logs/`

### Error al compilar

1. Verificar versiones de dependencias
2. Limpiar node_modules y reinstalar
3. Verificar TypeScript configuration

## Contribución

1. Desarrollar el nodo siguiendo las guías
2. Agregar tests
3. Documentar completamente
4. Crear PR para revisión
