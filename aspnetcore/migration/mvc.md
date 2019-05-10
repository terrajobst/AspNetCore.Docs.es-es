---
title: Migración de ASP.NET MVC a ASP.NET Core MVC
author: ardalis
description: Obtenga información sobre cómo empezar a migrar un proyecto de MVC de ASP.NET a ASP.NET Core MVC.
ms.author: riande
ms.date: 04/06/2019
uid: migration/mvc
ms.openlocfilehash: a9e2b41b933ed04a23515564892ed1694a4ac4f8
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/27/2019
ms.locfileid: "64893502"
---
# <a name="migrate-from-aspnet-mvc-to-aspnet-core-mvc"></a><span data-ttu-id="43f79-103">Migración de ASP.NET MVC a ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="43f79-103">Migrate from ASP.NET MVC to ASP.NET Core MVC</span></span>

<span data-ttu-id="43f79-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), y [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="43f79-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="43f79-105">Este artículo muestra cómo empezar a migrar un proyecto ASP.NET MVC a [ASP.NET Core MVC](../mvc/overview.md).</span><span class="sxs-lookup"><span data-stu-id="43f79-105">This article shows how to get started migrating an ASP.NET MVC project to [ASP.NET Core MVC](../mvc/overview.md).</span></span> <span data-ttu-id="43f79-106">En el proceso, destaca muchas de las cosas que han cambiado respecto a ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="43f79-106">In the process, it highlights many of the things that have changed from ASP.NET MVC.</span></span> <span data-ttu-id="43f79-107">Migración de ASP.NET MVC es un proceso de varios pasos y este artículo describe la configuración inicial, controladores básicos y vistas, contenido estático y las dependencias del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="43f79-107">Migrating from ASP.NET MVC is a multiple step process and this article covers the initial setup, basic controllers and views, static content, and client-side dependencies.</span></span> <span data-ttu-id="43f79-108">Otros artículos cubren migrar la configuración y código de identidad que se encuentra en muchos proyectos de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="43f79-108">Additional articles cover migrating configuration and identity code found in many ASP.NET MVC projects.</span></span>

> [!NOTE]
> <span data-ttu-id="43f79-109">Los números de versión en los ejemplos podrían no estar actualizados.</span><span class="sxs-lookup"><span data-stu-id="43f79-109">The version numbers in the samples might not be current.</span></span> <span data-ttu-id="43f79-110">Es posible que deba actualizar sus proyectos en consecuencia.</span><span class="sxs-lookup"><span data-stu-id="43f79-110">You may need to update your projects accordingly.</span></span>

## <a name="create-the-starter-aspnet-mvc-project"></a><span data-ttu-id="43f79-111">Crear el proyecto de ASP.NET MVC iniciador</span><span class="sxs-lookup"><span data-stu-id="43f79-111">Create the starter ASP.NET MVC project</span></span>

<span data-ttu-id="43f79-112">Para demostrar la actualización, comenzamos creando una aplicación ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="43f79-112">To demonstrate the upgrade, we'll start by creating a ASP.NET MVC app.</span></span> <span data-ttu-id="43f79-113">Crea uno con el nombre *WebApp1* para el espacio de nombres coincida con el proyecto de ASP.NET Core que creamos en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="43f79-113">Create it with the name *WebApp1* so the namespace matches the ASP.NET Core project we create in the next step.</span></span>

![El cuadro de diálogo de Visual Studio de nuevo proyecto](mvc/_static/new-project.png)

![Cuadro de diálogo nueva aplicación Web: Plantilla de proyecto MVC seleccionada en el panel de plantillas ASP.NET](mvc/_static/new-project-select-mvc-template.png)

<span data-ttu-id="43f79-116">*Opcional:* Cambiar el nombre de la solución desde *WebApp1* a *Mvc5*.</span><span class="sxs-lookup"><span data-stu-id="43f79-116">*Optional:* Change the name of the Solution from *WebApp1* to *Mvc5*.</span></span> <span data-ttu-id="43f79-117">Visual Studio muestra el nuevo nombre de la solución (*Mvc5*), lo que facilita indicar este proyecto desde el proyecto siguiente.</span><span class="sxs-lookup"><span data-stu-id="43f79-117">Visual Studio displays the new solution name (*Mvc5*), which makes it easier to tell this project from the next project.</span></span>

