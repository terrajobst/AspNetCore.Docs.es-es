---
title: Asistente de etiquetas de script en ASP.NET Core
author: rick-anderson
ms.author: riande
description: Descubra los atributos del asistente de etiquetas de script de ASP.NET Core y el papel que desempeña cada atributo al ampliar el comportamiento de la etiqueta de script de código HTML.
ms.custom: mvc
ms.date: 12/18/2018
uid: mvc/views/tag-helpers/builtin-th/script-tag-helper
ms.openlocfilehash: 5f2fb8a45048804afa8aff2989cd53489e45a33b
ms.sourcegitcommit: fae6f0e253f9d62d8f39de5884d2ba2b4b2a6050
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2019
ms.locfileid: "71256469"
---
# <a name="script-tag-helper-in-aspnet-core"></a><span data-ttu-id="5bb7d-103">Asistente de etiquetas de script en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5bb7d-103">Script Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="5bb7d-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5bb7d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5bb7d-105">El [asistente de etiquetas de script](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) genera un vínculo a un archivo de script principal o de reserva.</span><span class="sxs-lookup"><span data-stu-id="5bb7d-105">The [Script Tag Helper](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) generates a link to a primary or fall back script file.</span></span> <span data-ttu-id="5bb7d-106">Normalmente, el archivo de script principal se encuentra en una red [Content Delivery Network](/office365/enterprise/content-delivery-networks#what-exactly-is-a-cdn) (CDN).</span><span class="sxs-lookup"><span data-stu-id="5bb7d-106">Typically the primary script file is on a [Content Delivery Network](/office365/enterprise/content-delivery-networks#what-exactly-is-a-cdn) (CDN).</span></span>

[!INCLUDE[](~/includes/cdn.md)]

<span data-ttu-id="5bb7d-107">El asistente de etiquetas de script permite especificar una red CDN para el archivo de script y una reserva cuando dicha red no está disponible.</span><span class="sxs-lookup"><span data-stu-id="5bb7d-107">The Script Tag Helper allows you to specify a CDN for the script file and a fallback when the CDN is not available.</span></span> <span data-ttu-id="5bb7d-108">El asistente de etiquetas de script proporciona la ventaja de rendimiento de una red CDN con la solidez del hospedaje local.</span><span class="sxs-lookup"><span data-stu-id="5bb7d-108">The Script Tag Helper provides the performance advantage of a CDN with the robustness of local hosting.</span></span>

<span data-ttu-id="5bb7d-109">El siguiente marcado de Razor muestra el elemento `script` de un archivo de diseño creado con la plantilla de aplicación web ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="5bb7d-109">The following Razor markup shows the `script` element of a layout file created with the ASP.NET Core web app template:</span></span>

[!code-html[](link-tag-helper/sample/_Layout.cshtml?name=snippet2)]

<span data-ttu-id="5bb7d-110">Lo siguiente es similar al formato HTML representado del código anterior (en un entorno que no es de desarrollo):</span><span class="sxs-lookup"><span data-stu-id="5bb7d-110">The following is similar to the rendered HTML from the preceding code (in a non-Development environment):</span></span>

[!code-csharp[](link-tag-helper/sample/HtmlPage2.html)]

<span data-ttu-id="5bb7d-111">En el código anterior, el asistente de etiquetas de script generó el segundo elemento de script (`<script>  (window.jQuery || document.write(`), que comprueba `window.jQuery`.</span><span class="sxs-lookup"><span data-stu-id="5bb7d-111">In the preceding code, the Script Tag Helper generated the second script ( `<script>  (window.jQuery || document.write(`) element, which tests for `window.jQuery`.</span></span> <span data-ttu-id="5bb7d-112">Si `window.jQuery` no se encuentra, `document.write(` se ejecuta y crea un script.</span><span class="sxs-lookup"><span data-stu-id="5bb7d-112">If `window.jQuery` is not found, `document.write(` runs and creates a script</span></span> 

## <a name="commonly-used-script-tag-helper-attributes"></a><span data-ttu-id="5bb7d-113">Atributos del asistente de etiquetas de script utilizados habitualmente</span><span class="sxs-lookup"><span data-stu-id="5bb7d-113">Commonly used Script Tag Helper attributes</span></span>

