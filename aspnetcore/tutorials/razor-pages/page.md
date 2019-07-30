---
title: Páginas de Razor con scaffolding en ASP.NET Core
author: rick-anderson
description: Explica las páginas de Razor generadas por la técnica scaffolding.
ms.author: riande
ms.date: 07/22/2019
uid: tutorials/razor-pages/page
ms.openlocfilehash: 741ee4291cacbb1de0f8341673c8fd6ef0c9a462
ms.sourcegitcommit: 849af69ee3c94cdb9fd8fa1f1bb8f5a5dda7b9eb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/22/2019
ms.locfileid: "68371858"
---
# <a name="scaffolded-razor-pages-in-aspnet-core"></a><span data-ttu-id="25e09-103">Páginas de Razor con scaffolding en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="25e09-103">Scaffolded Razor Pages in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="25e09-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="25e09-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="25e09-105">En este tutorial se examinan las páginas de Razor creadas por la técnica scaffolding en el [tutorial anterior](xref:tutorials/razor-pages/model).</span><span class="sxs-lookup"><span data-stu-id="25e09-105">This tutorial examines the Razor Pages created by scaffolding in the [previous tutorial](xref:tutorials/razor-pages/model).</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="25e09-106">Páginas de creación, eliminación, detalles y edición</span><span class="sxs-lookup"><span data-stu-id="25e09-106">The Create, Delete, Details, and Edit pages</span></span>

<span data-ttu-id="25e09-107">Examine el modelo de página *Pages/Movies/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="25e09-107">Examine the *Pages/Movies/Index.cshtml.cs* Page Model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs)]

<span data-ttu-id="25e09-108">Las páginas de Razor se derivan de `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="25e09-108">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="25e09-109">Por convención, la clase derivada de `PageModel` se denomina `<PageName>Model`.</span><span class="sxs-lookup"><span data-stu-id="25e09-109">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="25e09-110">El constructor aplica la [inserción de dependencias](xref:fundamentals/dependency-injection) para agregar el `RazorPagesMovieContext` a la página.</span><span class="sxs-lookup"><span data-stu-id="25e09-110">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `RazorPagesMovieContext` to the page.</span></span> <span data-ttu-id="25e09-111">Todas las páginas con scaffolding siguen este patrón.</span><span class="sxs-lookup"><span data-stu-id="25e09-111">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="25e09-112">Vea [Código asincrónico](xref:data/ef-rp/intro#asynchronous-code) para obtener más información sobre programación asincrónica con Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="25e09-112">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programing with Entity Framework.</span></span>

<span data-ttu-id="25e09-113">Cuando se efectúa una solicitud para la página, el método `OnGetAsync` devuelve una lista de películas a la página de Razor.</span><span class="sxs-lookup"><span data-stu-id="25e09-113">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="25e09-114">Se llama a `OnGetAsync` o a `OnGet` en una página de Razor para inicializar el estado de la página.</span><span class="sxs-lookup"><span data-stu-id="25e09-114">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="25e09-115">En este caso, `OnGetAsync` obtiene una lista de películas y las muestra.</span><span class="sxs-lookup"><span data-stu-id="25e09-115">In this case, `OnGetAsync` gets a list of movies and displays them.</span></span>

<span data-ttu-id="25e09-116">Cuando `OnGet` devuelve `void` o `OnGetAsync` devuelve `Task`, no se utiliza ningún método de devolución.</span><span class="sxs-lookup"><span data-stu-id="25e09-116">When `OnGet` returns `void` or `OnGetAsync` returns`Task`, no return method is used.</span></span> <span data-ttu-id="25e09-117">Cuando el tipo de valor devuelto es `IActionResult` o `Task<IActionResult>`, se debe proporcionar una instrucción return.</span><span class="sxs-lookup"><span data-stu-id="25e09-117">When the return type is `IActionResult` or `Task<IActionResult>`, a return statement must be provided.</span></span> <span data-ttu-id="25e09-118">Por ejemplo, el método *Pages/Movies/Create.cshtml.cs*`OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="25e09-118">For example, the *Pages/Movies/Create.cshtml.cs* `OnPostAsync` method:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a> <span data-ttu-id="25e09-119">Examine la página de Razor *Pages/Movies/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="25e09-119">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml)]

<span data-ttu-id="25e09-120">Razor puede realizar la transición de HTML a C# o a un marcado específico de Razor.</span><span class="sxs-lookup"><span data-stu-id="25e09-120">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="25e09-121">Cuando el símbolo `@` va seguido de una [palabra clave reservada de Razor](xref:mvc/views/razor#razor-reserved-keywords), realiza una transición a un marcado específico de Razor; en caso contrario, realiza la transición a C#.</span><span class="sxs-lookup"><span data-stu-id="25e09-121">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="25e09-122">La directiva de Razor `@page` convierte el archivo en una acción de MVC, lo que significa que puede controlar las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="25e09-122">The `@page` Razor directive makes the file into an MVC action, which means that it can handle requests.</span></span> <span data-ttu-id="25e09-123">`@page` debe ser la primera directiva de Razor de una página.</span><span class="sxs-lookup"><span data-stu-id="25e09-123">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="25e09-124">`@page` es un ejemplo de la transición a un marcado específico de Razor.</span><span class="sxs-lookup"><span data-stu-id="25e09-124">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="25e09-125">Vea [Razor syntax](xref:mvc/views/razor#razor-syntax) (Sintaxis de Razor) para más información.</span><span class="sxs-lookup"><span data-stu-id="25e09-125">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="25e09-126">Examine la expresión lambda usada en el siguiente asistente de HTML:</span><span class="sxs-lookup"><span data-stu-id="25e09-126">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

