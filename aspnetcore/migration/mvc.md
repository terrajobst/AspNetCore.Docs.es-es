---
title: "Migración de MVC de ASP.NET a núcleo de ASP.NET MVC"
author: ardalis
description: "Obtenga información acerca de cómo empezar a migrar un proyecto de MVC de ASP.NET a ASP.NET MVC de núcleo."
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/mvc
ms.openlocfilehash: c9c9f63cd635f364d9b2e081dc051a46a44d3e4f
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2018
---
# <a name="migrating-from-aspnet-mvc-to-aspnet-core-mvc"></a><span data-ttu-id="c81b6-103">Migración de MVC de ASP.NET a núcleo de ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="c81b6-103">Migrating from ASP.NET MVC to ASP.NET Core MVC</span></span>

<span data-ttu-id="c81b6-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), y [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="c81b6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="c81b6-105">Este artículo muestra cómo empezar a migrar un proyecto de MVC de ASP.NET a [MVC de ASP.NET Core](../mvc/overview.md).</span><span class="sxs-lookup"><span data-stu-id="c81b6-105">This article shows how to get started migrating an ASP.NET MVC project to [ASP.NET Core MVC](../mvc/overview.md).</span></span> <span data-ttu-id="c81b6-106">En el proceso, resalta muchas de las cosas que han cambiado respecto a ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c81b6-106">In the process, it highlights many of the things that have changed from ASP.NET MVC.</span></span> <span data-ttu-id="c81b6-107">La migración de ASP.NET MVC es un proceso en varias fases y este artículo trata la instalación inicial, controladores básicos y vistas, contenido estático y las dependencias del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="c81b6-107">Migrating from ASP.NET MVC is a multiple step process and this article covers the initial setup, basic controllers and views, static content, and client-side dependencies.</span></span> <span data-ttu-id="c81b6-108">Migrar la configuración y el código de identidad que se encuentra en muchos proyectos de ASP.NET MVC tratan otros artículos.</span><span class="sxs-lookup"><span data-stu-id="c81b6-108">Additional articles cover migrating configuration and identity code found in many ASP.NET MVC projects.</span></span>

> [!NOTE]
> <span data-ttu-id="c81b6-109">Los números de versión en los ejemplos podrían no estar actualizados.</span><span class="sxs-lookup"><span data-stu-id="c81b6-109">The version numbers in the samples might not be current.</span></span> <span data-ttu-id="c81b6-110">Debe actualizar los proyectos en consecuencia.</span><span class="sxs-lookup"><span data-stu-id="c81b6-110">You may need to update your projects accordingly.</span></span>

## <a name="create-the-starter-aspnet-mvc-project"></a><span data-ttu-id="c81b6-111">Crear el proyecto de MVC de ASP.NET de inicio</span><span class="sxs-lookup"><span data-stu-id="c81b6-111">Create the starter ASP.NET MVC project</span></span>

<span data-ttu-id="c81b6-112">Para demostrar la actualización, comenzaremos mediante la creación de una aplicación de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c81b6-112">To demonstrate the upgrade, we'll start by creating a ASP.NET MVC app.</span></span> <span data-ttu-id="c81b6-113">Crear con el nombre *WebApp1* para el espacio de nombres coincidirá con el proyecto de ASP.NET Core que creamos en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="c81b6-113">Create it with the name *WebApp1* so the namespace will match the ASP.NET Core project we create in the next step.</span></span>

![El cuadro de diálogo de Visual Studio nuevo proyecto](mvc/_static/new-project.png)

![Cuadro de diálogo nueva aplicación Web: plantilla de proyecto MVC seleccionado en el panel de plantillas ASP.NET](mvc/_static/new-project-select-mvc-template.png)

<span data-ttu-id="c81b6-116">*Opcional:* cambiar el nombre de la solución de *WebApp1* a *Mvc5*.</span><span class="sxs-lookup"><span data-stu-id="c81b6-116">*Optional:* Change the name of the Solution from *WebApp1* to *Mvc5*.</span></span> <span data-ttu-id="c81b6-117">Visual Studio mostrará el nuevo nombre de la solución (*Mvc5*), que resultará más fácil indicar a este proyecto desde el proyecto siguiente.</span><span class="sxs-lookup"><span data-stu-id="c81b6-117">Visual Studio will display the new solution name (*Mvc5*), which will make it easier to tell this project from the next project.</span></span>

