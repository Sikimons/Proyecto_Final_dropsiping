# ADR-0002 · Audit log append-only para historial inmutable
**Estado:** aceptado
**Fecha:** 2026-07-04

## Contexto y fuerza

`requisitos.md → R-05`: "Cada cambio de estado del pedido debe registrar quién lo realizó y cuándo; los cambios deben **agregarse al historial sin sobrescribir** el estado anterior."

`user-stories.md → US-08` (criterio de aceptación): "Dado que el proveedor modifica una fecha confirmada, cuando guarda el cambio, entonces el historial registra la fecha anterior y la nueva con el motivo indicado, **sin sobrescribir**."

`user-stories.md → US-09`: los registros manuales del analista deben convivir en el mismo historial que los del proveedor, marcados como "registro manual". Si el historial fuera una columna mutable en la tabla de pedidos, estas dos fuentes de verdad serían imposibles de reconciliar sin pérdida de información.

## Decisión

El historial de cambios de estado de cada pedido se implementa como una **tabla de eventos append-only** (`order_events`). Cada fila representa un evento ocurrido: `{order_id, event_type, actor, timestamp, payload_json}`. Los estados actuales son **proyecciones** derivadas del último evento relevante.

La tabla `order_events` solo permite `INSERT`. No se permiten `UPDATE` ni `DELETE` sobre ella (restricción a nivel de base de datos y de capa de aplicación).

## Alternativas consideradas

- **Columna `status` mutable en la tabla de pedidos** — simple, pero destruye el historial en cada actualización. No cumple R-05 ni US-08. Descartado.
- **Event Sourcing completo** (toda la entidad como proyección de eventos) — excesivo para el MVP. Introduce complejidad de snapshots, projectors y eventual consistency que el equipo no necesita gestionar en esta fase. Descartado.

## Consecuencias

**Ganamos:** cumplimiento directo de R-05 y US-08 sin lógica adicional. El historial es auditable y forense. Los cambios manuales del analista (US-09) y los automáticos del proveedor se almacenan con el mismo mecanismo.

**Costo aceptado:** la tabla `order_events` crecerá con cada cambio de estado. Para el volumen de un MVP esto es irrelevante. Si el volumen cresce significativamente, se puede archivar eventos antiguos sin afectar la lógica de negocio.
