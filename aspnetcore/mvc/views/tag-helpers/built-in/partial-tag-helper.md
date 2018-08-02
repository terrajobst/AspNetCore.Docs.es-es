---
title: Aplicación auxiliar de etiquetas parciales en ASP.NET Core
author: scottaddie
description: Conozca la aplicación auxiliar de etiquetas parciales en ASP.NET Core y el rol que desempeña cada uno de sus atributos a la hora de representar una vista parcial.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 07/25/2018
uid: mvc/views/tag-helpers/builtin-th/partial-tag-helper
ms.openlocfilehash: cb63357b1859c3709b2eae9f4e380c4a74e5e448
ms.sourcegitcommit: c8e62aa766641aa55105f7db79cdf2b27a6e5977
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/25/2018
ms.locfileid: "39254758"
---
# <a name="partial-tag-helper-in-aspnet-core"></a><span data-ttu-id="5a68b-103">Aplicación auxiliar de etiquetas parciales en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5a68b-103">Partial Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="5a68b-104">Por [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="5a68b-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="5a68b-105">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5a68b-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="overview"></a><span data-ttu-id="5a68b-106">Información general</span><span class="sxs-lookup"><span data-stu-id="5a68b-106">Overview</span></span>

<span data-ttu-id="5a68b-107">La aplicación auxiliar de etiquetas parciales sirve para representar una [vista parcial](xref:mvc/views/partial) en las páginas de Razor y las aplicaciones MVC.</span><span class="sxs-lookup"><span data-stu-id="5a68b-107">The Partial Tag Helper is used for rendering a [partial view](xref:mvc/views/partial) in Razor Pages and MVC apps.</span></span> <span data-ttu-id="5a68b-108">Tenga en cuenta lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="5a68b-108">Consider that it:</span></span>

* <span data-ttu-id="5a68b-109">Es necesario ASP.NET Core 2.1 o una versión posterior.</span><span class="sxs-lookup"><span data-stu-id="5a68b-109">Requires ASP.NET Core 2.1 or later.</span></span>
* <span data-ttu-id="5a68b-110">Es una alternativa a la [sintaxis de la aplicación auxiliar HTML](xref:mvc/views/partial#reference-a-partial-view).</span><span class="sxs-lookup"><span data-stu-id="5a68b-110">Is an alternative to [HTML Helper syntax](xref:mvc/views/partial#reference-a-partial-view).</span></span>
* <span data-ttu-id="5a68b-111">Presenta la vista parcial de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="5a68b-111">Renders the partial view asynchronously.</span></span>

<span data-ttu-id="5a68b-112">Las opciones de la aplicación auxiliar HTML para representar una vista parcial son estas:</span><span class="sxs-lookup"><span data-stu-id="5a68b-112">The HTML Helper options for rendering a partial view include:</span></span>

* [<span data-ttu-id="5a68b-113">@await Html.PartialAsync</span><span class="sxs-lookup"><span data-stu-id="5a68b-113">@await Html.PartialAsync</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partialasync)
* [<span data-ttu-id="5a68b-114">@await Html.RenderPartialAsync</span><span class="sxs-lookup"><span data-stu-id="5a68b-114">@await Html.RenderPartialAsync</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartialasync)
* [@Html.Partial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partial)
* [@Html.RenderPartial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartial)

<span data-ttu-id="5a68b-115">En los ejemplos de todo este documento se usa el modelo *Product*:</span><span class="sxs-lookup"><span data-stu-id="5a68b-115">The *Product* model is used in samples throughout this document:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Models/Product.cs)]

<span data-ttu-id="5a68b-116">Ahora pasaremos a ver una relación de los atributos de la aplicación auxiliar de etiquetas parciales.</span><span class="sxs-lookup"><span data-stu-id="5a68b-116">An inventory of the Partial Tag Helper attributes follows.</span></span>

## <a name="name"></a><span data-ttu-id="5a68b-117">name</span><span class="sxs-lookup"><span data-stu-id="5a68b-117">name</span></span>

