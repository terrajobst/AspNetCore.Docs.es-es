---
title: "Áreas"
author: rick-anderson
description: "Muestra cómo trabajar con áreas."
keywords: "ASP.NET Core, áreas, enrutamiento, vistas"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: 5e16d5e8-5696-4cb2-8ec7-d36be305c922
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/areas
ms.openlocfilehash: e0958d6ba87dd34a7bf455d37ea8b29a32715104
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="areas"></a><span data-ttu-id="fc822-104">Áreas</span><span class="sxs-lookup"><span data-stu-id="fc822-104">Areas</span></span>

<span data-ttu-id="fc822-105">Por [Dhananjay Kumar](https://twitter.com/debug_mode) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fc822-105">By [Dhananjay Kumar](https://twitter.com/debug_mode)  and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fc822-106">Las áreas son una característica de ASP.NET MVC que se usa para organizar la funcionalidad relacionada en un grupo como un espacio de nombres independiente (para el enrutamiento) y la estructura de carpetas (para vistas).</span><span class="sxs-lookup"><span data-stu-id="fc822-106">Areas are an ASP.NET MVC feature used to organize related functionality into a group as a separate namespace (for routing) and folder structure (for views).</span></span> <span data-ttu-id="fc822-107">El uso de áreas crea una jerarquía con el fin de enrutamiento mediante la adición de otro parámetro de ruta, `area`a `controller` y `action`.</span><span class="sxs-lookup"><span data-stu-id="fc822-107">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area`, to `controller` and `action`.</span></span>

<span data-ttu-id="fc822-108">Las áreas permiten dividir una aplicación Web de MVC de ASP.NET Core grande en agrupaciones funcionales más pequeñas.</span><span class="sxs-lookup"><span data-stu-id="fc822-108">Areas provide a way to partition a large ASP.NET Core MVC Web app into smaller functional groupings.</span></span> <span data-ttu-id="fc822-109">Un área de forma eficaz es una estructura MVC dentro de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="fc822-109">An area is effectively an MVC structure inside an application.</span></span> <span data-ttu-id="fc822-110">En un proyecto MVC, los componentes lógicos como modelo, el controlador y la vista se guardan en carpetas diferentes y MVC usa las convenciones de nomenclatura para crear la relación entre estos componentes.</span><span class="sxs-lookup"><span data-stu-id="fc822-110">In an MVC project, logical components like Model, Controller, and View are kept in different folders, and MVC uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="fc822-111">Para una aplicación grande, puede ser conveniente dividir la aplicación en distintas áreas de nivel alto de funcionalidad.</span><span class="sxs-lookup"><span data-stu-id="fc822-111">For a large app, it may be advantageous to partition the  app into separate high level areas of functionality.</span></span> <span data-ttu-id="fc822-112">Por ejemplo, una aplicación de comercio electrónico con varias unidades de negocio, como la desprotección, facturación y búsqueda etcetera. Cada una de estas unidades tienen sus propias vistas de componente lógico, controladores y modelos.</span><span class="sxs-lookup"><span data-stu-id="fc822-112">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search etc. Each of these units have their own logical component views, controllers, and models.</span></span> <span data-ttu-id="fc822-113">En este escenario, puede utilizar las áreas para la partición físicamente de los componentes empresariales en el mismo proyecto.</span><span class="sxs-lookup"><span data-stu-id="fc822-113">In this scenario, you can use Areas to physically partition the business components in the same project.</span></span>

<span data-ttu-id="fc822-114">Un área puede definirse como funcionales unidades más pequeñas en un proyecto de MVC de ASP.NET Core con su propio conjunto de modelos, vistas y controladores.</span><span class="sxs-lookup"><span data-stu-id="fc822-114">An area can be defined as smaller functional units in an ASP.NET Core MVC project with its own set of controllers, views, and models.</span></span>

<span data-ttu-id="fc822-115">Considere el uso de las áreas en un MVC proyecto cuando:</span><span class="sxs-lookup"><span data-stu-id="fc822-115">Consider using Areas in an MVC project when:</span></span>

* <span data-ttu-id="fc822-116">La aplicación está formada por varios componentes funcionales de alto nivel que deben ir separados lógicamente</span><span class="sxs-lookup"><span data-stu-id="fc822-116">Your application is made of multiple high-level functional components that should be logically separated</span></span>

* <span data-ttu-id="fc822-117">Desea crear particiones en el proyecto MVC para que cada área funcional pueda encargarse de forma independiente</span><span class="sxs-lookup"><span data-stu-id="fc822-117">You want to partition your MVC project so that each functional area can be worked on independently</span></span>

<span data-ttu-id="fc822-118">Características de área:</span><span class="sxs-lookup"><span data-stu-id="fc822-118">Area features:</span></span>

* <span data-ttu-id="fc822-119">Una aplicación de MVC de ASP.NET Core puede tener cualquier número de áreas</span><span class="sxs-lookup"><span data-stu-id="fc822-119">An ASP.NET Core MVC app can have any number of areas</span></span>

* <span data-ttu-id="fc822-120">Cada área tiene sus propios controladores, modelos y vistas</span><span class="sxs-lookup"><span data-stu-id="fc822-120">Each area has its own controllers, models, and views</span></span>

* <span data-ttu-id="fc822-121">Le permite organizar los proyectos MVC grandes en varios componentes de alto nivel que pueden trabajar de forma independiente</span><span class="sxs-lookup"><span data-stu-id="fc822-121">Allows you to organize large MVC projects into multiple high-level components that can be worked on independently</span></span>

* <span data-ttu-id="fc822-122">Admite varios controladores con el mismo nombre - siempre que tienen diferentes *áreas*</span><span class="sxs-lookup"><span data-stu-id="fc822-122">Supports multiple controllers with the same name - as long as they have different *areas*</span></span>

<span data-ttu-id="fc822-123">¡Eche un vistazo a un ejemplo para ilustrar cómo se crean y utilizan las áreas.</span><span class="sxs-lookup"><span data-stu-id="fc822-123">Let's take a look at an example to illustrate how Areas are created and used.</span></span> <span data-ttu-id="fc822-124">Supongamos que tiene una aplicación de la tienda que tiene dos grupos distintos de controladores y vistas: productos y servicios.</span><span class="sxs-lookup"><span data-stu-id="fc822-124">Let's say you have a store app that has two distinct groupings of controllers and views: Products and Services.</span></span> <span data-ttu-id="fc822-125">Una carpeta típica de la estructura para que el uso de áreas MVC es similar a continuación:</span><span class="sxs-lookup"><span data-stu-id="fc822-125">A typical folder structure for that using MVC areas looks like below:</span></span>

* <span data-ttu-id="fc822-126">Nombre de proyecto</span><span class="sxs-lookup"><span data-stu-id="fc822-126">Project name</span></span>

  * <span data-ttu-id="fc822-127">Áreas</span><span class="sxs-lookup"><span data-stu-id="fc822-127">Areas</span></span>

    * <span data-ttu-id="fc822-128">Productos</span><span class="sxs-lookup"><span data-stu-id="fc822-128">Products</span></span>

      * <span data-ttu-id="fc822-129">Controladores</span><span class="sxs-lookup"><span data-stu-id="fc822-129">Controllers</span></span>

        * <span data-ttu-id="fc822-130">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="fc822-130">HomeController.cs</span></span>

        * <span data-ttu-id="fc822-131">ManageController.cs</span><span class="sxs-lookup"><span data-stu-id="fc822-131">ManageController.cs</span></span>

      * <span data-ttu-id="fc822-132">Vistas</span><span class="sxs-lookup"><span data-stu-id="fc822-132">Views</span></span>

        * <span data-ttu-id="fc822-133">Página principal</span><span class="sxs-lookup"><span data-stu-id="fc822-133">Home</span></span>

          * <span data-ttu-id="fc822-134">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="fc822-134">Index.cshtml</span></span>

        * <span data-ttu-id="fc822-135">Administrar</span><span class="sxs-lookup"><span data-stu-id="fc822-135">Manage</span></span>

          * <span data-ttu-id="fc822-136">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="fc822-136">Index.cshtml</span></span>

    * <span data-ttu-id="fc822-137">Servicios</span><span class="sxs-lookup"><span data-stu-id="fc822-137">Services</span></span>

      * <span data-ttu-id="fc822-138">Controladores</span><span class="sxs-lookup"><span data-stu-id="fc822-138">Controllers</span></span>

        * <span data-ttu-id="fc822-139">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="fc822-139">HomeController.cs</span></span>

      * <span data-ttu-id="fc822-140">Vistas</span><span class="sxs-lookup"><span data-stu-id="fc822-140">Views</span></span>

        * <span data-ttu-id="fc822-141">Página principal</span><span class="sxs-lookup"><span data-stu-id="fc822-141">Home</span></span>

          * <span data-ttu-id="fc822-142">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="fc822-142">Index.cshtml</span></span>

<span data-ttu-id="fc822-143">Cuando trata de MVC representar una vista en un área, de forma predeterminada, intenta buscar en las siguientes ubicaciones:</span><span class="sxs-lookup"><span data-stu-id="fc822-143">When MVC tries to render a view in an Area, by default, it tries to look in the following locations:</span></span>

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
   /Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
   /Views/Shared/<Action-Name>.cshtml
   ```

<span data-ttu-id="fc822-144">Éstas son las ubicaciones predeterminadas que se pueden cambiar a través de la `AreaViewLocationFormats` en el `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.</span><span class="sxs-lookup"><span data-stu-id="fc822-144">These are the default locations which can be changed via the `AreaViewLocationFormats` on the `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.</span></span>

<span data-ttu-id="fc822-145">Por ejemplo, en el siguiente código en lugar de tener el nombre de la carpeta como 'Áreas', se ha cambiado a 'Categorías'.</span><span class="sxs-lookup"><span data-stu-id="fc822-145">For example, in the below code instead of having the folder name as 'Areas', it has been changed to 'Categories'.</span></span>

```csharp
services.Configure<RazorViewEngineOptions>(options =>
   {
       options.AreaViewLocationFormats.Clear();
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/{1}/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/Shared/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Views/Shared/{0}.cshtml");
   });
   ```

<span data-ttu-id="fc822-146">Hay que destacar es que la estructura de la *vistas* carpeta es la única que se considera importante aquí y el contenido del resto de las carpetas, como *controladores* y *modelos* does **no** importa.</span><span class="sxs-lookup"><span data-stu-id="fc822-146">One thing to note is that the structure of the *Views* folder is the only one which is considered important here and the content of the rest of the folders like *Controllers* and *Models* does **not** matter.</span></span> <span data-ttu-id="fc822-147">Por ejemplo, no necesita tener un *controladores* y *modelos* carpeta en absoluto.</span><span class="sxs-lookup"><span data-stu-id="fc822-147">For example, you need not have a *Controllers* and *Models* folder at all.</span></span> <span data-ttu-id="fc822-148">Esto funciona porque el contenido de *controladores* y *modelos* es código que se compila en una .dll, mientras que el contenido de la *vistas* no es hasta que una solicitud para que se ha realizado la vista.</span><span class="sxs-lookup"><span data-stu-id="fc822-148">This works because the content of *Controllers* and *Models* is just code which gets compiled into a .dll where as the content of the *Views* is not until a request to that view has been made.</span></span>

<span data-ttu-id="fc822-149">Una vez que haya definido la jerarquía de carpetas, se debe indicar MVC que cada controlador está asociado a un área.</span><span class="sxs-lookup"><span data-stu-id="fc822-149">Once you've defined the folder hierarchy, you need to tell MVC that each controller is associated with an area.</span></span> <span data-ttu-id="fc822-150">Para hacerlo, decorando el nombre del controlador con la `[Area]` atributo.</span><span class="sxs-lookup"><span data-stu-id="fc822-150">You do that by decorating the controller name with the `[Area]` attribute.</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [4]}} -->

```csharp
...
   namespace MyStore.Areas.Products.Controllers
   {
       [Area("Products")]
       public class HomeController : Controller
       {
           // GET: /Products/Home/Index
           public IActionResult Index()
           {
               return View();
           }

           // GET: /Products/Home/Create
           public IActionResult Create()
           {
               return View();
           }
       }
   }
   ```

<span data-ttu-id="fc822-151">Establecer una definición de ruta que funcione con sus áreas recién creados.</span><span class="sxs-lookup"><span data-stu-id="fc822-151">Set up a route definition that works with your newly created areas.</span></span> <span data-ttu-id="fc822-152">El [enrutamiento a las acciones del controlador](routing.md) artículo entra en detalles acerca de cómo crear definiciones de route, incluido el uso de rutas convencionales frente a las rutas de atributo.</span><span class="sxs-lookup"><span data-stu-id="fc822-152">The [Routing to Controller Actions](routing.md) article goes into detail about how to create route definitions, including using conventional routes versus attribute routes.</span></span> <span data-ttu-id="fc822-153">En este ejemplo, vamos a usar una ruta convencional.</span><span class="sxs-lookup"><span data-stu-id="fc822-153">In this example, we'll use a conventional route.</span></span> <span data-ttu-id="fc822-154">Para ello, abra el *Startup.cs* archivo y modificarlo agregándole el `areaRoute` con el nombre de definición de ruta más adelante.</span><span class="sxs-lookup"><span data-stu-id="fc822-154">To do so, open the *Startup.cs* file and modify it by adding the `areaRoute` named route definition below.</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [4, 5, 6]}} -->

```csharp
...
   app.UseMvc(routes =>
   {
     routes.MapRoute(name: "areaRoute",
       template: "{area:exists}/{controller=Home}/{action=Index}");

     routes.MapRoute(
         name: "default",
         template: "{controller=Home}/{action=Index}/{id?}");
   });
   ```

<span data-ttu-id="fc822-155">Vaya a `http://<yourApp>/products`, el `Index` método de acción de la `HomeController` en la `Products` área se invocará.</span><span class="sxs-lookup"><span data-stu-id="fc822-155">Browsing to `http://<yourApp>/products`, the `Index` action method of the `HomeController` in the `Products` area will be invoked.</span></span>

## <a name="link-generation"></a><span data-ttu-id="fc822-156">Generación de vínculo</span><span class="sxs-lookup"><span data-stu-id="fc822-156">Link Generation</span></span>

* <span data-ttu-id="fc822-157">Generar vínculos de una acción en un área en función de controlador a otra acción en el mismo controlador.</span><span class="sxs-lookup"><span data-stu-id="fc822-157">Generating links from an action within an area based controller to another action within the same controller.</span></span>

  <span data-ttu-id="fc822-158">Supongamos que la ruta de acceso de la solicitud actual es como`/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="fc822-158">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="fc822-159">Sintaxis de HtmlHelper:`@Html.ActionLink("Go to Product's Home Page", "Index")`</span><span class="sxs-lookup"><span data-stu-id="fc822-159">HtmlHelper syntax: `@Html.ActionLink("Go to Product's Home Page", "Index")`</span></span>

  <span data-ttu-id="fc822-160">Sintaxis de TagHelper:`<a asp-action="Index">Go to Product's Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="fc822-160">TagHelper syntax: `<a asp-action="Index">Go to Product's Home Page</a>`</span></span>

  <span data-ttu-id="fc822-161">Tenga en cuenta que no se necesitamos proporcionar los valores 'área' y 'controlador' aquí y ya están disponibles en el contexto de la solicitud actual.</span><span class="sxs-lookup"><span data-stu-id="fc822-161">Note that we need not supply the 'area' and 'controller' values here as they are already available in the context of the current request.</span></span> <span data-ttu-id="fc822-162">Estos tipos de valores se denominan `ambient` valores.</span><span class="sxs-lookup"><span data-stu-id="fc822-162">These kind of values are called `ambient` values.</span></span>

* <span data-ttu-id="fc822-163">Generar vínculos de una acción en un área en la función de controlador a otra acción en un controlador diferente</span><span class="sxs-lookup"><span data-stu-id="fc822-163">Generating links from an action within an area based controller to another action on a different controller</span></span>

  <span data-ttu-id="fc822-164">Supongamos que la ruta de acceso de la solicitud actual es como`/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="fc822-164">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="fc822-165">Sintaxis de HtmlHelper:`@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Manage")`</span><span class="sxs-lookup"><span data-stu-id="fc822-165">HtmlHelper syntax: `@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Manage")`</span></span>

  <span data-ttu-id="fc822-166">Sintaxis de TagHelper:`<a asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="fc822-166">TagHelper syntax: `<a asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`</span></span>

  <span data-ttu-id="fc822-167">Tenga en cuenta que aquí se utiliza el valor ambiente de un área de' ', pero se especifica explícitamente el valor de 'controlador' anteriormente.</span><span class="sxs-lookup"><span data-stu-id="fc822-167">Note that here the ambient value of an 'area' is used but the 'controller' value is specified explicitly above.</span></span>

* <span data-ttu-id="fc822-168">Generar vínculos de una acción dentro de un área según controlador a otra acción en un controlador diferente y un área diferente.</span><span class="sxs-lookup"><span data-stu-id="fc822-168">Generating links from an action within an area based controller to another action on a different controller and a different area.</span></span>

  <span data-ttu-id="fc822-169">Supongamos que la ruta de acceso de la solicitud actual es como`/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="fc822-169">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="fc822-170">Sintaxis de HtmlHelper:`@Html.ActionLink("Go to Services’ Home Page", "Index", "Home", new { area = "Services" })`</span><span class="sxs-lookup"><span data-stu-id="fc822-170">HtmlHelper syntax: `@Html.ActionLink("Go to Services’ Home Page", "Index", "Home", new { area = "Services" })`</span></span>

  <span data-ttu-id="fc822-171">Sintaxis de TagHelper:`<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services’ Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="fc822-171">TagHelper syntax: `<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services’ Home Page</a>`</span></span>

  <span data-ttu-id="fc822-172">Tenga en cuenta que aquí no se utiliza ningún valor ambiente.</span><span class="sxs-lookup"><span data-stu-id="fc822-172">Note that here no ambient values are used.</span></span>

* <span data-ttu-id="fc822-173">Generación de vínculos de una acción en un controlador de área según a otra acción en un controlador diferente y **no** en un área.</span><span class="sxs-lookup"><span data-stu-id="fc822-173">Generating links from an action within an area based controller to another action on a different controller and **not** in an area.</span></span>

  <span data-ttu-id="fc822-174">Sintaxis de HtmlHelper:`@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Home", new { area = "" })`</span><span class="sxs-lookup"><span data-stu-id="fc822-174">HtmlHelper syntax: `@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Home", new { area = "" })`</span></span>

  <span data-ttu-id="fc822-175">Sintaxis de TagHelper:`<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="fc822-175">TagHelper syntax: `<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`</span></span>

  <span data-ttu-id="fc822-176">Puesto que deseamos generar vínculos a un área no basada en la acción de controlador, se vacía el valor ambiente para 'área'.</span><span class="sxs-lookup"><span data-stu-id="fc822-176">Since we want to generate links to a non-area based controller action, we empty the ambient value for 'area' here.</span></span>

## <a name="publishing-areas"></a><span data-ttu-id="fc822-177">Áreas de publicación</span><span class="sxs-lookup"><span data-stu-id="fc822-177">Publishing Areas</span></span>

<span data-ttu-id="fc822-178">Todos los `*.cshtml` y `wwwroot/**` los archivos se publican para la salida cuando `<Project Sdk="Microsoft.NET.Sdk.Web">` se incluye en el *.csproj* archivo.</span><span class="sxs-lookup"><span data-stu-id="fc822-178">All `*.cshtml` and `wwwroot/**` files are published to output when `<Project Sdk="Microsoft.NET.Sdk.Web">` is included in the *.csproj* file.</span></span>
