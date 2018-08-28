---
title: Middleware de ASP.NET Core
author: rick-anderson
description: Obtenga información sobre el middleware de ASP.NET Core y la canalización de solicitudes.
ms.author: riande
ms.custom: mvc
ms.date: 08/21/2018
uid: fundamentals/middleware/index
ms.openlocfilehash: e6dc76b7cb80e0dfda102df5aefb5d9ce9b821ed
ms.sourcegitcommit: 847cc1de5526ff42a7303491e6336c2dbdb45de4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "43055811"
---
# <a name="aspnet-core-middleware"></a>Middleware de ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Steve Smith](https://ardalis.com/)

El software intermedio es un software que se ensambla en una canalización de una aplicación para controlar las solicitudes y las respuestas. Cada componente puede hacer lo siguiente:

* Elegir si se pasa la solicitud al siguiente componente de la canalización.
* Realizar trabajos antes y después de invocar al siguiente componente de la canalización.

Los delegados de solicitudes se usan para crear la canalización de solicitudes. Estos también controlan las solicitudes HTTP.

Los delegados de solicitudes se configuran con los métodos de extensión <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> y <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>. Un delegado de solicitudes se puede especificar en línea como un método anónimo (denominado middleware en línea) o se puede definir en una clase reutilizable. Estas clases reutilizables y métodos anónimos en línea se conocen como *software intermedio* o *componentes de software intermedio*. Cada componente de software intermedio de la canalización de solicitudes es responsable de invocar el siguiente componente de la canalización o de cortocircuitar la canalización, en caso de ser necesario.

En <xref:migration/http-modules> se explica la diferencia entre las canalizaciones de solicitudes en ASP.NET Core y ASP.NET 4.x y se proporcionan más ejemplos de software intermedio.

## <a name="create-a-middleware-pipeline-with-iapplicationbuilder"></a>Creación de una canalización de software intermedio con IApplicationBuilder

La canalización de solicitudes de ASP.NET Core consiste en una secuencia de delegados de solicitud a los que se llama de uno en uno. En el siguiente diagrama se muestra este concepto. El subproceso de ejecución sigue las flechas negras.

![En el patrón de procesamiento de solicitudes se muestra una solicitud entrante, el procesamiento a través de tres softwares intermedios y la respuesta saliente de la aplicación. Cada middleware ejecuta su lógica y pasa la solicitud al siguiente middleware con la instrucción next(). Después de que el tercer software intermedio procese la solicitud, esta vuelve a pasar por los dos softwares intermedios anteriores en orden inverso. Esto se hace a modo de procesamiento adicional después de las instrucciones next() y antes de salir de la aplicación como respuesta al cliente.](index/_static/request-delegate-pipeline.png)

Cada delegado puede realizar operaciones antes y después del siguiente. También puede decidir no pasar una solicitud al siguiente delegado, lo que se denomina *cortocircuitar la canalización de solicitudes*. Este proceso es necesario muchas veces, ya que previene la realización de trabajo innecesario. Por ejemplo, el software intermedio de archivos estáticos puede devolver una solicitud para un archivo estático y cortocircuitar el resto de la canalización. Los delegados que controlan excepciones se llaman al principio de la canalización para que puedan capturar las excepciones que se produzcan en las fases siguientes de la canalización.

La aplicación ASP.NET Core más sencilla posible configura un solo delegado de solicitudes que controla todas las solicitudes. En este caso no se incluye una canalización de solicitudes real. En su lugar, solo se llama a una única función anónima en respuesta a todas las solicitudes HTTP.

[!code-csharp[](index/snapshot/Middleware/Startup.cs?name=snippet1)]

El primer delegado de <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> finaliza la canalización.

Encadene varios delegados de solicitudes con <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>. El parámetro `next` representa el siguiente delegado de la canalización. Si *no* llama al *siguiente* parámetro, puede cortocircuitar la canalización. Normalmente, puede realizar acciones antes y después del siguiente delegado, tal como se muestra en el ejemplo siguiente:

[!code-csharp[](index/snapshot/Chain/Startup.cs?name=snippet1)]

