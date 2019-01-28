---
title: Páginas de Razor con scaffolding en ASP.NET Core
author: rick-anderson
description: Explica las páginas de Razor generadas por la técnica scaffolding.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 12/4/2018
uid: tutorials/razor-pages/page
ms.openlocfilehash: c902e9e258b4f436e2f19e2940246b80549238b8
ms.sourcegitcommit: ca5f03210bedc61c6639a734ae5674bfe095dee8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2019
ms.locfileid: "55073119"
---
# <a name="scaffolded-razor-pages-in-aspnet-core"></a><span data-ttu-id="8756d-103">Páginas de Razor con scaffolding en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8756d-103">Scaffolded Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="8756d-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8756d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8756d-105">En este tutorial se examinan las páginas de Razor creadas por la técnica scaffolding en el tutorial anterior.</span><span class="sxs-lookup"><span data-stu-id="8756d-105">This tutorial examines the Razor Pages created by scaffolding in the previous tutorial.</span></span>

<span data-ttu-id="8756d-106">[Vea o descargue](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22) un ejemplo.</span><span class="sxs-lookup"><span data-stu-id="8756d-106">[View or download](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22) sample.</span></span>

## <a name="the-create-delete-details-and-edit-pages"></a><span data-ttu-id="8756d-107">Páginas de creación, eliminación, detalles y edición</span><span class="sxs-lookup"><span data-stu-id="8756d-107">The Create, Delete, Details, and Edit pages</span></span>

<span data-ttu-id="8756d-108">Examine el modelo de página *Pages/Movies/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="8756d-108">Examine the *Pages/Movies/Index.cshtml.cs* Page Model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs)]

<span data-ttu-id="8756d-109">Las páginas de Razor se derivan de `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="8756d-109">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="8756d-110">Por convención, la clase derivada de `PageModel` se denomina `<PageName>Model`.</span><span class="sxs-lookup"><span data-stu-id="8756d-110">By convention, the `PageModel`-derived class is called `<PageName>Model`.</span></span> <span data-ttu-id="8756d-111">El constructor aplica la [inserción de dependencias](xref:fundamentals/dependency-injection) para agregar el `RazorPagesMovieContext` a la página.</span><span class="sxs-lookup"><span data-stu-id="8756d-111">The constructor uses [dependency injection](xref:fundamentals/dependency-injection) to add the `RazorPagesMovieContext` to the page.</span></span> <span data-ttu-id="8756d-112">Todas las páginas con scaffolding siguen este patrón.</span><span class="sxs-lookup"><span data-stu-id="8756d-112">All the scaffolded pages follow this pattern.</span></span> <span data-ttu-id="8756d-113">Vea [Código asincrónico](xref:data/ef-rp/intro#asynchronous-code) para obtener más información sobre programación asincrónica con Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="8756d-113">See [Asynchronous code](xref:data/ef-rp/intro#asynchronous-code) for more information on asynchronous programing with Entity Framework.</span></span>

<span data-ttu-id="8756d-114">Cuando se efectúa una solicitud para la página, el método `OnGetAsync` devuelve una lista de películas a la página de Razor.</span><span class="sxs-lookup"><span data-stu-id="8756d-114">When a request is made for the page, the `OnGetAsync` method returns a list of movies to the Razor Page.</span></span> <span data-ttu-id="8756d-115">Se llama a `OnGetAsync` o a `OnGet` en una página de Razor para inicializar el estado de la página.</span><span class="sxs-lookup"><span data-stu-id="8756d-115">`OnGetAsync` or `OnGet` is called on a Razor Page to initialize the state for the page.</span></span> <span data-ttu-id="8756d-116">En este caso, `OnGetAsync` obtiene una lista de películas y las muestra.</span><span class="sxs-lookup"><span data-stu-id="8756d-116">In this case, `OnGetAsync` gets a list of movies and displays them.</span></span>

<span data-ttu-id="8756d-117">Cuando `OnGet` devuelve `void` o `OnGetAsync` devuelve `Task`, no se utiliza ningún método de devolución.</span><span class="sxs-lookup"><span data-stu-id="8756d-117">When `OnGet` returns `void` or `OnGetAsync` returns`Task`, no return method is used.</span></span> <span data-ttu-id="8756d-118">Cuando el tipo de valor devuelto es `IActionResult` o `Task<IActionResult>`, se debe proporcionar una instrucción return.</span><span class="sxs-lookup"><span data-stu-id="8756d-118">When the return type is `IActionResult` or `Task<IActionResult>`, a return statement must be provided.</span></span> <span data-ttu-id="8756d-119">Por ejemplo, el método *Pages/Movies/Create.cshtml.cs*`OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="8756d-119">For example, the *Pages/Movies/Create.cshtml.cs* `OnPostAsync` method:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml.cs?name=snippet)]

