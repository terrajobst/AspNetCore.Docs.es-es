---
title: Inserción de dependencias en controladores de requisitos en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo insertar controladores de requisitos de autorización en una aplicación ASP.NET Core mediante la inserción de dependencias.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/dependencyinjection
ms.openlocfilehash: 71d563e11d308a95c08e6d012d3a071f4697d2de
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78654365"
---
# <a name="dependency-injection-in-requirement-handlers-in-aspnet-core"></a>Inserción de dependencias en controladores de requisitos en ASP.NET Core

<a name="security-authorization-di"></a>

Los [controladores de autorización deben registrarse](xref:security/authorization/policies#handler-registration) en la colección de servicios durante la configuración (mediante la [inserción de dependencias](xref:fundamentals/dependency-injection)).

Supongamos que tiene un repositorio de reglas que desea evaluar dentro de un controlador de autorización y que ese repositorio se registró en la colección de servicios. La autorización se resolverá y se insertará en el constructor.

Por ejemplo, si desea utilizar ASP. La infraestructura de registro de la red que desea insertar `ILoggerFactory` en el controlador. Este tipo de controlador podría ser similar al siguiente:

```csharp
public class LoggingAuthorizationHandler : AuthorizationHandler<MyRequirement>
   {
       ILogger _logger;

       public LoggingAuthorizationHandler(ILoggerFactory loggerFactory)
       {
           _logger = loggerFactory.CreateLogger(this.GetType().FullName);
       }

       protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, MyRequirement requirement)
       {
           _logger.LogInformation("Inside my handler");
           // Check if the requirement is fulfilled.
           return Task.CompletedTask;
       }
   }
   ```

Registraría el controlador con `services.AddSingleton()`:

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

Cuando se inicia la aplicación, se creará una instancia del controlador y DI insertará el `ILoggerFactory` registrado en el constructor.

> [!NOTE]
> Los controladores que usan Entity Framework no deben registrarse como singletons.
