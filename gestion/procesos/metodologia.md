# Metodología de Desarrollo - Sistema de Facturación Electrónica

## 1. Descripción General

Este documento describe la metodología de desarrollo utilizada para el sistema de agentes de facturación electrónica con n8n.

## 2. Marco de Trabajo

### 2.1 Metodología Ágil
Utilizamos una metodología ágil adaptada con elementos de Scrum y Kanban:
- **Sprints**: Iteraciones de 2 semanas
- **Daily Standups**: Reuniones diarias de seguimiento
- **Sprint Planning**: Planificación al inicio de cada sprint
- **Sprint Review**: Revisión al final de cada sprint
- **Sprint Retrospective**: Retrospectiva para mejora continua

### 2.2 Flujo de Trabajo

```
Backlog → Planning → Development → Testing → Review → Deploy → Monitor
```

## 3. Ciclo de Vida de un Componente

### 3.1 Fase 1: Análisis y Especificación

**Responsable**: Analista Funcional / Product Owner

**Entregables**:
- Especificación funcional (`docs/especificaciones/`)
- Casos de uso documentados
- Criterios de aceptación definidos

**Duración**: 2-3 días

### 3.2 Fase 2: Diseño Técnico

**Responsable**: Arquitecto / Lead Developer

**Entregables**:
- Documentación técnica (`docs/tecnicas/`)
- Diagrama de arquitectura
- Definición de APIs e integraciones

**Duración**: 1-2 días

### 3.3 Fase 3: Desarrollo

**Responsable**: Desarrollador

**Actividades**:
1. Crear branch desde `develop`
   ```bash
   git checkout -b feature/[ID]-[nombre-componente]
   ```

2. Desarrollar el workflow en n8n
   - Configurar nodos
   - Implementar lógica de negocio
   - Agregar manejo de errores

3. Exportar workflow
   ```bash
   # Guardar en src/workflows/
   ```

4. Documentar código
   - Comentarios en Function nodes
   - Actualizar documentación técnica

5. Commit changes
   ```bash
   git add .
   git commit -m "feat: [ID] descripción del cambio"
   ```

**Duración**: 3-5 días

### 3.4 Fase 4: Testing

**Responsable**: QA / Developer

**Actividades**:
1. **Pruebas Unitarias**
   - Validar lógica de funciones individuales
   - Ubicación: `tests/unit/`

2. **Pruebas de Integración**
   - Validar integración con APIs
   - Verificar flujo completo del workflow
   - Ubicación: `tests/integration/`

3. **Pruebas End-to-End**
   - Simular escenarios reales
   - Verificar casos edge
   - Ubicación: `tests/e2e/`

**Criterios de Éxito**:
- [ ] Todos los test cases pasan
- [ ] Cobertura mínima 80%
- [ ] Sin errores críticos

**Duración**: 2-3 días

### 3.5 Fase 5: Code Review

**Responsable**: Tech Lead / Senior Developer

**Checklist**:
- [ ] Código sigue estándares del proyecto
- [ ] Documentación completa y actualizada
- [ ] Tests implementados y pasando
- [ ] Sin hardcoded credentials
- [ ] Manejo de errores implementado
- [ ] Performance aceptable

**Proceso**:
1. Crear Pull Request
2. Solicitar revisión
3. Atender comentarios
4. Obtener aprobación

**Duración**: 1 día

### 3.6 Fase 6: Despliegue

**Responsable**: DevOps / Tech Lead

**Ambientes**:

1. **Development**
   - Despliegue automático desde `develop`
   - Para desarrollo y pruebas internas

2. **Staging**
   - Despliegue manual desde `staging`
   - Para validación con stakeholders

3. **Production**
   - Despliegue manual desde `main`
   - Requiere aprobación formal

**Proceso**:
```bash
# Merge a develop
git checkout develop
git merge feature/[ID]-[nombre]

# Deploy a staging
git checkout staging
git merge develop
# Ejecutar proceso de despliegue

# Deploy a production (después de validación)
git checkout main
git merge staging
# Ejecutar proceso de despliegue
```

**Duración**: 0.5-1 día

### 3.7 Fase 7: Monitoreo

**Responsable**: Todos

**Actividades**:
- Monitorear logs y métricas
- Verificar alertas
- Responder a incidentes
- Recopilar feedback

**Duración**: Continuo

## 4. Gestión de Componentes

### 4.1 Registro de Componentes

Todos los componentes deben registrarse en `gestion/componentes/registro.md`:

| ID | Nombre | Tipo | Estado | Owner |
|----|--------|------|--------|-------|
| COMP-001 | [Nombre] | Workflow | Activo | [Nombre] |

### 4.2 Estados de Componente

