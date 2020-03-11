# <a name="aspnet-core-response-caching-sample"></a>ASP.NET Core ejemplo de almacenamiento en caché de respuestas

Este ejemplo muestra el uso de middleware de [almacenamiento en caché](https://docs.microsoft.com/aspnet/core/performance/caching/middleware)de ASP.net Core respuesta.

La aplicación responde con su página de índice, incluido un encabezado `Cache-Control` para configurar el comportamiento del almacenamiento en caché. La aplicación también establece el encabezado `Vary` para configurar la memoria caché para que atienda la respuesta solo si el encabezado `Accept-Encoding` de las solicitudes posteriores coincide con el de la solicitud original.

Al ejecutar el ejemplo, la página de índice se sirve desde la memoria caché al almacenarse y almacenarse en la memoria caché durante 10 segundos.

Para probar el comportamiento del almacenamiento en caché:

* No use un explorador para probar el comportamiento de almacenamiento en caché. A menudo, los exploradores agregan un encabezado de control de caché en la recarga que impide que el middleware proporcione una página almacenada en caché. Por ejemplo, el explorador puede Agregar un encabezado `Cache-Control` con un valor de `max-age=0`).
* Use una herramienta de desarrollador que permita establecer los encabezados de solicitud explícitamente, como <a href="https://www.telerik.com/fiddler">Fiddler</a> o <a href="https://www.getpostman.com/">Postman</a>.
