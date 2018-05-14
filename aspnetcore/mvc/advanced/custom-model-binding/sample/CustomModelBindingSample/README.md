# <a name="custom-model-binding-demo"></a>Demostración de enlace de modelos personalizado

Pruebe `ByteArrayModelBinder`; para ello, ejecute la aplicación y publique una cadena codificada en base64 en el punto de conexión `ImageController` (`/api/image/`). Especifique las propiedades de archivo y nombre de archivo en el cuerpo de la solicitud como datos de formulario (con [Postman](https://www.getpostman.com/) o con otra herramienta similar). Puede usar [esta cadena de ejemplo](Base64String.txt). El resultado se guardará en la carpeta *wwwroot/images/upload* con el nombre de archivo especificado.

Para probar el ejemplo de enlace personalizado, hágalo con los siguientes puntos de conexión:

* /api/authors/1
* /api/authors/2 (NOT FOUND)
* /api/boundauthors/1
* /api/boundauthors/2 (NOT FOUND)
* /api/boundauthors/get/1
* /api/boundauthors/get/2 (NO CONTENT) &ndash; Esta acción no comprueba si hay valores null y devuelve un error *404 No encontrado*.
