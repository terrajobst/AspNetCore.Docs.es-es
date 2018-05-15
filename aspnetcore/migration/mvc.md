---
title: Migrar de MVC de ASP.NET a ASP.NET Core MVC
author: ardalis
description: Obtenga información acerca de cómo empezar a migrar un proyecto de MVC de ASP.NET a ASP.NET MVC de núcleo.
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/mvc
ms.openlocfilehash: b8c913c0a6f47a1c993d508f9baae54981327957
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2018
---
# <a name="migrate-from-aspnet-mvc-to-aspnet-core-mvc"></a><span data-ttu-id="e7803-103">Migrar de MVC de ASP.NET a ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="e7803-103">Migrate from ASP.NET MVC to ASP.NET Core MVC</span></span>

<span data-ttu-id="e7803-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), y [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="e7803-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="e7803-105">Este artículo muestra cómo empezar a migrar un proyecto de MVC de ASP.NET a [MVC de ASP.NET Core](../mvc/overview.md).</span><span class="sxs-lookup"><span data-stu-id="e7803-105">This article shows how to get started migrating an ASP.NET MVC project to [ASP.NET Core MVC](../mvc/overview.md).</span></span> <span data-ttu-id="e7803-106">En el proceso, resalta muchas de las cosas que han cambiado respecto a ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e7803-106">In the process, it highlights many of the things that have changed from ASP.NET MVC.</span></span> <span data-ttu-id="e7803-107">La migración de ASP.NET MVC es un proceso en varias fases y este artículo trata la instalación inicial, controladores básicos y vistas, contenido estático y las dependencias del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="e7803-107">Migrating from ASP.NET MVC is a multiple step process and this article covers the initial setup, basic controllers and views, static content, and client-side dependencies.</span></span> <span data-ttu-id="e7803-108">Migrar la configuración y el código de identidad que se encuentra en muchos proyectos de ASP.NET MVC tratan otros artículos.</span><span class="sxs-lookup"><span data-stu-id="e7803-108">Additional articles cover migrating configuration and identity code found in many ASP.NET MVC projects.</span></span>

> [!NOTE]
> <span data-ttu-id="e7803-109">Los números de versión en los ejemplos podrían no estar actualizados.</span><span class="sxs-lookup"><span data-stu-id="e7803-109">The version numbers in the samples might not be current.</span></span> <span data-ttu-id="e7803-110">Debe actualizar los proyectos en consecuencia.</span><span class="sxs-lookup"><span data-stu-id="e7803-110">You may need to update your projects accordingly.</span></span>

## <a name="create-the-starter-aspnet-mvc-project"></a><span data-ttu-id="e7803-111">Crear el proyecto de MVC de ASP.NET de inicio</span><span class="sxs-lookup"><span data-stu-id="e7803-111">Create the starter ASP.NET MVC project</span></span>

<span data-ttu-id="e7803-112">Para demostrar la actualización, comenzaremos mediante la creación de una aplicación de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e7803-112">To demonstrate the upgrade, we'll start by creating a ASP.NET MVC app.</span></span> <span data-ttu-id="e7803-113">Crear con el nombre *WebApp1* para el espacio de nombres coincida con el proyecto de ASP.NET Core que creamos en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="e7803-113">Create it with the name *WebApp1* so the namespace matches the ASP.NET Core project we create in the next step.</span></span>

![El cuadro de diálogo de Visual Studio nuevo proyecto](mvc/_static/new-project.png)

![Cuadro de diálogo nueva aplicación Web: plantilla de proyecto MVC seleccionado en el panel de plantillas ASP.NET](mvc/_static/new-project-select-mvc-template.png)

<span data-ttu-id="e7803-116">*Opcional:* cambiar el nombre de la solución de *WebApp1* a *Mvc5*.</span><span class="sxs-lookup"><span data-stu-id="e7803-116">*Optional:* Change the name of the Solution from *WebApp1* to *Mvc5*.</span></span> <span data-ttu-id="e7803-117">Visual Studio muestra el nuevo nombre de la solución (*Mvc5*), lo que facilita indicar este proyecto desde el proyecto siguiente.</span><span class="sxs-lookup"><span data-stu-id="e7803-117">Visual Studio displays the new solution name (*Mvc5*), which makes it easier to tell this project from the next project.</span></span>

