---
title: Métodos de filtrado de páginas de Razor en ASP.NET Core
author: Rick-Anderson
description: Obtenga información sobre cómo crear métodos de filtrado de páginas de Razor en ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 04/05/2018
uid: razor-pages/filter
ms.openlocfilehash: 32613d75d966a698c6478234f3f5f9d5fc0628bc
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58264799"
---
# <a name="filter-methods-for-razor-pages-in-aspnet-core"></a><span data-ttu-id="bc964-103">Métodos de filtrado de páginas de Razor en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bc964-103">Filter methods for Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="bc964-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bc964-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="bc964-105">Los filtros de páginas de Razor [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) e [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) permiten que las páginas de Razor ejecuten código antes y después de que se haya ejecutado un controlador de páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="bc964-105">Razor Page filters [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) and [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) allow Razor Pages to run code before and after a Razor Page handler is run.</span></span> <span data-ttu-id="bc964-106">Estos filtros son similares a los [filtros de acción de ASP.NET Core MVC](xref:mvc/controllers/filters#action-filters), salvo por el hecho de que no se pueden usar con métodos de controlador de páginas individuales.</span><span class="sxs-lookup"><span data-stu-id="bc964-106">Razor Page filters are similar to [ASP.NET Core MVC action filters](xref:mvc/controllers/filters#action-filters), except they can't be applied to individual page handler methods.</span></span> 

<span data-ttu-id="bc964-107">Los filtros de páginas de Razor:</span><span class="sxs-lookup"><span data-stu-id="bc964-107">Razor Page filters:</span></span>

* <span data-ttu-id="bc964-108">Ejecutan código después de que se haya seleccionado un método de controlador, pero antes de que el enlace de modelos tenga lugar.</span><span class="sxs-lookup"><span data-stu-id="bc964-108">Run code after a handler method has been selected, but before model binding occurs.</span></span>
* <span data-ttu-id="bc964-109">Ejecutan código antes de que se ejecute el método de controlador, después de que el enlace de modelos se haya completado.</span><span class="sxs-lookup"><span data-stu-id="bc964-109">Run code before the handler method executes, after model binding is complete.</span></span>
* <span data-ttu-id="bc964-110">Ejecutan código después de que se haya ejecutado el método de controlador.</span><span class="sxs-lookup"><span data-stu-id="bc964-110">Run code after the handler method executes.</span></span>
* <span data-ttu-id="bc964-111">Se pueden implementar en una página o globalmente.</span><span class="sxs-lookup"><span data-stu-id="bc964-111">Can be implemented on a page or globally.</span></span>
* <span data-ttu-id="bc964-112">No se pueden usar con métodos de controlador de páginas específicas.</span><span class="sxs-lookup"><span data-stu-id="bc964-112">Cannot be applied to specific page handler methods.</span></span>

