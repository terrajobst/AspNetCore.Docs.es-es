# <a name="custom-model-binding-demo"></a>Demo de enlace de modelos personalizados

Puede probar el `ByteArrayModelBinder` , ejecute la aplicación y exponer una cadena codificada en base64 para el punto de conexión ImageController (/ api/imágenes /). Debe especificar el archivo y nombre de archivo proparties en el cuerpo como datos de formulario de solicitud (mediante Postman u otra herramienta similar). Puede usar [esta cadena de ejemplo](Base64String.txt). El resultado se guardará en la carpeta wwwroot/imágenes/carga con el nombre de archivo especificado.

Para probar el ejemplo de enlace personalizado, intente los siguientes extremos: /api/authors/1 /api/authors/2 (no encontrado) /api/boundauthors/1 /api/boundauthors/2 (no encontrado) /api/boundauthors/get/1 /api/boundauthors/get/2 (sin contenido) - no comprueba esta acción NULL y devolver un no encontrado
