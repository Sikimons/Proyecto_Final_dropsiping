# ADR-0003 · Portal del proveedor como módulo frontend dedicado
**Estado:** aceptado
**Fecha:** 2026-07-04

## Contexto y fuerza

`requisitos.md → R-30`: "El portal del proveedor debe ser **simple de operar**: pocos campos por acción, flujo lineal por pedido. Si el formulario es percibido como excesivo, el proveedor volverá al correo."

`mvp-canvas.md → Riesgo #1` (adoptado del texto literal): "El proveedor entrevistado advirtió explícitamente que si tiene demasiados campos, abandona el portal." y "El portal debe permitir completar cada hito en ≤ 3 acciones."

`mvp-canvas.md → Notas de implementación: "Validar con al menos un proveedor piloto antes de lanzar a todos."` — esto implica que el portal del proveedor debe poderse modificar con rapidez sin afectar el dashboard interno del analista.

El analista, por su parte, necesita una vista de densidad de información alta (muchos pedidos a la vez, filtros, alertas) que es incompatible con la filosofía "3 acciones" del portal del proveedor.

## Decisión

El portal del proveedor se implementa como un **módulo frontend dedicado** (ruta `/proveedor`, bundle separado), con su propia hoja de estilos y flujo de navegación. El dashboard del analista vive en un módulo aparte (ruta `/analista`).

Ambos módulos comparten el mismo backend (API Core) pero no comparten componentes de UI para evitar que la complejidad del dashboard "contamine" la experiencia del proveedor.

## Alternativas consideradas

- **Una sola SPA con roles y vistas condicionales** — el código de la vista del analista (tablas densas, filtros avanzados) acaba siendo cargado por el proveedor aunque no lo use. Más importante: los cambios en el dashboard del analista pueden romper el flujo del proveedor inadvertidamente. Descartado.
- **Aplicación nativa móvil para el proveedor** — el inbox menciona proveedores que operan desde el campo (US-04). Una PWA responsive cubre el caso móvil sin el costo de una app nativa para MVP. Descartado para esta fase.

## Consecuencias

**Ganamos:** la experiencia del proveedor puede iterarse de forma independiente. El bundle del proveedor es pequeño y carga rápido desde campo (conexión móvil). El analista puede tener una UI rica sin comprometer la simplicidad del proveedor.

**Costo aceptado:** dos módulos frontend implican algo de duplicación de código utilitario (llamadas a API, componentes base). Se mitiga con una biblioteca compartida interna de bajo nivel.
