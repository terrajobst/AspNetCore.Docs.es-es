---
title: Migrar desde ClaimsPrincipal.Current
author: mjrousos
description: Obtenga información acerca de cómo migrar sin ClaimsPrincipal.Current para recuperar notificaciones en ASP.NET Core y la identidad del usuario autenticado actual.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 05/04/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/claimsprincipal-current
ms.openlocfilehash: ea43d17e76380baf57cd9debbc508e8812cfa4a6
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2018
---
# <a name="migrate-from-claimsprincipalcurrent"></a>Migrar desde ClaimsPrincipal.Current

En los proyectos ASP.NET, era habitual usar [ClaimsPrincipal.Current](/dotnet/api/system.security.claims.claimsprincipal.current) recuperar la actual autenticado notificaciones y la identidad del usuario. En ASP.NET Core, ya no se establece esta propiedad. El código que se según lo debe actualizarse para obtener la identidad del usuario autenticado actual a través de un medio diferente.

## <a name="context-specific-data-instead-of-static-data"></a>Datos específicos del contexto en lugar de datos estáticos

Cuando se usa ASP.NET Core, los valores de ambos `ClaimsPrincipal.Current` y `Thread.CurrentPrincipal` no están establecidos. Estas propiedades representan el estado estático, que suele evitar el núcleo de ASP.NET. En su lugar, arquitectura de ASP.NET Core consiste en recuperar las dependencias (como la identidad del usuario actual) de las colecciones de servicio específicas del contexto (mediante su [inyección de dependencia](xref:fundamentals/dependency-injection) modelo (DI)). ¿Qué es más, `Thread.CurrentPrincipal` es subproceso estático, por lo que no pueden conservar cambios en algunos escenarios asincrónicos (y `ClaimsPrincipal.Current` simplemente llama `Thread.CurrentPrincipal` de forma predeterminada).

Para entender a los tipos de subprocesos de problemas pueden provocar que los miembros estáticos en escenarios asincrónicos, considere el siguiente fragmento de código:

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

El código de ejemplo anterior se establece `Thread.CurrentPrincipal` y comprueba su valor antes y después esperar una llamada asincrónica. `Thread.CurrentPrincipal` es específico de la *subproceso* en el que se establece y el método es apropiado reanudar la ejecución en un subproceso diferente después de la instrucción await. Por lo tanto, `Thread.CurrentPrincipal` está presente cuando se comprueba en primer lugar, pero es null después de llamar a `await Task.Yield()`.

Obtener la identidad del usuario actual de recopilación de la aplicación de servicio DI es más comprobable, demasiado, ya que las identidades de prueba se pueden insertar fácilmente.

## <a name="retrieve-the-current-user-in-an-aspnet-core-app"></a>Recuperar el usuario actual en una aplicación de ASP.NET Core

Existen varias opciones para la recuperación del usuario autenticado actual `ClaimsPrincipal` en ASP.NET Core en lugar de `ClaimsPrincipal.Current`:

* **ControllerBase.User**. Controladores MVC pueden tener acceso el usuario autenticado actual con sus [usuario](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.user) propiedad.
* **HttpContext.User**. Componentes con acceso a la actual `HttpContext` (software intermedio, por ejemplo) puede obtener el usuario actual `ClaimsPrincipal` de [HttpContext.User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user).
* **Pasa del autor de llamada**. Las bibliotecas no tienen acceso a la actual `HttpContext` a menudo se denominan de controladores o componentes de middleware y puede tener la identidad del usuario actual que se pasa como argumento.
* **IHttpContextAccessor**. El proyecto ASP.NET que se está migrando a ASP.NET Core puede ser demasiado grande para pasar fácilmente la identidad del usuario actual para todas las ubicaciones necesarias. En tales casos, [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) puede utilizarse para solucionar este problema. `IHttpContextAccessor` puede tener acceso a la actual `HttpContext` (si existe). Una solución a corto plazo para obtener la identidad del usuario actual en el código que no se ha actualizado para trabajar con la arquitectura orientada a DI de ASP.NET Core sería:

  * Asegúrese de `IHttpContextAccessor` disponibles en el contenedor de DI mediante una llamada a [AddHttpContextAccessor](https://github.com/aspnet/Hosting/issues/793) en `Startup.ConfigureServices`.
  * Obtener una instancia de `IHttpContextAccessor` durante el inicio y almacenarlo en una variable estática. La instancia se hace disponible para código que anteriormente estaba recuperando el usuario actual de una propiedad estática.
  * Recuperar el usuario actual `ClaimsPrincipal` con `HttpContextAccessor.HttpContext?.User`. Si este código se utiliza fuera del contexto de una solicitud HTTP, el `HttpContext` es null.

La última opción, mediante `IHttpContextAccessor`, es contraria a principios de ASP.NET Core (por lo que prefieren dependencias insertadas dependencias estáticas). Previsto eliminar eventualmente la dependencia en el método estático `IHttpContextAccessor` auxiliar. Puede ser un puente útil, sin embargo, al migrar grandes aplicaciones ASP.NET existentes que anteriormente estaban usando `ClaimsPrincipal.Current`.
