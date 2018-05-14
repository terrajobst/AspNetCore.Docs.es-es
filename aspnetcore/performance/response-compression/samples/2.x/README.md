# <a name="response-compression-sample-application-aspnet-core-2x"></a>Aplicación de ejemplo de compresión de respuesta (ASP.NET Core 2.x)

Este ejemplo muestra el uso de ASP.NET Core 2.x Middleware de compresión de respuesta para comprimir las respuestas HTTP para. El ejemplo muestra Gzip y proveedores personalizados de compresión para las respuestas de texto e imagen y muestra cómo agregar un tipo MIME para la compresión. Para obtener el ejemplo de 1.x ASP.NET Core, vea [aplicación de ejemplo de compresión de respuesta (ASP.NET Core 1.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples/1.x).

## <a name="examples-in-this-sample"></a>En los ejemplos de este ejemplo
* `GzipCompressionProvider`
  * `text/plain`
    * **/**-Respuesta de archivo de texto Lorem Ipsum en 2,044 bytes que se comprimirá 927 bytes
    * **/testfile1kb.txt** -respuesta de archivo de texto en 1,033 bytes que se comprimirá a bytes 47
    * **/ gradual** -respuesta emitida como caracteres individuales a intervalos de 1 segundo 
  * `image/svg+xml`
    * **/banner.SVG** -respuesta de imagen A Scalable Vector Graphics (SVG) en 9,707 bytes que se comprimirá a 4,459 bytes
* `CustomCompressionProvider`<br>Muestra cómo implementar un proveedor de compresión personalizada para su uso con el software intermedio

Cuando la solicitud incluye el `Accept-Encoding` compresión de encabezado y de respuesta es correcta, el middleware agrega automáticamente un `Vary: Accept-Encoding` encabezado a la respuesta. El `Vary` encabezado indica a las memorias caché mantener varias copias de la respuesta en función de los valores alternativos de `Accept-Encoding`, por lo tanto un comprimidos (gzip) y una versión sin comprimir se almacenan en las cachés para sistemas que pueden aceptan el comprimido o respuesta sin comprimir.

## <a name="using-the-sample"></a>Utilizar el ejemplo
1. Realizar una solicitud con [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), o [Postman](https://www.getpostman.com/) a la aplicación sin un `Accept-Encoding` encabezado y tenga en cuenta la carga de respuesta, el tamaño de respuesta, y encabezados de respuesta.
2. Agregar un `Accept-Encoding: gzip` encabezado y tenga en cuenta el tamaño comprimido de respuesta y encabezados de respuesta. Quita el tamaño de respuesta y el `Content-Encoding: gzip` encabezado de respuesta se incluye el middleware. Cuando examine el cuerpo de respuesta por Lorem Ipsum o **testfile1kb.txt** respuesta, verá que el texto está comprimido y no se puede leer.
3. Agregar un `Accept-Encoding: mycustomcompression` encabezado y tenga en cuenta los encabezados de respuesta. El `CustomCompressionProvider` es una implementación vacía que realmente no comprimir la respuesta, pero puede crear un contenedor de secuencia de compresión personalizada para el `CreateStream()` método.