- **Backlog**: En lista de espera
- **En Análisis**: En fase de especificación
- **En Desarrollo**: En implementación
- **En Testing**: En pruebas
- **En Review**: En revisión de código
- **Staging**: Desplegado en staging
- **Production**: Desplegado en producción
- **Deprecated**: Obsoleto (mantener para referencia)

### 4.3 Versionado

Seguimos Semantic Versioning (MAJOR.MINOR.PATCH):
- **MAJOR**: Cambios incompatibles con versiones anteriores
- **MINOR**: Nueva funcionalidad compatible
- **PATCH**: Bug fixes compatibles

Ejemplo: `1.2.3`

## 5. Convenciones de Código

### 5.1 Naming Conventions

**Workflows**:
- Formato: `[Categoría]-[Acción]-[Entidad]`
- Ejemplo: `FAC-Generar-Factura`, `NOT-Enviar-Email`

**Variables en n8n**:
- camelCase: `numeroFactura`, `fechaEmision`
- Descriptivos y en español

**Nodos**:
- Nombres descriptivos
- Usar prefijos: `API:`, `DB:`, `Transform:`

### 5.2 Git Commits

Seguir Conventional Commits:
```
<type>(<scope>): <subject>

<body>

<footer>
```

**Types**:
- `feat`: Nueva funcionalidad
- `fix`: Bug fix
- `docs`: Cambios en documentación
- `refactor`: Refactorización
- `test`: Agregar tests
- `chore`: Tareas de mantenimiento

**Ejemplo**:
```
feat(facturacion): agregar validación de NIT

Implementa validación de formato de NIT según normativa DIAN
antes de generar factura electrónica.

Closes #123
```

### 5.3 Estructura de Branches

```
main (production)
├── staging
│   └── develop
│       ├── feature/[ID]-[nombre]
│       ├── fix/[ID]-[nombre]
│       └── hotfix/[ID]-[nombre]
```

## 6. Gestión de Calidad

### 6.1 Definición de Done

Un componente está "Done" cuando:
- [ ] Código implementado y funcionando
- [ ] Tests implementados y pasando
- [ ] Documentación completa
- [ ] Code review aprobado
- [ ] Desplegado en staging
- [ ] Validado por stakeholders
- [ ] Desplegado en production

### 6.2 Métricas de Calidad

**Métricas de Código**:
- Cobertura de tests: ≥ 80%
- Complejidad ciclomática: ≤ 10
- Duplicación de código: ≤ 5%

**Métricas de Proceso**:
- Tiempo de ciclo: ≤ 10 días
- Lead time: ≤ 15 días
- Tasa de defectos: ≤ 5%

**Métricas de Performance**:
- Tiempo de ejecución: ≤ 30s
- Tasa de error: ≤ 1%
- Disponibilidad: ≥ 99%

## 7. Gestión de Riesgos

### 7.1 Identificación de Riesgos

| ID | Riesgo | Probabilidad | Impacto | Mitigación |
|----|--------|--------------|---------|------------|
| R-001 | Falla de API externa | Media | Alto | Implementar reintentos y fallback |
| R-002 | Pérdida de datos | Baja | Crítico | Backups automáticos |

### 7.2 Plan de Contingencia

Ver `gestion/procesos/plan_contingencia.md`

## 8. Comunicación

### 8.1 Canales

- **Slack/Teams**: Comunicación diaria
- **Email**: Comunicación formal
- **GitHub Issues**: Tracking de tareas
- **Confluence/Wiki**: Documentación colaborativa

### 8.2 Reuniones

| Reunión | Frecuencia | Duración | Participantes |
|---------|-----------|----------|---------------|
| Daily Standup | Diaria | 15 min | Todo el equipo |
| Sprint Planning | Quincenal | 2 horas | Todo el equipo |
| Sprint Review | Quincenal | 1 hora | Equipo + Stakeholders |
| Retrospective | Quincenal | 1 hora | Todo el equipo |

## 9. Capacitación

### 9.1 Onboarding

Nuevos miembros del equipo:
1. Revisar documentación del proyecto
2. Setup de ambiente de desarrollo
3. Pair programming con senior
4. Asignación de tarea simple

### 9.2 Recursos de Aprendizaje

- [Documentación oficial de n8n](https://docs.n8n.io/)
- Tutoriales internos: `docs/manuales/tutoriales/`
- Videos de capacitación: [link]

## 10. Mejora Continua

### 10.1 Retrospectives

Después de cada sprint:
- ¿Qué funcionó bien?
- ¿Qué no funcionó?
- ¿Qué podemos mejorar?

### 10.2 Acciones de Mejora

Documentar en `gestion/seguimiento/mejoras.md`

## Historial de Revisiones

| Versión | Fecha | Autor | Cambios |
|---------|-------|-------|---------|
| 1.0 | DD/MM/YYYY | [Nombre] | Versión inicial |
