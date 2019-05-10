---
title: Inserción de dependencias en controladores de requisitos en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo insertar controladores de requisito de autorización en una aplicación de ASP.NET Core con inserción de dependencias.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/dependencyinjection
ms.openlocfilehash: 71d563e11d308a95c08e6d012d3a071f4697d2de
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896372"
---
# <a name="dependency-injection-in-requirement-handlers-in-aspnet-core"></a>Inserción de dependencias en controladores de requisitos en ASP.NET Core

<a name="security-authorization-di"></a>

[Controladores de autorización deben estar registrados](xref:security/authorization/policies#handler-registration) en la colección de servicios durante la configuración (mediante [inserción de dependencias](xref:fundamentals/dependency-injection)).

Suponga que tiene un repositorio de reglas que desea evaluar dentro de un controlador de autorización y ese repositorio se registró en la colección de servicios. La autorización se resuelva e insertar en el constructor.

Por ejemplo, si deseara usar ASP. NET del registro de infraestructura que desea insertar `ILoggerFactory` en el controlador. Un controlador de este tipo podría ser similar:

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

¿Registrar el controlador con `services.AddSingleton()`:

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

Una instancia de la voluntad de controlador se crea cuando se inicia la aplicación y se DI inyectar registrado `ILoggerFactory` en su constructor.

> [!NOTE]
> Los controladores que usan Entity Framework no deben estar registrados como singleton.
