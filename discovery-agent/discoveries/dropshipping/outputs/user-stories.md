# User Stories — Dropshipping

## Prioridad 1 — Núcleo del MVP: portal del proveedor y visibilidad interna

- **[US-01]** Como Proveedor, quiero ver mis órdenes asignadas y poder aceptarlas o rechazarlas con una fecha estimada de despacho, para operar desde un solo canal sin depender del correo.
  - Criterios de aceptación:
    - Dado que el sistema genera una orden Dropshipping, cuando el proveedor accede al portal, entonces ve la orden con: código de producto, descripción, cantidad, dirección completa, contacto del cliente, fecha esperada y condiciones especiales.
    - Dado que el proveedor acepta la orden, cuando confirma con fecha estimada, entonces el pedido cambia a "Aceptado", registra actor y timestamp, y el analista recibe la actualización.
    - Dado que el proveedor rechaza la orden, cuando indica el motivo, entonces el pedido cambia a "Rechazado" y el equipo comercial recibe alerta inmediata para ofrecer alternativa al cliente.
  - Fuente: _entrevista_proveedor.md, analista_de_compras_y_logistica.md_ · R-01, R-02, R-03, R-32

- **[US-02]** Como Proveedor, quiero registrar el despacho del pedido con guía de transporte o información alternativa (placa, nombre de conductor, fecha de salida), para que el tracking llegue al cliente sin intervención manual del analista.
  - Criterios de aceptación:
    - Dado que el proveedor despacha el pedido, cuando registra la guía en el portal, entonces el pedido cambia a "En tránsito", el analista ve el update y el cliente recibe la notificación con el tracking.
    - Dado que no existe guía de transportadora, cuando el proveedor registra información de transporte propio (placa / conductor / fecha de salida), entonces el sistema acepta el despacho y actualiza el estado igualmente.
    - Dado que el proveedor intenta marcar despacho sin información de transporte alguna, cuando intenta guardar, entonces el sistema bloquea la acción y señala el campo requerido.
  - Fuente: _entrevista_proveedor.md, analista_de_compras_y_logistica.md_ · R-06

- **[US-03]** Como Proveedor, quiero confirmar la entrega al cliente con evidencia adjunta (firma, foto o nombre del receptor), para cerrar el pedido de forma verificable y reducir reclamos por entregas no reconocidas.
  - Criterios de aceptación:
    - Dado que el proveedor intenta marcar el pedido como "Entregado", cuando no adjunta evidencia, entonces el sistema bloquea la acción y muestra el requerimiento.
    - Dado que el proveedor adjunta evidencia válida, cuando guarda, entonces el pedido cambia a "Entregado", el archivo queda vinculado al historial y el cliente recibe la confirmación.
    - Dado que la entrega fue exitosa, cuando el sistema la registra, entonces la fecha y hora quedan en el historial inmutable del pedido.
  - Fuente: _entrevista_proveedor.md, analista_de_compras_y_logistica.md, cliente.md_ · R-07

- **[US-04]** Como Proveedor, quiero registrar una novedad de entrega desde el campo (cliente no disponible, dirección incorrecta, producto dañado) con foto u observación adjunta, para que todos los actores vean el mismo estado en tiempo real sin esperar que el conductor reporte internamente.
  - Criterios de aceptación:
    - Dado que se produce una novedad durante la entrega, cuando el proveedor la registra con tipo y observación, entonces el pedido cambia a "Novedad", se crea una incidencia con responsable asignado y el analista recibe alerta.
    - Dado que el proveedor adjunta foto en la novedad, cuando guarda, entonces la imagen queda vinculada a la incidencia como evidencia de estado en campo.
    - Dado que existe una novedad, cuando el sistema la registra, entonces el pedido NO se marca como "Entregado" ni queda en estado genérico de "Pendiente".
  - Fuente: _entrevista_proveedor.md, analista_de_compras_y_logistica.md_ · R-08, R-34

- **[US-05]** Como Analista de Compras y Logística, quiero ver el estado real de todos los pedidos Dropshipping activos en una sola vista, sin tener que consultar correos ni llamar al proveedor, para detectar retrasos antes de que el cliente reclame.
  - Criterios de aceptación:
    - Dado que existen pedidos Dropshipping en curso, cuando el analista abre el módulo, entonces ve todos los pedidos con su estado actual, proveedor asignado, fecha prometida y días transcurridos desde el último update.
    - Dado que un pedido lleva más tiempo del configurado sin actualización del proveedor, cuando se cumple el plazo, entonces el sistema genera una alerta visible en la vista del analista.
    - Dado que un proveedor cambia la fecha prometida, cuando lo registra en el portal, entonces el sistema conserva la fecha anterior en el historial y muestra "fecha modificada" en la vista del analista.
  - Fuente: _analista_de_compras_y_logistica.md_ · R-05, R-21