## <a name="create-the-aspnet-core-project"></a><span data-ttu-id="43f79-118">Crear el proyecto de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="43f79-118">Create the ASP.NET Core project</span></span>

<span data-ttu-id="43f79-119">Cree un nuevo *vacía* aplicación web de ASP.NET Core con el mismo nombre que el proyecto anterior (*WebApp1*) para que coincidan con los espacios de nombres en los dos proyectos.</span><span class="sxs-lookup"><span data-stu-id="43f79-119">Create a new *empty* ASP.NET Core web app with the same name as the previous project (*WebApp1*) so the namespaces in the two projects match.</span></span> <span data-ttu-id="43f79-120">Tener el mismo espacio de nombres resulta más fácil copiar código entre los dos proyectos.</span><span class="sxs-lookup"><span data-stu-id="43f79-120">Having the same namespace makes it easier to copy code between the two projects.</span></span> <span data-ttu-id="43f79-121">Tendrá que crear este proyecto en un directorio diferente que el proyecto anterior para usar el mismo nombre.</span><span class="sxs-lookup"><span data-stu-id="43f79-121">You'll have to create this project in a different directory than the previous project to use the same name.</span></span>

![Cuadro de diálogo Nuevo proyecto](mvc/_static/new_core.png)

![Cuadro de diálogo nueva aplicación Web de ASP.NET: Plantilla de proyecto vacía seleccionada en el panel de plantillas de ASP.NET Core](mvc/_static/new-project-select-empty-aspnet5-template.png)

* <span data-ttu-id="43f79-124">*Opcional:* Crear una nueva aplicación ASP.NET Core mediante la *aplicación Web* plantilla de proyecto.</span><span class="sxs-lookup"><span data-stu-id="43f79-124">*Optional:* Create a new ASP.NET Core app using the *Web Application* project template.</span></span> <span data-ttu-id="43f79-125">Denomine el proyecto *WebApp1*y seleccione una opción de autenticación de **cuentas de usuario individuales**.</span><span class="sxs-lookup"><span data-stu-id="43f79-125">Name the project *WebApp1*, and select an authentication option of **Individual User Accounts**.</span></span> <span data-ttu-id="43f79-126">Cambiar el nombre de esta aplicación *FullAspNetCore*.</span><span class="sxs-lookup"><span data-stu-id="43f79-126">Rename this app to *FullAspNetCore*.</span></span> <span data-ttu-id="43f79-127">Este proyecto le ahorra tiempo a crear en la conversión.</span><span class="sxs-lookup"><span data-stu-id="43f79-127">Creating this project saves you time in the conversion.</span></span> <span data-ttu-id="43f79-128">Puede mirar el código generado con plantilla para ver el resultado final o copie el código en el proyecto de conversión.</span><span class="sxs-lookup"><span data-stu-id="43f79-128">You can look at the template-generated code to see the end result or to copy code to the conversion project.</span></span> <span data-ttu-id="43f79-129">También es útil cuando se bloquea en un paso de conversión que se compara con el proyecto generado con plantilla.</span><span class="sxs-lookup"><span data-stu-id="43f79-129">It's also helpful when you get stuck on a conversion step to compare with the template-generated project.</span></span>

