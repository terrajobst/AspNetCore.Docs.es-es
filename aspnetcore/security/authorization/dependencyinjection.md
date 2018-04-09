---
title: Inserción de dependencias en los controladores de requisito en ASP.NET Core
author: rick-anderson
description: Obtenga información acerca de cómo insertar controladores de requisito de autorización en una aplicación de ASP.NET Core mediante la inserción de dependencias.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/dependencyinjection
ms.openlocfilehash: 4de7f0e49ade459968f8c30fbad76ce96a65815f
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2018
---
# <a name="dependency-injection-in-requirement-handlers-in-aspnet-core"></a>Inserción de dependencias en los controladores de requisito en ASP.NET Core

<a name="security-authorization-di"></a>

[Controladores de autorización deben estar registrados](xref:security/authorization/policies#handler-registration) en la colección de servicio durante la configuración (mediante [inyección de dependencia](xref:fundamentals/dependency-injection#fundamentals-dependency-injection)).

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
