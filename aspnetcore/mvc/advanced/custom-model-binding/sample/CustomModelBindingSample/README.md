# <a name="custom-model-binding-demo"></a>Demostración de enlace de modelos personalizado

Puede probar `ByteArrayModelBinder` si ejecuta la aplicación y publica una cadena codificada en base64 en el punto de conexión ImageController (/api/image/). Debe especificar las propiedades de archivo y nombre de archivo en el cuerpo de la solicitud como datos de formulario (mediante Postman u otra herramienta similar). Puede usar [esta cadena de ejemplo](Base64String.txt). El resultado se guardará en la carpeta wwwroot/images/upload con el nombre de archivo especificado.

Para probar el ejemplo de enlace personalizado, pruebe los siguientes puntos de conexión: //api/authors/1 /api/authors/2 (NO SE ENCONTRÓ) /api/boundauthors/1 /api/boundauthors/2 (NO SE ENCONTRÓ) /api/boundauthors/get/1 /api/boundauthors/get/2 (SIN CONTENIDO): esta acción no comprueba el valor NULL y devuelve No se encontró
