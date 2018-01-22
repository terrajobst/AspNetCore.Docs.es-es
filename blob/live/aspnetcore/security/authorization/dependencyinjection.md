---
title: "Inserción de dependencias en los controladores de requisito"
author: rick-anderson
description: "Este documento describe cómo insertar controladores de requisito de autorización en una aplicación de ASP.NET Core mediante la inserción de dependencias."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/dependencyinjection
ms.openlocfilehash: 9aaa356fe67a7e2c8177ffa1b886ec6b3dc13ef0
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="dependency-injection-in-requirement-handlers"></a><span data-ttu-id="456f3-103">Inserción de dependencias en los controladores de requisito</span><span class="sxs-lookup"><span data-stu-id="456f3-103">Dependency Injection in requirement handlers</span></span>

<a name="security-authorization-di"></a>

<span data-ttu-id="456f3-104">[Controladores de autorización deben estar registrados](policies.md#handler-registration) en la colección de servicio durante la configuración (mediante [inyección de dependencia](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection)).</span><span class="sxs-lookup"><span data-stu-id="456f3-104">[Authorization handlers must be registered](policies.md#handler-registration) in the service collection during configuration (using [dependency injection](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection)).</span></span>

<span data-ttu-id="456f3-105">Suponga que tiene un repositorio de reglas que desea evaluar dentro de un controlador de autorización y ese repositorio se ha registrado en la colección de servicio.</span><span class="sxs-lookup"><span data-stu-id="456f3-105">Suppose you had a repository of rules you wanted to evaluate inside an authorization handler and that repository was registered in the service collection.</span></span> <span data-ttu-id="456f3-106">La autorización se resuelva e insertar en el constructor.</span><span class="sxs-lookup"><span data-stu-id="456f3-106">Authorization will resolve and inject that into your constructor.</span></span>

<span data-ttu-id="456f3-107">Por ejemplo, si desea utilizar ASP. NET del registro de infraestructura que desea insertar `ILoggerFactory` en el controlador.</span><span class="sxs-lookup"><span data-stu-id="456f3-107">For example, if you wanted to use ASP.NET's logging infrastructure you would want to inject `ILoggerFactory` into your handler.</span></span> <span data-ttu-id="456f3-108">Un controlador de este tipo podría ser similar:</span><span class="sxs-lookup"><span data-stu-id="456f3-108">Such a handler might look like:</span></span>

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

<span data-ttu-id="456f3-109">Debería registrar el controlador con `services.AddSingleton()`:</span><span class="sxs-lookup"><span data-stu-id="456f3-109">You would register the handler with `services.AddSingleton()`:</span></span>

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

<span data-ttu-id="456f3-110">Una instancia de la voluntad de controlador se crea cuando se inicia la aplicación y se DI inyectar registrado `ILoggerFactory` en su constructor.</span><span class="sxs-lookup"><span data-stu-id="456f3-110">An instance of the handler will be created when your application starts, and DI will inject the registered `ILoggerFactory` into your constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="456f3-111">Los controladores que usan Entity Framework no deben registrarse como singleton.</span><span class="sxs-lookup"><span data-stu-id="456f3-111">Handlers that use Entity Framework shouldn't be registered as singletons.</span></span>
