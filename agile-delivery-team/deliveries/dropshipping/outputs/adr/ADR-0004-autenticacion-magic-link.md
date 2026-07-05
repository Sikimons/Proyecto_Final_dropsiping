# ADR-0004 · Magic-link para autenticación del proveedor
**Estado:** aceptado
**Fecha:** 2026-07-04

## Contexto y fuerza

`requisitos.md → R-30`: "El portal del proveedor debe ser simple de operar: pocos campos por acción, flujo lineal por pedido."

`mvp-canvas.md → Riesgo #1`: si el portal es percibido como complejo, el proveedor vuelve al correo. El login con contraseña suma fricción: el proveedor no accede al portal diariamente, olvidará la contraseña y el restablecimiento se vuelve una barrera que lo devuelve al correo.

`personas.md → Proveedor: objetivo principal` es "recibir las órdenes con información completa a través de un canal único y poder actualizar el estado sin depender del correo". Cualquier mecanismo de acceso que requiera gestión de credenciales compite con ese objetivo.

Los usuarios internos (analista, especialista de eCommerce) sí acceden al sistema diariamente y probablemente ya tienen una identidad corporativa (LDAP/SSO). Son dos poblaciones con requisitos de autenticación distintos.

## Decisión

**Proveedor:** autenticación por **magic-link** enviado al email del proveedor registrado. El proveedor hace clic en el enlace y queda autenticado con una sesión de duración configurable. No existen contraseñas para proveedores.

**Usuarios internos:** autenticación por SSO corporativo (si existe) o por sesión con usuario+contraseña gestionados internamente. Esta decisión se delega al equipo de infraestructura; el módulo de auth expone una interfaz que puede conectarse a cualquier proveedor de identidad.

## Alternativas consideradas

- **Usuario + contraseña para el proveedor** — requiere gestión de contraseñas, restablecimiento, bloqueo por intentos fallidos. Alta fricción para un usuario que accede esporádicamente. Descartado.
- **OAuth / Google / Microsoft** — requiere que el proveedor tenga cuenta en ese proveedor de identidad. No todos los proveedores del catálogo lo tienen. Añade una dependencia externa en el flujo crítico. Descartado para MVP.
- **SMS OTP** — canal adicional (proveedor de SMS), costo por mensaje, no todos los proveedores tienen número verificado en el sistema. Puede añadirse como alternativa al magic-link en v2. Descartado para MVP.

## Consecuencias

**Ganamos:** cero fricción de acceso para el proveedor. El "login" es abrir el correo y hacer clic — flujo ya familiar para quien opera por correo hoy. Reduce una barrera de adopción del portal.

**Costo aceptado:** el magic-link depende de que el proveedor tenga acceso a su correo en el momento de operar. Si el proveedor opera desde campo sin acceso a correo, no puede autenticarse en tiempo real. Mitigación: las sesiones tienen duración larga (configurable), por lo que el proveedor puede autenticarse antes de salir a campo.
