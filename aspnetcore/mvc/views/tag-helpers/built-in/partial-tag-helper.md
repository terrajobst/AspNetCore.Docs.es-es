---
title: Aplicación auxiliar de etiquetas parciales en ASP.NET Core
author: scottaddie
description: Conozca la aplicación auxiliar de etiquetas parciales en ASP.NET Core y el rol que desempeña cada uno de sus atributos a la hora de representar una vista parcial.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 04/13/2018
uid: mvc/views/tag-helpers/builtin-th/partial-tag-helper
ms.openlocfilehash: fea84621f185c4113147cf0dfd173704bc7b6d81
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274419"
---
# <a name="partial-tag-helper-in-aspnet-core"></a><span data-ttu-id="37b2f-103">Aplicación auxiliar de etiquetas parciales en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="37b2f-103">Partial Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="37b2f-104">Por [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="37b2f-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="37b2f-105">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="37b2f-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="overview"></a><span data-ttu-id="37b2f-106">Información general</span><span class="sxs-lookup"><span data-stu-id="37b2f-106">Overview</span></span>

<span data-ttu-id="37b2f-107">La aplicación auxiliar de etiquetas parciales sirve para representar una [vista parcial](xref:mvc/views/partial) en las páginas de Razor y las aplicaciones MVC.</span><span class="sxs-lookup"><span data-stu-id="37b2f-107">The Partial Tag Helper is used for rendering a [partial view](xref:mvc/views/partial) in Razor Pages and MVC apps.</span></span> <span data-ttu-id="37b2f-108">Tenga en cuenta lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="37b2f-108">Consider that it:</span></span>

* <span data-ttu-id="37b2f-109">Es necesario ASP.NET Core 2.1 o una versión posterior.</span><span class="sxs-lookup"><span data-stu-id="37b2f-109">Requires ASP.NET Core 2.1 or later.</span></span>
* <span data-ttu-id="37b2f-110">Es una alternativa a la [sintaxis de la aplicación auxiliar HTML](xref:mvc/views/partial#referencing-a-partial-view).</span><span class="sxs-lookup"><span data-stu-id="37b2f-110">Is an alternative to [HTML Helper syntax](xref:mvc/views/partial#referencing-a-partial-view).</span></span>
* <span data-ttu-id="37b2f-111">Presenta la vista parcial de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="37b2f-111">Renders the partial view asynchronously.</span></span>

<span data-ttu-id="37b2f-112">Las opciones de la aplicación auxiliar HTML para representar una vista parcial son estas:</span><span class="sxs-lookup"><span data-stu-id="37b2f-112">The HTML Helper options for rendering a partial view include:</span></span>

* [<span data-ttu-id="37b2f-113">@await Html.PartialAsync</span><span class="sxs-lookup"><span data-stu-id="37b2f-113">@await Html.PartialAsync</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partialasync)
* [<span data-ttu-id="37b2f-114">@await Html.RenderPartialAsync</span><span class="sxs-lookup"><span data-stu-id="37b2f-114">@await Html.RenderPartialAsync</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartialasync)
* [@Html.Partial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partial)
* [@Html.RenderPartial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartial)

<span data-ttu-id="37b2f-115">En los ejemplos de todo este documento se usa el modelo *Product*:</span><span class="sxs-lookup"><span data-stu-id="37b2f-115">The *Product* model is used in samples throughout this document:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Models/Product.cs)]

<span data-ttu-id="37b2f-116">Ahora pasaremos a ver una relación de los atributos de la aplicación auxiliar de etiquetas parciales.</span><span class="sxs-lookup"><span data-stu-id="37b2f-116">An inventory of the Partial Tag Helper attributes follows.</span></span>

## <a name="name"></a><span data-ttu-id="37b2f-117">name</span><span class="sxs-lookup"><span data-stu-id="37b2f-117">name</span></span>