> [!WARNING]
> No llame a `next.Invoke` después de haber enviado la respuesta al cliente. Si se modifica <xref:Microsoft.AspNetCore.Http.HttpResponse> después de haber iniciado la respuesta, se producirá una excepción. Por ejemplo, se producirá una excepción al realizar cambios tales como el establecimiento de encabezados o el código. Si escribe en el cuerpo de la respuesta después de llamar a `next`:
>
> * Puede provocar una infracción del protocolo. Por ejemplo, si escribe más de la longitud `Content-Length` establecida.
> * Puede dañar el formato del cuerpo. Por ejemplo, si escribe un pie de página en HTML en un archivo CSS.
>
> <xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> es una sugerencia útil para indicar si se han enviado los encabezados o se han realizado escrituras en el cuerpo.

## <a name="order"></a>Orden

El orden en el que se agregan los componentes de software intermedio en el método `Startup.Configure` define el orden en el que se invocarán los componentes de software intermedio en las solicitudes y el orden inverso de la respuesta. Por motivos de seguridad, rendimiento y funcionalidad, el orden es básico.

El método `Configure` siguiente agrega los siguientes componentes de software intermedio:

1. Control de errores y excepciones
2. Servidor de archivos estáticos
3. Autenticación
4. MVC

::: moniker range=">= aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    if (env.IsDevelopment())
    {
        // When the app runs in the Development environment:
        //   Use the Developer Exception Page to report app runtime errors.
        //   Use the Database Error Page to report database runtime errors.
        app.UseDeveloperExceptionPage();
        app.UseDatabaseErrorPage();
    }
    else
    {
        // When the app doesn't run in the Development environment:
        //   Enable the Exception Handler Middleware to catch exceptions
        //     thrown in the following middlewares.
        //   Use the HTTP Strict Transport Security Protocol (HSTS)
        //     Middleware.
        app.UseExceptionHandler("/Error");
        app.UseHsts();
    }

    // Use HTTPS Redirection Middleware to redirect HTTP requests to HTTPS.
    app.UseHttpsRedirection();

    // Return static files and end the pipeline.
    app.UseStaticFiles();

    // Use Cookie Policy Middleware to conform to EU General Data 
    //   Protection Regulation (GDPR) regulations.
    app.UseCookiePolicy();

    // Authenticate before the user accesses secure resources.
    app.UseAuthentication();

    // Add MVC to the request pipeline.
    app.UseMvc();
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    // Enable the Exception Handler Middleware to catch exceptions
    //   thrown in the following middlewares.
    app.UseExceptionHandler("/Home/Error");

    // Return static files and end the pipeline.
    app.UseStaticFiles();

    // Authenticate before you access secure resources.
    app.UseIdentity();

    // Add MVC to the request pipeline.
    app.UseMvcWithDefaultRoute();
}
```

::: moniker-end

En el código de ejemplo anterior, cada método de extensión de software intermedio se expone en <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> a través del espacio de nombres de <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName>.

<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> es el primer componente de software intermedio que se agrega a la canalización. Por lo tanto, el software intermedio del controlador de excepciones detectará las excepciones que se produzcan en las llamadas posteriores.

El software intermedio de archivos estáticos se llama al principio de la canalización para que pueda controlar solicitudes y realizar cortocircuitos sin pasar por los componentes restantes. Este software intermedio **no** proporciona comprobaciones de autorización. Los archivos que proporciona, incluidos los de *wwwroot*, están disponibles de forma pública. Para obtener más información sobre cómo proteger este tipo de archivos, vea <xref:fundamentals/static-files>.

::: moniker range=">= aspnetcore-2.0"

Si el software intermedio de archivos estáticos no controla la solicitud, se pasará al software intermedio de autenticación (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), que realizará la autenticación. Este software intermedio no cortocircuita las solicitudes sin autenticación. Aunque autentique solicitudes, la autorización (y también el rechazo) se producirán después de que MVC seleccione una página de Razor o un control y una acción de MVC concretos.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Si el software intermedio de archivos estáticos no controla la solicitud, se pasará al software intermedio de identidad (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>), que realizará la autenticación. Este middleware no cortocircuita las solicitudes sin autenticación. Aunque autentique solicitudes, la autorización (y también el rechazo) se producirán después de que MVC seleccione un control y una acción concretos.

::: moniker-end

En el ejemplo siguiente se muestra un orden de software intermedio en el que el software intermedio de archivos estáticos controla las solicitudes de archivos estáticos antes del software intermedio de compresión de respuestas. Los archivos estáticos no se comprimen en este orden de software intermedio. Las respuestas de MVC de <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> se pueden comprimir.

```csharp
public void Configure(IApplicationBuilder app)
{
    // Static files not compressed by Static Files Middleware.
    app.UseStaticFiles();
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

### <a name="use-run-and-map"></a>Use, Run y Map

Puede configurar la canalización HTTP con `Use`, `Run` y `Map`. El método `Use` puede cortocircuitar la canalización (solo si no llama a un delegado de solicitudes `next`). `Run` es una convención y es posible que algunos componentes de middleware expongan métodos `Run[Middleware]` que se ejecutan al final de la canalización.

Las extensiones <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> se usan como convenciones para la creación de ramas en la canalización. `Map*` crea una rama de la canalización de solicitudes según las coincidencias de la ruta de solicitud proporcionada. Si la ruta de solicitud comienza con la ruta proporcionada, se ejecuta la creación de la rama.

[!code-csharp[](index/snapshot/Chain/StartupMap.cs?name=snippet1)]

En la siguiente tabla se muestran las solicitudes y las respuestas de `http://localhost:1234` con el código anterior.

| Solicitud             | Respuesta                     |
| ------------------- | ---------------------------- |
| localhost:1234      | Saludos del delegado sin Map. |
| localhost:1234/map1 | Prueba 1 de Map                   |
| localhost:1234/map2 | Prueba 2 de Map                   |
| localhost:1234/map3 | Saludos del delegado sin Map. |

Cuando se usa `Map`, los segmentos de ruta que coincidan se eliminan de `HttpRequest.Path` y se anexan a `HttpRequest.PathBase` por cada solicitud.

[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) crea una rama de la canalización de solicitudes según el resultado del predicado proporcionado. Se puede usar cualquier predicado de tipo `Func<HttpContext, bool>` para asignar solicitudes a nuevas ramas de la canalización. En el ejemplo siguiente se usa un predicado para detectar la presencia de una `branch` variable de cadena de consulta:

[!code-csharp[](index/snapshot/Chain/StartupMapWhen.cs?name=snippet1)]

En la siguiente tabla se muestran las solicitudes y las respuestas de `http://localhost:1234` con el código anterior.

| Solicitud                       | Respuesta                     |
| ----------------------------- | ---------------------------- |
| localhost:1234                | Saludos del delegado sin Map. |
| localhost:1234/?branch=master | Rama usada = master         |

`Map` admite la anidación, por ejemplo:

```csharp
app.Map("/level1", level1App => {
    level1App.Map("/level2a", level2AApp => {
        // "/level1/level2a" processing
    });
    level1App.Map("/level2b", level2BApp => {
        // "/level1/level2b" processing
    });
});
   ```

`Map` puede hacer coincidir varios segmentos a la vez:

[!code-csharp[](index/snapshot/Chain/StartupMultiSeg.cs?name=snippet1&highlight=13)]

## <a name="built-in-middleware"></a>Middleware integrado

ASP.NET Core incluye los componentes de software intermedio siguientes. En la columna *Orden* se proporcionan notas sobre la ubicación del software intermedio en la solicitud de canalización, así como las condiciones con las que podría finalizar la solicitud y evitar que otro software intermedio procese una solicitud.

| Software intermedio | Descripción | Orden |
| ---------- | ----------- | ----- |
| [Autenticación](xref:security/authentication/identity) | Proporciona compatibilidad con autenticación. | Antes de que se necesite `HttpContext.User`. Terminal para devoluciones de llamadas OAuth. |
| [CORS](xref:security/cors) | Configura el uso compartido de recursos entre orígenes. | Antes de los componentes que usan CORS. |
| [Diagnóstico](xref:fundamentals/error-handling) | Configura el diagnóstico. | Antes de los componentes que generan errores. |
| [Encabezados reenviados](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | Reenvía encabezados con proxy a la solicitud actual. | Antes de los componentes que consumen los campos actualizados (ejemplos: esquema, host, cliente, IP y método). |
| [Invalidación del método HTTP](/dotnet/api/microsoft.aspnetcore.builder.httpmethodoverrideextensions) | Permite que una solicitud POST entrante invalide el método. | Antes de los componentes que consumen el método actualizado. |
| [Redireccionamiento de HTTPS](xref:security/enforcing-ssl#require-https) | Redireccione todas las solicitudes HTTP a HTTPS (ASP.NET Core 2.1 o posterior). | Antes de los componentes que consumen la dirección URL. |
| [Seguridad de transporte estricta de HTTP (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | Middleware de mejora de seguridad que agrega un encabezado de respuesta especial (ASP.NET Core 2.1 o posterior). | Antes de que se envíen las respuestas y después de los componentes que modifican las solicitudes (por ejemplo, encabezados reenviados, reescritura de URL). |
| [MVC](xref:mvc/overview) | Procesa la solicitudes con MVC o Razor Pages (ASP.NET Core 2.0 o versiones posteriores). | Si hay una solicitud que coincida con una ruta, será final. |
| [OWIN](xref:fundamentals/owin) | Puede interoperar con aplicaciones, servidores y software intermedio basados en OWIN. | Si el software intermedio de OWIN procesa completamente la solicitud, será final. |
| [Almacenamiento en caché de respuestas](xref:performance/caching/middleware) | Proporciona compatibilidad con la captura de respuestas. | Antes de los componentes que requieren el almacenamiento en caché. |
| [Compresión de respuesta](xref:performance/response-compression) | Proporciona compatibilidad con la compresión de respuestas. | Antes de los componentes que requieren compresión. |
| [Localización de solicitudes](xref:fundamentals/localization) | Proporciona compatibilidad con ubicación. | Antes de los componentes que dependen de la ubicación. |
| [Enrutamiento](xref:fundamentals/routing) | Define y restringe las rutas de la solicitud. | Terminal para rutas que coincidan. |
| [Sesión](xref:fundamentals/app-state) | Proporciona compatibilidad con la administración de sesiones de usuario. | Antes de los componentes que requieren Session. |
| [Archivos estáticos](xref:fundamentals/static-files) | Proporciona compatibilidad con la proporción de archivos estáticos y la exploración de directorios. | Si hay una solicitud que coincida con un archivo, será final. |
| [Reescritura de direcciones URL](xref:fundamentals/url-rewriting) | Proporciona compatibilidad con la reescritura de direcciones URL y la redirección de solicitudes. | Antes de los componentes que consumen la dirección URL. |
| [WebSockets](xref:fundamentals/websockets) | Habilita el protocolo WebSockets. | Antes de los componentes necesarios para aceptar solicitudes de WebSocket. |

## <a name="write-middleware"></a>Escritura de software intermedio

El middleware normalmente está encapsulado en una clase y se expone con un método de extensión. Use el siguiente software intermedio a modo de ejemplo. En este se establece la referencia cultural de la solicitud actual a partir de la cadena de solicitud:

[!code-csharp[](index/snapshot/Culture/StartupCulture.cs?name=snippet1)]

El código de ejemplo anterior se usa para mostrar la creación de un componente de software intermedio. Para obtener más información sobre la compatibilidad con la localización integrada de ASP.NET Core, vea <xref:fundamentals/localization>.

Puede probar el middleware pasando la referencia cultural, por ejemplo, `http://localhost:7997/?culture=no`.

El código siguiente mueve el delegado de middleware a una clase:

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddleware.cs)]

::: moniker range="< aspnetcore-2.0"

El nombre del software intermedio del método `Task` debe ser `Invoke`. En ASP.NET Core 2.0 o posterior, el nombre puede ser `Invoke` o `InvokeAsync`.

::: moniker-end

El método de extensión siguiente expone el software intermedio mediante <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddlewareExtensions.cs)]

El código siguiente llama al middleware desde `Startup.Configure`:

[!code-csharp[](index/snapshot/Culture/Startup.cs?name=snippet1&highlight=5)]

El middleware debería seguir el [principio de dependencias explicitas](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) mediante la exposición de sus dependencias en el constructor. El middleware se construye una vez por *duración de la aplicación*. Si necesita compartir servicios con software intermedio en una solicitud, vea la sección [Dependencias bajo solicitud](#per-request-dependencies).

Los componentes de software intermedio pueden resolver sus dependencias de una [inserción de dependencias (DI)](xref:fundamentals/dependency-injection) mediante parámetros del constructor. [UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) también puede aceptar parámetros adicionales directamente.

### <a name="per-request-dependencies"></a>Dependencias bajo solicitud

Dado que el software intermedio se construye al inicio de la aplicación y no bajo solicitud, los servicios de duración *con ámbito* que usan los constructores de software intermedio no se comparten con otros tipos insertados mediante dependencias durante cada solicitud. Si debe compartir un servicio *con ámbito* entre su middleware y otros tipos, agregue esos servicios a la signatura del método `Invoke`. El método `Invoke` puede aceptar parámetros adicionales que la inserción de dependencias propaga:

```csharp
public class CustomMiddleware
{
    private readonly RequestDelegate _next;

    public CustomMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    // IMyScopedService is injected into Invoke
    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="additional-resources"></a>Recursos adicionales

* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>