<span data-ttu-id="5a68b-118">El atributo `name` es necesario.</span><span class="sxs-lookup"><span data-stu-id="5a68b-118">The `name` attribute is required.</span></span> <span data-ttu-id="5a68b-119">Señala el nombre o la ruta de acceso de la vista parcial que se va a representar.</span><span class="sxs-lookup"><span data-stu-id="5a68b-119">It indicates the name or the path of the partial view to be rendered.</span></span> <span data-ttu-id="5a68b-120">Cuando se indica el nombre de una vista parcial, se inicia el proceso de [detección de vista](xref:mvc/views/overview#view-discovery).</span><span class="sxs-lookup"><span data-stu-id="5a68b-120">When a partial view name is provided, the [view discovery](xref:mvc/views/overview#view-discovery) process is initiated.</span></span> <span data-ttu-id="5a68b-121">Este proceso se omite cuando se proporciona una ruta de acceso explícita.</span><span class="sxs-lookup"><span data-stu-id="5a68b-121">That process is bypassed when an explicit path is provided.</span></span> <span data-ttu-id="5a68b-122">Para conocer todos los valores `name` aceptables, consulte [Detección de vistas parciales](xref:mvc/views/partial#partial-view-discovery).</span><span class="sxs-lookup"><span data-stu-id="5a68b-122">For all acceptable `name` values, see [Partial view discovery](xref:mvc/views/partial#partial-view-discovery).</span></span>

<span data-ttu-id="5a68b-123">En el siguiente marcado se usa una ruta de acceso explícita, lo que indica que *_ProductPartial.cshtml* debe cargarse desde la carpeta *Shared*.</span><span class="sxs-lookup"><span data-stu-id="5a68b-123">The following markup uses an explicit path, indicating that *_ProductPartial.cshtml* is to be loaded from the *Shared* folder.</span></span> <span data-ttu-id="5a68b-124">Mediante el atributo [for](#for), se pasa un modelo a la vista parcial para el enlace.</span><span class="sxs-lookup"><span data-stu-id="5a68b-124">Using the [for](#for) attribute, a model is passed to the partial view for binding.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Name)]

## <a name="for"></a><span data-ttu-id="5a68b-125">for</span><span class="sxs-lookup"><span data-stu-id="5a68b-125">for</span></span>

<span data-ttu-id="5a68b-126">El atributo `for` asigna una [ModelExpression](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.modelexpression) para que se evalúe según el modelo actual.</span><span class="sxs-lookup"><span data-stu-id="5a68b-126">The `for` attribute assigns a [ModelExpression](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.modelexpression) to be evaluated against the current model.</span></span> <span data-ttu-id="5a68b-127">`ModelExpression` deduce la sintaxis de `@Model.`.</span><span class="sxs-lookup"><span data-stu-id="5a68b-127">A `ModelExpression` infers the `@Model.` syntax.</span></span> <span data-ttu-id="5a68b-128">Por ejemplo, se puede usar `for="Product"` en lugar de `for="@Model.Product"`.</span><span class="sxs-lookup"><span data-stu-id="5a68b-128">For example, `for="Product"` can be used instead of `for="@Model.Product"`.</span></span> <span data-ttu-id="5a68b-129">Este comportamiento predeterminado de deducción queda invalidado si se usa el símbolo `@` para definir una expresión insertada.</span><span class="sxs-lookup"><span data-stu-id="5a68b-129">This default inference behavior is overridden by using the `@` symbol to define an inline expression.</span></span> <span data-ttu-id="5a68b-130">El atributo `for` no se puede usar con el atributo [model](#model).</span><span class="sxs-lookup"><span data-stu-id="5a68b-130">The `for` attribute can't be used with the [model](#model) attribute.</span></span>

<span data-ttu-id="5a68b-131">El siguiente marcado carga *_ProductPartial.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="5a68b-131">The following markup loads *_ProductPartial.cshtml*:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_For)]

<span data-ttu-id="5a68b-132">La vista parcial se enlaza a la propiedad `Product` del modelo de página asociado correspondiente:</span><span class="sxs-lookup"><span data-stu-id="5a68b-132">The partial view is bound to the associated page model's `Product` property:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Pages/Product.cshtml.cs?highlight=8)]

## <a name="model"></a><span data-ttu-id="5a68b-133">modelo</span><span class="sxs-lookup"><span data-stu-id="5a68b-133">model</span></span>

<span data-ttu-id="5a68b-134">El atributo `model` asigna una instancia de modelo para pasarla a la vista parcial.</span><span class="sxs-lookup"><span data-stu-id="5a68b-134">The `model` attribute assigns a model instance to pass to the partial view.</span></span> <span data-ttu-id="5a68b-135">El atributo `model` no se puede usar con el atributo [for](#for).</span><span class="sxs-lookup"><span data-stu-id="5a68b-135">The `model` attribute can't be used with the [for](#for) attribute.</span></span>

<span data-ttu-id="5a68b-136">En el siguiente código de marcado, se crea un objeto `Product` y se pasa al atributo `model` para el enlace:</span><span class="sxs-lookup"><span data-stu-id="5a68b-136">In the following markup, a new `Product` object is instantiated and passed to the `model` attribute for binding:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Model)]

