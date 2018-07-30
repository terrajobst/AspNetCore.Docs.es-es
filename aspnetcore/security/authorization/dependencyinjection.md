---
title: Inserción de dependencias en controladores de requisitos en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo insertar controladores de requisito de autorización en una aplicación de ASP.NET Core con inserción de dependencias.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/dependencyinjection
ms.openlocfilehash: 71d563e11d308a95c08e6d012d3a071f4697d2de
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342119"
---
# <a name="dependency-injection-in-requirement-handlers-in-aspnet-core"></a><span data-ttu-id="aa98b-103">Inserción de dependencias en controladores de requisitos en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aa98b-103">Dependency injection in requirement handlers in ASP.NET Core</span></span>

<a name="security-authorization-di"></a>

<span data-ttu-id="aa98b-104">[Controladores de autorización deben estar registrados](xref:security/authorization/policies#handler-registration) en la colección de servicios durante la configuración (mediante [inserción de dependencias](xref:fundamentals/dependency-injection)).</span><span class="sxs-lookup"><span data-stu-id="aa98b-104">[Authorization handlers must be registered](xref:security/authorization/policies#handler-registration) in the service collection during configuration (using [dependency injection](xref:fundamentals/dependency-injection)).</span></span>

<span data-ttu-id="aa98b-105">Suponga que tiene un repositorio de reglas que desea evaluar dentro de un controlador de autorización y ese repositorio se registró en la colección de servicios.</span><span class="sxs-lookup"><span data-stu-id="aa98b-105">Suppose you had a repository of rules you wanted to evaluate inside an authorization handler and that repository was registered in the service collection.</span></span> <span data-ttu-id="aa98b-106">La autorización se resuelva e insertar en el constructor.</span><span class="sxs-lookup"><span data-stu-id="aa98b-106">Authorization will resolve and inject that into your constructor.</span></span>

<span data-ttu-id="aa98b-107">Por ejemplo, si deseara usar ASP. NET del registro de infraestructura que desea insertar `ILoggerFactory` en el controlador.</span><span class="sxs-lookup"><span data-stu-id="aa98b-107">For example, if you wanted to use ASP.NET's logging infrastructure you would want to inject `ILoggerFactory` into your handler.</span></span> <span data-ttu-id="aa98b-108">Un controlador de este tipo podría ser similar:</span><span class="sxs-lookup"><span data-stu-id="aa98b-108">Such a handler might look like:</span></span>

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

<span data-ttu-id="aa98b-109">¿Registrar el controlador con `services.AddSingleton()`:</span><span class="sxs-lookup"><span data-stu-id="aa98b-109">You would register the handler with `services.AddSingleton()`:</span></span>

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

<span data-ttu-id="aa98b-110">Una instancia de la voluntad de controlador se crea cuando se inicia la aplicación y se DI inyectar registrado `ILoggerFactory` en su constructor.</span><span class="sxs-lookup"><span data-stu-id="aa98b-110">An instance of the handler will be created when your application starts, and DI will inject the registered `ILoggerFactory` into your constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="aa98b-111">Los controladores que usan Entity Framework no deben estar registrados como singleton.</span><span class="sxs-lookup"><span data-stu-id="aa98b-111">Handlers that use Entity Framework shouldn't be registered as singletons.</span></span>