<a name="index"></a> <span data-ttu-id="8756d-120">Examine la página de Razor *Pages/Movies/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="8756d-120">Examine the *Pages/Movies/Index.cshtml* Razor Page:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml)]

<span data-ttu-id="8756d-121">Razor puede realizar la transición de HTML a C# o a un marcado específico de Razor.</span><span class="sxs-lookup"><span data-stu-id="8756d-121">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="8756d-122">Cuando el símbolo `@` va seguido de una [palabra clave reservada de Razor](xref:mvc/views/razor#razor-reserved-keywords), realiza una transición a un marcado específico de Razor; en caso contrario, realiza la transición a C#.</span><span class="sxs-lookup"><span data-stu-id="8756d-122">When an `@` symbol is followed by a [Razor reserved keyword](xref:mvc/views/razor#razor-reserved-keywords), it transitions into Razor-specific markup, otherwise it transitions into C#.</span></span>

<span data-ttu-id="8756d-123">La directiva de Razor `@page` convierte el archivo en una acción de MVC, lo que significa que puede controlar las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="8756d-123">The `@page` Razor directive makes the file into an MVC action, which means that it can handle requests.</span></span> <span data-ttu-id="8756d-124">`@page` debe ser la primera directiva de Razor de una página.</span><span class="sxs-lookup"><span data-stu-id="8756d-124">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="8756d-125">`@page` es un ejemplo de la transición a un marcado específico de Razor.</span><span class="sxs-lookup"><span data-stu-id="8756d-125">`@page` is an example of transitioning into Razor-specific markup.</span></span> <span data-ttu-id="8756d-126">Vea [Razor syntax](xref:mvc/views/razor#razor-syntax) (Sintaxis de Razor) para más información.</span><span class="sxs-lookup"><span data-stu-id="8756d-126">See [Razor syntax](xref:mvc/views/razor#razor-syntax) for more information.</span></span>

<span data-ttu-id="8756d-127">Examine la expresión lambda usada en el siguiente asistente de HTML:</span><span class="sxs-lookup"><span data-stu-id="8756d-127">Examine the lambda expression used in the following HTML Helper:</span></span>

```cshtml
@Html.DisplayNameFor(model => model.Movie[0].Title))
```

<span data-ttu-id="8756d-128">El asistente de HTML `DisplayNameFor` inspecciona la propiedad `Title` a la que se hace referencia en la expresión lambda para determinar el nombre para mostrar.</span><span class="sxs-lookup"><span data-stu-id="8756d-128">The `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="8756d-129">La expresión lambda se inspecciona, no se evalúa.</span><span class="sxs-lookup"><span data-stu-id="8756d-129">The lambda expression is inspected rather than evaluated.</span></span> <span data-ttu-id="8756d-130">Esto significa que no hay ninguna infracción de acceso si `model`, `model.Movie` o `model.Movie[0]` son `null` o están vacíos.</span><span class="sxs-lookup"><span data-stu-id="8756d-130">That means there is no access violation when `model`, `model.Movie`, or `model.Movie[0]` are `null` or empty.</span></span> <span data-ttu-id="8756d-131">Al evaluar la expresión lambda (por ejemplo, con `@Html.DisplayFor(modelItem => item.Title)`), se evalúan los valores de propiedad del modelo.</span><span class="sxs-lookup"><span data-stu-id="8756d-131">When the lambda expression is evaluated (for example, with `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<a name="md"></a>
### <a name="the-model-directive"></a><span data-ttu-id="8756d-132">La directiva @model </span><span class="sxs-lookup"><span data-stu-id="8756d-132">The @model directive</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-2&highlight=2)]

<span data-ttu-id="8756d-133">La directiva `@model` especifica el tipo del modelo que se pasa a la página de Razor.</span><span class="sxs-lookup"><span data-stu-id="8756d-133">The `@model` directive specifies the type of the model passed to the Razor Page.</span></span> <span data-ttu-id="8756d-134">En el ejemplo anterior, la línea `@model` permite que la clase derivada de `PageModel` esté disponible en la página de Razor.</span><span class="sxs-lookup"><span data-stu-id="8756d-134">In the preceding example, the `@model` line makes the `PageModel`-derived class available to the Razor Page.</span></span> <span data-ttu-id="8756d-135">El modelo se usa en los [asistentes de HTML](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers)`@Html.DisplayNameFor` y `@Html.DisplayFor` de la página.</span><span class="sxs-lookup"><span data-stu-id="8756d-135">The model is used in the `@Html.DisplayNameFor` and `@Html.DisplayFor` [HTML Helpers](/aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs#understanding-html-helpers) on the page.</span></span>

### <a name="the-layout-page"></a><span data-ttu-id="8756d-136">Página de diseño</span><span class="sxs-lookup"><span data-stu-id="8756d-136">The layout page</span></span>

<span data-ttu-id="8756d-137">Seleccione los vínculos de menú (**RazorPagesMovie** [Película de Razor Pages], **Home** [Inicio] y **Privacy** [Privacidad]).</span><span class="sxs-lookup"><span data-stu-id="8756d-137">Select the menu links (**RazorPagesMovie**, **Home**, and **Privacy**).</span></span> <span data-ttu-id="8756d-138">Cada página muestra el mismo diseño de menú.</span><span class="sxs-lookup"><span data-stu-id="8756d-138">Each page shows the same menu layout.</span></span> <span data-ttu-id="8756d-139">El diseño de menú se implementa en el archivo *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8756d-139">The menu layout is implemented in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="8756d-140">Abra el archivo *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8756d-140">Open the *Pages/Shared/_Layout.cshtml* file.</span></span>

<span data-ttu-id="8756d-141">Las plantillas de [diseño](xref:mvc/views/layout) permiten especificar el diseño del contenedor HTML del sitio en un solo lugar y, después, aplicarlo en varias páginas del sitio.</span><span class="sxs-lookup"><span data-stu-id="8756d-141">[Layout](xref:mvc/views/layout) templates allow you to specify the HTML container layout of your site in one place and then apply it across multiple pages in your site.</span></span> <span data-ttu-id="8756d-142">Busque la línea `@RenderBody()`.</span><span class="sxs-lookup"><span data-stu-id="8756d-142">Find the `@RenderBody()` line.</span></span> <span data-ttu-id="8756d-143">`RenderBody` es un marcador de posición donde se mostrarán todas las vistas específicas de página que cree, *encapsuladas* en la página de diseño.</span><span class="sxs-lookup"><span data-stu-id="8756d-143">`RenderBody` is a placeholder where all the page-specific views you create show up, *wrapped* in the layout page.</span></span> <span data-ttu-id="8756d-144">Por ejemplo, si selecciona el vínculo **Privacy** (Privacidad), la vista **Pages/Privacy.cshtml** se representará dentro del método `RenderBody`.</span><span class="sxs-lookup"><span data-stu-id="8756d-144">For example, if you select the **Privacy** link, the **Pages/Privacy.cshtml** view is rendered inside the `RenderBody` method.</span></span>

<a name="vd"></a>
### <a name="viewdata-and-layout"></a><span data-ttu-id="8756d-145">Propiedades ViewData y Layout</span><span class="sxs-lookup"><span data-stu-id="8756d-145">ViewData and layout</span></span>

<span data-ttu-id="8756d-146">Tenga en cuenta el siguiente código del archivo *Pages/Movies/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="8756d-146">Consider the following code from the *Pages/Movies/Index.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Index.cshtml?range=1-6&highlight=4-999)]