## <a name="configure-the-site-to-use-mvc"></a><span data-ttu-id="43f79-130">Configurar el sitio para usar MVC</span><span class="sxs-lookup"><span data-stu-id="43f79-130">Configure the site to use MVC</span></span>

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="43f79-131">Cuando el destino es .NET Core, el [Microsoft.AspNetCore.App metapaquete](xref:fundamentals/metapackage-app) se hace referencia de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="43f79-131">When targeting .NET Core, the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) is referenced by default.</span></span> <span data-ttu-id="43f79-132">Este paquete contiene paquetes de paquetes que se usan con frecuencia las aplicaciones de MVC.</span><span class="sxs-lookup"><span data-stu-id="43f79-132">This package contains packages commonly used packages by MVC apps.</span></span> <span data-ttu-id="43f79-133">Si el destino es .NET Framework, las referencias de paquete deben aparecer por separado en el archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="43f79-133">If targeting .NET Framework, package references must be listed individually in the project file.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* <span data-ttu-id="43f79-134">Cuando el destino es .NET Core, el [metapaquete Microsoft.AspNetCore.All](xref:fundamentals/metapackage) se hace referencia de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="43f79-134">When targeting .NET Core, the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) is referenced by default.</span></span> <span data-ttu-id="43f79-135">Este paquete contiene paquetes de paquetes que se usan con frecuencia las aplicaciones de MVC.</span><span class="sxs-lookup"><span data-stu-id="43f79-135">This package contains packages commonly used packages by MVC apps.</span></span> <span data-ttu-id="43f79-136">Si el destino es .NET Framework, las referencias de paquete deben aparecer por separado en el archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="43f79-136">If targeting .NET Framework, package references must be listed individually in the project file.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* <span data-ttu-id="43f79-137">Cuando el destino es .NET Core o .NET Framework, los paquetes de paquetes que se usan con frecuencia las aplicaciones de MVC se enumeran individualmente en el archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="43f79-137">When targeting .NET Core or .NET Framework, packages commonly used packages by MVC apps are listed individually in the project file.</span></span>

::: moniker-end

<span data-ttu-id="43f79-138">`Microsoft.AspNetCore.Mvc` es el marco de ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="43f79-138">`Microsoft.AspNetCore.Mvc` is the ASP.NET Core MVC framework.</span></span> <span data-ttu-id="43f79-139">`Microsoft.AspNetCore.StaticFiles` es el controlador de archivo estático.</span><span class="sxs-lookup"><span data-stu-id="43f79-139">`Microsoft.AspNetCore.StaticFiles` is the static file handler.</span></span> <span data-ttu-id="43f79-140">El tiempo de ejecución de ASP.NET Core es modular, y debe elegir explícitamente en para servir archivos estáticos (consulte [archivos estáticos](xref:fundamentals/static-files)).</span><span class="sxs-lookup"><span data-stu-id="43f79-140">The ASP.NET Core runtime is modular, and you must explicitly opt in to serve static files (see [Static files](xref:fundamentals/static-files)).</span></span>

* <span data-ttu-id="43f79-141">Abra el *Startup.cs* de archivos y cambiar el código para que coincida con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="43f79-141">Open the *Startup.cs* file and change the code to match the following:</span></span>

  [!code-csharp[](mvc/sample/Startup.cs?highlight=13,26-31)]