<span data-ttu-id="5bb7d-114">Consulte [Asistente de etiquetas de script](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) para todos los atributos, propiedades y métodos del asistente de etiquetas de script.</span><span class="sxs-lookup"><span data-stu-id="5bb7d-114">See [Script Tag Helper](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) for all the Script Tag Helper attributes, properties, and methods.</span></span>

### <a name="href"></a><span data-ttu-id="5bb7d-115">href</span><span class="sxs-lookup"><span data-stu-id="5bb7d-115">href</span></span>

<span data-ttu-id="5bb7d-116">Dirección preferida del recurso vinculado.</span><span class="sxs-lookup"><span data-stu-id="5bb7d-116">Preferred address of the linked resource.</span></span> <span data-ttu-id="5bb7d-117">La dirección se pasa al código HTML generado en todos los casos.</span><span class="sxs-lookup"><span data-stu-id="5bb7d-117">The address is passed thought to the generated HTML in all cases.</span></span>

### <a name="asp-fallback-href"></a><span data-ttu-id="5bb7d-118">asp-fallback-href</span><span class="sxs-lookup"><span data-stu-id="5bb7d-118">asp-fallback-href</span></span>

<span data-ttu-id="5bb7d-119">La dirección URL de una hoja de estilos CSS en la que se va a realizar la reserva en caso de error en la dirección URL principal.</span><span class="sxs-lookup"><span data-stu-id="5bb7d-119">The URL of a CSS stylesheet to fallback to in the case the primary URL fails.</span></span>

### <a name="asp-fallback-test-class"></a><span data-ttu-id="5bb7d-120">asp-fallback-test-class</span><span class="sxs-lookup"><span data-stu-id="5bb7d-120">asp-fallback-test-class</span></span>

<span data-ttu-id="5bb7d-121">El nombre de clase definido en la hoja de estilos que se va a usar para la prueba de reserva.</span><span class="sxs-lookup"><span data-stu-id="5bb7d-121">The class name defined in the stylesheet to use for the fallback test.</span></span> <span data-ttu-id="5bb7d-122">Para más información, consulte <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestClass>.</span><span class="sxs-lookup"><span data-stu-id="5bb7d-122">For more information, see <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestClass>.</span></span>

### <a name="asp-fallback-test-property"></a><span data-ttu-id="5bb7d-123">asp-fallback-test-property</span><span class="sxs-lookup"><span data-stu-id="5bb7d-123">asp-fallback-test-property</span></span>

<span data-ttu-id="5bb7d-124">El nombre de la propiedad de CSS que se va a usar para la prueba de reserva.</span><span class="sxs-lookup"><span data-stu-id="5bb7d-124">The CSS property name to use for the fallback test.</span></span> <span data-ttu-id="5bb7d-125">Para más información, consulte <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestProperty>.</span><span class="sxs-lookup"><span data-stu-id="5bb7d-125">For more information, see <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestProperty>.</span></span>

### <a name="asp-fallback-test-value"></a><span data-ttu-id="5bb7d-126">asp-fallback-test-value</span><span class="sxs-lookup"><span data-stu-id="5bb7d-126">asp-fallback-test-value</span></span>

<span data-ttu-id="5bb7d-127">El valor de la propiedad de CSS que se va a usar para la prueba de reserva.</span><span class="sxs-lookup"><span data-stu-id="5bb7d-127">The CSS property value to use for the fallback test.</span></span> <span data-ttu-id="5bb7d-128">Para más información, consulte <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestValue>.</span><span class="sxs-lookup"><span data-stu-id="5bb7d-128">For more information, see <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestValue>.</span></span>

### <a name="asp-fallback-test-value"></a><span data-ttu-id="5bb7d-129">asp-fallback-test-value</span><span class="sxs-lookup"><span data-stu-id="5bb7d-129">asp-fallback-test-value</span></span>

<span data-ttu-id="5bb7d-130">El valor de la propiedad de CSS que se va a usar para la prueba de reserva.</span><span class="sxs-lookup"><span data-stu-id="5bb7d-130">The CSS property value to use for the fallback test.</span></span> <span data-ttu-id="5bb7d-131">Para obtener más información, vea <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestValue></span><span class="sxs-lookup"><span data-stu-id="5bb7d-131">For more information, see <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestValue></span></span>

## <a name="additional-resources"></a><span data-ttu-id="5bb7d-132">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="5bb7d-132">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/controllers/areas>
* <xref:razor-pages/index>
* <xref:mvc/compatibility-version>
