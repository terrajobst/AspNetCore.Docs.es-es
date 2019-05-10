# <a name="response-compression-sample-application-aspnet-core-1x"></a>Aplicación de ejemplo de compresión de respuesta (ASP.NET Core 1.x)

En este ejemplo se muestra el uso de ASP.NET Core 1.x Middleware de compresión de respuestas para comprimir las respuestas HTTP para. El ejemplo muestra cómo Gzip y proveedores de compresión personalizado para las respuestas de texto e imagen y muestra cómo agregar un tipo MIME para compresión. El ejemplo de ASP.NET Core 2.x, consulte [aplicación de ejemplo de compresión de respuesta (ASP.NET Core 2.x)](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples/2.x).

## <a name="examples-in-this-sample"></a>Ejemplos que se incluyen

* `GzipCompressionProvider`
  * `text/plain`
    * **/** : Respuesta de archivo de texto Lorem Ipsum 2,044 bytes que se comprimen en 927 bytes
    * **/testfile1kb.txt** -respuesta de archivo de texto en 1,033 bytes que se comprimen en bytes 47
    * **/ gradual** -respuesta que emite como caracteres individuales en intervalos de 1 segundo
  * `image/svg+xml`
    * **/banner.SVG** -respuesta de Scalable Vector Graphics (SVG) de una imagen en 9,707 bytes que se comprimen en bytes 4,459
* `CustomCompressionProvider`<br>Se muestra cómo implementar un proveedor de compresión personalizado para su uso con el software intermedio

Cuando la solicitud incluye el `Accept-Encoding` encabezado, el ejemplo agrega un `Vary: Accept-Encoding` encabezado a la respuesta. El `Vary` encabezado indica a las memorias caché mantener varias copias de la respuesta según los valores alternativos de `Accept-Encoding`, por lo que un comprimidos (gzip) y la versión sin comprimir se almacenan en las memorias caché de los sistemas que pueden aceptan comprimido o respuesta sin comprimir.

## <a name="using-the-sample"></a>Utilizar el ejemplo

1. Realizar una solicitud con [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), o [Postman](https://www.getpostman.com/) a la aplicación sin un `Accept-Encoding` encabezado y tenga en cuenta la carga de respuesta, el tamaño de respuesta, y encabezados de respuesta.
1. Agregar un `Accept-Encoding: gzip` encabezado y tenga en cuenta el tamaño de respuesta comprimida y encabezados de respuesta. Vea el tamaño de respuesta quitar y el `Content-Encoding: gzip` la aplicación de ejemplo incluye el encabezado de respuesta. Cuando se observa el cuerpo de respuesta para la Lorem Ipsum o **testfile1kb.txt** respuesta, verá que el texto está comprimido y no se puede leer.
1. Agregar un `Accept-Encoding: mycustomcompression` encabezado y tenga en cuenta los encabezados de respuesta. El `CustomCompressionProvider` es una implementación vacía que realmente no comprimir la respuesta, pero puede crear un contenedor de secuencia de compresión personalizado para el `CreateStream()` método.