## <a name="create-the-aspnet-core-project"></a><span data-ttu-id="c81b6-118">Crear el proyecto de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c81b6-118">Create the ASP.NET Core project</span></span>

<span data-ttu-id="c81b6-119">Crear un nuevo *vacía* aplicación web de ASP.NET Core con el mismo nombre que el proyecto anterior (*WebApp1*) para que coincidan con los espacios de nombres en los dos proyectos.</span><span class="sxs-lookup"><span data-stu-id="c81b6-119">Create a new *empty* ASP.NET Core web app with the same name as the previous project (*WebApp1*) so the namespaces in the two projects match.</span></span> <span data-ttu-id="c81b6-120">Tener el mismo espacio de nombres resulta más fácil copiar el código entre los dos proyectos.</span><span class="sxs-lookup"><span data-stu-id="c81b6-120">Having the same namespace makes it easier to copy code between the two projects.</span></span> <span data-ttu-id="c81b6-121">Tendrá que crear este proyecto en un directorio diferente que el proyecto anterior para utilizar el mismo nombre.</span><span class="sxs-lookup"><span data-stu-id="c81b6-121">You'll have to create this project in a different directory than the previous project to use the same name.</span></span>

![Cuadro de diálogo Nuevo proyecto](mvc/_static/new_core.png)

![Cuadro de diálogo nueva aplicación Web de ASP.NET: plantilla de proyecto vacía seleccionada en el panel de plantillas de núcleo de ASP.NET](mvc/_static/new-project-select-empty-aspnet5-template.png)

* <span data-ttu-id="c81b6-124">*Opcional:* crear una nueva aplicación ASP.NET Core usando la *aplicación Web* plantilla de proyecto.</span><span class="sxs-lookup"><span data-stu-id="c81b6-124">*Optional:* Create a new ASP.NET Core app using the *Web Application* project template.</span></span> <span data-ttu-id="c81b6-125">Denomine el proyecto *WebApp1*y seleccione una opción de autenticación de **cuentas de usuario individuales**.</span><span class="sxs-lookup"><span data-stu-id="c81b6-125">Name the project *WebApp1*, and select an authentication option of **Individual User Accounts**.</span></span> <span data-ttu-id="c81b6-126">Cambiar el nombre de esta aplicación *FullAspNetCore*.</span><span class="sxs-lookup"><span data-stu-id="c81b6-126">Rename this app to *FullAspNetCore*.</span></span> <span data-ttu-id="c81b6-127">Creación de este proyecto, ahorrará tiempo en la conversión.</span><span class="sxs-lookup"><span data-stu-id="c81b6-127">Creating this project will save you time in the conversion.</span></span> <span data-ttu-id="c81b6-128">También puede buscar en el código generado de plantilla para ver el resultado final o copiar el código al proyecto de conversión.</span><span class="sxs-lookup"><span data-stu-id="c81b6-128">You can look at the template-generated code to see the end result or to copy code to the conversion project.</span></span> <span data-ttu-id="c81b6-129">También es útil cuando bloquea en un paso de la conversión que se compara con el proyecto de plantilla generado.</span><span class="sxs-lookup"><span data-stu-id="c81b6-129">It's also helpful when you get stuck on a conversion step to compare with the template-generated project.</span></span>

## <a name="configure-the-site-to-use-mvc"></a><span data-ttu-id="c81b6-130">Configurar el sitio para usar MVC</span><span class="sxs-lookup"><span data-stu-id="c81b6-130">Configure the site to use MVC</span></span>

* <span data-ttu-id="c81b6-131">Instalar el `Microsoft.AspNetCore.Mvc` y `Microsoft.AspNetCore.StaticFiles` paquetes de NuGet.</span><span class="sxs-lookup"><span data-stu-id="c81b6-131">Install the `Microsoft.AspNetCore.Mvc` and `Microsoft.AspNetCore.StaticFiles` NuGet packages.</span></span>

  <span data-ttu-id="c81b6-132">`Microsoft.AspNetCore.Mvc` es el marco de MVC de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c81b6-132">`Microsoft.AspNetCore.Mvc` is the ASP.NET Core MVC framework.</span></span> <span data-ttu-id="c81b6-133">`Microsoft.AspNetCore.StaticFiles` es el controlador de archivos estáticos.</span><span class="sxs-lookup"><span data-stu-id="c81b6-133">`Microsoft.AspNetCore.StaticFiles` is the static file handler.</span></span> <span data-ttu-id="c81b6-134">El tiempo de ejecución ASP.NET es modular y explícitamente debe participar en servir archivos estáticos (vea [trabajar con archivos estáticos](../fundamentals/static-files.md)).</span><span class="sxs-lookup"><span data-stu-id="c81b6-134">The ASP.NET runtime is modular, and you must explicitly opt in to serve static files (see [Working with Static Files](../fundamentals/static-files.md)).</span></span>

