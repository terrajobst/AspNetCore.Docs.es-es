---
title: "Introducción a las páginas de Razor en ASP.NET Core"
author: Rick-Anderson
description: "Tutorial de ASP.NET Core en Razor Pages. Incluye MVC Core, ASP.NET Core 2.x, introducción al desarrollo web y Visual Studio 2017. Este documento proporciona información general acerca del uso de las páginas de Razor en ASP.NET Core para facilitar el desarrollo de escenarios centrados en páginas."
ms.author: riande
manager: wpickett
ms.date: 09/12/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/razor-pages/index
ms.openlocfilehash: a08c1b59c7be3a27fc11e6737a1cb4b4208f2901
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="0c41f-105">Introducción a las páginas de Razor en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0c41f-105">Introduction to Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="0c41f-106">Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Ryan Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="0c41f-106">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="0c41f-107">Las páginas de Razor son una nueva característica de ASP.NET Core MVC que facilita la codificación de escenarios centrados en páginas y hace que sea más productiva.</span><span class="sxs-lookup"><span data-stu-id="0c41f-107">Razor Pages is a new feature of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="0c41f-108">Si busca un tutorial que use el enfoque Model-View-Controller, consulte [Introducción a ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="0c41f-108">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Getting started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="0c41f-109">En este documento se proporciona una introducción a las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="0c41f-109">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="0c41f-110">No es un tutorial paso a paso.</span><span class="sxs-lookup"><span data-stu-id="0c41f-110">It's not a step by step tutorial.</span></span> <span data-ttu-id="0c41f-111">Si encuentra alguna sección difícil de seguir, vea [Introducción a las páginas de Razor](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="0c41f-111">If you find some of the sections difficult to follow, see [Getting started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span>

<a name="prerequisites"></a>

## <a name="aspnet-core-20-prerequisites"></a><span data-ttu-id="0c41f-112">Requisitos previos de ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="0c41f-112">ASP.NET Core 2.0 prerequisites</span></span>

