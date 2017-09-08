---
title: "Inserción de dependencias en los controladores de requisito"
author: rick-anderson
description: 
keywords: "Núcleo de ASP.NET,"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 5fb6625c-173a-4feb-8380-73c9844dc23c
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/dependencyinjection
ms.openlocfilehash: 37d197d7696a6e91fa236b2defc577959c95c49f
ms.sourcegitcommit: 0a70706a3814d2684f3ff96095d1e8291d559cc7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2017
---
# <a name="dependency-injection-in-requirement-handlers"></a><span data-ttu-id="96395-103">Inserción de dependencias en los controladores de requisito</span><span class="sxs-lookup"><span data-stu-id="96395-103">Dependency Injection in requirement handlers</span></span>

<a name=security-authorization-di></a>

<span data-ttu-id="96395-104">[Controladores de autorización deben estar registrados](policies.md#security-authorization-policies-based-handler-registration) en la colección de servicio durante la configuración (mediante [inyección de dependencia](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection)).</span><span class="sxs-lookup"><span data-stu-id="96395-104">[Authorization handlers must be registered](policies.md#security-authorization-policies-based-handler-registration) in the service collection during configuration (using [dependency injection](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection)).</span></span>

<span data-ttu-id="96395-105">Suponga que tiene un repositorio de reglas que desea evaluar dentro de un controlador de autorización y ese repositorio se ha registrado en la colección de servicio.</span><span class="sxs-lookup"><span data-stu-id="96395-105">Suppose you had a repository of rules you wanted to evaluate inside an authorization handler and that repository was registered in the service collection.</span></span>  <span data-ttu-id="96395-106">La autorización se resuelva e insertar en el constructor.</span><span class="sxs-lookup"><span data-stu-id="96395-106">Authorization will resolve and inject that into your constructor.</span></span>

<span data-ttu-id="96395-107">Por ejemplo, si desea utilizar ASP. NET del registro de infraestructura que desea insertar `ILoggerFactory` en el controlador.</span><span class="sxs-lookup"><span data-stu-id="96395-107">For example, if you wanted to use ASP.NET's logging infrastructure you would want to inject `ILoggerFactory` into your handler.</span></span> <span data-ttu-id="96395-108">Un controlador de este tipo podría ser similar:</span><span class="sxs-lookup"><span data-stu-id="96395-108">Such a handler might look like:</span></span>

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

<span data-ttu-id="96395-109">Debería registrar el controlador con `services.AddSingleton()`:</span><span class="sxs-lookup"><span data-stu-id="96395-109">You would register the handler with `services.AddSingleton()`:</span></span>

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
   ```

<span data-ttu-id="96395-110">Una instancia de la voluntad de controlador se crea cuando se inicia la aplicación y se DI inyectar registrado `ILoggerFactory` en su constructor.</span><span class="sxs-lookup"><span data-stu-id="96395-110">An instance of the handler will be created when your application starts, and DI will inject the registered `ILoggerFactory` into your constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="96395-111">Los controladores que usan Entity Framework no deben registrarse como singleton.</span><span class="sxs-lookup"><span data-stu-id="96395-111">Handlers that use Entity Framework shouldn't be registered as singletons.</span></span>