<span data-ttu-id="8756d-147">El código resaltado anterior es un ejemplo de Razor con una transición a C#.</span><span class="sxs-lookup"><span data-stu-id="8756d-147">The preceding highlighted code is an example of Razor transitioning into C#.</span></span> <span data-ttu-id="8756d-148">Los caracteres `{` y `}` delimitan un bloque de código de C#.</span><span class="sxs-lookup"><span data-stu-id="8756d-148">The `{` and `}` characters enclose a block of C# code.</span></span>

<span data-ttu-id="8756d-149">La clase base `PageModel` tiene una propiedad de diccionario `ViewData` que se puede usar para agregar datos que quiera pasar a una vista.</span><span class="sxs-lookup"><span data-stu-id="8756d-149">The `PageModel` base class has a `ViewData` dictionary property that can be used to add data that you want to pass to a View.</span></span> <span data-ttu-id="8756d-150">Puede agregar objetos al diccionario `ViewData` con un patrón de clave/valor.</span><span class="sxs-lookup"><span data-stu-id="8756d-150">You add objects into the `ViewData` dictionary using a key/value pattern.</span></span> <span data-ttu-id="8756d-151">En el ejemplo anterior, se agrega la propiedad "Title" al diccionario `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="8756d-151">In the preceding sample, the "Title" property is added to the `ViewData` dictionary.</span></span> 

<span data-ttu-id="8756d-152">La propiedad "Title" se usa en el archivo *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8756d-152">The "Title" property is used in the *Pages/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="8756d-153">En el siguiente marcado se muestran las primeras líneas del archivo *_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8756d-153">The following markup shows the first few lines of the *_Layout.cshtml* file.</span></span>

<!-- we need a snapshot copy of layout because we are
changing in in the next step. 
-->
[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/NU/_Layout.cshtml?highlight=6-99)]

<span data-ttu-id="8756d-154">La línea `@*Markup removed for brevity.*@` es un comentario de Razor que no aparece en el archivo de diseño.</span><span class="sxs-lookup"><span data-stu-id="8756d-154">The line `@*Markup removed for brevity.*@` is a Razor comment which doesn't appear in your layout file.</span></span> <span data-ttu-id="8756d-155">A diferencia de los comentarios HTML (`<!-- -->`), los comentarios de Razor no se envían al cliente.</span><span class="sxs-lookup"><span data-stu-id="8756d-155">Unlike HTML comments (`<!-- -->`), Razor comments are not sent to the client.</span></span>

### <a name="update-the-layout"></a><span data-ttu-id="8756d-156">Actualizar el diseño</span><span class="sxs-lookup"><span data-stu-id="8756d-156">Update the layout</span></span>

<span data-ttu-id="8756d-157">Al cambiar el elemento `<title>` del archivo *Pages/Shared/_Layout.cshtml*, se muestra **Movie** en lugar de **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="8756d-157">Change the `<title>` element in the *Pages/Shared/_Layout.cshtml* file display **Movie** rather than **RazorPagesMovie**.</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml?range=1-6&highlight=6)]


<span data-ttu-id="8756d-158">Busque el siguiente elemento delimitador en el archivo *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8756d-158">Find the following anchor element in the *Pages/Shared/_Layout.cshtml* file.</span></span>

```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Index">RazorPagesMovie</a>
```

<span data-ttu-id="8756d-159">Reemplace el elemento anterior por el marcado siguiente.</span><span class="sxs-lookup"><span data-stu-id="8756d-159">Replace the preceding element with the following markup.</span></span>

```cshtml
<a class="navbar-brand" asp-page="/Movies/Index">RpMovie</a>
```

<span data-ttu-id="8756d-160">El elemento delimitador anterior es un [asistente de etiquetas](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="8756d-160">The preceding anchor element is a [Tag Helper](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="8756d-161">En este caso, se trata de el [asistente de etiquetas Anchor](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="8756d-161">In this case, it's the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper).</span></span> <span data-ttu-id="8756d-162">El atributo y valor del asistente de etiquetas `asp-page="/Movies/Index"` crea un vínculo a la página de Razor `/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="8756d-162">The `asp-page="/Movies/Index"` Tag Helper attribute and value creates a link to the `/Movies/Index` Razor Page.</span></span> <span data-ttu-id="8756d-163">El valor de atributo `asp-area` está vacío, por lo que no se usa el área del vínculo.</span><span class="sxs-lookup"><span data-stu-id="8756d-163">The `asp-area` attribute value is empty, so the area isn't used in the link.</span></span> <span data-ttu-id="8756d-164">Consulte [Áreas](xref:mvc/controllers/areas) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="8756d-164">See [Areas](xref:mvc/controllers/areas) for more information.</span></span>

