---
uid: single-page-application/overview/templates/hottowel-template
title: Plantilla de hot toalla | Microsoft Docs
author: madskristensen
description: Plantilla HotTowel
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/09/2013
ms.topic: article
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
ms.technology: ''
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: de81f12f57d7f2fb7c6478bfa1f3a278ae905a39
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388556"
---
<a name="hot-towel-template"></a><span data-ttu-id="72409-103">Plantilla de hot Towel</span><span class="sxs-lookup"><span data-stu-id="72409-103">Hot Towel template</span></span>
====================
<span data-ttu-id="72409-104">por [Mads Kristensen](https://github.com/madskristensen)</span><span class="sxs-lookup"><span data-stu-id="72409-104">by [Mads Kristensen](https://github.com/madskristensen)</span></span>

> <span data-ttu-id="72409-105">La plantilla de MVC toalla activo está escrita por John Papa</span><span class="sxs-lookup"><span data-stu-id="72409-105">The Hot Towel MVC Template is written by John Papa</span></span>
> 
> <span data-ttu-id="72409-106">Elija qué versión desea descargar:</span><span class="sxs-lookup"><span data-stu-id="72409-106">Choose which version to download:</span></span>
> 
> [<span data-ttu-id="72409-107">Plantilla MVC toalla activo para Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="72409-107">Hot Towel MVC Template for Visual Studio 2012</span></span>](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [<span data-ttu-id="72409-108">Plantilla MVC toalla activo para Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="72409-108">Hot Towel MVC Template for Visual Studio 2013</span></span>](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)
> 
> 
> <span data-ttu-id="72409-109">Hot toalla: Ya no quiere ir a la SPA sin uno!</span><span class="sxs-lookup"><span data-stu-id="72409-109">Hot Towel: Because you don't want to go to the SPA without one!</span></span>


<span data-ttu-id="72409-110">¿Si desea crear una SPA, pero no se puede decidir dónde empezar?</span><span class="sxs-lookup"><span data-stu-id="72409-110">Want to build a SPA but can't decide where to start?</span></span> <span data-ttu-id="72409-111">Utilizar una toalla activo y en segundos, tendrá una SPA y todas las herramientas que necesita para crear en él!</span><span class="sxs-lookup"><span data-stu-id="72409-111">Use Hot Towel and in seconds you'll have a SPA and all the tools you need to build on it!</span></span>

<span data-ttu-id="72409-112">Hot toalla crea un excelente punto de partida para la creación de una aplicación de página única (SPA) con ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="72409-112">Hot Towel creates a great starting point for building a Single Page Application (SPA) with ASP.NET.</span></span> <span data-ttu-id="72409-113">De fábrica se proporciona una estructura modular para su código, navegación de la vista, el enlace de datos, administración de datos enriquecidos y estilo sencilla pero elegante.</span><span class="sxs-lookup"><span data-stu-id="72409-113">Out of the box you it provides a modular structure for your code, view navigation, data binding, rich data management and simple but elegant styling.</span></span> <span data-ttu-id="72409-114">Hot toalla proporciona todo lo que necesita para crear una SPA, para que pueda centrarse en la aplicación, no las conexiones subyacentes.</span><span class="sxs-lookup"><span data-stu-id="72409-114">Hot Towel provides everything you need to build a SPA, so you can focus on your app, not the plumbing.</span></span>

> <span data-ttu-id="72409-115">Más información acerca de cómo crear una SPA de [vídeos, tutoriales y cursos de Pluralsight de John Papa](http://johnpapa.net/spa?vsix).</span><span class="sxs-lookup"><span data-stu-id="72409-115">Learn more about building a SPA from [John Papa's videos, tutorials and Pluralsight courses](http://johnpapa.net/spa?vsix).</span></span>


## <a name="application-structure"></a><span data-ttu-id="72409-116">Estructura de la aplicación</span><span class="sxs-lookup"><span data-stu-id="72409-116">Application Structure</span></span>

<span data-ttu-id="72409-117">Hot SPA toalla proporciona una carpeta de la aplicación que contiene los archivos JavaScript y HTML que definen la aplicación.</span><span class="sxs-lookup"><span data-stu-id="72409-117">Hot Towel SPA provides an App folder which contains the JavaScript and HTML files that define your application.</span></span>

<span data-ttu-id="72409-118">Dentro de la carpeta de aplicación:</span><span class="sxs-lookup"><span data-stu-id="72409-118">Inside the App folder:</span></span>

- <span data-ttu-id="72409-119">Durandal</span><span class="sxs-lookup"><span data-stu-id="72409-119">durandal</span></span>
- <span data-ttu-id="72409-120">servicios</span><span class="sxs-lookup"><span data-stu-id="72409-120">services</span></span>
- <span data-ttu-id="72409-121">modelos de vista</span><span class="sxs-lookup"><span data-stu-id="72409-121">viewmodels</span></span>
- <span data-ttu-id="72409-122">vistas</span><span class="sxs-lookup"><span data-stu-id="72409-122">views</span></span>

<span data-ttu-id="72409-123">La carpeta de aplicación contiene una colección de módulos.</span><span class="sxs-lookup"><span data-stu-id="72409-123">The App folder contains a collection of modules.</span></span> <span data-ttu-id="72409-124">Estos módulos encapsulan la funcionalidad y declaran las dependencias en otros módulos.</span><span class="sxs-lookup"><span data-stu-id="72409-124">These modules encapsulate functionality and declare dependencies on other modules.</span></span> <span data-ttu-id="72409-125">La carpeta views contiene el código HTML de la aplicación y la carpeta viewmodels contiene la lógica de presentación para las vistas (un patrón común en MVVM).</span><span class="sxs-lookup"><span data-stu-id="72409-125">The views folder contains the HTML for your application and the viewmodels folder contains the presentation logic for the views (a common MVVM pattern).</span></span> <span data-ttu-id="72409-126">La carpeta de servicios es ideal para los servicios comunes que la aplicación puede necesitar, como la recuperación de datos HTTP o la interacción de almacenamiento local de la caja.</span><span class="sxs-lookup"><span data-stu-id="72409-126">The services folder is ideal for housing any common services that your application may need such as HTTP data retrieval or local storage interaction.</span></span> <span data-ttu-id="72409-127">Es común para varios ViewModel reutilizar el código de los módulos del servicio.</span><span class="sxs-lookup"><span data-stu-id="72409-127">It is common for multiple viewmodels to re-use code from the service modules.</span></span>

## <a name="aspnet-mvc-server-side-application-structure"></a><span data-ttu-id="72409-128">Estructura de aplicación de lado servidor de ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="72409-128">ASP.NET MVC Server Side Application Structure</span></span>

<span data-ttu-id="72409-129">Hot toalla se basa en la estructura de MVC de ASP.NET eficaces y familiar.</span><span class="sxs-lookup"><span data-stu-id="72409-129">Hot Towel builds on the familiar and powerful ASP.NET MVC structure.</span></span>

- <span data-ttu-id="72409-130">Aplicación\_iniciar</span><span class="sxs-lookup"><span data-stu-id="72409-130">App\_Start</span></span>
- <span data-ttu-id="72409-131">Contenido</span><span class="sxs-lookup"><span data-stu-id="72409-131">Content</span></span>
- <span data-ttu-id="72409-132">Controladores</span><span class="sxs-lookup"><span data-stu-id="72409-132">Controllers</span></span>
- <span data-ttu-id="72409-133">Modelos</span><span class="sxs-lookup"><span data-stu-id="72409-133">Models</span></span>
- <span data-ttu-id="72409-134">Scripts</span><span class="sxs-lookup"><span data-stu-id="72409-134">Scripts</span></span>
- <span data-ttu-id="72409-135">Vistas</span><span class="sxs-lookup"><span data-stu-id="72409-135">Views</span></span>

## <a name="featured-libraries"></a><span data-ttu-id="72409-136">Bibliotecas de destacados</span><span class="sxs-lookup"><span data-stu-id="72409-136">Featured Libraries</span></span>

- <span data-ttu-id="72409-137">ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="72409-137">ASP.NET MVC</span></span>
- <span data-ttu-id="72409-138">ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="72409-138">ASP.NET Web API</span></span>
- <span data-ttu-id="72409-139">Optimización de ASP.NET Web - unión y minificación</span><span class="sxs-lookup"><span data-stu-id="72409-139">ASP.NET Web Optimization - bundling and minification</span></span>
- <span data-ttu-id="72409-140">[BREEZE.js](http://Breezejs.com) -administración de datos enriquecidos</span><span class="sxs-lookup"><span data-stu-id="72409-140">[Breeze.js](http://Breezejs.com) - rich data management</span></span>
- <span data-ttu-id="72409-141">[Durandal.js](http://Durandaljs.com) -navegación y composición de la vista</span><span class="sxs-lookup"><span data-stu-id="72409-141">[Durandal.js](http://Durandaljs.com) - navigation and View composition</span></span>
- <span data-ttu-id="72409-142">[Knockout.js](http://Knockoutjs.com) -enlaces de datos</span><span class="sxs-lookup"><span data-stu-id="72409-142">[Knockout.js](http://Knockoutjs.com) - data bindings</span></span>
- <span data-ttu-id="72409-143">[Require.js](http://requirejs.org) -modularidad con AMD y optimización</span><span class="sxs-lookup"><span data-stu-id="72409-143">[Require.js](http://requirejs.org) - Modularity with AMD and optimization</span></span>
- <span data-ttu-id="72409-144">[Toastr.js](http://jpapa.me/c7toastr) -mensajes emergentes</span><span class="sxs-lookup"><span data-stu-id="72409-144">[Toastr.js](http://jpapa.me/c7toastr) - pop-up messages</span></span>
- <span data-ttu-id="72409-145">[Twitter Bootstrap](http://twitter.github.com/bootstrap/) : estilos de CSS sólido</span><span class="sxs-lookup"><span data-stu-id="72409-145">[Twitter Bootstrap](http://twitter.github.com/bootstrap/) - robust CSS styling</span></span>

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a><span data-ttu-id="72409-146">Instalar a través de la plantilla SPA toalla activo de Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="72409-146">Installing via the Visual Studio 2012 Hot Towel SPA Template</span></span>

<span data-ttu-id="72409-147">Hot toalla puede instalarse como una plantilla de Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="72409-147">Hot Towel can be installed as a Visual Studio 2012 template.</span></span> <span data-ttu-id="72409-148">Simplemente haga clic en `File`  |  `New Project` y elija `ASP.NET MVC 4 Web Application`.</span><span class="sxs-lookup"><span data-stu-id="72409-148">Just click `File` | `New Project` and choose `ASP.NET MVC 4 Web Application`.</span></span> <span data-ttu-id="72409-149">A continuación, seleccione el "Hot toalla de aplicación de página única" plantilla y ejecute!</span><span class="sxs-lookup"><span data-stu-id="72409-149">Then select the 'Hot Towel Single Page Application" template and run!</span></span>

## <a name="installing-via-the-nuget-package"></a><span data-ttu-id="72409-150">Instalar mediante el paquete NuGet</span><span class="sxs-lookup"><span data-stu-id="72409-150">Installing via the NuGet Package</span></span>

<span data-ttu-id="72409-151">Hot toalla también es un paquete de NuGet que aumenta en un proyecto vacío de ASP.NET MVC existente.</span><span class="sxs-lookup"><span data-stu-id="72409-151">Hot Towel is also a NuGet package that augments an existing empty ASP.NET MVC project.</span></span> <span data-ttu-id="72409-152">Sólo tiene que instalar mediante Nuget y, a continuación, ejecute.</span><span class="sxs-lookup"><span data-stu-id="72409-152">Just install using Nuget and then run!</span></span>

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a><span data-ttu-id="72409-153">¿Cómo se compila en caliente toalla?</span><span class="sxs-lookup"><span data-stu-id="72409-153">How Do I Build On Hot Towel?</span></span>

<span data-ttu-id="72409-154">Simplemente empiece a agregar código.</span><span class="sxs-lookup"><span data-stu-id="72409-154">Simply start adding code!</span></span>

1. <span data-ttu-id="72409-155">Agregar su propio código del lado servidor, preferiblemente de Entity Framework y API Web (que realmente se lucen con Breeze.js)</span><span class="sxs-lookup"><span data-stu-id="72409-155">Add your own server-side code, preferably Entity Framework and WebAPI (which really shine with Breeze.js)</span></span>
2. <span data-ttu-id="72409-156">Agregar vistas a la `App/views` carpeta</span><span class="sxs-lookup"><span data-stu-id="72409-156">Add views to the `App/views` folder</span></span>
3. <span data-ttu-id="72409-157">Agregar modelos de vista a la `App/viewmodels` carpeta</span><span class="sxs-lookup"><span data-stu-id="72409-157">Add viewmodels to the `App/viewmodels` folder</span></span>
4. <span data-ttu-id="72409-158">Agregar enlaces de datos HTML y Knockout a las nuevas vistas</span><span class="sxs-lookup"><span data-stu-id="72409-158">Add HTML and Knockout data bindings to your new views</span></span>
5. <span data-ttu-id="72409-159">Actualizar las rutas de navegación `shell.js`</span><span class="sxs-lookup"><span data-stu-id="72409-159">Update the navigation routes in `shell.js`</span></span>

## <a name="walkthrough-of-the-htmljavascript"></a><span data-ttu-id="72409-160">Tutorial sobre la HTML/JavaScript</span><span class="sxs-lookup"><span data-stu-id="72409-160">Walkthrough of the HTML/JavaScript</span></span>

### <a name="viewshottowelindexcshtml"></a><span data-ttu-id="72409-161">Views/HotTowel/index.cshtml</span><span class="sxs-lookup"><span data-stu-id="72409-161">Views/HotTowel/index.cshtml</span></span>

<span data-ttu-id="72409-162">index.cshtml es la ruta de inicio y la vista para la aplicación de MVC.</span><span class="sxs-lookup"><span data-stu-id="72409-162">index.cshtml is the starting route and view for the MVC application.</span></span> <span data-ttu-id="72409-163">Contiene todos los estándar etiquetas meta, vínculos de css y las referencias de JavaScript que cabría esperar.</span><span class="sxs-lookup"><span data-stu-id="72409-163">It contains all the standard meta tags, css links, and JavaScript references you would expect.</span></span> <span data-ttu-id="72409-164">El cuerpo contiene un único `<div>` que es donde todo el contenido (las vistas) se colocarán cuando se solicitan.</span><span class="sxs-lookup"><span data-stu-id="72409-164">The body contains a single `<div>` which is where all of the content (your views) will be placed when they are requested.</span></span> <span data-ttu-id="72409-165">El `@Scripts.Render` Require.js se utiliza para ejecutar el punto de entrada para el código de la aplicación, que se encuentra en el `main.js` archivo.</span><span class="sxs-lookup"><span data-stu-id="72409-165">The `@Scripts.Render` uses Require.js to run the entrance point for the application's code, which is contained in the `main.js` file.</span></span> <span data-ttu-id="72409-166">Se proporciona una pantalla de presentación para mostrar cómo crear una pantalla de presentación mientras se carga la aplicación.</span><span class="sxs-lookup"><span data-stu-id="72409-166">A splash screen is provided to demonstrate how to create a splash screen while your app loads.</span></span>

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a><span data-ttu-id="72409-167">App/Main.js</span><span class="sxs-lookup"><span data-stu-id="72409-167">App/main.js</span></span>

<span data-ttu-id="72409-168">El `main.js` archivo contiene el código que se ejecutará en cuanto se carga la aplicación.</span><span class="sxs-lookup"><span data-stu-id="72409-168">The `main.js` file contains the code that will run as soon as your app is loaded.</span></span> <span data-ttu-id="72409-169">Esto es donde desea definir las rutas de navegación, establezca el inicio de las vistas y realizar cualquier programa de instalación o de arranque como desbloquear los datos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="72409-169">This is where you want to define your navigation routes, set your start up views, and perform any setup/bootstrapping such as priming your application's data.</span></span>

<span data-ttu-id="72409-170">El `main.js` archivo define varios de los módulos de durandal para ayudar a la puesta en marcha aplicaciones de inicio.</span><span class="sxs-lookup"><span data-stu-id="72409-170">The `main.js` file defines several of durandal's modules to help the application kick start.</span></span> <span data-ttu-id="72409-171">La instrucción definir ayuda a resolver las dependencias de los módulos para que estén disponibles para la función.</span><span class="sxs-lookup"><span data-stu-id="72409-171">The define statement helps resolve the modules dependencies so they are available for the function.</span></span> <span data-ttu-id="72409-172">En primer lugar se habilitan los mensajes de depuración, que envían mensajes acerca de qué eventos de que la aplicación se está ejecutando en la ventana de consola.</span><span class="sxs-lookup"><span data-stu-id="72409-172">First the debugging messages are enabled, which send messages about what events the application is performing to the console window.</span></span> <span data-ttu-id="72409-173">El código app.start indica el marco de trabajo de durandal para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="72409-173">The app.start code tells durandal framework to start the application.</span></span> <span data-ttu-id="72409-174">Las convenciones se establecen para que durandal sabe todas las vistas y viewmodels están incluidos en las mismas carpetas con nombre, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="72409-174">The conventions are set so that durandal knows all views and viewmodels are contained in the same named folders, respectively.</span></span> <span data-ttu-id="72409-175">Por último, el `app.setRoot` cargas se inicia el `shell` mediante una plantilla predeterminada `entrance` animación.</span><span class="sxs-lookup"><span data-stu-id="72409-175">Finally, the `app.setRoot` kicks loads the `shell` using a predefined `entrance` animation.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a><span data-ttu-id="72409-176">Vistas</span><span class="sxs-lookup"><span data-stu-id="72409-176">Views</span></span>

<span data-ttu-id="72409-177">Las vistas se encuentran en el `App/views` carpeta.</span><span class="sxs-lookup"><span data-stu-id="72409-177">Views are found in the `App/views` folder.</span></span>

### <a name="shellhtml"></a><span data-ttu-id="72409-178">Shell.HTML</span><span class="sxs-lookup"><span data-stu-id="72409-178">shell.html</span></span>

<span data-ttu-id="72409-179">El `shell.html` contiene el diseño maestra para el código HTML.</span><span class="sxs-lookup"><span data-stu-id="72409-179">The `shell.html` contains the master layout for your HTML.</span></span> <span data-ttu-id="72409-180">Todas las vistas se compone en algún lugar en el lado de su `shell` vista.</span><span class="sxs-lookup"><span data-stu-id="72409-180">All of your other views will be composed somewhere in side of your `shell` view.</span></span> <span data-ttu-id="72409-181">Hot toalla proporciona un `shell` con tres de estas regiones: un encabezado, un área de contenido y un pie de página.</span><span class="sxs-lookup"><span data-stu-id="72409-181">Hot Towel provides a `shell` with three such regions: a header, a content area, and a footer.</span></span> <span data-ttu-id="72409-182">Cada una de estas regiones se carga con contenido forma a otras vistas cuando se solicita.</span><span class="sxs-lookup"><span data-stu-id="72409-182">Each of these regions is loaded with contents form other views when requested.</span></span>

<span data-ttu-id="72409-183">El `compose` enlaces para el encabezado y pie de página están codificados en toalla activo para que apunte a la `nav` y `footer` vistas, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="72409-183">The `compose` bindings for the header and footer are hard coded in Hot Towel to point to the `nav` and `footer` views, respectively.</span></span> <span data-ttu-id="72409-184">El enlace de compose de la sección `#content` está enlazado a la `router` elemento activo del módulo.</span><span class="sxs-lookup"><span data-stu-id="72409-184">The compose binding for the section `#content` is bound to the `router` module's active item.</span></span> <span data-ttu-id="72409-185">En otras palabras, al hacer clic en un vínculo de navegación es la vista correspondiente se carga en esta área.</span><span class="sxs-lookup"><span data-stu-id="72409-185">In other words, when you click a navigation link it's corresponding view is loaded in this area.</span></span>

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a><span data-ttu-id="72409-186">NAV.HTML</span><span class="sxs-lookup"><span data-stu-id="72409-186">nav.html</span></span>

<span data-ttu-id="72409-187">El `nav.html` contiene los vínculos de navegación de la SPA.</span><span class="sxs-lookup"><span data-stu-id="72409-187">The `nav.html` contains the navigation links for the SPA.</span></span> <span data-ttu-id="72409-188">Esto es donde la estructura de menú se puede colocar, por ejemplo.</span><span class="sxs-lookup"><span data-stu-id="72409-188">This is where the menu structure can be placed, for example.</span></span> <span data-ttu-id="72409-189">Suele ser datos enlazados (con Knockout) a la `router` módulo para mostrar el panel de navegación que definió en el `shell.js`.</span><span class="sxs-lookup"><span data-stu-id="72409-189">Often this is data bound (using Knockout) to the `router` module to display the navigation you defined in the `shell.js`.</span></span> <span data-ttu-id="72409-190">Knockout busca el enlace de datos de atributos y enlaza los de la `shell` viewmodel para mostrar las rutas de navegación y para mostrar una barra de progreso (con Twitter Bootstrap) si el `router` módulo está en medio de navegar desde una vista a otra (consulte `router.isNavigating`).</span><span class="sxs-lookup"><span data-stu-id="72409-190">Knockout looks for the data-bind attributes and binds those to the `shell` viewmodel to display the navigation routes and to show a progressbar (using Twitter Bootstrap) if the `router` module is in the middle of navigating from one view to another (see `router.isNavigating`).</span></span>

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a><span data-ttu-id="72409-191">Home.HTML y details.html</span><span class="sxs-lookup"><span data-stu-id="72409-191">home.html and details.html</span></span>

<span data-ttu-id="72409-192">Estas vistas contienen HTML para las vistas personalizadas.</span><span class="sxs-lookup"><span data-stu-id="72409-192">These views contain HTML for custom views.</span></span> <span data-ttu-id="72409-193">Cuando el `home` vincular en el `nav` se hace clic en el menú de la vista, el `home` vista se colocará en el área de contenido de la `shell` vista.</span><span class="sxs-lookup"><span data-stu-id="72409-193">When the `home` link in the `nav` view's menu is clicked, the `home` view will be placed in the content area of the `shell` view.</span></span> <span data-ttu-id="72409-194">Estas vistas se pueden ampliar o reemplazadas con sus propias vistas personalizadas.</span><span class="sxs-lookup"><span data-stu-id="72409-194">These views can be augmented or replaced with your own custom views.</span></span>

### <a name="footerhtml"></a><span data-ttu-id="72409-195">Footer.HTML</span><span class="sxs-lookup"><span data-stu-id="72409-195">footer.html</span></span>

<span data-ttu-id="72409-196">El `footer.html` contiene código HTML que aparece en el pie de página, en la parte inferior de la `shell` vista.</span><span class="sxs-lookup"><span data-stu-id="72409-196">The `footer.html` contains HTML that appears in the footer, at the bottom of the `shell` view.</span></span>

## <a name="viewmodels"></a><span data-ttu-id="72409-197">ViewModels</span><span class="sxs-lookup"><span data-stu-id="72409-197">ViewModels</span></span>

<span data-ttu-id="72409-198">ViewModel se encuentra en el `App/viewmodels` carpeta.</span><span class="sxs-lookup"><span data-stu-id="72409-198">ViewModels are found in the `App/viewmodels` folder.</span></span>

### <a name="shelljs"></a><span data-ttu-id="72409-199">Shell.js</span><span class="sxs-lookup"><span data-stu-id="72409-199">shell.js</span></span>

<span data-ttu-id="72409-200">El `shell` viewmodel contiene propiedades y funciones que están enlazadas a la `shell` vista.</span><span class="sxs-lookup"><span data-stu-id="72409-200">The `shell` viewmodel contains properties and functions that are bound to the `shell` view.</span></span> <span data-ttu-id="72409-201">A menudo esto es donde se encuentran los enlaces de navegación del menú (consulte la `router.mapNav` lógica).</span><span class="sxs-lookup"><span data-stu-id="72409-201">Often this is where the menu navigation bindings are found (see the `router.mapNav` logic).</span></span>

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a><span data-ttu-id="72409-202">Home.js y details.js</span><span class="sxs-lookup"><span data-stu-id="72409-202">home.js and details.js</span></span>

<span data-ttu-id="72409-203">Estos modelos de vista contienen las propiedades y funciones que están enlazadas a la `home` vista.</span><span class="sxs-lookup"><span data-stu-id="72409-203">These viewmodels contain the properties and functions that are bound to the `home` view.</span></span> <span data-ttu-id="72409-204">También contiene la lógica de presentación para la vista y es el pegamento entre los datos y la vista.</span><span class="sxs-lookup"><span data-stu-id="72409-204">it also contains the presentation logic for the view, and is the glue between the data and the view.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a><span data-ttu-id="72409-205">Servicios</span><span class="sxs-lookup"><span data-stu-id="72409-205">Services</span></span>

<span data-ttu-id="72409-206">Los servicios se encuentran en la carpeta de aplicaciones y servicios.</span><span class="sxs-lookup"><span data-stu-id="72409-206">Services are found in the App/services folder.</span></span> <span data-ttu-id="72409-207">Lo ideal es que los servicios futuros como un módulo de servicio de datos, que es responsable de obtener y enviar los datos remotos, podrían colocarse.</span><span class="sxs-lookup"><span data-stu-id="72409-207">Ideally your future services such as a dataservice module, that is responsible for getting and posting remote data, could be placed.</span></span>

### <a name="loggerjs"></a><span data-ttu-id="72409-208">Logger.js</span><span class="sxs-lookup"><span data-stu-id="72409-208">logger.js</span></span>

<span data-ttu-id="72409-209">Hot toalla proporciona un `logger` módulo en la carpeta de servicios.</span><span class="sxs-lookup"><span data-stu-id="72409-209">Hot Towel provides a `logger` module in the services folder.</span></span> <span data-ttu-id="72409-210">El `logger` módulo es ideal para registrar mensajes en la consola y al usuario en emergente notificaciones del sistema.</span><span class="sxs-lookup"><span data-stu-id="72409-210">The `logger` module is ideal for logging messages to the console and to the user in pop up toasts.</span></span>