<span data-ttu-id="43f79-142">El `UseStaticFiles` método de extensión agrega el controlador de archivos estáticos.</span><span class="sxs-lookup"><span data-stu-id="43f79-142">The `UseStaticFiles` extension method adds the static file handler.</span></span> <span data-ttu-id="43f79-143">Como se mencionó anteriormente, el tiempo de ejecución ASP.NET es modular, y debe elegir explícitamente en para servir archivos estáticos.</span><span class="sxs-lookup"><span data-stu-id="43f79-143">As mentioned previously, the ASP.NET runtime is modular, and you must explicitly opt in to serve static files.</span></span> <span data-ttu-id="43f79-144">El `UseMvc` método de extensión agrega enrutamiento.</span><span class="sxs-lookup"><span data-stu-id="43f79-144">The `UseMvc` extension method adds routing.</span></span> <span data-ttu-id="43f79-145">Para obtener más información, consulte [inicio de la aplicación](xref:fundamentals/startup) y [enrutamiento](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="43f79-145">For more information, see [Application Startup](xref:fundamentals/startup) and [Routing](xref:fundamentals/routing).</span></span>

## <a name="add-a-controller-and-view"></a><span data-ttu-id="43f79-146">Agregar un controlador y vista</span><span class="sxs-lookup"><span data-stu-id="43f79-146">Add a controller and view</span></span>

<span data-ttu-id="43f79-147">En esta sección, agregará un controlador mínima y la vista para que actúe como marcadores de posición para el controlador de MVC de ASP.NET y las vistas, debe migrar en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="43f79-147">In this section, you'll add a minimal controller and view to serve as placeholders for the ASP.NET MVC controller and views you'll migrate in the next section.</span></span>

* <span data-ttu-id="43f79-148">Agregar un *controladores* carpeta.</span><span class="sxs-lookup"><span data-stu-id="43f79-148">Add a *Controllers* folder.</span></span>

* <span data-ttu-id="43f79-149">Agregar un **clase de controlador** denominado *HomeController.cs* a la *controladores* carpeta.</span><span class="sxs-lookup"><span data-stu-id="43f79-149">Add a **Controller Class** named *HomeController.cs* to the *Controllers* folder.</span></span>

![Cuadro de diálogo Agregar nuevo elemento](mvc/_static/add_mvc_ctl.png)

* <span data-ttu-id="43f79-151">Agregar un *vistas* carpeta.</span><span class="sxs-lookup"><span data-stu-id="43f79-151">Add a *Views* folder.</span></span>

* <span data-ttu-id="43f79-152">Agregar un *vistas/inicio* carpeta.</span><span class="sxs-lookup"><span data-stu-id="43f79-152">Add a *Views/Home* folder.</span></span>

* <span data-ttu-id="43f79-153">Agregar un **vista Razor** denominado *Index.cshtml* a la *vistas/inicio* carpeta.</span><span class="sxs-lookup"><span data-stu-id="43f79-153">Add a **Razor View** named *Index.cshtml* to the *Views/Home* folder.</span></span>

![Cuadro de diálogo Agregar nuevo elemento](mvc/_static/view.png)

<span data-ttu-id="43f79-155">A continuación se muestra la estructura del proyecto:</span><span class="sxs-lookup"><span data-stu-id="43f79-155">The project structure is shown below:</span></span>

![Explorador de soluciones con archivos y carpetas de WebApp1](mvc/_static/project-structure-controller-view.png)

<span data-ttu-id="43f79-157">Reemplace el contenido de la *Views/Home/Index.cshtml* archivo por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="43f79-157">Replace the contents of the *Views/Home/Index.cshtml* file with the following:</span></span>

```html
<h1>Hello world!</h1>
```

<span data-ttu-id="43f79-158">Ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="43f79-158">Run the app.</span></span>

![Aplicación Web abierta en Microsoft Edge](mvc/_static/hello-world.png)

<span data-ttu-id="43f79-160">Consulte [controladores](xref:mvc/controllers/actions) y [vistas](xref:mvc/views/overview) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="43f79-160">See [Controllers](xref:mvc/controllers/actions) and [Views](xref:mvc/views/overview) for more information.</span></span>

<span data-ttu-id="43f79-161">Ahora que tenemos un proyecto ASP.NET Core trabajo mínimo, podemos comenzar a migrar la funcionalidad desde el proyecto de MVC de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="43f79-161">Now that we have a minimal working ASP.NET Core project, we can start migrating functionality from the ASP.NET MVC project.</span></span> <span data-ttu-id="43f79-162">Es necesario mover los siguientes:</span><span class="sxs-lookup"><span data-stu-id="43f79-162">We need to move the following:</span></span>

* <span data-ttu-id="43f79-163">contenido del lado cliente (CSS, fuentes y scripts)</span><span class="sxs-lookup"><span data-stu-id="43f79-163">client-side content (CSS, fonts, and scripts)</span></span>

* <span data-ttu-id="43f79-164">controladores</span><span class="sxs-lookup"><span data-stu-id="43f79-164">controllers</span></span>

* <span data-ttu-id="43f79-165">vistas</span><span class="sxs-lookup"><span data-stu-id="43f79-165">views</span></span>

* <span data-ttu-id="43f79-166">modelos</span><span class="sxs-lookup"><span data-stu-id="43f79-166">models</span></span>

* <span data-ttu-id="43f79-167">La Unión</span><span class="sxs-lookup"><span data-stu-id="43f79-167">bundling</span></span>

* <span data-ttu-id="43f79-168">filtros</span><span class="sxs-lookup"><span data-stu-id="43f79-168">filters</span></span>

* <span data-ttu-id="43f79-169">Registro de entrada/salida, la identidad (Esto se hace en el siguiente tutorial.)</span><span class="sxs-lookup"><span data-stu-id="43f79-169">Log in/out, Identity (This is done in the next tutorial.)</span></span>

## <a name="controllers-and-views"></a><span data-ttu-id="43f79-170">Controladores y vistas</span><span class="sxs-lookup"><span data-stu-id="43f79-170">Controllers and views</span></span>

* <span data-ttu-id="43f79-171">Copie cada uno de los métodos de ASP.NET MVC `HomeController` al nuevo `HomeController`.</span><span class="sxs-lookup"><span data-stu-id="43f79-171">Copy each of the methods from the ASP.NET MVC `HomeController` to the new `HomeController`.</span></span> <span data-ttu-id="43f79-172">Tenga en cuenta que en ASP.NET MVC, tipo devuelto de método de acción de controlador de la plantilla integrada [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); en ASP.NET Core MVC, los métodos de acción devueltos `IActionResult` en su lugar.</span><span class="sxs-lookup"><span data-stu-id="43f79-172">Note that in ASP.NET MVC, the built-in template's controller action method return type is [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); in ASP.NET Core MVC, the action methods return `IActionResult` instead.</span></span> <span data-ttu-id="43f79-173">`ActionResult` implementa `IActionResult`, por lo que no es necesario para cambiar el tipo de valor devuelto de los métodos de acción.</span><span class="sxs-lookup"><span data-stu-id="43f79-173">`ActionResult` implements `IActionResult`, so there's no need to change the return type of your action methods.</span></span>

* <span data-ttu-id="43f79-174">Copia el *About.cshtml*, *Contact.cshtml*, y *Index.cshtml* archivos de vista de Razor para el proyecto de ASP.NET Core desde el proyecto de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="43f79-174">Copy the *About.cshtml*, *Contact.cshtml*, and *Index.cshtml* Razor view files from the ASP.NET MVC project to the ASP.NET Core project.</span></span>

* <span data-ttu-id="43f79-175">Ejecute la aplicación de ASP.NET Core y cada método de prueba.</span><span class="sxs-lookup"><span data-stu-id="43f79-175">Run the ASP.NET Core app and test each method.</span></span> <span data-ttu-id="43f79-176">No hemos migramos los estilos o el archivo de diseño, por lo que las vistas representadas contienen solo el contenido de los archivos de vista.</span><span class="sxs-lookup"><span data-stu-id="43f79-176">We haven't migrated the layout file or styles yet, so the rendered views only contain the content in the view files.</span></span> <span data-ttu-id="43f79-177">No tendrá los vínculos de archivo generado de diseño para el `About` y `Contact` vistas, por lo que tendrá que invocar a ellos desde el explorador (reemplace **4492** con el número de puerto utilizado en el proyecto).</span><span class="sxs-lookup"><span data-stu-id="43f79-177">You won't have the layout file generated links for the `About` and `Contact` views, so you'll have to invoke them from the browser (replace **4492** with the port number used in your project).</span></span>

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![Página de contacto](mvc/_static/contact-page.png)

<span data-ttu-id="43f79-179">Tenga en cuenta la falta de estilo y elementos de menú.</span><span class="sxs-lookup"><span data-stu-id="43f79-179">Note the lack of styling and menu items.</span></span> <span data-ttu-id="43f79-180">Esto lo corregiremos en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="43f79-180">We'll fix that in the next section.</span></span>

## <a name="static-content"></a><span data-ttu-id="43f79-181">Contenido estático</span><span class="sxs-lookup"><span data-stu-id="43f79-181">Static content</span></span>

<span data-ttu-id="43f79-182">En versiones anteriores de ASP.NET MVC, contenido estático se hospedaba desde la raíz del proyecto web y se ha combinado con los archivos del servidor.</span><span class="sxs-lookup"><span data-stu-id="43f79-182">In previous versions of ASP.NET MVC, static content was hosted from the root of the web project and was intermixed with server-side files.</span></span> <span data-ttu-id="43f79-183">En ASP.NET Core, el contenido estático se hospeda en el *wwwroot* carpeta.</span><span class="sxs-lookup"><span data-stu-id="43f79-183">In ASP.NET Core, static content is hosted in the *wwwroot* folder.</span></span> <span data-ttu-id="43f79-184">Desea copiar el contenido estático de la antigua aplicación ASP.NET MVC para la *wwwroot* carpeta del proyecto de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="43f79-184">You'll want to copy the static content from your old ASP.NET MVC app to the *wwwroot* folder in your ASP.NET Core project.</span></span> <span data-ttu-id="43f79-185">En esta conversión de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="43f79-185">In this sample conversion:</span></span>

* <span data-ttu-id="43f79-186">Copia el *favicon.ico* archivo desde el proyecto MVC anterior a la *wwwroot* carpeta en el proyecto de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="43f79-186">Copy the *favicon.ico* file from the old MVC project to the *wwwroot* folder in the ASP.NET Core project.</span></span>

<span data-ttu-id="43f79-187">Proyecto de la antigua de ASP.NET MVC usa [Bootstrap](https://getbootstrap.com/) para su estilo y almacenes de archivos de la secuencia de inicio de la *contenido* y *Scripts* carpetas.</span><span class="sxs-lookup"><span data-stu-id="43f79-187">The old ASP.NET MVC project uses [Bootstrap](https://getbootstrap.com/) for its styling and stores the Bootstrap files in the *Content* and *Scripts* folders.</span></span> <span data-ttu-id="43f79-188">La plantilla, que genera el proyecto de ASP.NET MVC anterior, hace referencia a Bootstrap en el archivo de diseño (*Views/Shared/_Layout.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="43f79-188">The template, which generated the old ASP.NET MVC project, references Bootstrap in the layout file (*Views/Shared/_Layout.cshtml*).</span></span> <span data-ttu-id="43f79-189">Puede copiar el *bootstrap.js* y *bootstrap.css* proyecto de archivos de ASP.NET MVC a la *wwwroot* carpeta en el nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="43f79-189">You could copy the *bootstrap.js* and *bootstrap.css* files from the ASP.NET MVC project to the *wwwroot* folder in the new project.</span></span> <span data-ttu-id="43f79-190">En su lugar, vamos a agregar compatibilidad con Bootstrap (y otras bibliotecas de cliente) uso de CDN en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="43f79-190">Instead, we'll add support for Bootstrap (and other client-side libraries) using CDNs in the next section.</span></span>

## <a name="migrate-the-layout-file"></a><span data-ttu-id="43f79-191">Migrar el archivo de diseño</span><span class="sxs-lookup"><span data-stu-id="43f79-191">Migrate the layout file</span></span>

* <span data-ttu-id="43f79-192">Copia el *_ViewStart.cshtml* archivo desde el proyecto de ASP.NET MVC antiguo *vistas* carpeta en el proyecto de ASP.NET Core *vistas* carpeta.</span><span class="sxs-lookup"><span data-stu-id="43f79-192">Copy the *_ViewStart.cshtml* file from the old ASP.NET MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="43f79-193">El *_ViewStart.cshtml* archivo no ha cambiado en ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="43f79-193">The *_ViewStart.cshtml* file has not changed in ASP.NET Core MVC.</span></span>

* <span data-ttu-id="43f79-194">Crear un *Views/Shared* carpeta.</span><span class="sxs-lookup"><span data-stu-id="43f79-194">Create a *Views/Shared* folder.</span></span>

* <span data-ttu-id="43f79-195">*Opcional:* Copia *_ViewImports.cshtml* desde el *FullAspNetCore* del proyecto de MVC *vistas* carpeta en el proyecto de ASP.NET Core *vistas* carpeta.</span><span class="sxs-lookup"><span data-stu-id="43f79-195">*Optional:* Copy *_ViewImports.cshtml* from the *FullAspNetCore* MVC project's *Views* folder into the ASP.NET Core project's *Views* folder.</span></span> <span data-ttu-id="43f79-196">Quitar las declaraciones de espacio de nombres en el *_ViewImports.cshtml* archivo.</span><span class="sxs-lookup"><span data-stu-id="43f79-196">Remove any namespace declaration in the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="43f79-197">El *_ViewImports.cshtml* archivo proporciona los espacios de nombres para todos los archivos de vista y se pone [aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="43f79-197">The *_ViewImports.cshtml* file provides namespaces for all the view files and brings in [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="43f79-198">Aplicaciones auxiliares de etiquetas se usan en el nuevo archivo de diseño.</span><span class="sxs-lookup"><span data-stu-id="43f79-198">Tag Helpers are used in the new layout file.</span></span> <span data-ttu-id="43f79-199">El *_ViewImports.cshtml* archivo es nuevo en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="43f79-199">The *_ViewImports.cshtml* file is new for ASP.NET Core.</span></span>

* <span data-ttu-id="43f79-200">Copia el *_Layout.cshtml* archivo desde el proyecto de ASP.NET MVC antiguo *Views/Shared* carpeta en el proyecto de ASP.NET Core *Views/Shared* carpeta.</span><span class="sxs-lookup"><span data-stu-id="43f79-200">Copy the *_Layout.cshtml* file from the old ASP.NET MVC project's *Views/Shared* folder into the ASP.NET Core project's *Views/Shared* folder.</span></span>

<span data-ttu-id="43f79-201">Abra *_Layout.cshtml* de archivos y realice los cambios siguientes (el código completo se muestra a continuación):</span><span class="sxs-lookup"><span data-stu-id="43f79-201">Open *_Layout.cshtml* file and make the following changes (the completed code is shown below):</span></span>

* <span data-ttu-id="43f79-202">Reemplace `@Styles.Render("~/Content/css")` con un `<link>` elemento cargar *bootstrap.css* (ver abajo).</span><span class="sxs-lookup"><span data-stu-id="43f79-202">Replace `@Styles.Render("~/Content/css")` with a `<link>` element to load *bootstrap.css* (see below).</span></span>

* <span data-ttu-id="43f79-203">Quite `@Scripts.Render("~/bundles/modernizr")`.</span><span class="sxs-lookup"><span data-stu-id="43f79-203">Remove `@Scripts.Render("~/bundles/modernizr")`.</span></span>

* <span data-ttu-id="43f79-204">Marque como comentario el `@Html.Partial("_LoginPartial")` línea (Rodear con la línea de `@*...*@`).</span><span class="sxs-lookup"><span data-stu-id="43f79-204">Comment out the `@Html.Partial("_LoginPartial")` line (surround the line with `@*...*@`).</span></span> <span data-ttu-id="43f79-205">Para obtener más información, consulte [migrar autenticación e Identity a ASP.NET Core](xref:migration/identity)</span><span class="sxs-lookup"><span data-stu-id="43f79-205">For more information see [Migrate Authentication and Identity to ASP.NET Core](xref:migration/identity)</span></span>

* <span data-ttu-id="43f79-206">Reemplace `@Scripts.Render("~/bundles/jquery")` con un `<script>` elemento (ver abajo).</span><span class="sxs-lookup"><span data-stu-id="43f79-206">Replace `@Scripts.Render("~/bundles/jquery")` with a `<script>` element (see below).</span></span>

* <span data-ttu-id="43f79-207">Reemplace `@Scripts.Render("~/bundles/bootstrap")` con un `<script>` elemento (ver abajo).</span><span class="sxs-lookup"><span data-stu-id="43f79-207">Replace `@Scripts.Render("~/bundles/bootstrap")` with a `<script>` element (see below).</span></span>

<span data-ttu-id="43f79-208">El marcado de reemplazo para la inclusión de CSS de Bootstrap:</span><span class="sxs-lookup"><span data-stu-id="43f79-208">The replacement markup for Bootstrap CSS inclusion:</span></span>

```html
<link rel="stylesheet"
    href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css"
    integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u"
    crossorigin="anonymous">
```

<span data-ttu-id="43f79-209">El marcado de reemplazo para jQuery y la inclusión de JavaScript de Bootstrap:</span><span class="sxs-lookup"><span data-stu-id="43f79-209">The replacement markup for jQuery and Bootstrap JavaScript inclusion:</span></span>

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"
    integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
```

<span data-ttu-id="43f79-210">La actualización *_Layout.cshtml* archivo se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="43f79-210">The updated *_Layout.cshtml* file is shown below:</span></span>

[!code-cshtml[](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7-10,29,41-44)]

<span data-ttu-id="43f79-211">Ver el sitio en el explorador.</span><span class="sxs-lookup"><span data-stu-id="43f79-211">View the site in the browser.</span></span> <span data-ttu-id="43f79-212">Ahora deberían cargarse correctamente, con los estilos esperados en su lugar.</span><span class="sxs-lookup"><span data-stu-id="43f79-212">It should now load correctly, with the expected styles in place.</span></span>

* <span data-ttu-id="43f79-213">*Opcional:* Es posible que desee intentar usar el nuevo archivo de diseño.</span><span class="sxs-lookup"><span data-stu-id="43f79-213">*Optional:* You might want to try using the new layout file.</span></span> <span data-ttu-id="43f79-214">Para este proyecto puede copiar el archivo de diseño de la *FullAspNetCore* proyecto.</span><span class="sxs-lookup"><span data-stu-id="43f79-214">For this project you can copy the layout file from the *FullAspNetCore* project.</span></span> <span data-ttu-id="43f79-215">El nuevo archivo de diseño usa [aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro) y tiene otras mejoras.</span><span class="sxs-lookup"><span data-stu-id="43f79-215">The new layout file uses [Tag Helpers](xref:mvc/views/tag-helpers/intro) and has other improvements.</span></span>

## <a name="configure-bundling-and-minification"></a><span data-ttu-id="43f79-216">Configurar la unión y minificación</span><span class="sxs-lookup"><span data-stu-id="43f79-216">Configure bundling and minification</span></span>

<span data-ttu-id="43f79-217">Para obtener información sobre cómo configurar la unión y minificación, consulte [unión y Minificación](../client-side/bundling-and-minification.md).</span><span class="sxs-lookup"><span data-stu-id="43f79-217">For information about how to configure bundling and minification, see [Bundling and Minification](../client-side/bundling-and-minification.md).</span></span>

## <a name="solve-http-500-errors"></a><span data-ttu-id="43f79-218">Solucionar errores HTTP 500</span><span class="sxs-lookup"><span data-stu-id="43f79-218">Solve HTTP 500 errors</span></span>

<span data-ttu-id="43f79-219">Hay muchos problemas que pueden causar un mensaje de error HTTP 500 que no contienen información sobre el origen del problema.</span><span class="sxs-lookup"><span data-stu-id="43f79-219">There are many problems that can cause a HTTP 500 error message that contain no information on the source of the problem.</span></span> <span data-ttu-id="43f79-220">Por ejemplo, si la *Views/_viewimports.cshtml* archivo contiene un espacio de nombres que no existe en el proyecto, obtendrá un error HTTP 500.</span><span class="sxs-lookup"><span data-stu-id="43f79-220">For example, if the *Views/_ViewImports.cshtml* file contains a namespace that doesn't exist in your project, you'll get a HTTP 500 error.</span></span> <span data-ttu-id="43f79-221">De forma predeterminada en las aplicaciones ASP.NET Core, el `UseDeveloperExceptionPage` extensión se agrega a la `IApplicationBuilder` y se ejecutan cuando la configuración está *desarrollo*.</span><span class="sxs-lookup"><span data-stu-id="43f79-221">By default in ASP.NET Core apps, the `UseDeveloperExceptionPage` extension is added to the `IApplicationBuilder` and executed when the configuration is *Development*.</span></span> <span data-ttu-id="43f79-222">Esto se detalla en el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="43f79-222">This is detailed in the following code:</span></span>

[!code-csharp[](mvc/sample/Startup.cs?highlight=19-22)]

<span data-ttu-id="43f79-223">ASP.NET Core convierte las excepciones no controladas en una aplicación web en las respuestas de error HTTP 500.</span><span class="sxs-lookup"><span data-stu-id="43f79-223">ASP.NET Core converts unhandled exceptions in a web app into HTTP 500 error responses.</span></span> <span data-ttu-id="43f79-224">Normalmente, los detalles del error no se incluyen en estas respuestas para evitar la divulgación de información potencialmente confidencial sobre el servidor.</span><span class="sxs-lookup"><span data-stu-id="43f79-224">Normally, error details aren't included in these responses to prevent disclosure of potentially sensitive information about the server.</span></span> <span data-ttu-id="43f79-225">Consulte **mediante la página de excepciones de desarrollador** en [controlar los errores](../fundamentals/error-handling.md) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="43f79-225">See **Using the Developer Exception Page** in [Handle errors](../fundamentals/error-handling.md) for more information.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="43f79-226">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="43f79-226">Additional resources</span></span>

* <xref:blazor/index>
* <xref:mvc/views/tag-helpers/intro>
