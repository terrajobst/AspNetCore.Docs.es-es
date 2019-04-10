---
title: Introducción a las páginas de Razor en ASP.NET Core
author: Rick-Anderson
description: Obtenga información sobre cómo las páginas de Razor de ASP.NET Core facilitan la programación de escenarios centrados en páginas y hacen que resulte más productiva que con MVC.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 04/06/2019
uid: razor-pages/index
ms.openlocfilehash: e79ce1e2ddfc0e1a4d72e3f67f702d6eb938b8d3
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/10/2019
ms.locfileid: "59468700"
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="a7051-103">Introducción a las páginas de Razor en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a7051-103">Introduction to Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="a7051-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Ryan Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="a7051-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="a7051-105">Las páginas de Razor son una nueva característica de ASP.NET Core MVC que facilita la codificación de escenarios centrados en páginas y hace que sea más productiva.</span><span class="sxs-lookup"><span data-stu-id="a7051-105">Razor Pages is a new aspect of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="a7051-106">Si busca un tutorial que use el enfoque Model-View-Controller, consulte [Introducción a ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="a7051-106">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="a7051-107">En este documento se proporciona una introducción a las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="a7051-107">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="a7051-108">No es un tutorial paso a paso.</span><span class="sxs-lookup"><span data-stu-id="a7051-108">It's not a step by step tutorial.</span></span> <span data-ttu-id="a7051-109">Si encuentra que alguna sección es demasiado avanzada, consulte [Introducción a las páginas de Razor](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="a7051-109">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="a7051-110">Para obtener información general de ASP.NET Core, vea [Introducción a ASP.NET Core](xref:index).</span><span class="sxs-lookup"><span data-stu-id="a7051-110">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a7051-111">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="a7051-111">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

<a name="rpvs17"></a>

## <a name="create-a-razor-pages-project"></a><span data-ttu-id="a7051-112">Creación de un proyecto de Razor Pages</span><span class="sxs-lookup"><span data-stu-id="a7051-112">Create a Razor Pages project</span></span>

# [<a name="visual-studio"></a><span data-ttu-id="a7051-113">Programa para la mejora</span><span class="sxs-lookup"><span data-stu-id="a7051-113">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a7051-114">Vea [Introducción a Razor Pages](xref:tutorials/razor-pages/razor-pages-start) para obtener instrucciones detalladas sobre cómo crear un proyecto Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="a7051-114">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project.</span></span>

# [<a name="visual-studio-for-mac"></a><span data-ttu-id="a7051-115">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="a7051-115">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a7051-116">Ejecute `dotnet new webapp` desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="a7051-116">Run `dotnet new webapp` from the command line.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="a7051-117">Ejecute `dotnet new razor` desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="a7051-117">Run `dotnet new razor` from the command line.</span></span>

::: moniker-end

<span data-ttu-id="a7051-118">Abra el archivo *.csproj* generado desde Visual Studio para Mac.</span><span class="sxs-lookup"><span data-stu-id="a7051-118">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# [<a name="visual-studio-code"></a><span data-ttu-id="a7051-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a7051-119">Visual Studio Code</span></span>](#tab/visual-studio-code)

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a7051-120">Ejecute `dotnet new webapp` desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="a7051-120">Run `dotnet new webapp` from the command line.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="a7051-121">Ejecute `dotnet new razor` desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="a7051-121">Run `dotnet new razor` from the command line.</span></span>

::: moniker-end

---

## <a name="razor-pages"></a><span data-ttu-id="a7051-122">Páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="a7051-122">Razor Pages</span></span>

<span data-ttu-id="a7051-123">Páginas de Razor está habilitada en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="a7051-123">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="a7051-124">Considere la posibilidad de una página básica: <a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="a7051-124">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="a7051-125">El código anterior se parece mucho a un [archivo de vista de Razor](xref:tutorials/first-mvc-app/adding-view) que se utiliza en una aplicación ASP.NET Core con controladores y vistas.</span><span class="sxs-lookup"><span data-stu-id="a7051-125">The preceding code looks a lot like a [Razor view file](xref:tutorials/first-mvc-app/adding-view) used in an ASP.NET Core app with controllers and views.</span></span> <span data-ttu-id="a7051-126">La directiva `@page` lo hace diferente.</span><span class="sxs-lookup"><span data-stu-id="a7051-126">What makes it different is the `@page` directive.</span></span> `@page` <span data-ttu-id="a7051-127">transforma el archivo en una acción de MVC, lo que significa que administra las solicitudes directamente, sin tener que pasar a través de un controlador.</span><span class="sxs-lookup"><span data-stu-id="a7051-127">makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> `@page` <span data-ttu-id="a7051-128">debe ser la primera directiva de Razor de una página.</span><span class="sxs-lookup"><span data-stu-id="a7051-128">must be the first Razor directive on a page.</span></span> `@page` <span data-ttu-id="a7051-129">afecta al comportamiento de otras construcciones de Razor.</span><span class="sxs-lookup"><span data-stu-id="a7051-129">affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="a7051-130">Una página similar, con una clase `PageModel`, se muestra en los dos archivos siguientes.</span><span class="sxs-lookup"><span data-stu-id="a7051-130">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="a7051-131">El archivo *Pages/Index2.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="a7051-131">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="a7051-132">Modelo de página *Pages/Index2.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="a7051-132">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="a7051-133">Por convención, el archivo de clase `PageModel` tiene el mismo nombre que el archivo de páginas de Razor con *.cs* anexado.</span><span class="sxs-lookup"><span data-stu-id="a7051-133">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="a7051-134">Por ejemplo, la página de Razor anterior es *Pages/Index2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a7051-134">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="a7051-135">El archivo que contiene la clase `PageModel` se denomina *Pages/Index2.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="a7051-135">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="a7051-136">Las asociaciones de rutas de dirección URL a páginas se determinan según la ubicación de la página en el sistema de archivos.</span><span class="sxs-lookup"><span data-stu-id="a7051-136">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="a7051-137">En la tabla siguiente, se muestra una ruta de acceso de página de Razor y la dirección URL correspondiente:</span><span class="sxs-lookup"><span data-stu-id="a7051-137">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="a7051-138">Ruta de acceso y nombre de archivo</span><span class="sxs-lookup"><span data-stu-id="a7051-138">File name and path</span></span>               | <span data-ttu-id="a7051-139">URL correspondiente</span><span class="sxs-lookup"><span data-stu-id="a7051-139">matching URL</span></span> |
| ----------------- | ------------ |
| *<span data-ttu-id="a7051-140">/Pages/Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="a7051-140">/Pages/Index.cshtml</span></span>* | `/` <span data-ttu-id="a7051-141">o</span><span class="sxs-lookup"><span data-stu-id="a7051-141">or</span></span> `/Index` |
| *<span data-ttu-id="a7051-142">/Pages/Contact.cshtml</span><span class="sxs-lookup"><span data-stu-id="a7051-142">/Pages/Contact.cshtml</span></span>* | `/Contact` |
| *<span data-ttu-id="a7051-143">/Pages/Store/Contact.cshtml</span><span class="sxs-lookup"><span data-stu-id="a7051-143">/Pages/Store/Contact.cshtml</span></span>* | `/Store/Contact` |
| *<span data-ttu-id="a7051-144">/Pages/Store/Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="a7051-144">/Pages/Store/Index.cshtml</span></span>* | `/Store` <span data-ttu-id="a7051-145">o</span><span class="sxs-lookup"><span data-stu-id="a7051-145">or</span></span> `/Store/Index` |

