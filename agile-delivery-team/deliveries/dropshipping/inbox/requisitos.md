# Requisitos Candidatos — Dropshipping

## Flujo de orden Dropshipping (proveedor)

- **[R-01]** Al confirmar una venta Dropshipping, el sistema debe identificar automáticamente al proveedor del producto y generar la solicitud/orden de compra correspondiente.
  - Tipo: funcional
  - Origen: _analista_de_compras_y_logistica.md_ · Analista de Compras y Logística

- **[R-02]** El sistema debe notificar al proveedor la orden generada y exigir una respuesta explícita de aceptación o rechazo con fecha estimada de despacho.
  - Tipo: funcional
  - Origen: _analista_de_compras_y_logistica.md_ · Analista de Compras y Logística

- **[R-03]** Si el proveedor rechaza el pedido, el sistema debe notificar de inmediato al equipo comercial para ofrecer una alternativa al cliente.
  - Tipo: funcional
  - Origen: _analista_de_compras_y_logistica.md_ · Analista de Compras y Logística

- **[R-04]** El proveedor debe poder actualizar la disponibilidad de sus productos en el portal; cada actualización debe registrar la fecha y hora en que fue realizada.
  - Tipo: funcional
  - Origen: _analista_de_compras_y_logistica.md_ · Analista de Compras y Logística

- **[R-05]** Cada cambio de estado del pedido debe registrar quién lo realizó y cuándo; los cambios deben agregarse al historial sin sobrescribir el estado anterior.
  - Tipo: funcional
  - Origen: _analista_de_compras_y_logistica.md, entrevista_proveedor.md_ · Analista de Compras y Logística, Proveedor

- **[R-06]** El proveedor debe poder registrar número de guía/tracking, transportista y fecha de despacho. Si no hay tracking tradicional, debe permitirse registrar placa, nombre del conductor, contacto o fecha de salida.
  - Tipo: funcional
  - Origen: _analista_de_compras_y_logistica.md, entrevista_proveedor.md_ · Analista de Compras y Logística, Proveedor

- **[R-07]** Al confirmar la entrega, el sistema debe requerir evidencia (firma, fotografía o nombre de quien recibió) como condición para marcar el pedido como entregado.
  - Tipo: funcional
  - Origen: _analista_de_compras_y_logistica.md, entrevista_proveedor.md, cliente.md_ · Analista, Proveedor, Cliente

- **[R-08]** Las novedades de entrega (cliente ausente, dirección incorrecta, entrega rechazada, etc.) deben gestionarse como incidencias con responsable asignado; no deben resolverse con un estado genérico.
  - Tipo: funcional
  - Origen: _analista_de_compras_y_logistica.md, entrevista_proveedor.md_ · Analista, Proveedor

- **[R-09]** En pedidos con múltiples proveedores, cada proveedor recibe únicamente sus líneas; el pedido avanza línea a línea de forma independiente y muestra una vista consolidada del estado general.
  - Tipo: funcional
  - Origen: _analista_de_compras_y_logistica.md_ · Analista de Compras y Logística

- **[R-32]** La orden enviada al proveedor debe incluir todos los datos necesarios para procesar sin consultas adicionales: código del producto, descripción, cantidad, dirección completa, ciudad, contacto del cliente, fecha esperada y condiciones especiales (instalación, armado, coordinación previa).
  - Tipo: funcional
  - Origen: _entrevista_proveedor.md_ · Proveedor

- **[R-33]** El sistema debe bloquear (reservar) la unidad de inventario del proveedor en el momento en que se confirma el pedido, para impedir doble venta; la reserva se libera únicamente si el pedido es rechazado o cancelado.
  - Tipo: funcional
  - Origen: _entrevista_proveedor.md_ · Proveedor

- **[R-34]** El proveedor debe poder registrar novedades de entrega (cliente no disponible, dirección incorrecta, producto dañado) desde el campo, en el momento que ocurren, con opción de adjuntar foto u observación.
  - Tipo: funcional
  - Origen: _entrevista_proveedor.md_ · Proveedor

