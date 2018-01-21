---
title: "Áreas"
author: rick-anderson
description: "Muestra cómo trabajar con áreas."
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/areas
ms.openlocfilehash: 666be2da6b38ffb538ae3888ea879a4104c8fd12
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="areas"></a><span data-ttu-id="cb27d-103">Áreas</span><span class="sxs-lookup"><span data-stu-id="cb27d-103">Areas</span></span>

<span data-ttu-id="cb27d-104">Por [Dhananjay Kumar](https://twitter.com/debug_mode) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="cb27d-104">By [Dhananjay Kumar](https://twitter.com/debug_mode)  and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="cb27d-105">Las áreas son una característica de ASP.NET MVC que se usa para organizar la funcionalidad relacionada en un grupo como un espacio de nombres independiente (para el enrutamiento) y la estructura de carpetas (para vistas).</span><span class="sxs-lookup"><span data-stu-id="cb27d-105">Areas are an ASP.NET MVC feature used to organize related functionality into a group as a separate namespace (for routing) and folder structure (for views).</span></span> <span data-ttu-id="cb27d-106">El uso de áreas crea una jerarquía con el fin de enrutamiento mediante la adición de otro parámetro de ruta, `area`a `controller` y `action`.</span><span class="sxs-lookup"><span data-stu-id="cb27d-106">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area`, to `controller` and `action`.</span></span>

<span data-ttu-id="cb27d-107">Las áreas permiten dividir una aplicación Web de MVC de ASP.NET Core grande en agrupaciones funcionales más pequeñas.</span><span class="sxs-lookup"><span data-stu-id="cb27d-107">Areas provide a way to partition a large ASP.NET Core MVC Web app into smaller functional groupings.</span></span> <span data-ttu-id="cb27d-108">Un área de forma eficaz es una estructura MVC dentro de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="cb27d-108">An area is effectively an MVC structure inside an application.</span></span> <span data-ttu-id="cb27d-109">En un proyecto MVC, los componentes lógicos como modelo, el controlador y la vista se guardan en carpetas diferentes y MVC usa las convenciones de nomenclatura para crear la relación entre estos componentes.</span><span class="sxs-lookup"><span data-stu-id="cb27d-109">In an MVC project, logical components like Model, Controller, and View are kept in different folders, and MVC uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="cb27d-110">Para una aplicación grande, puede ser conveniente dividir la aplicación en distintas áreas de nivel alto de funcionalidad.</span><span class="sxs-lookup"><span data-stu-id="cb27d-110">For a large app, it may be advantageous to partition the  app into separate high level areas of functionality.</span></span> <span data-ttu-id="cb27d-111">Por ejemplo, una aplicación de comercio electrónico con varias unidades de negocio, como la desprotección, facturación y búsqueda etcetera. Cada una de estas unidades tienen sus propias vistas de componente lógico, controladores y modelos.</span><span class="sxs-lookup"><span data-stu-id="cb27d-111">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search etc. Each of these units have their own logical component views, controllers, and models.</span></span> <span data-ttu-id="cb27d-112">En este escenario, puede utilizar las áreas para la partición físicamente de los componentes empresariales en el mismo proyecto.</span><span class="sxs-lookup"><span data-stu-id="cb27d-112">In this scenario, you can use Areas to physically partition the business components in the same project.</span></span>

<span data-ttu-id="cb27d-113">Un área puede definirse como funcionales unidades más pequeñas en un proyecto de MVC de ASP.NET Core con su propio conjunto de modelos, vistas y controladores.</span><span class="sxs-lookup"><span data-stu-id="cb27d-113">An area can be defined as smaller functional units in an ASP.NET Core MVC project with its own set of controllers, views, and models.</span></span>

<span data-ttu-id="cb27d-114">Considere el uso de las áreas en un MVC proyecto cuando:</span><span class="sxs-lookup"><span data-stu-id="cb27d-114">Consider using Areas in an MVC project when:</span></span>

* <span data-ttu-id="cb27d-115">La aplicación está formada por varios componentes funcionales de alto nivel que deben ir separados lógicamente</span><span class="sxs-lookup"><span data-stu-id="cb27d-115">Your application is made of multiple high-level functional components that should be logically separated</span></span>

* <span data-ttu-id="cb27d-116">Desea crear particiones en el proyecto MVC para que cada área funcional pueda encargarse de forma independiente</span><span class="sxs-lookup"><span data-stu-id="cb27d-116">You want to partition your MVC project so that each functional area can be worked on independently</span></span>

<span data-ttu-id="cb27d-117">Características de área:</span><span class="sxs-lookup"><span data-stu-id="cb27d-117">Area features:</span></span>

* <span data-ttu-id="cb27d-118">Una aplicación de MVC de ASP.NET Core puede tener cualquier número de áreas</span><span class="sxs-lookup"><span data-stu-id="cb27d-118">An ASP.NET Core MVC app can have any number of areas</span></span>

* <span data-ttu-id="cb27d-119">Cada área tiene sus propios controladores, modelos y vistas</span><span class="sxs-lookup"><span data-stu-id="cb27d-119">Each area has its own controllers, models, and views</span></span>

* <span data-ttu-id="cb27d-120">Le permite organizar los proyectos MVC grandes en varios componentes de alto nivel que pueden trabajar de forma independiente</span><span class="sxs-lookup"><span data-stu-id="cb27d-120">Allows you to organize large MVC projects into multiple high-level components that can be worked on independently</span></span>

* <span data-ttu-id="cb27d-121">Admite varios controladores con el mismo nombre - siempre que tienen diferentes *áreas*</span><span class="sxs-lookup"><span data-stu-id="cb27d-121">Supports multiple controllers with the same name - as long as they have different *areas*</span></span>

<span data-ttu-id="cb27d-122">¡Eche un vistazo a un ejemplo para ilustrar cómo se crean y utilizan las áreas.</span><span class="sxs-lookup"><span data-stu-id="cb27d-122">Let's take a look at an example to illustrate how Areas are created and used.</span></span> <span data-ttu-id="cb27d-123">Supongamos que tiene una aplicación de la tienda que tiene dos grupos distintos de controladores y vistas: productos y servicios.</span><span class="sxs-lookup"><span data-stu-id="cb27d-123">Let's say you have a store app that has two distinct groupings of controllers and views: Products and Services.</span></span> <span data-ttu-id="cb27d-124">Una carpeta típica de la estructura para que el uso de áreas MVC es similar a continuación:</span><span class="sxs-lookup"><span data-stu-id="cb27d-124">A typical folder structure for that using MVC areas looks like below:</span></span>

* <span data-ttu-id="cb27d-125">Nombre de proyecto</span><span class="sxs-lookup"><span data-stu-id="cb27d-125">Project name</span></span>

  * <span data-ttu-id="cb27d-126">Áreas</span><span class="sxs-lookup"><span data-stu-id="cb27d-126">Areas</span></span>

    * <span data-ttu-id="cb27d-127">Productos</span><span class="sxs-lookup"><span data-stu-id="cb27d-127">Products</span></span>

      * <span data-ttu-id="cb27d-128">Controladores</span><span class="sxs-lookup"><span data-stu-id="cb27d-128">Controllers</span></span>

        * <span data-ttu-id="cb27d-129">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="cb27d-129">HomeController.cs</span></span>

        * <span data-ttu-id="cb27d-130">ManageController.cs</span><span class="sxs-lookup"><span data-stu-id="cb27d-130">ManageController.cs</span></span>

      * <span data-ttu-id="cb27d-131">Vistas</span><span class="sxs-lookup"><span data-stu-id="cb27d-131">Views</span></span>

        * <span data-ttu-id="cb27d-132">Página principal</span><span class="sxs-lookup"><span data-stu-id="cb27d-132">Home</span></span>

          * <span data-ttu-id="cb27d-133">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="cb27d-133">Index.cshtml</span></span>

        * <span data-ttu-id="cb27d-134">Administrar</span><span class="sxs-lookup"><span data-stu-id="cb27d-134">Manage</span></span>

          * <span data-ttu-id="cb27d-135">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="cb27d-135">Index.cshtml</span></span>

    * <span data-ttu-id="cb27d-136">Servicios</span><span class="sxs-lookup"><span data-stu-id="cb27d-136">Services</span></span>

      * <span data-ttu-id="cb27d-137">Controladores</span><span class="sxs-lookup"><span data-stu-id="cb27d-137">Controllers</span></span>

        * <span data-ttu-id="cb27d-138">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="cb27d-138">HomeController.cs</span></span>

      * <span data-ttu-id="cb27d-139">Vistas</span><span class="sxs-lookup"><span data-stu-id="cb27d-139">Views</span></span>

        * <span data-ttu-id="cb27d-140">Página principal</span><span class="sxs-lookup"><span data-stu-id="cb27d-140">Home</span></span>

          * <span data-ttu-id="cb27d-141">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="cb27d-141">Index.cshtml</span></span>

<span data-ttu-id="cb27d-142">Cuando trata de MVC representar una vista en un área, de forma predeterminada, intenta buscar en las siguientes ubicaciones:</span><span class="sxs-lookup"><span data-stu-id="cb27d-142">When MVC tries to render a view in an Area, by default, it tries to look in the following locations:</span></span>

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
   /Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
   /Views/Shared/<Action-Name>.cshtml
   ```

<span data-ttu-id="cb27d-143">Éstas son las ubicaciones predeterminadas que se pueden cambiar a través de la `AreaViewLocationFormats` en el `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.</span><span class="sxs-lookup"><span data-stu-id="cb27d-143">These are the default locations which can be changed via the `AreaViewLocationFormats` on the `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.</span></span>

<span data-ttu-id="cb27d-144">Por ejemplo, en el siguiente código en lugar de tener el nombre de la carpeta como 'Áreas', se ha cambiado a 'Categorías'.</span><span class="sxs-lookup"><span data-stu-id="cb27d-144">For example, in the below code instead of having the folder name as 'Areas', it has been changed to 'Categories'.</span></span>

```csharp
services.Configure<RazorViewEngineOptions>(options =>
   {
       options.AreaViewLocationFormats.Clear();
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/{1}/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/Shared/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Views/Shared/{0}.cshtml");
   });
   ```

<span data-ttu-id="cb27d-145">Hay que destacar es que la estructura de la *vistas* carpeta es la única que se considera importante aquí y el contenido del resto de las carpetas, como *controladores* y *modelos* does **no** importa.</span><span class="sxs-lookup"><span data-stu-id="cb27d-145">One thing to note is that the structure of the *Views* folder is the only one which is considered important here and the content of the rest of the folders like *Controllers* and *Models* does **not** matter.</span></span> <span data-ttu-id="cb27d-146">Por ejemplo, no necesita tener un *controladores* y *modelos* carpeta en absoluto.</span><span class="sxs-lookup"><span data-stu-id="cb27d-146">For example, you need not have a *Controllers* and *Models* folder at all.</span></span> <span data-ttu-id="cb27d-147">Esto funciona porque el contenido de *controladores* y *modelos* es código que se compila en una .dll, mientras que el contenido de la *vistas* no es hasta que una solicitud para que se ha realizado la vista.</span><span class="sxs-lookup"><span data-stu-id="cb27d-147">This works because the content of *Controllers* and *Models* is just code which gets compiled into a .dll where as the content of the *Views* is not until a request to that view has been made.</span></span>

<span data-ttu-id="cb27d-148">Una vez que haya definido la jerarquía de carpetas, se debe indicar MVC que cada controlador está asociado a un área.</span><span class="sxs-lookup"><span data-stu-id="cb27d-148">Once you've defined the folder hierarchy, you need to tell MVC that each controller is associated with an area.</span></span> <span data-ttu-id="cb27d-149">Para hacerlo, decorando el nombre del controlador con la `[Area]` atributo.</span><span class="sxs-lookup"><span data-stu-id="cb27d-149">You do that by decorating the controller name with the `[Area]` attribute.</span></span>

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

<span data-ttu-id="cb27d-150">Establecer una definición de ruta que funcione con sus áreas recién creados.</span><span class="sxs-lookup"><span data-stu-id="cb27d-150">Set up a route definition that works with your newly created areas.</span></span> <span data-ttu-id="cb27d-151">El [enrutamiento a las acciones del controlador](routing.md) artículo entra en detalles acerca de cómo crear definiciones de route, incluido el uso de rutas convencionales frente a las rutas de atributo.</span><span class="sxs-lookup"><span data-stu-id="cb27d-151">The [Routing to Controller Actions](routing.md) article goes into detail about how to create route definitions, including using conventional routes versus attribute routes.</span></span> <span data-ttu-id="cb27d-152">En este ejemplo, vamos a usar una ruta convencional.</span><span class="sxs-lookup"><span data-stu-id="cb27d-152">In this example, we'll use a conventional route.</span></span> <span data-ttu-id="cb27d-153">Para ello, abra el *Startup.cs* archivo y modificarlo agregándole el `areaRoute` con el nombre de definición de ruta más adelante.</span><span class="sxs-lookup"><span data-stu-id="cb27d-153">To do so, open the *Startup.cs* file and modify it by adding the `areaRoute` named route definition below.</span></span>

```csharp
...
   app.UseMvc(routes =>
   {
     routes.MapRoute(
         name: "areaRoute",
         template: "{area:exists}/{controller=Home}/{action=Index}/{id?}");

     routes.MapRoute(
         name: "default",
         template: "{controller=Home}/{action=Index}/{id?}");
   });
   ```

<span data-ttu-id="cb27d-154">Vaya a `http://<yourApp>/products`, el `Index` método de acción de la `HomeController` en la `Products` área se invocará.</span><span class="sxs-lookup"><span data-stu-id="cb27d-154">Browsing to `http://<yourApp>/products`, the `Index` action method of the `HomeController` in the `Products` area will be invoked.</span></span>

## <a name="link-generation"></a><span data-ttu-id="cb27d-155">Generación de vínculo</span><span class="sxs-lookup"><span data-stu-id="cb27d-155">Link Generation</span></span>

* <span data-ttu-id="cb27d-156">Generar vínculos de una acción en un área en función de controlador a otra acción en el mismo controlador.</span><span class="sxs-lookup"><span data-stu-id="cb27d-156">Generating links from an action within an area based controller to another action within the same controller.</span></span>

  <span data-ttu-id="cb27d-157">Supongamos que la ruta de acceso de la solicitud actual es como`/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="cb27d-157">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="cb27d-158">Sintaxis de HtmlHelper:`@Html.ActionLink("Go to Product's Home Page", "Index")`</span><span class="sxs-lookup"><span data-stu-id="cb27d-158">HtmlHelper syntax: `@Html.ActionLink("Go to Product's Home Page", "Index")`</span></span>

  <span data-ttu-id="cb27d-159">Sintaxis de TagHelper:`<a asp-action="Index">Go to Product's Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="cb27d-159">TagHelper syntax: `<a asp-action="Index">Go to Product's Home Page</a>`</span></span>

  <span data-ttu-id="cb27d-160">Tenga en cuenta que no se necesitamos proporcionar los valores 'área' y 'controlador' aquí y ya están disponibles en el contexto de la solicitud actual.</span><span class="sxs-lookup"><span data-stu-id="cb27d-160">Note that we need not supply the 'area' and 'controller' values here as they are already available in the context of the current request.</span></span> <span data-ttu-id="cb27d-161">Estos tipos de valores se denominan `ambient` valores.</span><span class="sxs-lookup"><span data-stu-id="cb27d-161">These kind of values are called `ambient` values.</span></span>

* <span data-ttu-id="cb27d-162">Generar vínculos de una acción en un área en la función de controlador a otra acción en un controlador diferente</span><span class="sxs-lookup"><span data-stu-id="cb27d-162">Generating links from an action within an area based controller to another action on a different controller</span></span>

  <span data-ttu-id="cb27d-163">Supongamos que la ruta de acceso de la solicitud actual es como`/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="cb27d-163">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="cb27d-164">Sintaxis de HtmlHelper:`@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Manage")`</span><span class="sxs-lookup"><span data-stu-id="cb27d-164">HtmlHelper syntax: `@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Manage")`</span></span>

  <span data-ttu-id="cb27d-165">Sintaxis de TagHelper:`<a asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="cb27d-165">TagHelper syntax: `<a asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`</span></span>

  <span data-ttu-id="cb27d-166">Tenga en cuenta que aquí se utiliza el valor ambiente de un área de' ', pero se especifica explícitamente el valor de 'controlador' anteriormente.</span><span class="sxs-lookup"><span data-stu-id="cb27d-166">Note that here the ambient value of an 'area' is used but the 'controller' value is specified explicitly above.</span></span>

* <span data-ttu-id="cb27d-167">Generar vínculos de una acción dentro de un área según controlador a otra acción en un controlador diferente y un área diferente.</span><span class="sxs-lookup"><span data-stu-id="cb27d-167">Generating links from an action within an area based controller to another action on a different controller and a different area.</span></span>

  <span data-ttu-id="cb27d-168">Supongamos que la ruta de acceso de la solicitud actual es como`/Products/Home/Create`</span><span class="sxs-lookup"><span data-stu-id="cb27d-168">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="cb27d-169">Sintaxis de HtmlHelper:`@Html.ActionLink("Go to Services’ Home Page", "Index", "Home", new { area = "Services" })`</span><span class="sxs-lookup"><span data-stu-id="cb27d-169">HtmlHelper syntax: `@Html.ActionLink("Go to Services’ Home Page", "Index", "Home", new { area = "Services" })`</span></span>

  <span data-ttu-id="cb27d-170">Sintaxis de TagHelper:`<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services’ Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="cb27d-170">TagHelper syntax: `<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services’ Home Page</a>`</span></span>

  <span data-ttu-id="cb27d-171">Tenga en cuenta que aquí no se utiliza ningún valor ambiente.</span><span class="sxs-lookup"><span data-stu-id="cb27d-171">Note that here no ambient values are used.</span></span>

* <span data-ttu-id="cb27d-172">Generación de vínculos de una acción en un controlador de área según a otra acción en un controlador diferente y **no** en un área.</span><span class="sxs-lookup"><span data-stu-id="cb27d-172">Generating links from an action within an area based controller to another action on a different controller and **not** in an area.</span></span>

  <span data-ttu-id="cb27d-173">Sintaxis de HtmlHelper:`@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Home", new { area = "" })`</span><span class="sxs-lookup"><span data-stu-id="cb27d-173">HtmlHelper syntax: `@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Home", new { area = "" })`</span></span>

  <span data-ttu-id="cb27d-174">Sintaxis de TagHelper:`<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="cb27d-174">TagHelper syntax: `<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`</span></span>

  <span data-ttu-id="cb27d-175">Puesto que deseamos generar vínculos a un área no basada en la acción de controlador, se vacía el valor ambiente para 'área'.</span><span class="sxs-lookup"><span data-stu-id="cb27d-175">Since we want to generate links to a non-area based controller action, we empty the ambient value for 'area' here.</span></span>

## <a name="publishing-areas"></a><span data-ttu-id="cb27d-176">Áreas de publicación</span><span class="sxs-lookup"><span data-stu-id="cb27d-176">Publishing Areas</span></span>

<span data-ttu-id="cb27d-177">Todos los `*.cshtml` y `wwwroot/**` los archivos se publican para la salida cuando `<Project Sdk="Microsoft.NET.Sdk.Web">` se incluye en el *.csproj* archivo.</span><span class="sxs-lookup"><span data-stu-id="cb27d-177">All `*.cshtml` and `wwwroot/**` files are published to output when `<Project Sdk="Microsoft.NET.Sdk.Web">` is included in the *.csproj* file.</span></span>
