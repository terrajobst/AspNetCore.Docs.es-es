---
title: Activación de middleware con un contenedor de terceros en ASP.NET Core
author: guardrex
description: Aprenda a usar middleware fuertemente tipado con la implementación de una activación basada en Factory y un contenedor de terceros en ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/03/2019
uid: fundamentals/middleware/extensibility-third-party-container
ms.openlocfilehash: 4bc99b4c336aba611287c9fbe03d4252f8abee5b
ms.sourcegitcommit: f6e6730872a7d6f039f97d1df762f0d0bd5e34cf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/04/2019
ms.locfileid: "67561651"
---
# <a name="middleware-activation-with-a-third-party-container-in-aspnet-core"></a><span data-ttu-id="270c3-103">Activación de middleware con un contenedor de terceros en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="270c3-103">Middleware activation with a third-party container in ASP.NET Core</span></span>

<span data-ttu-id="270c3-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="270c3-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="270c3-105">En este artículo se explica cómo usar <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> y <xref:Microsoft.AspNetCore.Http.IMiddleware> como un punto de extensibilidad para la activación de [middleware](xref:fundamentals/middleware/index) con un contenedor de terceros.</span><span class="sxs-lookup"><span data-stu-id="270c3-105">This article demonstrates how to use <xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> and <xref:Microsoft.AspNetCore.Http.IMiddleware> as an extensibility point for [middleware](xref:fundamentals/middleware/index) activation with a third-party container.</span></span> <span data-ttu-id="270c3-106">Para información de introducción sobre `IMiddlewareFactory` y `IMiddleware`, consulte <xref:fundamentals/middleware/extensibility>.</span><span class="sxs-lookup"><span data-stu-id="270c3-106">For introductory information on `IMiddlewareFactory` and `IMiddleware`, see <xref:fundamentals/middleware/extensibility>.</span></span>

<span data-ttu-id="270c3-107">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility-third-party-container/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="270c3-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/middleware/extensibility-third-party-container/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="270c3-108">En la aplicación de ejemplo se muestra una activación de middleware por medio de una implementación de `IMiddlewareFactory`, `SimpleInjectorMiddlewareFactory`.</span><span class="sxs-lookup"><span data-stu-id="270c3-108">The sample app demonstrates middleware activation by an `IMiddlewareFactory` implementation, `SimpleInjectorMiddlewareFactory`.</span></span> <span data-ttu-id="270c3-109">En el ejemplo se usa el contenedor de inserción de dependencias [Simple Injector](https://simpleinjector.org).</span><span class="sxs-lookup"><span data-stu-id="270c3-109">The sample uses the [Simple Injector](https://simpleinjector.org) dependency injection (DI) container.</span></span>

<span data-ttu-id="270c3-110">La implementación de middleware del ejemplo registra el valor proporcionado por un parámetro de cadena de consulta (`key`).</span><span class="sxs-lookup"><span data-stu-id="270c3-110">The sample's middleware implementation records the value provided by a query string parameter (`key`).</span></span> <span data-ttu-id="270c3-111">El middleware usa un contexto de base de datos insertado (un servicio con ámbito) para registrar el valor de cadena de consulta en una base de datos en memoria.</span><span class="sxs-lookup"><span data-stu-id="270c3-111">The middleware uses an injected database context (a scoped service) to record the query string value in an in-memory database.</span></span>

