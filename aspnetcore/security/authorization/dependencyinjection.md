---
title: "Inserción de dependencias en los controladores de requisito"
author: rick-anderson
description: "Este documento describe cómo insertar controladores de requisito de autorización en una aplicación de ASP.NET Core mediante la inserción de dependencias."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/dependencyinjection
ms.openlocfilehash: 1b7506b49109264a8c628ea2e39ded9f5ace95d3
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="dependency-injection-in-requirement-handlers"></a>Inserción de dependencias en los controladores de requisito

<a name="security-authorization-di"></a>

[Controladores de autorización deben estar registrados](policies.md#handler-registration) en la colección de servicio durante la configuración (mediante [inyección de dependencia](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection)).

Suponga que tiene un repositorio de reglas que desea evaluar dentro de un controlador de autorización y ese repositorio se ha registrado en la colección de servicio. La autorización se resuelva e insertar en el constructor.

Por ejemplo, si desea utilizar ASP. NET del registro de infraestructura que desea insertar `ILoggerFactory` en el controlador. Un controlador de este tipo podría ser similar:

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

Debería registrar el controlador con `services.AddSingleton()`:

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

Una instancia de la voluntad de controlador se crea cuando se inicia la aplicación y se DI inyectar registrado `ILoggerFactory` en su constructor.

> [!NOTE]
> Los controladores que usan Entity Framework no deben registrarse como singleton.