## <a name="create-the-aspnet-core-project"></a><span data-ttu-id="e7803-118">Crear el proyecto de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e7803-118">Create the ASP.NET Core project</span></span>

<span data-ttu-id="e7803-119">Crear un nuevo *vacía* aplicación web de ASP.NET Core con el mismo nombre que el proyecto anterior (*WebApp1*) para que coincidan con los espacios de nombres en los dos proyectos.</span><span class="sxs-lookup"><span data-stu-id="e7803-119">Create a new *empty* ASP.NET Core web app with the same name as the previous project (*WebApp1*) so the namespaces in the two projects match.</span></span> <span data-ttu-id="e7803-120">Tener el mismo espacio de nombres resulta más fácil copiar el código entre los dos proyectos.</span><span class="sxs-lookup"><span data-stu-id="e7803-120">Having the same namespace makes it easier to copy code between the two projects.</span></span> <span data-ttu-id="e7803-121">Tendrá que crear este proyecto en un directorio diferente que el proyecto anterior para utilizar el mismo nombre.</span><span class="sxs-lookup"><span data-stu-id="e7803-121">You'll have to create this project in a different directory than the previous project to use the same name.</span></span>

![Cuadro de diálogo Nuevo proyecto](mvc/_static/new_core.png)

![Cuadro de diálogo nueva aplicación Web de ASP.NET: plantilla de proyecto vacía seleccionada en el panel de plantillas de núcleo de ASP.NET](mvc/_static/new-project-select-empty-aspnet5-template.png)

* <span data-ttu-id="e7803-124">*Opcional:* crear una nueva aplicación ASP.NET Core usando la *aplicación Web* plantilla de proyecto.</span><span class="sxs-lookup"><span data-stu-id="e7803-124">*Optional:* Create a new ASP.NET Core app using the *Web Application* project template.</span></span> <span data-ttu-id="e7803-125">Denomine el proyecto *WebApp1*y seleccione una opción de autenticación de **cuentas de usuario individuales**.</span><span class="sxs-lookup"><span data-stu-id="e7803-125">Name the project *WebApp1*, and select an authentication option of **Individual User Accounts**.</span></span> <span data-ttu-id="e7803-126">Cambiar el nombre de esta aplicación *FullAspNetCore*.</span><span class="sxs-lookup"><span data-stu-id="e7803-126">Rename this app to *FullAspNetCore*.</span></span> <span data-ttu-id="e7803-127">Creación de este proyecto le permite ahorrar tiempo en la conversión.</span><span class="sxs-lookup"><span data-stu-id="e7803-127">Creating this project saves you time in the conversion.</span></span> <span data-ttu-id="e7803-128">También puede buscar en el código generado de plantilla para ver el resultado final o copiar el código al proyecto de conversión.</span><span class="sxs-lookup"><span data-stu-id="e7803-128">You can look at the template-generated code to see the end result or to copy code to the conversion project.</span></span> <span data-ttu-id="e7803-129">También es útil cuando bloquea en un paso de la conversión que se compara con el proyecto de plantilla generado.</span><span class="sxs-lookup"><span data-stu-id="e7803-129">It's also helpful when you get stuck on a conversion step to compare with the template-generated project.</span></span>

## <a name="configure-the-site-to-use-mvc"></a><span data-ttu-id="e7803-130">Configurar el sitio para usar MVC</span><span class="sxs-lookup"><span data-stu-id="e7803-130">Configure the site to use MVC</span></span>