* <span data-ttu-id="c81b6-135">Abra la *.csproj* archivo (haga clic en el proyecto en **el Explorador de soluciones** y seleccione **editar WebApp1.csproj**) y agregue un `PrepareForPublish` destino:</span><span class="sxs-lookup"><span data-stu-id="c81b6-135">Open the *.csproj* file (right-click the project in **Solution Explorer** and select **Edit WebApp1.csproj**) and add a `PrepareForPublish` target:</span></span>

  [!code-xml[](mvc/sample/WebApp1.csproj?range=21-23)]

  <span data-ttu-id="c81b6-136">El `PrepareForPublish` destino es necesario para adquirir las bibliotecas de cliente a través de Bower.</span><span class="sxs-lookup"><span data-stu-id="c81b6-136">The `PrepareForPublish` target is needed for acquiring client-side libraries via Bower.</span></span> <span data-ttu-id="c81b6-137">Hablaremos sobre el más adelante.</span><span class="sxs-lookup"><span data-stu-id="c81b6-137">We'll talk about that later.</span></span>

* <span data-ttu-id="c81b6-138">Abra la *Startup.cs* y cambie el código para que coincida con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="c81b6-138">Open the *Startup.cs* file and change the code to match the following:</span></span>

  [!code-csharp[](mvc/sample/Startup.cs?highlight=14,27-34)]

  <span data-ttu-id="c81b6-139">El `UseStaticFiles` método de extensión agrega el controlador de archivos estáticos.</span><span class="sxs-lookup"><span data-stu-id="c81b6-139">The `UseStaticFiles` extension method adds the static file handler.</span></span> <span data-ttu-id="c81b6-140">Como se mencionó anteriormente, el tiempo de ejecución ASP.NET es modular y explícitamente debe participar en servir archivos estáticos.</span><span class="sxs-lookup"><span data-stu-id="c81b6-140">As mentioned previously, the ASP.NET runtime is modular, and you must explicitly opt in to serve static files.</span></span> <span data-ttu-id="c81b6-141">El `UseMvc` método de extensión agrega enrutamiento.</span><span class="sxs-lookup"><span data-stu-id="c81b6-141">The `UseMvc` extension method adds routing.</span></span> <span data-ttu-id="c81b6-142">Para obtener más información, consulte [inicio de la aplicación](../fundamentals/startup.md) y [enrutamiento](../fundamentals/routing.md).</span><span class="sxs-lookup"><span data-stu-id="c81b6-142">For more information, see [Application Startup](../fundamentals/startup.md) and [Routing](../fundamentals/routing.md).</span></span>

## <a name="add-a-controller-and-view"></a><span data-ttu-id="c81b6-143">Agregar un controlador y una vista</span><span class="sxs-lookup"><span data-stu-id="c81b6-143">Add a controller and view</span></span>

<span data-ttu-id="c81b6-144">En esta sección, agregará un controlador mínima y la vista para que actúe como marcadores de posición para el controlador de MVC de ASP.NET y las vistas a migrar en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="c81b6-144">In this section, you'll add a minimal controller and view to serve as placeholders for the ASP.NET MVC controller and views you'll migrate in the next section.</span></span>

* <span data-ttu-id="c81b6-145">Agregar un *controladores* carpeta.</span><span class="sxs-lookup"><span data-stu-id="c81b6-145">Add a *Controllers* folder.</span></span>

* <span data-ttu-id="c81b6-146">Agregar un **clase de controlador MVC** con el nombre *HomeController.cs* a la *controladores* carpeta.</span><span class="sxs-lookup"><span data-stu-id="c81b6-146">Add an **MVC controller class** with the name *HomeController.cs* to the *Controllers* folder.</span></span>

![Cuadro de diálogo Agregar nuevo elemento](mvc/_static/add_mvc_ctl.png)

* <span data-ttu-id="c81b6-148">Agregar un *vistas* carpeta.</span><span class="sxs-lookup"><span data-stu-id="c81b6-148">Add a *Views* folder.</span></span>