## <a name="view-data"></a><span data-ttu-id="5a68b-137">view-data</span><span class="sxs-lookup"><span data-stu-id="5a68b-137">view-data</span></span>

<span data-ttu-id="5a68b-138">El atributo `view-data` asigna un [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) para pasarlo a la vista parcial.</span><span class="sxs-lookup"><span data-stu-id="5a68b-138">The `view-data` attribute assigns a [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) to pass to the partial view.</span></span> <span data-ttu-id="5a68b-139">El siguiente marcado hace que toda la colección ViewData esté accesible para la vista parcial:</span><span class="sxs-lookup"><span data-stu-id="5a68b-139">The following markup makes the entire ViewData collection accessible to the partial view:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_ViewData&highlight=5-)]

<span data-ttu-id="5a68b-140">En el código anterior, el valor de clave `IsNumberReadOnly` está establecido en `true` y se ha agregado a la colección ViewData.</span><span class="sxs-lookup"><span data-stu-id="5a68b-140">In the preceding code, the `IsNumberReadOnly` key value is set to `true` and added to the ViewData collection.</span></span> <span data-ttu-id="5a68b-141">Por tanto, `ViewData["IsNumberReadOnly"]` estará accesible dentro de la vista parcial siguiente:</span><span class="sxs-lookup"><span data-stu-id="5a68b-141">Consequently, `ViewData["IsNumberReadOnly"]` is made accessible within the following partial view:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Shared/_ProductViewDataPartial.cshtml?highlight=5)]

<span data-ttu-id="5a68b-142">En este ejemplo, el valor de `ViewData["IsNumberReadOnly"]` determina si el campo *Number* se muestra como de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="5a68b-142">In this example, the value of `ViewData["IsNumberReadOnly"]` determines whether the *Number* field is displayed as read only.</span></span>

## <a name="migrate-from-an-html-helper"></a><span data-ttu-id="5a68b-143">Migración desde una aplicación auxiliar HTML</span><span class="sxs-lookup"><span data-stu-id="5a68b-143">Migrate from an HTML Helper</span></span>

<span data-ttu-id="5a68b-144">Tenga en cuenta el siguiente ejemplo de aplicación auxiliar HTML asincrónica.</span><span class="sxs-lookup"><span data-stu-id="5a68b-144">Consider the following asynchronous HTML Helper example.</span></span> <span data-ttu-id="5a68b-145">Se itera y se muestra una colección de productos.</span><span class="sxs-lookup"><span data-stu-id="5a68b-145">A collection of products is iterated and displayed.</span></span> <span data-ttu-id="5a68b-146">Según el primer parámetro del método `PartialAsync`, se carga la vista parcial *_ProductPartial.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5a68b-146">Per the `PartialAsync` method's first parameter, the *_ProductPartial.cshtml* partial view is loaded.</span></span> <span data-ttu-id="5a68b-147">Se pasa una instancia del modelo `Product` a la vista parcial para el enlace.</span><span class="sxs-lookup"><span data-stu-id="5a68b-147">An instance of the `Product` model is passed to the partial view for binding.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Products.cshtml?name=snippet_HtmlHelper&highlight=3)]

<span data-ttu-id="5a68b-148">La siguiente aplicación auxiliar de etiquetas parciales logra el mismo comportamiento de representación asincrónica que la aplicación auxiliar HTML `PartialAsync`.</span><span class="sxs-lookup"><span data-stu-id="5a68b-148">The following Partial Tag Helper achieves the same asynchronous rendering behavior as the `PartialAsync` HTML Helper.</span></span> <span data-ttu-id="5a68b-149">El atributo `model` tiene asignada una instancia del modelo `Product` para el enlace a la vista parcial.</span><span class="sxs-lookup"><span data-stu-id="5a68b-149">The `model` attribute is assigned a `Product` model instance for binding to the partial view.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Products.cshtml?name=snippet_TagHelper&highlight=3)]

## <a name="additional-resources"></a><span data-ttu-id="5a68b-150">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="5a68b-150">Additional resources</span></span>

* <xref:mvc/views/partial>
* <xref:mvc/views/overview#weakly-typed-data-viewdata-viewdata-attribute-and-viewbag>
