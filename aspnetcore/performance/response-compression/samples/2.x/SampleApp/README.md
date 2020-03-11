# <a name="response-compression-sample-application-aspnet-core-2x"></a>Aplicación de ejemplo de compresión de respuesta (ASP.NET Core 2. x)

En este ejemplo se muestra el uso de middleware de compresión de respuesta de ASP.NET Core 2. x para comprimir las respuestas HTTP. En el ejemplo se muestran los proveedores de compresión personalizados, Brotli y gzip para las respuestas de texto e imagen, y se muestra cómo agregar un tipo MIME para la compresión. Para obtener el ejemplo ASP.NET Core 1. x, vea [aplicación de ejemplo de compresión de respuesta (ASP.net Core 1. x)](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples/1.x).

## <a name="examples-in-this-sample"></a>Ejemplos incluidos

* `BrotliCompressionProvider`
  * `text/plain`
    * la respuesta del archivo de texto **/** -Lorem Ipsum en 2.044 bytes que comprime hasta ~ 979 bytes.
    * **/testfile1kb.txt** : respuesta del archivo de texto en 1.033 bytes que comprime hasta ~ 36 bytes.
    * **/Trickle** : respuesta emitida como caracteres individuales a intervalos de 1 segundo.
* `GzipCompressionProvider`
  * `text/plain`
    * la respuesta del archivo de texto **/** -Lorem Ipsum en 2.044 bytes que comprime hasta ~ 927 bytes.
    * **/testfile1kb.txt** : respuesta del archivo de texto en 1.033 bytes que comprime hasta ~ 47 bytes.
    * **/Trickle** : respuesta emitida como caracteres individuales a intervalos de 1 segundo.
  * `image/svg+xml`
    * **/banner.svg** : respuesta de imagen de gráficos vectoriales escalables (SVG) en 9.707 bytes que comprime hasta ~ 4.459 bytes.
* `CustomCompressionProvider`<br>Muestra cómo implementar un proveedor de compresión personalizado para su uso con el middleware.

Cuando la solicitud incluye el encabezado `Accept-Encoding` y la compresión de respuesta es correcta, el middleware agrega automáticamente un encabezado de `Vary: Accept-Encoding` a la respuesta. El encabezado `Vary` indica a las memorias caché que conserven varias copias de la respuesta en función de valores alternativos de `Accept-Encoding`, por lo que tanto una versión comprimida (gzip o Brotli) como una versión sin comprimir se almacenan en cachés para sistemas que pueden aceptar la respuesta comprimida o sin comprimir.

## <a name="use-the-sample"></a>Usar el ejemplo

1. Realice una solicitud con [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/)o [Postman](https://www.getpostman.com/) a la aplicación sin un encabezado `Accept-Encoding` y anote la carga de respuesta, el tamaño de la respuesta y los encabezados de respuesta.
1. Agregue un encabezado `Accept-Encoding: br` o `Accept-Encoding: gzip` y tenga en cuenta el tamaño de la respuesta comprimida y los encabezados de respuesta. El tamaño de la respuesta desciende y el encabezado de respuesta `Content-Encoding` está incluido en el middleware que indica que se ha producido la compresión con gzip o Brotli. Cuando examine el cuerpo de la respuesta para la respuesta Lorem ipsum o **testfile1kb. txt** , verá que el texto está comprimido e ilegible.
1. Agregue un encabezado `Accept-Encoding: mycustomcompression` y anote los encabezados de respuesta. El `CustomCompressionProvider` es una implementación vacía que no comprime realmente la respuesta, pero puede crear un contenedor de secuencias de compresión personalizado para el método `CreateStream()`.