* <span data-ttu-id="c81b6-149">Agregar un *vistas/inicio* carpeta.</span><span class="sxs-lookup"><span data-stu-id="c81b6-149">Add a *Views/Home* folder.</span></span>

* <span data-ttu-id="c81b6-150">Agregar un *Index.cshtml* página de vista de MVC a la *vistas/inicio* carpeta.</span><span class="sxs-lookup"><span data-stu-id="c81b6-150">Add an *Index.cshtml* MVC view page to the *Views/Home* folder.</span></span>

![Cuadro de diálogo Agregar nuevo elemento](mvc/_static/view.png)

<span data-ttu-id="c81b6-152">A continuación se muestra la estructura del proyecto:</span><span class="sxs-lookup"><span data-stu-id="c81b6-152">The project structure is shown below:</span></span>

![Explorador de soluciones con archivos y carpetas de WebApp1](mvc/_static/project-structure-controller-view.png)

<span data-ttu-id="c81b6-154">Reemplace el contenido de la *Views/Home/Index.cshtml* archivo con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="c81b6-154">Replace the contents of the *Views/Home/Index.cshtml* file with the following:</span></span>

```html
<h1>Hello world!</h1>
```

<span data-ttu-id="c81b6-155">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c81b6-155">Run the app.</span></span>

![Aplicación Web abierta en Microsoft Edge](mvc/_static/hello-world.png)

<span data-ttu-id="c81b6-157">Vea [controladores](xref:mvc/controllers/actions) y [vistas](xref:mvc/views/overview) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="c81b6-157">See [Controllers](xref:mvc/controllers/actions) and [Views](xref:mvc/views/overview) for more information.</span></span>

<span data-ttu-id="c81b6-158">Ahora que tenemos un proyecto ASP.NET Core trabajo mínimo, se podemos empezar a migrar la funcionalidad desde el proyecto de MVC de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c81b6-158">Now that we have a minimal working ASP.NET Core project, we can start migrating functionality from the ASP.NET MVC project.</span></span> <span data-ttu-id="c81b6-159">Tendrá que mover lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="c81b6-159">We will need to move the following:</span></span>

* <span data-ttu-id="c81b6-160">contenido del lado cliente (CSS, fuentes y secuencias de comandos)</span><span class="sxs-lookup"><span data-stu-id="c81b6-160">client-side content (CSS, fonts, and scripts)</span></span>

* <span data-ttu-id="c81b6-161">controladores</span><span class="sxs-lookup"><span data-stu-id="c81b6-161">controllers</span></span>

* <span data-ttu-id="c81b6-162">vistas</span><span class="sxs-lookup"><span data-stu-id="c81b6-162">views</span></span>

* <span data-ttu-id="c81b6-163">modelos</span><span class="sxs-lookup"><span data-stu-id="c81b6-163">models</span></span>

* <span data-ttu-id="c81b6-164">Cómo agrupar</span><span class="sxs-lookup"><span data-stu-id="c81b6-164">bundling</span></span>

* <span data-ttu-id="c81b6-165">filtros</span><span class="sxs-lookup"><span data-stu-id="c81b6-165">filters</span></span>

* <span data-ttu-id="c81b6-166">Registro de entrada/salida, la identidad (Esto se hace en el tutorial siguiente).</span><span class="sxs-lookup"><span data-stu-id="c81b6-166">Log in/out, identity (This will be done in the next tutorial.)</span></span>

## <a name="controllers-and-views"></a><span data-ttu-id="c81b6-167">Controladores y vistas</span><span class="sxs-lookup"><span data-stu-id="c81b6-167">Controllers and views</span></span>

* <span data-ttu-id="c81b6-168">Copiar cada uno de los métodos de ASP.NET MVC `HomeController` al nuevo `HomeController`.</span><span class="sxs-lookup"><span data-stu-id="c81b6-168">Copy each of the methods from the ASP.NET MVC `HomeController` to the new `HomeController`.</span></span> <span data-ttu-id="c81b6-169">Tenga en cuenta que en ASP.NET MVC, tipo de valor devuelto de método de acción de controlador de la plantilla integrada [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); en el núcleo de ASP.NET MVC, los métodos de acción devueltos `IActionResult` en su lugar.</span><span class="sxs-lookup"><span data-stu-id="c81b6-169">Note that in ASP.NET MVC, the built-in template's controller action method return type is [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); in ASP.NET Core MVC, the action methods return `IActionResult` instead.</span></span> <span data-ttu-id="c81b6-170">`ActionResult` implementa `IActionResult`, por lo que no es necesario para cambiar el tipo de valor devuelto de los métodos de acción.</span><span class="sxs-lookup"><span data-stu-id="c81b6-170">`ActionResult` implements `IActionResult`, so there's no need to change the return type of your action methods.</span></span>

