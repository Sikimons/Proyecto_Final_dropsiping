# Historias refinadas — Dropshipping
<!-- Definition of Ready verificada por dor-invest-gate -->

Épicas en scope MVP: **E-01** (Ciclo Dropshipping portal proveedor) · **E-02** (Visibilidad analista) · **E-03** (Notificaciones al cliente)
Total: **12 historias · 40 story points**

---

## E-01 · Ciclo Dropshipping en portal del proveedor

### US-01 · Ver y gestionar órdenes asignadas   ·   épica E-01   ·   5 pts
**Como** Proveedor, **quiero** ver mis órdenes asignadas con información completa (código de producto, descripción, cantidad, dirección completa, contacto del cliente, fecha esperada y condiciones especiales) y aceptarlas o rechazarlas indicando fecha estimada de despacho o motivo de rechazo, **para** operar desde un solo canal sin depender del correo para recibir, confirmar o rechazar órdenes.

Criterios de aceptación:
- Dado que el sistema genera una orden Dropshipping, cuando el proveedor accede al portal, entonces ve la orden con: código de producto, descripción, cantidad, dirección completa, contacto del cliente, fecha esperada y condiciones especiales.
- Dado que el proveedor acepta la orden, cuando confirma con fecha estimada de despacho, entonces el pedido cambia a "Aceptado", registra actor y timestamp, y el analista ve la actualización.
- Dado que el proveedor rechaza la orden, cuando indica el motivo, entonces el pedido cambia a "Rechazado" y el equipo comercial recibe alerta inmediata para ofrecer alternativa al cliente.

Origen: `user-stories.md → US-01` · `requisitos.md → R-01, R-02, R-03, R-32`

---

### US-02 · Registrar despacho con tracking   ·   épica E-01   ·   3 pts
**Como** Proveedor, **quiero** registrar el despacho del pedido con guía de transportadora o con información alternativa (placa, nombre del conductor y fecha de salida) cuando no existe guía formal, **para** que el tracking llegue al cliente sin intervención manual del analista.

Criterios de aceptación:
- Dado que el proveedor despacha el pedido con guía, cuando la registra en el portal, entonces el pedido cambia a "En tránsito" y el analista ve el update.
- Dado que no existe guía de transportadora, cuando el proveedor registra información de transporte propio (placa, conductor, fecha de salida), entonces el sistema acepta el despacho y actualiza el estado igualmente.
- Dado que el proveedor intenta marcar despacho sin información de transporte alguna, cuando intenta guardar, entonces el sistema bloquea la acción y señala el campo requerido.

Dependencias: US-01
Origen: `user-stories.md → US-02` · `requisitos.md → R-06`

---

### US-03 · Confirmar entrega con evidencia obligatoria   ·   épica E-01   ·   3 pts
**Como** Proveedor, **quiero** adjuntar evidencia obligatoria (firma, foto o nombre del receptor) para confirmar la entrega al cliente y cerrar el pedido, **para** cerrar el ciclo de forma verificable y reducir reclamos por entregas no reconocidas.

Criterios de aceptación:
- Dado que el proveedor intenta marcar el pedido como "Entregado", cuando no adjunta evidencia, entonces el sistema bloquea la acción y muestra el requerimiento de evidencia.
- Dado que el proveedor adjunta evidencia válida (firma, foto o nombre del receptor), cuando guarda, entonces el pedido cambia a "Entregado" y el archivo queda vinculado al historial.
- Dado que la entrega fue exitosa, cuando el sistema la registra, entonces la fecha y hora quedan en el historial inmutable del pedido.

Dependencias: US-02
Origen: `user-stories.md → US-03` · `requisitos.md → R-07`

---

### US-04 · Registrar novedad de entrega desde el campo   ·   épica E-01   ·   3 pts
**Como** Proveedor, **quiero** registrar una novedad de entrega desde el campo (cliente no disponible, dirección incorrecta, producto dañado) con tipo, observación y foto adjunta opcional, **para** que todos los actores vean el mismo estado en tiempo real sin esperar que el conductor reporte internamente.

Criterios de aceptación:
- Dado que se produce una novedad durante la entrega, cuando el proveedor la registra con tipo y observación, entonces el pedido cambia a "Novedad", se crea una incidencia con responsable asignado y el analista recibe alerta.
- Dado que el proveedor adjunta foto en la novedad, cuando guarda, entonces la imagen queda vinculada a la incidencia como evidencia de estado en campo.
- Dado que existe una novedad, cuando el sistema la registra, entonces el pedido NO se marca como "Entregado" ni queda en estado genérico de "Pendiente".

