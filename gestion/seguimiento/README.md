# Seguimiento del Proyecto

Este directorio contiene documentos relacionados con el seguimiento y control del proyecto.

## Contenido

- **sprints/**: Información de cada sprint
- **metricas/**: Métricas del proyecto
- **reuniones/**: Actas de reuniones
- **mejoras/**: Acciones de mejora continua

## Archivos

### sprints/

Documentación de cada sprint:
- Objetivos del sprint
- Tareas completadas
- Burndown chart
- Retrospectiva

**Formato**: `sprint-[numero]-[fechas].md`

### metricas/

Métricas del proyecto:
- Velocidad del equipo
- Tiempo de ciclo
- Lead time
- Tasa de defectos
- Coverage de tests

### reuniones/

Actas de reuniones:
- Daily standups (resumenes semanales)
- Sprint planning
- Sprint review
- Sprint retrospective
- Reuniones con stakeholders

**Formato**: `[tipo]-[fecha].md`

### mejoras/

Acciones de mejora continua:
- Identificadas en retrospectivas
- Estado de implementación
- Resultados

## Templates

### Sprint Planning

```markdown
# Sprint [Número] Planning

**Fecha**: DD/MM/YYYY
**Participantes**: Lista de participantes

## Objetivo del Sprint
Descripción del objetivo principal

## Backlog Items Seleccionados

| ID | Título | Story Points | Asignado a |
|----|--------|--------------|------------|
| #123 | Feature X | 5 | @usuario |

## Definición de Done
- [ ] Código implementado
- [ ] Tests pasando
- [ ] Code review aprobado
- [ ] Documentación actualizada
- [ ] Desplegado en staging

## Notas
Notas adicionales
```

### Retrospectiva

```markdown
# Sprint [Número] Retrospective

**Fecha**: DD/MM/YYYY
**Participantes**: Lista de participantes

## ¿Qué funcionó bien? 🌟
- Item 1
- Item 2

## ¿Qué no funcionó? 😞
- Item 1
- Item 2

## ¿Qué podemos mejorar? 💡
- Mejora 1
- Mejora 2

## Acciones
| Acción | Responsable | Fecha Límite |
|--------|-------------|--------------|
| Acción 1 | @usuario | DD/MM/YYYY |
```

## Uso

### Crear Nuevo Sprint

```bash
# Copiar template
cp gestion/seguimiento/templates/sprint-planning.md \
   gestion/seguimiento/sprints/sprint-[numero]-planning.md

# Editar con información del sprint
```

### Registrar Métricas

Actualizar archivo de métricas semanalmente:
```bash
# Editar archivo de métricas
nano gestion/seguimiento/metricas/metricas-[año]-[mes].md
```

## Reportes

Generar reportes periódicos:
- **Semanal**: Estado de tareas, blockers
- **Por Sprint**: Velocidad, burndown
- **Mensual**: Tendencias, métricas acumuladas
- **Trimestral**: Review de objetivos

## Herramientas

- **GitHub Projects**: Tracking de issues y PRs
- **Documentos**: Información detallada y retrospectivas
- **Métricas**: Dashboards y reportes

## Contacto

Para consultas sobre seguimiento del proyecto, contactar al Scrum Master o Project Manager.
