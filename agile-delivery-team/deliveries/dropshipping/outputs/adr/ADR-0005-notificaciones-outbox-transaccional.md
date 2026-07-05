# ADR-0005 · Outbox transaccional para notificaciones al cliente
**Estado:** aceptado
**Fecha:** 2026-07-04

## Contexto y fuerza

`mvp-canvas.md → Funcionalidades mínimas`: "Notificaciones automáticas al cliente: proveedor aceptó (con fecha estimada), pedido despachado (con tracking si existe), entregado, cambio de fecha."

`user-stories.md → US-10` (criterio de aceptación): cuando el proveedor acepta, el cliente "recibe automáticamente" la notificación. La palabra "automáticamente" implica que el envío debe ocurrir como consecuencia directa del cambio de estado, sin intervención manual y sin pérdida.

Fuerza técnica: si el proceso que cambia el estado y el proceso que envía la notificación son independientes, existe una ventana de fallo. Ejemplo: la transacción de base de datos se confirma pero el servidor de email no responde → la notificación se pierde. Sin un mecanismo de garantía, el cliente no recibe el aviso y llama al equipo, que es exactamente lo que el MVP busca evitar.

`mvp-canvas.md → Métrica de éxito`: ≥70 % de pedidos sin consulta activa del analista al proveedor. Si las notificaciones se pierden, el cliente llama, el analista interviene, y la métrica no se cumple.

## Decisión

Se adopta el patrón **Transactional Outbox**: al escribir el cambio de estado de un pedido en la base de datos, en la **misma transacción** se escribe una fila en la tabla `notification_outbox` con el tipo de notificación, el destinatario y el payload.

Un **worker asíncrono** (proceso separado o job programado) lee las filas de `notification_outbox` con estado `PENDIENTE`, las envía al proveedor de email, y marca cada fila como `ENVIADO` o `FALLIDO`. Los fallos se reintentarán con backoff exponencial hasta un máximo configurable.

```
notification_outbox(id, order_id, type, recipient, payload_json, status, created_at, sent_at, retries)
```

## Alternativas consideradas

- **Envío síncrono directo en la misma request** — si el proveedor de email falla o está lento, la operación del proveedor en el portal falla o se demora. Inaceptable: la experiencia del proveedor no puede depender de un servicio externo. Descartado.
- **Cola de mensajes (RabbitMQ, SQS)** — garantías similares al outbox pero requiere infraestructura adicional que administrar. Para el volumen del MVP es sobredimensionado. Si en el futuro se necesita escalar el worker, migrar de outbox a cola es directo. Descartado para MVP.
- **Polling sin outbox** — el worker escanea la tabla de pedidos buscando cambios recientes. Introduce acoplamiento temporal (¿cada cuánto?) y puede enviar duplicados o perder notificaciones en ventanas de poll. Descartado.

## Consecuencias

**Ganamos:** garantía de entrega de notificaciones (at-least-once). Si el email falla, el sistema reintenta sin intervención. El cambio de estado y la promesa de notificación son atómicos desde la perspectiva del negocio.

**Costo aceptado:** latencia de entrega de la notificación depende de la frecuencia del worker (segundos a minutos). Para el caso de uso (el cliente recibe aviso de aceptación/despacho/entrega) una latencia de minutos es aceptable. Si se requiere notificación en tiempo real (< 1s), habrá que revisar este ADR.

**Supuesto pendiente:** el canal de notificación (email vs. SMS vs. push) no está definido en el inbox. Este ADR asume email como mínimo viable. Si se agrega SMS, el worker necesita un provider adicional pero la arquitectura no cambia.