<span data-ttu-id="a7051-146">Notas:</span><span class="sxs-lookup"><span data-stu-id="a7051-146">Notes:</span></span>

* <span data-ttu-id="a7051-147">El runtime busca archivos de páginas de Razor en la carpeta *Páginas* de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="a7051-147">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* `Index` <span data-ttu-id="a7051-148">es la página predeterminada cuando una URL no incluye una página.</span><span class="sxs-lookup"><span data-stu-id="a7051-148">is the default page when a URL doesn't include a page.</span></span>

## <a name="write-a-basic-form"></a><span data-ttu-id="a7051-149">Escritura de un formulario básico</span><span class="sxs-lookup"><span data-stu-id="a7051-149">Write a basic form</span></span>

<span data-ttu-id="a7051-150">Las páginas de Razor están diseñadas para facilitar la implementación de patrones comunes que se usan con exploradores web al compilar una aplicación.</span><span class="sxs-lookup"><span data-stu-id="a7051-150">Razor Pages is designed to make common patterns used with web browsers easy to implement when building an app.</span></span> <span data-ttu-id="a7051-151">Los [enlaces de modelos](xref:mvc/models/model-binding), los [asistentes de etiquetas](xref:mvc/views/tag-helpers/intro) y los asistentes de HTML *simplemente funcionan* con las propiedades definidas en una clase de página de Razor.</span><span class="sxs-lookup"><span data-stu-id="a7051-151">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="a7051-152">Considere la posibilidad de una página que implementa un formulario básico del estilo "Póngase en contacto con nosotros" para el modelo `Contact`:</span><span class="sxs-lookup"><span data-stu-id="a7051-152">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="a7051-153">Para los ejemplos de este documento, `DbContext` se inicializa en el archivo [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16).</span><span class="sxs-lookup"><span data-stu-id="a7051-153">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="a7051-154">El modelo de datos:</span><span class="sxs-lookup"><span data-stu-id="a7051-154">The data model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="a7051-155">El contexto de la base de datos:</span><span class="sxs-lookup"><span data-stu-id="a7051-155">The db context:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="a7051-156">El archivo de vista *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="a7051-156">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="a7051-157">Modelo de página *Pages/Create.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="a7051-157">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="a7051-158">Por convención, la clase `PageModel` se denomina `<PageName>Model` y se encuentra en el mismo espacio de nombres que la página.</span><span class="sxs-lookup"><span data-stu-id="a7051-158">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="a7051-159">La clase `PageModel` permite la separación de la lógica de una página de su presentación.</span><span class="sxs-lookup"><span data-stu-id="a7051-159">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="a7051-160">Define los controladores de página para solicitudes que se envían a la página y los datos que usan para representar la página.</span><span class="sxs-lookup"><span data-stu-id="a7051-160">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="a7051-161">Esta separación le permite administrar dependencias de páginas mediante la [inyección de dependencias](xref:fundamentals/dependency-injection) y para realizar [pruebas unitarias](xref:test/razor-pages-tests) de las páginas.</span><span class="sxs-lookup"><span data-stu-id="a7051-161">This separation allows you to manage page dependencies through [dependency injection](xref:fundamentals/dependency-injection) and to [unit test](xref:test/razor-pages-tests) the pages.</span></span>

<span data-ttu-id="a7051-162">La página tiene un *método de controlador* `OnPostAsync`, que se ejecuta en solicitudes `POST` (cuando un usuario envía el formulario).</span><span class="sxs-lookup"><span data-stu-id="a7051-162">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="a7051-163">Puede agregar métodos de controlador para cualquier verbo HTTP.</span><span class="sxs-lookup"><span data-stu-id="a7051-163">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="a7051-164">Los controladores más comunes son:</span><span class="sxs-lookup"><span data-stu-id="a7051-164">The most common handlers are:</span></span>