Dependencias: US-01
Origen: `user-stories.md → US-04` · `requisitos.md → R-08, R-34`

---

### US-05 · Reservar inventario al confirmar pedido   ·   épica E-01   ·   3 pts
**Como** Sistema, **quiero** reservar (bloquear) la unidad de inventario del proveedor al confirmar el pedido Dropshipping y liberar la reserva únicamente si el pedido es rechazado o cancelado, **para** impedir la doble venta de la misma unidad mientras el ciclo de entrega está activo.

Criterios de aceptación:
- Dado que se confirma un pedido Dropshipping, cuando el proveedor acepta, entonces la unidad queda marcada como "comprometida" y no puede ser asignada a otro pedido.
- Dado que el proveedor rechaza el pedido, cuando el sistema registra el rechazo, entonces la reserva se libera automáticamente.
- Dado que el pedido está activo, cuando el sistema muestra la disponibilidad de inventario, entonces la unidad comprometida no aparece como disponible para nuevas ventas.

Dependencias: US-01
Origen: `mvp-canvas.md → Notas de implementación: reserva de inventario` · `requisitos.md → R-33`

---

## E-02 · Visibilidad operativa del analista

### US-06 · Vista global de pedidos Dropshipping activos   ·   épica E-02   ·   5 pts
**Como** Analista de Compras y Logística, **quiero** ver el estado real de todos los pedidos Dropshipping activos en una sola vista con estado actual, proveedor asignado, fecha prometida y cantidad de días transcurridos desde el último update del proveedor, **para** detectar retrasos antes de que el cliente reclame sin tener que consultar correos ni llamar al proveedor.

Criterios de aceptación:
- Dado que existen pedidos Dropshipping en curso, cuando el analista abre el módulo, entonces ve todos los pedidos con estado actual, proveedor asignado, fecha prometida y días transcurridos desde el último update.
- Dado que un proveedor cambia la fecha prometida, cuando lo registra en el portal, entonces el sistema muestra "fecha modificada" en la vista del analista con la fecha anterior registrada en el historial.

Dependencias: US-01, US-02, US-03, US-04
Origen: `user-stories.md → US-05` · `requisitos.md → R-05, R-21`

---

### US-07 · Alerta automática por inactividad del proveedor   ·   épica E-02   ·   3 pts
**Como** Analista de Compras y Logística, **quiero** recibir una alerta automática dentro del sistema cuando un pedido Dropshipping lleva más tiempo del umbral configurado sin ninguna actualización del proveedor, **para** intervenir proactivamente antes de que el cliente reclame sin tener que revisar manualmente cada pedido.

Criterios de aceptación:
- Dado que un pedido no ha recibido update del proveedor en el plazo configurado, cuando se cumple el umbral, entonces el sistema genera una alerta visible en la vista del analista con el pedido afectado.
- Dado que el analista atiende el pedido en alerta, cuando el proveedor registra un update posterior, entonces la alerta se desactiva automáticamente.

Dependencias: US-06
Origen: `user-stories.md → US-05` · `requisitos.md → R-21`

---

### US-08 · Historial inmutable de cambios de estado   ·   épica E-02   ·   3 pts
**Como** Analista de Compras y Logística, **quiero** consultar el historial completo de cambios de estado de cada pedido Dropshipping con el actor responsable y el timestamp exacto de cada cambio, incluyendo modificaciones de fecha, **para** tener trazabilidad ante reclamos o discrepancias sin depender de correos o llamadas.

Criterios de aceptación:
- Dado que consulto el detalle de un pedido, cuando accedo al historial, entonces veo una línea de tiempo cronológica con cada cambio de estado, el actor responsable y el timestamp exacto.
- Dado que el proveedor modifica una fecha confirmada, cuando guarda el cambio, entonces el historial registra la fecha anterior y la nueva con el motivo indicado, sin sobrescribir el registro anterior.
- Dado que un cambio fue realizado fuera del portal, cuando el analista lo registra manualmente, entonces queda en el historial marcado como "registro manual" con el nombre del analista y timestamp.

Dependencias: US-01
Origen: `user-stories.md → US-06` · `requisitos.md → R-05`

---

### US-09 · Registro manual de updates de proveedores por correo   ·   épica E-02   ·   2 pts
**Como** Analista de Compras y Logística, **quiero** registrar manualmente una actualización de estado de un proveedor que opera por correo, indicando el nuevo estado y el motivo, con la entrada marcada como "registro manual", **para** mantener la trazabilidad completa del pedido aunque el proveedor no use el portal.