<span data-ttu-id="25e09-127">El asistente de HTML `DisplayNameFor` inspecciona la propiedad `Title` a la que se hace referencia en la expresión lambda para determinar el nombre para mostrar.</span><span class="sxs-lookup"><span data-stu-id="25e09-127">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="25e09-128">La expresión lambda se inspecciona, no se evalúa.</span><span class="sxs-lookup"><span data-stu-id="25e09-128">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="25e09-129">Esto significa que no hay ninguna infracción de acceso si `model`, `model.Movie` o `model.Movie[0]` son `null` o están vacíos.</span><span class="sxs-lookup"><span data-stu-id="25e09-129">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="25e09-130">Al evaluar la expresión lambda (por ejemplo, con `@Html.DisplayFor(modelItem => item.Title)`), se evalúan los valores de propiedad del modelo.</span><span class="sxs-lookup"><span data-stu-id="25e09-130">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>

### <a name="the-model-directive"></a><span data-ttu-id="25e09-131">La directiva @model</span><span class="sxs-lookup"><span data-stu-id="25e09-131">The @model directive</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="25e09-132">La directiva `@model` especifica el tipo del modelo que se pasa a la página de Razor.</span><span class="sxs-lookup"><span data-stu-id="25e09-132">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="25e09-133">En el ejemplo anterior, la línea `@model` permite que la clase derivada de `PageModel` esté disponible en la página de Razor.</span><span class="sxs-lookup"><span data-stu-id="25e09-133">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="25e09-134">El modelo se usa en los [asistentes de HTML](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers)`@Html.DisplayNameFor` y `@Html.DisplayFor` de la página.</span><span class="sxs-lookup"><span data-stu-id="25e09-134">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayFor` [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

### <a name="the-layout-page"></a><span data-ttu-id="25e09-135">Página de diseño</span><span class="sxs-lookup"><span data-stu-id="25e09-135">The layout page</span></span>

<span data-ttu-id="25e09-136">Seleccione los vínculos de menú (**RazorPagesMovie** [Película de Razor Pages], **Home** [Inicio] y **Privacy** [Privacidad]).</span><span class="sxs-lookup"><span data-stu-id="25e09-136">Select the menu links (**RazorPagesMovie**, **Home**, and **Privacy**).</span></span> <span data-ttu-id="25e09-137">Cada página muestra el mismo diseño de menú.</span><span class="sxs-lookup"><span data-stu-id="25e09-137">Each page shows the same menu layout.</span></span> <span data-ttu-id="25e09-138">El diseño de menú se implementa en el archivo *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="25e09-138">The menu layout is implemented in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="25e09-139">Abra el archivo *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="25e09-139">Open the *Pages/Shared/_Layout.cshtml* file.</span></span>

<span data-ttu-id="25e09-140">Las plantillas de [diseño](xref:mvc/views/layout) permiten que el diseño del contenedor HTML:</span><span class="sxs-lookup"><span data-stu-id="25e09-140">[Layout](xref:mvc/views/layout) templates allow the HTML container layout to be:</span></span>

* <span data-ttu-id="25e09-141">Esté especificado en un solo lugr.</span><span class="sxs-lookup"><span data-stu-id="25e09-141">Specified in one place.</span></span>
* <span data-ttu-id="25e09-142">Se aplique en varias páginas del sitio.</span><span class="sxs-lookup"><span data-stu-id="25e09-142">Applied in multiple pages in the site.</span></span>

<span data-ttu-id="25e09-143">Busque la línea `@RenderBody()`.</span><span class="sxs-lookup"><span data-stu-id="25e09-143">Find the `@RenderBody()` line.</span></span> <span data-ttu-id="25e09-144">`RenderBody` es un marcador de posición donde se muestran todas las vistas específicas de página, *encapsuladas* en la página de diseño.</span><span class="sxs-lookup"><span data-stu-id="25e09-144">`RenderBody` is a placeholder where all the page-specific views show up, *wrapped* in the layout page.</span></span> <span data-ttu-id="25e09-145">Por ejemplo, seleccione el vínculo **Privacidad** y la vista *Pages/Privacy.cshtml* se representa dentro del método `RenderBody`.</span><span class="sxs-lookup"><span data-stu-id="25e09-145">For example, select the **Privacy** link and the *Pages/Privacy.cshtml* view is rendered inside the `RenderBody` method.</span></span>

<a name="vd"></a>

### <a name="viewdata-and-layout"></a><span data-ttu-id="25e09-146">Propiedades ViewData y Layout</span><span class="sxs-lookup"><span data-stu-id="25e09-146">ViewData and layout</span></span>

<span data-ttu-id="25e09-147">Tenga en cuenta el siguiente marcado del archivo *Pages/Movies/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="25e09-147">Consider the following markup from the *Pages/Movies/Index.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

<span data-ttu-id="25e09-148">El marcado resaltado anterior es un ejemplo de Razor con una transición a C#.</span><span class="sxs-lookup"><span data-stu-id="25e09-148">The preceding highlighted markup is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="25e09-149">Los caracteres `{` y `}` delimitan un bloque de código de C#.</span><span class="sxs-lookup"><span data-stu-id="25e09-149">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="25e09-150">La clase base `PageModel` contiene una propiedad de diccionario `ViewData` que se puede usar para agregar datos y pasarlos a una vista.</span><span class="sxs-lookup"><span data-stu-id="25e09-150">The `PageModel` base class contains a `ViewData` dictionary property that can be used to add data that and pass it to a View.</span></span> <span data-ttu-id="25e09-151">Los objetos se agregan al diccionario `ViewData` con un patrón clave-valor.</span><span class="sxs-lookup"><span data-stu-id="25e09-151">Objects are added to the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="25e09-152">En el ejemplo anterior, la propiedad `"Title"` se agrega al diccionario `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="25e09-152">In the preceding sample, the `"Title"` property is added to the `ViewData` dictionary.</span></span>

<span data-ttu-id="25e09-153">La propiedad `"Title"` se usa en el archivo *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="25e09-153">The `"Title"` property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="25e09-154">En el siguiente marcado se muestran las primeras líneas del archivo *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="25e09-154">The following markup shows the first few lines of the *_Layout.cshtml* file.</span></span>

