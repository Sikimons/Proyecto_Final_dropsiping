# Sprint 1 — Ciclo Dropshipping completo en portal del proveedor + trazabilidad del analista

**Sprint Goal:** El proveedor opera el ciclo Dropshipping completo desde el portal (aceptar, despachar, entregar, reportar novedades) con inventario reservado; el analista puede auditar cualquier cambio de estado con actor y timestamp — sin depender del correo en ninguno de los dos casos.

**Capacidad:** 20 pts · **Comprometido:** 20 pts

---

## Historias comprometidas

| Historia | Título | Épica | Pts | Prioridad |
|----------|--------|-------|-----|-----------|
| US-01 | Ver y gestionar órdenes asignadas | E-01 | 5 | 1 |
| US-02 | Registrar despacho con tracking | E-01 | 3 | 2 |
| US-03 | Confirmar entrega con evidencia obligatoria | E-01 | 3 | 3 |
| US-04 | Registrar novedad de entrega desde el campo | E-01 | 3 | 4 |
| US-05 | Reservar inventario al confirmar pedido | E-01 | 3 | 5 |
| US-08 | Historial inmutable de cambios de estado | E-02 | 3 | 8 |

---

## Decisiones del Scrum Master

### ¿Por qué US-08 antes que US-06 y US-07?

US-06 (Vista global de pedidos, 5 pts) requeriría 17 + 5 = **22 pts**, superando la capacidad de 20 pts. No entra en este sprint.

US-08 (Historial inmutable, 3 pts) **solo depende de US-01**, que sí está en el sprint. Aporta valor tangible: el analista puede auditar los cambios de estado que el proveedor realiza durante el sprint, verificando la trazabilidad end-to-end en el propio sprint de validación. Llevar US-08 junto con E-01 completa permite demostrar el ciclo proveedor → registro → auditoría en la Sprint Review.

US-07 y US-09 quedan bloqueados (dependen de US-06 y US-08 respectivamente; US-07 requiere US-06 que no entra).

### ¿Por qué no incluir E-03 (notificaciones)?

US-10, US-11, US-12 dependen de US-01, US-02, US-03 como precondición técnica (deben existir los eventos de estado para disparar notificaciones). Aunque las dependencias estarían satisfechas al final del sprint, incluirlas sumaría 10 pts adicionales (30 pts total), muy por encima de la capacidad. Se planifican íntegras en Sprint 2.

---

## Backlog del siguiente sprint (provisional)

| Historia | Título | Épica | Pts | Bloqueo |
|----------|--------|-------|-----|---------|
| US-06 | Vista global pedidos activos | E-02 | 5 | ninguno |
| US-07 | Alerta automática por inactividad | E-02 | 3 | US-06 |
| US-09 | Registro manual de updates por correo | E-02 | 2 | US-08 ✓ |
| US-10 | Notificaciones automáticas hitos clave | E-03 | 5 | US-01 US-02 US-03 ✓ |
| US-11 | Notificación proactiva cambio de fecha | E-03 | 3 | US-10 |
| US-12 | Preaviso antes de contacto del proveedor | E-03 | 2 | US-10 |
| **Total S2** | | | **20 pts** | |

Sprint 2 cierra E-02 + E-03 con exactamente 20 pts.

---

## Definición de Done del sprint

- Todos los cambios de estado del proveedor (US-01 a US-04) quedan registrados en el audit log con actor y timestamp (verifica US-08).
- El proveedor puede completar cada hito en ≤ 3 acciones (criterio R-30).
- La unidad de inventario queda marcada "comprometida" al aceptar y se libera al rechazar (US-05).
- El historial de US-08 muestra los eventos generados por US-01 a US-04 en la sprint review.
- Ningún cambio de estado puede realizarse fuera de las transiciones definidas en ADR-0006.