* <span data-ttu-id="c81b6-171">Copia la *About.cshtml*, *Contact.cshtml*, y *Index.cshtml* archivos de vista Razor desde el proyecto de MVC de ASP.NET para el proyecto de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c81b6-171">Copy the *About.cshtml*, *Contact.cshtml*, and *Index.cshtml* Razor view files from the ASP.NET MVC project to the ASP.NET Core project.</span></span>

* <span data-ttu-id="c81b6-172">Ejecutar la aplicación de ASP.NET Core y cada método de prueba.</span><span class="sxs-lookup"><span data-stu-id="c81b6-172">Run the ASP.NET Core app and test each method.</span></span> <span data-ttu-id="c81b6-173">No hemos migramos los estilos o el archivo de diseño, por lo que las vistas representadas solo contendrá el contenido de los archivos de vista.</span><span class="sxs-lookup"><span data-stu-id="c81b6-173">We haven't migrated the layout file or styles yet, so the rendered views will only contain the content in the view files.</span></span> <span data-ttu-id="c81b6-174">No tendrá los vínculos de archivo generado de diseño para la `About` y `Contact` vistas, por lo que tendrá que invocar a los mismos desde el explorador (reemplace **4492** con el número de puerto utilizado en el proyecto).</span><span class="sxs-lookup"><span data-stu-id="c81b6-174">You won't have the layout file generated links for the `About` and `Contact` views, so you'll have to invoke them from the browser (replace **4492** with the port number used in your project).</span></span>

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![Página contacto](mvc/_static/contact-page.png)

<span data-ttu-id="c81b6-176">Tenga en cuenta la falta de un estilo y elementos de menú.</span><span class="sxs-lookup"><span data-stu-id="c81b6-176">Note the lack of styling and menu items.</span></span> <span data-ttu-id="c81b6-177">Esto lo corregiremos en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="c81b6-177">We'll fix that in the next section.</span></span>

## <a name="static-content"></a><span data-ttu-id="c81b6-178">Contenido estático</span><span class="sxs-lookup"><span data-stu-id="c81b6-178">Static content</span></span>

<span data-ttu-id="c81b6-179">En versiones anteriores de ASP.NET MVC, contenido estático se alojó desde la raíz del proyecto web y se ha combinado con archivos de servidor.</span><span class="sxs-lookup"><span data-stu-id="c81b6-179">In previous versions of  ASP.NET MVC, static content was hosted from the root of the web project and was intermixed with server-side files.</span></span> <span data-ttu-id="c81b6-180">En el núcleo de ASP.NET, contenido estático se hospeda en el *wwwroot* carpeta.</span><span class="sxs-lookup"><span data-stu-id="c81b6-180">In ASP.NET Core, static content is hosted in the *wwwroot* folder.</span></span> <span data-ttu-id="c81b6-181">Desea copiar el contenido estático de la aplicación de MVC de ASP.NET anterior a la *wwwroot* carpeta en el proyecto de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c81b6-181">You'll want to copy the static content from your old ASP.NET MVC app to the *wwwroot* folder in your ASP.NET Core project.</span></span> <span data-ttu-id="c81b6-182">En esta conversión de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="c81b6-182">In this sample conversion:</span></span>

* <span data-ttu-id="c81b6-183">Copia la *favicon.ico* archivo desde el proyecto MVC anterior a la *wwwroot* carpeta del proyecto de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c81b6-183">Copy the *favicon.ico* file from the old MVC project to the *wwwroot* folder in the ASP.NET Core project.</span></span>