- **[US-06]** Como Analista de Compras y Logística, quiero ver el historial completo de cambios de estado de cada pedido, con quién hizo cada cambio y cuándo, para tener trazabilidad ante reclamos o discrepancias sin depender de correos o llamadas.
  - Criterios de aceptación:
    - Dado que consulto el detalle de un pedido Dropshipping, cuando accedo al historial, entonces veo una línea de tiempo cronológica con cada cambio de estado, el actor responsable y el timestamp exacto.
    - Dado que el proveedor modifica una fecha confirmada, cuando guarda el cambio, entonces el historial registra la fecha anterior y la nueva con el motivo indicado, sin sobrescribir.
    - Dado que un cambio fue realizado fuera del portal (proceso alternativo por correo), cuando el analista lo registra manualmente, entonces queda en el historial marcado como "registro manual" con el nombre del analista.
  - Fuente: _analista_de_compras_y_logistica.md, entrevista_proveedor.md_ · R-05, R-24

---

## Prioridad 2 — Notificaciones al cliente (Dropshipping)

- **[US-07]** Como Especialista de eCommerce y Servicio al Cliente, quiero que el cliente reciba notificaciones automáticas en los hitos clave del pedido Dropshipping (aceptado, despachado, entregado), con el nombre del producto afectado y el detalle del cambio, para reducir los contactos de "¿dónde está mi pedido?".
  - Criterios de aceptación:
    - Dado que el proveedor acepta el pedido, cuando registra la aceptación, entonces el cliente recibe automáticamente: "Tu producto [nombre] fue aceptado por el proveedor. Fecha estimada de entrega: [fecha]."
    - Dado que el proveedor marca el despacho con tracking, cuando lo registra, entonces el cliente recibe: "Tu producto [nombre] fue despachado. Número de seguimiento: [guía]."
    - Dado que el pedido es marcado como entregado, cuando se registra, entonces el cliente recibe la confirmación de entrega. El sistema no envía notificaciones por movimientos internos intermedios.
  - Fuente: _especialista_de_ecommerce_y_servicio_al_cliente.md, cliente.md_ · R-13

- **[US-08]** Como Cliente, quiero recibir un aviso antes de que el proveedor me contacte para coordinar la entrega, para saber que la llamada es legítima y no una comunicación no deseada.
  - Criterios de aceptación:
    - Dado que el proveedor necesita coordinar la entrega con el cliente, cuando el sistema detecta que el estado avanza a coordinación, entonces envía al cliente: "Un proveedor autorizado de [empresa] se comunicará contigo para coordinar la entrega de [producto]. Motivo: [descripción breve]."
    - Dado que el proveedor contacta sin previo aviso del sistema, cuando no existe el evento de preaviso en el historial, entonces el analista puede generarlo manualmente desde la vista de pedido.
  - Fuente: _cliente.md_ · R-29

- **[US-09]** Como Especialista de eCommerce y Servicio al Cliente, quiero que el cliente reciba una notificación proactiva cuando cambia la fecha de entrega, explicando qué cambió y cuál es la nueva fecha, para que el cliente no se entere al preguntar.
  - Criterios de aceptación:
    - Dado que el proveedor modifica la fecha prometida de un pedido, cuando guarda el cambio, entonces el sistema genera automáticamente una notificación al cliente: "La fecha estimada de entrega de [producto] cambió de [fecha anterior] a [nueva fecha]."
    - Dado que el mensaje de cambio de fecha se genera, cuando el cliente lo recibe, entonces incluye un canal de contacto central para consultas; no redirige al proveedor directamente.
  - Fuente: _especialista_de_ecommerce_y_servicio_al_cliente.md, entrevista_proveedor.md, cliente.md_ · R-14

---

## Prioridad 3 — Checkout diferenciado por modalidad

- **[US-10]** Como Especialista de eCommerce y Servicio al Cliente, quiero que la ficha de cada producto muestre su modalidad de entrega disponible (Pickup / Delivery / Dropshipping) con tiempo estimado antes de que el cliente llegue al checkout, para que las expectativas estén claras desde el inicio.
  - Criterios de aceptación:
    - Dado un producto Pickup, cuando el cliente lo consulta en la ficha, entonces ve las tiendas donde está disponible y la fecha estimada desde la que puede retirar.
    - Dado un producto Dropshipping, cuando el cliente lo consulta, entonces ve que la entrega es realizada por un proveedor autorizado con el tiempo estimado correspondiente.
    - Dado que no hay stock en ninguna tienda para un Pickup, cuando el cliente consulta, entonces la opción Pickup aparece deshabilitada con mensaje explicativo; no se genera una promesa falsa.
  - Fuente: _cliente.md, especialista_de_ecommerce_y_servicio_al_cliente.md_ · R-26, R-10

- **[US-11]** Como Especialista de eCommerce y Servicio al Cliente, quiero que el checkout separe visualmente los grupos logísticos del carrito y requiera que el cliente acepte explícitamente que puede recibir entregas en fechas distintas, para eliminar el reclamo de "pensé que todo llegaba junto".
  - Criterios de aceptación:
    - Dado un carrito con productos de múltiples modalidades, cuando el cliente llega al paso de entrega, entonces los productos están agrupados en secciones: "Retiras en tienda", "Entrega a domicilio" y "Entrega directa del proveedor".
    - Dado que hay al menos dos modalidades en el carrito, cuando el cliente llega a la confirmación de pago, entonces debe marcar activamente que entiende que los productos pueden entregarse en fechas distintas.
    - Dado que un producto solo admite Pickup, cuando el cliente intenta seleccionar envío a domicilio para ese producto, entonces el sistema bloquea la opción con un mensaje claro y muestra las tiendas disponibles.
  - Fuente: _especialista_de_ecommerce_y_servicio_al_cliente.md, cliente.md_ · R-11, R-12