<span data-ttu-id="0c41f-113">Instale [.NET Core](https://www.microsoft.com/net/core) 2.0.0 o una versión posterior.</span><span class="sxs-lookup"><span data-stu-id="0c41f-113">Install [.NET Core](https://www.microsoft.com/net/core) 2.0.0 or later.</span></span>

<span data-ttu-id="0c41f-114">Si usa Visual Studio, instale [Visual Studio](https://www.visualstudio.com/vs/) 2017 versión 15.3, o una versión posterior con las cargas de trabajo siguientes:</span><span class="sxs-lookup"><span data-stu-id="0c41f-114">If you're using Visual Studio, install [Visual Studio](https://www.visualstudio.com/vs/) 2017 version 15.3 or later with the following workloads:</span></span>

* <span data-ttu-id="0c41f-115">**Desarrollo de ASP.NET y web**</span><span class="sxs-lookup"><span data-stu-id="0c41f-115">**ASP.NET and web development**</span></span>
* <span data-ttu-id="0c41f-116">**Desarrollo multiplataforma de .NET Core**</span><span class="sxs-lookup"><span data-stu-id="0c41f-116">**.NET Core cross-platform development**</span></span>

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a><span data-ttu-id="0c41f-117">Crear un proyecto de páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="0c41f-117">Creating a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0c41f-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0c41f-118">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="0c41f-119">Vea [Introducción a las páginas de Razor](xref:tutorials/razor-pages/razor-pages-start) para obtener instrucciones detalladas sobre cómo crear un proyecto de páginas de Razor con Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0c41f-119">See [Getting started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project using Visual Studio.</span></span>

#   <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0c41f-120">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="0c41f-120">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="0c41f-121">Ejecute `dotnet new razor` desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="0c41f-121">Run `dotnet new razor` from the command line.</span></span>

<span data-ttu-id="0c41f-122">Abra el archivo *.csproj* generado desde Visual Studio para Mac.</span><span class="sxs-lookup"><span data-stu-id="0c41f-122">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0c41f-123">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0c41f-123">Visual Studio Code</span></span>](#tab/visual-studio-code) 

<span data-ttu-id="0c41f-124">Ejecute `dotnet new razor` desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="0c41f-124">Run `dotnet new razor` from the command line.</span></span>

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="0c41f-125">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="0c41f-125">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="0c41f-126">Ejecute `dotnet new razor` desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="0c41f-126">Run `dotnet new razor` from the command line.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="0c41f-127">Páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="0c41f-127">Razor Pages</span></span>

<span data-ttu-id="0c41f-128">Páginas de Razor está habilitada en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0c41f-128">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[main](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="0c41f-129">Considere la posibilidad de una página básica: <a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="0c41f-129">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="0c41f-130">El código anterior es muy parecido a un archivo de vista de Razor.</span><span class="sxs-lookup"><span data-stu-id="0c41f-130">The preceding code looks a lot like a Razor view file.</span></span> <span data-ttu-id="0c41f-131">La directiva `@page` lo hace diferente.</span><span class="sxs-lookup"><span data-stu-id="0c41f-131">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="0c41f-132">`@page` transforma el archivo en una acción de MVC, lo que significa que administra las solicitudes directamente, sin tener que pasar a través de un controlador.</span><span class="sxs-lookup"><span data-stu-id="0c41f-132">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="0c41f-133">`@page` debe ser la primera directiva de Razor de una página.</span><span class="sxs-lookup"><span data-stu-id="0c41f-133">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="0c41f-134">`@page` afecta al comportamiento de otras construcciones de Razor.</span><span class="sxs-lookup"><span data-stu-id="0c41f-134">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="0c41f-135">Una página similar, con una clase `PageModel`, se muestra en los dos archivos siguientes.</span><span class="sxs-lookup"><span data-stu-id="0c41f-135">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="0c41f-136">El archivo *Pages/Index2.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="0c41f-136">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="0c41f-137">El archivo de código subyacente *Pages/Index2.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="0c41f-137">The *Pages/Index2.cshtml.cs* "code-behind" file:</span></span>

[!code-cs[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="0c41f-138">Por convención, el archivo de clase `PageModel` tiene el mismo nombre que el archivo de páginas de Razor con *.cs* anexado.</span><span class="sxs-lookup"><span data-stu-id="0c41f-138">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="0c41f-139">Por ejemplo, la página de Razor anterior es *Pages/Index2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="0c41f-139">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="0c41f-140">El archivo que contiene la clase `PageModel` se denomina *Pages/Index2.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="0c41f-140">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="0c41f-141">Las asociaciones de rutas de dirección URL a páginas se determinan según la ubicación de la página en el sistema de archivos.</span><span class="sxs-lookup"><span data-stu-id="0c41f-141">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="0c41f-142">En la tabla siguiente, se muestra una ruta de acceso de página de Razor y la dirección URL correspondiente:</span><span class="sxs-lookup"><span data-stu-id="0c41f-142">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="0c41f-143">Ruta de acceso y nombre de archivo</span><span class="sxs-lookup"><span data-stu-id="0c41f-143">File name and path</span></span>               | <span data-ttu-id="0c41f-144">URL correspondiente</span><span class="sxs-lookup"><span data-stu-id="0c41f-144">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="0c41f-145">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="0c41f-145">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="0c41f-146">`/` o `/Index`</span><span class="sxs-lookup"><span data-stu-id="0c41f-146">`/` or `/Index`</span></span> |
| <span data-ttu-id="0c41f-147">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="0c41f-147">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="0c41f-148">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="0c41f-148">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="0c41f-149">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="0c41f-149">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="0c41f-150">`/Store` o `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="0c41f-150">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="0c41f-151">Notas:</span><span class="sxs-lookup"><span data-stu-id="0c41f-151">Notes:</span></span>

* <span data-ttu-id="0c41f-152">El runtime busca archivos de páginas de Razor en la carpeta *Páginas* de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="0c41f-152">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="0c41f-153">`Index` es la página predeterminada cuando una URL no incluye una página.</span><span class="sxs-lookup"><span data-stu-id="0c41f-153">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="writing-a-basic-form"></a><span data-ttu-id="0c41f-154">Escribir un formulario básico</span><span class="sxs-lookup"><span data-stu-id="0c41f-154">Writing a basic form</span></span>

<span data-ttu-id="0c41f-155">Las características de páginas de Razor están diseñadas para facilitar patrones comunes que se usan con exploradores web.</span><span class="sxs-lookup"><span data-stu-id="0c41f-155">Razor Pages features are designed to make common patterns used with web browsers easy.</span></span> <span data-ttu-id="0c41f-156">Los [enlaces de modelos](xref:mvc/models/model-binding), las [aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro) y las aplicaciones auxiliares de HTML *simplemente funcionan* con las propiedades definidas en una clase de página de Razor.</span><span class="sxs-lookup"><span data-stu-id="0c41f-156">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="0c41f-157">Considere la posibilidad de una página que implementa un formulario básico del estilo "Póngase en contacto con nosotros" para el modelo `Contact`:</span><span class="sxs-lookup"><span data-stu-id="0c41f-157">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="0c41f-158">Para los ejemplos de este documento, `DbContext` se inicializa en el archivo [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16).</span><span class="sxs-lookup"><span data-stu-id="0c41f-158">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="0c41f-159">El modelo de datos:</span><span class="sxs-lookup"><span data-stu-id="0c41f-159">The data model:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="0c41f-160">El contexto de la base de datos:</span><span class="sxs-lookup"><span data-stu-id="0c41f-160">The db context:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="0c41f-161">El archivo de vista *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="0c41f-161">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="0c41f-162">El archivo de código subyacente *Pages/Movies/Create.cshtml.cs* de la vista:</span><span class="sxs-lookup"><span data-stu-id="0c41f-162">The *Pages/Create.cshtml.cs* code-behind file for the view:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="0c41f-163">Por convención, la clase `PageModel` se denomina `<PageName>Model` y se encuentra en el mismo espacio de nombres que la página.</span><span class="sxs-lookup"><span data-stu-id="0c41f-163">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="0c41f-164">La clase `PageModel` permite la separación de la lógica de una página de su presentación.</span><span class="sxs-lookup"><span data-stu-id="0c41f-164">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="0c41f-165">Define los controladores de página para solicitudes que se envían a la página y los datos que usan para representar la página.</span><span class="sxs-lookup"><span data-stu-id="0c41f-165">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="0c41f-166">Esta separación le permite administrar dependencias de páginas mediante la [inyección de dependencias](xref:fundamentals/dependency-injection) y para realizar [pruebas unitarias](xref:testing/razor-pages-testing) de las páginas.</span><span class="sxs-lookup"><span data-stu-id="0c41f-166">This separation allows you to manage page dependencies through [dependency injection](xref:fundamentals/dependency-injection) and to [unit test](xref:testing/razor-pages-testing) the pages.</span></span>

<span data-ttu-id="0c41f-167">La página tiene un *método de controlador* `OnPostAsync`, que se ejecuta en solicitudes `POST` (cuando un usuario envía el formulario).</span><span class="sxs-lookup"><span data-stu-id="0c41f-167">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="0c41f-168">Puede agregar métodos de controlador para cualquier verbo HTTP.</span><span class="sxs-lookup"><span data-stu-id="0c41f-168">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="0c41f-169">Los controladores más comunes son:</span><span class="sxs-lookup"><span data-stu-id="0c41f-169">The most common handlers are:</span></span>

* <span data-ttu-id="0c41f-170">`OnGet` para inicializar el estado necesario para la página.</span><span class="sxs-lookup"><span data-stu-id="0c41f-170">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="0c41f-171">Ejemplo [OnGet](#OnGet).</span><span class="sxs-lookup"><span data-stu-id="0c41f-171">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="0c41f-172">`OnPost` para controlar los envíos del formulario.</span><span class="sxs-lookup"><span data-stu-id="0c41f-172">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="0c41f-173">El sufijo de nombre `Async` es opcional, pero se usa a menudo por convención para funciones asincrónicas.</span><span class="sxs-lookup"><span data-stu-id="0c41f-173">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="0c41f-174">El código `OnPostAsync` en el ejemplo anterior es similar a lo que escribiría normalmente en un controlador.</span><span class="sxs-lookup"><span data-stu-id="0c41f-174">The `OnPostAsync` code in the preceding example looks similar to what you would normally write in a controller.</span></span> <span data-ttu-id="0c41f-175">El código anterior es típico de las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="0c41f-175">The preceding code is typical for Razor Pages.</span></span> <span data-ttu-id="0c41f-176">La mayoría de primitivos MVC como el [enlace de modelos](xref:mvc/models/model-binding), la [validación](xref:mvc/models/validation) y los resultados de acciones se comparten.</span><span class="sxs-lookup"><span data-stu-id="0c41f-176">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results are shared.</span></span>  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

<span data-ttu-id="0c41f-177">El método `OnPostAsync` anterior:</span><span class="sxs-lookup"><span data-stu-id="0c41f-177">The previous `OnPostAsync` method:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="0c41f-178">El flujo básico de `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="0c41f-178">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="0c41f-179">Compruebe los errores de validación.</span><span class="sxs-lookup"><span data-stu-id="0c41f-179">Check for validation errors.</span></span>

*  <span data-ttu-id="0c41f-180">Si no hay ningún error, guarde los datos y redirija.</span><span class="sxs-lookup"><span data-stu-id="0c41f-180">If there are no errors, save the data and redirect.</span></span>
*  <span data-ttu-id="0c41f-181">Si hay errores, muestre la página de nuevo con mensajes de validación.</span><span class="sxs-lookup"><span data-stu-id="0c41f-181">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="0c41f-182">La validación del lado cliente es idéntica a las aplicaciones de ASP.NET Core MVC tradicionales.</span><span class="sxs-lookup"><span data-stu-id="0c41f-182">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="0c41f-183">En muchos casos, los errores de validación se detectan en el cliente y nunca se envían al servidor.</span><span class="sxs-lookup"><span data-stu-id="0c41f-183">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="0c41f-184">Cuando los datos se escriben correctamente, el método del controlador `OnPostAsync` llama al método auxiliar `RedirectToPage` para devolver una instancia de `RedirectToPageResult`.</span><span class="sxs-lookup"><span data-stu-id="0c41f-184">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="0c41f-185">`RedirectToPage` es un resultado de acción nueva, similar a `RedirectToAction` o `RedirectToRoute`, pero personalizada para las páginas.</span><span class="sxs-lookup"><span data-stu-id="0c41f-185">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="0c41f-186">En el ejemplo anterior, redirige a la página de índice raíz (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="0c41f-186">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="0c41f-187">`RedirectToPage` se detalla en la sección [Generación de direcciones URL para las páginas](#url_gen).</span><span class="sxs-lookup"><span data-stu-id="0c41f-187">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="0c41f-188">Cuando el formulario enviado tiene errores de validación (que se pasan al servidor), el método del controlador `OnPostAsync` llama al método auxiliar `Page`.</span><span class="sxs-lookup"><span data-stu-id="0c41f-188">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="0c41f-189">`Page` devuelve una instancia de `PageResult`.</span><span class="sxs-lookup"><span data-stu-id="0c41f-189">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="0c41f-190">Devolver `Page` es similar a cómo las acciones en los controladores devuelven `View`.</span><span class="sxs-lookup"><span data-stu-id="0c41f-190">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="0c41f-191">`PageResult` es el tipo de valor devuelto <!-- Review  --> predeterminado para un método de controlador.</span><span class="sxs-lookup"><span data-stu-id="0c41f-191">`PageResult` is the default <!-- Review  --> return type for a handler method.</span></span> <span data-ttu-id="0c41f-192">Un método de controlador que devuelve `void` representa la página.</span><span class="sxs-lookup"><span data-stu-id="0c41f-192">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="0c41f-193">La propiedad `Customer` usa el atributo `[BindProperty]` para participar en el enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="0c41f-193">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="0c41f-194">De forma predeterminada, las páginas de Razor enlazan propiedades solo con verbos que no sean GET.</span><span class="sxs-lookup"><span data-stu-id="0c41f-194">Razor Pages, by default, bind properties only with non-GET verbs.</span></span> <span data-ttu-id="0c41f-195">Enlazar a propiedades puede reducir la cantidad de código que se debe escribir.</span><span class="sxs-lookup"><span data-stu-id="0c41f-195">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="0c41f-196">Enlazar reduce el código al usar la misma propiedad para representar los campos de formulario (`<input asp-for="Customer.Name" />`) y aceptar la entrada.</span><span class="sxs-lookup"><span data-stu-id="0c41f-196">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name" />`) and accept the input.</span></span>

<span data-ttu-id="0c41f-197">La página principal (*Index.cshtml*):</span><span class="sxs-lookup"><span data-stu-id="0c41f-197">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="0c41f-198">El archivo de código subyacente *Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="0c41f-198">The code behind *Index.cshtml.cs* file:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="0c41f-199">El archivo *Index.cshtml* contiene el siguiente marcado para crear un vínculo de edición para cada contacto:</span><span class="sxs-lookup"><span data-stu-id="0c41f-199">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="0c41f-200">La [aplicación auxiliar de etiquetas delimitadoras](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) ha usado el atributo `asp-route-{value}` para generar un vínculo a la página de edición.</span><span class="sxs-lookup"><span data-stu-id="0c41f-200">The [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="0c41f-201">El vínculo contiene datos de ruta con el identificador del contacto.</span><span class="sxs-lookup"><span data-stu-id="0c41f-201">The link contains route data with the contact ID.</span></span> <span data-ttu-id="0c41f-202">Por ejemplo: `http://localhost:5000/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="0c41f-202">For example, `http://localhost:5000/Edit/1`.</span></span>

<span data-ttu-id="0c41f-203">El archivo *Pages/Edit.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="0c41f-203">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="0c41f-204">La primera línea contiene la directiva `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="0c41f-204">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="0c41f-205">La restricción de enrutamiento `"{id:int}"` indica a la página que acepte las solicitudes a la página que contienen datos de ruta `int`.</span><span class="sxs-lookup"><span data-stu-id="0c41f-205">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="0c41f-206">Si una solicitud a la página no contiene datos de ruta que se puedan convertir en `int`, el tiempo de ejecución devuelve un error HTTP 404 (no encontrado).</span><span class="sxs-lookup"><span data-stu-id="0c41f-206">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span>

<span data-ttu-id="0c41f-207">El archivo *Pages/Edit.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="0c41f-207">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="0c41f-208">El archivo *index.cshtml* también contiene una marca para crear un botón de eliminar para cada contacto de cliente:</span><span class="sxs-lookup"><span data-stu-id="0c41f-208">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="0c41f-209">Al representar dicho botón de eliminar en HTML, `formaction` incluye parámetros para:</span><span class="sxs-lookup"><span data-stu-id="0c41f-209">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="0c41f-210">Id. de contacto de cliente especificado mediante el atributo `asp-route-id`.</span><span class="sxs-lookup"><span data-stu-id="0c41f-210">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="0c41f-211">`handler` especificado mediante el atributo `asp-page-handler`.</span><span class="sxs-lookup"><span data-stu-id="0c41f-211">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="0c41f-212">Aquí tiene un ejemplo de un botón de eliminar representado con un id. de contacto de cliente de `1`:</span><span class="sxs-lookup"><span data-stu-id="0c41f-212">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="0c41f-213">Al seleccionar el botón, se envía una solicitud de formulario `POST` al servidor.</span><span class="sxs-lookup"><span data-stu-id="0c41f-213">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="0c41f-214">De forma predeterminada, el nombre del método de control se selecciona de acuerdo con el valor del parámetro `handler` y según el esquema `OnPost[handler]Async`.</span><span class="sxs-lookup"><span data-stu-id="0c41f-214">By convention, the name of the handler method is selected based the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="0c41f-215">Como en este ejemplo `handler` es `delete`, el método de control `OnPostDeleteAsync` se usa para procesar la solicitud `POST`.</span><span class="sxs-lookup"><span data-stu-id="0c41f-215">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="0c41f-216">Si `asp-page-handler` se establece en otro valor, como `remove`, se seleccionará un método de control de páginas con el nombre `OnPostRemoveAsync`.</span><span class="sxs-lookup"><span data-stu-id="0c41f-216">If the `asp-page-handler` is set to a different value, such as `remove`, a page handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="0c41f-217">El método `OnPostDeleteAsync` realiza las acciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="0c41f-217">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="0c41f-218">Acepta el elemento `id` de la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="0c41f-218">Accepts the `id` from the query string.</span></span>
* <span data-ttu-id="0c41f-219">Realiza una consulta a la base de datos del contacto de cliente con `FindAsync`.</span><span class="sxs-lookup"><span data-stu-id="0c41f-219">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="0c41f-220">Si encuentra dicho contacto de cliente, se quita de la lista correspondiente.</span><span class="sxs-lookup"><span data-stu-id="0c41f-220">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="0c41f-221">Luego, se actualiza la base de datos.</span><span class="sxs-lookup"><span data-stu-id="0c41f-221">The database is updated.</span></span>
* <span data-ttu-id="0c41f-222">Llama a `RedirectToPage` para redirigir la página Index raíz (`/Index`).</span><span class="sxs-lookup"><span data-stu-id="0c41f-222">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="0c41f-223">XSRF/CSRF y páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="0c41f-223">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="0c41f-224">No tiene que escribir ningún código para la [validación antifalsificación](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="0c41f-224">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="0c41f-225">La validación y generación de tokens antifalsificación se incluyen automáticamente en las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="0c41f-225">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="0c41f-226">Usar diseños, parciales, plantillas y aplicaciones auxiliares de etiquetas con las páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="0c41f-226">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="0c41f-227">Las páginas funcionan con todas las características del motor de vista de Razor.</span><span class="sxs-lookup"><span data-stu-id="0c41f-227">Pages work with all the features of the Razor view engine.</span></span> <span data-ttu-id="0c41f-228">Los diseños, parciales, plantillas, aplicaciones auxiliares de etiquetas, *_ViewStart.cshtml*, *_ViewImports.cshtml* funcionan de la misma manera que lo hacen con las vistas de Razor convencionales.</span><span class="sxs-lookup"><span data-stu-id="0c41f-228">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="0c41f-229">Para simplificar esta página, aprovecharemos algunas de esas características.</span><span class="sxs-lookup"><span data-stu-id="0c41f-229">Let's declutter this page by taking advantage of some of those features.</span></span>

<span data-ttu-id="0c41f-230">Agregue una [página de diseño](xref:mvc/views/layout) a *Pages/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="0c41f-230">Add a [layout page](xref:mvc/views/layout) to *Pages/_Layout.cshtml*:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="0c41f-231">El [diseño](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="0c41f-231">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="0c41f-232">Controla el diseño de cada página (a no ser que la página no tenga diseño).</span><span class="sxs-lookup"><span data-stu-id="0c41f-232">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="0c41f-233">Importa las estructuras HTML como JavaScript y hojas de estilos.</span><span class="sxs-lookup"><span data-stu-id="0c41f-233">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="0c41f-234">Vea [Layout page](xref:mvc/views/layout) (Página de diseño) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="0c41f-234">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="0c41f-235">La propiedad [Layout](xref:mvc/views/layout#specifying-a-layout) se establece en *Pages/_ViewStart.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="0c41f-235">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="0c41f-236">**Nota:** El diseño está en la carpeta *Pages*.</span><span class="sxs-lookup"><span data-stu-id="0c41f-236">**Note:** The layout is in the *Pages* folder.</span></span> <span data-ttu-id="0c41f-237">Las páginas buscan otras vistas (diseños, plantillas, parciales) de forma jerárquica, a partir de la misma carpeta que la página actual.</span><span class="sxs-lookup"><span data-stu-id="0c41f-237">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="0c41f-238">Un diseño en la carpeta *Pages* se puede usar desde cualquier página de Razor en la carpeta *Pages*.</span><span class="sxs-lookup"><span data-stu-id="0c41f-238">A layout in the *Pages* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="0c41f-239">Le recomendamos que **no** coloque el archivo de diseño en la carpeta *Views/Shared*.</span><span class="sxs-lookup"><span data-stu-id="0c41f-239">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="0c41f-240">*Views/Shared* es un patrón de vistas de MVC.</span><span class="sxs-lookup"><span data-stu-id="0c41f-240">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="0c41f-241">Las páginas de Razor están diseñadas para basarse en la jerarquía de carpetas, no en las convenciones de ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="0c41f-241">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="0c41f-242">La búsqueda de vistas de una página de Razor incluye la carpeta *Pages*.</span><span class="sxs-lookup"><span data-stu-id="0c41f-242">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="0c41f-243">Los diseños, plantillas y parciales que usa con los controladores de MVC y las vistas de Razor convencionales *simplemente funcionan*.</span><span class="sxs-lookup"><span data-stu-id="0c41f-243">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="0c41f-244">Agregue un archivo *Pages/_ViewImports.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="0c41f-244">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="0c41f-245">`@namespace` se explica más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="0c41f-245">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="0c41f-246">La directiva `@addTagHelper` pone las [aplicaciones auxiliares de etiquetas integradas](xref:mvc/views/tag-helpers/builtin-th/Index) en todas las páginas de la carpeta *Pages*.</span><span class="sxs-lookup"><span data-stu-id="0c41f-246">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="0c41f-247">Cuando la directiva `@namespace` se usa explícitamente en una página:</span><span class="sxs-lookup"><span data-stu-id="0c41f-247">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="0c41f-248">La directiva establece el espacio de nombres de la página.</span><span class="sxs-lookup"><span data-stu-id="0c41f-248">The directive sets the namespace for the page.</span></span> <span data-ttu-id="0c41f-249">La directiva `@model` no necesita incluir el espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="0c41f-249">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="0c41f-250">Cuando la directiva `@namespace` se encuentra en *_ViewImports.cshtml*, el espacio de nombres especificado proporciona el prefijo del espacio de nombres generado en la página que importa la directiva `@namespace`.</span><span class="sxs-lookup"><span data-stu-id="0c41f-250">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="0c41f-251">El resto del espacio de nombres generado (la parte del sufijo) es la ruta de acceso relativa separada por puntos entre la carpeta que contiene *_ViewImports.cshtml* y la carpeta que contiene la página.</span><span class="sxs-lookup"><span data-stu-id="0c41f-251">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="0c41f-252">Por ejemplo, el archivo de código subyacente *Pages/Customers/Edit.cshtml.cs* establece explícitamente el espacio de nombres:</span><span class="sxs-lookup"><span data-stu-id="0c41f-252">For example, the code behind file *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="0c41f-253">El archivo *Pages/_ViewImports.cshtml* establece el espacio de nombres siguiente:</span><span class="sxs-lookup"><span data-stu-id="0c41f-253">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="0c41f-254">El espacio de nombres generado para la página de Razor *Pages/Customers/Edit.cshtml* es el mismo que el archivo de código subyacente.</span><span class="sxs-lookup"><span data-stu-id="0c41f-254">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the code behind file.</span></span> <span data-ttu-id="0c41f-255">La directiva `@namespace` se ha diseñado para que las clases de C# agregadas a un proyecto y el código generado de páginas *funcionen* sin tener que agregar una directiva `@using` para el archivo de código subyacente.</span><span class="sxs-lookup"><span data-stu-id="0c41f-255">The `@namespace` directive was designed so the C# classes added to a project and pages-generated code *just work* without having to add an `@using` directive for the code behind file.</span></span>

<span data-ttu-id="0c41f-256">**Nota:** `@namespace` también funciona con las vistas de Razor convencionales.</span><span class="sxs-lookup"><span data-stu-id="0c41f-256">**Note:** `@namespace` also works with conventional Razor views.</span></span>

<span data-ttu-id="0c41f-257">El archivo de vista *Pages/Create.cshtml* original:</span><span class="sxs-lookup"><span data-stu-id="0c41f-257">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="0c41f-258">El archivo de vista *Pages/Create.cshtml* actualizado:</span><span class="sxs-lookup"><span data-stu-id="0c41f-258">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="0c41f-259">El [proyecto de inicio de las páginas de Razor](#rpvs17) contiene *Pages/_ValidationScriptsPartial.cshtml*, que enlaza la validación del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="0c41f-259">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="0c41f-260">Generación de direcciones URL para las páginas</span><span class="sxs-lookup"><span data-stu-id="0c41f-260">URL generation for Pages</span></span>

<span data-ttu-id="0c41f-261">La página `Create`, mostrada anteriormente, usa `RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="0c41f-261">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="0c41f-262">La aplicación tiene la siguiente estructura de archivos o carpetas:</span><span class="sxs-lookup"><span data-stu-id="0c41f-262">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="0c41f-263">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="0c41f-263">*/Pages*</span></span>

  * <span data-ttu-id="0c41f-264">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="0c41f-264">*Index.cshtml*</span></span>
  * <span data-ttu-id="0c41f-265">*/Customer*</span><span class="sxs-lookup"><span data-stu-id="0c41f-265">*/Customer*</span></span>

    * <span data-ttu-id="0c41f-266">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="0c41f-266">*Create.cshtml*</span></span>
    * <span data-ttu-id="0c41f-267">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="0c41f-267">*Edit.cshtml*</span></span>
    * <span data-ttu-id="0c41f-268">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="0c41f-268">*Index.cshtml*</span></span>

<span data-ttu-id="0c41f-269">Las páginas *Pages/Customers/Create.cshtml* y *Pages/Customers/Edit.cshtml* redirigen a *Pages/Index.cshtml* si se realiza correctamente.</span><span class="sxs-lookup"><span data-stu-id="0c41f-269">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="0c41f-270">La cadena `/Index` forma parte del URI para tener acceso a la página anterior.</span><span class="sxs-lookup"><span data-stu-id="0c41f-270">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="0c41f-271">La cadena `/Index` puede usarse para generar los URI para la página *Pages/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="0c41f-271">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="0c41f-272">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="0c41f-272">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="0c41f-273">El nombre de página es la ruta de acceso a la página de la carpeta raíz */Pages* (incluido un `/` inicial, por ejemplo `/Index`).</span><span class="sxs-lookup"><span data-stu-id="0c41f-273">The page name is the path to the page from the root */Pages* folder (including a leading `/`, for example `/Index`).</span></span> <span data-ttu-id="0c41f-274">Los ejemplos de generación de URL anteriores tienen muchas más características que simplemente codificar una dirección URL.</span><span class="sxs-lookup"><span data-stu-id="0c41f-274">The preceding URL generation samples are much more feature rich than just hardcoding a URL.</span></span> <span data-ttu-id="0c41f-275">La generación de direcciones URL usa el [enrutamiento](xref:mvc/controllers/routing) y puede generar y codificar parámetros según cómo se defina la ruta en la ruta de acceso de destino.</span><span class="sxs-lookup"><span data-stu-id="0c41f-275">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="0c41f-276">La generación de direcciones URL para las páginas admite nombres relativos.</span><span class="sxs-lookup"><span data-stu-id="0c41f-276">URL generation for pages supports relative names.</span></span> <span data-ttu-id="0c41f-277">En la siguiente tabla, se muestra qué página de índice está seleccionada con diferentes parámetros `RedirectToPage` de *Pages/Customers/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="0c41f-277">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="0c41f-278">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="0c41f-278">RedirectToPage(x)</span></span>| <span data-ttu-id="0c41f-279">Página</span><span class="sxs-lookup"><span data-stu-id="0c41f-279">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="0c41f-280">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="0c41f-280">RedirectToPage("/Index")</span></span> | <span data-ttu-id="0c41f-281">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="0c41f-281">*Pages/Index*</span></span> |
| <span data-ttu-id="0c41f-282">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="0c41f-282">RedirectToPage("./Index");</span></span> | <span data-ttu-id="0c41f-283">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="0c41f-283">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="0c41f-284">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="0c41f-284">RedirectToPage("../Index")</span></span> | <span data-ttu-id="0c41f-285">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="0c41f-285">*Pages/Index*</span></span> |
| <span data-ttu-id="0c41f-286">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="0c41f-286">RedirectToPage("Index")</span></span>  | <span data-ttu-id="0c41f-287">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="0c41f-287">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="0c41f-288">`RedirectToPage("Index")`, `RedirectToPage("./Index")` y `RedirectToPage("../Index")` son *nombres relativos*.</span><span class="sxs-lookup"><span data-stu-id="0c41f-288">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are *relative names*.</span></span> <span data-ttu-id="0c41f-289">El parámetro `RedirectToPage` se *combina* con la ruta de acceso de la página actual para calcular el nombre de la página de destino.</span><span class="sxs-lookup"><span data-stu-id="0c41f-289">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page. -- page name, not page path -->

<span data-ttu-id="0c41f-290">Vincular el nombre relativo es útil al crear sitios con una estructura compleja.</span><span class="sxs-lookup"><span data-stu-id="0c41f-290">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="0c41f-291">Si usa nombres relativos para vincular entre páginas en una carpeta, puede cambiar el nombre de esa carpeta.</span><span class="sxs-lookup"><span data-stu-id="0c41f-291">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="0c41f-292">Todos los vínculos seguirán funcionando (porque no incluyen el nombre de carpeta).</span><span class="sxs-lookup"><span data-stu-id="0c41f-292">All the links still work (because they didn't include the folder name).</span></span>

## <a name="tempdata"></a><span data-ttu-id="0c41f-293">TempData</span><span class="sxs-lookup"><span data-stu-id="0c41f-293">TempData</span></span>

<span data-ttu-id="0c41f-294">ASP.NET Core expone la propiedad [TempData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) en un [controlador](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span><span class="sxs-lookup"><span data-stu-id="0c41f-294">ASP.NET Core exposes the [TempData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="0c41f-295">Esta propiedad almacena datos hasta que se leen.</span><span class="sxs-lookup"><span data-stu-id="0c41f-295">This property stores data until it's read.</span></span> <span data-ttu-id="0c41f-296">Los métodos `Keep` y `Peek` se pueden usar para examinar los datos sin que se eliminen.</span><span class="sxs-lookup"><span data-stu-id="0c41f-296">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="0c41f-297">`TempData` es útil para el redireccionamiento cuando se necesitan los datos de más de una única solicitud.</span><span class="sxs-lookup"><span data-stu-id="0c41f-297">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="0c41f-298">El atributo `[TempData]` es nuevo en ASP.NET Core 2.0 y es compatible con controladores y páginas.</span><span class="sxs-lookup"><span data-stu-id="0c41f-298">The `[TempData]` attribute is new in ASP.NET Core 2.0 and is supported on controllers and pages.</span></span>

<span data-ttu-id="0c41f-299">El siguiente código establece el valor de `Message` mediante `TempData`:</span><span class="sxs-lookup"><span data-stu-id="0c41f-299">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="0c41f-300">El siguiente marcado en el archivo *Pages/Customers/Index.cshtml* muestra el valor de `Message` mediante `TempData`.</span><span class="sxs-lookup"><span data-stu-id="0c41f-300">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="0c41f-301">El archivo de código subyacente *Pages/Customers/Index.cshtml.cs* aplica el atributo `[TempData]` a la propiedad `Message`.</span><span class="sxs-lookup"><span data-stu-id="0c41f-301">The *Pages/Customers/Index.cshtml.cs* code-behind file applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="0c41f-302">Vea [TempData](xref:fundamentals/app-state#temp) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="0c41f-302">See [TempData](xref:fundamentals/app-state#temp) for more information.</span></span>

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a><span data-ttu-id="0c41f-303">Varios controladores por página</span><span class="sxs-lookup"><span data-stu-id="0c41f-303">Multiple handlers per page</span></span>

<span data-ttu-id="0c41f-304">La siguiente página genera marcado para dos controladores de páginas mediante la aplicación auxiliar de etiquetas `asp-page-handler`:</span><span class="sxs-lookup"><span data-stu-id="0c41f-304">The following page generates markup for two page handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

<span data-ttu-id="0c41f-305">El formulario del ejemplo anterior tiene dos botones de envío, y cada uno de ellos usa `FormActionTagHelper` para enviar a una dirección URL diferente.</span><span class="sxs-lookup"><span data-stu-id="0c41f-305">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="0c41f-306">El atributo `asp-page-handler` es un complemento de `asp-page`.</span><span class="sxs-lookup"><span data-stu-id="0c41f-306">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="0c41f-307">`asp-page-handler` genera direcciones URL que envían a cada uno de los métodos de controlador definidos por una página.</span><span class="sxs-lookup"><span data-stu-id="0c41f-307">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="0c41f-308">`asp-page` no se especifica porque el ejemplo se vincula a la página actual.</span><span class="sxs-lookup"><span data-stu-id="0c41f-308">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="0c41f-309">El archivo de código subyacente:</span><span class="sxs-lookup"><span data-stu-id="0c41f-309">The code-behind file:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="0c41f-310">El código anterior usa *métodos de controlador con nombre*.</span><span class="sxs-lookup"><span data-stu-id="0c41f-310">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="0c41f-311">Los métodos de controlador con nombre se crean tomando el texto en el nombre después de `On<HTTP Verb>` y antes de `Async` (si existe).</span><span class="sxs-lookup"><span data-stu-id="0c41f-311">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="0c41f-312">En el ejemplo anterior, los métodos de página son OnPost**JoinList**Async y OnPost**JoinListUC**Async.</span><span class="sxs-lookup"><span data-stu-id="0c41f-312">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="0c41f-313">Quitando *OnPost* y *Async*, los nombres de controlador son `JoinList` y `JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="0c41f-313">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="0c41f-314">Al usar el código anterior, la ruta de dirección URL que envía a `OnPostJoinListAsync` es `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span><span class="sxs-lookup"><span data-stu-id="0c41f-314">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="0c41f-315">La ruta de dirección URL que envía a `OnPostJoinListUCAsync` es `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span><span class="sxs-lookup"><span data-stu-id="0c41f-315">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="customizing-routing"></a><span data-ttu-id="0c41f-316">Personalización del enrutamiento</span><span class="sxs-lookup"><span data-stu-id="0c41f-316">Customizing Routing</span></span>

<span data-ttu-id="0c41f-317">Si no le gusta la cadena de consulta `?handler=JoinList` en la dirección URL, puede cambiar la ruta para poner el nombre del controlador en la parte de la ruta de la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="0c41f-317">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="0c41f-318">Para personalizar la ruta, se puede agregar una plantilla de ruta entre comillas dobles después de la directiva `@page`.</span><span class="sxs-lookup"><span data-stu-id="0c41f-318">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="0c41f-319">La ruta anterior coloca el nombre del controlador en la ruta de dirección URL en lugar de la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="0c41f-319">The preceding route puts the handler name in the URL path instead of the query string.</span></span> <span data-ttu-id="0c41f-320">El signo `?` que sigue a `handler` significa que el parámetro de ruta es opcional.</span><span class="sxs-lookup"><span data-stu-id="0c41f-320">The `?` following `handler` means the route parameter is optional.</span></span>

<span data-ttu-id="0c41f-321">Puede usar `@page` para agregar segmentos y parámetros adicionales a la ruta de la página.</span><span class="sxs-lookup"><span data-stu-id="0c41f-321">You can use `@page` to add additional segments and parameters to a page's route.</span></span> <span data-ttu-id="0c41f-322">Todo lo que se **anexe** a la ruta predeterminada de la página.</span><span class="sxs-lookup"><span data-stu-id="0c41f-322">Whatever's there's **appended** to the default route of the page.</span></span> <span data-ttu-id="0c41f-323">No se puede usar una ruta de acceso absoluta o virtual para cambiar la ruta de la página (como `"~/Some/Other/Path"`).</span><span class="sxs-lookup"><span data-stu-id="0c41f-323">Using an absolute or virtual path to change the page's route (like `"~/Some/Other/Path"`) isn't supported.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="0c41f-324">Valores de configuración</span><span class="sxs-lookup"><span data-stu-id="0c41f-324">Configuration and settings</span></span>

<span data-ttu-id="0c41f-325">Para configurar opciones avanzadas, use el método de extensión `AddRazorPagesOptions` en el generador de MVC:</span><span class="sxs-lookup"><span data-stu-id="0c41f-325">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[main](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="0c41f-326">Actualmente, puede usar `RazorPagesOptions` para establecer el directorio raíz de páginas, o agregar convenciones de modelo de aplicación a las páginas.</span><span class="sxs-lookup"><span data-stu-id="0c41f-326">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="0c41f-327">En el futuro habilitaremos más extensibilidad de este modo.</span><span class="sxs-lookup"><span data-stu-id="0c41f-327">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="0c41f-328">Para precompilar vistas, vea [Razor view compilation](xref:mvc/views/view-compilation) (Compilación de vistas de Razor).</span><span class="sxs-lookup"><span data-stu-id="0c41f-328">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="0c41f-329">[Descargue o vea el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="0c41f-329">[Download or view sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span></span>

<span data-ttu-id="0c41f-330">Consulte [Introducción a las páginas de Razor en ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start), en el que se desarrolla esta introducción.</span><span class="sxs-lookup"><span data-stu-id="0c41f-330">See [Getting started with Razor Pages in ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="0c41f-331">Especificar que las páginas de Razor se encuentran en la raíz de contenido</span><span class="sxs-lookup"><span data-stu-id="0c41f-331">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="0c41f-332">De forma predeterminada, la ruta raíz de las páginas de Razor es el directorio */Pages*.</span><span class="sxs-lookup"><span data-stu-id="0c41f-332">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="0c41f-333">Agregue [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) a [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) para especificar que las páginas de Razor están en la ruta raíz de contenido ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="0c41f-333">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the content root ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="0c41f-334">Especificar que las páginas de Razor se encuentran en un directorio raíz personalizado</span><span class="sxs-lookup"><span data-stu-id="0c41f-334">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="0c41f-335">Agregue [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) a [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) para especificar que las páginas de Razor están en un directorio raíz personalizado en la aplicación (proporcione una ruta de acceso relativa):</span><span class="sxs-lookup"><span data-stu-id="0c41f-335">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="see-also"></a><span data-ttu-id="0c41f-336">Vea también</span><span class="sxs-lookup"><span data-stu-id="0c41f-336">See also</span></span>

* [<span data-ttu-id="0c41f-337">Introducción a las páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="0c41f-337">Getting started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="0c41f-338">Convenciones de autorización de las páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="0c41f-338">Razor Pages authorization conventions</span></span>](xref:security/authorization/razor-pages-authorization)
* [<span data-ttu-id="0c41f-339">Proveedores personalizados de rutas y modelos de página de páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="0c41f-339">Razor Pages custom route and page model providers</span></span>](xref:mvc/razor-pages/razor-pages-convention-features)
* [<span data-ttu-id="0c41f-340">Pruebas unitarias y de integración de las páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="0c41f-340">Razor Pages unit and integration testing</span></span>](xref:testing/razor-pages-testing)
