---
title: Métodos de filtrado de páginas de Razor en ASP.NET Core
author: Rick-Anderson
description: Obtenga información sobre cómo crear métodos de filtrado de páginas de Razor en ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 2/18/2020
uid: razor-pages/filter
ms.openlocfilehash: a60b17685c6f836de7c0afcc5b89a9894fb8b28f
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77447236"
---
# <a name="filter-methods-for-razor-pages-in-aspnet-core"></a><span data-ttu-id="fcc50-103">Métodos de filtrado de páginas de Razor en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fcc50-103">Filter methods for Razor Pages in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="fcc50-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fcc50-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fcc50-105">Los filtros de páginas de Razor [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) e [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) permiten que las páginas de Razor ejecuten código antes y después de que se haya ejecutado un controlador de páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="fcc50-105">Razor Page filters [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) and [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) allow Razor Pages to run code before and after a Razor Page handler is run.</span></span> <span data-ttu-id="fcc50-106">Estos filtros son similares a los [filtros de acción de ASP.NET Core MVC](xref:mvc/controllers/filters#action-filters), salvo por el hecho de que no se pueden usar con métodos de controlador de páginas individuales.</span><span class="sxs-lookup"><span data-stu-id="fcc50-106">Razor Page filters are similar to [ASP.NET Core MVC action filters](xref:mvc/controllers/filters#action-filters), except they can't be applied to individual page handler methods.</span></span>

<span data-ttu-id="fcc50-107">Los filtros de páginas de Razor:</span><span class="sxs-lookup"><span data-stu-id="fcc50-107">Razor Page filters:</span></span>