<!-- we need a snapshot copy of layout because we are
changing in in the next step.
-->
[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout.cshtml?highlight=6)]

<span data-ttu-id="25e09-155">La línea `@*Markup removed for brevity.*@` es un comentario de Razor.</span><span class="sxs-lookup"><span data-stu-id="25e09-155">The line `@*Markup removed for brevity.*@` is a Razor comment.</span></span> <span data-ttu-id="25e09-156">A diferencia de los comentarios HTML (`<!-- -->`), los comentarios de Razor no se envían al cliente.</span><span class="sxs-lookup"><span data-stu-id="25e09-156">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="25e09-157">Actualizar el diseño</span><span class="sxs-lookup"><span data-stu-id="25e09-157">Update the layout</span></span>

<span data-ttu-id="25e09-158">Cambie el elemento `<title>` del archivo *Pages/Shared/_Layout.cshtml* para mostrar **Movie** en lugar de **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="25e09-158">Change the `<title>` element in the *Pages/Shared/_Layout.cshtml* file to display **Movie** rather than **RazorPagesMovie**.</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie30/Pages/Shared/_Layout.cshtml?range=1-6&highlight=6)]

<span data-ttu-id="25e09-159">Busque el siguiente elemento delimitador en el archivo *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="25e09-159">Find the following anchor element in the *Pages/Shared/_Layout.cshtml* file.</span></span>

```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Index">RazorPagesMovie</a>
```

<span data-ttu-id="25e09-160">Reemplace el elemento anterior por el marcado siguiente:</span><span class="sxs-lookup"><span data-stu-id="25e09-160">Replace the preceding element with the following markup:</span></span>

```cshtml
<a class="navbar-brand" asp-page="/Movies/Index">RpMovie</a>
```

