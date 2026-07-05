# ADR-0007 · Object storage para evidencias de entrega
**Estado:** aceptado
**Fecha:** 2026-07-04

## Contexto y fuerza

`user-stories.md → US-03` (criterio de aceptación): "Dado que el proveedor adjunta evidencia válida (firma, foto o nombre del receptor), cuando guarda, entonces el pedido cambia a 'Entregado' y **el archivo queda vinculado al historial**."

`user-stories.md → US-04` (criterio de aceptación): "Dado que el proveedor adjunta foto en la novedad, cuando guarda, entonces la imagen queda **vinculada a la incidencia** como evidencia de estado en campo."

`requisitos.md → R-07`: "Al confirmar la entrega, el sistema debe requerir evidencia (firma, fotografía o nombre de quien recibió) como condición para marcar el pedido como entregado."

Las evidencias son archivos binarios (imágenes, firmas digitales) que pueden pesar varios megabytes. Almacenarlos directamente en la base de datos relacional degradaría el rendimiento de las consultas y aumentaría el tamaño de los backups innecesariamente.

`mvp-canvas.md → Riesgo #1`: el proveedor opera desde campo, posiblemente con conexión móvil degradada. El mecanismo de upload debe ser robusto ante interrupciones y no bloquear la UI mientras el archivo sube.

## Decisión

Los archivos de evidencia se almacenan en **object storage compatible con S3** (AWS S3, MinIO, Google Cloud Storage, u otro según infraestructura disponible). La base de datos relacional almacena únicamente la **referencia** (URL o clave del objeto) junto con metadatos: `{order_event_id, file_key, file_type, uploaded_at, uploaded_by}`.

El upload se realiza mediante **presigned URL**: el backend genera una URL de subida temporal y se la entrega al frontend; el frontend sube el archivo directamente al object storage sin pasar por el servidor de aplicación. Una vez confirmado el upload, el frontend notifica al backend para registrar la referencia.

## Alternativas consideradas

- **Almacenamiento en columna BLOB en la base de datos** — funciona para volúmenes pequeños, pero degrada el rendimiento de la DB, dificulta backups selectivos y no escala bien si se agregan fotos de alta resolución en campo. Descartado.
- **Filesystem del servidor de aplicación** — no es compatible con despliegues de múltiples instancias (el archivo queda en un solo nodo). Requiere un volumen compartido o NFS, añadiendo dependencia de infraestructura. Descartado.
- **Codificación base64 en JSON** — solo válido para archivos muy pequeños. Las fotos desde móvil pueden superar 2-5 MB; base64 las inflaría un 33 % adicional. Descartado.

## Consecuencias

**Ganamos:** la base de datos relacional no almacena blobs. Los archivos de evidencia son accesibles directamente desde su URL (con firma temporal para control de acceso). El sistema puede escalar el almacenamiento de forma independiente al de datos transaccionales.

**Costo aceptado:** el upload en dos pasos (presigned URL + confirmación) añade complejidad al frontend del proveedor. Se mitiga con un componente de upload reutilizable entre el portal del proveedor y cualquier otro punto donde se requiera subir archivos.

**Supuesto pendiente:** el proveedor de object storage concreto (AWS, GCS, MinIO) no está definido en el inbox y depende de la infraestructura existente de la empresa. La decisión de proveedor se delega al equipo de infraestructura; la capa de aplicación solo debe conocer la interfaz S3-compatible.