> [!NOTE]
> <span data-ttu-id="270c3-112">En la aplicación de ejemplo se usa [Simple Injector](https://github.com/simpleinjector/SimpleInjector) única y exclusivamente con fines de demostración.</span><span class="sxs-lookup"><span data-stu-id="270c3-112">The sample app uses [Simple Injector](https://github.com/simpleinjector/SimpleInjector) purely for demonstration purposes.</span></span> <span data-ttu-id="270c3-113">El uso de Simple Injector no está avalado.</span><span class="sxs-lookup"><span data-stu-id="270c3-113">Use of Simple Injector isn't an endorsement.</span></span> <span data-ttu-id="270c3-114">Los métodos de activación de middleware descritos en la documentación de Simple Injector y los problemas de GitHub está recomendado por los responsables de Simple Injector.</span><span class="sxs-lookup"><span data-stu-id="270c3-114">Middleware activation approaches described in the Simple Injector documentation and GitHub issues are recommended by the maintainers of Simple Injector.</span></span> <span data-ttu-id="270c3-115">Para más información, vea la [documentación de Simple Injector](https://simpleinjector.readthedocs.io/en/latest/index.html) y el [repositorio de GitHub de Simple Injector](https://github.com/simpleinjector/SimpleInjector).</span><span class="sxs-lookup"><span data-stu-id="270c3-115">For more information, see the [Simple Injector documentation](https://simpleinjector.readthedocs.io/en/latest/index.html) and [Simple Injector GitHub repository](https://github.com/simpleinjector/SimpleInjector).</span></span>

## <a name="imiddlewarefactory"></a><span data-ttu-id="270c3-116">IMiddlewareFactory</span><span class="sxs-lookup"><span data-stu-id="270c3-116">IMiddlewareFactory</span></span>

<span data-ttu-id="270c3-117"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> proporciona métodos para crear middleware.</span><span class="sxs-lookup"><span data-stu-id="270c3-117"><xref:Microsoft.AspNetCore.Http.IMiddlewareFactory> provides methods to create middleware.</span></span>

<span data-ttu-id="270c3-118">En la aplicación de ejemplo, se implementa un Middleware Factory para crear una instancia de `SimpleInjectorActivatedMiddleware`.</span><span class="sxs-lookup"><span data-stu-id="270c3-118">In the sample app, a middleware factory is implemented to create an `SimpleInjectorActivatedMiddleware` instance.</span></span> <span data-ttu-id="270c3-119">Ese Middleware Factory usa el contenedor de Simple Injector para resolver el middleware:</span><span class="sxs-lookup"><span data-stu-id="270c3-119">The middleware factory uses the Simple Injector container to resolve the middleware:</span></span>

[!code-csharp[](extensibility-third-party-container/samples/2.x/SampleApp/Middleware/SimpleInjectorMiddlewareFactory.cs?name=snippet1&highlight=5-8,12)]

## <a name="imiddleware"></a><span data-ttu-id="270c3-120">IMiddleware</span><span class="sxs-lookup"><span data-stu-id="270c3-120">IMiddleware</span></span>

<span data-ttu-id="270c3-121"><xref:Microsoft.AspNetCore.Http.IMiddleware> define el middleware para la canalización de solicitudes de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="270c3-121"><xref:Microsoft.AspNetCore.Http.IMiddleware> defines middleware for the app's request pipeline.</span></span>

<span data-ttu-id="270c3-122">Middleware activado por una implementación de `IMiddlewareFactory` (*Middleware/SimpleInjectorActivatedMiddleware.cs*):</span><span class="sxs-lookup"><span data-stu-id="270c3-122">Middleware activated by an `IMiddlewareFactory` implementation (*Middleware/SimpleInjectorActivatedMiddleware.cs*):</span></span>

[!code-csharp[](extensibility-third-party-container/samples/2.x/SampleApp/Middleware/SimpleInjectorActivatedMiddleware.cs?name=snippet1)]

<span data-ttu-id="270c3-123">Se crea una extensión para el middleware (*Middleware/MiddlewareExtensions.cs*):</span><span class="sxs-lookup"><span data-stu-id="270c3-123">An extension is created for the middleware (*Middleware/MiddlewareExtensions.cs*):</span></span>

[!code-csharp[](extensibility-third-party-container/samples/2.x/SampleApp/Middleware/MiddlewareExtensions.cs?name=snippet1)]

<span data-ttu-id="270c3-124">`Startup.ConfigureServices` debe realizar varias tareas:</span><span class="sxs-lookup"><span data-stu-id="270c3-124">`Startup.ConfigureServices` must perform several tasks:</span></span>

* <span data-ttu-id="270c3-125">Configurar el contenedor de Simple Injector.</span><span class="sxs-lookup"><span data-stu-id="270c3-125">Set up the Simple Injector container.</span></span>
* <span data-ttu-id="270c3-126">Registrar tanto el Factory como el Middleware.</span><span class="sxs-lookup"><span data-stu-id="270c3-126">Register the factory and middleware.</span></span>
* <span data-ttu-id="270c3-127">Poner disponible el contexto de base de datos de la aplicación en el contenedor de Simple Injector.</span><span class="sxs-lookup"><span data-stu-id="270c3-127">Make the app's database context available from the Simple Injector container.</span></span>

[!code-csharp[](extensibility-third-party-container/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="270c3-128">El middleware se registra en la canalización de procesamiento de solicitudes en `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="270c3-128">The middleware is registered in the request processing pipeline in `Startup.Configure`:</span></span>

[!code-csharp[](extensibility-third-party-container/samples/2.x/SampleApp/Startup.cs?name=snippet2&highlight=13)]

## <a name="additional-resources"></a><span data-ttu-id="270c3-129">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="270c3-129">Additional resources</span></span>

* [<span data-ttu-id="270c3-130">Middleware</span><span class="sxs-lookup"><span data-stu-id="270c3-130">Middleware</span></span>](xref:fundamentals/middleware/index)
* <span data-ttu-id="270c3-131">[Factory-based middleware activation](xref:fundamentals/middleware/extensibility) (Activación de middleware basada en Factory)</span><span class="sxs-lookup"><span data-stu-id="270c3-131">[Factory-based middleware activation](xref:fundamentals/middleware/extensibility)</span></span>
* [<span data-ttu-id="270c3-132">Repositorio de GitHub de Simple Injector</span><span class="sxs-lookup"><span data-stu-id="270c3-132">Simple Injector GitHub repository</span></span>](https://github.com/simpleinjector/SimpleInjector)
* [<span data-ttu-id="270c3-133">Documentación de Simple Injector</span><span class="sxs-lookup"><span data-stu-id="270c3-133">Simple Injector documentation</span></span>](https://simpleinjector.readthedocs.io/en/latest/index.html)