<span data-ttu-id="8756d-165">Guarde los cambios y pruebe la aplicación haciendo clic en el vínculo **RpMovie**.</span><span class="sxs-lookup"><span data-stu-id="8756d-165">Save your changes, and test the app by clicking on the **RpMovie** link.</span></span> <span data-ttu-id="8756d-166">Si tiene cualquier problema, consulte el archivo [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml) en GitHub.</span><span class="sxs-lookup"><span data-stu-id="8756d-166">See the [_Layout.cshtml](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Shared/_Layout.cshtml) file in GitHub if you have any problems.</span></span>

<span data-ttu-id="8756d-167">Pruebe los otros vínculos (**Inicio**, **RpMovie**, **Crear**, **Editar** y **Eliminar**).</span><span class="sxs-lookup"><span data-stu-id="8756d-167">Test the other links (**Home**, **RpMovie**, **Create**, **Edit**, and **Delete**).</span></span> <span data-ttu-id="8756d-168">Cada página establece el título, que puede ver en la pestaña del explorador. Al marcar una página, se usa el título para el marcador.</span><span class="sxs-lookup"><span data-stu-id="8756d-168">Each page sets the title, which you can see in the browser tab. When you bookmark a page, the title is used for the bookmark.</span></span>

> [!NOTE]
> <span data-ttu-id="8756d-169">Es posible que no pueda escribir comas decimales en el campo `Price`.</span><span class="sxs-lookup"><span data-stu-id="8756d-169">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="8756d-170">Para que la [validación de jQuery](https://jqueryvalidation.org/) sea compatible con configuraciones regionales distintas del inglés que usan una coma (",") en lugar de un punto decimal y formatos de fecha distintos del de Estados Unidos, debe seguir unos pasos para globalizar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8756d-170">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="8756d-171">Consulte el [problema 4076 de GitHub](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) para obtener instrucciones sobre cómo agregar la coma decimal.</span><span class="sxs-lookup"><span data-stu-id="8756d-171">This [GitHub issue 4076](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420) for instructions on adding decimal comma.</span></span>

<span data-ttu-id="8756d-172">La propiedad `Layout` se establece en el archivo *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="8756d-172">The `Layout` property is set in the *Pages/_ViewStart.cshtml* file:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/_ViewStart.cshtml)]