<span data-ttu-id="c81b6-184">La antigua ASP.NET MVC project utiliza [Bootstrap](http://getbootstrap.com/) para su aplicación de estilos y almacena el arranque de los archivos del *contenido* y *Scripts* carpetas.</span><span class="sxs-lookup"><span data-stu-id="c81b6-184">The old ASP.NET MVC project uses [Bootstrap](http://getbootstrap.com/) for its styling and stores the Bootstrap files in the *Content* and *Scripts* folders.</span></span> <span data-ttu-id="c81b6-185">Hace referencia a la plantilla, que genera el proyecto de MVC de ASP.NET anterior, arranque en el archivo de diseño (*Views/Shared/_Layout.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="c81b6-185">The template, which generated the old ASP.NET MVC project, references Bootstrap in the layout file (*Views/Shared/_Layout.cshtml*).</span></span> <span data-ttu-id="c81b6-186">Podría copiar el *bootstrap.js* y *bootstrap.css* proyecto de archivos de ASP.NET MVC a la *wwwroot* carpeta en el nuevo proyecto, pero ese método no usa el mecanismo mejorado para administrar las dependencias del lado cliente en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c81b6-186">You could copy the *bootstrap.js* and *bootstrap.css* files from the ASP.NET MVC project to the *wwwroot* folder in the new project, but that approach doesn't use the improved mechanism for managing client-side dependencies in ASP.NET Core.</span></span>

<span data-ttu-id="c81b6-187">En el nuevo proyecto, vamos a agregar compatibilidad con arranque (y otras bibliotecas de cliente) con [Bower](https://bower.io/):</span><span class="sxs-lookup"><span data-stu-id="c81b6-187">In the new project, we'll add support for Bootstrap (and other client-side libraries) using [Bower](https://bower.io/):</span></span>

* <span data-ttu-id="c81b6-188">Agregar un [Bower](https://bower.io/) archivo de configuración denominado *bower.json* a la raíz del proyecto (haga doble clic en el proyecto y, a continuación, **Agregar > nuevo elemento > archivo de configuración de Bower**).</span><span class="sxs-lookup"><span data-stu-id="c81b6-188">Add a [Bower](https://bower.io/) configuration file named *bower.json* to the project root (Right-click on the project, and then **Add > New Item > Bower Configuration File**).</span></span> <span data-ttu-id="c81b6-189">Agregar [arranque](http://getbootstrap.com/) y [jQuery](https://jquery.com/) al archivo (vea las líneas resaltadas a continuación).</span><span class="sxs-lookup"><span data-stu-id="c81b6-189">Add [Bootstrap](http://getbootstrap.com/) and [jQuery](https://jquery.com/) to the file (see the highlighted lines below).</span></span>

  [!code-json[](mvc/sample/bower.json?highlight=5-6)]

<span data-ttu-id="c81b6-190">Al guardar el archivo, Bower descargará automáticamente las dependencias para el *wwwroot/lib* carpeta.</span><span class="sxs-lookup"><span data-stu-id="c81b6-190">Upon saving the file, Bower will automatically download the dependencies to the *wwwroot/lib* folder.</span></span> <span data-ttu-id="c81b6-191">Puede usar el **el Explorador de soluciones de búsqueda** cuadro para buscar la ruta de acceso de los activos:</span><span class="sxs-lookup"><span data-stu-id="c81b6-191">You can use the **Search Solution Explorer** box to find the path of the assets:</span></span>

![activos de jQuery que se muestra en los resultados de búsqueda del explorador de soluciones](mvc/_static/search.png)

<span data-ttu-id="c81b6-193">Vea [administrar paquetes de cliente con Bower](../client-side/bower.md) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="c81b6-193">See [Manage Client-Side Packages with Bower](../client-side/bower.md) for more information.</span></span>

<a name="migrate-layout-file"></a>

## <a name="migrate-the-layout-file"></a><span data-ttu-id="c81b6-194">Migrar el archivo de diseño</span><span class="sxs-lookup"><span data-stu-id="c81b6-194">Migrate the layout file</span></span>

* <span data-ttu-id="c81b6-195">Copia la *_ViewStart.cshtml* archivo desde el proyecto de MVC de ASP.NET anterior *vistas* carpeta en el proyecto de ASP.NET Core *vistas* carpeta.</span><span class="sxs-lookup"><span data-stu-id="c81b6-195">Copy the *_ViewStart.cshtml* file from the old ASP.NET MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="c81b6-196">El *_ViewStart.cshtml* archivo no ha cambiado en MVC de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c81b6-196">The *_ViewStart.cshtml* file has not changed in ASP.NET Core MVC.</span></span>

* <span data-ttu-id="c81b6-197">Crear un *vistas/compartidas* carpeta.</span><span class="sxs-lookup"><span data-stu-id="c81b6-197">Create a *Views/Shared* folder.</span></span>

* <span data-ttu-id="c81b6-198">*Opcional:* copia *_ViewImports.cshtml* desde el *FullAspNetCore* del proyecto MVC *vistas* carpeta en el proyecto de ASP.NET Core  *Vistas* carpeta.</span><span class="sxs-lookup"><span data-stu-id="c81b6-198">*Optional:* Copy *_ViewImports.cshtml* from the *FullAspNetCore* MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="c81b6-199">Quite las declaraciones de espacio de nombres en el *_ViewImports.cshtml* archivo.</span><span class="sxs-lookup"><span data-stu-id="c81b6-199">Remove any namespace declaration in the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="c81b6-200">El *_ViewImports.cshtml* archivo proporciona los espacios de nombres para todos los archivos de vista y la pone [aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="c81b6-200">The *_ViewImports.cshtml* file provides namespaces for all the view files and brings in [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="c81b6-201">Aplicaciones auxiliares de etiquetas se usan en el nuevo archivo de diseño.</span><span class="sxs-lookup"><span data-stu-id="c81b6-201">Tag Helpers are used in the new layout file.</span></span> <span data-ttu-id="c81b6-202">El *_ViewImports.cshtml* archivo es una novedad de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c81b6-202">The *_ViewImports.cshtml* file is new for ASP.NET Core.</span></span>

* <span data-ttu-id="c81b6-203">Copia la *_Layout.cshtml* archivo desde el proyecto de MVC de ASP.NET anterior *vistas/compartidas* carpeta en el proyecto de ASP.NET Core *vistas/compartidas* carpeta.</span><span class="sxs-lookup"><span data-stu-id="c81b6-203">Copy the *_Layout.cshtml* file from the old ASP.NET MVC project's *Views/Shared* folder into the ASP.NET Core project's *Views/Shared* folder.</span></span>

<span data-ttu-id="c81b6-204">Abra *_Layout.cshtml* de archivos y realice los cambios siguientes (el código completo se muestra a continuación):</span><span class="sxs-lookup"><span data-stu-id="c81b6-204">Open *_Layout.cshtml* file and make the following changes (the completed code is shown below):</span></span>

   * <span data-ttu-id="c81b6-205">Reemplace `@Styles.Render("~/Content/css")` con un `<link>` elemento que se va a cargar *bootstrap.css* (ver abajo).</span><span class="sxs-lookup"><span data-stu-id="c81b6-205">Replace `@Styles.Render("~/Content/css")` with a `<link>` element to load *bootstrap.css* (see below).</span></span>

   * <span data-ttu-id="c81b6-206">Quite `@Scripts.Render("~/bundles/modernizr")`.</span><span class="sxs-lookup"><span data-stu-id="c81b6-206">Remove `@Scripts.Render("~/bundles/modernizr")`.</span></span>

   * <span data-ttu-id="c81b6-207">Convierta en comentario la `@Html.Partial("_LoginPartial")` línea (incluya la línea con `@*...*@`).</span><span class="sxs-lookup"><span data-stu-id="c81b6-207">Comment out the `@Html.Partial("_LoginPartial")` line (surround the line with `@*...*@`).</span></span> <span data-ttu-id="c81b6-208">Se tendrá que volver a él en un tutorial posterior.</span><span class="sxs-lookup"><span data-stu-id="c81b6-208">We'll return to it in a future tutorial.</span></span>

   * <span data-ttu-id="c81b6-209">Reemplace `@Scripts.Render("~/bundles/jquery")` con un `<script>` elemento (ver abajo).</span><span class="sxs-lookup"><span data-stu-id="c81b6-209">Replace `@Scripts.Render("~/bundles/jquery")` with a `<script>` element (see below).</span></span>

   * <span data-ttu-id="c81b6-210">Reemplace `@Scripts.Render("~/bundles/bootstrap")` con un `<script>` elemento (ver abajo)...</span><span class="sxs-lookup"><span data-stu-id="c81b6-210">Replace `@Scripts.Render("~/bundles/bootstrap")` with a `<script>` element (see below)..</span></span>

<span data-ttu-id="c81b6-211">El vínculo CSS de reemplazo:</span><span class="sxs-lookup"><span data-stu-id="c81b6-211">The replacement CSS link:</span></span>

```html
<link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.css" />
```

<span data-ttu-id="c81b6-212">Las etiquetas de script de reemplazo:</span><span class="sxs-lookup"><span data-stu-id="c81b6-212">The replacement script tags:</span></span>

```html
<script src="~/lib/jquery/dist/jquery.js"></script>
<script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
```

<span data-ttu-id="c81b6-213">La actualización *_Layout.cshtml* archivo se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="c81b6-213">The updated *_Layout.cshtml* file is shown below:</span></span>

[!code-html[](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7,27,39-40)]

<span data-ttu-id="c81b6-214">Ver el sitio en el explorador.</span><span class="sxs-lookup"><span data-stu-id="c81b6-214">View the site in the browser.</span></span> <span data-ttu-id="c81b6-215">Ahora deberían cargarse correctamente, con los estilos esperados en su lugar.</span><span class="sxs-lookup"><span data-stu-id="c81b6-215">It should now load correctly, with the expected styles in place.</span></span>

* <span data-ttu-id="c81b6-216">*Opcional:* desea intentar usar el nuevo archivo de diseño.</span><span class="sxs-lookup"><span data-stu-id="c81b6-216">*Optional:* You might want to try using the new layout file.</span></span> <span data-ttu-id="c81b6-217">Para este proyecto se puede copiar el archivo de diseño de la *FullAspNetCore* proyecto.</span><span class="sxs-lookup"><span data-stu-id="c81b6-217">For this project you can copy the layout file from the *FullAspNetCore* project.</span></span> <span data-ttu-id="c81b6-218">El nuevo archivo de diseño usa [aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro) y tiene otras mejoras.</span><span class="sxs-lookup"><span data-stu-id="c81b6-218">The new layout file uses [Tag Helpers](xref:mvc/views/tag-helpers/intro) and has other improvements.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="c81b6-219">Configurar la agrupación y minificación</span><span class="sxs-lookup"><span data-stu-id="c81b6-219">Configure bundling and minification</span></span>

<span data-ttu-id="c81b6-220">Para obtener información sobre cómo configurar la agrupación y minificación, consulte [unión y Minificación](../client-side/bundling-and-minification.md).</span><span class="sxs-lookup"><span data-stu-id="c81b6-220">For information about how to configure bundling and minification, see [Bundling and Minification](../client-side/bundling-and-minification.md).</span></span>

## <a name="solving-http-500-errors"></a><span data-ttu-id="c81b6-221">Para resolver errores de HTTP 500</span><span class="sxs-lookup"><span data-stu-id="c81b6-221">Solving HTTP 500 errors</span></span>

<span data-ttu-id="c81b6-222">Hay muchos problemas que pueden provocar un mensaje de error HTTP 500 que no contienen ninguna información en el origen del problema.</span><span class="sxs-lookup"><span data-stu-id="c81b6-222">There are many problems that can cause a HTTP 500 error message that contain no information on the source of the problem.</span></span> <span data-ttu-id="c81b6-223">Por ejemplo, si la *Views/_ViewImports.cshtml* archivo contiene un espacio de nombres que no existe en el proyecto, obtendrá un error HTTP 500.</span><span class="sxs-lookup"><span data-stu-id="c81b6-223">For example, if the *Views/_ViewImports.cshtml* file contains a namespace that doesn't exist in your project, you'll get a HTTP 500 error.</span></span> <span data-ttu-id="c81b6-224">Para obtener un mensaje de error detallado, agregue el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="c81b6-224">To get a detailed error message, add the following code:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    if (env.IsDevelopment())
    {
         app.UseDeveloperExceptionPage();
    }

    app.UseStaticFiles();

    app.UseMvc(routes =>
    {
        routes.MapRoute(
            name: "default",
            template: "{controller=Home}/{action=Index}/{id?}");
    });
}
```

<span data-ttu-id="c81b6-225">Vea **mediante la página de excepción para desarrolladores** en [control de errores](../fundamentals/error-handling.md) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="c81b6-225">See **Using the Developer Exception Page** in [Error Handling](../fundamentals/error-handling.md) for more information.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c81b6-226">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="c81b6-226">Additional resources</span></span>

* [<span data-ttu-id="c81b6-227">Desarrollo en el cliente</span><span class="sxs-lookup"><span data-stu-id="c81b6-227">Client-Side Development</span></span>](xref:client-side/index)
* [<span data-ttu-id="c81b6-228">Aplicaciones auxiliares de etiquetas</span><span class="sxs-lookup"><span data-stu-id="c81b6-228">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
