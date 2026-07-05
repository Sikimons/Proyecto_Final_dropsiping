Reflexión — Agile Delivery Team (caso: dropshipping)

¿Qué cambió en tu plan cuando separaste el trabajo en cuatro roles en vez de pensarlo "todo junto"?

Al analizar el plan desde los cuatro roles definidos, el principal cambio fue la forma de organizar el trabajo y establecer prioridades, pues al analizar el proyecto como un solo bloque, todas las tareas parecían tener la misma importancia, dificultando definir por dónde empezar. En cambio, al separar las responsabilidades por roles, cada uno aportó una perspectiva específica de la siguiente manera:

- El Product Owner permitió enfocar el MVP en las funcionalidades que generaban mayor valor para el flujo de Dropshipping, dejando para fases posteriores aquellas que no eran indispensables. 
- El Architect orientó las decisiones técnicas en función de las necesidades del backlog, asegurando que la solución respondiera al problema antes que a preferencias tecnológicas. 
- El Scrum Master ayudó a ajustar el alcance del sprint a la capacidad real del equipo, priorizando lo que realmente podía entregarse. 

En conjunto, este enfoque permitió construir un plan más claro, realista y ordenado.

¿Qué historia te costó más dejar lista según INVEST, y por qué?

La historia más difícil fue US-06: Vista global de pedidos Dropshipping activos, porque inicialmente abarcaba demasiadas funcionalidades, como alertas, historial, filtros e indicadores, lo que dificultaba definir su alcance, estimarla y validarla.

Aplicando el criterio INVEST, fue necesario reducir la historia a lo esencial: 
- visualizar el estado del pedido
- el proveedor
- la fecha comprometida
- la última actualización.

El resto de funcionalidades se dividió en historias independientes, logrando una historia más clara, manejable y preparada para el desarrollo.

¿Para qué te serviría un gate de Definition of Ready en tu equipo real?

Un Definition of Ready sería útil para asegurar que las historias ingresen al sprint con la información necesaria y sin aspectos críticos pendientes. Esto evitaría dudas durante el desarrollo, cambios de alcance y retrabajos.

Además, una validación objetiva, como la realizada por el hook "dor-invest-gate.py", permite verificar que cada historia cuente con criterios de aceptación, estimación, dependencias identificadas y un alcance bien definido. De esta manera, el equipo puede iniciar el sprint con mayor claridad, reducir riesgos y concentrarse en la ejecución del trabajo.