* <span data-ttu-id="e7803-131">Cuando el destino es .NET Core, el metapackage de ASP.NET Core se agrega al proyecto, denominado `Microsoft.AspNetCore.All` de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="e7803-131">When targeting .NET Core, the ASP.NET Core metapackage is added to the project, called `Microsoft.AspNetCore.All` by default.</span></span> <span data-ttu-id="e7803-132">Este paquete contiene paquetes como `Microsoft.AspNetCore.Mvc` y `Microsoft.AspNetCore.StaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="e7803-132">This package contains packages like `Microsoft.AspNetCore.Mvc` and `Microsoft.AspNetCore.StaticFiles`.</span></span> <span data-ttu-id="e7803-133">Si el destino es .NET Framework, las referencias del paquete tiene que mostrar individualmente en el archivo \*.csproj.</span><span class="sxs-lookup"><span data-stu-id="e7803-133">If targeting .NET Framework, package references need to be listed individually in the \*.csproj file.</span></span>

<span data-ttu-id="e7803-134">`Microsoft.AspNetCore.Mvc` es el marco de MVC de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e7803-134">`Microsoft.AspNetCore.Mvc` is the ASP.NET Core MVC framework.</span></span> <span data-ttu-id="e7803-135">`Microsoft.AspNetCore.StaticFiles` es el controlador de archivos estáticos.</span><span class="sxs-lookup"><span data-stu-id="e7803-135">`Microsoft.AspNetCore.StaticFiles` is the static file handler.</span></span> <span data-ttu-id="e7803-136">El tiempo de ejecución de ASP.NET Core es modular y explícitamente debe participar en servir archivos estáticos (vea [archivos estáticos](xref:fundamentals/static-files)).</span><span class="sxs-lookup"><span data-stu-id="e7803-136">The ASP.NET Core runtime is modular, and you must explicitly opt in to serve static files (see [Static files](xref:fundamentals/static-files)).</span></span>

* <span data-ttu-id="e7803-137">Abra la *Startup.cs* y cambie el código para que coincida con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="e7803-137">Open the *Startup.cs* file and change the code to match the following:</span></span>

  [!code-csharp[](mvc/sample/Startup.cs?highlight=13,26-31)]

<span data-ttu-id="e7803-138">El `UseStaticFiles` método de extensión agrega el controlador de archivos estáticos.</span><span class="sxs-lookup"><span data-stu-id="e7803-138">The `UseStaticFiles` extension method adds the static file handler.</span></span> <span data-ttu-id="e7803-139">Como se mencionó anteriormente, el tiempo de ejecución ASP.NET es modular y explícitamente debe participar en servir archivos estáticos.</span><span class="sxs-lookup"><span data-stu-id="e7803-139">As mentioned previously, the ASP.NET runtime is modular, and you must explicitly opt in to serve static files.</span></span> <span data-ttu-id="e7803-140">El `UseMvc` método de extensión agrega enrutamiento.</span><span class="sxs-lookup"><span data-stu-id="e7803-140">The `UseMvc` extension method adds routing.</span></span> <span data-ttu-id="e7803-141">Para obtener más información, consulte [inicio de la aplicación](xref:fundamentals/startup) y [enrutamiento](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="e7803-141">For more information, see [Application Startup](xref:fundamentals/startup) and [Routing](xref:fundamentals/routing).</span></span>

## <a name="add-a-controller-and-view"></a><span data-ttu-id="e7803-142">Agregar un controlador y una vista</span><span class="sxs-lookup"><span data-stu-id="e7803-142">Add a controller and view</span></span>

<span data-ttu-id="e7803-143">En esta sección, agregará un controlador mínima y la vista para que actúe como marcadores de posición para el controlador de MVC de ASP.NET y las vistas a migrar en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="e7803-143">In this section, you'll add a minimal controller and view to serve as placeholders for the ASP.NET MVC controller and views you'll migrate in the next section.</span></span>

* <span data-ttu-id="e7803-144">Agregar un *controladores* carpeta.</span><span class="sxs-lookup"><span data-stu-id="e7803-144">Add a *Controllers* folder.</span></span>

* <span data-ttu-id="e7803-145">Agregar un **clase de controlador** denominado *HomeController.cs* a la *controladores* carpeta.</span><span class="sxs-lookup"><span data-stu-id="e7803-145">Add a **Controller Class** named *HomeController.cs* to the *Controllers* folder.</span></span>

![Cuadro de diálogo Agregar nuevo elemento](mvc/_static/add_mvc_ctl.png)

* <span data-ttu-id="e7803-147">Agregar un *vistas* carpeta.</span><span class="sxs-lookup"><span data-stu-id="e7803-147">Add a *Views* folder.</span></span>

* <span data-ttu-id="e7803-148">Agregar un *vistas/inicio* carpeta.</span><span class="sxs-lookup"><span data-stu-id="e7803-148">Add a *Views/Home* folder.</span></span>

* <span data-ttu-id="e7803-149">Agregar un **vista Razor** denominado *Index.cshtml* a la *vistas/inicio* carpeta.</span><span class="sxs-lookup"><span data-stu-id="e7803-149">Add a **Razor View** named *Index.cshtml* to the *Views/Home* folder.</span></span>

![Cuadro de diálogo Agregar nuevo elemento](mvc/_static/view.png)

<span data-ttu-id="e7803-151">A continuación se muestra la estructura del proyecto:</span><span class="sxs-lookup"><span data-stu-id="e7803-151">The project structure is shown below:</span></span>

![Explorador de soluciones con archivos y carpetas de WebApp1](mvc/_static/project-structure-controller-view.png)

<span data-ttu-id="e7803-153">Reemplace el contenido de la *Views/Home/Index.cshtml* archivo con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="e7803-153">Replace the contents of the *Views/Home/Index.cshtml* file with the following:</span></span>

```html
<h1>Hello world!</h1>
```

<span data-ttu-id="e7803-154">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e7803-154">Run the app.</span></span>

![Aplicación Web abierta en Microsoft Edge](mvc/_static/hello-world.png)

<span data-ttu-id="e7803-156">Vea [controladores](xref:mvc/controllers/actions) y [vistas](xref:mvc/views/overview) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="e7803-156">See [Controllers](xref:mvc/controllers/actions) and [Views](xref:mvc/views/overview) for more information.</span></span>

<span data-ttu-id="e7803-157">Ahora que tenemos un proyecto ASP.NET Core trabajo mínimo, se podemos empezar a migrar la funcionalidad desde el proyecto de MVC de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e7803-157">Now that we have a minimal working ASP.NET Core project, we can start migrating functionality from the ASP.NET MVC project.</span></span> <span data-ttu-id="e7803-158">Es preciso mover lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="e7803-158">We need to move the following:</span></span>

* <span data-ttu-id="e7803-159">contenido del lado cliente (CSS, fuentes y secuencias de comandos)</span><span class="sxs-lookup"><span data-stu-id="e7803-159">client-side content (CSS, fonts, and scripts)</span></span>

* <span data-ttu-id="e7803-160">controladores</span><span class="sxs-lookup"><span data-stu-id="e7803-160">controllers</span></span>

* <span data-ttu-id="e7803-161">vistas</span><span class="sxs-lookup"><span data-stu-id="e7803-161">views</span></span>

* <span data-ttu-id="e7803-162">modelos</span><span class="sxs-lookup"><span data-stu-id="e7803-162">models</span></span>

* <span data-ttu-id="e7803-163">Cómo agrupar</span><span class="sxs-lookup"><span data-stu-id="e7803-163">bundling</span></span>

* <span data-ttu-id="e7803-164">filtros</span><span class="sxs-lookup"><span data-stu-id="e7803-164">filters</span></span>

* <span data-ttu-id="e7803-165">Registro de entrada/salida, la identidad (Esto se hace en el siguiente tutorial.)</span><span class="sxs-lookup"><span data-stu-id="e7803-165">Log in/out, Identity (This is done in the next tutorial.)</span></span>

## <a name="controllers-and-views"></a><span data-ttu-id="e7803-166">Controladores y vistas</span><span class="sxs-lookup"><span data-stu-id="e7803-166">Controllers and views</span></span>

* <span data-ttu-id="e7803-167">Copiar cada uno de los métodos de ASP.NET MVC `HomeController` al nuevo `HomeController`.</span><span class="sxs-lookup"><span data-stu-id="e7803-167">Copy each of the methods from the ASP.NET MVC `HomeController` to the new `HomeController`.</span></span> <span data-ttu-id="e7803-168">Tenga en cuenta que en ASP.NET MVC, tipo de valor devuelto de método de acción de controlador de la plantilla integrada [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); en el núcleo de ASP.NET MVC, los métodos de acción devueltos `IActionResult` en su lugar.</span><span class="sxs-lookup"><span data-stu-id="e7803-168">Note that in ASP.NET MVC, the built-in template's controller action method return type is [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); in ASP.NET Core MVC, the action methods return `IActionResult` instead.</span></span> <span data-ttu-id="e7803-169">`ActionResult` implementa `IActionResult`, por lo que no es necesario para cambiar el tipo de valor devuelto de los métodos de acción.</span><span class="sxs-lookup"><span data-stu-id="e7803-169">`ActionResult` implements `IActionResult`, so there's no need to change the return type of your action methods.</span></span>

* <span data-ttu-id="e7803-170">Copia la *About.cshtml*, *Contact.cshtml*, y *Index.cshtml* archivos de vista Razor desde el proyecto de MVC de ASP.NET para el proyecto de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e7803-170">Copy the *About.cshtml*, *Contact.cshtml*, and *Index.cshtml* Razor view files from the ASP.NET MVC project to the ASP.NET Core project.</span></span>

* <span data-ttu-id="e7803-171">Ejecutar la aplicación de ASP.NET Core y cada método de prueba.</span><span class="sxs-lookup"><span data-stu-id="e7803-171">Run the ASP.NET Core app and test each method.</span></span> <span data-ttu-id="e7803-172">No hemos migramos los estilos o el archivo de diseño, por lo que las vistas representadas solo contienen el contenido de los archivos de vista.</span><span class="sxs-lookup"><span data-stu-id="e7803-172">We haven't migrated the layout file or styles yet, so the rendered views only contain the content in the view files.</span></span> <span data-ttu-id="e7803-173">No tendrá los vínculos de archivo generado de diseño para la `About` y `Contact` vistas, por lo que tendrá que invocar a los mismos desde el explorador (reemplace **4492** con el número de puerto utilizado en el proyecto).</span><span class="sxs-lookup"><span data-stu-id="e7803-173">You won't have the layout file generated links for the `About` and `Contact` views, so you'll have to invoke them from the browser (replace **4492** with the port number used in your project).</span></span>

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![Página contacto](mvc/_static/contact-page.png)

<span data-ttu-id="e7803-175">Tenga en cuenta la falta de un estilo y elementos de menú.</span><span class="sxs-lookup"><span data-stu-id="e7803-175">Note the lack of styling and menu items.</span></span> <span data-ttu-id="e7803-176">Esto lo corregiremos en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="e7803-176">We'll fix that in the next section.</span></span>

## <a name="static-content"></a><span data-ttu-id="e7803-177">Contenido estático</span><span class="sxs-lookup"><span data-stu-id="e7803-177">Static content</span></span>

<span data-ttu-id="e7803-178">En versiones anteriores de ASP.NET MVC, contenido estático se alojó desde la raíz del proyecto web y se ha combinado con archivos de servidor.</span><span class="sxs-lookup"><span data-stu-id="e7803-178">In previous versions of ASP.NET MVC, static content was hosted from the root of the web project and was intermixed with server-side files.</span></span> <span data-ttu-id="e7803-179">En el núcleo de ASP.NET, contenido estático se hospeda en el *wwwroot* carpeta.</span><span class="sxs-lookup"><span data-stu-id="e7803-179">In ASP.NET Core, static content is hosted in the *wwwroot* folder.</span></span> <span data-ttu-id="e7803-180">Desea copiar el contenido estático de la aplicación de MVC de ASP.NET anterior a la *wwwroot* carpeta en el proyecto de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e7803-180">You'll want to copy the static content from your old ASP.NET MVC app to the *wwwroot* folder in your ASP.NET Core project.</span></span> <span data-ttu-id="e7803-181">En esta conversión de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="e7803-181">In this sample conversion:</span></span>

* <span data-ttu-id="e7803-182">Copia la *favicon.ico* archivo desde el proyecto MVC anterior a la *wwwroot* carpeta del proyecto de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e7803-182">Copy the *favicon.ico* file from the old MVC project to the *wwwroot* folder in the ASP.NET Core project.</span></span>

<span data-ttu-id="e7803-183">La antigua ASP.NET MVC project utiliza [Bootstrap](https://getbootstrap.com/) para su aplicación de estilos y almacena el arranque de los archivos del *contenido* y *Scripts* carpetas.</span><span class="sxs-lookup"><span data-stu-id="e7803-183">The old ASP.NET MVC project uses [Bootstrap](https://getbootstrap.com/) for its styling and stores the Bootstrap files in the *Content* and *Scripts* folders.</span></span> <span data-ttu-id="e7803-184">Hace referencia a la plantilla, que genera el proyecto de MVC de ASP.NET anterior, arranque en el archivo de diseño (*Views/Shared/_Layout.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="e7803-184">The template, which generated the old ASP.NET MVC project, references Bootstrap in the layout file (*Views/Shared/_Layout.cshtml*).</span></span> <span data-ttu-id="e7803-185">Podría copiar el *bootstrap.js* y *bootstrap.css* proyecto de archivos de ASP.NET MVC a la *wwwroot* carpeta en el nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="e7803-185">You could copy the *bootstrap.js* and *bootstrap.css* files from the ASP.NET MVC project to the *wwwroot* folder in the new project.</span></span> <span data-ttu-id="e7803-186">En su lugar, vamos a agregar compatibilidad para el arranque (y otras bibliotecas de cliente) con CDN en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="e7803-186">Instead, we'll add support for Bootstrap (and other client-side libraries) using CDNs in the next section.</span></span>

## <a name="migrate-the-layout-file"></a><span data-ttu-id="e7803-187">Migrar el archivo de diseño</span><span class="sxs-lookup"><span data-stu-id="e7803-187">Migrate the layout file</span></span>

* <span data-ttu-id="e7803-188">Copia la *_ViewStart.cshtml* archivo desde el proyecto de MVC de ASP.NET anterior *vistas* carpeta en el proyecto de ASP.NET Core *vistas* carpeta.</span><span class="sxs-lookup"><span data-stu-id="e7803-188">Copy the *_ViewStart.cshtml* file from the old ASP.NET MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="e7803-189">El *_ViewStart.cshtml* archivo no ha cambiado en MVC de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e7803-189">The *_ViewStart.cshtml* file has not changed in ASP.NET Core MVC.</span></span>

* <span data-ttu-id="e7803-190">Crear un *vistas/compartidas* carpeta.</span><span class="sxs-lookup"><span data-stu-id="e7803-190">Create a *Views/Shared* folder.</span></span>

* <span data-ttu-id="e7803-191">*Opcional:* copia *_ViewImports.cshtml* desde el *FullAspNetCore* del proyecto MVC *vistas* carpeta en el proyecto de ASP.NET Core  *Vistas* carpeta.</span><span class="sxs-lookup"><span data-stu-id="e7803-191">*Optional:* Copy *_ViewImports.cshtml* from the *FullAspNetCore* MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="e7803-192">Quite las declaraciones de espacio de nombres en el *_ViewImports.cshtml* archivo.</span><span class="sxs-lookup"><span data-stu-id="e7803-192">Remove any namespace declaration in the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="e7803-193">El *_ViewImports.cshtml* archivo proporciona los espacios de nombres para todos los archivos de vista y la pone [aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="e7803-193">The *_ViewImports.cshtml* file provides namespaces for all the view files and brings in [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="e7803-194">Aplicaciones auxiliares de etiquetas se usan en el nuevo archivo de diseño.</span><span class="sxs-lookup"><span data-stu-id="e7803-194">Tag Helpers are used in the new layout file.</span></span> <span data-ttu-id="e7803-195">El *_ViewImports.cshtml* archivo es una novedad de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e7803-195">The *_ViewImports.cshtml* file is new for ASP.NET Core.</span></span>

* <span data-ttu-id="e7803-196">Copia la *_Layout.cshtml* archivo desde el proyecto de MVC de ASP.NET anterior *vistas/compartidas* carpeta en el proyecto de ASP.NET Core *vistas/compartidas* carpeta.</span><span class="sxs-lookup"><span data-stu-id="e7803-196">Copy the *_Layout.cshtml* file from the old ASP.NET MVC project's *Views/Shared* folder into the ASP.NET Core project's *Views/Shared* folder.</span></span>

<span data-ttu-id="e7803-197">Abra *_Layout.cshtml* de archivos y realice los cambios siguientes (el código completo se muestra a continuación):</span><span class="sxs-lookup"><span data-stu-id="e7803-197">Open *_Layout.cshtml* file and make the following changes (the completed code is shown below):</span></span>

* <span data-ttu-id="e7803-198">Reemplace `@Styles.Render("~/Content/css")` con un `<link>` elemento que se va a cargar *bootstrap.css* (ver abajo).</span><span class="sxs-lookup"><span data-stu-id="e7803-198">Replace `@Styles.Render("~/Content/css")` with a `<link>` element to load *bootstrap.css* (see below).</span></span>

* <span data-ttu-id="e7803-199">Quite `@Scripts.Render("~/bundles/modernizr")`.</span><span class="sxs-lookup"><span data-stu-id="e7803-199">Remove `@Scripts.Render("~/bundles/modernizr")`.</span></span>

* <span data-ttu-id="e7803-200">Convierta en comentario la `@Html.Partial("_LoginPartial")` línea (incluya la línea con `@*...*@`).</span><span class="sxs-lookup"><span data-stu-id="e7803-200">Comment out the `@Html.Partial("_LoginPartial")` line (surround the line with `@*...*@`).</span></span> <span data-ttu-id="e7803-201">Se tendrá que volver a él en un tutorial posterior.</span><span class="sxs-lookup"><span data-stu-id="e7803-201">We'll return to it in a future tutorial.</span></span>

* <span data-ttu-id="e7803-202">Reemplace `@Scripts.Render("~/bundles/jquery")` con un `<script>` elemento (ver abajo).</span><span class="sxs-lookup"><span data-stu-id="e7803-202">Replace `@Scripts.Render("~/bundles/jquery")` with a `<script>` element (see below).</span></span>

* <span data-ttu-id="e7803-203">Reemplace `@Scripts.Render("~/bundles/bootstrap")` con un `<script>` elemento (ver abajo).</span><span class="sxs-lookup"><span data-stu-id="e7803-203">Replace `@Scripts.Render("~/bundles/bootstrap")` with a `<script>` element (see below).</span></span>

<span data-ttu-id="e7803-204">El marcado de reemplazo para la inclusión de CSS de arranque:</span><span class="sxs-lookup"><span data-stu-id="e7803-204">The replacement markup for Bootstrap CSS inclusion:</span></span>

```html
<link rel="stylesheet"
    href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css"
    integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u"
    crossorigin="anonymous">
