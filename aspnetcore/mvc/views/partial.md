---
title: Vistas parciales en ASP.NET Core
author: ardalis
description: Descubra cómo usar las vistas parciales para dividir los archivos de marcado de gran tamaño y reducir la duplicación de marcado común en las páginas web en aplicaciones ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 06/12/2019
uid: mvc/views/partial
ms.openlocfilehash: 901fd52f89969141713e443890781a77308bd901
ms.sourcegitcommit: 335a88c1b6e7f0caa8a3a27db57c56664d676d34
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/12/2019
ms.locfileid: "67034916"
---
# <a name="partial-views-in-aspnet-core"></a><span data-ttu-id="d80ce-103">Vistas parciales en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d80ce-103">Partial views in ASP.NET Core</span></span>

<span data-ttu-id="d80ce-104">Por [Steve Smith](https://ardalis.com/), [Luke Latham](https://github.com/guardrex), [Maher JENDOUBI](https://twitter.com/maherjend), [Rick Anderson](https://twitter.com/RickAndMSFT) y [Scott Sauber](https://twitter.com/scottsauber)</span><span class="sxs-lookup"><span data-stu-id="d80ce-104">By [Steve Smith](https://ardalis.com/), [Luke Latham](https://github.com/guardrex), [Maher JENDOUBI](https://twitter.com/maherjend), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Scott Sauber](https://twitter.com/scottsauber)</span></span>

<span data-ttu-id="d80ce-105">Una vista parcial es un archivo de marcado [Razor](xref:mvc/views/razor) (*.cshtml*) que representa una salida HTML *dentro* de otra salida representada del archivo de marcado.</span><span class="sxs-lookup"><span data-stu-id="d80ce-105">A partial view is a [Razor](xref:mvc/views/razor) markup file (*.cshtml*) that renders HTML output *within* another markup file's rendered output.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="d80ce-106">El término *vista parcial* se usa al desarrollar una aplicación MVC, donde a los archivos de marcado se les denomina *vistas*, o una aplicación de Razor Pages, donde a los archivos de marcado se les denomina *páginas*.</span><span class="sxs-lookup"><span data-stu-id="d80ce-106">The term *partial view* is used when developing either an MVC app, where markup files are called *views*, or a Razor Pages app, where markup files are called *pages*.</span></span> <span data-ttu-id="d80ce-107">En este tema se hace referencia genéricamente a las vistas de MVC y a las páginas de Razor Pages como *archivos de marcado*.</span><span class="sxs-lookup"><span data-stu-id="d80ce-107">This topic generically refers to MVC views and Razor Pages pages as *markup files*.</span></span>

::: moniker-end

<span data-ttu-id="d80ce-108">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/partial/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d80ce-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/partial/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-partial-views"></a><span data-ttu-id="d80ce-109">¿Cuándo se usan las vistas parciales?</span><span class="sxs-lookup"><span data-stu-id="d80ce-109">When to use partial views</span></span>

<span data-ttu-id="d80ce-110">Las vistas parciales son una forma eficaz de:</span><span class="sxs-lookup"><span data-stu-id="d80ce-110">Partial views are an effective way to:</span></span>

* <span data-ttu-id="d80ce-111">Dividir los archivos de marcado grandes en componentes más pequeños.</span><span class="sxs-lookup"><span data-stu-id="d80ce-111">Break up large markup files into smaller components.</span></span>

  <span data-ttu-id="d80ce-112">En un gran archivo de marcado complejo formado por varias partes lógicas, se ofrece la ventaja de trabajar con cada pieza aislada en una vista parcial.</span><span class="sxs-lookup"><span data-stu-id="d80ce-112">In a large, complex markup file composed of several logical pieces, there's an advantage to working with each piece isolated into a partial view.</span></span> <span data-ttu-id="d80ce-113">El código del archivo de marcado es fácil de administrar porque el marcado solo contiene la estructura general de la página y las referencias a las vistas parciales.</span><span class="sxs-lookup"><span data-stu-id="d80ce-113">The code in the markup file is manageable because the markup only contains the overall page structure and references to partial views.</span></span>
* <span data-ttu-id="d80ce-114">Reducir la duplicación de contenido de marcado común en todos los archivos de marcado.</span><span class="sxs-lookup"><span data-stu-id="d80ce-114">Reduce the duplication of common markup content across markup files.</span></span>

  <span data-ttu-id="d80ce-115">Cuando se usan los mismos elementos de marcado en todos los archivos de marcado, una vista parcial elimina la duplicación del contenido de marcado en un archivo de vista parcial.</span><span class="sxs-lookup"><span data-stu-id="d80ce-115">When the same markup elements are used across markup files, a partial view removes the duplication of markup content into one partial view file.</span></span> <span data-ttu-id="d80ce-116">Cuando se cambia el marcado en la vista parcial, actualiza la salida representada de los archivos de marcado que utilizan la vista parcial.</span><span class="sxs-lookup"><span data-stu-id="d80ce-116">When the markup is changed in the partial view, it updates the rendered output of the markup files that use the partial view.</span></span>

<span data-ttu-id="d80ce-117">Las vistas parciales no deben utilizarse para mantener los elementos de diseño comunes.</span><span class="sxs-lookup"><span data-stu-id="d80ce-117">Partial views shouldn't be used to maintain common layout elements.</span></span> <span data-ttu-id="d80ce-118">Se deben especificar elementos de diseño comunes en los archivos [_Layout.cshtml](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="d80ce-118">Common layout elements should be specified in [_Layout.cshtml](xref:mvc/views/layout) files.</span></span>

<span data-ttu-id="d80ce-119">No utilice una vista parcial donde se requiera la ejecución de código o lógica de representación compleja para representar el marcado.</span><span class="sxs-lookup"><span data-stu-id="d80ce-119">Don't use a partial view where complex rendering logic or code execution is required to render the markup.</span></span> <span data-ttu-id="d80ce-120">En lugar de una vista parcial, use un [componente de vista](xref:mvc/views/view-components).</span><span class="sxs-lookup"><span data-stu-id="d80ce-120">Instead of a partial view, use a [view component](xref:mvc/views/view-components).</span></span>

## <a name="declare-partial-views"></a><span data-ttu-id="d80ce-121">Declaración de vistas parciales</span><span class="sxs-lookup"><span data-stu-id="d80ce-121">Declare partial views</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d80ce-122">Una vista parcial es un archivo de marcado *.cshtml* que se mantiene dentro de la carpeta *Vistas* (MVC) o *Páginas* (Razor Pages).</span><span class="sxs-lookup"><span data-stu-id="d80ce-122">A partial view is a *.cshtml* markup file maintained within the *Views* folder (MVC) or *Pages* folder (Razor Pages).</span></span>

<span data-ttu-id="d80ce-123">En ASP.NET Core MVC, la clase <xref:Microsoft.AspNetCore.Mvc.ViewResult> de un controlador es capaz de devolver una vista o una vista parcial.</span><span class="sxs-lookup"><span data-stu-id="d80ce-123">In ASP.NET Core MVC, a controller's <xref:Microsoft.AspNetCore.Mvc.ViewResult> is capable of returning either a view or a partial view.</span></span> <span data-ttu-id="d80ce-124">En Razor Pages, un elemento <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> puede devolver una vista parcial representada como un objeto <xref:Microsoft.AspNetCore.Mvc.PartialViewResult>.</span><span class="sxs-lookup"><span data-stu-id="d80ce-124">In Razor Pages, a <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> can return a partial view represented as a <xref:Microsoft.AspNetCore.Mvc.PartialViewResult> object.</span></span> <span data-ttu-id="d80ce-125">La referencia y representación de vistas parciales se describe en la sección [Referencia a una vista parcial](#reference-a-partial-view).</span><span class="sxs-lookup"><span data-stu-id="d80ce-125">Referencing and rendering partial views is described in the [Reference a partial view](#reference-a-partial-view) section.</span></span>

<span data-ttu-id="d80ce-126">A diferencia de la representación de páginas o vistas de MVC, una vista parcial no ejecuta *_ViewStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="d80ce-126">Unlike MVC view or page rendering, a partial view doesn't run *_ViewStart.cshtml*.</span></span> <span data-ttu-id="d80ce-127">Para más información sobre *_ViewStart.cshtml*, vea <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="d80ce-127">For more information on *_ViewStart.cshtml*, see <xref:mvc/views/layout>.</span></span>

<span data-ttu-id="d80ce-128">Los nombres de archivo de las vistas parciales suelen comenzar con un guión bajo (`_`).</span><span class="sxs-lookup"><span data-stu-id="d80ce-128">Partial view file names often begin with an underscore (`_`).</span></span> <span data-ttu-id="d80ce-129">Esta convención de nomenclatura no es obligatoria, pero ayuda a diferenciar visualmente las vistas parciales de las vistas y las páginas.</span><span class="sxs-lookup"><span data-stu-id="d80ce-129">This naming convention isn't required, but it helps to visually differentiate partial views from views and pages.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="d80ce-130">Una vista parcial es un archivo de marcado *.cshtml* que se mantiene dentro de la carpeta *Vistas*.</span><span class="sxs-lookup"><span data-stu-id="d80ce-130">A partial view is a *.cshtml* markup file maintained within the *Views* folder.</span></span>

<span data-ttu-id="d80ce-131">La clase <xref:Microsoft.AspNetCore.Mvc.ViewResult> de un controlador es capaz de devolver una vista o una vista parcial.</span><span class="sxs-lookup"><span data-stu-id="d80ce-131">A controller's <xref:Microsoft.AspNetCore.Mvc.ViewResult> is capable of returning either a view or a partial view.</span></span> <span data-ttu-id="d80ce-132">La referencia y representación de vistas parciales se describe en la sección [Referencia a una vista parcial](#reference-a-partial-view).</span><span class="sxs-lookup"><span data-stu-id="d80ce-132">Referencing and rendering partial views is described in the [Reference a partial view](#reference-a-partial-view) section.</span></span>

<span data-ttu-id="d80ce-133">A diferencia de la representación de vistas de MVC, una vista parcial no ejecuta *_ViewStart.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="d80ce-133">Unlike MVC view rendering, a partial view doesn't run *_ViewStart.cshtml*.</span></span> <span data-ttu-id="d80ce-134">Para más información sobre *_ViewStart.cshtml*, vea <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="d80ce-134">For more information on *_ViewStart.cshtml*, see <xref:mvc/views/layout>.</span></span>

<span data-ttu-id="d80ce-135">Los nombres de archivo de las vistas parciales suelen comenzar con un guión bajo (`_`).</span><span class="sxs-lookup"><span data-stu-id="d80ce-135">Partial view file names often begin with an underscore (`_`).</span></span> <span data-ttu-id="d80ce-136">Esta convención de nomenclatura no es obligatoria, pero ayuda a diferenciar visualmente las vistas parciales de las vistas.</span><span class="sxs-lookup"><span data-stu-id="d80ce-136">This naming convention isn't required, but it helps to visually differentiate partial views from views.</span></span>

::: moniker-end

## <a name="reference-a-partial-view"></a><span data-ttu-id="d80ce-137">Referencia a una vista parcial</span><span class="sxs-lookup"><span data-stu-id="d80ce-137">Reference a partial view</span></span>

::: moniker range=">= aspnetcore-2.0"

### <a name="use-a-partial-view-in-a-razor-pages-pagemodel"></a><span data-ttu-id="d80ce-138">Uso de una vista parcial en una clase PageModel de Razor Pages</span><span class="sxs-lookup"><span data-stu-id="d80ce-138">Use a partial view in a Razor Pages PageModel</span></span>

<span data-ttu-id="d80ce-139">En ASP.NET Core 2.0 o 2.1, el método de controlador siguiente representa la vista parcial *\_AuthorPartialRP.cshtml* en la respuesta:</span><span class="sxs-lookup"><span data-stu-id="d80ce-139">In ASP.NET Core 2.0 or 2.1, the following handler method renders the *\_AuthorPartialRP.cshtml* partial view to the response:</span></span>

```csharp
public IActionResult OnGetPartial() =>
    new PartialViewResult
    {
        ViewName = "_AuthorPartialRP",
        ViewData = ViewData,
    };
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="d80ce-140">En ASP.NET Core 2.2 o posterior, un método de controlador puede llamar de forma alternativa al método <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageBase.Partial*> para generar un objeto `PartialViewResult`:</span><span class="sxs-lookup"><span data-stu-id="d80ce-140">In ASP.NET Core 2.2 or later, a handler method can alternatively call the <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageBase.Partial*> method to produce a `PartialViewResult` object:</span></span>

[!code-csharp[](partial/sample/PartialViewsSample/Pages/DiscoveryRP.cshtml.cs?name=snippet_OnGetPartial)]

::: moniker-end

### <a name="use-a-partial-view-in-a-markup-file"></a><span data-ttu-id="d80ce-141">Uso de una vista parcial en un archivo de marcado</span><span class="sxs-lookup"><span data-stu-id="d80ce-141">Use a partial view in a markup file</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="d80ce-142">Dentro de un archivo de marcado, hay varias maneras de hacer referencia a una vista parcial.</span><span class="sxs-lookup"><span data-stu-id="d80ce-142">Within a markup file, there are several ways to reference a partial view.</span></span> <span data-ttu-id="d80ce-143">Se recomienda que las aplicaciones usen uno de los siguientes métodos de representación asincrónica:</span><span class="sxs-lookup"><span data-stu-id="d80ce-143">We recommend that apps use one of the following asynchronous rendering approaches:</span></span>

* [<span data-ttu-id="d80ce-144">Asistente de etiquetas parciales</span><span class="sxs-lookup"><span data-stu-id="d80ce-144">Partial Tag Helper</span></span>](#partial-tag-helper)
* [<span data-ttu-id="d80ce-145">Asistente de HTML asincrónico</span><span class="sxs-lookup"><span data-stu-id="d80ce-145">Asynchronous HTML Helper</span></span>](#asynchronous-html-helper)

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="d80ce-146">Dentro de un archivo de marcado, hay dos formas de hacer referencia a una vista parcial:</span><span class="sxs-lookup"><span data-stu-id="d80ce-146">Within a markup file, there are two ways to reference a partial view:</span></span>

* [<span data-ttu-id="d80ce-147">Asistente de HTML asincrónico</span><span class="sxs-lookup"><span data-stu-id="d80ce-147">Asynchronous HTML Helper</span></span>](#asynchronous-html-helper)
* [<span data-ttu-id="d80ce-148">Asistente de HTML sincrónico</span><span class="sxs-lookup"><span data-stu-id="d80ce-148">Synchronous HTML Helper</span></span>](#synchronous-html-helper)

<span data-ttu-id="d80ce-149">Se recomienda que las aplicaciones usen el [Asistente de HTML asincrónico](#asynchronous-html-helper).</span><span class="sxs-lookup"><span data-stu-id="d80ce-149">We recommend that apps use the [Asynchronous HTML Helper](#asynchronous-html-helper).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="partial-tag-helper"></a><span data-ttu-id="d80ce-150">Asistente de etiquetas parciales</span><span class="sxs-lookup"><span data-stu-id="d80ce-150">Partial Tag Helper</span></span>

<span data-ttu-id="d80ce-151">El [asistente de etiquetas parciales](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper) requiere ASP.NET Core 2.1 o posterior.</span><span class="sxs-lookup"><span data-stu-id="d80ce-151">The [Partial Tag Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper) requires ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="d80ce-152">El asistente de etiquetas parciales representa contenido de forma asincrónica y usa una sintaxis similar a HTML:</span><span class="sxs-lookup"><span data-stu-id="d80ce-152">The Partial Tag Helper renders content asynchronously and uses an HTML-like syntax:</span></span>

```cshtml
<partial name="_PartialName" />
```

<span data-ttu-id="d80ce-153">Cuando hay una extensión de archivo, el asistente de etiquetas hace referencia a una vista parcial que debe estar en la misma carpeta que el archivo de marcado que llama a la vista parcial:</span><span class="sxs-lookup"><span data-stu-id="d80ce-153">When a file extension is present, the Tag Helper references a partial view that must be in the same folder as the markup file calling the partial view:</span></span>

```cshtml
<partial name="_PartialName.cshtml" />
```

<span data-ttu-id="d80ce-154">En el ejemplo siguiente se hace referencia a una vista parcial desde la raíz de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d80ce-154">The following example references a partial view from the app root.</span></span> <span data-ttu-id="d80ce-155">Las rutas de acceso que comienzan con una tilde de la ñ y una barra diagonal (`~/`) o una barra diagonal (`/`) hacen referencia a la raíz de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="d80ce-155">Paths that start with a tilde-slash (`~/`) or a slash (`/`) refer to the app root:</span></span>

<span data-ttu-id="d80ce-156">**Páginas de Razor**</span><span class="sxs-lookup"><span data-stu-id="d80ce-156">**Razor Pages**</span></span>

```cshtml
<partial name="~/Pages/Folder/_PartialName.cshtml" />
<partial name="/Pages/Folder/_PartialName.cshtml" />
```

<span data-ttu-id="d80ce-157">**MVC**</span><span class="sxs-lookup"><span data-stu-id="d80ce-157">**MVC**</span></span>

```cshtml
<partial name="~/Views/Folder/_PartialName.cshtml" />
<partial name="/Views/Folder/_PartialName.cshtml" />
```

<span data-ttu-id="d80ce-158">En el ejemplo siguiente se hace referencia a una vista parcial con una ruta de acceso relativa:</span><span class="sxs-lookup"><span data-stu-id="d80ce-158">The following example references a partial view with a relative path:</span></span>

```cshtml
<partial name="../Account/_PartialName.cshtml" />
```

<span data-ttu-id="d80ce-159">Para obtener más información, vea <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>.</span><span class="sxs-lookup"><span data-stu-id="d80ce-159">For more information, see <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>.</span></span>

::: moniker-end

### <a name="asynchronous-html-helper"></a><span data-ttu-id="d80ce-160">Asistente de HTML asincrónica</span><span class="sxs-lookup"><span data-stu-id="d80ce-160">Asynchronous HTML Helper</span></span>

<span data-ttu-id="d80ce-161">Cuando se usa un asistente de HTML, el procedimiento recomendado es usar <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.PartialAsync*>.</span><span class="sxs-lookup"><span data-stu-id="d80ce-161">When using an HTML Helper, the best practice is to use <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.PartialAsync*>.</span></span> <span data-ttu-id="d80ce-162">`PartialAsync` devuelve un tipo <xref:Microsoft.AspNetCore.Html.IHtmlContent> encapsulado en una clase <xref:System.Threading.Tasks.Task%601>.</span><span class="sxs-lookup"><span data-stu-id="d80ce-162">`PartialAsync` returns an <xref:Microsoft.AspNetCore.Html.IHtmlContent> type wrapped in a <xref:System.Threading.Tasks.Task%601>.</span></span> <span data-ttu-id="d80ce-163">Para hacer referencia al método, se agrega a la llamada awaited un carácter `@` como prefijo:</span><span class="sxs-lookup"><span data-stu-id="d80ce-163">The method is referenced by prefixing the awaited call with an `@` character:</span></span>

```cshtml
@await Html.PartialAsync("_PartialName")
```

<span data-ttu-id="d80ce-164">Cuando la extensión del archivo está presente, el asistente de HTML hace referencia a una vista parcial que debe estar en la misma carpeta que el archivo de marcado que llama a la vista parcial:</span><span class="sxs-lookup"><span data-stu-id="d80ce-164">When the file extension is present, the HTML Helper references a partial view that must be in the same folder as the markup file calling the partial view:</span></span>

```cshtml
@await Html.PartialAsync("_PartialName.cshtml")
```

<span data-ttu-id="d80ce-165">En el ejemplo siguiente se hace referencia a una vista parcial desde la raíz de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d80ce-165">The following example references a partial view from the app root.</span></span> <span data-ttu-id="d80ce-166">Las rutas de acceso que comienzan con una tilde de la ñ y una barra diagonal (`~/`) o una barra diagonal (`/`) hacen referencia a la raíz de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="d80ce-166">Paths that start with a tilde-slash (`~/`) or a slash (`/`) refer to the app root:</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="d80ce-167">**Páginas de Razor**</span><span class="sxs-lookup"><span data-stu-id="d80ce-167">**Razor Pages**</span></span>

```cshtml
@await Html.PartialAsync("~/Pages/Folder/_PartialName.cshtml")
@await Html.PartialAsync("/Pages/Folder/_PartialName.cshtml")
```

<span data-ttu-id="d80ce-168">**MVC**</span><span class="sxs-lookup"><span data-stu-id="d80ce-168">**MVC**</span></span>

::: moniker-end

```cshtml
@await Html.PartialAsync("~/Views/Folder/_PartialName.cshtml")
@await Html.PartialAsync("/Views/Folder/_PartialName.cshtml")
```

<span data-ttu-id="d80ce-169">En el ejemplo siguiente se hace referencia a una vista parcial con una ruta de acceso relativa:</span><span class="sxs-lookup"><span data-stu-id="d80ce-169">The following example references a partial view with a relative path:</span></span>

```cshtml
@await Html.PartialAsync("../Account/_LoginPartial.cshtml")
```

<span data-ttu-id="d80ce-170">También puede representar una vista parcial con <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartialAsync*>.</span><span class="sxs-lookup"><span data-stu-id="d80ce-170">Alternatively, you can render a partial view with <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartialAsync*>.</span></span> <span data-ttu-id="d80ce-171">Este método no devuelve <xref:Microsoft.AspNetCore.Html.IHtmlContent>.</span><span class="sxs-lookup"><span data-stu-id="d80ce-171">This method doesn't return an <xref:Microsoft.AspNetCore.Html.IHtmlContent>.</span></span> <span data-ttu-id="d80ce-172">sino que transmite por secuencias la salida representada directamente a la respuesta.</span><span class="sxs-lookup"><span data-stu-id="d80ce-172">It streams the rendered output directly to the response.</span></span> <span data-ttu-id="d80ce-173">Como el método no devuelve ningún resultado, debe llamarse desde un bloque de código de Razor:</span><span class="sxs-lookup"><span data-stu-id="d80ce-173">Because the method doesn't return a result, it must be called within a Razor code block:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_RenderPartialAsync)]

<span data-ttu-id="d80ce-174">Puesto que `RenderPartialAsync` transmite contenido representado, ofrece mayor rendimiento en algunos escenarios.</span><span class="sxs-lookup"><span data-stu-id="d80ce-174">Since `RenderPartialAsync` streams rendered content, it provides better performance in some scenarios.</span></span> <span data-ttu-id="d80ce-175">En situaciones críticas de rendimiento, realice pruebas comparativas de la página con ambos métodos y use el que genera una respuesta más rápida.</span><span class="sxs-lookup"><span data-stu-id="d80ce-175">In performance-critical situations, benchmark the page using both approaches and use the approach that generates a faster response.</span></span>

### <a name="synchronous-html-helper"></a><span data-ttu-id="d80ce-176">Asistente de HTML sincrónico</span><span class="sxs-lookup"><span data-stu-id="d80ce-176">Synchronous HTML Helper</span></span>

<span data-ttu-id="d80ce-177"><xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.Partial*> y <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartial*> son los equivalentes sincrónicos de `PartialAsync` y `RenderPartialAsync`, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="d80ce-177"><xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.Partial*> and <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartial*> are the synchronous equivalents of `PartialAsync` and `RenderPartialAsync`, respectively.</span></span> <span data-ttu-id="d80ce-178">Los equivalentes sincrónicos no son aconsejables, ya que hay escenarios donde producen interbloqueos.</span><span class="sxs-lookup"><span data-stu-id="d80ce-178">The synchronous equivalents aren't recommended because there are scenarios in which they deadlock.</span></span> <span data-ttu-id="d80ce-179">Se prevé la eliminación de los métodos sincrónicos en una futura versión.</span><span class="sxs-lookup"><span data-stu-id="d80ce-179">The synchronous methods are targeted for removal in a future release.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d80ce-180">Si no necesita ejecutar código, use un [componente de vista](xref:mvc/views/view-components) en lugar de una vista parcial.</span><span class="sxs-lookup"><span data-stu-id="d80ce-180">If you need to execute code, use a [view component](xref:mvc/views/view-components) instead of a partial view.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="d80ce-181">La llamada a `Partial` o `RenderPartial` resulta en una advertencia del analizador de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d80ce-181">Calling `Partial` or `RenderPartial` results in a Visual Studio analyzer warning.</span></span> <span data-ttu-id="d80ce-182">Por ejemplo, la presencia de `Partial` genera el siguiente mensaje de advertencia:</span><span class="sxs-lookup"><span data-stu-id="d80ce-182">For example, the presence of `Partial` yields the following warning message:</span></span>

> <span data-ttu-id="d80ce-183">Use of IHtmlHelper.Partial may result in application deadlocks.</span><span class="sxs-lookup"><span data-stu-id="d80ce-183">Use of IHtmlHelper.Partial may result in application deadlocks.</span></span> <span data-ttu-id="d80ce-184">Considere la posibilidad de utilizar el asistente de etiquetas &lt;parciales&gt; o IHtmlHelper.PartialAsync.</span><span class="sxs-lookup"><span data-stu-id="d80ce-184">Consider using &lt;partial&gt; Tag Helper or IHtmlHelper.PartialAsync.</span></span>

<span data-ttu-id="d80ce-185">Reemplace las llamadas a `@Html.Partial` por `@await Html.PartialAsync` o el [asistente de etiquetas parciales](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="d80ce-185">Replace calls to `@Html.Partial` with `@await Html.PartialAsync` or the [Partial Tag Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span></span> <span data-ttu-id="d80ce-186">Para más información sobre la migración del asistente de etiquetas parciales, vea [Migración desde un asistente de HTML](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper#migrate-from-an-html-helper).</span><span class="sxs-lookup"><span data-stu-id="d80ce-186">For more information on Partial Tag Helper migration, see [Migrate from an HTML Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper#migrate-from-an-html-helper).</span></span>

::: moniker-end

## <a name="partial-view-discovery"></a><span data-ttu-id="d80ce-187">Detección de vistas parciales</span><span class="sxs-lookup"><span data-stu-id="d80ce-187">Partial view discovery</span></span>

<span data-ttu-id="d80ce-188">Cuando se hace referencia a una vista parcial por su nombre sin una extensión de archivo, se busca en las siguientes ubicaciones en el orden indicado:</span><span class="sxs-lookup"><span data-stu-id="d80ce-188">When a partial view is referenced by name without a file extension, the following locations are searched in the stated order:</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="d80ce-189">**Páginas de Razor**</span><span class="sxs-lookup"><span data-stu-id="d80ce-189">**Razor Pages**</span></span>

1. <span data-ttu-id="d80ce-190">Carpeta de la página en ejecución actualmente</span><span class="sxs-lookup"><span data-stu-id="d80ce-190">Currently executing page's folder</span></span>
1. <span data-ttu-id="d80ce-191">Gráfico de directorio por encima de la carpeta de la página</span><span class="sxs-lookup"><span data-stu-id="d80ce-191">Directory graph above the page's folder</span></span>
1. `/Shared`
1. `/Pages/Shared`
1. `/Views/Shared`

<span data-ttu-id="d80ce-192">**MVC**</span><span class="sxs-lookup"><span data-stu-id="d80ce-192">**MVC**</span></span>

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

<span data-ttu-id="d80ce-193">Las convenciones siguientes se aplican a la detección de la vista parcial:</span><span class="sxs-lookup"><span data-stu-id="d80ce-193">The following conventions apply to partial view discovery:</span></span>

* <span data-ttu-id="d80ce-194">Se admiten diferentes vistas parciales con el mismo nombre de archivo cuando las vistas parciales están en carpetas diferentes.</span><span class="sxs-lookup"><span data-stu-id="d80ce-194">Different partial views with the same file name are allowed when the partial views are in different folders.</span></span>
* <span data-ttu-id="d80ce-195">Al hacer referencia a una vista parcial por su nombre sin una extensión de archivo y la vista parcial está presente tanto en la carpeta del autor de la llamada como en la carpeta *compartida*, la vista parcial de la carpeta del autor de la llamada proporciona la vista parcial.</span><span class="sxs-lookup"><span data-stu-id="d80ce-195">When referencing a partial view by name without a file extension and the partial view is present in both the caller's folder and the *Shared* folder, the partial view in the caller's folder supplies the partial view.</span></span> <span data-ttu-id="d80ce-196">Si la vista parcial no está presente en la carpeta del autor de la llamada, se proporciona la vista parcial desde la carpeta *compartida*.</span><span class="sxs-lookup"><span data-stu-id="d80ce-196">If the partial view isn't present in the caller's folder, the partial view is provided from the *Shared* folder.</span></span> <span data-ttu-id="d80ce-197">Las vistas parciales de la carpeta *compartida* se denominan *vistas parciales compartidas* o *vistas parciales predeterminadas*.</span><span class="sxs-lookup"><span data-stu-id="d80ce-197">Partial views in the *Shared* folder are called *shared partial views* or *default partial views*.</span></span>
* <span data-ttu-id="d80ce-198">Las vistas parciales pueden estar *encadenadas*: una vista parcial puede llamar a otra vista parcial si las llamadas no forman una referencia circular.</span><span class="sxs-lookup"><span data-stu-id="d80ce-198">Partial views can be *chained*&mdash;a partial view can call another partial view if a circular reference isn't formed by the calls.</span></span> <span data-ttu-id="d80ce-199">Las rutas de acceso relativas siempre guardan relación con el archivo actual, no con la raíz ni el elemento primario del archivo.</span><span class="sxs-lookup"><span data-stu-id="d80ce-199">Relative paths are always relative to the current file, not to the root or parent of the file.</span></span>

> [!NOTE]
> <span data-ttu-id="d80ce-200">Una clase `section` de [Razor](xref:mvc/views/razor) definida en una vista parcial no es visible para los archivos de marcado principales.</span><span class="sxs-lookup"><span data-stu-id="d80ce-200">A [Razor](xref:mvc/views/razor) `section` defined in a partial view is invisible to parent markup files.</span></span> <span data-ttu-id="d80ce-201">La `section` solo es visible para la vista parcial en la que está definida.</span><span class="sxs-lookup"><span data-stu-id="d80ce-201">The `section` is only visible to the partial view in which it's defined.</span></span>

## <a name="access-data-from-partial-views"></a><span data-ttu-id="d80ce-202">Acceso a datos desde vistas parciales</span><span class="sxs-lookup"><span data-stu-id="d80ce-202">Access data from partial views</span></span>

<span data-ttu-id="d80ce-203">Cuando se crea una instancia de una vista parcial, recibe una *copia* del diccionario `ViewData` del elemento primario.</span><span class="sxs-lookup"><span data-stu-id="d80ce-203">When a partial view is instantiated, it receives a *copy* of the parent's `ViewData` dictionary.</span></span> <span data-ttu-id="d80ce-204">Las actualizaciones realizadas en los datos dentro de la vista parcial no se conservan en la vista principal.</span><span class="sxs-lookup"><span data-stu-id="d80ce-204">Updates made to the data within the partial view aren't persisted to the parent view.</span></span> <span data-ttu-id="d80ce-205">Los cambios de `ViewData` en una vista parcial se pierden cuando se devuelve la vista parcial.</span><span class="sxs-lookup"><span data-stu-id="d80ce-205">`ViewData` changes in a partial view are lost when the partial view returns.</span></span>

<span data-ttu-id="d80ce-206">En el ejemplo siguiente se muestra cómo pasar una instancia de [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) a una vista parcial:</span><span class="sxs-lookup"><span data-stu-id="d80ce-206">The following example demonstrates how to pass an instance of [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) to a partial view:</span></span>

```cshtml
@await Html.PartialAsync("_PartialName", customViewData)
```

<span data-ttu-id="d80ce-207">Puede pasar un modelo a una vista parcial.</span><span class="sxs-lookup"><span data-stu-id="d80ce-207">You can pass a model into a partial view.</span></span> <span data-ttu-id="d80ce-208">El modelo puede ser un objeto personalizado.</span><span class="sxs-lookup"><span data-stu-id="d80ce-208">The model can be a custom object.</span></span> <span data-ttu-id="d80ce-209">Puede pasar un modelo con `PartialAsync` (representa un bloque de contenido al autor de la llamada) o `RenderPartialAsync` (transmite el contenido a la salida):</span><span class="sxs-lookup"><span data-stu-id="d80ce-209">You can pass a model with `PartialAsync` (renders a block of content to the caller) or `RenderPartialAsync` (streams the content to the output):</span></span>

```cshtml
@await Html.PartialAsync("_PartialName", model)
```

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="d80ce-210">**Páginas de Razor**</span><span class="sxs-lookup"><span data-stu-id="d80ce-210">**Razor Pages**</span></span>

<span data-ttu-id="d80ce-211">El siguiente marcado de la aplicación de ejemplo proviene de la página *Pages/ArticlesRP/ReadRP.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="d80ce-211">The following markup in the sample app is from the *Pages/ArticlesRP/ReadRP.cshtml* page.</span></span> <span data-ttu-id="d80ce-212">La página contiene dos vistas parciales.</span><span class="sxs-lookup"><span data-stu-id="d80ce-212">The page contains two partial views.</span></span> <span data-ttu-id="d80ce-213">La segunda vista parcial se pasa a un modelo y `ViewData` a la vista parcial.</span><span class="sxs-lookup"><span data-stu-id="d80ce-213">The second partial view passes in a model and `ViewData` to the partial view.</span></span> <span data-ttu-id="d80ce-214">La sobrecarga del constructor de `ViewDataDictionary` se usa para pasar un nuevo diccionario `ViewData` a la vez que conserva el diccionario `ViewData` existente.</span><span class="sxs-lookup"><span data-stu-id="d80ce-214">The `ViewDataDictionary` constructor overload is used to pass a new `ViewData` dictionary while retaining the existing `ViewData` dictionary.</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Pages/ArticlesRP/ReadRP.cshtml?name=snippet_ReadPartialViewRP&highlight=5,15-20)]

<span data-ttu-id="d80ce-215">*Pages/Shared/_AuthorPartialRP.cshtml* es la primera vista parcial a la que hace referencia el archivo de marcado *ReadRP.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="d80ce-215">*Pages/Shared/_AuthorPartialRP.cshtml* is the first partial view referenced by the *ReadRP.cshtml* markup file:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Pages/Shared/_AuthorPartialRP.cshtml)]

<span data-ttu-id="d80ce-216">*Pages/ArticlesRP/_ArticleSectionRP.cshtml* es la segunda vista parcial a la que hace referencia el archivo de marcado *ReadRP.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="d80ce-216">*Pages/ArticlesRP/_ArticleSectionRP.cshtml* is the second partial view referenced by the *ReadRP.cshtml* markup file:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Pages/ArticlesRP/_ArticleSectionRP.cshtml)]

<span data-ttu-id="d80ce-217">**MVC**</span><span class="sxs-lookup"><span data-stu-id="d80ce-217">**MVC**</span></span>

::: moniker-end

<span data-ttu-id="d80ce-218">El marcado siguiente de la aplicación de ejemplo muestra la vista *Views/Articles/Read.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="d80ce-218">The following markup in the sample app shows the *Views/Articles/Read.cshtml* view.</span></span> <span data-ttu-id="d80ce-219">La vista contiene dos vistas parciales.</span><span class="sxs-lookup"><span data-stu-id="d80ce-219">The view contains two partial views.</span></span> <span data-ttu-id="d80ce-220">La segunda vista parcial se pasa a un modelo y `ViewData` a la vista parcial.</span><span class="sxs-lookup"><span data-stu-id="d80ce-220">The second partial view passes in a model and `ViewData` to the partial view.</span></span> <span data-ttu-id="d80ce-221">La sobrecarga del constructor de `ViewDataDictionary` se usa para pasar un nuevo diccionario `ViewData` a la vez que conserva el diccionario `ViewData` existente.</span><span class="sxs-lookup"><span data-stu-id="d80ce-221">The `ViewDataDictionary` constructor overload is used to pass a new `ViewData` dictionary while retaining the existing `ViewData` dictionary.</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/Read.cshtml?name=snippet_ReadPartialView&highlight=5,15-20)]

<span data-ttu-id="d80ce-222">*Views/Shared/_AuthorPartial.cshtml* es la primera vista parcial a la que hace referencia el archivo de marcado *ReadRP.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="d80ce-222">*Views/Shared/_AuthorPartial.cshtml* is the first partial view referenced by the *ReadRP.cshtml* markup file:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Shared/_AuthorPartial.cshtml)]

<span data-ttu-id="d80ce-223">*Views/Articles/_ArticleSection.cshtml* es la segunda vista parcial a la que hace referencia el archivo de marcado *Read.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="d80ce-223">*Views/Articles/_ArticleSection.cshtml* is the second partial view referenced by the *Read.cshtml* markup file:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/_ArticleSection.cshtml)]

<span data-ttu-id="d80ce-224">En tiempo de ejecución, las vistas parciales se representan en la salida representada del archivo de marcado principal, que a su vez se representa dentro de *_Layout.cshtml* compartido.</span><span class="sxs-lookup"><span data-stu-id="d80ce-224">At runtime, the partials are rendered into the parent markup file's rendered output, which itself is rendered within the shared *_Layout.cshtml*.</span></span> <span data-ttu-id="d80ce-225">La primera vista parcial representa la fecha de publicación y el nombre del autor del artículo:</span><span class="sxs-lookup"><span data-stu-id="d80ce-225">The first partial view renders the article author's name and publication date:</span></span>

> <span data-ttu-id="d80ce-226">Abraham Lincoln</span><span class="sxs-lookup"><span data-stu-id="d80ce-226">Abraham Lincoln</span></span>
>
> <span data-ttu-id="d80ce-227">Esta vista parcial procede de la &lt;ruta de acceso de archivo de la vista parcial compartida&gt;.</span><span class="sxs-lookup"><span data-stu-id="d80ce-227">This partial view from &lt;shared partial view file path&gt;.</span></span>
> <span data-ttu-id="d80ce-228">19/11/1863 12:00:00 a. m.</span><span class="sxs-lookup"><span data-stu-id="d80ce-228">11/19/1863 12:00:00 AM</span></span>

<span data-ttu-id="d80ce-229">La segunda vista parcial representa las secciones del artículo:</span><span class="sxs-lookup"><span data-stu-id="d80ce-229">The second partial view renders the article's sections:</span></span>

> <span data-ttu-id="d80ce-230">Índice de la sección uno: 0</span><span class="sxs-lookup"><span data-stu-id="d80ce-230">Section One Index: 0</span></span>
>
> <span data-ttu-id="d80ce-231">Puntuación de cuatro y hace siete años...</span><span class="sxs-lookup"><span data-stu-id="d80ce-231">Four score and seven years ago ...</span></span>
>
> <span data-ttu-id="d80ce-232">Índice de la sección dos: 1</span><span class="sxs-lookup"><span data-stu-id="d80ce-232">Section Two Index: 1</span></span>
>
> <span data-ttu-id="d80ce-233">Ahora nos encontramos en una verdadera guerra civil, probando...</span><span class="sxs-lookup"><span data-stu-id="d80ce-233">Now we are engaged in a great civil war, testing ...</span></span>
>
> <span data-ttu-id="d80ce-234">Índice de la sección tres: 2</span><span class="sxs-lookup"><span data-stu-id="d80ce-234">Section Three Index: 2</span></span>
>
> <span data-ttu-id="d80ce-235">Pero, en un sentido amplio, no nos podemos dedicar...</span><span class="sxs-lookup"><span data-stu-id="d80ce-235">But, in a larger sense, we can not dedicate ...</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d80ce-236">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="d80ce-236">Additional resources</span></span>

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