<span data-ttu-id="8756d-173">El marcado anterior establece el archivo de diseño en *Pages/Shared/_Layout.cshtml* para todos los archivos de Razor en la carpeta *Pages*.</span><span class="sxs-lookup"><span data-stu-id="8756d-173">The preceding markup sets the layout file to *Pages/Shared/_Layout.cshtml* for all Razor files under the *Pages* folder.</span></span> <span data-ttu-id="8756d-174">Vea [Layout](xref:razor-pages/index#layout) (Diseño) para más información.</span><span class="sxs-lookup"><span data-stu-id="8756d-174">See [Layout](xref:razor-pages/index#layout) for more information.</span></span>

### <a name="the-create-page-model"></a><span data-ttu-id="8756d-175">Modelo de página Crear</span><span class="sxs-lookup"><span data-stu-id="8756d-175">The Create page model</span></span>

<span data-ttu-id="8756d-176">Examine el modelo de página *Pages/Movies/Create.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="8756d-176">Examine the *Pages/Movies/Create.cshtml.cs* page model:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetALL)]

<span data-ttu-id="8756d-177">El método `OnGet` inicializa cualquier estado necesario para la página.</span><span class="sxs-lookup"><span data-stu-id="8756d-177">The `OnGet` method initializes any state needed for the page.</span></span> <span data-ttu-id="8756d-178">La página Crear no tiene ningún estado que inicializar, de modo que se devuelve `Page`.</span><span class="sxs-lookup"><span data-stu-id="8756d-178">The Create page doesn't have any state to initialize, so `Page` is returned.</span></span> <span data-ttu-id="8756d-179">Más adelante en el tutorial veremos el estado de inicialización del método `OnGet`.</span><span class="sxs-lookup"><span data-stu-id="8756d-179">Later in the tutorial you see `OnGet` method initialize state.</span></span> <span data-ttu-id="8756d-180">El método `Page` crea un objeto `PageResult` que representa la página *Create.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8756d-180">The `Page` method creates a `PageResult` object that renders the *Create.cshtml* page.</span></span>