Criterios de aceptación:
- Dado que un proveedor no usa el portal y envía update por correo, cuando el analista registra la actualización manualmente, entonces el cambio de estado queda en el historial marcado como "registro manual" con nombre del analista y timestamp.
- Dado que el analista guarda un registro manual, cuando lo confirma, entonces el pedido cambia al estado indicado con los mismos efectos que si el proveedor lo hubiera registrado directamente en el portal.

Dependencias: US-08
Origen: `mvp-canvas.md → Notas: proceso alternativo por correo (R-24)` · `requisitos.md → R-24`

---

## E-03 · Notificaciones automáticas al cliente (Dropshipping)

### US-10 · Notificaciones automáticas en hitos clave   ·   épica E-03   ·   5 pts
**Como** Especialista de eCommerce y Servicio al Cliente, **quiero** que el sistema envíe notificaciones automáticas al cliente en los tres hitos Dropshipping (orden aceptada, pedido despachado, entrega confirmada) indicando el nombre del producto y el detalle específico del cambio, **para** reducir los contactos entrantes de "¿dónde está mi pedido?" sin intervención manual del equipo.

Criterios de aceptación:
- Dado que el proveedor acepta el pedido, cuando registra la aceptación, entonces el cliente recibe: "Tu producto [nombre] fue aceptado por el proveedor. Fecha estimada de entrega: [fecha]."
- Dado que el proveedor registra el despacho con tracking, cuando lo guarda, entonces el cliente recibe: "Tu producto [nombre] fue despachado. Número de seguimiento: [guía]."
- Dado que el pedido es marcado como entregado con evidencia, cuando el sistema lo registra, entonces el cliente recibe la confirmación de entrega con nombre del producto. El sistema no envía notificaciones por movimientos internos intermedios.

Dependencias: US-01, US-02, US-03
Origen: `user-stories.md → US-07` · `requisitos.md → R-13` · `mvp-canvas.md → Funcionalidades mínimas: notificaciones automáticas`

---

### US-11 · Notificación proactiva de cambio de fecha   ·   épica E-03   ·   3 pts
**Como** Especialista de eCommerce y Servicio al Cliente, **quiero** que el sistema genere automáticamente una notificación al cliente cuando el proveedor modifica la fecha de entrega prometida, indicando la fecha anterior y la nueva fecha, **para** que el cliente no se entere del cambio al preguntar sino antes de que intente recibir el producto en la fecha original.

Criterios de aceptación:
- Dado que el proveedor modifica la fecha prometida de un pedido, cuando guarda el cambio, entonces el sistema genera automáticamente: "La fecha estimada de entrega de [producto] cambió de [fecha anterior] a [nueva fecha]."
- Dado que la notificación de cambio de fecha se genera, cuando el cliente la recibe, entonces incluye un canal de contacto central para consultas y no redirige al proveedor directamente.

Dependencias: US-10
Origen: `user-stories.md → US-09` · `requisitos.md → R-14`

---

### US-12 · Preaviso antes de contacto del proveedor   ·   épica E-03   ·   2 pts
**Como** Cliente, **quiero** recibir una notificación del sistema indicando que un proveedor autorizado de la empresa se comunicará conmigo para coordinar la entrega, antes de que el proveedor me contacte, **para** identificar la llamada como legítima y no rechazarla pensando que es una comunicación no deseada.

Criterios de aceptación:
- Dado que el proveedor necesita coordinar la entrega con el cliente, cuando el sistema detecta que el estado avanza a coordinación, entonces envía al cliente: "Un proveedor autorizado de [empresa] se comunicará contigo para coordinar la entrega de [producto]. Motivo: [descripción breve]."
- Dado que el analista necesita generar el preaviso manualmente, cuando lo activa desde la vista del pedido, entonces el cliente recibe la misma notificación de preaviso y esta queda registrada en el historial del pedido.

Dependencias: US-10
Origen: `user-stories.md → US-08` · `requisitos.md → R-29`

---

## Resumen de capacidad

| Épica | Historias | Story points |
|-------|-----------|-------------|
| E-01 · Portal proveedor | US-01 US-02 US-03 US-04 US-05 | 17 pts |
| E-02 · Visibilidad analista | US-06 US-07 US-08 US-09 | 13 pts |
| E-03 · Notificaciones cliente | US-10 US-11 US-12 | 10 pts |
| **Total MVP** | **12 historias** | **40 pts** |

> E-04 (Checkout diferenciado) y E-05 (Flujo Pickup) están fuera del alcance de este sprint por decisión del MVP canvas. Sus historias candidatas (HC-13…HC-20) permanecen en el backlog para el siguiente ciclo.
