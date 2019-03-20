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
# <a name="filter-methods-for-razor-pages-in-aspnet-core"></a>Métodos de filtrado de páginas de Razor en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Los filtros de páginas de Razor [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) e [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) permiten que las páginas de Razor ejecuten código antes y después de que se haya ejecutado un controlador de páginas de Razor. Estos filtros son similares a los [filtros de acción de ASP.NET Core MVC](xref:mvc/controllers/filters#action-filters), salvo por el hecho de que no se pueden usar con métodos de controlador de páginas individuales. 

Los filtros de páginas de Razor:

* Ejecutan código después de que se haya seleccionado un método de controlador, pero antes de que el enlace de modelos tenga lugar.
* Ejecutan código antes de que se ejecute el método de controlador, después de que el enlace de modelos se haya completado.
* Ejecutan código después de que se haya ejecutado el método de controlador.
* Se pueden implementar en una página o globalmente.
* No se pueden usar con métodos de controlador de páginas específicas.

Se puede ejecutar código antes de que un método de controlador se ejecute por medio del constructor de página o de middleware, pero solo los filtros de páginas de Razor tienen acceso a [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_HttpContext). Los filtros tienen un parámetro derivado [FilterContext](/dotnet/api/microsoft.aspnetcore.mvc.filters.filtercontext?view=aspnetcore-2.0) que concede acceso a `HttpContext`. Por ejemplo, en el ejemplo [Implementar un atributo de filtro](#ifa) se agrega un encabezado a la respuesta, cosa que no es posible con constructores o con middleware.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/filter/sample/PageFilter) ([cómo descargarlo](xref:index#how-to-download-a-sample))

Los filtros de páginas de Razor proporcionan los siguientes métodos, que se pueden usar globalmente o bien en el nivel de página:

* Métodos sincrónicos:

  * [OnPageHandlerSelected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0) : Se llama después de que se ha seleccionado un método de controlador, pero antes de modelo se produce el enlace.
  * [OnPageHandlerExecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0) : Se llama antes de que se ejecuta el método de controlador, una vez completado el enlace de modelos.
  * [OnPageHandlerExecuted](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0) : Llamado cuando se ejecuta el método de controlador, antes de resultado de la acción.

* Métodos asincrónicos:

  * [OnPageHandlerSelectionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0) : Llamar de forma asincrónica una vez que se ha seleccionado el método de controlador, pero antes de que se produce el enlace de modelos.
  * [OnPageHandlerExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0) : Se llama asincrónicamente antes de invoca el método de controlador, una vez completado el enlace de modelos.

> [!NOTE]
> Implemente la versión sincrónica **o** la versión asincrónica de una interfaz de filtro, pero no ambas. El marco comprueba primero si el filtro implementa la interfaz asincrónica y, si es así, es a la interfaz que llama. De lo contrario, llamará a métodos de interfaz sincrónicos. Si se implementan ambas interfaces, solo se llamará a los métodos asincrónicos. La misma regla se cumple con las invalidaciones en páginas: implemente la versión sincrónica o asincrónica de la invalidación, pero no ambas.

## <a name="implement-razor-page-filters-globally"></a>Implementar filtros de páginas de Razor globalmente

El siguiente código implementa `IAsyncPageFilter`:

[!code-csharp[Main](filter/sample/PageFilter/Filters/SampleAsyncPageFilter.cs?name=snippet1)]

En el código anterior, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger?view=aspnetcore-2.0) no es necesario; se usa para proporcionar información de seguimiento relativa a la aplicación.

El siguiente código habilita `SampleAsyncPageFilter` en la clase `Startup`:

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet2&highlight=11)]

El siguiente código muestra la clase `Startup` completa:

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet1)]

El siguiente código llama a `AddFolderApplicationModelConvention` para aplicar `SampleAsyncPageFilter` solo a las páginas en */subFolder*:

[!code-csharp[Main](filter/sample/PageFilter/Startup2.cs?name=snippet2)]

El siguiente código implementa el método sincrónico `IPageFilter`:

[!code-csharp[Main](filter/sample/PageFilter/Filters/SamplePageFilter.cs?name=snippet1)]

El siguiente código habilita `SamplePageFilter`:

[!code-csharp[Main](filter/sample/PageFilter/StartupSync.cs?name=snippet2&highlight=11)]

::: moniker range=">= aspnetcore-2.1"

## <a name="implement-razor-page-filters-by-overriding-filter-methods"></a>Implementar filtros de páginas de Razor invalidando métodos de filtro

El siguiente código invalida los filtros de páginas de Razor sincrónicos:

[!code-csharp[Main](filter/sample/PageFilter/Pages/Index.cshtml.cs)]

::: moniker-end

<a name="ifa"></a>

## <a name="implement-a-filter-attribute"></a>Implementar un atributo de filtro

El filtro basado en atributos integrado [OnResultExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncresultfilter.onresultexecutionasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Filters_IAsyncResultFilter_OnResultExecutionAsync_Microsoft_AspNetCore_Mvc_Filters_ResultExecutingContext_Microsoft_AspNetCore_Mvc_Filters_ResultExecutionDelegate_) puede ser una subclase. El siguiente filtro agrega un encabezado a la respuesta:

[!code-csharp[Main](filter/sample/PageFilter/Filters/AddHeaderAttribute.cs)]

El siguiente código se aplica al atributo `AddHeader`:

[!code-csharp[Main](filter/sample/PageFilter/Pages/Contact.cshtml.cs?name=snippet1)]

Vea [Invalidación del orden predeterminado](xref:mvc/controllers/filters#overriding-the-default-order) para obtener instrucciones sobre cómo invalidar el orden.

Vea [Cancelación y cortocircuito](xref:mvc/controllers/filters#cancellation-and-short-circuiting) para obtener instrucciones sobre cómo cortocircuitar la canalización de filtro de un filtro. 

<a name="auth"></a>

## <a name="authorize-filter-attribute"></a>Atributo de filtro Authorize

El atributo [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) se puede aplicar a un `PageModel`:

[!code-csharp[Main](filter/sample/PageFilter/Pages/ModelWithAuthFilter.cshtml.cs?highlight=7)]
