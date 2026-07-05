# ADR-0001 · Monolito modular sobre microservicios
**Estado:** aceptado
**Fecha:** 2026-07-04

## Contexto y fuerza

El MVP de Dropshipping debe ser entregado por un equipo pequeño en un ciclo corto. Las funcionalidades cubren un dominio cohesionado: un proveedor opera sus pedidos, un analista los monitorea, y el sistema notifica al cliente. No existe un mandato de escalabilidad independiente por módulo en esta fase.

Fuerza principal: `mvp-canvas.md → Riesgo #1: "si el portal es percibido como excesivo, el proveedor vuelve al correo"`. Reducir la complejidad del sistema reduce también la complejidad operativa y los tiempos de ciclo de desarrollo, lo que protege la fecha de lanzamiento.

Fuerza secundaria: `requisitos.md → R-25`: "antes de activar la automatización, el sistema debe validar que las reglas, responsables y excepciones estén definidos; no debe automatizar sobre información incompleta". Un monolito permite definir esas reglas en un solo lugar antes de distribuirlas.

## Decisión

Se adopta un **monolito modular**: una sola base de código desplegada como una unidad, con módulos internos bien delimitados (proveedor, analista, notificaciones, core/estados). Cada módulo expone una interfaz interna clara y no accede directamente a la base de datos de otro módulo.

## Alternativas consideradas

- **Microservicios** — requiere infraestructura de orquestación (service mesh, trazabilidad distribuida, múltiples pipelines CI/CD). Costo operativo desproporcionado para MVP y equipo pequeño. Distribuye la complejidad antes de que el dominio esté estabilizado.
- **Functions-as-a-service / serverless** — dificulta la gestión de estado transaccional (outbox, máquina de estados). El cold start afecta la experiencia del proveedor en el portal.

## Consecuencias

**Ganamos:** velocidad de desarrollo, un solo pipeline de CI/CD, transacciones ACID naturales entre módulos (crítico para outbox + cambio de estado atómico), menor carga cognitiva.

**Costo aceptado:** si en el futuro un módulo necesita escalar de forma independiente (p.ej. notificaciones en picos), se requerirá extraerlo. Los límites de módulo definidos hoy facilitan esa extracción futura sin reescritura total.
