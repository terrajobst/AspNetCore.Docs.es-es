# <a name="aspnet-core-response-caching-sample-aspnet-core-2x"></a>Almacenamiento en caché de ejemplo de respuesta de núcleo de ASP.NET (ASP.NET Core 2.x)

Este ejemplo muestra el uso de ASP.NET Core [Middleware de almacenamiento en caché de respuesta](xref:performance/caching/middleware) con ASP.NET Core 2.x. Para obtener el ejemplo de 1.x ASP.NET Core, vea [ejemplo de almacenamiento en caché de ASP.NET Core respuesta (ASP.NET Core 1.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/middleware/samples/1.x).

La aplicación envía una `Hello World!` mensaje y la hora actual junto con un `Cache-Control` encabezado para configurar el comportamiento de almacenamiento en caché. La aplicación también envía una `Vary` encabezado para configurar la caché para dar servicio a la respuesta solo si el `Accept-Encoding` encabezado de las solicitudes posteriores coincide con la solicitud original.

Al ejecutar el ejemplo, una respuesta se sirve desde la memoria caché cuando sea posible y se almacenan hasta 10 segundos.