- **[R-35]** El sistema debe permitir registrar evidencia fotográfica del estado del producto al momento de despachar (antes de la entrega), para poder determinar responsabilidades en caso de daño en tránsito.
  - Tipo: funcional
  - Origen: _entrevista_proveedor.md_ · Proveedor

## Checkout y experiencia del cliente

- **[R-10]** El checkout debe mostrar la modalidad de entrega (Pickup / Delivery / Dropshipping) por producto individual —no solo a nivel de pedido— antes de que el cliente confirme la compra.
  - Tipo: funcional
  - Origen: _especialista_de_ecommerce_y_servicio_al_cliente.md, cliente.md_ · Especialista de eCommerce, Cliente

- **[R-11]** El sistema no debe permitir seleccionar envío a domicilio para productos de modalidad exclusiva Pickup. Debe mostrar un mensaje explicativo y presentar las tiendas disponibles; si no hay tienda con stock, debe bloquear la compra con un mensaje claro.
  - Tipo: funcional
  - Origen: _especialista_de_ecommerce_y_servicio_al_cliente.md_ · Especialista de eCommerce

- **[R-12]** El checkout debe separar visualmente los grupos logísticos (retiro en tienda / entrega propia / entrega por proveedor) y advertir que pueden existir fechas de entrega distintas; el cliente debe aceptar explícitamente esta condición antes de pagar.
  - Tipo: funcional
  - Origen: _especialista_de_ecommerce_y_servicio_al_cliente.md, cliente.md_ · Especialista de eCommerce, Cliente

- **[R-13]** Las notificaciones al cliente deben indicar explícitamente a qué producto se refieren y qué cambió; no deben usar mensajes genéricos.
  - Tipo: funcional
  - Origen: _especialista_de_ecommerce_y_servicio_al_cliente.md, cliente.md_ · Especialista de eCommerce, Cliente

- **[R-14]** Cuando cambie la fecha de entrega, el sistema debe generar proactivamente una notificación al cliente explicando qué cambió y cuál es la nueva fecha.
  - Tipo: funcional
  - Origen: _especialista_de_ecommerce_y_servicio_al_cliente.md, entrevista_proveedor.md, cliente.md_ · Especialista, Proveedor, Cliente

- **[R-15]** El cliente debe tener acceso en su portal a una vista por línea de pedido que muestre el estado individual de cada producto (Pickup, Delivery, Dropshipping) y un historial cronológico simplificado, todo bajo un único número de compra.
  - Tipo: funcional
  - Origen: _especialista_de_ecommerce_y_servicio_al_cliente.md, cliente.md_ · Especialista de eCommerce, Cliente

- **[R-26]** La modalidad de entrega, tiempo estimado y disponibilidad de tiendas para Pickup deben mostrarse desde la ficha del producto, antes de llegar al checkout.
  - Tipo: funcional
  - Origen: _cliente.md_ · Cliente

- **[R-27]** La notificación de "pedido confirmado" y la de "listo para retirar" deben ser mensajes distintos y explícitos; la primera no debe dar a entender que el producto ya está disponible en tienda.
  - Tipo: funcional
  - Origen: _cliente.md_ · Cliente

- **[R-28]** Cuando el stock Pickup prometido no está disponible, el sistema debe presentar al cliente alternativas inmediatas: cambiar de tienda, recibir a domicilio (si aplica), esperar reposición o cancelar esa línea del pedido.
  - Tipo: funcional
  - Origen: _cliente.md_ · Cliente

- **[R-29]** Antes de que el proveedor contacte al cliente para coordinar la entrega, el sistema debe enviar una notificación al cliente indicando que un proveedor autorizado se comunicará, con el motivo y el canal esperado.
  - Tipo: funcional
  - Origen: _cliente.md_ · Cliente

- **[R-31]** El cliente debe tener un único canal de contacto para consultas sobre su pedido, aunque internamente participen tienda, logística y proveedor.
  - Tipo: funcional
  - Origen: _cliente.md_ · Cliente