* `OnGet` <span data-ttu-id="a7051-165">para inicializar el estado necesario para la página.</span><span class="sxs-lookup"><span data-stu-id="a7051-165">to initialize state needed for the page.</span></span> <span data-ttu-id="a7051-166">Ejemplo [OnGet](#OnGet).</span><span class="sxs-lookup"><span data-stu-id="a7051-166">[OnGet](#OnGet) sample.</span></span>
* `OnPost` <span data-ttu-id="a7051-167">para controlar los envíos del formulario.</span><span class="sxs-lookup"><span data-stu-id="a7051-167">to handle form submissions.</span></span>

<span data-ttu-id="a7051-168">El sufijo de nombre `Async` es opcional, pero se usa a menudo por convención para funciones asincrónicas.</span><span class="sxs-lookup"><span data-stu-id="a7051-168">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="a7051-169">El código `OnPostAsync` en el ejemplo anterior es similar a lo que escribiría normalmente en un controlador.</span><span class="sxs-lookup"><span data-stu-id="a7051-169">The `OnPostAsync` code in the preceding example looks similar to what you would normally write in a controller.</span></span> <span data-ttu-id="a7051-170">El código anterior es típico de las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="a7051-170">The preceding code is typical for Razor Pages.</span></span> <span data-ttu-id="a7051-171">La mayoría de primitivos MVC como el [enlace de modelos](xref:mvc/models/model-binding), la [validación](xref:mvc/models/validation) y los resultados de acciones se comparten.</span><span class="sxs-lookup"><span data-stu-id="a7051-171">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results are shared.</span></span>  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

<span data-ttu-id="a7051-172">El método `OnPostAsync` anterior:</span><span class="sxs-lookup"><span data-stu-id="a7051-172">The previous `OnPostAsync` method:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="a7051-173">El flujo básico de `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="a7051-173">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="a7051-174">Compruebe los errores de validación.</span><span class="sxs-lookup"><span data-stu-id="a7051-174">Check for validation errors.</span></span>

* <span data-ttu-id="a7051-175">Si no hay ningún error, guarde los datos y redirija.</span><span class="sxs-lookup"><span data-stu-id="a7051-175">If there are no errors, save the data and redirect.</span></span>
* <span data-ttu-id="a7051-176">Si hay errores, muestre la página de nuevo con mensajes de validación.</span><span class="sxs-lookup"><span data-stu-id="a7051-176">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="a7051-177">La validación del lado cliente es idéntica a las aplicaciones de ASP.NET Core MVC tradicionales.</span><span class="sxs-lookup"><span data-stu-id="a7051-177">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="a7051-178">En muchos casos, los errores de validación se detectan en el cliente y nunca se envían al servidor.</span><span class="sxs-lookup"><span data-stu-id="a7051-178">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="a7051-179">Cuando los datos se escriben correctamente, el método del controlador `OnPostAsync` llama al método del asistente `RedirectToPage` para devolver una instancia de `RedirectToPageResult`.</span><span class="sxs-lookup"><span data-stu-id="a7051-179">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> `RedirectToPage` <span data-ttu-id="a7051-180">es un resultado de acción nueva, similar a `RedirectToAction` o `RedirectToRoute`, pero personalizada para las páginas.</span><span class="sxs-lookup"><span data-stu-id="a7051-180">is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="a7051-181">En el ejemplo anterior, redirige a la página de índice raíz (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="a7051-181">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> `RedirectToPage` <span data-ttu-id="a7051-182">se detalla en la sección [Generación de direcciones URL para las páginas](#url_gen).</span><span class="sxs-lookup"><span data-stu-id="a7051-182">is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="a7051-183">Cuando el formulario enviado tiene errores de validación (que se pasan al servidor), el método del controlador `OnPostAsync` llama al método del asistente `Page`.</span><span class="sxs-lookup"><span data-stu-id="a7051-183">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> `Page` <span data-ttu-id="a7051-184">devuelve una instancia de `PageResult`.</span><span class="sxs-lookup"><span data-stu-id="a7051-184">returns an instance of `PageResult`.</span></span> <span data-ttu-id="a7051-185">Devolver `Page` es similar a cómo las acciones en los controladores devuelven `View`.</span><span class="sxs-lookup"><span data-stu-id="a7051-185">Returning `Page` is similar to how actions in controllers return `View`.</span></span> `PageResult` <span data-ttu-id="a7051-186">es el predeterminado</span><span class="sxs-lookup"><span data-stu-id="a7051-186">is the default</span></span> <!-- Review  --> <span data-ttu-id="a7051-187">que se devuelve para un método de controlador.</span><span class="sxs-lookup"><span data-stu-id="a7051-187">return type for a handler method.</span></span> <span data-ttu-id="a7051-188">Un método de controlador que devuelve `void` representa la página.</span><span class="sxs-lookup"><span data-stu-id="a7051-188">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="a7051-189">La propiedad `Customer` usa el atributo `[BindProperty]` para participar en el enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="a7051-189">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="a7051-190">De forma predeterminada, las páginas de Razor enlazan propiedades solo con verbos que no sean GET.</span><span class="sxs-lookup"><span data-stu-id="a7051-190">Razor Pages, by default, bind properties only with non-GET verbs.</span></span> <span data-ttu-id="a7051-191">Enlazar a propiedades puede reducir la cantidad de código que se debe escribir.</span><span class="sxs-lookup"><span data-stu-id="a7051-191">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="a7051-192">Enlazar reduce el código al usar la misma propiedad para representar los campos de formulario (`<input asp-for="Customer.Name">`) y aceptar la entrada.</span><span class="sxs-lookup"><span data-stu-id="a7051-192">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name">`) and accept the input.</span></span>

[!INCLUDE[](~/includes/bind-get.md)]

