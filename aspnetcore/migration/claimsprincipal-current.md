---
title: Migrar de ClaimsPrincipal.Current
author: mjrousos
description: Obtenga información sobre cómo migrar sin ClaimsPrincipal.Current para recuperar notificaciones en ASP.NET Core y la identidad del usuario autenticado actual.
ms.author: scaddie
ms.custom: mvc
ms.date: 03/26/2019
uid: migration/claimsprincipal-current
ms.openlocfilehash: 526cc3cf3a58a656e2a1b162cfaccacc7694dc51
ms.sourcegitcommit: 687ffb15ebe65379f75c84739ea851d5a0d788b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2019
ms.locfileid: "58488647"
---
# <a name="migrate-from-claimsprincipalcurrent"></a>Migrar de ClaimsPrincipal.Current

En proyectos ASP.NET 4.x, era habitual usar [ClaimsPrincipal.Current](/dotnet/api/system.security.claims.claimsprincipal.current) recuperar actual autenticado notificaciones y la identidad del usuario. En ASP.NET Core, ya no se establece esta propiedad. Código que dependía de la debe actualizarse para obtener la identidad del usuario autenticado actual mediante un medio diferente.

## <a name="context-specific-data-instead-of-static-data"></a>Datos específicos del contexto en lugar de datos estáticos

Cuando se usa ASP.NET Core, los valores de `ClaimsPrincipal.Current` y `Thread.CurrentPrincipal` no están establecidos. Estas propiedades representan el estado estático, lo que generalmente evita ASP.NET Core. En su lugar, arquitectura de ASP.NET Core es recuperar las dependencias (como la identidad del usuario actual) de las colecciones específicas del contexto de servicio (mediante su [inserción de dependencias](xref:fundamentals/dependency-injection) modelo (DI)). ¿Qué es más, `Thread.CurrentPrincipal` es subproceso estático, por lo que no pueden conservar los cambios en algunos escenarios asincrónicos (y `ClaimsPrincipal.Current` simplemente llama a `Thread.CurrentPrincipal` de forma predeterminada).

Para entender a los tipos de subprocesos de problemas pueden provocar que los miembros estáticos en escenarios asincrónicos, observe el siguiente fragmento de código:

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

El código de ejemplo anterior establece `Thread.CurrentPrincipal` y comprueba su valor antes y después de esperar una llamada asincrónica. `Thread.CurrentPrincipal` es específico de la *subproceso* en el que se ha configurado y el método es apropiado reanudar la ejecución en un subproceso diferente después del await. Por lo tanto, `Thread.CurrentPrincipal` está presente cuando se comprueba en primer lugar, pero es null tras la llamada a `await Task.Yield()`.

Obtener la identidad del usuario actual de recopilación de la aplicación de servicio de inserción de dependencias es más estable, demasiado, puesto que se pueden insertar fácilmente las identidades de la prueba.

## <a name="retrieve-the-current-user-in-an-aspnet-core-app"></a>Recuperar el usuario actual en una aplicación ASP.NET Core

Hay varias opciones para la recuperación del usuario autenticado actual `ClaimsPrincipal` en ASP.NET Core en lugar de `ClaimsPrincipal.Current`:

* **ControllerBase.User**. Los controladores MVC pueden tener acceso al usuario autenticado actual con sus [usuario](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.user) propiedad.
* **HttpContext.User**. Los componentes con acceso a la actual `HttpContext` (middleware, por ejemplo) puede obtener el usuario actual `ClaimsPrincipal` desde [HttpContext.User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user).
* **Pasa del autor de llamada**. Bibliotecas sin acceso a la actual `HttpContext` a menudo se llaman desde los controladores o componentes de middleware y puede tener la identidad del usuario actual se pasa como un argumento.
* **IHttpContextAccessor**. El proyecto que se está migrando a ASP.NET Core puede ser demasiado grande para pasar fácilmente la identidad del usuario actual para todas las ubicaciones necesarias. En tales casos, [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) puede usarse como solución alternativa. `IHttpContextAccessor` puede tener acceso a la actual `HttpContext` (si existe). Sería una solución a corto plazo para obtener la identidad del usuario actual en el código que aún no se ha actualizado para trabajar con la arquitectura controlada por DI de ASP.NET Core:

  * Asegúrese de `IHttpContextAccessor` disponibles en el contenedor de inserción de dependencias mediante una llamada a [AddHttpContextAccessor](https://github.com/aspnet/Hosting/issues/793) en `Startup.ConfigureServices`.
  * Obtener una instancia de `IHttpContextAccessor` durante el inicio y almacenarlo en una variable estática. La instancia está disponible para código que anteriormente estaba recuperando el usuario actual de una propiedad estática.
  * Recuperar el usuario actual `ClaimsPrincipal` mediante `HttpContextAccessor.HttpContext?.User`. Si este código se usa fuera del contexto de una solicitud HTTP, el `HttpContext` es null.

La última opción, mediante un `IHttpContextAccessor` instancia almacenada en una variable estática, es la contraria a principios de ASP.NET Core (prefiere sobre dependencias insertadas dependencia estática). Vaya a recuperar finalmente `IHttpContextAccessor` instancias de la inyección de dependencia en su lugar. Una aplicación auxiliar estática puede ser un puente útil, sin embargo, al migrar grandes aplicaciones ASP.NET existentes que estaban usando anteriormente `ClaimsPrincipal.Current`.
