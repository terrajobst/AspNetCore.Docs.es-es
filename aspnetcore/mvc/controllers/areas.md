---
title: Áreas de ASP.NET Core
author: rick-anderson
description: Obtenga información sobre las áreas, una característica de ASP.NET MVC que se usa para organizar funciones relacionadas en un grupo como un espacio de nombres independiente (para el enrutamiento) y una estructura de carpetas (para las vistas).
ms.author: riande
ms.date: 02/14/2017
uid: mvc/controllers/areas
ms.openlocfilehash: 19e818fa198936ea1bee0da8039e88a3c0abbf6b
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410617"
---
# <a name="areas-in-aspnet-core"></a><span data-ttu-id="78e04-103">Áreas de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="78e04-103">Areas in ASP.NET Core</span></span>

<span data-ttu-id="78e04-104">Por [Dhananjay Kumar](https://twitter.com/debug_mode) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="78e04-104">By [Dhananjay Kumar](https://twitter.com/debug_mode)  and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="78e04-105">Las áreas son una característica de MVC de ASP.NET que se usa para organizar funciones relacionadas en un grupo como un espacio de nombres independiente (para el enrutamiento) y una estructura de carpetas (para las vistas).</span><span class="sxs-lookup"><span data-stu-id="78e04-105">Areas are an ASP.NET MVC feature used to organize related functionality into a group as a separate namespace (for routing) and folder structure (for views).</span></span> <span data-ttu-id="78e04-106">El uso de áreas crea una jerarquía para el enrutamiento mediante la adición de otro parámetro de ruta, `area`, a `controller` y `action`.</span><span class="sxs-lookup"><span data-stu-id="78e04-106">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area`, to `controller` and `action`.</span></span>

<span data-ttu-id="78e04-107">Las áreas ofrecen una manera de dividir una aplicación web ASP.NET Core MVC de gran tamaño en agrupaciones funcionales más pequeñas.</span><span class="sxs-lookup"><span data-stu-id="78e04-107">Areas provide a way to partition a large ASP.NET Core MVC Web app into smaller functional groupings.</span></span> <span data-ttu-id="78e04-108">Un área es en realidad una estructura de MVC dentro de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="78e04-108">An area is effectively an MVC structure inside an application.</span></span> <span data-ttu-id="78e04-109">En un proyecto de MVC, los componentes lógicos como el modelo, el controlador y la vista se guardan en carpetas diferentes, y MVC usa las convenciones de nomenclatura para crear la relación entre estos componentes.</span><span class="sxs-lookup"><span data-stu-id="78e04-109">In an MVC project, logical components like Model, Controller, and View are kept in different folders, and MVC uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="78e04-110">Para una aplicación grande, puede ser conveniente dividir la aplicación en distintas áreas de funciones de alto nivel.</span><span class="sxs-lookup"><span data-stu-id="78e04-110">For a large app, it may be advantageous to partition the  app into separate high level areas of functionality.</span></span> <span data-ttu-id="78e04-111">Por ejemplo, una aplicación de comercio electrónico con varias unidades de negocio, como la finalización de la compra, la facturación, la búsqueda, etc. Cada una de estas unidades tiene sus propias vistas, controladores y modelos de componentes lógicos.</span><span class="sxs-lookup"><span data-stu-id="78e04-111">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search etc. Each of these units have their own logical component views, controllers, and models.</span></span> <span data-ttu-id="78e04-112">En este escenario, puede usar las áreas para dividir físicamente los componentes empresariales del mismo proyecto.</span><span class="sxs-lookup"><span data-stu-id="78e04-112">In this scenario, you can use Areas to physically partition the business components in the same project.</span></span>

<span data-ttu-id="78e04-113">Un área puede definirse como unidades funcionales más pequeñas en un proyecto de ASP.NET Core MVC con su propio conjunto de modelos, vistas y controladores.</span><span class="sxs-lookup"><span data-stu-id="78e04-113">An area can be defined as smaller functional units in an ASP.NET Core MVC project with its own set of controllers, views, and models.</span></span>

<span data-ttu-id="78e04-114">Considere el uso de áreas en un proyecto de MVC en los casos siguientes:</span><span class="sxs-lookup"><span data-stu-id="78e04-114">Consider using Areas in an MVC project when:</span></span>

* <span data-ttu-id="78e04-115">La aplicación está formada por varios componentes funcionales de alto nivel que deben separarse lógicamente.</span><span class="sxs-lookup"><span data-stu-id="78e04-115">Your application is made of multiple high-level functional components that should be logically separated</span></span>

* <span data-ttu-id="78e04-116">Le interesa dividir el proyecto de MVC para que se pueda trabajar en cada área funcional de forma independiente.</span><span class="sxs-lookup"><span data-stu-id="78e04-116">You want to partition your MVC project so that each functional area can be worked on independently</span></span>

<span data-ttu-id="78e04-117">Características de las áreas:</span><span class="sxs-lookup"><span data-stu-id="78e04-117">Area features:</span></span>

* <span data-ttu-id="78e04-118">Una aplicación ASP.NET Core MVC puede tener cualquier número de áreas.</span><span class="sxs-lookup"><span data-stu-id="78e04-118">An ASP.NET Core MVC app can have any number of areas.</span></span>

* <span data-ttu-id="78e04-119">Cada área tiene sus propios controladores, modelos y vistas.</span><span class="sxs-lookup"><span data-stu-id="78e04-119">Each area has its own controllers, models, and views.</span></span>

* <span data-ttu-id="78e04-120">Las áreas permiten organizar proyectos de MVC de gran tamaño en varios componentes generales en los que se puede trabajar de forma independiente.</span><span class="sxs-lookup"><span data-stu-id="78e04-120">Areas allow you to organize large MVC projects into multiple high-level components that can be worked on independently.</span></span>

* <span data-ttu-id="78e04-121">Las áreas admiten varios controladores con el mismo nombre, siempre y cuando tengan *áreas* diferentes.</span><span class="sxs-lookup"><span data-stu-id="78e04-121">Areas support multiple controllers with the same name, as long as they have different *areas*.</span></span>

<span data-ttu-id="78e04-122">Veamos un ejemplo para ilustrar cómo se crean y se usan las áreas.</span><span class="sxs-lookup"><span data-stu-id="78e04-122">Let's take a look at an example to illustrate how Areas are created and used.</span></span> <span data-ttu-id="78e04-123">Supongamos que tiene una aplicación de tienda con dos grupos distintos de controladores y vistas: Productos y Servicios.</span><span class="sxs-lookup"><span data-stu-id="78e04-123">Let's say you have a store app that has two distinct groupings of controllers and views: Products and Services.</span></span> <span data-ttu-id="78e04-124">Una estructura de carpetas típica para dicha aplicación con áreas de MVC tendría un aspecto similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="78e04-124">A typical folder structure for that using MVC areas looks like below:</span></span>

* <span data-ttu-id="78e04-125">Nombre del proyecto</span><span class="sxs-lookup"><span data-stu-id="78e04-125">Project name</span></span>
  * <span data-ttu-id="78e04-126">Áreas</span><span class="sxs-lookup"><span data-stu-id="78e04-126">Areas</span></span>
    * <span data-ttu-id="78e04-127">Productos</span><span class="sxs-lookup"><span data-stu-id="78e04-127">Products</span></span>
      * <span data-ttu-id="78e04-128">Controladores</span><span class="sxs-lookup"><span data-stu-id="78e04-128">Controllers</span></span>
        * <span data-ttu-id="78e04-129">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="78e04-129">HomeController.cs</span></span>
        * <span data-ttu-id="78e04-130">ManageController.cs</span><span class="sxs-lookup"><span data-stu-id="78e04-130">ManageController.cs</span></span>
      * <span data-ttu-id="78e04-131">Vistas</span><span class="sxs-lookup"><span data-stu-id="78e04-131">Views</span></span>
        * <span data-ttu-id="78e04-132">Página principal</span><span class="sxs-lookup"><span data-stu-id="78e04-132">Home</span></span>
          * <span data-ttu-id="78e04-133">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="78e04-133">Index.cshtml</span></span>
        * <span data-ttu-id="78e04-134">Administrar</span><span class="sxs-lookup"><span data-stu-id="78e04-134">Manage</span></span>
          * <span data-ttu-id="78e04-135">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="78e04-135">Index.cshtml</span></span>
    * <span data-ttu-id="78e04-136">Servicios</span><span class="sxs-lookup"><span data-stu-id="78e04-136">Services</span></span>
      * <span data-ttu-id="78e04-137">Controladores</span><span class="sxs-lookup"><span data-stu-id="78e04-137">Controllers</span></span>
        * <span data-ttu-id="78e04-138">HomeController.cs</span><span class="sxs-lookup"><span data-stu-id="78e04-138">HomeController.cs</span></span>
      * <span data-ttu-id="78e04-139">Vistas</span><span class="sxs-lookup"><span data-stu-id="78e04-139">Views</span></span>
        * <span data-ttu-id="78e04-140">Página principal</span><span class="sxs-lookup"><span data-stu-id="78e04-140">Home</span></span>
          * <span data-ttu-id="78e04-141">Index.cshtml</span><span class="sxs-lookup"><span data-stu-id="78e04-141">Index.cshtml</span></span>

<span data-ttu-id="78e04-142">Cuando MVC intenta representar una vista en un área, de forma predeterminada, busca en las ubicaciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="78e04-142">When MVC tries to render a view in an Area, by default, it tries to look in the following locations:</span></span>

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
   /Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
   /Views/Shared/<Action-Name>.cshtml
   ```

<span data-ttu-id="78e04-143">Estas son las ubicaciones predeterminadas que se pueden cambiar mediante `AreaViewLocationFormats` en `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.</span><span class="sxs-lookup"><span data-stu-id="78e04-143">These are the default locations which can be changed via the `AreaViewLocationFormats` on the `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.</span></span>

<span data-ttu-id="78e04-144">Por ejemplo, en el código siguiente, el nombre de carpeta "Areas" se ha cambiado a "Categories".</span><span class="sxs-lookup"><span data-stu-id="78e04-144">For example, in the below code instead of having the folder name as 'Areas', it has been changed to 'Categories'.</span></span>

```csharp
services.Configure<RazorViewEngineOptions>(options =>
   {
       options.AreaViewLocationFormats.Clear();
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/{1}/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/Shared/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Views/Shared/{0}.cshtml");
   });
   ```

<span data-ttu-id="78e04-145">Hay que tener en cuenta que la estructura de la carpeta *Views* es la única que se considera importante aquí; el contenido de las demás carpetas, como *Controllers* y *Models*, **no** importa.</span><span class="sxs-lookup"><span data-stu-id="78e04-145">One thing to note is that the structure of the *Views* folder is the only one which is considered important here and the content of the rest of the folders like *Controllers* and *Models* does **not** matter.</span></span> <span data-ttu-id="78e04-146">Por ejemplo, no hace falta tener una carpeta *Controllers* y *Models*.</span><span class="sxs-lookup"><span data-stu-id="78e04-146">For example, you need not have a *Controllers* and *Models* folder at all.</span></span> <span data-ttu-id="78e04-147">Esto funciona porque el contenido de *Controllers* y *Models* solo es código que se compila en una .dll, mientras que el contenido de *Views* no lo es mientras no se realice una solicitud a esa vista.</span><span class="sxs-lookup"><span data-stu-id="78e04-147">This works because the content of *Controllers* and *Models* is just code which gets compiled into a .dll where as the content of the *Views* isn't until a request to that view has been made.</span></span>

<span data-ttu-id="78e04-148">Una vez que haya definido la jerarquía de carpetas, debe indicar a MVC que cada controlador está asociado a un área.</span><span class="sxs-lookup"><span data-stu-id="78e04-148">Once you've defined the folder hierarchy, you need to tell MVC that each controller is associated with an area.</span></span> <span data-ttu-id="78e04-149">Para hacerlo, decore el nombre del controlador con el atributo `[Area]`.</span><span class="sxs-lookup"><span data-stu-id="78e04-149">You do that by decorating the controller name with the `[Area]` attribute.</span></span>

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

<span data-ttu-id="78e04-150">Establezca una definición de ruta que funcione con las áreas recién creadas.</span><span class="sxs-lookup"><span data-stu-id="78e04-150">Set up a route definition that works with your newly created areas.</span></span> <span data-ttu-id="78e04-151">En el artículo [Enrutamiento a acciones del controlador](routing.md) se explica en detalle cómo crear definiciones de ruta, incluido el uso de rutas convencionales frente a rutas de atributo.</span><span class="sxs-lookup"><span data-stu-id="78e04-151">The [Route to controller actions](routing.md) article goes into detail about how to create route definitions, including using conventional routes versus attribute routes.</span></span> <span data-ttu-id="78e04-152">En este ejemplo usaremos una ruta convencional.</span><span class="sxs-lookup"><span data-stu-id="78e04-152">In this example, we'll use a conventional route.</span></span> <span data-ttu-id="78e04-153">Para ello, abra el archivo *Startup.cs* y modifíquelo mediante la adición de la definición de ruta con nombre `areaRoute` que se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="78e04-153">To do so, open the *Startup.cs* file and modify it by adding the `areaRoute` named route definition below.</span></span>

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

<span data-ttu-id="78e04-154">Si va a `http://<yourApp>/products`, se invocará el método de acción `Index` de `HomeController` en el área `Products`.</span><span class="sxs-lookup"><span data-stu-id="78e04-154">Browsing to `http://<yourApp>/products`, the `Index` action method of the `HomeController` in the `Products` area will be invoked.</span></span>

## <a name="link-generation"></a><span data-ttu-id="78e04-155">Generación de vínculos</span><span class="sxs-lookup"><span data-stu-id="78e04-155">Link Generation</span></span>

* <span data-ttu-id="78e04-156">Generación de vínculos desde una acción dentro de un controlador basado en área en otra acción dentro del mismo controlador.</span><span class="sxs-lookup"><span data-stu-id="78e04-156">Generating links from an action within an area based controller to another action within the same controller.</span></span>

  <span data-ttu-id="78e04-157">Supongamos que la ruta de la solicitud actual es `/Products/Home/Create`.</span><span class="sxs-lookup"><span data-stu-id="78e04-157">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="78e04-158">Sintaxis de HtmlHelper: `@Html.ActionLink("Go to Product's Home Page", "Index")`</span><span class="sxs-lookup"><span data-stu-id="78e04-158">HtmlHelper syntax: `@Html.ActionLink("Go to Product's Home Page", "Index")`</span></span>

  <span data-ttu-id="78e04-159">Sintaxis de TagHelper: `<a asp-action="Index">Go to Product's Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="78e04-159">TagHelper syntax: `<a asp-action="Index">Go to Product's Home Page</a>`</span></span>

  <span data-ttu-id="78e04-160">Tenga en cuenta que es necesario proporcionar aquí los valores de área y controlador, dado que ya están disponibles en el contexto de la solicitud actual.</span><span class="sxs-lookup"><span data-stu-id="78e04-160">Note that we need not supply the 'area' and 'controller' values here as they're already available in the context of the current request.</span></span> <span data-ttu-id="78e04-161">Estos tipos de valores se denominan valores `ambient`.</span><span class="sxs-lookup"><span data-stu-id="78e04-161">These kind of values are called `ambient` values.</span></span>

* <span data-ttu-id="78e04-162">Generación de vínculos desde una acción dentro de un controlador basado en área en otra acción en un controlador diferente.</span><span class="sxs-lookup"><span data-stu-id="78e04-162">Generating links from an action within an area based controller to another action on a different controller</span></span>

  <span data-ttu-id="78e04-163">Supongamos que la ruta de la solicitud actual es `/Products/Home/Create`.</span><span class="sxs-lookup"><span data-stu-id="78e04-163">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="78e04-164">Sintaxis de HtmlHelper: `@Html.ActionLink("Go to Manage Products Home Page", "Index", "Manage")`</span><span class="sxs-lookup"><span data-stu-id="78e04-164">HtmlHelper syntax: `@Html.ActionLink("Go to Manage Products Home Page", "Index", "Manage")`</span></span>

  <span data-ttu-id="78e04-165">Sintaxis de TagHelper: `<a asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="78e04-165">TagHelper syntax: `<a asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`</span></span>

  <span data-ttu-id="78e04-166">Tenga en cuenta que aquí se usa el valor de ambiente de área, pero el valor de controlador se especifica explícitamente más arriba.</span><span class="sxs-lookup"><span data-stu-id="78e04-166">Note that here the ambient value of an 'area' is used but the 'controller' value is specified explicitly above.</span></span>

* <span data-ttu-id="78e04-167">Generación de vínculos desde una acción dentro de un controlador basado en área en otra acción en un controlador diferente y un área diferente.</span><span class="sxs-lookup"><span data-stu-id="78e04-167">Generating links from an action within an area based controller to another action on a different controller and a different area.</span></span>

  <span data-ttu-id="78e04-168">Supongamos que la ruta de la solicitud actual es `/Products/Home/Create`.</span><span class="sxs-lookup"><span data-stu-id="78e04-168">Let's say the current request's path is like `/Products/Home/Create`</span></span>

  <span data-ttu-id="78e04-169">Sintaxis de HtmlHelper: `@Html.ActionLink("Go to Services Home Page", "Index", "Home", new { area = "Services" })`</span><span class="sxs-lookup"><span data-stu-id="78e04-169">HtmlHelper syntax: `@Html.ActionLink("Go to Services Home Page", "Index", "Home", new { area = "Services" })`</span></span>

  <span data-ttu-id="78e04-170">Sintaxis de TagHelper: `<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="78e04-170">TagHelper syntax: `<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services Home Page</a>`</span></span>

  <span data-ttu-id="78e04-171">Tenga en cuenta que aquí no se usan valores de ambiente.</span><span class="sxs-lookup"><span data-stu-id="78e04-171">Note that here no ambient values are used.</span></span>

* <span data-ttu-id="78e04-172">Generación de vínculos desde una acción dentro de un controlador basado en área en otra acción en un controlador diferente y **no** en un área.</span><span class="sxs-lookup"><span data-stu-id="78e04-172">Generating links from an action within an area based controller to another action on a different controller and **not** in an area.</span></span>

  <span data-ttu-id="78e04-173">Sintaxis de HtmlHelper: `@Html.ActionLink("Go to Manage Products  Home Page", "Index", "Home", new { area = "" })`</span><span class="sxs-lookup"><span data-stu-id="78e04-173">HtmlHelper syntax: `@Html.ActionLink("Go to Manage Products  Home Page", "Index", "Home", new { area = "" })`</span></span>

  <span data-ttu-id="78e04-174">Sintaxis de TagHelper: `<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`</span><span class="sxs-lookup"><span data-stu-id="78e04-174">TagHelper syntax: `<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`</span></span>

  <span data-ttu-id="78e04-175">Dado que queremos generar vínculos en una acción de controlador que no está basada en un área, vaciaremos aquí el valor de ambiente para el área.</span><span class="sxs-lookup"><span data-stu-id="78e04-175">Since we want to generate links to a non-area based controller action, we empty the ambient value for 'area' here.</span></span>

## <a name="publishing-areas"></a><span data-ttu-id="78e04-176">Publicación de áreas</span><span class="sxs-lookup"><span data-stu-id="78e04-176">Publishing Areas</span></span>

<span data-ttu-id="78e04-177">Todos los archivos `*.cshtml` y `wwwroot/**` se publican en la salida cuando se incluye `<Project Sdk="Microsoft.NET.Sdk.Web">` en el archivo *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="78e04-177">All `*.cshtml` and `wwwroot/**` files are published to output when `<Project Sdk="Microsoft.NET.Sdk.Web">` is included in the *.csproj* file.</span></span>
