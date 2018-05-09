---
title: Middleware de ASP.NET Core
author: rick-anderson
description: Obtenga información sobre el middleware de ASP.NET Core y la canalización de solicitudes.
manager: wpickett
ms.author: riande
ms.date: 01/22/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/middleware/index
ms.openlocfilehash: 016f15c13470db53252941acafa25a3c6caf8db5
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2018
---
# <a name="aspnet-core-middleware"></a>Middleware de ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Steve Smith](https://ardalis.com/)

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/index/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-middleware"></a>¿Qué es el middleware?

El middleware es un software que se ensambla a una canalización de una aplicación para controlar las solicitudes y las respuestas. Cada componente puede hacer lo siguiente:

* Elegir si se pasa la solicitud al siguiente componente de la canalización.
* Realizar trabajos antes y después de invocar al siguiente componente de la canalización. 

Los delegados de solicitudes se usan para crear la canalización de solicitudes. Estos también controlan las solicitudes HTTP.

Los delegados de solicitudes se configuran con los métodos de extensión [Run](/dotnet/api/microsoft.aspnetcore.builder.runextensions), [Map](/dotnet/api/microsoft.aspnetcore.builder.mapextensions) y [Use](/dotnet/api/microsoft.aspnetcore.builder.useextensions). Un delegado de solicitudes se puede especificar en línea como un método anónimo (denominado middleware en línea) o se puede definir en una clase reutilizable. Estas clases reutilizables y métodos anónimos en línea son *middleware* o *componentes de middleware*. Cada componente de middleware de la canalización de solicitudes es responsable de invocar al siguiente componente de la canalización o de cortocircuitar la cadena en caso de ser necesario.

En [Migración de módulos HTTP a middleware](xref:migration/http-modules) se explica la diferencia entre las canalizaciones de solicitudes en ASP.NET Core y ASP.NET 4.x y se proporcionan más ejemplos de middleware.

## <a name="creating-a-middleware-pipeline-with-iapplicationbuilder"></a>Creación de una canalización de middleware con IApplicationBuilder

La canalización de solicitudes de ASP.NET Core está compuesta por una secuencia de delegados de solicitudes, a los que se llama uno después de otro, tal y como se muestra en este diagrama (el hilo de ejecución sigue las flechas negras):

![En el patrón de procesamiento de solicitudes se muestra una solicitud que llega, el procesamiento a través de tres middleware y la respuesta que abandona la aplicación. Cada middleware ejecuta su lógica y pasa la solicitud al siguiente middleware con la instrucción next(). Después de que el tercer middleware procese la solicitud, esta vuelve a pasar por los dos middleware anteriores en orden inverso. Esto se hace a modo de procesamiento adicional después de las instrucciones next() y antes de salir de la aplicación como respuesta al cliente.](index/_static/request-delegate-pipeline.png)

Cada delegado puede realizar operaciones antes y después del siguiente. También puede decidir no pasar una solicitud al siguiente delegado, lo que se denomina "cortocircuitar" la canalización de solicitudes. Este proceso es necesario muchas veces, ya que previene la realización de trabajo innecesario. Por ejemplo, el middleware de archivos estáticos puede devolver una solicitud para un archivo estático y cortocircuitar el resto de la canalización. Los delegados que controlan excepciones deben llamarse al principio de la canalización para que puedan capturar las excepciones que se producen en las fases siguientes de la canalización.

La aplicación ASP.NET Core más sencilla posible configura un solo delegado de solicitudes que controla todas las solicitudes. En este caso no se incluye una canalización de solicitudes real. En su lugar, solo se llama a una única función anónima en respuesta a todas las solicitudes HTTP.

[!code-csharp[](index/sample/Middleware/Startup.cs)]

El primer delegado [app.Run](/dotnet/api/microsoft.aspnetcore.builder.runextensions) finaliza la canalización.

