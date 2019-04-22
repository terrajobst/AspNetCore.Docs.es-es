---
ms.openlocfilehash: 41021e30ae85dd0ae42cbe6f1606727e21bd7707
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59705528"
---
# <a name="aspnet-core-middleware-extensibility-sample"></a><span data-ttu-id="c6857-101">Ejemplo de extensibilidad de middleware de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c6857-101">ASP.NET Core Middleware Extensibility Sample</span></span>

<span data-ttu-id="c6857-102">En este ejemplo se muestran los escenarios descritos en [Activación de middleware basada en Factory en ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/middleware/middleware-extensibility).</span><span class="sxs-lookup"><span data-stu-id="c6857-102">This sample demonstrates the scenarios described in [Factory-based middleware activation in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/middleware/middleware-extensibility).</span></span>

<span data-ttu-id="c6857-103">La aplicación de ejemplo muestra middleware activado por:</span><span class="sxs-lookup"><span data-stu-id="c6857-103">The sample app demonstrates middleware activated by:</span></span>

* <span data-ttu-id="c6857-104">Convención.</span><span class="sxs-lookup"><span data-stu-id="c6857-104">Convention.</span></span> <span data-ttu-id="c6857-105">Para obtener más información sobre la activación de middleware convencional, consulte el tema [Middleware](https://docs.microsoft.com/aspnet/core/fundamentals/middleware/).</span><span class="sxs-lookup"><span data-stu-id="c6857-105">For more information on conventional middleware activation, see the [Middleware](https://docs.microsoft.com/aspnet/core/fundamentals/middleware/) topic.</span></span>
* <span data-ttu-id="c6857-106">Una implementación de [IMiddleware](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.http.imiddleware).</span><span class="sxs-lookup"><span data-stu-id="c6857-106">An [IMiddleware](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.http.imiddleware) implementation.</span></span> <span data-ttu-id="c6857-107">La clase [IMiddlewareFactory](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) predeterminada activa el middleware.</span><span class="sxs-lookup"><span data-stu-id="c6857-107">The default [IMiddlewareFactory](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) class activates the middleware.</span></span>

<span data-ttu-id="c6857-108">Las implementaciones de middleware funcionan de forma idéntica y registran el valor proporcionado por un parámetro de cadena de consulta (`key`).</span><span class="sxs-lookup"><span data-stu-id="c6857-108">The middleware implementations function identically and record the value provided by a query string parameter (`key`).</span></span> <span data-ttu-id="c6857-109">El middleware usa un contexto de base de datos insertado (un servicio con ámbito) para registrar el valor de cadena de consulta en una base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="c6857-109">The middlewares use an injected database context (a scoped service) to record the query string value in an in-memory database.</span></span>
