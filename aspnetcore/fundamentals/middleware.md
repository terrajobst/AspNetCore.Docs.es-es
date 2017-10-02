---
title: "Middleware de núcleo de ASP.NET"
author: rick-anderson
description: "Obtenga información sobre el software intermedio ASP.NET Core y la canalización de solicitudes."
keywords: "Núcleo de ASP.NET, Middleware, canalización, delegado"
ms.author: riande
manager: wpickett
ms.date: 08/14/2017
ms.topic: article
ms.assetid: db9a86ab-46c2-40e0-baed-86e38c16af1f
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware
ms.openlocfilehash: 3cd15c7e8ed4956e1d451f3bd5935fc175999d1f
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2017
---
# <a name="aspnet-core-middleware-fundamentals"></a>Conceptos básicos de Middleware de núcleo de ASP.NET

<a name=fundamentals-middleware></a>

Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Steve Smith](https://ardalis.com/)

[Ver o descargar el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample) ([cómo descargar](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-middleware"></a>¿Qué es middleware

Software intermedio es un software que se monta en una canalización de la aplicación para controlar las solicitudes y respuestas. Cada componente:

* Elige si se debe pasar la solicitud al siguiente componente de la canalización.
* Puede realizar el trabajo antes y después se invoca el componente siguiente en la canalización. 

Los delegados de la solicitud se utilizan para crear la canalización de solicitudes. Los delegados de solicitud controlen cada solicitud HTTP.

Solicitar los delegados se configuran mediante [ejecutar](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [mapa](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions), y [Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) métodos de extensión. Un delegado de solicitud individual puede ser especificado en línea como un método anónimo (denominado middleware en línea) o puede definirse en una clase reutilizable. Estas clases reutilizables y métodos anónimos en línea están *middleware*, o *componentes de middleware*. Cada componente de middleware en la canalización de solicitud es responsable de invocar el siguiente componente de la canalización o evaluación "cortocircuitada" de la cadena si es necesario.

[Migrar módulos HTTP de middleware](../migration/http-modules.md) explica la diferencia entre las canalizaciones de solicitud de ASP.NET Core y las versiones anteriores y proporciona más ejemplos de middleware.

## <a name="creating-a-middleware-pipeline-with-iapplicationbuilder"></a>Crear una canalización de middleware con IApplicationBuilder

La canalización de solicitudes de ASP.NET Core consta de una secuencia de delegados de la solicitud, denominada uno tras otro, como muestra en este diagrama (el subproceso de ejecución sigue las flechas negras):

![Patrón de procesamiento de solicitud que muestra una solicitud que llega, el procesamiento a través de tres middlewares y la respuesta salir de la aplicación. Cada middleware ejecuta su lógica y entrega la solicitud al siguiente middleware en la instrucción next(). Después de que el tercer middleware procesa la solicitud es mano hacia atrás por la dos middlewares anteriores para un procesamiento adicional después de las instrucciones de next() a su vez antes de salir de la aplicación como una respuesta al cliente.](middleware/_static/request-delegate-pipeline.png)

Cada delegado puede realizar operaciones antes y después de la siguiente delegado. También puede decidir que un delegado no pasar una solicitud para el delegado siguiente, que se denomina evaluación "cortocircuitada" de la canalización de solicitudes. Evaluación "cortocircuitada" es a menudo deseable porque evita el trabajo innecesario. Por ejemplo, el middleware de archivos estáticos puede devolver una solicitud de un archivo estático y el resto de la canalización de cortocircuito. Los delegados de control de excepciones deben llamarse al principio de la canalización, por lo que pueden detectar las excepciones que se producen en fases posteriores de la canalización.

La aplicación de ASP.NET Core más simple posible establece un delegado de solicitud único que controla todas las solicitudes. En este caso no incluye una canalización de solicitud real. En su lugar, se llama a una función anónima única en respuesta a cada solicitud HTTP.

[!code-csharp[Main](middleware/sample/Middleware/Startup.cs)]

La primera [aplicación. Ejecutar](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) delegado finaliza la canalización.

Puede encadenar varios delegados de la solicitud junto con [aplicación. Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions). El `next` parámetro representa el delegado siguiente en la canalización. (Recuerde que puede cortocircuita la canalización por *no* al llamar a la *siguiente* parámetro.) Por lo general puede realizar acciones antes y después de la siguiente delegado, como se muestra en este ejemplo:

[!code-csharp[Main](middleware/sample/Chain/Startup.cs?name=snippet1)]

>[!WARNING]
> No llame a `next.Invoke` una vez enviada la respuesta al cliente. Cambia a `HttpResponse` una vez iniciada la respuesta se iniciará una excepción. Por ejemplo, los cambios, como establecer los encabezados, código de estado, etcetera, iniciará una excepción. Escribir en el cuerpo de respuesta después de llamar a `next`:
> - Puede provocar una infracción del protocolo. Por ejemplo, escribir más de los indicados `content-length`.
> - Podría dañarse el formato del cuerpo. Por ejemplo, escribiendo un pie de página HTML en un archivo CSS.
>
> [HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) es una sugerencia útil para indicar si se han enviado los encabezados o el cuerpo se ha escrito en.

## <a name="ordering"></a>Ordenación

El orden en que se agregan los componentes de middleware en la `Configure` método define el orden en el que se invocan en las solicitudes y el orden inverso para la respuesta. Esta ordenación es fundamental para la seguridad, rendimiento y funcionalidad.

El método Configure (se muestra a continuación) agrega los siguientes componentes de software intermedio:

1. Control de excepciones y errores
2. Servidor de archivos estáticos
3. Autenticación
4. MVC

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

En el código anterior, `UseExceptionHandler` es el primer componente de middleware que se agrega a la canalización, por lo tanto, detecta cualquier excepción que se produce en las llamadas posteriores.

El middleware de archivos estáticos se llama al principio de la canalización para que pueda controlar las solicitudes y sin tener que pasar a través de los demás componentes de cortocircuito. Proporciona el middleware de archivos estáticos **sin** comprobaciones de autorización. Los archivos atendido por él, las de incluidas *wwwroot*, estén disponibles públicamente. Vea [trabajar con archivos estáticos](xref:fundamentals/static-files) para obtener un enfoque proteger los archivos estáticos.

Si la solicitud no está controlada por el middleware de archivos estáticos, se pasa el middleware de identidad (`app.UseIdentity`), que realiza la autenticación. Identidad no cortocircuita las solicitudes no autenticadas. Aunque identidad autentica las solicitudes, autorización (y rechazo) se produce después de MVC selecciona una acción y controlador específico.

En el ejemplo siguiente se muestra un middleware de ordenación que se administran las solicitudes de archivos estáticos por el middleware de archivos estáticos antes el middleware de compresión de respuesta. Archivos estáticos no se comprimen con esta ordenación del middleware de. Las respuestas MVC de [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) se pueden comprimir.

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseStaticFiles();         // Static files not compressed
                                  // by middleware.
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

<a name=middleware-run-map-use></a>

### <a name="use-run-and-map"></a>Usar, ejecutar y asignar

Configurar la canalización HTTP con `Use`, `Run`, y `Map`. El `Use` método puede cortocircuito la canalización (es decir, si no llama al método un `next` delegado de la solicitud). `Run`es una convención y pueden exponer algunos componentes de middleware `Run[Middleware]` métodos que se ejecutan al final de la canalización.

`Map*`las extensiones se utilizan como una convención para la bifurcación de la canalización. [Mapa](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) bifurcaciones de la canalización de solicitudes basándose en coincidencias de la ruta de acceso de solicitud dada. Si la ruta de acceso de solicitud empieza con la ruta de acceso especificada, se ejecuta la rama.

[!code-csharp[Main](middleware/sample/Chain/StartupMap.cs?name=snippet1)]

La siguiente tabla muestra las solicitudes y respuestas de `http://localhost:1234` utilizando el código anterior:

| Solicitud | Respuesta |
| --- | --- |
| localhost:1234 | Hola desde el delegado de no asignación.  |
| localhost:1234 / map1 | Prueba 1 de mapa |
| localhost:1234 / map2 | Prueba 2 de mapa |
| localhost:1234 / sitio3 | Hola desde el delegado de no asignación.  |

Cuando `Map` es utilizado, se quitan los segmentos de ruta de acceso coincidentes de `HttpRequest.Path` y agrega a `HttpRequest.PathBase` para cada solicitud.

[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) bifurcaciones de la canalización de solicitudes en función del resultado del predicado proporcionado. Cualquier predicado de tipo `Func<HttpContext, bool>` puede usarse para asignar solicitudes a una nueva bifurcación de la canalización. En el ejemplo siguiente, se utiliza un predicado para detectar la presencia de una variable de cadena de consulta `branch`:

[!code-csharp[Main](middleware/sample/Chain/StartupMapWhen.cs?name=snippet1)]

La siguiente tabla muestra las solicitudes y respuestas de `http://localhost:1234` utilizando el código anterior:

| Solicitud | Respuesta |
| --- | --- |
| localhost:1234 | Hola desde el delegado de no asignación.  |
| localhost:1234 /? bifurcación = master | Bifurcación usa = master|

`Map`admite la anidación, por ejemplo:

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

`Map`puede hacer coincidir varios segmentos a la vez, por ejemplo:

 ```csharp
app.Map("/level1/level2", HandleMultiSeg);
```

## <a name="built-in-middleware"></a>Middleware integrado

ASP.NET Core incluye los siguientes componentes de software intermedio:

| Software intermedio | Descripción |
| ----- | ------- |
| [Autenticación](xref:security/authentication/identity) | Proporciona compatibilidad con la autenticación. |
| [CORS](xref:security/cors) | Define el uso compartido de recursos entre orígenes. |
| [Almacenamiento en caché de respuestas](xref:performance/caching/middleware) | Proporciona compatibilidad para almacenar en caché las respuestas. |
| [Compresión de respuesta](xref:performance/response-compression) | Proporciona compatibilidad para la compresión de las respuestas. |
| [Enrutamiento](xref:fundamentals/routing) | Define y restringe las rutas de la solicitud. |
| [Sesión](xref:fundamentals/app-state) | Proporciona compatibilidad para administrar sesiones de usuario. |
| [Archivos estáticos](xref:fundamentals/static-files) | Proporciona compatibilidad para servir archivos estáticos y examen de directorios. |
| [Middleware de reescritura de dirección URL](xref:fundamentals/url-rewriting) | Proporciona compatibilidad para volver a escribir las direcciones URL y redirigir las solicitudes. |

<a name=middleware-writing-middleware></a>

## <a name="writing-middleware"></a>Middleware de escritura

Middleware en general se encapsulan en una clase y exponen a través de un método de extensión. Tenga en cuenta el siguiente middleware, que establece la referencia cultural de la solicitud actual de la cadena de consulta:

[!code-csharp[Main](middleware/sample/Culture/StartupCulture.cs?name=snippet1)]

Nota: El código de ejemplo anterior se utiliza para mostrar cómo crear un componente de middleware. Vea [ globalización y localización](xref:fundamentals/localization) para la compatibilidad de localización integrado del núcleo de ASP.NET.

Puede probar el middleware pasando en la referencia cultural, por ejemplo `http://localhost:7997/?culture=no`.

El siguiente código mueve el delegado de middleware a una clase:

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddleware.cs)]

El siguiente método de extensión expone el middleware a través de [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddlewareExtensions.cs)]

El código siguiente llama el middleware de `Configure`:

[!code-csharp[Main](middleware/sample/Culture/Startup.cs?name=snippet1&highlight=5)]

Middleware debe seguir la [principio de dependencias explícitas](http://deviq.com/explicit-dependencies-principle/) mediante la exposición de sus dependencias en su constructor. Middleware se construye una vez por *duración de la aplicación*. Vea *dependencias por solicitud* siguiente si se tienen que compartir servicios con middleware dentro de una solicitud.

Componentes de software intermedio pueden resolver sus dependencias de inyección de dependencia a través de los parámetros del constructor. [`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary)También se puede aceptar parámetros adicionales directamente.

### <a name="per-request-dependencies"></a>Dependencias de cada solicitud

Dado que se construye middleware al iniciar la aplicación, no por solicitud, *ámbito* usados por los constructores de middleware de servicios de duración no se comparten con otros tipos de dependencia inyectado durante cada solicitud. Si debe compartir un *ámbito* entre el middleware y otros tipos de servicio, agregue estos servicios para el `Invoke` la firma del método. El `Invoke` método puede aceptar parámetros adicionales que se rellenan con inserción de dependencias. Por ejemplo:

```c#
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

## <a name="resources"></a>Recursos

* [Código de ejemplo utilizado en este documento](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample)
* [Migración de módulos HTTP a middleware](../migration/http-modules.md)
* [Inicio de aplicaciones](startup.md)
* [Solicitud de características](request-features.md)