<span data-ttu-id="37b2f-118">El atributo `name` es necesario.</span><span class="sxs-lookup"><span data-stu-id="37b2f-118">The `name` attribute is required.</span></span> <span data-ttu-id="37b2f-119">Señala el nombre o la ruta de acceso de la vista parcial que se va a representar.</span><span class="sxs-lookup"><span data-stu-id="37b2f-119">It indicates the name or the path of the partial view to be rendered.</span></span> <span data-ttu-id="37b2f-120">Cuando se indica el nombre de una vista parcial, se inicia el proceso de [detección de vista](xref:mvc/views/overview#view-discovery).</span><span class="sxs-lookup"><span data-stu-id="37b2f-120">When a partial view name is provided, the [view discovery](xref:mvc/views/overview#view-discovery) process is initiated.</span></span> <span data-ttu-id="37b2f-121">Este proceso se omite cuando se proporciona una ruta de acceso explícita.</span><span class="sxs-lookup"><span data-stu-id="37b2f-121">That process is bypassed when an explicit path is provided.</span></span>

<span data-ttu-id="37b2f-122">En el siguiente marcado se usa una ruta de acceso explícita, lo que indica que *_ProductPartial.cshtml* debe cargarse desde la carpeta *Shared*.</span><span class="sxs-lookup"><span data-stu-id="37b2f-122">The following markup uses an explicit path, indicating that *_ProductPartial.cshtml* is to be loaded from the *Shared* folder.</span></span> <span data-ttu-id="37b2f-123">Mediante el atributo [for](#for), se pasa un modelo a la vista parcial para el enlace.</span><span class="sxs-lookup"><span data-stu-id="37b2f-123">Using the [for](#for) attribute, a model is passed to the partial view for binding.</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Name)]

## <a name="for"></a><span data-ttu-id="37b2f-124">for</span><span class="sxs-lookup"><span data-stu-id="37b2f-124">for</span></span>

<span data-ttu-id="37b2f-125">El atributo `for` asigna una [ModelExpression](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.modelexpression) para que se evalúe según el modelo actual.</span><span class="sxs-lookup"><span data-stu-id="37b2f-125">The `for` attribute assigns a [ModelExpression](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.modelexpression) to be evaluated against the current model.</span></span> <span data-ttu-id="37b2f-126">`ModelExpression` deduce la sintaxis de `@Model.`.</span><span class="sxs-lookup"><span data-stu-id="37b2f-126">A `ModelExpression` infers the `@Model.` syntax.</span></span> <span data-ttu-id="37b2f-127">Por ejemplo, se puede usar `for="Product"` en lugar de `for="@Model.Product"`.</span><span class="sxs-lookup"><span data-stu-id="37b2f-127">For example, `for="Product"` can be used instead of `for="@Model.Product"`.</span></span> <span data-ttu-id="37b2f-128">Este comportamiento predeterminado de deducción queda invalidado si se usa el símbolo `@` para definir una expresión insertada.</span><span class="sxs-lookup"><span data-stu-id="37b2f-128">This default inference behavior is overridden by using the `@` symbol to define an inline expression.</span></span> <span data-ttu-id="37b2f-129">El atributo `for` no se puede usar con el atributo [model](#model).</span><span class="sxs-lookup"><span data-stu-id="37b2f-129">The `for` attribute can't be used with the [model](#model) attribute.</span></span>

<span data-ttu-id="37b2f-130">El siguiente marcado carga *_ProductPartial.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="37b2f-130">The following markup loads *_ProductPartial.cshtml*:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_For)]

<span data-ttu-id="37b2f-131">La vista parcial se enlaza a la propiedad `Product` del modelo de página asociado correspondiente:</span><span class="sxs-lookup"><span data-stu-id="37b2f-131">The partial view is bound to the associated page model's `Product` property:</span></span>

[!code-csharp[](samples/TagHelpersBuiltIn/Pages/Product.cshtml.cs?highlight=8)]

## <a name="model"></a><span data-ttu-id="37b2f-132">modelo</span><span class="sxs-lookup"><span data-stu-id="37b2f-132">model</span></span>

<span data-ttu-id="37b2f-133">El atributo `model` asigna una instancia de modelo para pasarla a la vista parcial.</span><span class="sxs-lookup"><span data-stu-id="37b2f-133">The `model` attribute assigns a model instance to pass to the partial view.</span></span> <span data-ttu-id="37b2f-134">El atributo `model` no se puede usar con el atributo [for](#for).</span><span class="sxs-lookup"><span data-stu-id="37b2f-134">The `model` attribute can't be used with the [for](#for) attribute.</span></span>

<span data-ttu-id="37b2f-135">En el siguiente código de marcado, se crea un objeto `Product` y se pasa al atributo `model` para el enlace:</span><span class="sxs-lookup"><span data-stu-id="37b2f-135">In the following markup, a new `Product` object is instantiated and passed to the `model` attribute for binding:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_Model)]

## <a name="view-data"></a><span data-ttu-id="37b2f-136">view-data</span><span class="sxs-lookup"><span data-stu-id="37b2f-136">view-data</span></span>

<span data-ttu-id="37b2f-137">El atributo `view-data` asigna un [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) para pasarlo a la vista parcial.</span><span class="sxs-lookup"><span data-stu-id="37b2f-137">The `view-data` attribute assigns a [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) to pass to the partial view.</span></span> <span data-ttu-id="37b2f-138">El siguiente marcado hace que toda la colección ViewData esté accesible para la vista parcial:</span><span class="sxs-lookup"><span data-stu-id="37b2f-138">The following markup makes the entire ViewData collection accessible to the partial view:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Product.cshtml?name=snippet_ViewData&highlight=5-)]

<span data-ttu-id="37b2f-139">En el código anterior, el valor de clave `IsNumberReadOnly` está establecido en `true` y se ha agregado a la colección ViewData.</span><span class="sxs-lookup"><span data-stu-id="37b2f-139">In the preceding code, the `IsNumberReadOnly` key value is set to `true` and added to the ViewData collection.</span></span> <span data-ttu-id="37b2f-140">Por tanto, `ViewData["IsNumberReadOnly"]` estará accesible dentro de la vista parcial siguiente:</span><span class="sxs-lookup"><span data-stu-id="37b2f-140">Consequently, `ViewData["IsNumberReadOnly"]` is made accessible within the following partial view:</span></span>

[!code-cshtml[](samples/TagHelpersBuiltIn/Pages/Shared/_ProductViewDataPartial.cshtml?highlight=5)]

<span data-ttu-id="37b2f-141">En este ejemplo, el valor de `ViewData["IsNumberReadOnly"]` determina si el campo *Number* se muestra como de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="37b2f-141">In this example, the value of `ViewData["IsNumberReadOnly"]` determines whether the *Number* field is displayed as read only.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="37b2f-142">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="37b2f-142">Additional resources</span></span>

* [<span data-ttu-id="37b2f-143">Vistas parciales</span><span class="sxs-lookup"><span data-stu-id="37b2f-143">Partial views</span></span>](xref:mvc/views/partial)
* [<span data-ttu-id="37b2f-144">Datos con establecimiento flexible de tipos (ViewData, atributo ViewData y ViewBag)</span><span class="sxs-lookup"><span data-stu-id="37b2f-144">Weakly typed data (ViewData, ViewData attribute, and ViewBag)</span></span>](xref:mvc/views/overview#weakly-typed-data-viewdata-viewdata-attribute-and-viewbag)