<span data-ttu-id="a7051-193">La página principal (*Index.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="a7051-193">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="a7051-194">La clase `PageModel` asociada (*Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="a7051-194">The associated `PageModel` class (*Index.cshtml.cs*):</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="a7051-195">El archivo *Index.cshtml* contiene el siguiente marcado para crear un vínculo de edición para cada contacto:</span><span class="sxs-lookup"><span data-stu-id="a7051-195">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="a7051-196">El [asistente de etiquetas delimitadoras](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) ha usado el atributo `asp-route-{value}` para generar un vínculo a la página de edición.</span><span class="sxs-lookup"><span data-stu-id="a7051-196">The [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="a7051-197">El vínculo contiene datos de ruta con el identificador del contacto.</span><span class="sxs-lookup"><span data-stu-id="a7051-197">The link contains route data with the contact ID.</span></span> <span data-ttu-id="a7051-198">Por ejemplo: `http://localhost:5000/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="a7051-198">For example, `http://localhost:5000/Edit/1`.</span></span> <span data-ttu-id="a7051-199">Use el atributo `asp-area` para especificar un área.</span><span class="sxs-lookup"><span data-stu-id="a7051-199">Use the `asp-area` attribute to specify an area.</span></span> <span data-ttu-id="a7051-200">Para obtener más información, vea <xref:mvc/controllers/areas>.</span><span class="sxs-lookup"><span data-stu-id="a7051-200">For more information, see <xref:mvc/controllers/areas>.</span></span>

<span data-ttu-id="a7051-201">El archivo *Pages/Edit.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="a7051-201">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="a7051-202">La primera línea contiene la directiva `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="a7051-202">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="a7051-203">La restricción de enrutamiento `"{id:int}"` indica a la página que acepte las solicitudes a la página que contienen datos de ruta `int`.</span><span class="sxs-lookup"><span data-stu-id="a7051-203">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="a7051-204">Si una solicitud a la página no contiene datos de ruta que se puedan convertir en `int`, el tiempo de ejecución devuelve un error HTTP 404 (no encontrado).</span><span class="sxs-lookup"><span data-stu-id="a7051-204">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="a7051-205">Para que el identificador sea opcional, anexe `?` a la restricción de ruta:</span><span class="sxs-lookup"><span data-stu-id="a7051-205">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="a7051-206">El archivo *Pages/Edit.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="a7051-206">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="a7051-207">El archivo *index.cshtml* también contiene una marca para crear un botón de eliminar para cada contacto de cliente:</span><span class="sxs-lookup"><span data-stu-id="a7051-207">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="a7051-208">Al representar dicho botón de eliminar en HTML, `formaction` incluye parámetros para:</span><span class="sxs-lookup"><span data-stu-id="a7051-208">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="a7051-209">Id. de contacto de cliente especificado mediante el atributo `asp-route-id`.</span><span class="sxs-lookup"><span data-stu-id="a7051-209">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="a7051-210">`handler` especificado mediante el atributo `asp-page-handler`.</span><span class="sxs-lookup"><span data-stu-id="a7051-210">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="a7051-211">Aquí tiene un ejemplo de un botón de eliminar representado con un id. de contacto de cliente de `1`:</span><span class="sxs-lookup"><span data-stu-id="a7051-211">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="a7051-212">Al seleccionar el botón, se envía una solicitud de formulario `POST` al servidor.</span><span class="sxs-lookup"><span data-stu-id="a7051-212">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="a7051-213">De forma predeterminada, el nombre del método de control se selecciona de acuerdo con el valor del parámetro `handler` y según el esquema `OnPost[handler]Async`.</span><span class="sxs-lookup"><span data-stu-id="a7051-213">By convention, the name of the handler method is selected based on the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="a7051-214">Como en este ejemplo `handler` es `delete`, el método de control `OnPostDeleteAsync` se usa para procesar la solicitud `POST`.</span><span class="sxs-lookup"><span data-stu-id="a7051-214">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="a7051-215">Si `asp-page-handler` se establece en otro valor, como `remove`, se seleccionará un método de control de páginas con el nombre `OnPostRemoveAsync`.</span><span class="sxs-lookup"><span data-stu-id="a7051-215">If the `asp-page-handler` is set to a different value, such as `remove`, a page handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="a7051-216">El método `OnPostDeleteAsync` realiza las acciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="a7051-216">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="a7051-217">Acepta el elemento `id` de la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="a7051-217">Accepts the `id` from the query string.</span></span>
* <span data-ttu-id="a7051-218">Realiza una consulta a la base de datos del contacto de cliente con `FindAsync`.</span><span class="sxs-lookup"><span data-stu-id="a7051-218">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="a7051-219">Si encuentra dicho contacto de cliente, se quita de la lista correspondiente.</span><span class="sxs-lookup"><span data-stu-id="a7051-219">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="a7051-220">Luego, se actualiza la base de datos.</span><span class="sxs-lookup"><span data-stu-id="a7051-220">The database is updated.</span></span>
* <span data-ttu-id="a7051-221">Llama a `RedirectToPage` para redirigir la página Index raíz (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="a7051-221">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="mark-page-properties-as-required"></a><span data-ttu-id="a7051-222">Marcado de las propiedades de página según sea necesario</span><span class="sxs-lookup"><span data-stu-id="a7051-222">Mark page properties as required</span></span>

<span data-ttu-id="a7051-223">Las propiedades de un valor `PageModel` se pueden decorar con el atributo [Required](/dotnet/api/system.componentmodel.dataannotations.requiredattribute):</span><span class="sxs-lookup"><span data-stu-id="a7051-223">Properties on a `PageModel` can be decorated with the [Required](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) attribute:</span></span>

[!code-cs[](index/sample/Create.cshtml.cs?highlight=3,15-16)]

<span data-ttu-id="a7051-224">Para obtener más información, vea [Validación de modelos](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="a7051-224">For more information, see [Model validation](xref:mvc/models/validation).</span></span>

## <a name="manage-head-requests-with-the-onget-handler"></a><span data-ttu-id="a7051-225">Administración de solicitudes HEAD con el controlador OnGet</span><span class="sxs-lookup"><span data-stu-id="a7051-225">Manage HEAD requests with the OnGet handler</span></span>

<span data-ttu-id="a7051-226">Las solicitudes HEAD le permiten recuperar los encabezados de un recurso específico.</span><span class="sxs-lookup"><span data-stu-id="a7051-226">HEAD requests allow you to retrieve the headers for a specific resource.</span></span> <span data-ttu-id="a7051-227">A diferencia de las solicitudes GET, las solicitudes HEAD no devuelven un cuerpo de respuesta.</span><span class="sxs-lookup"><span data-stu-id="a7051-227">Unlike GET requests, HEAD requests don't return a response body.</span></span>

<span data-ttu-id="a7051-228">Normalmente, se crea un controlador HEAD al que se llama para las solicitudes HEAD:</span><span class="sxs-lookup"><span data-stu-id="a7051-228">Ordinarily, a HEAD handler is created and called for HEAD requests:</span></span> 

```csharp
public void OnHead()
{
    HttpContext.Response.Headers.Add("HandledBy", "Handled by OnHead!");
}
```

<span data-ttu-id="a7051-229">Si no se define ningún controlador HEAD (`OnHead`), las páginas de Razor vuelven a llamar al controlador de páginas GET (`OnGet`) en ASP.NET Core 2.1 o posterior.</span><span class="sxs-lookup"><span data-stu-id="a7051-229">If no HEAD handler (`OnHead`) is defined, Razor Pages falls back to calling the GET page handler (`OnGet`) in ASP.NET Core 2.1 or later.</span></span> <span data-ttu-id="a7051-230">En ASP.NET Core 2.1 y 2.2, este comportamiento se produce con [SetCompatibilityVersion](xref:mvc/compatibility-version) en `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="a7051-230">In ASP.NET Core 2.1 and 2.2, this behavior occurs with the [SetCompatibilityVersion](xref:mvc/compatibility-version) in `Startup.Configure`:</span></span>

```csharp
services.AddMvc()
    .SetCompatibilityVersion(Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1);
```

<span data-ttu-id="a7051-231">Las plantillas predeterminadas generan la llamada `SetCompatibilityVersion` en ASP.NET Core 2.1 y 2.2.</span><span class="sxs-lookup"><span data-stu-id="a7051-231">The default templates generate the `SetCompatibilityVersion` call in ASP.NET Core 2.1 and 2.2.</span></span>

<span data-ttu-id="a7051-232">`SetCompatibilityVersion` define con eficacia la opción de las páginas de Razor `AllowMappingHeadRequestsToGetHandler` como `true`.</span><span class="sxs-lookup"><span data-stu-id="a7051-232">`SetCompatibilityVersion` effectively sets the Razor Pages option `AllowMappingHeadRequestsToGetHandler` to `true`.</span></span>

<span data-ttu-id="a7051-233">En lugar de participar en todos los comportamientos de 2.1 con `SetCompatibilityVersion`, puede participar explícitamente en comportamientos específicos.</span><span class="sxs-lookup"><span data-stu-id="a7051-233">Rather than opting into all 2.1 behaviors with `SetCompatibilityVersion`, you can explicitly opt-in to specific behaviors.</span></span> <span data-ttu-id="a7051-234">El código que se indica a continuación participa en las solicitudes HEAD de asignación del controlador GET.</span><span class="sxs-lookup"><span data-stu-id="a7051-234">The following code opts into the mapping HEAD requests to the GET handler.</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.AllowMappingHeadRequestsToGetHandler = true;
    });
```

::: moniker-end

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="a7051-235">XSRF/CSRF y páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="a7051-235">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="a7051-236">No tiene que escribir ningún código para la [validación antifalsificación](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="a7051-236">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="a7051-237">La validación y generación de tokens antifalsificación se incluyen automáticamente en las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="a7051-237">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>

## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="a7051-238">Usar diseños, parciales, plantillas y asistentes de etiquetas con las páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="a7051-238">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="a7051-239">Las páginas funcionan con todas las características del motor de vista de Razor.</span><span class="sxs-lookup"><span data-stu-id="a7051-239">Pages work with all the capabilities of the Razor view engine.</span></span> <span data-ttu-id="a7051-240">Los diseños, parciales, plantillas, asistentes de etiquetas, *_ViewStart.cshtml*, *_ViewImports.cshtml* funcionan de la misma manera que lo hacen con las vistas de Razor convencionales.</span><span class="sxs-lookup"><span data-stu-id="a7051-240">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="a7051-241">Para simplificar esta página, aprovecharemos algunas de esas características.</span><span class="sxs-lookup"><span data-stu-id="a7051-241">Let's declutter this page by taking advantage of some of those capabilities.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a7051-242">Agregue una [página de diseño](xref:mvc/views/layout) a *Pages/Shared/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="a7051-242">Add a [layout page](xref:mvc/views/layout) to *Pages/Shared/_Layout.cshtml*:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="a7051-243">Agregue una [página de diseño](xref:mvc/views/layout) a *Pages/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="a7051-243">Add a [layout page](xref:mvc/views/layout) to *Pages/_Layout.cshtml*:</span></span>

::: moniker-end

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="a7051-244">El [diseño](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="a7051-244">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="a7051-245">Controla el diseño de cada página (a no ser que la página no tenga diseño).</span><span class="sxs-lookup"><span data-stu-id="a7051-245">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="a7051-246">Importa las estructuras HTML como JavaScript y hojas de estilos.</span><span class="sxs-lookup"><span data-stu-id="a7051-246">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="a7051-247">Vea [Layout page](xref:mvc/views/layout) (Página de diseño) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="a7051-247">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="a7051-248">La propiedad [Layout](xref:mvc/views/layout#specifying-a-layout) se establece en *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="a7051-248">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a7051-249">El diseño está en la carpeta *Pages/Shared*.</span><span class="sxs-lookup"><span data-stu-id="a7051-249">The layout is in the *Pages/Shared* folder.</span></span> <span data-ttu-id="a7051-250">Las páginas buscan otras vistas (diseños, plantillas, parciales) de forma jerárquica, a partir de la misma carpeta que la página actual.</span><span class="sxs-lookup"><span data-stu-id="a7051-250">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="a7051-251">Un diseño en la carpeta *Pages/Shared* se puede usar desde cualquier página de Razor en la carpeta *Pages*.</span><span class="sxs-lookup"><span data-stu-id="a7051-251">A layout in the *Pages/Shared* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="a7051-252">El archivo de diseño debería ir en la carpeta *Pages/Shared*.</span><span class="sxs-lookup"><span data-stu-id="a7051-252">The layout file should go in the *Pages/Shared* folder.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="a7051-253">El diseño está en la carpeta *Pages*.</span><span class="sxs-lookup"><span data-stu-id="a7051-253">The layout is in the *Pages* folder.</span></span> <span data-ttu-id="a7051-254">Las páginas buscan otras vistas (diseños, plantillas, parciales) de forma jerárquica, a partir de la misma carpeta que la página actual.</span><span class="sxs-lookup"><span data-stu-id="a7051-254">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="a7051-255">Un diseño en la carpeta *Pages* se puede usar desde cualquier página de Razor en la carpeta *Pages*.</span><span class="sxs-lookup"><span data-stu-id="a7051-255">A layout in the *Pages* folder can be used from any Razor page under the *Pages* folder.</span></span>

::: moniker-end

<span data-ttu-id="a7051-256">Le recomendamos que **no** coloque el archivo de diseño en la carpeta *Views/Shared*.</span><span class="sxs-lookup"><span data-stu-id="a7051-256">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="a7051-257">*Views/Shared* es un patrón de vistas de MVC.</span><span class="sxs-lookup"><span data-stu-id="a7051-257">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="a7051-258">Las páginas de Razor están diseñadas para basarse en la jerarquía de carpetas, no en las convenciones de ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="a7051-258">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="a7051-259">La búsqueda de vistas de una página de Razor incluye la carpeta *Pages*.</span><span class="sxs-lookup"><span data-stu-id="a7051-259">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="a7051-260">Los diseños, plantillas y parciales que usa con los controladores de MVC y las vistas de Razor convencionales *simplemente funcionan*.</span><span class="sxs-lookup"><span data-stu-id="a7051-260">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="a7051-261">Agregue un archivo *Pages/_ViewImports.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="a7051-261">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

`@namespace` <span data-ttu-id="a7051-262">se explica más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="a7051-262">is explained later in the tutorial.</span></span> <span data-ttu-id="a7051-263">La directiva `@addTagHelper` pone los [asistentes de etiquetas integradas](xref:mvc/views/tag-helpers/builtin-th/Index) en todas las páginas de la carpeta *Pages*.</span><span class="sxs-lookup"><span data-stu-id="a7051-263">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="a7051-264">Cuando la directiva `@namespace` se usa explícitamente en una página:</span><span class="sxs-lookup"><span data-stu-id="a7051-264">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="a7051-265">La directiva establece el espacio de nombres de la página.</span><span class="sxs-lookup"><span data-stu-id="a7051-265">The directive sets the namespace for the page.</span></span> <span data-ttu-id="a7051-266">La directiva `@model` no necesita incluir el espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="a7051-266">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="a7051-267">Cuando la directiva `@namespace` se encuentra en *_ViewImports.cshtml*, el espacio de nombres especificado proporciona el prefijo del espacio de nombres generado en la página que importa la directiva `@namespace`.</span><span class="sxs-lookup"><span data-stu-id="a7051-267">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="a7051-268">El resto del espacio de nombres generado (la parte del sufijo) es la ruta de acceso relativa separada por puntos entre la carpeta que contiene *_ViewImports.cshtml* y la carpeta que contiene la página.</span><span class="sxs-lookup"><span data-stu-id="a7051-268">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="a7051-269">Por ejemplo, la clase `PageModel` *Pages/Customers/Edit.cshtml.cs* establece explícitamente el espacio de nombres:</span><span class="sxs-lookup"><span data-stu-id="a7051-269">For example, the `PageModel` class *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="a7051-270">El archivo *Pages/_ViewImports.cshtml* establece el espacio de nombres siguiente:</span><span class="sxs-lookup"><span data-stu-id="a7051-270">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="a7051-271">El espacio de nombres generado para la página de Razor *Pages/Customers/Edit.cshtml* es el mismo que la clase `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="a7051-271">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the `PageModel` class.</span></span>

`@namespace` *<span data-ttu-id="a7051-272">también funciona con las vistas de Razor convencionales.</span><span class="sxs-lookup"><span data-stu-id="a7051-272">also works with conventional Razor views.</span></span>*

<span data-ttu-id="a7051-273">El archivo de vista *Pages/Create.cshtml* original:</span><span class="sxs-lookup"><span data-stu-id="a7051-273">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="a7051-274">El archivo de vista *Pages/Create.cshtml* actualizado:</span><span class="sxs-lookup"><span data-stu-id="a7051-274">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="a7051-275">El [proyecto de inicio de las páginas de Razor](#rpvs17) contiene *Pages/_ValidationScriptsPartial.cshtml*, que enlaza la validación del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="a7051-275">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<span data-ttu-id="a7051-276">Para más información sobre las vistas parciales, vea <xref:mvc/views/partial>.</span><span class="sxs-lookup"><span data-stu-id="a7051-276">For more information on partial views, see <xref:mvc/views/partial>.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="a7051-277">Generación de direcciones URL para las páginas</span><span class="sxs-lookup"><span data-stu-id="a7051-277">URL generation for Pages</span></span>

<span data-ttu-id="a7051-278">La página `Create`, mostrada anteriormente, usa `RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="a7051-278">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="a7051-279">La aplicación tiene la siguiente estructura de archivos o carpetas:</span><span class="sxs-lookup"><span data-stu-id="a7051-279">The app has the following file/folder structure:</span></span>

* *<span data-ttu-id="a7051-280">/Pages</span><span class="sxs-lookup"><span data-stu-id="a7051-280">/Pages</span></span>*

  * *<span data-ttu-id="a7051-281">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="a7051-281">Index.cshtml</span></span>*
  * *<span data-ttu-id="a7051-282">/Customers</span><span class="sxs-lookup"><span data-stu-id="a7051-282">/Customers</span></span>*

    * *<span data-ttu-id="a7051-283">Create.cshtml</span><span class="sxs-lookup"><span data-stu-id="a7051-283">Create.cshtml</span></span>*
    * *<span data-ttu-id="a7051-284">Edit.cshtml</span><span class="sxs-lookup"><span data-stu-id="a7051-284">Edit.cshtml</span></span>*
    * *<span data-ttu-id="a7051-285">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="a7051-285">Index.cshtml</span></span>*

<span data-ttu-id="a7051-286">Las páginas *Pages/Customers/Create.cshtml* y *Pages/Customers/Edit.cshtml* redirigen a *Pages/Index.cshtml* si se realiza correctamente.</span><span class="sxs-lookup"><span data-stu-id="a7051-286">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="a7051-287">La cadena `/Index` forma parte del URI para tener acceso a la página anterior.</span><span class="sxs-lookup"><span data-stu-id="a7051-287">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="a7051-288">La cadena `/Index` puede usarse para generar los URI para la página *Pages/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="a7051-288">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="a7051-289">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="a7051-289">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="a7051-290">El nombre de página es la ruta de acceso a la página de la carpeta raíz */Pages*, incluido un `/` inicial, por ejemplo `/Index`.</span><span class="sxs-lookup"><span data-stu-id="a7051-290">The page name is the path to the page from the root */Pages* folder including a leading `/` (for example, `/Index`).</span></span> <span data-ttu-id="a7051-291">Los ejemplos anteriores de generación de URL ofrecen opciones mejoradas y capacidades funcionales en comparación con la escritura a mano de estas.</span><span class="sxs-lookup"><span data-stu-id="a7051-291">The preceding URL generation samples offer enhanced options and functional capabilities over hardcoding a URL.</span></span> <span data-ttu-id="a7051-292">La generación de direcciones URL usa el [enrutamiento](xref:mvc/controllers/routing) y puede generar y codificar parámetros según cómo se defina la ruta en la ruta de acceso de destino.</span><span class="sxs-lookup"><span data-stu-id="a7051-292">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="a7051-293">La generación de direcciones URL para las páginas admite nombres relativos.</span><span class="sxs-lookup"><span data-stu-id="a7051-293">URL generation for pages supports relative names.</span></span> <span data-ttu-id="a7051-294">En la siguiente tabla, se muestra qué página de índice está seleccionada con diferentes parámetros `RedirectToPage` de *Pages/Customers/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="a7051-294">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="a7051-295">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="a7051-295">RedirectToPage(x)</span></span>| <span data-ttu-id="a7051-296">Página</span><span class="sxs-lookup"><span data-stu-id="a7051-296">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="a7051-297">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="a7051-297">RedirectToPage("/Index")</span></span> | *<span data-ttu-id="a7051-298">Pages/Index</span><span class="sxs-lookup"><span data-stu-id="a7051-298">Pages/Index</span></span>* |
| <span data-ttu-id="a7051-299">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="a7051-299">RedirectToPage("./Index");</span></span> | *<span data-ttu-id="a7051-300">Pages/Customers/Index</span><span class="sxs-lookup"><span data-stu-id="a7051-300">Pages/Customers/Index</span></span>* |
| <span data-ttu-id="a7051-301">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="a7051-301">RedirectToPage("../Index")</span></span> | *<span data-ttu-id="a7051-302">Pages/Index</span><span class="sxs-lookup"><span data-stu-id="a7051-302">Pages/Index</span></span>* |
| <span data-ttu-id="a7051-303">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="a7051-303">RedirectToPage("Index")</span></span>  | *<span data-ttu-id="a7051-304">Pages/Customers/Index</span><span class="sxs-lookup"><span data-stu-id="a7051-304">Pages/Customers/Index</span></span>* |

`RedirectToPage("Index")`<span data-ttu-id="a7051-305">, `RedirectToPage("./Index")` y `RedirectToPage("../Index")` son *nombres relativos*.</span><span class="sxs-lookup"><span data-stu-id="a7051-305">, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are *relative names*.</span></span> <span data-ttu-id="a7051-306">El parámetro `RedirectToPage` se *combina* con la ruta de acceso de la página actual para calcular el nombre de la página de destino.</span><span class="sxs-lookup"><span data-stu-id="a7051-306">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page.  page name, not page path -->

<span data-ttu-id="a7051-307">Vincular el nombre relativo es útil al crear sitios con una estructura compleja.</span><span class="sxs-lookup"><span data-stu-id="a7051-307">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="a7051-308">Si usa nombres relativos para vincular entre páginas en una carpeta, puede cambiar el nombre de esa carpeta.</span><span class="sxs-lookup"><span data-stu-id="a7051-308">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="a7051-309">Todos los vínculos seguirán funcionando (porque no incluyen el nombre de carpeta).</span><span class="sxs-lookup"><span data-stu-id="a7051-309">All the links still work (because they didn't include the folder name).</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a7051-310">Para redirigir a una página en otra [área](xref:mvc/controllers/areas), especifique el área:</span><span class="sxs-lookup"><span data-stu-id="a7051-310">To redirect to a page in a different [Area](xref:mvc/controllers/areas), specify the area:</span></span>

```csharp
RedirectToPage("/Index", new { area = "Services" });
```

<span data-ttu-id="a7051-311">Para obtener más información, vea <xref:mvc/controllers/areas>.</span><span class="sxs-lookup"><span data-stu-id="a7051-311">For more information, see <xref:mvc/controllers/areas>.</span></span>

## <a name="viewdata-attribute"></a><span data-ttu-id="a7051-312">Atributo ViewData</span><span class="sxs-lookup"><span data-stu-id="a7051-312">ViewData attribute</span></span>

<span data-ttu-id="a7051-313">Se pueden pasar datos a una página con [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span><span class="sxs-lookup"><span data-stu-id="a7051-313">Data can be passed to a page with [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span></span> <span data-ttu-id="a7051-314">Los valores de las propiedades de controladores o modelos de página de Razor decoradas con `[ViewData]` se almacenan y cargan desde [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span><span class="sxs-lookup"><span data-stu-id="a7051-314">Properties on controllers or Razor Page models decorated with `[ViewData]` have their values stored and loaded from the [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span></span>

<span data-ttu-id="a7051-315">En el ejemplo siguiente, el valor `AboutModel` contiene una propiedad `Title` decorada con `[ViewData]`.</span><span class="sxs-lookup"><span data-stu-id="a7051-315">In the following example, the `AboutModel` contains a `Title` property decorated with `[ViewData]`.</span></span> <span data-ttu-id="a7051-316">La propiedad `Title` se establece en el título de la página Acerca de:</span><span class="sxs-lookup"><span data-stu-id="a7051-316">The `Title` property is set to the title of the About page:</span></span>

```csharp
public class AboutModel : PageModel
{
    [ViewData]
    public string Title { get; } = "About";

    public void OnGet()
    {
    }
}
```

<span data-ttu-id="a7051-317">En la página Acerca de, acceda a la propiedad `Title` como propiedad de modelo:</span><span class="sxs-lookup"><span data-stu-id="a7051-317">In the About page, access the `Title` property as a model property:</span></span>

```cshtml
<h1>@Model.Title</h1>
```

<span data-ttu-id="a7051-318">En el diseño, el título se lee desde el diccionario ViewData:</span><span class="sxs-lookup"><span data-stu-id="a7051-318">In the layout, the title is read from the ViewData dictionary:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```

::: moniker-end

## <a name="tempdata"></a><span data-ttu-id="a7051-319">TempData</span><span class="sxs-lookup"><span data-stu-id="a7051-319">TempData</span></span>

<span data-ttu-id="a7051-320">ASP.NET Core expone la propiedad [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) en un [controlador](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span><span class="sxs-lookup"><span data-stu-id="a7051-320">ASP.NET Core exposes the [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="a7051-321">Esta propiedad almacena datos hasta que se leen.</span><span class="sxs-lookup"><span data-stu-id="a7051-321">This property stores data until it's read.</span></span> <span data-ttu-id="a7051-322">Los métodos `Keep` y `Peek` se pueden usar para examinar los datos sin que se eliminen.</span><span class="sxs-lookup"><span data-stu-id="a7051-322">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> `TempData` <span data-ttu-id="a7051-323">es útil para el redireccionamiento cuando se necesitan los datos de más de una única solicitud.</span><span class="sxs-lookup"><span data-stu-id="a7051-323">is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="a7051-324">El atributo `[TempData]` es nuevo en ASP.NET Core 2.0 y es compatible con controladores y páginas.</span><span class="sxs-lookup"><span data-stu-id="a7051-324">The `[TempData]` attribute is new in ASP.NET Core 2.0 and is supported on controllers and pages.</span></span>

<span data-ttu-id="a7051-325">El siguiente código establece el valor de `Message` mediante `TempData`:</span><span class="sxs-lookup"><span data-stu-id="a7051-325">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="a7051-326">El siguiente marcado en el archivo *Pages/Customers/Index.cshtml* muestra el valor de `Message` mediante `TempData`.</span><span class="sxs-lookup"><span data-stu-id="a7051-326">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="a7051-327">El modelo de página *Pages/Customers/Index.cshtml.cs* aplica el atributo `[TempData]` a la propiedad `Message`.</span><span class="sxs-lookup"><span data-stu-id="a7051-327">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="a7051-328">Para obtener más información, vea [TempData](xref:fundamentals/app-state#tempdata).</span><span class="sxs-lookup"><span data-stu-id="a7051-328">For more information, see [TempData](xref:fundamentals/app-state#tempdata) .</span></span>

<a name="mhpp"></a>

## <a name="multiple-handlers-per-page"></a><span data-ttu-id="a7051-329">Varios controladores por página</span><span class="sxs-lookup"><span data-stu-id="a7051-329">Multiple handlers per page</span></span>

<span data-ttu-id="a7051-330">La siguiente página genera marcado para dos controladores de páginas mediante el asistente de etiquetas `asp-page-handler`:</span><span class="sxs-lookup"><span data-stu-id="a7051-330">The following page generates markup for two page handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

<span data-ttu-id="a7051-331">El formulario del ejemplo anterior tiene dos botones de envío, y cada uno de ellos usa `FormActionTagHelper` para enviar a una dirección URL diferente.</span><span class="sxs-lookup"><span data-stu-id="a7051-331">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="a7051-332">El atributo `asp-page-handler` es un complemento de `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="a7051-332">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> `asp-page-handler` <span data-ttu-id="a7051-333">genera direcciones URL que envían a cada uno de los métodos de controlador definidos por una página.</span><span class="sxs-lookup"><span data-stu-id="a7051-333">generates URLs that submit to each of the handler methods defined by a page.</span></span> `asp-page` <span data-ttu-id="a7051-334">no se especifica porque el ejemplo se vincula a la página actual.</span><span class="sxs-lookup"><span data-stu-id="a7051-334">isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="a7051-335">Modelo de página:</span><span class="sxs-lookup"><span data-stu-id="a7051-335">The page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="a7051-336">El código anterior usa *métodos de controlador con nombre*.</span><span class="sxs-lookup"><span data-stu-id="a7051-336">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="a7051-337">Los métodos de controlador con nombre se crean tomando el texto en el nombre después de `On<HTTP Verb>` y antes de `Async` (si existe).</span><span class="sxs-lookup"><span data-stu-id="a7051-337">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="a7051-338">En el ejemplo anterior, los métodos de página son OnPost**JoinList**Async y OnPost**JoinListUC**Async.</span><span class="sxs-lookup"><span data-stu-id="a7051-338">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="a7051-339">Quitando *OnPost* y *Async*, los nombres de controlador son `JoinList` y `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="a7051-339">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="a7051-340">Al usar el código anterior, la ruta de dirección URL que envía a `OnPostJoinListAsync` es `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="a7051-340">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="a7051-341">La ruta de dirección URL que envía a `OnPostJoinListUCAsync` es `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="a7051-341">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="custom-routes"></a><span data-ttu-id="a7051-342">Rutas personalizadas</span><span class="sxs-lookup"><span data-stu-id="a7051-342">Custom routes</span></span>

<span data-ttu-id="a7051-343">Use la directiva `@page` para:</span><span class="sxs-lookup"><span data-stu-id="a7051-343">Use the `@page` directive to:</span></span>

* <span data-ttu-id="a7051-344">Especificar una ruta personalizada a una página.</span><span class="sxs-lookup"><span data-stu-id="a7051-344">Specify a custom route to a page.</span></span> <span data-ttu-id="a7051-345">Por ejemplo, la ruta a la página Acerca de se puede establecer en `/Some/Other/Path` con `@page "/Some/Other/Path"`.</span><span class="sxs-lookup"><span data-stu-id="a7051-345">For example, the route to the About page can be set to `/Some/Other/Path` with `@page "/Some/Other/Path"`.</span></span>
* <span data-ttu-id="a7051-346">Anexar segmentos a la ruta predeterminada de una página.</span><span class="sxs-lookup"><span data-stu-id="a7051-346">Append segments to a page's default route.</span></span> <span data-ttu-id="a7051-347">Por ejemplo, se puede agregar un segmento "item" a la ruta predeterminada de una página con `@page "item"`.</span><span class="sxs-lookup"><span data-stu-id="a7051-347">For example, an "item" segment can be added to a page's default route with `@page "item"`.</span></span>
* <span data-ttu-id="a7051-348">Anexar parámetros a la ruta predeterminada de una página.</span><span class="sxs-lookup"><span data-stu-id="a7051-348">Append parameters to a page's default route.</span></span> <span data-ttu-id="a7051-349">Por ejemplo, un parámetro de identificador, `id`, puede ser necesario para una página con `@page "{id}"`.</span><span class="sxs-lookup"><span data-stu-id="a7051-349">For example, an ID parameter, `id`, can be required for a page with `@page "{id}"`.</span></span>

<span data-ttu-id="a7051-350">Se admite una ruta de acceso relativa raíz designada por una tilde (`~`) al principio de la ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="a7051-350">A root-relative path designated by a tilde (`~`) at the beginning of the path is supported.</span></span> <span data-ttu-id="a7051-351">Por ejemplo, `@page "~/Some/Other/Path"` es lo mismo que `@page "/Some/Other/Path"`.</span><span class="sxs-lookup"><span data-stu-id="a7051-351">For example, `@page "~/Some/Other/Path"` is the same as `@page "/Some/Other/Path"`.</span></span>

<span data-ttu-id="a7051-352">La cadena de consulta `?handler=JoinList` de la dirección URL se puede cambiar por un segmento de ruta `/JoinList`, para lo cual hay que especificar la plantilla de ruta `@page "{handler?}"`.</span><span class="sxs-lookup"><span data-stu-id="a7051-352">You can change the query string `?handler=JoinList` in the URL to a route segment `/JoinList` by specifying the route template `@page "{handler?}"`.</span></span>

<span data-ttu-id="a7051-353">Si no le gusta la cadena de consulta `?handler=JoinList` en la dirección URL, puede cambiar la ruta para poner el nombre del controlador en la parte de la ruta de la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="a7051-353">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="a7051-354">Para personalizar la ruta, se puede agregar una plantilla de ruta entre comillas dobles después de la directiva `@page`.</span><span class="sxs-lookup"><span data-stu-id="a7051-354">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="a7051-355">Al usar el código anterior, la ruta de dirección URL que envía a `OnPostJoinListAsync` es `http://localhost:5000/Customers/CreateFATH/JoinList`.</span><span class="sxs-lookup"><span data-stu-id="a7051-355">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH/JoinList`.</span></span> <span data-ttu-id="a7051-356">La ruta de dirección URL que envía a `OnPostJoinListUCAsync` es `http://localhost:5000/Customers/CreateFATH/JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="a7051-356">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH/JoinListUC`.</span></span>

<span data-ttu-id="a7051-357">El signo `?` que sigue a `handler` significa que el parámetro de ruta es opcional.</span><span class="sxs-lookup"><span data-stu-id="a7051-357">The `?` following `handler` means the route parameter is optional.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="a7051-358">Valores de configuración</span><span class="sxs-lookup"><span data-stu-id="a7051-358">Configuration and settings</span></span>

<span data-ttu-id="a7051-359">Para configurar opciones avanzadas, use el método de extensión `AddRazorPagesOptions` en el generador de MVC:</span><span class="sxs-lookup"><span data-stu-id="a7051-359">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="a7051-360">Actualmente, puede usar `RazorPagesOptions` para establecer el directorio raíz de páginas, o agregar convenciones de modelo de aplicación a las páginas.</span><span class="sxs-lookup"><span data-stu-id="a7051-360">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="a7051-361">En el futuro habilitaremos más extensibilidad de este modo.</span><span class="sxs-lookup"><span data-stu-id="a7051-361">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="a7051-362">Para precompilar vistas, vea [Razor view compilation](xref:mvc/views/view-compilation) (Compilación de vistas de Razor).</span><span class="sxs-lookup"><span data-stu-id="a7051-362">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="a7051-363">[Descargue o vea el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="a7051-363">[Download or view sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/index/sample).</span></span>

<span data-ttu-id="a7051-364">Vea [Introducción a las páginas de Razor](xref:tutorials/razor-pages/razor-pages-start), que se basa en esta introducción.</span><span class="sxs-lookup"><span data-stu-id="a7051-364">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="a7051-365">Especificar que las páginas de Razor se encuentran en la raíz de contenido</span><span class="sxs-lookup"><span data-stu-id="a7051-365">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="a7051-366">De forma predeterminada, la ruta raíz de las páginas de Razor es el directorio */Pages*.</span><span class="sxs-lookup"><span data-stu-id="a7051-366">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="a7051-367">Agregue [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) a [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) para especificar que las páginas de Razor están en la ruta raíz de contenido ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="a7051-367">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the content root ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="a7051-368">Especificar que las páginas de Razor se encuentran en un directorio raíz personalizado</span><span class="sxs-lookup"><span data-stu-id="a7051-368">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="a7051-369">Agregue [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) a [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) para especificar que las páginas de Razor están en un directorio raíz personalizado en la aplicación (proporcione una ruta de acceso relativa):</span><span class="sxs-lookup"><span data-stu-id="a7051-369">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="additional-resources"></a><span data-ttu-id="a7051-370">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="a7051-370">Additional resources</span></span>

* <xref:index>
* <xref:mvc/views/razor>
* <xref:mvc/controllers/areas>
* <xref:tutorials/razor-pages/razor-pages-start>
* <xref:security/authorization/razor-pages-authorization>
* <xref:razor-pages/razor-pages-conventions>
* <xref:test/razor-pages-tests>
* <xref:mvc/views/partial>