```

<span data-ttu-id="e7803-205">El marcado de reemplazo de jQuery y la inclusión de JavaScript de arranque:</span><span class="sxs-lookup"><span data-stu-id="e7803-205">The replacement markup for jQuery and Bootstrap JavaScript inclusion:</span></span>

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"
    integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
```

<span data-ttu-id="e7803-206">La actualización *_Layout.cshtml* archivo se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="e7803-206">The updated *_Layout.cshtml* file is shown below:</span></span>

[!code-cshtml[](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7-10,29,41-44)]

<span data-ttu-id="e7803-207">Ver el sitio en el explorador.</span><span class="sxs-lookup"><span data-stu-id="e7803-207">View the site in the browser.</span></span> <span data-ttu-id="e7803-208">Ahora deberían cargarse correctamente, con los estilos esperados en su lugar.</span><span class="sxs-lookup"><span data-stu-id="e7803-208">It should now load correctly, with the expected styles in place.</span></span>

* <span data-ttu-id="e7803-209">*Opcional:* desea intentar usar el nuevo archivo de diseño.</span><span class="sxs-lookup"><span data-stu-id="e7803-209">*Optional:* You might want to try using the new layout file.</span></span> <span data-ttu-id="e7803-210">Para este proyecto se puede copiar el archivo de diseño de la *FullAspNetCore* proyecto.</span><span class="sxs-lookup"><span data-stu-id="e7803-210">For this project you can copy the layout file from the *FullAspNetCore* project.</span></span> <span data-ttu-id="e7803-211">El nuevo archivo de diseño usa [aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro) y tiene otras mejoras.</span><span class="sxs-lookup"><span data-stu-id="e7803-211">The new layout file uses [Tag Helpers](xref:mvc/views/tag-helpers/intro) and has other improvements.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="e7803-212">Configurar la agrupación y minificación</span><span class="sxs-lookup"><span data-stu-id="e7803-212">Configure bundling and minification</span></span>

