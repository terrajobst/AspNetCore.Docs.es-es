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
ms.openlocfilehash: 3096d6404ff9c7e34eefcfb1990e7bf1ccab27ba
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2017
---
# <a name="areas"></a>Áreas

Por [Dhananjay Kumar](https://twitter.com/debug_mode) y [Rick Anderson](https://twitter.com/RickAndMSFT)

Las áreas son una característica de ASP.NET MVC que se usa para organizar la funcionalidad relacionada en un grupo como un espacio de nombres independiente (para el enrutamiento) y la estructura de carpetas (para vistas). El uso de áreas crea una jerarquía con el fin de enrutamiento mediante la adición de otro parámetro de ruta, `area`a `controller` y `action`.

Las áreas permiten dividir una aplicación Web de MVC de ASP.NET Core grande en agrupaciones funcionales más pequeñas. Un área de forma eficaz es una estructura MVC dentro de una aplicación. En un proyecto MVC, los componentes lógicos como modelo, el controlador y la vista se guardan en carpetas diferentes y MVC usa las convenciones de nomenclatura para crear la relación entre estos componentes. Para una aplicación grande, puede ser conveniente dividir la aplicación en distintas áreas de nivel alto de funcionalidad. Por ejemplo, una aplicación de comercio electrónico con varias unidades de negocio, como la desprotección, facturación y búsqueda etcetera. Cada una de estas unidades tienen sus propias vistas de componente lógico, controladores y modelos. En este escenario, puede utilizar las áreas para la partición físicamente de los componentes empresariales en el mismo proyecto.

Un área puede definirse como funcionales unidades más pequeñas en un proyecto de MVC de ASP.NET Core con su propio conjunto de modelos, vistas y controladores.

Considere el uso de las áreas en un MVC proyecto cuando:

* La aplicación está formada por varios componentes funcionales de alto nivel que deben ir separados lógicamente

* Desea crear particiones en el proyecto MVC para que cada área funcional pueda encargarse de forma independiente

Características de área:

* Una aplicación de MVC de ASP.NET Core puede tener cualquier número de áreas

* Cada área tiene sus propios controladores, modelos y vistas

* Le permite organizar los proyectos MVC grandes en varios componentes de alto nivel que pueden trabajar de forma independiente

* Admite varios controladores con el mismo nombre - siempre que tienen diferentes *áreas*

¡Eche un vistazo a un ejemplo para ilustrar cómo se crean y utilizan las áreas. Supongamos que tiene una aplicación de la tienda que tiene dos grupos distintos de controladores y vistas: productos y servicios. Una carpeta típica de la estructura para que el uso de áreas MVC es similar a continuación:

* Nombre de proyecto

  * Áreas

    * Productos

      * Controladores

        * HomeController.cs

        * ManageController.cs

      * Vistas

        * Página principal

          * Index.cshtml

        * Administrar

          * Index.cshtml

    * Servicios

      * Controladores

        * HomeController.cs

      * Vistas

        * Página principal

          * Index.cshtml

Cuando trata de MVC representar una vista en un área, de forma predeterminada, intenta buscar en las siguientes ubicaciones:

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
   /Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
   /Views/Shared/<Action-Name>.cshtml
   ```

Éstas son las ubicaciones predeterminadas que se pueden cambiar a través de la `AreaViewLocationFormats` en el `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.

Por ejemplo, en el siguiente código en lugar de tener el nombre de la carpeta como 'Áreas', se ha cambiado a 'Categorías'.

```csharp
services.Configure<RazorViewEngineOptions>(options =>
   {
       options.AreaViewLocationFormats.Clear();
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/{1}/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/Shared/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Views/Shared/{0}.cshtml");
   });
   ```

Hay que destacar es que la estructura de la *vistas* carpeta es la única que se considera importante aquí y el contenido del resto de las carpetas, como *controladores* y *modelos* does **no** importa. Por ejemplo, no necesita tener un *controladores* y *modelos* carpeta en absoluto. Esto funciona porque el contenido de *controladores* y *modelos* es código que se compila en una .dll, mientras que el contenido de la *vistas* no es hasta que una solicitud para que se ha realizado la vista.

Una vez que haya definido la jerarquía de carpetas, se debe indicar MVC que cada controlador está asociado a un área. Para hacerlo, decorando el nombre del controlador con la `[Area]` atributo.

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

Establecer una definición de ruta que funcione con sus áreas recién creados. El [enrutamiento a las acciones del controlador](routing.md) artículo entra en detalles acerca de cómo crear definiciones de route, incluido el uso de rutas convencionales frente a las rutas de atributo. En este ejemplo, vamos a usar una ruta convencional. Para ello, abra el *Startup.cs* archivo y modificarlo agregándole el `areaRoute` con el nombre de definición de ruta más adelante.

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

Vaya a `http://<yourApp>/products`, el `Index` método de acción de la `HomeController` en la `Products` área se invocará.

## <a name="link-generation"></a>Generación de vínculo

* Generar vínculos de una acción en un área en función de controlador a otra acción en el mismo controlador.

  Supongamos que la ruta de acceso de la solicitud actual es como`/Products/Home/Create`

  Sintaxis de HtmlHelper:`@Html.ActionLink("Go to Product's Home Page", "Index")`

  Sintaxis de TagHelper:`<a asp-action="Index">Go to Product's Home Page</a>`

  Tenga en cuenta que no se necesitamos proporcionar los valores 'área' y 'controlador' aquí y ya están disponibles en el contexto de la solicitud actual. Estos tipos de valores se denominan `ambient` valores.

* Generar vínculos de una acción en un área en la función de controlador a otra acción en un controlador diferente

  Supongamos que la ruta de acceso de la solicitud actual es como`/Products/Home/Create`

  Sintaxis de HtmlHelper:`@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Manage")`

  Sintaxis de TagHelper:`<a asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`

  Tenga en cuenta que aquí se utiliza el valor ambiente de un área de' ', pero se especifica explícitamente el valor de 'controlador' anteriormente.

* Generar vínculos de una acción dentro de un área según controlador a otra acción en un controlador diferente y un área diferente.

  Supongamos que la ruta de acceso de la solicitud actual es como`/Products/Home/Create`

  Sintaxis de HtmlHelper:`@Html.ActionLink("Go to Services’ Home Page", "Index", "Home", new { area = "Services" })`

  Sintaxis de TagHelper:`<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services’ Home Page</a>`

  Tenga en cuenta que aquí no se utiliza ningún valor ambiente.

* Generación de vínculos de una acción en un controlador de área según a otra acción en un controlador diferente y **no** en un área.

  Sintaxis de HtmlHelper:`@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Home", new { area = "" })`

  Sintaxis de TagHelper:`<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`

  Puesto que deseamos generar vínculos a un área no basada en la acción de controlador, se vacía el valor ambiente para 'área'.

## <a name="publishing-areas"></a>Áreas de publicación

Todos los `*.cshtml` y `wwwroot/**` los archivos se publican para la salida cuando `<Project Sdk="Microsoft.NET.Sdk.Web">` se incluye en el *.csproj* archivo.