<span data-ttu-id="25e09-161">El elemento delimitador anterior es un [asistente de etiquetas](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="25e09-161">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="25e09-162">En este caso, se trata de el [asistente de etiquetas Anchor](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="25e09-162">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="25e09-163">El atributo y valor del asistente de etiquetas `asp-page="/Movies/Index"` crea un vínculo a la página de Razor `/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="25e09-163">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span> <span data-ttu-id="25e09-164">El valor de atributo `asp-area` está vacío, por lo que no se usa el área del vínculo.</span><span class="sxs-lookup"><span data-stu-id="25e09-164">The `asp-area` attribute value is empty, so the area isn't used in the link.</span></span> <span data-ttu-id="25e09-165">Consulte [Áreas](xref:mvc/controllers/areas) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="25e09-165">See [Areas](xref:mvc/controllers/areas) for more information.</span></span>

<span data-ttu-id="25e09-166">Guarde los cambios y pruebe la aplicación haciendo clic en el vínculo **RpMovie**.</span><span class="sxs-lookup"><span data-stu-id="25e09-166">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="25e09-167">Si tiene cualquier problema, consulte el archivo [_Layout.cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Pages/Shared/_Layout.cshtml) en GitHub.</span><span class="sxs-lookup"><span data-stu-id="25e09-167">See the [_Layout.cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Pages/Shared/_Layout.cshtml) file in GitHub if you have any problems.</span></span>

<span data-ttu-id="25e09-168">Pruebe los otros vínculos (**Inicio**, **RpMovie**, **Crear**, **Editar** y **Eliminar**).</span><span class="sxs-lookup"><span data-stu-id="25e09-168">Test the other links (**Home**, **RpMovie**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="25e09-169">Cada página establece el título, que puede ver en la pestaña del explorador. Al marcar una página, se usa el título para el marcador.</span><span class="sxs-lookup"><span data-stu-id="25e09-169">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span>

> [!NOTE]
> <span data-ttu-id="25e09-170">Es posible que no pueda escribir comas decimales en el campo `Price`.</span><span class="sxs-lookup"><span data-stu-id="25e09-170">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="25e09-171">Para que la [validación de jQuery](https://jqueryvalidation.org/) sea compatible con configuraciones regionales distintas del inglés que usan una coma (",") en lugar de un punto decimal y formatos de fecha distintos del de Estados Unidos, debe seguir unos pasos para globalizar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="25e09-171">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="25e09-172">Consulte este [problema 4076 de GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) para instrucciones sobre cómo agregar la coma decimal.</span><span class="sxs-lookup"><span data-stu-id="25e09-172">See this [GitHub issue 4076](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) for instructions on adding decimal comma.</span></span>

<span data-ttu-id="25e09-173">La propiedad `Layout` se establece en el archivo *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="25e09-173">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie30/Pages/_ViewStart.cshtml)]

<span data-ttu-id="25e09-174">El marcado anterior establece el archivo de diseño en *Pages/Shared/_Layout.cshtml* para todos los archivos de Razor en la carpeta *Pages*.</span><span class="sxs-lookup"><span data-stu-id="25e09-174">The preceding markup sets the layout file to *Pages/Shared/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="25e09-175">Vea [Layout](xref:razor-pages/index#layout) (Diseño) para más información.</span><span class="sxs-lookup"><span data-stu-id="25e09-175">See [Layout](xref:razor-pages/index#layout) for more information.</span></span>

### <a name="the-create-page-model"></a><span data-ttu-id="25e09-176">Modelo de página Crear</span><span class="sxs-lookup"><span data-stu-id="25e09-176">The Create page model</span></span>

<span data-ttu-id="25e09-177">Examine el modelo de página *Pages/Movies/Create.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="25e09-177">Examine the *Pages/Movies/Create.cshtml.cs* page model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

<span data-ttu-id="25e09-178">El método `OnGet` inicializa cualquier estado necesario para la página.</span><span class="sxs-lookup"><span data-stu-id="25e09-178">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="25e09-179">La página Crear no tiene ningún estado que inicializar, de modo que se devuelve `Page`.</span><span class="sxs-lookup"><span data-stu-id="25e09-179">The Create page doesn't have any state to initialize, so `Page` is returned.</span></span> <span data-ttu-id="25e09-180">Más adelante en el tutorial se muestra un ejemplo del estado de inicialización de `OnGet`.</span><span class="sxs-lookup"><span data-stu-id="25e09-180">Later in the tutorial, an example of `OnGet` initializing state is shown.</span></span> <span data-ttu-id="25e09-181">El método `Page` crea un objeto `PageResult` que representa la página *Create.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="25e09-181">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="25e09-182">La propiedad `Movie` usa el atributo `[BindProperty]` para participar en el [enlace de modelos](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="25e09-182">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="25e09-183">Cuando el formulario de creación publica los valores del formulario, el tiempo de ejecución de ASP.NET Core enlaza los valores publicados con el modelo `Movie`.</span><span class="sxs-lookup"><span data-stu-id="25e09-183">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="25e09-184">El método `OnPostAsync` se ejecuta cuando la página publica los datos del formulario:</span><span class="sxs-lookup"><span data-stu-id="25e09-184">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="25e09-185">Si hay algún error de modelo, se vuelve a mostrar el formulario, junto con los datos del formulario publicados.</span><span class="sxs-lookup"><span data-stu-id="25e09-185">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="25e09-186">La mayoría de los errores de modelo se pueden capturar en el cliente antes de que se publique el formulario.</span><span class="sxs-lookup"><span data-stu-id="25e09-186">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="25e09-187">Un ejemplo de un error de modelo consiste en publicar un valor para el campo de fecha que no se puede convertir en una fecha.</span><span class="sxs-lookup"><span data-stu-id="25e09-187">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="25e09-188">Más adelante en el tutorial, hablaremos de la validación del lado cliente y de la validación de modelos.</span><span class="sxs-lookup"><span data-stu-id="25e09-188">Client-side validation and model validation are discussed later in the tutorial.</span></span>

<span data-ttu-id="25e09-189">Si no hay ningún error de modelo, los datos se guardan y el explorador se redirige a la página Índice.</span><span class="sxs-lookup"><span data-stu-id="25e09-189">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="25e09-190">Página de Razor Crear</span><span class="sxs-lookup"><span data-stu-id="25e09-190">The Create Razor Page</span></span>

<span data-ttu-id="25e09-191">Examine el archivo de la página de Razor *Pages/Movies/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="25e09-191">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml)]

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="25e09-192">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="25e09-192">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="25e09-193">Visual Studio muestra las etiquetas siguientes con una fuente negrita diferenciada que se aplica a los asistentes de etiquetas:</span><span class="sxs-lookup"><span data-stu-id="25e09-193">Visual Studio displays the following tags in a distinctive bold font used for Tag Helpers:</span></span>

* `<form method="post">`
* `<div asp-validation-summary="ModelOnly" class="text-danger"></div>`
* `<label asp-for="Movie.Title" class="control-label"></label>`
* `<input asp-for="Movie.Title" class="form-control" />`
* `<span asp-validation-for="Movie.Title" class="text-danger"></span>`

![Vista de VS17 de la página Create.cshtml](page/_static/th3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="25e09-195">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="25e09-195">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="25e09-196">Los asistentes de etiquetas siguientes se muestran en el marcado anterior:</span><span class="sxs-lookup"><span data-stu-id="25e09-196">The following Tag Helpers are shown in the preceding markup:</span></span>

* `<form method="post">`
* `<div asp-validation-summary="ModelOnly" class="text-danger"></div>`
* `<label asp-for="Movie.Title" class="control-label"></label>`
* `<input asp-for="Movie.Title" class="form-control" />`
* `<span asp-validation-for="Movie.Title" class="text-danger"></span>`

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="25e09-197">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="25e09-197">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="25e09-198">Visual Studio muestra las etiquetas siguientes con una fuente negrita diferenciada que se aplica a los asistentes de etiquetas:</span><span class="sxs-lookup"><span data-stu-id="25e09-198">Visual Studio displays the following tags in a distinctive bold font used for Tag Helpers:</span></span>

* `<form method="post">`
* `<div asp-validation-summary="ModelOnly" class="text-danger"></div>`
* `<label asp-for="Movie.Title" class="control-label"></label>`
* `<input asp-for="Movie.Title" class="form-control" />`
* `<span asp-validation-for="Movie.Title" class="text-danger"></span>`

---

<span data-ttu-id="25e09-199">El elemento `<form method="post">` es un [asistente de etiquetas de formulario](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="25e09-199">The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="25e09-200">El asistente de etiquetas de formulario incluye automáticamente un [token antifalsificación](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="25e09-200">The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).</span></span>

<span data-ttu-id="25e09-201">El motor de scaffolding crea un marcado de Razor para cada campo del modelo (excepto el identificador) similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="25e09-201">The scaffolding engine creates Razor markup for each field in the model (except the ID) similar to the following:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample3/RazorPagesMovie30/Pages/Movies/Create.cshtml?range=15-20)]

<span data-ttu-id="25e09-202">Los [asistentes de etiquetas de validación](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` y `<span asp-validation-for`) muestran errores de validación.</span><span class="sxs-lookup"><span data-stu-id="25e09-202">The [Validation Tag Helpers](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` and `<span asp-validation-for`) display validation errors.</span></span> <span data-ttu-id="25e09-203">La validación se trata con más detalle en un punto posterior de esta serie.</span><span class="sxs-lookup"><span data-stu-id="25e09-203">Validation is covered in more detail later in this series.</span></span>

<span data-ttu-id="25e09-204">El [asistente de etiquetas](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) genera el título de la etiqueta y el atributo `for` para la propiedad `Title`.</span><span class="sxs-lookup"><span data-stu-id="25e09-204">The [Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generates the label caption and `for` attribute for the `Title` property.</span></span>

<span data-ttu-id="25e09-205">El [asistente de etiquetas de entrada](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`) usa los atributos [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) y genera los atributos HTML necesarios para la validación de jQuery en el lado del cliente.</span><span class="sxs-lookup"><span data-stu-id="25e09-205">The [Input Tag Helper](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`) uses the [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span>

<span data-ttu-id="25e09-206">Para obtener más información sobre los asistentes de etiquetas como `<form method="post">`, consulte [Asistentes de etiquetas en ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="25e09-206">For more information on Tag Helpers such as `<form method="post">`, see [Tag Helpers in ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="25e09-207">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="25e09-207">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="25e09-208">[Anterior: Adición de un modelo](xref:tutorials/razor-pages/model)
> [Siguiente: Base de datos](xref:tutorials/razor-pages/sql)</span><span class="sxs-lookup"><span data-stu-id="25e09-208">[Previous: Adding a model](xref:tutorials/razor-pages/model)
[Next: Database](xref:tutorials/razor-pages/sql)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="25e09-209">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="25e09-209">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="25e09-210">En este tutorial se examinan las páginas de Razor creadas por la técnica scaffolding en el [tutorial anterior](xref:tutorials/razor-pages/model).</span><span class="sxs-lookup"><span data-stu-id="25e09-210">This tutorial examines the Razor Pages created by scaffolding in the [previous tutorial](xref:tutorials/razor-pages/model).</span></span>

<span data-ttu-id="25e09-211">[Vea o descargue](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22) un ejemplo.</span><span class="sxs-lookup"><span data-stu-id="25e09-211">[View or download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22) sample.</span></span>

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="25e09-212">Páginas de creación, eliminación, detalles y edición</span><span class="sxs-lookup"><span data-stu-id="25e09-212">The Create, Delete, Details, and Edit pages</span></span>

<span data-ttu-id="25e09-213">Examine el modelo de página *Pages/Movies/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="25e09-213">Examine the *Pages/Movies/Index.cshtml.cs* Page Model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]

<span data-ttu-id="25e09-214">Las páginas de Razor se derivan de `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="25e09-214">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="25e09-215">Por convención, la clase derivada de `PageModel` se denomina `<PageName>Model`.</span><span class="sxs-lookup"><span data-stu-id="25e09-215">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="25e09-216">El constructor aplica la [inserción de dependencias](xref:fundamentals/dependency-injection) para agregar el `RazorPagesMovieContext` a la página.</span><span class="sxs-lookup"><span data-stu-id="25e09-216">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `RazorPagesMovieContext` to the page.</span></span> <span data-ttu-id="25e09-217">Todas las páginas con scaffolding siguen este patrón.</span><span class="sxs-lookup"><span data-stu-id="25e09-217">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="25e09-218">Vea [Código asincrónico](xref:data/ef-rp/intro#asynchronous-code) para obtener más información sobre programación asincrónica con Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="25e09-218">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programing with Entity Framework.</span></span>

<span data-ttu-id="25e09-219">Cuando se efectúa una solicitud para la página, el método `OnGetAsync` devuelve una lista de películas a la página de Razor.</span><span class="sxs-lookup"><span data-stu-id="25e09-219">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="25e09-220">Se llama a `OnGetAsync` o a `OnGet` en una página de Razor para inicializar el estado de la página.</span><span class="sxs-lookup"><span data-stu-id="25e09-220">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="25e09-221">En este caso, `OnGetAsync` obtiene una lista de películas y las muestra.</span><span class="sxs-lookup"><span data-stu-id="25e09-221">In this case, `OnGetAsync` gets a list of movies and displays them.</span></span>

<span data-ttu-id="25e09-222">Cuando `OnGet` devuelve `void` o `OnGetAsync` devuelve `Task`, no se utiliza ningún método de devolución.</span><span class="sxs-lookup"><span data-stu-id="25e09-222">When `OnGet` returns `void` or `OnGetAsync` returns`Task`, no return method is used.</span></span> <span data-ttu-id="25e09-223">Cuando el tipo de valor devuelto es `IActionResult` o `Task<IActionResult>`, se debe proporcionar una instrucción return.</span><span class="sxs-lookup"><span data-stu-id="25e09-223">When the return type is `IActionResult` or `Task<IActionResult>`, a return statement must be provided.</span></span> <span data-ttu-id="25e09-224">Por ejemplo, el método *Pages/Movies/Create.cshtml.cs*`OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="25e09-224">For example, the *Pages/Movies/Create.cshtml.cs* `OnPostAsync` method:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a> <span data-ttu-id="25e09-225">Examine la página de Razor *Pages/Movies/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="25e09-225">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

<span data-ttu-id="25e09-226">Razor puede realizar la transición de HTML a C# o a un marcado específico de Razor.</span><span class="sxs-lookup"><span data-stu-id="25e09-226">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="25e09-227">Cuando el símbolo `@` va seguido de una [palabra clave reservada de Razor](xref:mvc/views/razor#razor-reserved-keywords), realiza una transición a un marcado específico de Razor; en caso contrario, realiza la transición a C#.</span><span class="sxs-lookup"><span data-stu-id="25e09-227">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="25e09-228">La directiva de Razor `@page` convierte el archivo en una acción de MVC, lo que significa que puede controlar las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="25e09-228">The `@page` Razor directive makes the file into an MVC action, which means that it can handle requests.</span></span> <span data-ttu-id="25e09-229">`@page` debe ser la primera directiva de Razor de una página.</span><span class="sxs-lookup"><span data-stu-id="25e09-229">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="25e09-230">`@page` es un ejemplo de la transición a un marcado específico de Razor.</span><span class="sxs-lookup"><span data-stu-id="25e09-230">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="25e09-231">Vea [Razor syntax](xref:mvc/views/razor#razor-syntax) (Sintaxis de Razor) para más información.</span><span class="sxs-lookup"><span data-stu-id="25e09-231">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="25e09-232">Examine la expresión lambda usada en el siguiente asistente de HTML:</span><span class="sxs-lookup"><span data-stu-id="25e09-232">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

<span data-ttu-id="25e09-233">El asistente de HTML `DisplayNameFor` inspecciona la propiedad `Title` a la que se hace referencia en la expresión lambda para determinar el nombre para mostrar.</span><span class="sxs-lookup"><span data-stu-id="25e09-233">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="25e09-234">La expresión lambda se inspecciona, no se evalúa.</span><span class="sxs-lookup"><span data-stu-id="25e09-234">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="25e09-235">Esto significa que no hay ninguna infracción de acceso si `model`, `model.Movie` o `model.Movie[0]` son `null` o están vacíos.</span><span class="sxs-lookup"><span data-stu-id="25e09-235">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="25e09-236">Al evaluar la expresión lambda (por ejemplo, con `@Html.DisplayFor(modelItem => item.Title)`), se evalúan los valores de propiedad del modelo.</span><span class="sxs-lookup"><span data-stu-id="25e09-236">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>

### <a name="the-model-directive"></a><span data-ttu-id="25e09-237">La directiva @model</span><span class="sxs-lookup"><span data-stu-id="25e09-237">The @model directive</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="25e09-238">La directiva `@model` especifica el tipo del modelo que se pasa a la página de Razor.</span><span class="sxs-lookup"><span data-stu-id="25e09-238">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="25e09-239">En el ejemplo anterior, la línea `@model` permite que la clase derivada de `PageModel` esté disponible en la página de Razor.</span><span class="sxs-lookup"><span data-stu-id="25e09-239">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="25e09-240">El modelo se usa en los [asistentes de HTML](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers)`@Html.DisplayNameFor` y `@Html.DisplayFor` de la página.</span><span class="sxs-lookup"><span data-stu-id="25e09-240">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayFor` [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

### <a name="the-layout-page"></a><span data-ttu-id="25e09-241">Página de diseño</span><span class="sxs-lookup"><span data-stu-id="25e09-241">The layout page</span></span>

<span data-ttu-id="25e09-242">Seleccione los vínculos de menú (**RazorPagesMovie** [Película de Razor Pages], **Home** [Inicio] y **Privacy** [Privacidad]).</span><span class="sxs-lookup"><span data-stu-id="25e09-242">Select the menu links (**RazorPagesMovie**, **Home**, and **Privacy**).</span></span> <span data-ttu-id="25e09-243">Cada página muestra el mismo diseño de menú.</span><span class="sxs-lookup"><span data-stu-id="25e09-243">Each page shows the same menu layout.</span></span> <span data-ttu-id="25e09-244">El diseño de menú se implementa en el archivo *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="25e09-244">The menu layout is implemented in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="25e09-245">Abra el archivo *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="25e09-245">Open the *Pages/Shared/_Layout.cshtml* file.</span></span>

<span data-ttu-id="25e09-246">Las plantillas de [diseño](xref:mvc/views/layout) permiten especificar el diseño del contenedor HTML del sitio en un solo lugar y, después, aplicarlo en varias páginas del sitio.</span><span class="sxs-lookup"><span data-stu-id="25e09-246">[Layout](xref:mvc/views/layout) templates allow you to specify the HTML container layout of your site in one place and then apply it across multiple pages in your site.</span></span> <span data-ttu-id="25e09-247">Busque la línea `@RenderBody()`.</span><span class="sxs-lookup"><span data-stu-id="25e09-247">Find the `@RenderBody()` line.</span></span> <span data-ttu-id="25e09-248">`RenderBody` es un marcador de posición donde se mostrarán todas las vistas específicas de página que cree, *encapsuladas* en la página de diseño.</span><span class="sxs-lookup"><span data-stu-id="25e09-248">`RenderBody` is a placeholder where all the page-specific views you create show up, *wrapped* in the layout page.</span></span> <span data-ttu-id="25e09-249">Por ejemplo, si selecciona el vínculo **Privacy** (Privacidad), la vista **Pages/Privacy.cshtml** se representará dentro del método `RenderBody`.</span><span class="sxs-lookup"><span data-stu-id="25e09-249">For example, if you select the **Privacy** link, the **Pages/Privacy.cshtml** view is rendered inside the `RenderBody` method.</span></span>

<a name="vd"></a>

### <a name="viewdata-and-layout"></a><span data-ttu-id="25e09-250">Propiedades ViewData y Layout</span><span class="sxs-lookup"><span data-stu-id="25e09-250">ViewData and layout</span></span>

<span data-ttu-id="25e09-251">Tenga en cuenta el siguiente código del archivo *Pages/Movies/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="25e09-251">Consider the following code from the *Pages/Movies/Index.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

<span data-ttu-id="25e09-252">El código resaltado anterior es un ejemplo de Razor con una transición a C#.</span><span class="sxs-lookup"><span data-stu-id="25e09-252">The preceding highlighted code is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="25e09-253">Los caracteres `{` y `}` delimitan un bloque de código de C#.</span><span class="sxs-lookup"><span data-stu-id="25e09-253">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="25e09-254">La clase base `PageModel` tiene una propiedad de diccionario `ViewData` que se puede usar para agregar datos que quiera pasar a una vista.</span><span class="sxs-lookup"><span data-stu-id="25e09-254">The `PageModel` base class has a `ViewData` dictionary property that can be used to add data that you want to pass to a View.</span></span> <span data-ttu-id="25e09-255">Puede agregar objetos al diccionario `ViewData` con un patrón de clave/valor.</span><span class="sxs-lookup"><span data-stu-id="25e09-255">You add objects into the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="25e09-256">En el ejemplo anterior, se agrega la propiedad "Title" al diccionario `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="25e09-256">In the preceding sample, the "Title" property is added to the `ViewData` dictionary.</span></span>

<span data-ttu-id="25e09-257">La propiedad "Title" se usa en el archivo *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="25e09-257">The "Title" property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="25e09-258">En el siguiente marcado se muestran las primeras líneas del archivo *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="25e09-258">The following markup shows the first few lines of the *_Layout.cshtml* file.</span></span>

<!-- we need a snapshot copy of layout because we are
changing in in the next step.
-->
[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout.cshtml?highlight=6-99)]

<span data-ttu-id="25e09-259">La línea `@*Markup removed for brevity.*@` es un comentario de Razor que no aparece en el archivo de diseño.</span><span class="sxs-lookup"><span data-stu-id="25e09-259">The line `@*Markup removed for brevity.*@` is a Razor comment which doesn't appear in your layout file.</span></span> <span data-ttu-id="25e09-260">A diferencia de los comentarios HTML (`<!-- -->`), los comentarios de Razor no se envían al cliente.</span><span class="sxs-lookup"><span data-stu-id="25e09-260">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="25e09-261">Actualizar el diseño</span><span class="sxs-lookup"><span data-stu-id="25e09-261">Update the layout</span></span>

<span data-ttu-id="25e09-262">Cambie el elemento `<title>` del archivo *Pages/Shared/_Layout.cshtml* para mostrar **Movie** en lugar de **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="25e09-262">Change the `<title>` element in the *Pages/Shared/_Layout.cshtml* file to display **Movie** rather than **RazorPagesMovie**.</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml?range=1-6&highlight=6)]

<span data-ttu-id="25e09-263">Busque el siguiente elemento delimitador en el archivo *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="25e09-263">Find the following anchor element in the *Pages/Shared/_Layout.cshtml* file.</span></span>

```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Index">RazorPagesMovie</a>
```

<span data-ttu-id="25e09-264">Reemplace el elemento anterior por el marcado siguiente.</span><span class="sxs-lookup"><span data-stu-id="25e09-264">Replace the preceding element with the following markup.</span></span>

```cshtml
<a class="navbar-brand" asp-page="/Movies/Index">RpMovie</a>
```

<span data-ttu-id="25e09-265">El elemento delimitador anterior es un [asistente de etiquetas](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="25e09-265">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="25e09-266">En este caso, se trata de el [asistente de etiquetas Anchor](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="25e09-266">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="25e09-267">El atributo y valor del asistente de etiquetas `asp-page="/Movies/Index"` crea un vínculo a la página de Razor `/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="25e09-267">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span> <span data-ttu-id="25e09-268">El valor de atributo `asp-area` está vacío, por lo que no se usa el área del vínculo.</span><span class="sxs-lookup"><span data-stu-id="25e09-268">The `asp-area` attribute value is empty, so the area isn't used in the link.</span></span> <span data-ttu-id="25e09-269">Consulte [Áreas](xref:mvc/controllers/areas) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="25e09-269">See [Areas](xref:mvc/controllers/areas) for more information.</span></span>

<span data-ttu-id="25e09-270">Guarde los cambios y pruebe la aplicación haciendo clic en el vínculo **RpMovie**.</span><span class="sxs-lookup"><span data-stu-id="25e09-270">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="25e09-271">Si tiene cualquier problema, consulte el archivo [_Layout.cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml) en GitHub.</span><span class="sxs-lookup"><span data-stu-id="25e09-271">See the [_Layout.cshtml](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml) file in GitHub if you have any problems.</span></span>

<span data-ttu-id="25e09-272">Pruebe los otros vínculos (**Inicio**, **RpMovie**, **Crear**, **Editar** y **Eliminar**).</span><span class="sxs-lookup"><span data-stu-id="25e09-272">Test the other links (**Home**, **RpMovie**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="25e09-273">Cada página establece el título, que puede ver en la pestaña del explorador. Al marcar una página, se usa el título para el marcador.</span><span class="sxs-lookup"><span data-stu-id="25e09-273">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span>

> [!NOTE]
> <span data-ttu-id="25e09-274">Es posible que no pueda escribir comas decimales en el campo `Price`.</span><span class="sxs-lookup"><span data-stu-id="25e09-274">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="25e09-275">Para que la [validación de jQuery](https://jqueryvalidation.org/) sea compatible con configuraciones regionales distintas del inglés que usan una coma (",") en lugar de un punto decimal y formatos de fecha distintos del de Estados Unidos, debe seguir unos pasos para globalizar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="25e09-275">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="25e09-276">Consulte el [problema 4076 de GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) para obtener instrucciones sobre cómo agregar la coma decimal.</span><span class="sxs-lookup"><span data-stu-id="25e09-276">This [GitHub issue 4076](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) for instructions on adding decimal comma.</span></span>

<span data-ttu-id="25e09-277">La propiedad `Layout` se establece en el archivo *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="25e09-277">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/_ViewStart.cshtml)]

<span data-ttu-id="25e09-278">El marcado anterior establece el archivo de diseño en *Pages/Shared/_Layout.cshtml* para todos los archivos de Razor en la carpeta *Pages*.</span><span class="sxs-lookup"><span data-stu-id="25e09-278">The preceding markup sets the layout file to *Pages/Shared/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="25e09-279">Vea [Layout](xref:razor-pages/index#layout) (Diseño) para más información.</span><span class="sxs-lookup"><span data-stu-id="25e09-279">See [Layout](xref:razor-pages/index#layout) for more information.</span></span>

### <a name="the-create-page-model"></a><span data-ttu-id="25e09-280">Modelo de página Crear</span><span class="sxs-lookup"><span data-stu-id="25e09-280">The Create page model</span></span>

<span data-ttu-id="25e09-281">Examine el modelo de página *Pages/Movies/Create.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="25e09-281">Examine the *Pages/Movies/Create.cshtml.cs* page model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

<span data-ttu-id="25e09-282">El método `OnGet` inicializa cualquier estado necesario para la página.</span><span class="sxs-lookup"><span data-stu-id="25e09-282">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="25e09-283">La página Crear no tiene ningún estado que inicializar, de modo que se devuelve `Page`.</span><span class="sxs-lookup"><span data-stu-id="25e09-283">The Create page doesn't have any state to initialize, so `Page` is returned.</span></span> <span data-ttu-id="25e09-284">Más adelante en el tutorial veremos el estado de inicialización del método `OnGet`.</span><span class="sxs-lookup"><span data-stu-id="25e09-284">Later in the tutorial you see `OnGet` method initialize state.</span></span> <span data-ttu-id="25e09-285">El método `Page` crea un objeto `PageResult` que representa la página *Create.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="25e09-285">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="25e09-286">La propiedad `Movie` usa el atributo `[BindProperty]` para participar en el [enlace de modelos](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="25e09-286">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="25e09-287">Cuando el formulario de creación publica los valores del formulario, el tiempo de ejecución de ASP.NET Core enlaza los valores publicados con el modelo `Movie`.</span><span class="sxs-lookup"><span data-stu-id="25e09-287">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="25e09-288">El método `OnPostAsync` se ejecuta cuando la página publica los datos del formulario:</span><span class="sxs-lookup"><span data-stu-id="25e09-288">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="25e09-289">Si hay algún error de modelo, se vuelve a mostrar el formulario, junto con los datos del formulario publicados.</span><span class="sxs-lookup"><span data-stu-id="25e09-289">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="25e09-290">La mayoría de los errores de modelo se pueden capturar en el cliente antes de que se publique el formulario.</span><span class="sxs-lookup"><span data-stu-id="25e09-290">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="25e09-291">Un ejemplo de un error de modelo consiste en publicar un valor para el campo de fecha que no se puede convertir en una fecha.</span><span class="sxs-lookup"><span data-stu-id="25e09-291">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="25e09-292">Más adelante en el tutorial, hablaremos de la validación del lado cliente y de la validación de modelos.</span><span class="sxs-lookup"><span data-stu-id="25e09-292">Client-side validation and model validation are discussed later in the tutorial.</span></span>

<span data-ttu-id="25e09-293">Si no hay ningún error de modelo, los datos se guardan y el explorador se redirige a la página Índice.</span><span class="sxs-lookup"><span data-stu-id="25e09-293">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="25e09-294">Página de Razor Crear</span><span class="sxs-lookup"><span data-stu-id="25e09-294">The Create Razor Page</span></span>

<span data-ttu-id="25e09-295">Examine el archivo de la página de Razor *Pages/Movies/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="25e09-295">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="25e09-296">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="25e09-296">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="25e09-297">Visual Studio muestra la etiqueta `<form method="post">` con una fuente negrita diferenciada que se aplica a los asistentes de etiquetas:</span><span class="sxs-lookup"><span data-stu-id="25e09-297">Visual Studio displays the `<form method="post">` tag in a distinctive bold font used for Tag Helpers:</span></span>

![Vista de VS17 de la página Create.cshtml](page/_static/th.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="25e09-299">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="25e09-299">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="25e09-300">Para obtener más información sobre los asistentes de etiquetas como `<form method="post">`, consulte [Asistentes de etiquetas en ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="25e09-300">For more information on Tag Helpers such as `<form method="post">`, see [Tag Helpers in ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="25e09-301">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="25e09-301">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="25e09-302">Visual Studio para Mac muestra la etiqueta `<form method="post">` con una fuente negrita diferenciada que se aplica a los asistentes de etiquetas.</span><span class="sxs-lookup"><span data-stu-id="25e09-302">Visual Studio for Mac displays the `<form method="post">` tag in a distinctive bold font used for Tag Helpers.</span></span>

---

<span data-ttu-id="25e09-303">El elemento `<form method="post">` es un [asistente de etiquetas de formulario](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="25e09-303">The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="25e09-304">El asistente de etiquetas de formulario incluye automáticamente un [token antifalsificación](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="25e09-304">The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).</span></span>

<span data-ttu-id="25e09-305">El motor de scaffolding crea un marcado de Razor para cada campo del modelo (excepto el identificador) similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="25e09-305">The scaffolding engine creates Razor markup for each field in the model (except the ID) similar to the following:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

<span data-ttu-id="25e09-306">Los [asistentes de etiquetas de validación](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` y `<span asp-validation-for`) muestran errores de validación.</span><span class="sxs-lookup"><span data-stu-id="25e09-306">The [Validation Tag Helpers](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` and `<span asp-validation-for`) display validation errors.</span></span> <span data-ttu-id="25e09-307">La validación se trata con más detalle en un punto posterior de esta serie.</span><span class="sxs-lookup"><span data-stu-id="25e09-307">Validation is covered in more detail later in this series.</span></span>

<span data-ttu-id="25e09-308">El [asistente de etiquetas](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) genera el título de la etiqueta y el atributo `for` para la propiedad `Title`.</span><span class="sxs-lookup"><span data-stu-id="25e09-308">The [Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generates the label caption and `for` attribute for the `Title` property.</span></span>

<span data-ttu-id="25e09-309">El [asistente de etiquetas de entrada](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`) usa los atributos [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) y genera los atributos HTML necesarios para la validación de jQuery en el lado del cliente.</span><span class="sxs-lookup"><span data-stu-id="25e09-309">The [Input Tag Helper](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control">`) uses the [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="25e09-310">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="25e09-310">Additional resources</span></span>

* [<span data-ttu-id="25e09-311">Versión en YouTube de este tutorial</span><span class="sxs-lookup"><span data-stu-id="25e09-311">YouTube version of this tutorial</span></span>](https://youtu.be/zxgKjPYnOMM)

> [!div class="step-by-step"]
> <span data-ttu-id="25e09-312">[Anterior: Adición de un modelo](xref:tutorials/razor-pages/model)
> [Siguiente: Base de datos](xref:tutorials/razor-pages/sql)</span><span class="sxs-lookup"><span data-stu-id="25e09-312">[Previous: Adding a model](xref:tutorials/razor-pages/model)
[Next: Database](xref:tutorials/razor-pages/sql)</span></span>

::: moniker-end