<span data-ttu-id="e7803-213">Para obtener información sobre cómo configurar la agrupación y minificación, consulte [unión y Minificación](../client-side/bundling-and-minification.md).</span><span class="sxs-lookup"><span data-stu-id="e7803-213">For information about how to configure bundling and minification, see [Bundling and Minification](../client-side/bundling-and-minification.md).</span></span>

## <a name="solve-http-500-errors"></a><span data-ttu-id="e7803-214">Solucionar errores de HTTP 500</span><span class="sxs-lookup"><span data-stu-id="e7803-214">Solve HTTP 500 errors</span></span>

<span data-ttu-id="e7803-215">Hay muchos problemas que pueden provocar un mensaje de error HTTP 500 que no contienen ninguna información en el origen del problema.</span><span class="sxs-lookup"><span data-stu-id="e7803-215">There are many problems that can cause a HTTP 500 error message that contain no information on the source of the problem.</span></span> <span data-ttu-id="e7803-216">Por ejemplo, si la *Views/_ViewImports.cshtml* archivo contiene un espacio de nombres que no existe en el proyecto, obtendrá un error HTTP 500.</span><span class="sxs-lookup"><span data-stu-id="e7803-216">For example, if the *Views/_ViewImports.cshtml* file contains a namespace that doesn't exist in your project, you'll get a HTTP 500 error.</span></span> <span data-ttu-id="e7803-217">De forma predeterminada en las aplicaciones ASP.NET Core, el `UseDeveloperExceptionPage` extensión se agrega a la `IApplicationBuilder` y se ejecuta cuando la configuración es *desarrollo*.</span><span class="sxs-lookup"><span data-stu-id="e7803-217">By default in ASP.NET Core apps, the `UseDeveloperExceptionPage` extension is added to the `IApplicationBuilder` and executed when the configuration is *Development*.</span></span> <span data-ttu-id="e7803-218">Esto se detalla en el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="e7803-218">This is detailed in the following code:</span></span>

