# <a name="response-compression-sample-application-aspnet-core-2x"></a>Aplicación de ejemplo de compresión de respuesta (ASP.NET Core 2.x)

En este ejemplo se muestra el uso de ASP.NET Core 2.x Middleware de compresión de respuestas para comprimir las respuestas HTTP. El ejemplo muestra cómo Gzip, Brotli y proveedores de compresión personalizado para las respuestas de texto e imagen y muestra cómo agregar un tipo MIME para compresión. El ejemplo de ASP.NET Core 1.x, vea [aplicación de ejemplo de compresión de respuesta (ASP.NET Core 1.x)](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples/1.x).

## <a name="examples-in-this-sample"></a>Ejemplos que se incluyen

* `BrotliCompressionProvider`
  * `text/plain`
    * **/** -Lorem Ipsum texto respuesta de archivo en bytes 2,044 que comprime a ~ 979 bytes.
    * **/testfile1kb.txt** -respuesta de archivo de texto en bytes 1,033 que comprime a ~ 36 bytes.
    * **/ gradual** -respuesta que emite como caracteres individuales en intervalos de 1 segundo.
* `GzipCompressionProvider`
  * `text/plain`
    * **/** -Lorem Ipsum texto respuesta de archivo en bytes 2,044 que comprime a ~ 927 bytes.
    * **/testfile1kb.txt** -respuesta de archivo de texto en bytes 1,033 que comprime a ~ 47 bytes.
    * **/ gradual** -respuesta que emite como caracteres individuales en intervalos de 1 segundo.
  * `image/svg+xml`
    * **/banner.SVG** -respuesta de imagen A Scalable Vector Graphics (SVG) en bytes 9,707 que comprime a ~ 4,459 bytes.
* `CustomCompressionProvider`<br>Se muestra cómo implementar un proveedor de compresión personalizado para su uso con el middleware.

Cuando la solicitud incluye el `Accept-Encoding` compresión de encabezado y la respuesta es correcta, el software intermedio agrega automáticamente un `Vary: Accept-Encoding` encabezado a la respuesta. El `Vary` encabezado indica a las memorias caché mantener varias copias de la respuesta según los valores alternativos de `Accept-Encoding`, por lo que tanto un comprimidos (Gzip o Brotli) y versión sin comprimir se almacenan en las memorias caché de sistemas que pueden aceptan el comprimidos o la respuesta sin comprimir.

## <a name="use-the-sample"></a>Use el ejemplo

1. Realizar una solicitud con [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), o [Postman](https://www.getpostman.com/) a la aplicación sin un `Accept-Encoding` encabezado y tenga en cuenta la carga de respuesta, el tamaño de respuesta, y encabezados de respuesta.
1. Agregar un `Accept-Encoding: br` o `Accept-Encoding: gzip` encabezado y tenga en cuenta el tamaño de respuesta comprimida y encabezados de respuesta. Quita el tamaño de respuesta y el `Content-Encoding` el middleware que indica que la compresión con cualquier Gzip incluye el encabezado de respuesta o Brotli se ha producido. Cuando se observa el cuerpo de respuesta para la Lorem Ipsum o **testfile1kb.txt** respuesta, verá que el texto está comprimido y no se puede leer.
1. Agregar un `Accept-Encoding: mycustomcompression` encabezado y tenga en cuenta los encabezados de respuesta. El `CustomCompressionProvider` es una implementación vacía que realmente no comprimir la respuesta, pero puede crear un contenedor de secuencia de compresión personalizado para el `CreateStream()` método.