<span data-ttu-id="bc964-113">Se puede ejecutar código antes de que un método de controlador se ejecute por medio del constructor de página o de middleware, pero solo los filtros de páginas de Razor tienen acceso a [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_HttpContext).</span><span class="sxs-lookup"><span data-stu-id="bc964-113">Code can be run before a handler method executes using the page constructor or middleware, but only Razor Page filters have access to [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_HttpContext).</span></span> <span data-ttu-id="bc964-114">Los filtros tienen un parámetro derivado [FilterContext](/dotnet/api/microsoft.aspnetcore.mvc.filters.filtercontext?view=aspnetcore-2.0) que concede acceso a `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="bc964-114">Filters have a [FilterContext](/dotnet/api/microsoft.aspnetcore.mvc.filters.filtercontext?view=aspnetcore-2.0) derived parameter, which provides access to `HttpContext`.</span></span> <span data-ttu-id="bc964-115">Por ejemplo, en el ejemplo [Implementar un atributo de filtro](#ifa) se agrega un encabezado a la respuesta, cosa que no es posible con constructores o con middleware.</span><span class="sxs-lookup"><span data-stu-id="bc964-115">For example, the [Implement a filter attribute](#ifa) sample adds a header to the response, something that can't be done with constructors or middleware.</span></span>

<span data-ttu-id="bc964-116">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/filter/sample/PageFilter) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="bc964-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/filter/sample/PageFilter) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="bc964-117">Los filtros de páginas de Razor proporcionan los siguientes métodos, que se pueden usar globalmente o bien en el nivel de página:</span><span class="sxs-lookup"><span data-stu-id="bc964-117">Razor Page filters provide the following methods, which can be applied globally or at the page level:</span></span>

* <span data-ttu-id="bc964-118">Métodos sincrónicos:</span><span class="sxs-lookup"><span data-stu-id="bc964-118">Synchronous methods:</span></span>

  * <span data-ttu-id="bc964-119">[OnPageHandlerSelected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0) : Se llama después de que se ha seleccionado un método de controlador, pero antes de modelo se produce el enlace.</span><span class="sxs-lookup"><span data-stu-id="bc964-119">[OnPageHandlerSelected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0) : Called after a handler method has been selected, but before model binding occurs.</span></span>
  * <span data-ttu-id="bc964-120">[OnPageHandlerExecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0) : Se llama antes de que se ejecuta el método de controlador, una vez completado el enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="bc964-120">[OnPageHandlerExecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0) : Called before the handler method executes, after model binding is complete.</span></span>
  * <span data-ttu-id="bc964-121">[OnPageHandlerExecuted](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0) : Llamado cuando se ejecuta el método de controlador, antes de resultado de la acción.</span><span class="sxs-lookup"><span data-stu-id="bc964-121">[OnPageHandlerExecuted](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0) : Called after the handler method executes, before the action result.</span></span>

* <span data-ttu-id="bc964-122">Métodos asincrónicos:</span><span class="sxs-lookup"><span data-stu-id="bc964-122">Asynchronous methods:</span></span>

  * <span data-ttu-id="bc964-123">[OnPageHandlerSelectionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0) : Llamar de forma asincrónica una vez que se ha seleccionado el método de controlador, pero antes de que se produce el enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="bc964-123">[OnPageHandlerSelectionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0) : Called asynchronously after the handler method has been selected, but before model binding occurs.</span></span>
  * <span data-ttu-id="bc964-124">[OnPageHandlerExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0) : Se llama asincrónicamente antes de invoca el método de controlador, una vez completado el enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="bc964-124">[OnPageHandlerExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0) : Called asynchronously before the handler method is invoked, after model binding is complete.</span></span>

> [!NOTE]
> <span data-ttu-id="bc964-125">Implemente la versión sincrónica **o** la versión asincrónica de una interfaz de filtro, pero no ambas.</span><span class="sxs-lookup"><span data-stu-id="bc964-125">Implement **either** the synchronous or the async version of a filter interface, not both.</span></span> <span data-ttu-id="bc964-126">El marco comprueba primero si el filtro implementa la interfaz asincrónica y, si es así, es a la interfaz que llama.</span><span class="sxs-lookup"><span data-stu-id="bc964-126">The framework checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="bc964-127">De lo contrario, llamará a métodos de interfaz sincrónicos.</span><span class="sxs-lookup"><span data-stu-id="bc964-127">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="bc964-128">Si se implementan ambas interfaces, solo se llamará a los métodos asincrónicos.</span><span class="sxs-lookup"><span data-stu-id="bc964-128">If both interfaces are implemented, only the async methods are be called.</span></span> <span data-ttu-id="bc964-129">La misma regla se cumple con las invalidaciones en páginas: implemente la versión sincrónica o asincrónica de la invalidación, pero no ambas.</span><span class="sxs-lookup"><span data-stu-id="bc964-129">The same rule applies to overrides in pages, implement the synchronous or the async version of the override, not both.</span></span>

## <a name="implement-razor-page-filters-globally"></a><span data-ttu-id="bc964-130">Implementar filtros de páginas de Razor globalmente</span><span class="sxs-lookup"><span data-stu-id="bc964-130">Implement Razor Page filters globally</span></span>

<span data-ttu-id="bc964-131">El siguiente código implementa `IAsyncPageFilter`:</span><span class="sxs-lookup"><span data-stu-id="bc964-131">The following code implements `IAsyncPageFilter`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Filters/SampleAsyncPageFilter.cs?name=snippet1)]

<span data-ttu-id="bc964-132">En el código anterior, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger?view=aspnetcore-2.0) no es necesario;</span><span class="sxs-lookup"><span data-stu-id="bc964-132">In the preceding code, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger?view=aspnetcore-2.0) is not required.</span></span> <span data-ttu-id="bc964-133">se usa para proporcionar información de seguimiento relativa a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bc964-133">It's used in the sample to provide trace information for the application.</span></span>

<span data-ttu-id="bc964-134">El siguiente código habilita `SampleAsyncPageFilter` en la clase `Startup`:</span><span class="sxs-lookup"><span data-stu-id="bc964-134">The following code enables the `SampleAsyncPageFilter` in the `Startup` class:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet2&highlight=11)]

<span data-ttu-id="bc964-135">El siguiente código muestra la clase `Startup` completa:</span><span class="sxs-lookup"><span data-stu-id="bc964-135">The following code shows the complete `Startup` class:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet1)]

<span data-ttu-id="bc964-136">El siguiente código llama a `AddFolderApplicationModelConvention` para aplicar `SampleAsyncPageFilter` solo a las páginas en */subFolder*:</span><span class="sxs-lookup"><span data-stu-id="bc964-136">The following code calls `AddFolderApplicationModelConvention` to apply the `SampleAsyncPageFilter` to only pages in */subFolder*:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Startup2.cs?name=snippet2)]

<span data-ttu-id="bc964-137">El siguiente código implementa el método sincrónico `IPageFilter`:</span><span class="sxs-lookup"><span data-stu-id="bc964-137">The following code implements the synchronous `IPageFilter`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Filters/SamplePageFilter.cs?name=snippet1)]

<span data-ttu-id="bc964-138">El siguiente código habilita `SamplePageFilter`:</span><span class="sxs-lookup"><span data-stu-id="bc964-138">The following code enables the `SamplePageFilter`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/StartupSync.cs?name=snippet2&highlight=11)]

::: moniker range=">= aspnetcore-2.1"

## <a name="implement-razor-page-filters-by-overriding-filter-methods"></a><span data-ttu-id="bc964-139">Implementar filtros de páginas de Razor invalidando métodos de filtro</span><span class="sxs-lookup"><span data-stu-id="bc964-139">Implement Razor Page filters by overriding filter methods</span></span>

<span data-ttu-id="bc964-140">El siguiente código invalida los filtros de páginas de Razor sincrónicos:</span><span class="sxs-lookup"><span data-stu-id="bc964-140">The following code overrides the synchronous Razor Page filters:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Pages/Index.cshtml.cs)]

::: moniker-end

<a name="ifa"></a>

## <a name="implement-a-filter-attribute"></a><span data-ttu-id="bc964-141">Implementar un atributo de filtro</span><span class="sxs-lookup"><span data-stu-id="bc964-141">Implement a filter attribute</span></span>

<span data-ttu-id="bc964-142">El filtro basado en atributos integrado [OnResultExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncresultfilter.onresultexecutionasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Filters_IAsyncResultFilter_OnResultExecutionAsync_Microsoft_AspNetCore_Mvc_Filters_ResultExecutingContext_Microsoft_AspNetCore_Mvc_Filters_ResultExecutionDelegate_) puede ser una subclase.</span><span class="sxs-lookup"><span data-stu-id="bc964-142">The built-in attribute-based filter [OnResultExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncresultfilter.onresultexecutionasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Filters_IAsyncResultFilter_OnResultExecutionAsync_Microsoft_AspNetCore_Mvc_Filters_ResultExecutingContext_Microsoft_AspNetCore_Mvc_Filters_ResultExecutionDelegate_) filter can be subclassed.</span></span> <span data-ttu-id="bc964-143">El siguiente filtro agrega un encabezado a la respuesta:</span><span class="sxs-lookup"><span data-stu-id="bc964-143">The following filter adds a header to the response:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Filters/AddHeaderAttribute.cs)]

<span data-ttu-id="bc964-144">El siguiente código se aplica al atributo `AddHeader`:</span><span class="sxs-lookup"><span data-stu-id="bc964-144">The following code applies the `AddHeader` attribute:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Pages/Contact.cshtml.cs?name=snippet1)]

<span data-ttu-id="bc964-145">Vea [Invalidación del orden predeterminado](xref:mvc/controllers/filters#overriding-the-default-order) para obtener instrucciones sobre cómo invalidar el orden.</span><span class="sxs-lookup"><span data-stu-id="bc964-145">See [Overriding the default order](xref:mvc/controllers/filters#overriding-the-default-order) for instructions on overriding the order.</span></span>

<span data-ttu-id="bc964-146">Vea [Cancelación y cortocircuito](xref:mvc/controllers/filters#cancellation-and-short-circuiting) para obtener instrucciones sobre cómo cortocircuitar la canalización de filtro de un filtro.</span><span class="sxs-lookup"><span data-stu-id="bc964-146">See [Cancellation and short circuiting](xref:mvc/controllers/filters#cancellation-and-short-circuiting) for instructions to short-circuit the filter pipeline from a filter.</span></span> 

<a name="auth"></a>

## <a name="authorize-filter-attribute"></a><span data-ttu-id="bc964-147">Atributo de filtro Authorize</span><span class="sxs-lookup"><span data-stu-id="bc964-147">Authorize filter attribute</span></span>

<span data-ttu-id="bc964-148">El atributo [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) se puede aplicar a un `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="bc964-148">The [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) attribute can be applied to a `PageModel`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Pages/ModelWithAuthFilter.cshtml.cs?highlight=7)]
