---
title: Métodos de filtrado de páginas de Razor en ASP.NET Core
author: Rick-Anderson
description: Obtenga información sobre cómo crear métodos de filtrado de páginas de Razor en ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 2/18/2020
uid: razor-pages/filter
ms.openlocfilehash: cd772da8ed565bc779d8c6bcc7c9949a0c1c7c60
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2020
ms.locfileid: "78648053"
---
# <a name="filter-methods-for-razor-pages-in-aspnet-core"></a>Métodos de filtrado de páginas de Razor en ASP.NET Core

::: moniker range=">= aspnetcore-3.0"

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Los filtros de páginas de Razor [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) e [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) permiten que las páginas de Razor ejecuten código antes y después de que se haya ejecutado un controlador de páginas de Razor. Estos filtros son similares a los [filtros de acción de ASP.NET Core MVC](xref:mvc/controllers/filters#action-filters), salvo por el hecho de que no se pueden usar con métodos de controlador de páginas individuales.

Los filtros de páginas de Razor:

* Ejecutan código después de que se haya seleccionado un método de controlador, pero antes de que el enlace de modelos tenga lugar.
* Ejecutan código antes de que se ejecute el método de controlador, después de que el enlace de modelos se haya completado.
* Ejecutan código después de que se haya ejecutado el método de controlador.
* Se pueden implementar en una página o globalmente.
* No se pueden usar con métodos de controlador de páginas específicas.
* Pueden tener dependencias de constructor rellenadas mediante la [Inserción de dependencias](xref:fundamentals/dependency-injection) (DI). Para obtener más información, vea [ServiceFilterAttribute](/aspnet/core/mvc/controllers/filters#servicefilterattribute) y [TypeFilterAttribute](/aspnet/core/mvc/controllers/filters#typefilterattribute).

Mientras que los constructores de páginas y el middleware permiten la ejecución de código personalizado antes de que se ejecute un método de controlador, solo los filtros de Página de Razor permiten el acceso a <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.HttpContext> y a la página. El middleware tiene acceso a `HttpContext`, pero no al "contexto de la página". Los filtros tienen un parámetro derivado <xref:Microsoft.AspNetCore.Mvc.Filters.FilterContext> que proporciona acceso a `HttpContext`. Por ejemplo, en el ejemplo [Implementar un atributo de filtro](#ifa) se agrega un encabezado a la respuesta, cosa que no es posible con constructores o con middleware.

[Vea o descargue el código de ejemplo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/filter/3.1sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))

Los filtros de páginas de Razor proporcionan los siguientes métodos, que se pueden usar globalmente o bien en el nivel de página:

* Métodos sincrónicos:

  * [OnPageHandlerSelected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0): se llama a este método después de que se haya seleccionado un método de controlador, pero antes de que el enlace de modelos tenga lugar.
  * [OnPageHandlerExecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0): se llama a este método antes de que se ejecute el método de controlador, después de que el enlace de modelos se haya completado.
  * [OnPageHandlerExecuted](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0): se llama a este método después de que se ejecute el método de controlador, antes de obtener el resultado de la acción.

* Métodos asincrónicos:

  * [OnPageHandlerSelectionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0): se llama a este método de forma asincrónica después de que se haya seleccionado el método de controlador, pero antes de que el enlace de modelos tenga lugar.
  * [OnPageHandlerExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0): se llama a este método de forma asincrónica antes de que se invoque el método de controlador, después de que el enlace de modelos se haya completado.

Implemente la versión sincrónica **o** la versión asincrónica de una interfaz de filtro, pero **no** ambas. El marco comprueba primero si el filtro implementa la interfaz asincrónica y, si es así, es a la interfaz que llama. De lo contrario, llamará a métodos de interfaz sincrónicos. Si se implementan ambas interfaces, solo se llamará a los métodos asincrónicos. La misma regla se cumple con las invalidaciones en páginas: implemente la versión sincrónica o asincrónica de la invalidación, pero no ambas.

## <a name="implement-razor-page-filters-globally"></a>Implementar filtros de páginas de Razor globalmente

El siguiente código implementa `IAsyncPageFilter`:

[!code-csharp[Main](filter/3.1sample/PageFilter/Filters/SampleAsyncPageFilter.cs?name=snippet1)]

En el código anterior, `ProcessUserAgent.Write` es código proporcionado por el usuario que funciona con la cadena del agente de usuario.

El siguiente código habilita `SampleAsyncPageFilter` en la clase `Startup`:

[!code-csharp[Main](filter/3.1sample/PageFilter/Startup.cs?name=snippet2)]

El código siguiente llama a <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> para aplicar `SampleAsyncPageFilter` solo a las páginas en */Movies*:

[!code-csharp[Main](filter/3.1sample/PageFilter/Startup2.cs?name=snippet2)]

El siguiente código implementa el método sincrónico `IPageFilter`:

[!code-csharp[Main](filter/3.1sample/PageFilter/Filters/SamplePageFilter.cs?name=snippet1)]

El siguiente código habilita `SamplePageFilter`:

[!code-csharp[Main](filter/3.1sample/PageFilter/StartupSync.cs?name=snippet2)]

## <a name="implement-razor-page-filters-by-overriding-filter-methods"></a>Implementar filtros de páginas de Razor invalidando métodos de filtro

El código siguiente invalida los filtros de Página de Razor asincrónicos:

[!code-csharp[Main](filter/3.1sample/PageFilter/Pages/Index.cshtml.cs?name=snippet)]

<a name="ifa"></a>

## <a name="implement-a-filter-attribute"></a>Implementar un atributo de filtro

Se pueden crear subclases del filtro <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter.OnResultExecutionAsync*> basado en atributos integrados. El siguiente filtro agrega un encabezado a la respuesta:

[!code-csharp[Main](filter/3.1sample/PageFilter/Filters/AddHeaderAttribute.cs)]

El siguiente código se aplica al atributo `AddHeader`:

[!code-csharp[Main](filter/3.1sample/PageFilter/Pages/Movies/Test.cshtml.cs)]

Para examinar los encabezados, use una herramienta como, por ejemplo, las herramientas de desarrollo del explorador. En **Encabezados de respuesta**, se muestra `author: Rick`.

Vea [Invalidación del orden predeterminado](xref:mvc/controllers/filters#overriding-the-default-order) para obtener instrucciones sobre cómo invalidar el orden.

Vea [Cancelación y cortocircuito](xref:mvc/controllers/filters#cancellation-and-short-circuiting) para obtener instrucciones sobre cómo cortocircuitar la canalización de filtro de un filtro.

<a name="auth"></a>

## <a name="authorize-filter-attribute"></a>Atributo de filtro Authorize

El atributo [Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) se puede aplicar a un `PageModel`:

[!code-csharp[Main](filter/sample/PageFilter/Pages/ModelWithAuthFilter.cshtml.cs?highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Los filtros de páginas de Razor [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) e [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) permiten que las páginas de Razor ejecuten código antes y después de que se haya ejecutado un controlador de páginas de Razor. Estos filtros son similares a los [filtros de acción de ASP.NET Core MVC](xref:mvc/controllers/filters#action-filters), salvo por el hecho de que no se pueden usar con métodos de controlador de páginas individuales.

Los filtros de páginas de Razor:

* Ejecutan código después de que se haya seleccionado un método de controlador, pero antes de que el enlace de modelos tenga lugar.
* Ejecutan código antes de que se ejecute el método de controlador, después de que el enlace de modelos se haya completado.
* Ejecutan código después de que se haya ejecutado el método de controlador.
* Se pueden implementar en una página o globalmente.
* No se pueden usar con métodos de controlador de páginas específicas.

Se puede ejecutar código antes de que un método de controlador se ejecute por medio del constructor de página o de middleware, pero solo los filtros de páginas de Razor tienen acceso a [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_HttpContext). Los filtros tienen un parámetro derivado [FilterContext](/dotnet/api/microsoft.aspnetcore.mvc.filters.filtercontext?view=aspnetcore-2.0) que concede acceso a `HttpContext`. Por ejemplo, en el ejemplo [Implementar un atributo de filtro](#ifa) se agrega un encabezado a la respuesta, cosa que no es posible con constructores o con middleware.

[Vea o descargue el código de ejemplo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/filter/sample/PageFilter) ([cómo descargarlo](xref:index#how-to-download-a-sample))

Los filtros de páginas de Razor proporcionan los siguientes métodos, que se pueden usar globalmente o bien en el nivel de página:

* Métodos sincrónicos:

  * [OnPageHandlerSelected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0): se llama a este método después de que se haya seleccionado un método de controlador, pero antes de que el enlace de modelos tenga lugar.
  * [OnPageHandlerExecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0): se llama a este método antes de que se ejecute el método de controlador, después de que el enlace de modelos se haya completado.
  * [OnPageHandlerExecuted](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0): se llama a este método después de que se ejecute el método de controlador, antes de obtener el resultado de la acción.

* Métodos asincrónicos:

  * [OnPageHandlerSelectionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0): se llama a este método de forma asincrónica después de que se haya seleccionado el método de controlador, pero antes de que el enlace de modelos tenga lugar.
  * [OnPageHandlerExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0): se llama a este método de forma asincrónica antes de que se invoque el método de controlador, después de que el enlace de modelos se haya completado.

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

## <a name="implement-razor-page-filters-by-overriding-filter-methods"></a>Implementar filtros de páginas de Razor invalidando métodos de filtro

El siguiente código invalida los filtros de páginas de Razor sincrónicos:

[!code-csharp[Main](filter/sample/PageFilter/Pages/Index.cshtml.cs)]

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

::: moniker-end
