# Agile Delivery Team

Equipo ágil de entrega impulsado por IA que transforma los insumos de discovery en un plan de ejecución listo para construir: épicas priorizadas, backlog refinado, arquitectura con ADRs y un sprint plan validado por criterios de calidad.

## Resumen

`Agile Delivery Team` convierte artefactos de discovery en entregables concretos de delivery mediante un flujo dividido en cuatro roles: Product Owner, Developer, Architect y Scrum Master. Este enfoque permite priorizar mejor el valor, refinar historias listas para desarrollo, sostener decisiones técnicas con ADRs y construir un sprint plan realista según la capacidad del equipo.

En este repositorio, el caso trabajado es `dropshipping`, con evidencias complementarias en el video `agile-delivery-team.mp4` y en la reflexión documentada en `reflexion.md`.

## Propósito

Este repositorio muestra cómo un flujo de trabajo dividido en cuatro roles puede convertir artefactos de discovery en entregables de delivery más claros, trazables y ejecutables.

El caso desarrollado en este repositorio es `dropshipping`, y su resultado puede revisarse tanto en los artefactos generados como en las evidencias complementarias incluidas en la raíz del proyecto.

## Evidencias

- Video de demostración: [agile-delivery-team.mp4](./agile-delivery-team.mp4)
- Reflexión del ejercicio: [reflexion.md](./reflexion.md)

La reflexión resume qué cambió al separar el trabajo por roles, cuál fue la historia más difícil de dejar en estado Ready y por qué un gate de Definition of Ready aporta valor en un equipo real.

## Qué produce este equipo

A partir de los archivos ubicados en `deliveries/<nombre>/inbox/`, el flujo genera:

1. `epics.md`: épicas priorizadas según valor de negocio.
2. `backlog.json`: backlog estructurado y enriquecido.
3. `stories.md`: historias refinadas con criterios INVEST y Definition of Ready.
4. `architecture.md`: propuesta de arquitectura mínima viable.
5. `outputs/adr/*.md`: decisiones técnicas documentadas como ADRs.
6. `sprint-plan.md` y `sprint-plan.json`: selección de historias listas según capacidad.
7. `report.html`: reporte visual consolidado.

## Roles del flujo

| Rol | Enfoque principal | Artefactos |
|---|---|---|
| Product Owner | Priorizar valor y alcance | `epics.md`, `backlog.json` |
| Developer | Refinar historias listas para construir | `stories.md`, `backlog.json` |
| Architect | Definir la estructura técnica y decisiones clave | `architecture.md`, `adr/` |
| Scrum Master | Ajustar el plan a la capacidad real del equipo | `sprint-plan.md`, `sprint-plan.json` |

## Flujo de trabajo

```bash
# 1. Copiar los insumos del Discovery Agent
cp discovery-outputs/* deliveries/<nombre>/inbox/

# 2. Generar épicas y backlog
/delivery:generate-epics <nombre>

# 3. Refinar historias y validar DoR/INVEST
/delivery:generate-stories <nombre>

# 4. Definir arquitectura y ADRs
/delivery:architecture <nombre>

# 5. Construir el sprint plan
/delivery:sprint-plan <nombre>

# 6. Generar el reporte visual
/delivery:report <nombre>
```

## Estructura del repositorio

```text
deliveries/<nombre>/
├── inbox/
│   ├── mvp-canvas.md
│   ├── user-stories.md
│   ├── requisitos.md
│   ├── personas.md
│   └── evidence-map.json
└── outputs/
    ├── epics.md
    ├── backlog.json
    ├── stories.md
    ├── architecture.md
    ├── adr/
    │   └── ADR-NNNN-<slug>.md
    ├── sprint-plan.md
    ├── sprint-plan.json
    └── report.html
```

## Reglas del proceso

- Trazabilidad completa: cada entrega debe apoyarse en evidencia del `inbox/`.
- Cero invención: lo no respaldado se registra como supuesto o pregunta abierta.
- INVEST + DoR: las historias deben estar listas antes de entrar al sprint.
- Prioridad por valor: primero outcome e impacto, después conveniencia técnica.
- Aislamiento por delivery: cada caso se mantiene separado en su propia carpeta.

## Caso disponible

| Delivery | Estado | Resultado |
|---|---|---|
| [`dropshipping`](./deliveries/dropshipping/) | Completo | 5 épicas, 17 historias y Sprint 1 definido |

## Requisitos

- Python 3.x para validaciones y generación de reportes.
- Claude Code con acceso a los agentes configurados para este flujo.
