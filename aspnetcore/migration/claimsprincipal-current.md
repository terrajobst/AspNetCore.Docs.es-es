---
title: Migrar desde ClaimsPrincipal. Current
author: mjrousos
description: Obtenga información sobre cómo migrar de ClaimsPrincipal. Current para recuperar la identidad del usuario autenticado actual y las notificaciones en ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 03/26/2019
uid: migration/claimsprincipal-current
ms.openlocfilehash: f7472f5b851d3869da3d26b881e276ce4ca004fb
ms.sourcegitcommit: 231780c8d7848943e5e9fd55e93f437f7e5a371d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2019
ms.locfileid: "74115974"
---
# <a name="migrate-from-claimsprincipalcurrent"></a>Migrar desde ClaimsPrincipal. Current

En los proyectos de ASP.NET 4. x, era habitual usar [ClaimsPrincipal. Current](/dotnet/api/system.security.claims.claimsprincipal.current) para recuperar la identidad y las notificaciones del usuario autenticado actual. En ASP.NET Core, esta propiedad ya no se establece. El código que depende de él debe actualizarse para obtener la identidad del usuario autenticado actual a través de un medio diferente.

## <a name="context-specific-data-instead-of-static-data"></a>Datos específicos del contexto en lugar de datos estáticos

Cuando se usa ASP.NET Core, no se establecen los valores de `ClaimsPrincipal.Current` y `Thread.CurrentPrincipal`. Estas propiedades representan el estado estático, que normalmente evita ASP.NET Core. En su lugar, la arquitectura de ASP.NET Core es recuperar las dependencias (por ejemplo, la identidad del usuario actual) de las colecciones de servicios específicas del contexto (mediante el modelo de [inserción de dependencias](xref:fundamentals/dependency-injection) ). Además, `Thread.CurrentPrincipal` es un subproceso estático, por lo que puede que no conserve los cambios en algunos escenarios asincrónicos (y `ClaimsPrincipal.Current` simplemente llama a `Thread.CurrentPrincipal` de forma predeterminada).

Para comprender los tipos de problemas que los miembros estáticos de subproceso pueden provocar en escenarios asincrónicos, considere el siguiente fragmento de código:

```csharp
// Create a ClaimsPrincipal and set Thread.CurrentPrincipal
var identity = new ClaimsIdentity();
identity.AddClaim(new Claim(ClaimTypes.Name, "User1"));
Thread.CurrentPrincipal = new ClaimsPrincipal(identity);

// Check the current user
Console.WriteLine($"Current user: {Thread.CurrentPrincipal?.Identity.Name}");

// For the method to complete asynchronously
await Task.Yield();

// Check the current user after
Console.WriteLine($"Current user: {Thread.CurrentPrincipal?.Identity.Name}");
```

En el código de ejemplo anterior se establece `Thread.CurrentPrincipal` y se comprueba su valor antes y después de esperar una llamada asincrónica. `Thread.CurrentPrincipal` es específico del *subproceso* en el que se establece y es probable que el método reanude la ejecución en un subproceso diferente después de la espera. Por consiguiente, `Thread.CurrentPrincipal` está presente cuando se activa por primera vez pero es NULL después de la llamada a `await Task.Yield()`.

Obtener la identidad del usuario actual de la colección de servicios de DI de la aplicación también es más fácil de probar, ya que las identidades de prueba se pueden insertar fácilmente.

## <a name="retrieve-the-current-user-in-an-aspnet-core-app"></a>Recuperar el usuario actual en una aplicación ASP.NET Core

Hay varias opciones para recuperar el `ClaimsPrincipal` del usuario autenticado actual en ASP.NET Core en lugar de `ClaimsPrincipal.Current`:

* **ControllerBase. usuario**. Los controladores MVC pueden tener acceso al usuario autenticado actual con su propiedad [User](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.user) .
* **HttpContext. User**. Los componentes con acceso al `HttpContext` actual (por ejemplo, middleware) pueden obtener los `ClaimsPrincipal` del usuario actual de [HttpContext. User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user).
* **Pasado del llamador**. A menudo se llama a las bibliotecas sin acceso a la `HttpContext` actual desde los controladores o componentes de middleware, y puede hacer que la identidad del usuario actual se pase como argumento.
* **IHttpContextAccessor**. El proyecto que se va a migrar a ASP.NET Core puede ser demasiado grande para pasar fácilmente la identidad del usuario actual a todas las ubicaciones necesarias. En tales casos, [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) se puede usar como solución alternativa. `IHttpContextAccessor` puede tener acceso al `HttpContext` actual (si existe). Si se usa DI, vea <xref:fundamentals/httpcontext>. Una solución a corto plazo para obtener la identidad del usuario actual en el código que todavía no se ha actualizado para funcionar con la arquitectura basada en DI de ASP.NET Core sería:

  * Haga que `IHttpContextAccessor` esté disponible en el contenedor de DI llamando a [AddHttpContextAccessor](https://github.com/aspnet/Hosting/issues/793) en `Startup.ConfigureServices`.
  * Obtiene una instancia de `IHttpContextAccessor` durante el inicio y la almacena en una variable estática. La instancia de se pone a disposición del código que antes recuperaba el usuario actual de una propiedad estática.
  * Recupere el `ClaimsPrincipal` del usuario actual mediante `HttpContextAccessor.HttpContext?.User`. Si este código se usa fuera del contexto de una solicitud HTTP, el `HttpContext` es NULL.

La última opción, el uso de una instancia de `IHttpContextAccessor` almacenada en una variable estática, es contraria a los principios ASP.NET Core (la preferencia de dependencias insertadas a dependencias estáticas). En su lugar, planee recuperar `IHttpContextAccessor` instancias de la inserción de dependencias. Sin embargo, una aplicación auxiliar estática puede ser un puente útil a la hora de migrar aplicaciones de ASP.NET grandes existentes que usaban anteriormente `ClaimsPrincipal.Current`.