[!code-csharp[](mvc/sample/Startup.cs?highlight=19-22)]

<span data-ttu-id="e7803-219">ASP.NET Core convierte las excepciones no controladas en una aplicación web en las respuestas de error HTTP 500.</span><span class="sxs-lookup"><span data-stu-id="e7803-219">ASP.NET Core converts unhandled exceptions in a web app into HTTP 500 error responses.</span></span> <span data-ttu-id="e7803-220">Normalmente, los detalles del error no se incluyen en estas respuestas para evitar la divulgación de información potencialmente confidencial acerca del servidor.</span><span class="sxs-lookup"><span data-stu-id="e7803-220">Normally, error details aren't included in these responses to prevent disclosure of potentially sensitive information about the server.</span></span> <span data-ttu-id="e7803-221">Vea **mediante la página de excepción para desarrolladores** en [controlar los errores](../fundamentals/error-handling.md) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="e7803-221">See **Using the Developer Exception Page** in [Handle errors](../fundamentals/error-handling.md) for more information.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e7803-222">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="e7803-222">Additional resources</span></span>

* [<span data-ttu-id="e7803-223">Desarrollo del lado cliente</span><span class="sxs-lookup"><span data-stu-id="e7803-223">Client-side development</span></span>](xref:client-side/index)
* [<span data-ttu-id="e7803-224">Aplicaciones auxiliares de etiquetas</span><span class="sxs-lookup"><span data-stu-id="e7803-224">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