* <span data-ttu-id="fcc50-108">Ejecutan código después de que se haya seleccionado un método de controlador, pero antes de que el enlace de modelos tenga lugar.</span><span class="sxs-lookup"><span data-stu-id="fcc50-108">Run code after a handler method has been selected, but before model binding occurs.</span></span>
* <span data-ttu-id="fcc50-109">Ejecutan código antes de que se ejecute el método de controlador, después de que el enlace de modelos se haya completado.</span><span class="sxs-lookup"><span data-stu-id="fcc50-109">Run code before the handler method executes, after model binding is complete.</span></span>
* <span data-ttu-id="fcc50-110">Ejecutan código después de que se haya ejecutado el método de controlador.</span><span class="sxs-lookup"><span data-stu-id="fcc50-110">Run code after the handler method executes.</span></span>
* <span data-ttu-id="fcc50-111">Se pueden implementar en una página o globalmente.</span><span class="sxs-lookup"><span data-stu-id="fcc50-111">Can be implemented on a page or globally.</span></span>
* <span data-ttu-id="fcc50-112">No se pueden usar con métodos de controlador de páginas específicas.</span><span class="sxs-lookup"><span data-stu-id="fcc50-112">Cannot be applied to specific page handler methods.</span></span>
* <span data-ttu-id="fcc50-113">Puede tener dependencias de constructor rellenadas por la [inserción de dependencias](xref:fundamentals/dependency-injection) (di).</span><span class="sxs-lookup"><span data-stu-id="fcc50-113">Can have constructor dependencies populated by [Dependency Injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="fcc50-114">Para obtener más información, vea [ServiceFilterAttribute](/aspnet/core/mvc/controllers/filters#servicefilterattribute) y [TypeFilterAttribute](/aspnet/core/mvc/controllers/filters#typefilterattribute).</span><span class="sxs-lookup"><span data-stu-id="fcc50-114">For more information, see [ServiceFilterAttribute](/aspnet/core/mvc/controllers/filters#servicefilterattribute) and [TypeFilterAttribute](/aspnet/core/mvc/controllers/filters#typefilterattribute).</span></span>

<span data-ttu-id="fcc50-115">Mientras que los constructores de páginas y el middleware permiten la ejecución de código personalizado antes de que se ejecute un método de controlador, solo los filtros de página de Razor permiten el acceso a <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.HttpContext> y la página.</span><span class="sxs-lookup"><span data-stu-id="fcc50-115">While page constructors and middleware enable executing custom code before a handler method executes, only Razor Page filters enable access to <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.HttpContext> and the page.</span></span> <span data-ttu-id="fcc50-116">El middleware tiene acceso al `HttpContext`, pero no al "contexto de la página".</span><span class="sxs-lookup"><span data-stu-id="fcc50-116">Middleware has access to the `HttpContext`, but not to the "page context".</span></span> <span data-ttu-id="fcc50-117">Los filtros tienen un <xref:Microsoft.AspNetCore.Mvc.Filters.FilterContext> parámetro derivado, que proporciona acceso a `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="fcc50-117">Filters have a <xref:Microsoft.AspNetCore.Mvc.Filters.FilterContext> derived parameter, which provides access to `HttpContext`.</span></span> <span data-ttu-id="fcc50-118">Por ejemplo, en el ejemplo [Implementar un atributo de filtro](#ifa) se agrega un encabezado a la respuesta, cosa que no es posible con constructores o con middleware.</span><span class="sxs-lookup"><span data-stu-id="fcc50-118">For example, the [Implement a filter attribute](#ifa) sample adds a header to the response, something that can't be done with constructors or middleware.</span></span>

<span data-ttu-id="fcc50-119">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/filter/3.1sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fcc50-119">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/filter/3.1sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="fcc50-120">Los filtros de páginas de Razor proporcionan los siguientes métodos, que se pueden usar globalmente o bien en el nivel de página:</span><span class="sxs-lookup"><span data-stu-id="fcc50-120">Razor Page filters provide the following methods, which can be applied globally or at the page level:</span></span>

* <span data-ttu-id="fcc50-121">Métodos sincrónicos:</span><span class="sxs-lookup"><span data-stu-id="fcc50-121">Synchronous methods:</span></span>

  * <span data-ttu-id="fcc50-122">[OnPageHandlerSelected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0): se llama a este método después de que se haya seleccionado un método de controlador, pero antes de que el enlace de modelos tenga lugar.</span><span class="sxs-lookup"><span data-stu-id="fcc50-122">[OnPageHandlerSelected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0) : Called after a handler method has been selected, but before model binding occurs.</span></span>
  * <span data-ttu-id="fcc50-123">[OnPageHandlerExecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0): se llama a este método antes de que se ejecute el método de controlador, después de que el enlace de modelos se haya completado.</span><span class="sxs-lookup"><span data-stu-id="fcc50-123">[OnPageHandlerExecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0) : Called before the handler method executes, after model binding is complete.</span></span>
  * <span data-ttu-id="fcc50-124">[OnPageHandlerExecuted](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0): se llama a este método después de que se ejecute el método de controlador, antes de obtener el resultado de la acción.</span><span class="sxs-lookup"><span data-stu-id="fcc50-124">[OnPageHandlerExecuted](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0) : Called after the handler method executes, before the action result.</span></span>

* <span data-ttu-id="fcc50-125">Métodos asincrónicos:</span><span class="sxs-lookup"><span data-stu-id="fcc50-125">Asynchronous methods:</span></span>

  * <span data-ttu-id="fcc50-126">[OnPageHandlerSelectionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0): se llama a este método de forma asincrónica después de que se haya seleccionado el método de controlador, pero antes de que el enlace de modelos tenga lugar.</span><span class="sxs-lookup"><span data-stu-id="fcc50-126">[OnPageHandlerSelectionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0) : Called asynchronously after the handler method has been selected, but before model binding occurs.</span></span>
  * <span data-ttu-id="fcc50-127">[OnPageHandlerExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0): se llama a este método de forma asincrónica antes de que se invoque el método de controlador, después de que el enlace de modelos se haya completado.</span><span class="sxs-lookup"><span data-stu-id="fcc50-127">[OnPageHandlerExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0) : Called asynchronously before the handler method is invoked, after model binding is complete.</span></span>

<span data-ttu-id="fcc50-128">Implemente la versión sincrónica **o** la versión asincrónica de una interfaz de filtro, pero **no** ambas.</span><span class="sxs-lookup"><span data-stu-id="fcc50-128">Implement **either** the synchronous or the async version of a filter interface, **not** both.</span></span> <span data-ttu-id="fcc50-129">El marco comprueba primero si el filtro implementa la interfaz asincrónica y, si es así, es a la interfaz que llama.</span><span class="sxs-lookup"><span data-stu-id="fcc50-129">The framework checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="fcc50-130">De lo contrario, llamará a métodos de interfaz sincrónicos.</span><span class="sxs-lookup"><span data-stu-id="fcc50-130">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="fcc50-131">Si se implementan ambas interfaces, solo se llama a los métodos asincrónicos.</span><span class="sxs-lookup"><span data-stu-id="fcc50-131">If both interfaces are implemented, only the async methods are called.</span></span> <span data-ttu-id="fcc50-132">La misma regla se cumple con las invalidaciones en páginas: implemente la versión sincrónica o asincrónica de la invalidación, pero no ambas.</span><span class="sxs-lookup"><span data-stu-id="fcc50-132">The same rule applies to overrides in pages, implement the synchronous or the async version of the override, not both.</span></span>

## <a name="implement-razor-page-filters-globally"></a><span data-ttu-id="fcc50-133">Implementar filtros de páginas de Razor globalmente</span><span class="sxs-lookup"><span data-stu-id="fcc50-133">Implement Razor Page filters globally</span></span>

<span data-ttu-id="fcc50-134">El siguiente código implementa `IAsyncPageFilter`:</span><span class="sxs-lookup"><span data-stu-id="fcc50-134">The following code implements `IAsyncPageFilter`:</span></span>

[!code-csharp[Main](filter/3.1sample/PageFilter/Filters/SampleAsyncPageFilter.cs?name=snippet1)]

<span data-ttu-id="fcc50-135">En el código anterior, `ProcessUserAgent.Write` es código proporcionado por el usuario que funciona con la cadena de agente de usuario.</span><span class="sxs-lookup"><span data-stu-id="fcc50-135">In the preceding code, `ProcessUserAgent.Write` is user supplied code that works with the user agent string.</span></span>

<span data-ttu-id="fcc50-136">El siguiente código habilita `SampleAsyncPageFilter` en la clase `Startup`:</span><span class="sxs-lookup"><span data-stu-id="fcc50-136">The following code enables the `SampleAsyncPageFilter` in the `Startup` class:</span></span>

[!code-csharp[Main](filter/3.1sample/PageFilter/Startup.cs?name=snippet2)]

<span data-ttu-id="fcc50-137">En el código siguiente se llama a <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> para aplicar el `SampleAsyncPageFilter` solo a las páginas de */Movies*:</span><span class="sxs-lookup"><span data-stu-id="fcc50-137">The following code calls <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> to apply the `SampleAsyncPageFilter` to only pages in */Movies*:</span></span>

[!code-csharp[Main](filter/3.1sample/PageFilter/Startup2.cs?name=snippet2)]

<span data-ttu-id="fcc50-138">El siguiente código implementa el método sincrónico `IPageFilter`:</span><span class="sxs-lookup"><span data-stu-id="fcc50-138">The following code implements the synchronous `IPageFilter`:</span></span>

[!code-csharp[Main](filter/3.1sample/PageFilter/Filters/SamplePageFilter.cs?name=snippet1)]

<span data-ttu-id="fcc50-139">El siguiente código habilita `SamplePageFilter`:</span><span class="sxs-lookup"><span data-stu-id="fcc50-139">The following code enables the `SamplePageFilter`:</span></span>

[!code-csharp[Main](filter/3.1sample/PageFilter/StartupSync.cs?name=snippet2)]

## <a name="implement-razor-page-filters-by-overriding-filter-methods"></a><span data-ttu-id="fcc50-140">Implementar filtros de páginas de Razor invalidando métodos de filtro</span><span class="sxs-lookup"><span data-stu-id="fcc50-140">Implement Razor Page filters by overriding filter methods</span></span>

<span data-ttu-id="fcc50-141">El código siguiente invalida los filtros de página de Razor asincrónicos:</span><span class="sxs-lookup"><span data-stu-id="fcc50-141">The following code overrides the asynchronous Razor Page filters:</span></span>

[!code-csharp[Main](filter/3.1sample/PageFilter/Pages/Index.cshtml.cs?name=snippet)]

<a name="ifa"></a>

## <a name="implement-a-filter-attribute"></a><span data-ttu-id="fcc50-142">Implementar un atributo de filtro</span><span class="sxs-lookup"><span data-stu-id="fcc50-142">Implement a filter attribute</span></span>

<span data-ttu-id="fcc50-143">Se puede crear una subclase del filtro integrado basado en atributos <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter.OnResultExecutionAsync*>.</span><span class="sxs-lookup"><span data-stu-id="fcc50-143">The built-in attribute-based filter <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter.OnResultExecutionAsync*> filter can be subclassed.</span></span> <span data-ttu-id="fcc50-144">El siguiente filtro agrega un encabezado a la respuesta:</span><span class="sxs-lookup"><span data-stu-id="fcc50-144">The following filter adds a header to the response:</span></span>

[!code-csharp[Main](filter/3.1sample/PageFilter/Filters/AddHeaderAttribute.cs)]

<span data-ttu-id="fcc50-145">El siguiente código se aplica al atributo `AddHeader`:</span><span class="sxs-lookup"><span data-stu-id="fcc50-145">The following code applies the `AddHeader` attribute:</span></span>

[!code-csharp[Main](filter/3.1sample/PageFilter/Pages/Movies/Test.cshtml.cs)]

<span data-ttu-id="fcc50-146">Use una herramienta como las herramientas de desarrollo del explorador para examinar los encabezados.</span><span class="sxs-lookup"><span data-stu-id="fcc50-146">Use a tool such as the browser developer tools to examine the headers.</span></span> <span data-ttu-id="fcc50-147">En **Encabezados de respuesta**, se muestra `author: Rick`.</span><span class="sxs-lookup"><span data-stu-id="fcc50-147">Under **Response Headers**, `author: Rick` is displayed.</span></span>

<span data-ttu-id="fcc50-148">Vea [Invalidación del orden predeterminado](xref:mvc/controllers/filters#overriding-the-default-order) para obtener instrucciones sobre cómo invalidar el orden.</span><span class="sxs-lookup"><span data-stu-id="fcc50-148">See [Overriding the default order](xref:mvc/controllers/filters#overriding-the-default-order) for instructions on overriding the order.</span></span>

<span data-ttu-id="fcc50-149">Vea [Cancelación y cortocircuito](xref:mvc/controllers/filters#cancellation-and-short-circuiting) para obtener instrucciones sobre cómo cortocircuitar la canalización de filtro de un filtro.</span><span class="sxs-lookup"><span data-stu-id="fcc50-149">See [Cancellation and short circuiting](xref:mvc/controllers/filters#cancellation-and-short-circuiting) for instructions to short-circuit the filter pipeline from a filter.</span></span>

<a name="auth"></a>

## <a name="authorize-filter-attribute"></a><span data-ttu-id="fcc50-150">Atributo de filtro Authorize</span><span class="sxs-lookup"><span data-stu-id="fcc50-150">Authorize filter attribute</span></span>

<span data-ttu-id="fcc50-151">El atributo [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) se puede aplicar a un `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="fcc50-151">The [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) attribute can be applied to a `PageModel`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Pages/ModelWithAuthFilter.cshtml.cs?highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="fcc50-152">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fcc50-152">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fcc50-153">Los filtros de páginas de Razor [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) e [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) permiten que las páginas de Razor ejecuten código antes y después de que se haya ejecutado un controlador de páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="fcc50-153">Razor Page filters [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) and [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) allow Razor Pages to run code before and after a Razor Page handler is run.</span></span> <span data-ttu-id="fcc50-154">Estos filtros son similares a los [filtros de acción de ASP.NET Core MVC](xref:mvc/controllers/filters#action-filters), salvo por el hecho de que no se pueden usar con métodos de controlador de páginas individuales.</span><span class="sxs-lookup"><span data-stu-id="fcc50-154">Razor Page filters are similar to [ASP.NET Core MVC action filters](xref:mvc/controllers/filters#action-filters), except they can't be applied to individual page handler methods.</span></span>

<span data-ttu-id="fcc50-155">Los filtros de páginas de Razor:</span><span class="sxs-lookup"><span data-stu-id="fcc50-155">Razor Page filters:</span></span>

* <span data-ttu-id="fcc50-156">Ejecutan código después de que se haya seleccionado un método de controlador, pero antes de que el enlace de modelos tenga lugar.</span><span class="sxs-lookup"><span data-stu-id="fcc50-156">Run code after a handler method has been selected, but before model binding occurs.</span></span>
* <span data-ttu-id="fcc50-157">Ejecutan código antes de que se ejecute el método de controlador, después de que el enlace de modelos se haya completado.</span><span class="sxs-lookup"><span data-stu-id="fcc50-157">Run code before the handler method executes, after model binding is complete.</span></span>
* <span data-ttu-id="fcc50-158">Ejecutan código después de que se haya ejecutado el método de controlador.</span><span class="sxs-lookup"><span data-stu-id="fcc50-158">Run code after the handler method executes.</span></span>
* <span data-ttu-id="fcc50-159">Se pueden implementar en una página o globalmente.</span><span class="sxs-lookup"><span data-stu-id="fcc50-159">Can be implemented on a page or globally.</span></span>
* <span data-ttu-id="fcc50-160">No se pueden usar con métodos de controlador de páginas específicas.</span><span class="sxs-lookup"><span data-stu-id="fcc50-160">Cannot be applied to specific page handler methods.</span></span>

<span data-ttu-id="fcc50-161">Se puede ejecutar código antes de que un método de controlador se ejecute por medio del constructor de página o de middleware, pero solo los filtros de páginas de Razor tienen acceso a [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_HttpContext).</span><span class="sxs-lookup"><span data-stu-id="fcc50-161">Code can be run before a handler method executes using the page constructor or middleware, but only Razor Page filters have access to [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_HttpContext).</span></span> <span data-ttu-id="fcc50-162">Los filtros tienen un parámetro derivado [FilterContext](/dotnet/api/microsoft.aspnetcore.mvc.filters.filtercontext?view=aspnetcore-2.0) que concede acceso a `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="fcc50-162">Filters have a [FilterContext](/dotnet/api/microsoft.aspnetcore.mvc.filters.filtercontext?view=aspnetcore-2.0) derived parameter, which provides access to `HttpContext`.</span></span> <span data-ttu-id="fcc50-163">Por ejemplo, en el ejemplo [Implementar un atributo de filtro](#ifa) se agrega un encabezado a la respuesta, cosa que no es posible con constructores o con middleware.</span><span class="sxs-lookup"><span data-stu-id="fcc50-163">For example, the [Implement a filter attribute](#ifa) sample adds a header to the response, something that can't be done with constructors or middleware.</span></span>

<span data-ttu-id="fcc50-164">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/filter/sample/PageFilter) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fcc50-164">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/filter/sample/PageFilter) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="fcc50-165">Los filtros de páginas de Razor proporcionan los siguientes métodos, que se pueden usar globalmente o bien en el nivel de página:</span><span class="sxs-lookup"><span data-stu-id="fcc50-165">Razor Page filters provide the following methods, which can be applied globally or at the page level:</span></span>

* <span data-ttu-id="fcc50-166">Métodos sincrónicos:</span><span class="sxs-lookup"><span data-stu-id="fcc50-166">Synchronous methods:</span></span>

  * <span data-ttu-id="fcc50-167">[OnPageHandlerSelected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0): se llama a este método después de que se haya seleccionado un método de controlador, pero antes de que el enlace de modelos tenga lugar.</span><span class="sxs-lookup"><span data-stu-id="fcc50-167">[OnPageHandlerSelected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0) : Called after a handler method has been selected, but before model binding occurs.</span></span>
  * <span data-ttu-id="fcc50-168">[OnPageHandlerExecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0): se llama a este método antes de que se ejecute el método de controlador, después de que el enlace de modelos se haya completado.</span><span class="sxs-lookup"><span data-stu-id="fcc50-168">[OnPageHandlerExecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0) : Called before the handler method executes, after model binding is complete.</span></span>
  * <span data-ttu-id="fcc50-169">[OnPageHandlerExecuted](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0): se llama a este método después de que se ejecute el método de controlador, antes de obtener el resultado de la acción.</span><span class="sxs-lookup"><span data-stu-id="fcc50-169">[OnPageHandlerExecuted](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0) : Called after the handler method executes, before the action result.</span></span>

* <span data-ttu-id="fcc50-170">Métodos asincrónicos:</span><span class="sxs-lookup"><span data-stu-id="fcc50-170">Asynchronous methods:</span></span>

  * <span data-ttu-id="fcc50-171">[OnPageHandlerSelectionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0): se llama a este método de forma asincrónica después de que se haya seleccionado el método de controlador, pero antes de que el enlace de modelos tenga lugar.</span><span class="sxs-lookup"><span data-stu-id="fcc50-171">[OnPageHandlerSelectionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0) : Called asynchronously after the handler method has been selected, but before model binding occurs.</span></span>
  * <span data-ttu-id="fcc50-172">[OnPageHandlerExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0): se llama a este método de forma asincrónica antes de que se invoque el método de controlador, después de que el enlace de modelos se haya completado.</span><span class="sxs-lookup"><span data-stu-id="fcc50-172">[OnPageHandlerExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0) : Called asynchronously before the handler method is invoked, after model binding is complete.</span></span>

> [!NOTE]
> <span data-ttu-id="fcc50-173">Implemente la versión sincrónica **o** la versión asincrónica de una interfaz de filtro, pero no ambas.</span><span class="sxs-lookup"><span data-stu-id="fcc50-173">Implement **either** the synchronous or the async version of a filter interface, not both.</span></span> <span data-ttu-id="fcc50-174">El marco comprueba primero si el filtro implementa la interfaz asincrónica y, si es así, es a la interfaz que llama.</span><span class="sxs-lookup"><span data-stu-id="fcc50-174">The framework checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="fcc50-175">De lo contrario, llamará a métodos de interfaz sincrónicos.</span><span class="sxs-lookup"><span data-stu-id="fcc50-175">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="fcc50-176">Si se implementan ambas interfaces, solo se llama a los métodos asincrónicos.</span><span class="sxs-lookup"><span data-stu-id="fcc50-176">If both interfaces are implemented, only the async methods are called.</span></span> <span data-ttu-id="fcc50-177">La misma regla se cumple con las invalidaciones en páginas: implemente la versión sincrónica o asincrónica de la invalidación, pero no ambas.</span><span class="sxs-lookup"><span data-stu-id="fcc50-177">The same rule applies to overrides in pages, implement the synchronous or the async version of the override, not both.</span></span>

## <a name="implement-razor-page-filters-globally"></a><span data-ttu-id="fcc50-178">Implementar filtros de páginas de Razor globalmente</span><span class="sxs-lookup"><span data-stu-id="fcc50-178">Implement Razor Page filters globally</span></span>

<span data-ttu-id="fcc50-179">El siguiente código implementa `IAsyncPageFilter`:</span><span class="sxs-lookup"><span data-stu-id="fcc50-179">The following code implements `IAsyncPageFilter`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Filters/SampleAsyncPageFilter.cs?name=snippet1)]

<span data-ttu-id="fcc50-180">En el código anterior, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger?view=aspnetcore-2.0) no es necesario;</span><span class="sxs-lookup"><span data-stu-id="fcc50-180">In the preceding code, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger?view=aspnetcore-2.0) is not required.</span></span> <span data-ttu-id="fcc50-181">se usa para proporcionar información de seguimiento relativa a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="fcc50-181">It's used in the sample to provide trace information for the application.</span></span>

<span data-ttu-id="fcc50-182">El siguiente código habilita `SampleAsyncPageFilter` en la clase `Startup`:</span><span class="sxs-lookup"><span data-stu-id="fcc50-182">The following code enables the `SampleAsyncPageFilter` in the `Startup` class:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet2&highlight=11)]

<span data-ttu-id="fcc50-183">El siguiente código muestra la clase `Startup` completa:</span><span class="sxs-lookup"><span data-stu-id="fcc50-183">The following code shows the complete `Startup` class:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet1)]

<span data-ttu-id="fcc50-184">El siguiente código llama a `AddFolderApplicationModelConvention` para aplicar `SampleAsyncPageFilter` solo a las páginas en */subFolder*:</span><span class="sxs-lookup"><span data-stu-id="fcc50-184">The following code calls `AddFolderApplicationModelConvention` to apply the `SampleAsyncPageFilter` to only pages in */subFolder*:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Startup2.cs?name=snippet2)]

<span data-ttu-id="fcc50-185">El siguiente código implementa el método sincrónico `IPageFilter`:</span><span class="sxs-lookup"><span data-stu-id="fcc50-185">The following code implements the synchronous `IPageFilter`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Filters/SamplePageFilter.cs?name=snippet1)]

<span data-ttu-id="fcc50-186">El siguiente código habilita `SamplePageFilter`:</span><span class="sxs-lookup"><span data-stu-id="fcc50-186">The following code enables the `SamplePageFilter`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/StartupSync.cs?name=snippet2&highlight=11)]

## <a name="implement-razor-page-filters-by-overriding-filter-methods"></a><span data-ttu-id="fcc50-187">Implementar filtros de páginas de Razor invalidando métodos de filtro</span><span class="sxs-lookup"><span data-stu-id="fcc50-187">Implement Razor Page filters by overriding filter methods</span></span>

<span data-ttu-id="fcc50-188">El siguiente código invalida los filtros de páginas de Razor sincrónicos:</span><span class="sxs-lookup"><span data-stu-id="fcc50-188">The following code overrides the synchronous Razor Page filters:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Pages/Index.cshtml.cs)]

<a name="ifa"></a>

## <a name="implement-a-filter-attribute"></a><span data-ttu-id="fcc50-189">Implementar un atributo de filtro</span><span class="sxs-lookup"><span data-stu-id="fcc50-189">Implement a filter attribute</span></span>

<span data-ttu-id="fcc50-190">El filtro basado en atributos integrado [OnResultExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncresultfilter.onresultexecutionasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Filters_IAsyncResultFilter_OnResultExecutionAsync_Microsoft_AspNetCore_Mvc_Filters_ResultExecutingContext_Microsoft_AspNetCore_Mvc_Filters_ResultExecutionDelegate_) puede ser una subclase.</span><span class="sxs-lookup"><span data-stu-id="fcc50-190">The built-in attribute-based filter [OnResultExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncresultfilter.onresultexecutionasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Filters_IAsyncResultFilter_OnResultExecutionAsync_Microsoft_AspNetCore_Mvc_Filters_ResultExecutingContext_Microsoft_AspNetCore_Mvc_Filters_ResultExecutionDelegate_) filter can be subclassed.</span></span> <span data-ttu-id="fcc50-191">El siguiente filtro agrega un encabezado a la respuesta:</span><span class="sxs-lookup"><span data-stu-id="fcc50-191">The following filter adds a header to the response:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Filters/AddHeaderAttribute.cs)]

<span data-ttu-id="fcc50-192">El siguiente código se aplica al atributo `AddHeader`:</span><span class="sxs-lookup"><span data-stu-id="fcc50-192">The following code applies the `AddHeader` attribute:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Pages/Contact.cshtml.cs?name=snippet1)]

<span data-ttu-id="fcc50-193">Vea [Invalidación del orden predeterminado](xref:mvc/controllers/filters#overriding-the-default-order) para obtener instrucciones sobre cómo invalidar el orden.</span><span class="sxs-lookup"><span data-stu-id="fcc50-193">See [Overriding the default order](xref:mvc/controllers/filters#overriding-the-default-order) for instructions on overriding the order.</span></span>

<span data-ttu-id="fcc50-194">Vea [Cancelación y cortocircuito](xref:mvc/controllers/filters#cancellation-and-short-circuiting) para obtener instrucciones sobre cómo cortocircuitar la canalización de filtro de un filtro.</span><span class="sxs-lookup"><span data-stu-id="fcc50-194">See [Cancellation and short circuiting](xref:mvc/controllers/filters#cancellation-and-short-circuiting) for instructions to short-circuit the filter pipeline from a filter.</span></span> 

<a name="auth"></a>

## <a name="authorize-filter-attribute"></a><span data-ttu-id="fcc50-195">Atributo de filtro Authorize</span><span class="sxs-lookup"><span data-stu-id="fcc50-195">Authorize filter attribute</span></span>

<span data-ttu-id="fcc50-196">El atributo [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) se puede aplicar a un `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="fcc50-196">The [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) attribute can be applied to a `PageModel`:</span></span>

[!code-csharp[Main](filter/sample/PageFilter/Pages/ModelWithAuthFilter.cshtml.cs?highlight=7)]

::: moniker-end
