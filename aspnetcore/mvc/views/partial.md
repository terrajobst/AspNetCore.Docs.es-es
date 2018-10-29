---
title: Vistas parciales en ASP.NET Core
author: ardalis
description: Descubra cómo usar las vistas parciales para dividir los archivos de marcado de gran tamaño y reducir la duplicación de marcado común en las páginas web en aplicaciones ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 09/11/2018
uid: mvc/views/partial
ms.openlocfilehash: ff4b99580990edbd768128d77214e664a1e29e56
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207230"
---
# <a name="partial-views-in-aspnet-core"></a><span data-ttu-id="ef6d7-103">Vistas parciales en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ef6d7-103">Partial views in ASP.NET Core</span></span>

<span data-ttu-id="ef6d7-104">Por [Steve Smith](https://ardalis.com/), [Luke Latham](https://github.com/guardrex), [Maher JENDOUBI](https://twitter.com/maherjend), [Rick Anderson](https://twitter.com/RickAndMSFT) y [Scott Sauber](https://twitter.com/scottsauber)</span><span class="sxs-lookup"><span data-stu-id="ef6d7-104">By [Steve Smith](https://ardalis.com/), [Luke Latham](https://github.com/guardrex), [Maher JENDOUBI](https://twitter.com/maherjend), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Scott Sauber](https://twitter.com/scottsauber)</span></span>

<span data-ttu-id="ef6d7-105">Una vista parcial es un archivo de marcado [Razor](xref:mvc/views/razor) (*.cshtml*) que representa una salida HTML *dentro* de otra salida representada del archivo de marcado.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-105">A partial view is a [Razor](xref:mvc/views/razor) markup file (*.cshtml*) that renders HTML output *within* another markup file's rendered output.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="ef6d7-106">El término *vista parcial* se usa al desarrollar una aplicación MVC, donde a los archivos de marcado se les denomina *vistas*, o una aplicación de Razor Pages, donde a los archivos de marcado se les denomina *páginas*.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-106">The term *partial view* is used when developing either an MVC app, where markup files are called *views*, or a Razor Pages app, where markup files are called *pages*.</span></span> <span data-ttu-id="ef6d7-107">En este tema se hace referencia genéricamente a las vistas de MVC y a las páginas de Razor Pages como *archivos de marcado*.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-107">This topic generically refers to MVC views and Razor Pages pages as *markup files*.</span></span>

::: moniker-end

<span data-ttu-id="ef6d7-108">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/partial/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ef6d7-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/partial/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-partial-views"></a><span data-ttu-id="ef6d7-109">¿Cuándo se usan las vistas parciales?</span><span class="sxs-lookup"><span data-stu-id="ef6d7-109">When to use partial views</span></span>

<span data-ttu-id="ef6d7-110">Las vistas parciales son una forma eficaz de:</span><span class="sxs-lookup"><span data-stu-id="ef6d7-110">Partial views are an effective way to:</span></span>

* <span data-ttu-id="ef6d7-111">Dividir los archivos de marcado grandes en componentes más pequeños.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-111">Break up large markup files into smaller components.</span></span>

  <span data-ttu-id="ef6d7-112">En un gran archivo de marcado complejo formado por varias partes lógicas, se ofrece la ventaja de trabajar con cada pieza aislada en una vista parcial.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-112">In a large, complex markup file composed of several logical pieces, there's an advantage to working with each piece isolated into a partial view.</span></span> <span data-ttu-id="ef6d7-113">El código del archivo de marcado es fácil de administrar porque el marcado solo contiene la estructura general de la página y las referencias a las vistas parciales.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-113">The code in the markup file is manageable because the markup only contains the overall page structure and references to partial views.</span></span>
* <span data-ttu-id="ef6d7-114">Reducir la duplicación de contenido de marcado común en todos los archivos de marcado.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-114">Reduce the duplication of common markup content across markup files.</span></span>

  <span data-ttu-id="ef6d7-115">Cuando se usan los mismos elementos de marcado en todos los archivos de marcado, una vista parcial elimina la duplicación del contenido de marcado en un archivo de vista parcial.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-115">When the same markup elements are used across markup files, a partial view removes the duplication of markup content into one partial view file.</span></span> <span data-ttu-id="ef6d7-116">Cuando se cambia el marcado en la vista parcial, actualiza la salida representada de los archivos de marcado que utilizan la vista parcial.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-116">When the markup is changed in the partial view, it updates the rendered output of the markup files that use the partial view.</span></span>

<span data-ttu-id="ef6d7-117">Las vistas parciales no deben utilizarse para mantener los elementos de diseño comunes.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-117">Partial views shouldn't be used to maintain common layout elements.</span></span> <span data-ttu-id="ef6d7-118">Se deben especificar elementos de diseño comunes en los archivos [_Layout.cshtml](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="ef6d7-118">Common layout elements should be specified in [_Layout.cshtml](xref:mvc/views/layout) files.</span></span>

<span data-ttu-id="ef6d7-119">No utilice una vista parcial donde se requiera la ejecución de código o lógica de representación compleja para representar el marcado.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-119">Don't use a partial view where complex rendering logic or code execution is required to render the markup.</span></span> <span data-ttu-id="ef6d7-120">En lugar de una vista parcial, use un [componente de vista](xref:mvc/views/view-components).</span><span class="sxs-lookup"><span data-stu-id="ef6d7-120">Instead of a partial view, use a [view component](xref:mvc/views/view-components).</span></span>

## <a name="declare-partial-views"></a><span data-ttu-id="ef6d7-121">Declaración de vistas parciales</span><span class="sxs-lookup"><span data-stu-id="ef6d7-121">Declare partial views</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="ef6d7-122">Una vista parcial es un archivo de marcado *.cshtml* que se mantiene dentro de la carpeta *Vistas* (MVC) o *Páginas* (Razor Pages).</span><span class="sxs-lookup"><span data-stu-id="ef6d7-122">A partial view is a *.cshtml* markup file maintained within the *Views* folder (MVC) or *Pages* folder (Razor Pages).</span></span>

<span data-ttu-id="ef6d7-123">En ASP.NET Core MVC, la clase <xref:Microsoft.AspNetCore.Mvc.ViewResult> de un controlador es capaz de devolver una vista o una vista parcial.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-123">In ASP.NET Core MVC, a controller's <xref:Microsoft.AspNetCore.Mvc.ViewResult> is capable of returning either a view or a partial view.</span></span> <span data-ttu-id="ef6d7-124">Está planeada una funcionalidad similar para Razor Pages en ASP.NET Core 2.2.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-124">An analogous capability is planned for Razor Pages in ASP.NET Core 2.2.</span></span> <span data-ttu-id="ef6d7-125">En Razor Pages, una clase <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> puede devolver una clase <xref:Microsoft.AspNetCore.Mvc.PartialViewResult>.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-125">In Razor Pages, a <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> can return a <xref:Microsoft.AspNetCore.Mvc.PartialViewResult>.</span></span> <span data-ttu-id="ef6d7-126">La referencia y representación de vistas parciales se describe en la sección [Referencia a una vista parcial](#reference-a-partial-view).</span><span class="sxs-lookup"><span data-stu-id="ef6d7-126">Referencing and rendering partial views is described in the [Reference a partial view](#reference-a-partial-view) section.</span></span>

<span data-ttu-id="ef6d7-127">A diferencia de la representación de páginas o vistas de MVC, una vista parcial no ejecuta *_ViewStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-127">Unlike MVC view or page rendering, a partial view doesn't run *_ViewStart.cshtml*.</span></span> <span data-ttu-id="ef6d7-128">Para más información sobre *_ViewStart.cshtml*, vea <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-128">For more information on *_ViewStart.cshtml*, see <xref:mvc/views/layout>.</span></span>

<span data-ttu-id="ef6d7-129">Los nombres de archivo de las vistas parciales suelen comenzar con un guión bajo (`_`).</span><span class="sxs-lookup"><span data-stu-id="ef6d7-129">Partial view file names often begin with an underscore (`_`).</span></span> <span data-ttu-id="ef6d7-130">Esta convención de nomenclatura no es obligatoria, pero ayuda a diferenciar visualmente las vistas parciales de las vistas y las páginas.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-130">This naming convention isn't required, but it helps to visually differentiate partial views from views and pages.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="ef6d7-131">Una vista parcial es un archivo de marcado *.cshtml* que se mantiene dentro de la carpeta *Vistas*.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-131">A partial view is a *.cshtml* markup file maintained within the *Views* folder.</span></span>

<span data-ttu-id="ef6d7-132">La clase <xref:Microsoft.AspNetCore.Mvc.ViewResult> de un controlador es capaz de devolver una vista o una vista parcial.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-132">A controller's <xref:Microsoft.AspNetCore.Mvc.ViewResult> is capable of returning either a view or a partial view.</span></span>

<span data-ttu-id="ef6d7-133">A diferencia de la representación de vistas de MVC, una vista parcial no ejecuta *_ViewStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-133">Unlike MVC view rendering, a partial view doesn't run *_ViewStart.cshtml*.</span></span> <span data-ttu-id="ef6d7-134">Para más información sobre *_ViewStart.cshtml*, vea <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-134">For more information on *_ViewStart.cshtml*, see <xref:mvc/views/layout>.</span></span>

<span data-ttu-id="ef6d7-135">Los nombres de archivo de las vistas parciales suelen comenzar con un guión bajo (`_`).</span><span class="sxs-lookup"><span data-stu-id="ef6d7-135">Partial view file names often begin with an underscore (`_`).</span></span> <span data-ttu-id="ef6d7-136">Esta convención de nomenclatura no es obligatoria, pero ayuda a diferenciar visualmente las vistas parciales de las vistas.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-136">This naming convention isn't required, but it helps to visually differentiate partial views from views.</span></span>

::: moniker-end

## <a name="reference-a-partial-view"></a><span data-ttu-id="ef6d7-137">Referencia a una vista parcial</span><span class="sxs-lookup"><span data-stu-id="ef6d7-137">Reference a partial view</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="ef6d7-138">Dentro de un archivo de marcado, hay varias maneras de hacer referencia a una vista parcial.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-138">Within a markup file, there are several ways to reference a partial view.</span></span> <span data-ttu-id="ef6d7-139">Se recomienda que las aplicaciones usen uno de los siguientes métodos de representación asincrónica:</span><span class="sxs-lookup"><span data-stu-id="ef6d7-139">We recommend that apps use one of the following asynchronous rendering approaches:</span></span>

* [<span data-ttu-id="ef6d7-140">Asistente de etiquetas parciales</span><span class="sxs-lookup"><span data-stu-id="ef6d7-140">Partial Tag Helper</span></span>](#partial-tag-helper)
* [<span data-ttu-id="ef6d7-141">Asistente de HTML asincrónico</span><span class="sxs-lookup"><span data-stu-id="ef6d7-141">Asynchronous HTML Helper</span></span>](#asynchronous-html-helper)

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="ef6d7-142">Dentro de un archivo de marcado, hay dos formas de hacer referencia a una vista parcial:</span><span class="sxs-lookup"><span data-stu-id="ef6d7-142">Within a markup file, there are two ways to reference a partial view:</span></span>

* [<span data-ttu-id="ef6d7-143">Asistente de HTML asincrónico</span><span class="sxs-lookup"><span data-stu-id="ef6d7-143">Asynchronous HTML Helper</span></span>](#asynchronous-html-helper)
* [<span data-ttu-id="ef6d7-144">Asistente de HTML sincrónico</span><span class="sxs-lookup"><span data-stu-id="ef6d7-144">Synchronous HTML Helper</span></span>](#synchronous-html-helper)

<span data-ttu-id="ef6d7-145">Se recomienda que las aplicaciones usen el [Asistente de HTML asincrónico](#asynchronous-html-helper).</span><span class="sxs-lookup"><span data-stu-id="ef6d7-145">We recommend that apps use the [Asynchronous HTML Helper](#asynchronous-html-helper).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="partial-tag-helper"></a><span data-ttu-id="ef6d7-146">Asistente de etiquetas parciales</span><span class="sxs-lookup"><span data-stu-id="ef6d7-146">Partial Tag Helper</span></span>

<span data-ttu-id="ef6d7-147">El [asistente de etiquetas parciales](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper) requiere ASP.NET Core 2.1 o posterior.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-147">The [Partial Tag Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper) requires ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="ef6d7-148">El asistente de etiquetas parciales representa contenido de forma asincrónica y usa una sintaxis similar a HTML:</span><span class="sxs-lookup"><span data-stu-id="ef6d7-148">The Partial Tag Helper renders content asynchronously and uses an HTML-like syntax:</span></span>

```cshtml
<partial name="_PartialName" />
```

<span data-ttu-id="ef6d7-149">Cuando hay una extensión de archivo, el asistente de etiquetas hace referencia a una vista parcial que debe estar en la misma carpeta que el archivo de marcado que llama a la vista parcial:</span><span class="sxs-lookup"><span data-stu-id="ef6d7-149">When a file extension is present, the Tag Helper references a partial view that must be in the same folder as the markup file calling the partial view:</span></span>

```cshtml
<partial name="_PartialName.cshtml" />
```

<span data-ttu-id="ef6d7-150">En el ejemplo siguiente se hace referencia a una vista parcial desde la raíz de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-150">The following example references a partial view from the app root.</span></span> <span data-ttu-id="ef6d7-151">Las rutas de acceso que comienzan con una tilde de la ñ y una barra diagonal (`~/`) o una barra diagonal (`/`) hacen referencia a la raíz de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="ef6d7-151">Paths that start with a tilde-slash (`~/`) or a slash (`/`) refer to the app root:</span></span>

<span data-ttu-id="ef6d7-152">**Páginas de Razor**</span><span class="sxs-lookup"><span data-stu-id="ef6d7-152">**Razor Pages**</span></span>

```cshtml
<partial name="~/Pages/Folder/_PartialName.cshtml" />
<partial name="/Pages/Folder/_PartialName.cshtml" />
```

<span data-ttu-id="ef6d7-153">**MVC**</span><span class="sxs-lookup"><span data-stu-id="ef6d7-153">**MVC**</span></span>

```cshtml
<partial name="~/Views/Folder/_PartialName.cshtml" />
<partial name="/Views/Folder/_PartialName.cshtml" />
```

<span data-ttu-id="ef6d7-154">En el ejemplo siguiente se hace referencia a una vista parcial con una ruta de acceso relativa:</span><span class="sxs-lookup"><span data-stu-id="ef6d7-154">The following example references a partial view with a relative path:</span></span>

```cshtml
<partial name="../Account/_PartialName.cshtml" />
```

<span data-ttu-id="ef6d7-155">Para obtener más información, vea <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-155">For more information, see <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>.</span></span>

::: moniker-end

### <a name="asynchronous-html-helper"></a><span data-ttu-id="ef6d7-156">Asistente de HTML asincrónica</span><span class="sxs-lookup"><span data-stu-id="ef6d7-156">Asynchronous HTML Helper</span></span>

<span data-ttu-id="ef6d7-157">Cuando se usa un asistente de HTML, el procedimiento recomendado es usar <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.PartialAsync*>.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-157">When using an HTML Helper, the best practice is to use <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.PartialAsync*>.</span></span> <span data-ttu-id="ef6d7-158">`PartialAsync` devuelve un tipo <xref:Microsoft.AspNetCore.Html.IHtmlContent> encapsulado en una clase <xref:System.Threading.Tasks.Task`1>.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-158">`PartialAsync` returns an <xref:Microsoft.AspNetCore.Html.IHtmlContent> type wrapped in a <xref:System.Threading.Tasks.Task`1>.</span></span> <span data-ttu-id="ef6d7-159">Para hacer referencia al método, se agrega a la llamada awaited un carácter `@` como prefijo:</span><span class="sxs-lookup"><span data-stu-id="ef6d7-159">The method is referenced by prefixing the awaited call with an `@` character:</span></span>

```cshtml
@await Html.PartialAsync("_PartialName")
```

<span data-ttu-id="ef6d7-160">Cuando la extensión del archivo está presente, el asistente de HTML hace referencia a una vista parcial que debe estar en la misma carpeta que el archivo de marcado que llama a la vista parcial:</span><span class="sxs-lookup"><span data-stu-id="ef6d7-160">When the file extension is present, the HTML Helper references a partial view that must be in the same folder as the markup file calling the partial view:</span></span>

```cshtml
@await Html.PartialAsync("_PartialName.cshtml")
```

<span data-ttu-id="ef6d7-161">En el ejemplo siguiente se hace referencia a una vista parcial desde la raíz de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-161">The following example references a partial view from the app root.</span></span> <span data-ttu-id="ef6d7-162">Las rutas de acceso que comienzan con una tilde de la ñ y una barra diagonal (`~/`) o una barra diagonal (`/`) hacen referencia a la raíz de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="ef6d7-162">Paths that start with a tilde-slash (`~/`) or a slash (`/`) refer to the app root:</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="ef6d7-163">**Páginas de Razor**</span><span class="sxs-lookup"><span data-stu-id="ef6d7-163">**Razor Pages**</span></span>

```cshtml
@await Html.PartialAsync("~/Pages/Folder/_PartialName.cshtml")
@await Html.PartialAsync("/Pages/Folder/_PartialName.cshtml")
```

<span data-ttu-id="ef6d7-164">**MVC**</span><span class="sxs-lookup"><span data-stu-id="ef6d7-164">**MVC**</span></span>

::: moniker-end

```cshtml
@await Html.PartialAsync("~/Views/Folder/_PartialName.cshtml")
@await Html.PartialAsync("/Views/Folder/_PartialName.cshtml")
```

<span data-ttu-id="ef6d7-165">En el ejemplo siguiente se hace referencia a una vista parcial con una ruta de acceso relativa:</span><span class="sxs-lookup"><span data-stu-id="ef6d7-165">The following example references a partial view with a relative path:</span></span>

```cshtml
@await Html.PartialAsync("../Account/_LoginPartial.cshtml")
```

<span data-ttu-id="ef6d7-166">También puede representar una vista parcial con <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartialAsync*>.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-166">Alternatively, you can render a partial view with <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartialAsync*>.</span></span> <span data-ttu-id="ef6d7-167">Este método no devuelve <xref:Microsoft.AspNetCore.Html.IHtmlContent>.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-167">This method doesn't return an <xref:Microsoft.AspNetCore.Html.IHtmlContent>.</span></span> <span data-ttu-id="ef6d7-168">sino que transmite por secuencias la salida representada directamente a la respuesta.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-168">It streams the rendered output directly to the response.</span></span> <span data-ttu-id="ef6d7-169">Como el método no devuelve ningún resultado, debe llamarse desde un bloque de código de Razor:</span><span class="sxs-lookup"><span data-stu-id="ef6d7-169">Because the method doesn't return a result, it must be called within a Razor code block:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_RenderPartialAsync)]

<span data-ttu-id="ef6d7-170">Puesto que `RenderPartialAsync` transmite contenido representado, ofrece mayor rendimiento en algunos escenarios.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-170">Since `RenderPartialAsync` streams rendered content, it provides better performance in some scenarios.</span></span> <span data-ttu-id="ef6d7-171">En situaciones críticas de rendimiento, realice pruebas comparativas de la página con ambos métodos y use el que genera una respuesta más rápida.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-171">In performance-critical situations, benchmark the page using both approaches and use the approach that generates a faster response.</span></span>

### <a name="synchronous-html-helper"></a><span data-ttu-id="ef6d7-172">Asistente de HTML sincrónico</span><span class="sxs-lookup"><span data-stu-id="ef6d7-172">Synchronous HTML Helper</span></span>

<span data-ttu-id="ef6d7-173"><xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.Partial*> y <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartial*> son los equivalentes sincrónicos de `PartialAsync` y `RenderPartialAsync`, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-173"><xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.Partial*> and <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartial*> are the synchronous equivalents of `PartialAsync` and `RenderPartialAsync`, respectively.</span></span> <span data-ttu-id="ef6d7-174">Los equivalentes sincrónicos no son aconsejables, ya que hay escenarios donde producen interbloqueos.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-174">The synchronous equivalents aren't recommended because there are scenarios in which they deadlock.</span></span> <span data-ttu-id="ef6d7-175">Se prevé la eliminación de los métodos sincrónicos en una futura versión.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-175">The synchronous methods are targeted for removal in a future release.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ef6d7-176">Si no necesita ejecutar código, use un [componente de vista](xref:mvc/views/view-components) en lugar de una vista parcial.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-176">If you need to execute code, use a [view component](xref:mvc/views/view-components) instead of a partial view.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="ef6d7-177">La llamada a `Partial` o `RenderPartial` resulta en una advertencia del analizador de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-177">Calling `Partial` or `RenderPartial` results in a Visual Studio analyzer warning.</span></span> <span data-ttu-id="ef6d7-178">Por ejemplo, la presencia de `Partial` genera el siguiente mensaje de advertencia:</span><span class="sxs-lookup"><span data-stu-id="ef6d7-178">For example, the presence of `Partial` yields the following warning message:</span></span>

> <span data-ttu-id="ef6d7-179">Use of IHtmlHelper.Partial may result in application deadlocks.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-179">Use of IHtmlHelper.Partial may result in application deadlocks.</span></span> <span data-ttu-id="ef6d7-180">Considere la posibilidad de utilizar el asistente de etiquetas &lt;parciales&gt; o IHtmlHelper.PartialAsync.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-180">Consider using &lt;partial&gt; Tag Helper or IHtmlHelper.PartialAsync.</span></span>

<span data-ttu-id="ef6d7-181">Reemplace las llamadas a `@Html.Partial` por `@await Html.PartialAsync` o el [asistente de etiquetas parciales](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="ef6d7-181">Replace calls to `@Html.Partial` with `@await Html.PartialAsync` or the [Partial Tag Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span></span> <span data-ttu-id="ef6d7-182">Para más información sobre la migración del asistente de etiquetas parciales, vea [Migración desde un asistente de HTML](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper#migrate-from-an-html-helper).</span><span class="sxs-lookup"><span data-stu-id="ef6d7-182">For more information on Partial Tag Helper migration, see [Migrate from an HTML Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper#migrate-from-an-html-helper).</span></span>

::: moniker-end

## <a name="partial-view-discovery"></a><span data-ttu-id="ef6d7-183">Detección de vistas parciales</span><span class="sxs-lookup"><span data-stu-id="ef6d7-183">Partial view discovery</span></span>

<span data-ttu-id="ef6d7-184">Cuando se hace referencia a una vista parcial por su nombre sin una extensión de archivo, se busca en las siguientes ubicaciones en el orden indicado:</span><span class="sxs-lookup"><span data-stu-id="ef6d7-184">When a partial view is referenced by name without a file extension, the following locations are searched in the stated order:</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="ef6d7-185">**Páginas de Razor**</span><span class="sxs-lookup"><span data-stu-id="ef6d7-185">**Razor Pages**</span></span>

1. <span data-ttu-id="ef6d7-186">Carpeta de la página en ejecución actualmente</span><span class="sxs-lookup"><span data-stu-id="ef6d7-186">Currently executing page's folder</span></span>
1. <span data-ttu-id="ef6d7-187">Gráfico de directorio por encima de la carpeta de la página</span><span class="sxs-lookup"><span data-stu-id="ef6d7-187">Directory graph above the page's folder</span></span>
1. `/Shared`
1. `/Pages/Shared`
1. `/Views/Shared`

<span data-ttu-id="ef6d7-188">**MVC**</span><span class="sxs-lookup"><span data-stu-id="ef6d7-188">**MVC**</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

1. `/Areas/<Area-Name>/Views/<Controller-Name>`
1. `/Areas/<Area-Name>/Views/Shared`
1. `/Views/Shared`
1. `/Pages/Shared`

::: moniker-end

::: moniker range="< aspnetcore-2.0"

1. `/Areas/<Area-Name>/Views/<Controller-Name>`
1. `/Areas/<Area-Name>/Views/Shared`
1. `/Views/Shared`

::: moniker-end

<span data-ttu-id="ef6d7-189">Las convenciones siguientes se aplican a la detección de la vista parcial:</span><span class="sxs-lookup"><span data-stu-id="ef6d7-189">The following conventions apply to partial view discovery:</span></span>

* <span data-ttu-id="ef6d7-190">Se admiten diferentes vistas parciales con el mismo nombre de archivo cuando las vistas parciales están en carpetas diferentes.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-190">Different partial views with the same file name are allowed when the partial views are in different folders.</span></span>
* <span data-ttu-id="ef6d7-191">Al hacer referencia a una vista parcial por su nombre sin una extensión de archivo y la vista parcial está presente tanto en la carpeta del autor de la llamada como en la carpeta *compartida*, la vista parcial de la carpeta del autor de la llamada proporciona la vista parcial.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-191">When referencing a partial view by name without a file extension and the partial view is present in both the caller's folder and the *Shared* folder, the partial view in the caller's folder supplies the partial view.</span></span> <span data-ttu-id="ef6d7-192">Si la vista parcial no está presente en la carpeta del autor de la llamada, se proporciona la vista parcial desde la carpeta *compartida*.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-192">If the partial view isn't present in the caller's folder, the partial view is provided from the *Shared* folder.</span></span> <span data-ttu-id="ef6d7-193">Las vistas parciales de la carpeta *compartida* se denominan *vistas parciales compartidas* o *vistas parciales predeterminadas*.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-193">Partial views in the *Shared* folder are called *shared partial views* or *default partial views*.</span></span>
* <span data-ttu-id="ef6d7-194">Las vistas parciales pueden estar *encadenadas*: una vista parcial puede llamar a otra vista parcial si las llamadas no forman una referencia circular.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-194">Partial views can be *chained*&mdash;a partial view can call another partial view if a circular reference isn't formed by the calls.</span></span> <span data-ttu-id="ef6d7-195">Las rutas de acceso relativas siempre guardan relación con el archivo actual, no con la raíz ni el elemento primario del archivo.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-195">Relative paths are always relative to the current file, not to the root or parent of the file.</span></span>

> [!NOTE]
> <span data-ttu-id="ef6d7-196">Una clase `section` de [Razor](xref:mvc/views/razor) definida en una vista parcial no es visible para los archivos de marcado principales.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-196">A [Razor](xref:mvc/views/razor) `section` defined in a partial view is invisible to parent markup files.</span></span> <span data-ttu-id="ef6d7-197">La `section` solo es visible para la vista parcial en la que está definida.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-197">The `section` is only visible to the partial view in which it's defined.</span></span>

## <a name="access-data-from-partial-views"></a><span data-ttu-id="ef6d7-198">Acceso a datos desde vistas parciales</span><span class="sxs-lookup"><span data-stu-id="ef6d7-198">Access data from partial views</span></span>

<span data-ttu-id="ef6d7-199">Cuando se crea una instancia de una vista parcial, recibe una *copia* del diccionario `ViewData` del elemento primario.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-199">When a partial view is instantiated, it receives a *copy* of the parent's `ViewData` dictionary.</span></span> <span data-ttu-id="ef6d7-200">Las actualizaciones realizadas en los datos dentro de la vista parcial no se conservan en la vista principal.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-200">Updates made to the data within the partial view aren't persisted to the parent view.</span></span> <span data-ttu-id="ef6d7-201">Los cambios de `ViewData` en una vista parcial se pierden cuando se devuelve la vista parcial.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-201">`ViewData` changes in a partial view are lost when the partial view returns.</span></span>

<span data-ttu-id="ef6d7-202">En el ejemplo siguiente se muestra cómo pasar una instancia de [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) a una vista parcial:</span><span class="sxs-lookup"><span data-stu-id="ef6d7-202">The following example demonstrates how to pass an instance of [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) to a partial view:</span></span>

```cshtml
@await Html.PartialAsync("_PartialName", customViewData)
```

<span data-ttu-id="ef6d7-203">Puede pasar un modelo a una vista parcial.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-203">You can pass a model into a partial view.</span></span> <span data-ttu-id="ef6d7-204">El modelo puede ser un objeto personalizado.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-204">The model can be a custom object.</span></span> <span data-ttu-id="ef6d7-205">Puede pasar un modelo con `PartialAsync` (representa un bloque de contenido al autor de la llamada) o `RenderPartialAsync` (transmite el contenido a la salida):</span><span class="sxs-lookup"><span data-stu-id="ef6d7-205">You can pass a model with `PartialAsync` (renders a block of content to the caller) or `RenderPartialAsync` (streams the content to the output):</span></span>

```cshtml
@await Html.PartialAsync("_PartialName", model)
```

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="ef6d7-206">**Páginas de Razor**</span><span class="sxs-lookup"><span data-stu-id="ef6d7-206">**Razor Pages**</span></span>

<span data-ttu-id="ef6d7-207">El siguiente marcado de la aplicación de ejemplo proviene de la página *Pages/ArticlesRP/ReadRP.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-207">The following markup in the sample app is from the *Pages/ArticlesRP/ReadRP.cshtml* page.</span></span> <span data-ttu-id="ef6d7-208">La página contiene dos vistas parciales.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-208">The page contains two partial views.</span></span> <span data-ttu-id="ef6d7-209">La segunda vista parcial se pasa a un modelo y `ViewData` a la vista parcial.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-209">The second partial view passes in a model and `ViewData` to the partial view.</span></span> <span data-ttu-id="ef6d7-210">La sobrecarga del constructor de `ViewDataDictionary` se usa para pasar un nuevo diccionario `ViewData` a la vez que conserva el diccionario `ViewData` existente.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-210">The `ViewDataDictionary` constructor overload is used to pass a new `ViewData` dictionary while retaining the existing `ViewData` dictionary.</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Pages/ArticlesRP/ReadRP.cshtml?name=snippet_ReadPartialViewRP&highlight=5,15-19)]

<span data-ttu-id="ef6d7-211">*Pages/Shared/_AuthorPartialRP.cshtml* es la primera vista parcial a la que hace referencia el archivo de marcado *ReadRP.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ef6d7-211">*Pages/Shared/_AuthorPartialRP.cshtml* is the first partial view referenced by the *ReadRP.cshtml* markup file:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Pages/Shared/_AuthorPartialRP.cshtml)]

<span data-ttu-id="ef6d7-212">*Pages/ArticlesRP/_ArticleSectionRP.cshtml* es la segunda vista parcial a la que hace referencia el archivo de marcado *ReadRP.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ef6d7-212">*Pages/ArticlesRP/_ArticleSectionRP.cshtml* is the second partial view referenced by the *ReadRP.cshtml* markup file:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Pages/ArticlesRP/_ArticleSectionRP.cshtml)]

<span data-ttu-id="ef6d7-213">**MVC**</span><span class="sxs-lookup"><span data-stu-id="ef6d7-213">**MVC**</span></span>

::: moniker-end

<span data-ttu-id="ef6d7-214">El marcado siguiente de la aplicación de ejemplo muestra la vista *Views/Articles/Read.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-214">The following markup in the sample app shows the *Views/Articles/Read.cshtml* view.</span></span> <span data-ttu-id="ef6d7-215">La vista contiene dos vistas parciales.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-215">The view contains two partial views.</span></span> <span data-ttu-id="ef6d7-216">La segunda vista parcial se pasa a un modelo y `ViewData` a la vista parcial.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-216">The second partial view passes in a model and `ViewData` to the partial view.</span></span> <span data-ttu-id="ef6d7-217">La sobrecarga del constructor de `ViewDataDictionary` se usa para pasar un nuevo diccionario `ViewData` a la vez que conserva el diccionario `ViewData` existente.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-217">The `ViewDataDictionary` constructor overload is used to pass a new `ViewData` dictionary while retaining the existing `ViewData` dictionary.</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/Read.cshtml?name=snippet_ReadPartialView&highlight=5,15-19)]

<span data-ttu-id="ef6d7-218">*Views/Shared/_AuthorPartial.cshtml* es la primera vista parcial a la que hace referencia el archivo de marcado *ReadRP.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ef6d7-218">*Views/Shared/_AuthorPartial.cshtml* is the first partial view referenced by the *ReadRP.cshtml* markup file:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Shared/_AuthorPartial.cshtml)]

<span data-ttu-id="ef6d7-219">*Views/Articles/_ArticleSection.cshtml* es la segunda vista parcial a la que hace referencia el archivo de marcado *Read.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="ef6d7-219">*Views/Articles/_ArticleSection.cshtml* is the second partial view referenced by the *Read.cshtml* markup file:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/_ArticleSection.cshtml)]

<span data-ttu-id="ef6d7-220">En tiempo de ejecución, las vistas parciales se representan en la salida representada del archivo de marcado principal, que a su vez se representa dentro de *_Layout.cshtml* compartido.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-220">At runtime, the partials are rendered into the parent markup file's rendered output, which itself is rendered within the shared *_Layout.cshtml*.</span></span> <span data-ttu-id="ef6d7-221">La primera vista parcial representa la fecha de publicación y el nombre del autor del artículo:</span><span class="sxs-lookup"><span data-stu-id="ef6d7-221">The first partial view renders the article author's name and publication date:</span></span>

> <span data-ttu-id="ef6d7-222">Abraham Lincoln</span><span class="sxs-lookup"><span data-stu-id="ef6d7-222">Abraham Lincoln</span></span>
>
> <span data-ttu-id="ef6d7-223">Esta vista parcial procede de la &lt;ruta de acceso de archivo de la vista parcial compartida&gt;.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-223">This partial view from &lt;shared partial view file path&gt;.</span></span>
> <span data-ttu-id="ef6d7-224">19/11/1863 12:00:00 a. m.</span><span class="sxs-lookup"><span data-stu-id="ef6d7-224">11/19/1863 12:00:00 AM</span></span>

<span data-ttu-id="ef6d7-225">La segunda vista parcial representa las secciones del artículo:</span><span class="sxs-lookup"><span data-stu-id="ef6d7-225">The second partial view renders the article's sections:</span></span>

> <span data-ttu-id="ef6d7-226">Índice de la sección uno: 0</span><span class="sxs-lookup"><span data-stu-id="ef6d7-226">Section One Index: 0</span></span>
>
> <span data-ttu-id="ef6d7-227">Puntuación de cuatro y hace siete años...</span><span class="sxs-lookup"><span data-stu-id="ef6d7-227">Four score and seven years ago ...</span></span>
>
> <span data-ttu-id="ef6d7-228">Índice de la sección dos: 1</span><span class="sxs-lookup"><span data-stu-id="ef6d7-228">Section Two Index: 1</span></span>
>
> <span data-ttu-id="ef6d7-229">Ahora nos encontramos en una verdadera guerra civil, probando...</span><span class="sxs-lookup"><span data-stu-id="ef6d7-229">Now we are engaged in a great civil war, testing ...</span></span>
>
> <span data-ttu-id="ef6d7-230">Índice de la sección tres: 2</span><span class="sxs-lookup"><span data-stu-id="ef6d7-230">Section Three Index: 2</span></span>
>
> <span data-ttu-id="ef6d7-231">Pero, en un sentido amplio, no nos podemos dedicar...</span><span class="sxs-lookup"><span data-stu-id="ef6d7-231">But, in a larger sense, we can not dedicate ...</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ef6d7-232">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="ef6d7-232">Additional resources</span></span>

::: moniker range=">= aspnetcore-2.1"

* <xref:mvc/views/razor>
* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>
* <xref:mvc/views/view-components>
* <xref:mvc/controllers/areas>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

* <xref:mvc/views/razor>
* <xref:mvc/views/view-components>
* <xref:mvc/controllers/areas>

::: moniker-end
