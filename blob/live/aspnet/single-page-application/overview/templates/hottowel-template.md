---
uid: single-page-application/overview/templates/hottowel-template
title: Plantilla de toallas activa | Documentos de Microsoft
author: madskristensen
description: Plantilla de HotTowel
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/09/2013
ms.topic: article
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: bfc6e2c884c422f44e8be5f4f29554ae86f7ecb6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="hot-towel-template"></a><span data-ttu-id="26e6b-103">Plantilla de toallas activa</span><span class="sxs-lookup"><span data-stu-id="26e6b-103">Hot Towel template</span></span>
====================
<span data-ttu-id="26e6b-104">por [Mads Kristensen](https://github.com/madskristensen)</span><span class="sxs-lookup"><span data-stu-id="26e6b-104">by [Mads Kristensen](https://github.com/madskristensen)</span></span>

> <span data-ttu-id="26e6b-105">La plantilla de MVC de toallas activa esté escrita por John Papa</span><span class="sxs-lookup"><span data-stu-id="26e6b-105">The Hot Towel MVC Template is written by John Papa</span></span>
> 
> <span data-ttu-id="26e6b-106">Elija qué versión desea descargar:</span><span class="sxs-lookup"><span data-stu-id="26e6b-106">Choose which version to download:</span></span>
> 
> [<span data-ttu-id="26e6b-107">Plantilla MVC de toallas activa para Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="26e6b-107">Hot Towel MVC Template for Visual Studio 2012</span></span>](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [<span data-ttu-id="26e6b-108">Plantilla MVC de toallas activa para Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="26e6b-108">Hot Towel MVC Template for Visual Studio 2013</span></span>](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)


> <span data-ttu-id="26e6b-109">Toallas activa: Dado que no desea ir a la aplicación SPA sin una!</span><span class="sxs-lookup"><span data-stu-id="26e6b-109">Hot Towel: Because you don't want to go to the SPA without one!</span></span>


<span data-ttu-id="26e6b-110">¿Desea crear un SPA pero no se puede decidir dónde empezar?</span><span class="sxs-lookup"><span data-stu-id="26e6b-110">Want to build a SPA but can't decide where to start?</span></span> <span data-ttu-id="26e6b-111">Usar toallas activa y en segundos, tendrá un SPA y todas las herramientas que necesita para crear en ella.</span><span class="sxs-lookup"><span data-stu-id="26e6b-111">Use Hot Towel and in seconds you'll have a SPA and all the tools you need to build on it!</span></span>

<span data-ttu-id="26e6b-112">Toallas activa crea un buen punto de partida para la creación de una aplicación de página única (SPA) con ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="26e6b-112">Hot Towel creates a great starting point for building a Single Page Application (SPA) with ASP.NET.</span></span> <span data-ttu-id="26e6b-113">De fábrica le proporciona una estructura modular de código, navegación por la vista, enlace de datos, administración de datos enriquecidos y un estilo sencillo y elegante.</span><span class="sxs-lookup"><span data-stu-id="26e6b-113">Out of the box you it provides a modular structure for your code, view navigation, data binding, rich data management and simple but elegant styling.</span></span> <span data-ttu-id="26e6b-114">Toallas activa proporciona todo lo que necesita para compilar un SPA, de forma que pueda centrarse en la aplicación, no la mecánica.</span><span class="sxs-lookup"><span data-stu-id="26e6b-114">Hot Towel provides everything you need to build a SPA, so you can focus on your app, not the plumbing.</span></span>

> <span data-ttu-id="26e6b-115">Más información acerca de cómo generar un SPA desde [John Papa vídeos, tutoriales y Pluralsight cursos](http://johnpapa.net/spa?vsix).</span><span class="sxs-lookup"><span data-stu-id="26e6b-115">Learn more about building a SPA from [John Papa's videos, tutorials and Pluralsight courses](http://johnpapa.net/spa?vsix).</span></span>


## <a name="application-structure"></a><span data-ttu-id="26e6b-116">Estructura de aplicación</span><span class="sxs-lookup"><span data-stu-id="26e6b-116">Application Structure</span></span>

<span data-ttu-id="26e6b-117">Acceso rápido de toallas de SPA proporciona una carpeta de la aplicación que contiene los archivos JavaScript y HTML que definen la aplicación.</span><span class="sxs-lookup"><span data-stu-id="26e6b-117">Hot Towel SPA provides an App folder which contains the JavaScript and HTML files that define your application.</span></span>

<span data-ttu-id="26e6b-118">Dentro de la carpeta de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="26e6b-118">Inside the App folder:</span></span>

- <span data-ttu-id="26e6b-119">durandal</span><span class="sxs-lookup"><span data-stu-id="26e6b-119">durandal</span></span>
- <span data-ttu-id="26e6b-120">servicios</span><span class="sxs-lookup"><span data-stu-id="26e6b-120">services</span></span>
- <span data-ttu-id="26e6b-121">ViewModels</span><span class="sxs-lookup"><span data-stu-id="26e6b-121">viewmodels</span></span>
- <span data-ttu-id="26e6b-122">vistas</span><span class="sxs-lookup"><span data-stu-id="26e6b-122">views</span></span>

<span data-ttu-id="26e6b-123">La carpeta de la aplicación contiene una colección de módulos.</span><span class="sxs-lookup"><span data-stu-id="26e6b-123">The App folder contains a collection of modules.</span></span> <span data-ttu-id="26e6b-124">Estos módulos encapsulan la funcionalidad y declaran las dependencias en otros módulos.</span><span class="sxs-lookup"><span data-stu-id="26e6b-124">These modules encapsulate functionality and declare dependencies on other modules.</span></span> <span data-ttu-id="26e6b-125">La carpeta views contiene el código HTML de la aplicación y la carpeta viewmodels contiene la lógica de presentación para las vistas (un modelo común de MVVM).</span><span class="sxs-lookup"><span data-stu-id="26e6b-125">The views folder contains the HTML for your application and the viewmodels folder contains the presentation logic for the views (a common MVVM pattern).</span></span> <span data-ttu-id="26e6b-126">La carpeta de servicios es ideal para albergar los servicios comunes que la aplicación puede necesitar, como la recuperación de datos HTTP o la interacción de almacenamiento local.</span><span class="sxs-lookup"><span data-stu-id="26e6b-126">The services folder is ideal for housing any common services that your application may need such as HTTP data retrieval or local storage interaction.</span></span> <span data-ttu-id="26e6b-127">Es habitual que varias viewmodels reutilizar el código de los módulos de servicio.</span><span class="sxs-lookup"><span data-stu-id="26e6b-127">It is common for multiple viewmodels to re-use code from the service modules.</span></span>

## <a name="aspnet-mvc-server-side-application-structure"></a><span data-ttu-id="26e6b-128">Estructura de aplicación de lado de servidor de ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="26e6b-128">ASP.NET MVC Server Side Application Structure</span></span>

<span data-ttu-id="26e6b-129">Toallas activa se basa en la estructura de MVC de ASP.NET conocida y eficaces.</span><span class="sxs-lookup"><span data-stu-id="26e6b-129">Hot Towel builds on the familiar and powerful ASP.NET MVC structure.</span></span>

- <span data-ttu-id="26e6b-130">Aplicación\_iniciar</span><span class="sxs-lookup"><span data-stu-id="26e6b-130">App\_Start</span></span>
- <span data-ttu-id="26e6b-131">Contenido</span><span class="sxs-lookup"><span data-stu-id="26e6b-131">Content</span></span>
- <span data-ttu-id="26e6b-132">Controladores</span><span class="sxs-lookup"><span data-stu-id="26e6b-132">Controllers</span></span>
- <span data-ttu-id="26e6b-133">Modelos</span><span class="sxs-lookup"><span data-stu-id="26e6b-133">Models</span></span>
- <span data-ttu-id="26e6b-134">Scripts</span><span class="sxs-lookup"><span data-stu-id="26e6b-134">Scripts</span></span>
- <span data-ttu-id="26e6b-135">Vistas</span><span class="sxs-lookup"><span data-stu-id="26e6b-135">Views</span></span>

## <a name="featured-libraries"></a><span data-ttu-id="26e6b-136">Bibliotecas destacadas</span><span class="sxs-lookup"><span data-stu-id="26e6b-136">Featured Libraries</span></span>

- <span data-ttu-id="26e6b-137">ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="26e6b-137">ASP.NET MVC</span></span>
- <span data-ttu-id="26e6b-138">ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="26e6b-138">ASP.NET Web API</span></span>
- <span data-ttu-id="26e6b-139">Optimización de ASP.NET Web - agrupar y minificar</span><span class="sxs-lookup"><span data-stu-id="26e6b-139">ASP.NET Web Optimization - bundling and minification</span></span>
- <span data-ttu-id="26e6b-140">[BREEZE.js](http://Breezejs.com) -administración de datos enriquecidos</span><span class="sxs-lookup"><span data-stu-id="26e6b-140">[Breeze.js](http://Breezejs.com) - rich data management</span></span>
- <span data-ttu-id="26e6b-141">[Durandal.js](http://Durandaljs.com) -navegación y composición de la vista</span><span class="sxs-lookup"><span data-stu-id="26e6b-141">[Durandal.js](http://Durandaljs.com) - navigation and View composition</span></span>
- <span data-ttu-id="26e6b-142">[Knockout.js](http://Knockoutjs.com) -enlaces de datos</span><span class="sxs-lookup"><span data-stu-id="26e6b-142">[Knockout.js](http://Knockoutjs.com) - data bindings</span></span>
- <span data-ttu-id="26e6b-143">[Require.js](http://requirejs.org) -modularidad con la optimización y AMD</span><span class="sxs-lookup"><span data-stu-id="26e6b-143">[Require.js](http://requirejs.org) - Modularity with AMD and optimization</span></span>
- <span data-ttu-id="26e6b-144">[Toastr.js](http://jpapa.me/c7toastr) -mensajes emergentes</span><span class="sxs-lookup"><span data-stu-id="26e6b-144">[Toastr.js](http://jpapa.me/c7toastr) - pop-up messages</span></span>
- <span data-ttu-id="26e6b-145">[Arranque de Twitter](http://twitter.github.com/bootstrap/) : aplicar estilo de CSS sólido</span><span class="sxs-lookup"><span data-stu-id="26e6b-145">[Twitter Bootstrap](http://twitter.github.com/bootstrap/) - robust CSS styling</span></span>

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a><span data-ttu-id="26e6b-146">Instalar a través de la plantilla de Visual Studio 2012 toallas activa SPA</span><span class="sxs-lookup"><span data-stu-id="26e6b-146">Installing via the Visual Studio 2012 Hot Towel SPA Template</span></span>

<span data-ttu-id="26e6b-147">Toallas activa se pueden instalar como una plantilla de Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="26e6b-147">Hot Towel can be installed as a Visual Studio 2012 template.</span></span> <span data-ttu-id="26e6b-148">Simplemente haga clic en `File`  |  `New Project` y elija `ASP.NET MVC 4 Web Application`.</span><span class="sxs-lookup"><span data-stu-id="26e6b-148">Just click `File` | `New Project` and choose `ASP.NET MVC 4 Web Application`.</span></span> <span data-ttu-id="26e6b-149">A continuación, seleccione el ' activa toallas única página de aplicación "plantilla y ejecutar!</span><span class="sxs-lookup"><span data-stu-id="26e6b-149">Then select the 'Hot Towel Single Page Application" template and run!</span></span>

## <a name="installing-via-the-nuget-package"></a><span data-ttu-id="26e6b-150">Instalar mediante el paquete de NuGet</span><span class="sxs-lookup"><span data-stu-id="26e6b-150">Installing via the NuGet Package</span></span>

<span data-ttu-id="26e6b-151">Toallas activa también es un paquete de NuGet que aumenta un proyecto de MVC de ASP.NET vacío existente.</span><span class="sxs-lookup"><span data-stu-id="26e6b-151">Hot Towel is also a NuGet package that augments an existing empty ASP.NET MVC project.</span></span> <span data-ttu-id="26e6b-152">Solo tiene que instalar mediante Nuget y, a continuación, ejecute!</span><span class="sxs-lookup"><span data-stu-id="26e6b-152">Just install using Nuget and then run!</span></span>

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a><span data-ttu-id="26e6b-153">¿Cómo se genera en toallas activa?</span><span class="sxs-lookup"><span data-stu-id="26e6b-153">How Do I Build On Hot Towel?</span></span>

<span data-ttu-id="26e6b-154">Simplemente empiece a agregar el código.</span><span class="sxs-lookup"><span data-stu-id="26e6b-154">Simply start adding code!</span></span>

1. <span data-ttu-id="26e6b-155">Agregar su propio código del lado servidor, preferiblemente Entity Framework y WebAPI (que realmente se destacan con Breeze.js)</span><span class="sxs-lookup"><span data-stu-id="26e6b-155">Add your own server-side code, preferably Entity Framework and WebAPI (which really shine with Breeze.js)</span></span>
2. <span data-ttu-id="26e6b-156">Agregar vistas para la `App/views` carpeta</span><span class="sxs-lookup"><span data-stu-id="26e6b-156">Add views to the `App/views` folder</span></span>
3. <span data-ttu-id="26e6b-157">Agregar viewmodels a la `App/viewmodels` carpeta</span><span class="sxs-lookup"><span data-stu-id="26e6b-157">Add viewmodels to the `App/viewmodels` folder</span></span>
4. <span data-ttu-id="26e6b-158">Agregar enlaces de datos HTML y Knockout a las vistas nuevas</span><span class="sxs-lookup"><span data-stu-id="26e6b-158">Add HTML and Knockout data bindings to your new views</span></span>
5. <span data-ttu-id="26e6b-159">Actualizar las rutas de navegación de`shell.js`</span><span class="sxs-lookup"><span data-stu-id="26e6b-159">Update the navigation routes in `shell.js`</span></span>

## <a name="walkthrough-of-the-htmljavascript"></a><span data-ttu-id="26e6b-160">Tutorial de HTML/JavaScript</span><span class="sxs-lookup"><span data-stu-id="26e6b-160">Walkthrough of the HTML/JavaScript</span></span>

### <a name="viewshottowelindexcshtml"></a><span data-ttu-id="26e6b-161">Views/HotTowel/index.cshtml</span><span class="sxs-lookup"><span data-stu-id="26e6b-161">Views/HotTowel/index.cshtml</span></span>

<span data-ttu-id="26e6b-162">index.cshtml es la ruta de inicio y la vista para la aplicación de MVC.</span><span class="sxs-lookup"><span data-stu-id="26e6b-162">index.cshtml is the starting route and view for the MVC application.</span></span> <span data-ttu-id="26e6b-163">Contiene todos los metaetiquetas estándar, vínculos de css y referencias de JavaScript que se esperaría.</span><span class="sxs-lookup"><span data-stu-id="26e6b-163">It contains all the standard meta tags, css links, and JavaScript references you would expect.</span></span> <span data-ttu-id="26e6b-164">El cuerpo contiene un único `<div>` que es donde todo el contenido (las vistas) se colocará cuando las soliciten.</span><span class="sxs-lookup"><span data-stu-id="26e6b-164">The body contains a single `<div>` which is where all of the content (your views) will be placed when they are requested.</span></span> <span data-ttu-id="26e6b-165">El `@Scripts.Render` Require.js se utiliza para ejecutar el punto de entrada para el código de la aplicación, que se encuentra en el `main.js` archivo.</span><span class="sxs-lookup"><span data-stu-id="26e6b-165">The `@Scripts.Render` uses Require.js to run the entrance point for the application's code, which is contained in the `main.js` file.</span></span> <span data-ttu-id="26e6b-166">Se proporciona una pantalla de presentación para mostrar cómo crear una pantalla de presentación mientras se carga la aplicación.</span><span class="sxs-lookup"><span data-stu-id="26e6b-166">A splash screen is provided to demonstrate how to create a splash screen while your app loads.</span></span>

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a><span data-ttu-id="26e6b-167">App/Main.js</span><span class="sxs-lookup"><span data-stu-id="26e6b-167">App/main.js</span></span>

<span data-ttu-id="26e6b-168">El `main.js` archivo contiene el código que se ejecutará en cuanto se carga la aplicación.</span><span class="sxs-lookup"><span data-stu-id="26e6b-168">The `main.js` file contains the code that will run as soon as your app is loaded.</span></span> <span data-ttu-id="26e6b-169">Esto es donde desea definir sus rutas de navegación, establezca la vistas de inicio y realizar cualquier instalación/arranque como desbloquear los datos de aplicación.</span><span class="sxs-lookup"><span data-stu-id="26e6b-169">This is where you want to define your navigation routes, set your start up views, and perform any setup/bootstrapping such as priming your application's data.</span></span>

<span data-ttu-id="26e6b-170">El `main.js` archivo define varios módulos del durandal para ayudar a la puesta en marcha aplicación iniciar.</span><span class="sxs-lookup"><span data-stu-id="26e6b-170">The `main.js` file defines several of durandal's modules to help the application kick start.</span></span> <span data-ttu-id="26e6b-171">La instrucción de definir ayuda a resolver las dependencias de módulos para que estén disponibles para la función.</span><span class="sxs-lookup"><span data-stu-id="26e6b-171">The define statement helps resolve the modules dependencies so they are available for the function.</span></span> <span data-ttu-id="26e6b-172">Los mensajes de depuración se habilitan por primera vez, que enviar mensajes sobre eventos que la aplicación está realizando en la ventana de consola.</span><span class="sxs-lookup"><span data-stu-id="26e6b-172">First the debugging messages are enabled, which send messages about what events the application is performing to the console window.</span></span> <span data-ttu-id="26e6b-173">El código de app.start indica durandal framework para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="26e6b-173">The app.start code tells durandal framework to start the application.</span></span> <span data-ttu-id="26e6b-174">Las convenciones se establecen para que durandal sabe todas las vistas y viewmodels están incluidos en las mismas carpetas con nombre, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="26e6b-174">The conventions are set so that durandal knows all views and viewmodels are contained in the same named folders, respectively.</span></span> <span data-ttu-id="26e6b-175">Por último, el `app.setRoot` inicia, carga el `shell` mediante una plantilla predeterminada `entrance` animación.</span><span class="sxs-lookup"><span data-stu-id="26e6b-175">Finally, the `app.setRoot` kicks loads the `shell` using a predefined `entrance` animation.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a><span data-ttu-id="26e6b-176">Vistas</span><span class="sxs-lookup"><span data-stu-id="26e6b-176">Views</span></span>

<span data-ttu-id="26e6b-177">Las vistas se encuentran en el `App/views` carpeta.</span><span class="sxs-lookup"><span data-stu-id="26e6b-177">Views are found in the `App/views` folder.</span></span>

### <a name="shellhtml"></a><span data-ttu-id="26e6b-178">Shell.HTML</span><span class="sxs-lookup"><span data-stu-id="26e6b-178">shell.html</span></span>

<span data-ttu-id="26e6b-179">El `shell.html` contiene el diseño del patrón para el código HTML.</span><span class="sxs-lookup"><span data-stu-id="26e6b-179">The `shell.html` contains the master layout for your HTML.</span></span> <span data-ttu-id="26e6b-180">Todas las demás vistas se compondrán en algún lugar en el lado de su `shell` vista.</span><span class="sxs-lookup"><span data-stu-id="26e6b-180">All of your other views will be composed somewhere in side of your `shell` view.</span></span> <span data-ttu-id="26e6b-181">Toallas activa proporciona un `shell` con tres de estas regiones: un encabezado, un área de contenido y un pie de página.</span><span class="sxs-lookup"><span data-stu-id="26e6b-181">Hot Towel provides a `shell` with three such regions: a header, a content area, and a footer.</span></span> <span data-ttu-id="26e6b-182">Cada una de estas regiones se ha cargado con contenido forma otras vistas cuando se solicita.</span><span class="sxs-lookup"><span data-stu-id="26e6b-182">Each of these regions is loaded with contents form other views when requested.</span></span>

<span data-ttu-id="26e6b-183">El `compose` enlaces para el encabezado y pie de página son difíciles de codificar de toallas activa para que apunte a la `nav` y `footer` vistas, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="26e6b-183">The `compose` bindings for the header and footer are hard coded in Hot Towel to point to the `nav` and `footer` views, respectively.</span></span> <span data-ttu-id="26e6b-184">El enlace de redacción de la sección `#content` está enlazado a la `router` elemento activo del módulo.</span><span class="sxs-lookup"><span data-stu-id="26e6b-184">The compose binding for the section `#content` is bound to the `router` module's active item.</span></span> <span data-ttu-id="26e6b-185">En otras palabras, al hacer clic en un vínculo de navegación es vista correspondiente se carga en esta área.</span><span class="sxs-lookup"><span data-stu-id="26e6b-185">In other words, when you click a navigation link it's corresponding view is loaded in this area.</span></span>

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a><span data-ttu-id="26e6b-186">NAV.HTML</span><span class="sxs-lookup"><span data-stu-id="26e6b-186">nav.html</span></span>

<span data-ttu-id="26e6b-187">El `nav.html` contiene los vínculos de navegación de la aplicación SPA.</span><span class="sxs-lookup"><span data-stu-id="26e6b-187">The `nav.html` contains the navigation links for the SPA.</span></span> <span data-ttu-id="26e6b-188">Esto es que la estructura de menú se puede colocar, por ejemplo.</span><span class="sxs-lookup"><span data-stu-id="26e6b-188">This is where the menu structure can be placed, for example.</span></span> <span data-ttu-id="26e6b-189">Con frecuencia, es datos enlazados (mediante Knockout) a la `router` módulo para mostrar el panel de navegación definida en el `shell.js`.</span><span class="sxs-lookup"><span data-stu-id="26e6b-189">Often this is data bound (using Knockout) to the `router` module to display the navigation you defined in the `shell.js`.</span></span> <span data-ttu-id="26e6b-190">Knockout busca el enlace de datos, atributos y enlaza los utilizados para la `shell` viewmodel para mostrar las rutas de navegación y para mostrar una barra de progreso (mediante arranque de Twitter) si el `router` módulo está en medio de navegar desde una vista a otra (consulte `router.isNavigating`).</span><span class="sxs-lookup"><span data-stu-id="26e6b-190">Knockout looks for the data-bind attributes and binds those to the `shell` viewmodel to display the navigation routes and to show a progressbar (using Twitter Bootstrap) if the `router` module is in the middle of navigating from one view to another (see `router.isNavigating`).</span></span>

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a><span data-ttu-id="26e6b-191">Home.HTML y details.html</span><span class="sxs-lookup"><span data-stu-id="26e6b-191">home.html and details.html</span></span>

<span data-ttu-id="26e6b-192">Estas vistas contienen HTML de vistas personalizadas.</span><span class="sxs-lookup"><span data-stu-id="26e6b-192">These views contain HTML for custom views.</span></span> <span data-ttu-id="26e6b-193">Cuando el `home` vincular en el `nav` se hace clic en el menú de la vista, la `home` vista se colocará en el área de contenido de la `shell` vista.</span><span class="sxs-lookup"><span data-stu-id="26e6b-193">When the `home` link in the `nav` view's menu is clicked, the `home` view will be placed in the content area of the `shell` view.</span></span> <span data-ttu-id="26e6b-194">Estas vistas se pueden aumentar o reemplazar con sus propias vistas personalizadas.</span><span class="sxs-lookup"><span data-stu-id="26e6b-194">These views can be augmented or replaced with your own custom views.</span></span>

### <a name="footerhtml"></a><span data-ttu-id="26e6b-195">Footer.HTML</span><span class="sxs-lookup"><span data-stu-id="26e6b-195">footer.html</span></span>

<span data-ttu-id="26e6b-196">El `footer.html` contiene código HTML que aparece en el pie de página, en la parte inferior de la `shell` vista.</span><span class="sxs-lookup"><span data-stu-id="26e6b-196">The `footer.html` contains HTML that appears in the footer, at the bottom of the `shell` view.</span></span>

## <a name="viewmodels"></a><span data-ttu-id="26e6b-197">ViewModels</span><span class="sxs-lookup"><span data-stu-id="26e6b-197">ViewModels</span></span>

<span data-ttu-id="26e6b-198">ViewModels se encuentran en el `App/viewmodels` carpeta.</span><span class="sxs-lookup"><span data-stu-id="26e6b-198">ViewModels are found in the `App/viewmodels` folder.</span></span>

### <a name="shelljs"></a><span data-ttu-id="26e6b-199">Shell.js</span><span class="sxs-lookup"><span data-stu-id="26e6b-199">shell.js</span></span>

<span data-ttu-id="26e6b-200">El `shell` viewmodel contiene propiedades y funciones que están enlazadas a la `shell` vista.</span><span class="sxs-lookup"><span data-stu-id="26e6b-200">The `shell` viewmodel contains properties and functions that are bound to the `shell` view.</span></span> <span data-ttu-id="26e6b-201">Con frecuencia, es donde se encuentran los enlaces de navegación de menú (consulte la `router.mapNav` lógica).</span><span class="sxs-lookup"><span data-stu-id="26e6b-201">Often this is where the menu navigation bindings are found (see the `router.mapNav` logic).</span></span>

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a><span data-ttu-id="26e6b-202">Home.js y details.js</span><span class="sxs-lookup"><span data-stu-id="26e6b-202">home.js and details.js</span></span>

<span data-ttu-id="26e6b-203">Estos viewmodels contienen las propiedades y las funciones que están enlazadas a la `home` vista.</span><span class="sxs-lookup"><span data-stu-id="26e6b-203">These viewmodels contain the properties and functions that are bound to the `home` view.</span></span> <span data-ttu-id="26e6b-204">También contiene la lógica de presentación de la vista y es el elemento aglutinante entre los datos y la vista.</span><span class="sxs-lookup"><span data-stu-id="26e6b-204">it also contains the presentation logic for the view, and is the glue between the data and the view.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a><span data-ttu-id="26e6b-205">Servicios</span><span class="sxs-lookup"><span data-stu-id="26e6b-205">Services</span></span>

<span data-ttu-id="26e6b-206">Los servicios se encuentran en la carpeta/Servicios de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="26e6b-206">Services are found in the App/services folder.</span></span> <span data-ttu-id="26e6b-207">Lo ideal es que los servicios futuros como un módulo dataservice, que es responsable de obtener y enviar datos remotos, pudieron colocarse.</span><span class="sxs-lookup"><span data-stu-id="26e6b-207">Ideally your future services such as a dataservice module, that is responsible for getting and posting remote data, could be placed.</span></span>

### <a name="loggerjs"></a><span data-ttu-id="26e6b-208">Logger.js</span><span class="sxs-lookup"><span data-stu-id="26e6b-208">logger.js</span></span>

<span data-ttu-id="26e6b-209">Toallas activa proporciona un `logger` módulo en la carpeta de servicios.</span><span class="sxs-lookup"><span data-stu-id="26e6b-209">Hot Towel provides a `logger` module in the services folder.</span></span> <span data-ttu-id="26e6b-210">El `logger` módulo es ideal para registrar los mensajes en la consola y para el usuario en emergente notificaciones del sistema.</span><span class="sxs-lookup"><span data-stu-id="26e6b-210">The `logger` module is ideal for logging messages to the console and to the user in pop up toasts.</span></span>