- **[US-12]** Como Cliente, quiero que la notificación de "pedido confirmado" y la de "listo para retirar" sean mensajes distintos y explícitos, para no ir a la tienda antes de tiempo.
  - Criterios de aceptación:
    - Dado que el cliente completa el pago de un Pickup, cuando recibe la confirmación de compra, entonces el mensaje incluye explícitamente: "Tu producto aún está siendo preparado. Te avisaremos cuando esté listo para retirar."
    - Dado que el operador marca el pedido como "Listo para retiro", cuando el sistema envía la notificación, entonces el mensaje dice claramente: "Tu producto [nombre] ya está disponible para retiro en [tienda], [dirección]. Puedes ir a partir de ahora."
    - Dado que el cliente llega a la tienda antes de recibir la notificación de "listo para retiro", cuando el operador consulta el estado del pedido, entonces ve que aún no está confirmado y puede explicárselo al cliente con el estado actual.
  - Fuente: _cliente.md_ · R-27

- **[US-13]** Como Cliente, quiero recibir opciones inmediatas cuando el producto Pickup que compré no está disponible en la tienda seleccionada, para no quedarme sin solución ni tener que llamar.
  - Criterios de aceptación:
    - Dado que el operador detecta que el producto Pickup no está disponible, cuando lo marca en el sistema, entonces el cliente recibe automáticamente una notificación con las opciones disponibles: cambiar de tienda, recibir a domicilio (si aplica), esperar reposición o cancelar esa línea.
    - Dado que el cliente selecciona una opción de reemplazo, cuando confirma, entonces el sistema actualiza el pedido y notifica al operador o área responsable de la nueva modalidad.
  - Fuente: _cliente.md_ · R-28

---

## Prioridad 4 — Flujo Pickup (tienda)

- **[US-14]** Como Operador de Tienda, quiero recibir una alerta dentro del sistema cuando se genera un pedido Pickup, que escale al jefe de tienda si no se atiende en el tiempo configurado, para no perder ningún pedido por un correo que se mezcló con los demás.
  - Criterios de aceptación:
    - Dado que se genera un pedido Pickup para mi tienda, cuando el pedido es confirmado, entonces aparece una alerta en el módulo de tareas del operador.
    - Dado que la alerta no es atendida en el tiempo configurado, cuando se cumple el plazo, entonces el sistema escala automáticamente una notificación al jefe de tienda con el pedido afectado.
  - Fuente: _operador_de_tienda.md_ · R-16

- **[US-15]** Como Operador de Tienda, quiero que el sistema muestre si el stock de un producto está realmente disponible para retiro o tiene alguna condición que lo impida (comprometido, en exhibición), para no confirmarle al cliente algo que no puedo cumplir.
  - Criterios de aceptación:
    - Dado un producto con unidades en inventario, cuando el operador consulta su disponibilidad para un pedido Pickup, entonces el sistema muestra el estado: libre / comprometido / en exhibición / con novedad.
    - Dado que el operador confirma disponibilidad, cuando el sistema registra la reserva, entonces ese stock queda marcado como comprometido y no puede ser tomado por otro pedido o canal hasta que se libere.
  - Fuente: _operador_de_tienda.md_ · R-17

- **[US-16]** Como Operador de Tienda, quiero registrar digitalmente quién retiró el producto, con fecha, hora y opción de foto o firma, para tener trazabilidad ante cualquier reclamo sin buscar papeles.
  - Criterios de aceptación:
    - Dado que el cliente se presenta a retirar, cuando el operador registra la entrega, entonces debe capturar: nombre del receptor, nombre del operador, fecha y hora. Los campos son obligatorios.
    - Dado que el operador adjunta firma o foto, cuando guarda, entonces el archivo queda vinculado al historial del pedido accesible para el especialista de eCommerce.
  - Fuente: _operador_de_tienda.md_ · R-19

- **[US-17]** Como Operador de Tienda, quiero que el sistema aplique una regla de vencimiento de reserva configurable, con recordatorio al cliente antes del plazo y liberación solo tras autorización, para no tener stock bloqueado indefinidamente.
  - Criterios de aceptación:
    - Dado que un pedido Pickup lleva más del X% del plazo configurado sin retirarse, cuando se cumple el umbral de recordatorio, entonces el sistema envía aviso al cliente y una tarea al operador para hacer seguimiento.
    - Dado que vence el plazo total, cuando el sistema lo detecta, entonces genera una tarea de autorización para el operador o jefe de tienda; no libera la reserva de forma automática.
  - Fuente: _operador_de_tienda.md_ · R-20
