# ADR-0006 · Máquina de estados explícita para pedidos Dropshipping
**Estado:** aceptado
**Fecha:** 2026-07-04

## Contexto y fuerza

`user-stories.md → US-04` (criterio de aceptación): "Dado que existe una novedad, cuando el sistema la registra, entonces el pedido **NO** se marca como 'Entregado' ni queda en **estado genérico de 'Pendiente'**."

`requisitos.md → R-08`: "Las novedades de entrega deben gestionarse como incidencias con responsable asignado; **no deben resolverse con un estado genérico**."

`requisitos.md → R-05`: cada cambio de estado registra actor y timestamp. Esto implica que "estado" es un concepto de primera clase en el sistema, no un campo libre de texto.

Si el estado del pedido se modela como una columna `status VARCHAR` con valores arbitrarios, nada impide que el código de diferentes partes del sistema escriba valores distintos para el mismo concepto, o que se realicen transiciones inválidas (p.ej. pasar de `Entregado` a `Pendiente`).

## Decisión

Los estados del pedido Dropshipping se definen en una **máquina de estados finita explícita** en el código. Las transiciones válidas están enumeradas. Cualquier intento de transición no autorizada es rechazado por la capa de aplicación antes de llegar a la base de datos.

Estados:
```
Pendiente → Aceptado (proveedor acepta + fecha)
Pendiente → Rechazado (proveedor rechaza + motivo)
Aceptado → EnTransito (proveedor registra despacho)
EnTransito → Entregado (proveedor adjunta evidencia)
EnTransito → Novedad (proveedor registra incidencia de campo)
Novedad → EnTransito (incidencia resuelta, reintento de entrega)
Novedad → Entregado (entrega exitosa tras novedad)
```

Estados terminales: `Rechazado`, `Entregado`.

La máquina de estados también dispara efectos secundarios definidos por transición: escribir en audit log (ADR-0002), escribir en outbox de notificaciones (ADR-0005).

## Alternativas consideradas

- **Columna `status` mutable con validación en cada endpoint** — la lógica de transición queda dispersa en múltiples controladores. Fácil de omitir en nuevos endpoints o en cambios de estado manuales (analista). Descartado.
- **Flags booleanos por estado** (`is_accepted`, `is_dispatched`, etc.) — combinatoria de estados inválidos (p.ej. `is_accepted=true` y `is_dispatched=true` y `is_pending=true` simultáneamente). Imposible de auditar. Descartado.

## Consecuencias

**Ganamos:** las transiciones inválidas son imposibles por diseño. El estado `Novedad` es un ciudadano de primera clase, no un "pendiente genérico". Los efectos secundarios (notificaciones, audit log) están centralizados en un solo lugar.

**Costo aceptado:** agregar un nuevo estado o transición requiere modificar la definición de la máquina de estados y sus tests. Esto es intencional: los cambios de estado son decisiones de negocio que deben pasar por revisión, no adiciones ad-hoc. El MVP canvas marca como fuera de alcance los pedidos multi-proveedor (R-09); si se incorpora en v2, la máquina de estados necesitará extenderse para gestionar estados por línea de pedido.