Puede encadenar varios delegados de solicitudes con [app.Use](/dotnet/api/microsoft.aspnetcore.builder.useextensions). El parámetro `next` representa el siguiente delegado de la canalización. Recuerde que puede cortocircuitar la canalización si *no* llama al *siguiente* parámetro. Normalmente puede realizar acciones antes y después del siguiente delegado, tal y como se muestra en este ejemplo:

[!code-csharp[](index/sample/Chain/Startup.cs?name=snippet1)]

>[!WARNING]
> No llame a `next.Invoke` después de haber enviado la respuesta al cliente. Se producirá una excepción si se modifica `HttpResponse` después de haber iniciado la respuesta. Por ejemplo, se producirá una excepción al realizar cambios como el establecimiento de encabezados, el código de estado, etc. Si escribe en el cuerpo de la respuesta después de llamar a `next`:
> - Puede provocar una infracción del protocolo. Por ejemplo, si escribe más de la longitud `content-length` establecida.
> - Puede dañar el formato del cuerpo. Por ejemplo, si escribe un pie de página en HTML en un archivo CSS.
>
> [HttpResponse.HasStarted](/dotnet/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) es una sugerencia útil para indicar si se han enviado los encabezados o si se han realizado escrituras en el cuerpo.

## <a name="ordering"></a>Ordenación

El orden en el que se agregan los componentes de middleware en el método `Configure` define el orden en el que se invocarán en las solicitudes y el orden inverso de la respuesta. Este orden es esencial por motivos de seguridad, rendimiento y funcionalidad.

El método Configure (que se muestra aquí) agrega los siguientes componentes de middleware:

1. Control de errores y excepciones
2. Servidor de archivos estáticos
3. Autenticación
4. MVC

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)


```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseAuthentication();               // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseIdentity();                     // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

-----------

En el código anterior, `UseExceptionHandler` es el primer componente de middleware que se agrega a la canalización. Por tanto, captura todas las excepciones que se puedan producir en las llamadas posteriores.

El middleware de archivos estáticos se llama al principio de la canalización para que pueda controlar solicitudes y realizar cortocircuitos sin pasar por los componentes restantes. Este middleware **no** proporciona comprobaciones de autorización. Los archivos que proporciona, incluidos los de *wwwroot*, están disponibles de forma pública. Consulte [Archivos estáticos](xref:fundamentals/static-files) para obtener más información sobre cómo proteger este tipo de archivos.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)


Si el middleware de archivos estáticos no controla la solicitud, se pasa al middleware de identidad (`app.UseAuthentication`), que realiza la autenticación. Este middleware no cortocircuita las solicitudes sin autenticación. Aunque autentique solicitudes, la autorización (y el rechazo) se producen después de que MVC seleccione una página de Razor o un control y una acción concretos.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Si el middleware de archivos estáticos no controla la solicitud, se pasa al middleware de identidad (`app.UseIdentity`), que realiza la autenticación. Este middleware no cortocircuita las solicitudes sin autenticación. Aunque autentique solicitudes, la autorización (y el rechazo) se producen después de que MVC seleccione un control y una acción concretos.

-----------

En el ejemplo siguiente se muestra un orden de middleware en el que el middleware de archivos estáticos controla las solicitudes de archivos estáticos antes del middleware de compresión de respuestas. Los archivos estáticos no se comprimen con este orden de middleware. Las respuestas MVC de [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) se pueden comprimir.

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseStaticFiles();         // Static files not compressed
                                  // by middleware.
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

<a name="middleware-run-map-use"></a>

### <a name="use-run-and-map"></a>Use, Run y Map

Puede configurar la canalización HTTP con `Use`, `Run` y `Map`. El método `Use` puede cortocircuitar la canalización (solo si no llama a un delegado de solicitudes `next`). `Run` es una convención y es posible que algunos componentes de middleware expongan métodos `Run[Middleware]` que se ejecutan al final de la canalización.

Las extensiones `Map*` se usan como convenciones para la creación de ramas en la canalización. [Map](/dotnet/api/microsoft.aspnetcore.builder.mapextensions) crea una rama de la canalización de solicitudes según las coincidencias de la ruta de solicitud proporcionada. Si la ruta de solicitud comienza con la ruta proporcionada, se ejecuta la creación de la rama.

[!code-csharp[](index/sample/Chain/StartupMap.cs?name=snippet1)]

En la siguiente tabla se muestran las solicitudes y las respuestas de `http://localhost:1234` con el código anterior:

| Solicitud | Respuesta |
| --- | --- |
| localhost:1234 | Saludos del delegado sin Map.  |
| localhost:1234/map1 | Prueba 1 de Map |
| localhost:1234/map2 | Prueba 2 de Map |
| localhost:1234/map3 | Saludos del delegado sin Map.  |

Cuando se usa `Map`, los segmentos de ruta que coincidan se eliminan de `HttpRequest.Path` y se anexan a `HttpRequest.PathBase` por cada solicitud.

[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) crea una rama de la canalización de solicitudes según el resultado del predicado proporcionado. Se puede usar cualquier predicado de tipo `Func<HttpContext, bool>` para asignar solicitudes a nuevas ramas de la canalización. En el ejemplo siguiente se usa un predicado para detectar la presencia de una `branch` variable de cadena de consulta:

[!code-csharp[](index/sample/Chain/StartupMapWhen.cs?name=snippet1)]

En la siguiente tabla se muestran las solicitudes y las respuestas de `http://localhost:1234` con el código anterior:

| Solicitud | Respuesta |
| --- | --- |
| localhost:1234 | Saludos del delegado sin Map.  |
| localhost:1234/?branch=master | Rama usada = master|

`Map` admite la anidación, por ejemplo:

```csharp
app.Map("/level1", level1App => {
       level1App.Map("/level2a", level2AApp => {
           // "/level1/level2a"
           //...
       });
       level1App.Map("/level2b", level2BApp => {
           // "/level1/level2b"
           //...
       });
   });
   ```

`Map` puede hacer coincidir varios segmentos a la vez, por ejemplo:

 ```csharp
app.Map("/level1/level2", HandleMultiSeg);
```

## <a name="built-in-middleware"></a>Middleware integrado

ASP.NET Core incluye los siguientes componentes de middleware, así como una descripción del orden en el que se deberían agregar:

| Software intermedio | Description | Orden |
| ---------- | ----------- | ----- |
| [Autenticación](xref:security/authentication/identity) | Proporciona compatibilidad con autenticación. | Antes de que se necesite `HttpContext.User`. Terminal para devoluciones de llamadas OAuth. |
| [CORS](xref:security/cors) | Configura el uso compartido de recursos entre orígenes. | Antes de los componentes que usan CORS. |
| [Diagnóstico](xref:fundamentals/error-handling) | Configura el diagnóstico. | Antes de los componentes que generan errores. |
| [ForwardedHeaders/HttpOverrides](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | Reenvía encabezados con proxy a la solicitud actual. | Antes de los componentes que consumen los campos actualizados (ejemplos: Scheme, Host, ClientIP, Method). |
| [Almacenamiento en caché de respuestas](xref:performance/caching/middleware) | Proporciona compatibilidad con la captura de respuestas. | Antes de los componentes que requieren el almacenamiento en caché. |
| [Compresión de respuesta](xref:performance/response-compression) | Proporciona compatibilidad con la compresión de respuestas. | Antes de los componentes que requieren compresión. |
| [RequestLocalization](xref:fundamentals/localization) | Proporciona compatibilidad con ubicación. | Antes de los componentes que dependen de la ubicación. |
| [Enrutamiento](xref:fundamentals/routing) | Define y restringe las rutas de la solicitud. | Terminal para rutas que coincidan. |
| [Sesión](xref:fundamentals/app-state) | Proporciona compatibilidad con la administración de sesiones de usuario. | Antes de los componentes que requieren Session. |
| [Archivos estáticos](xref:fundamentals/static-files) | Proporciona compatibilidad con la proporción de archivos estáticos y la exploración de directorios. | Terminal si hay una solicitud que coincida con archivos. |
| [Reescritura de direcciones URL](xref:fundamentals/url-rewriting) | Proporciona compatibilidad con la reescritura de direcciones URL y la redirección de solicitudes. | Antes de los componentes que consumen la dirección URL. |
| [WebSockets](xref:fundamentals/websockets) | Habilita el protocolo WebSockets. | Antes de los componentes necesarios para aceptar solicitudes de WebSocket. |

<a name="middleware-writing-middleware"></a>

## <a name="writing-middleware"></a>Escritura de middleware

El middleware normalmente está encapsulado en una clase y se expone con un método de extensión. Use el siguiente middleware a modo de ejemplo. En este se establece la referencia cultural de la solicitud actual a partir de la cadena de solicitud:

[!code-csharp[](index/sample/Culture/StartupCulture.cs?name=snippet1)]

Nota: El código de ejemplo anterior se usa para mostrar la creación de un componente de middleware. Vea [Globalization and localization](xref:fundamentals/localization) (Globalización y localización) para ver la compatibilidad con localización integrada de ASP.NET Core.

Puede probar el middleware pasando la referencia cultural, por ejemplo, `http://localhost:7997/?culture=no`.

El código siguiente mueve el delegado de middleware a una clase:

[!code-csharp[](index/sample/Culture/RequestCultureMiddleware.cs)]

> [!NOTE]
> En ASP.NET Core 1.x, el nombre del método del middleware `Task` debe ser `Invoke`. En ASP.NET Core 2.0 o posterior, el nombre puede ser `Invoke` o `InvokeAsync`.

El método de extensión siguiente expone el middleware mediante [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder):

[!code-csharp[](index/sample/Culture/RequestCultureMiddlewareExtensions.cs)]

El código siguiente llama al middleware desde `Configure`:

[!code-csharp[](index/sample/Culture/Startup.cs?name=snippet1&highlight=5)]

El middleware debería seguir el [principio de dependencias explicitas](http://deviq.com/explicit-dependencies-principle/) mediante la exposición de sus dependencias en el constructor. El middleware se construye una vez por *duración de la aplicación*. Vea *Dependencias bajo solicitud* a continuación si necesita compartir servicios con middleware en una solicitud.

Los componentes de middleware pueden resolver las dependencias de una inserción de dependencias mediante parámetros del constructor. [`UseMiddleware<T>`](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) también puede aceptar parámetros adicionales directamente.

### <a name="per-request-dependencies"></a>Dependencias bajo solicitud

Dado que el middleware se construye al inicio de la aplicación y no bajo solicitud, los servicios de duración *con ámbito* que usan los constructores de middleware no se comparten con otros tipos insertados mediante dependencias durante cada solicitud. Si debe compartir un servicio *con ámbito* entre su middleware y otros tipos, agregue esos servicios a la signatura del método `Invoke`. El método `Invoke` puede aceptar parámetros adicionales que propaga la inserción de dependencias. Por ejemplo:

```csharp
public class MyMiddleware
{
    private readonly RequestDelegate _next;

    public MyMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="additional-resources"></a>Recursos adicionales

* [Migración de módulos HTTP a middleware](xref:migration/http-modules)
* [Inicio de aplicaciones](xref:fundamentals/startup)
* [Solicitud de características](xref:fundamentals/request-features)
* [Factory-based middleware activation](xref:fundamentals/middleware/extensibility) (Activación de middleware basada en Factory)
* [Activación de middleware con un contenedor de terceros](xref:fundamentals/middleware/extensibility-third-party-container)