<span data-ttu-id="8756d-181">La propiedad `Movie` usa el atributo `[BindProperty]` para participar en el [enlace de modelos](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="8756d-181">The `Movie` property uses the `[BindProperty]` attribute to opt-in to [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="8756d-182">Cuando el formulario de creación publica los valores del formulario, el tiempo de ejecución de ASP.NET Core enlaza los valores publicados con el modelo `Movie`.</span><span class="sxs-lookup"><span data-stu-id="8756d-182">When the Create form posts the form values, the ASP.NET Core runtime binds the posted values to the `Movie` model.</span></span>

<span data-ttu-id="8756d-183">El método `OnPostAsync` se ejecuta cuando la página publica los datos del formulario:</span><span class="sxs-lookup"><span data-stu-id="8756d-183">The `OnPostAsync` method is run when the page posts form data:</span></span>

[!code-csharp[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml.cs?name=snippetPost)]

<span data-ttu-id="8756d-184">Si hay algún error de modelo, se vuelve a mostrar el formulario, junto con los datos del formulario publicados.</span><span class="sxs-lookup"><span data-stu-id="8756d-184">If there are any model errors, the form is redisplayed, along with any form data posted.</span></span> <span data-ttu-id="8756d-185">La mayoría de los errores de modelo se pueden capturar en el cliente antes de que se publique el formulario.</span><span class="sxs-lookup"><span data-stu-id="8756d-185">Most model errors can be caught on the client-side before the form is posted.</span></span> <span data-ttu-id="8756d-186">Un ejemplo de un error de modelo consiste en publicar un valor para el campo de fecha que no se puede convertir en una fecha.</span><span class="sxs-lookup"><span data-stu-id="8756d-186">An example of a model error is posting a value for the date field that cannot be converted to a date.</span></span> <span data-ttu-id="8756d-187">Más adelante en el tutorial, hablaremos de la validación del lado cliente y de la validación de modelos.</span><span class="sxs-lookup"><span data-stu-id="8756d-187">Client-side validation and model validation are discussed later in the tutorial.</span></span>

<span data-ttu-id="8756d-188">Si no hay ningún error de modelo, los datos se guardan y el explorador se redirige a la página Índice.</span><span class="sxs-lookup"><span data-stu-id="8756d-188">If there are no model errors, the data is saved, and the browser is redirected to the Index page.</span></span>

### <a name="the-create-razor-page"></a><span data-ttu-id="8756d-189">Página de Razor Crear</span><span class="sxs-lookup"><span data-stu-id="8756d-189">The Create Razor Page</span></span>

<span data-ttu-id="8756d-190">Examine el archivo de la página de Razor *Pages/Movies/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="8756d-190">Examine the *Pages/Movies/Create.cshtml* Razor Page file:</span></span>

[!code-cshtml[](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml)]

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8756d-191">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8756d-191">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="8756d-192">Visual Studio muestra la etiqueta `<form method="post">` con una fuente negrita diferenciada que se aplica a los asistentes de etiquetas:</span><span class="sxs-lookup"><span data-stu-id="8756d-192">Visual Studio displays the `<form method="post">` tag in a distinctive bold font used for Tag Helpers:</span></span>

<span data-ttu-id="8756d-193">![Vista de VS17 de la página Create.cshtml](page/_static/th.png)
<!-- Code --------------------------></span><span class="sxs-lookup"><span data-stu-id="8756d-193">![VS17 view of Create.cshtml page](page/_static/th.png)
<!-- Code --------------------------></span></span>
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8756d-194">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8756d-194">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="8756d-195">Para obtener más información sobre los asistentes de etiquetas como `<form method="post">`, consulte [Asistentes de etiquetas en ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="8756d-195">For more information on Tag Helpers such as `<form method="post">`, see [Tag Helpers in ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span></span>

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8756d-196">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="8756d-196">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="8756d-197">Visual Studio para Mac muestra la etiqueta `<form method="post">` con una fuente negrita diferenciada que se aplica a los asistentes de etiquetas.</span><span class="sxs-lookup"><span data-stu-id="8756d-197">Visual Studio for Mac displays the `<form method="post">` tag in a distinctive bold font used for Tag Helpers.</span></span>

---  
<!-- End of VS tabs -->

<span data-ttu-id="8756d-198">El elemento `<form method="post">` es un [asistente de etiquetas de formulario](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="8756d-198">The `<form method="post">` element is a [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="8756d-199">El asistente de etiquetas de formulario incluye automáticamente un [token antifalsificación](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="8756d-199">The Form Tag Helper automatically includes an [antiforgery token](xref:security/anti-request-forgery).</span></span>

<span data-ttu-id="8756d-200">El motor de scaffolding crea un marcado de Razor para cada campo del modelo (excepto el identificador) similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="8756d-200">The scaffolding engine creates Razor markup for each field in the model (except the ID) similar to the following:</span></span>

[!code-cshtml[](~/tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

<span data-ttu-id="8756d-201">Los [asistentes de etiquetas de validación](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` y ` <span asp-validation-for`) muestran errores de validación.</span><span class="sxs-lookup"><span data-stu-id="8756d-201">The [Validation Tag Helpers](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` and ` <span asp-validation-for`) display validation errors.</span></span> <span data-ttu-id="8756d-202">La validación se trata con más detalle en un punto posterior de esta serie.</span><span class="sxs-lookup"><span data-stu-id="8756d-202">Validation is covered in more detail later in this series.</span></span>

<span data-ttu-id="8756d-203">El [asistente de etiquetas](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) genera el título de la etiqueta y el atributo `for` para la propiedad `Title`.</span><span class="sxs-lookup"><span data-stu-id="8756d-203">The [Label Tag Helper](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) generates the label caption and `for` attribute for the `Title` property.</span></span>

<span data-ttu-id="8756d-204">El [asistente de etiquetas de entrada](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) usa los atributos [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) y genera los atributos HTML necesarios para la validación de jQuery en el lado del cliente.</span><span class="sxs-lookup"><span data-stu-id="8756d-204">The [Input Tag Helper](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) uses the [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8756d-205">[Anterior: Adición de un modelo](xref:tutorials/razor-pages/model)
> [Siguiente: Base de datos](xref:tutorials/razor-pages/sql)</span><span class="sxs-lookup"><span data-stu-id="8756d-205">[Previous: Adding a model](xref:tutorials/razor-pages/model)
[Next: Data Base](xref:tutorials/razor-pages/sql)</span></span>