## Flujo Pickup (tienda)

- **[R-16]** Los pedidos Pickup deben generar una alerta dentro del sistema para el operador de tienda; si nadie atiende la alerta en el tiempo configurado, debe escalar automáticamente al jefe de tienda.
  - Tipo: funcional
  - Origen: _operador_de_tienda.md_ · Operador de Tienda

- **[R-17]** El sistema debe mostrar el estado de disponibilidad real del stock para Pickup: libre para venta/retiro, comprometido, en exhibición u otra condición que lo haga no disponible.
  - Tipo: funcional
  - Origen: _operador_de_tienda.md, cliente.md_ · Operador de Tienda, Cliente

- **[R-18]** El flujo Pickup debe incluir los siguientes estados gestionables: Recibido → Validando stock → Reservado → En preparación → Listo para retiro → Entregado / Novedad.
  - Tipo: funcional
  - Origen: _operador_de_tienda.md_ · Operador de Tienda

- **[R-19]** Al registrar el retiro, el sistema debe capturar: quién entregó, quién recibió, fecha y hora; debe permitir adjuntar firma o foto del comprobante.
  - Tipo: funcional
  - Origen: _operador_de_tienda.md_ · Operador de Tienda

- **[R-20]** El sistema debe aplicar una regla de vencimiento de reserva configurable: recordar al cliente antes del vencimiento y liberar la reserva solo tras autorización explícita, nunca de forma automática silenciosa.
  - Tipo: funcional
  - Origen: _operador_de_tienda.md_ · Operador de Tienda

## Indicadores y reportes

- **[R-21]** El sistema debe exponer indicadores de desempeño del proceso Dropshipping: tiempo de aceptación del proveedor, cumplimiento de fecha prometida, pedidos rechazados por falta de stock, entregas con novedad y tiempo total hasta entrega.
  - Tipo: funcional
  - Origen: _analista_de_compras_y_logistica.md_ · Analista de Compras y Logística

- **[R-22]** El sistema debe exponer indicadores del proceso Pickup: tiempo desde recepción del pedido hasta "listo para retiro", pedidos con problemas de stock, clientes que llegan antes de la confirmación y tiempo promedio de retiro.
  - Tipo: funcional
  - Origen: _operador_de_tienda.md_ · Operador de Tienda

- **[R-36]** Los indicadores del proveedor deben incluir, además del cumplimiento de fechas, la tasa de pedidos recibidos con información incompleta y la tasa de pedidos que cambiaron después de ser aceptados, para distinguir retrasos imputables al proveedor de los causados por la empresa.
  - Tipo: funcional
  - Origen: _entrevista_proveedor.md_ · Proveedor

## No funcionales

- **[R-23]** El proveedor solo debe recibir los datos del cliente estrictamente necesarios para ejecutar la entrega; no debe tener acceso a información adicional del comprador.
  - Tipo: no funcional (privacidad de datos)
  - Origen: _analista_de_compras_y_logistica.md_ · Analista de Compras y Logística

- **[R-24]** El sistema debe soportar un proceso alternativo para proveedores que no usen el portal (comunicación por correo), manteniendo la trazabilidad completa de todos los cambios de estado.
  - Tipo: no funcional (interoperabilidad / continuidad operativa)
  - Origen: _analista_de_compras_y_logistica.md_ · Analista de Compras y Logística

- **[R-25]** Antes de activar la automatización del flujo Dropshipping, el sistema debe validar que las reglas, responsables y excepciones estén definidos; no debe automatizar sobre información o configuración incompleta.
  - Tipo: no funcional (integridad operativa)
  - Origen: _analista_de_compras_y_logistica.md_ · Analista de Compras y Logística

- **[R-30]** El portal del proveedor debe ser simple de operar: pocos campos por acción, flujo lineal por pedido. Si el formulario es percibido como excesivo, el proveedor volverá al correo.
  - Tipo: no funcional (usabilidad)
  - Origen: _entrevista_proveedor.md_ · Proveedor
