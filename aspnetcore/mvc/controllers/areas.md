---
title: Áreas de ASP.NET Core
author: rick-anderson
description: Obtenga información sobre las áreas, una característica de ASP.NET MVC que se usa para organizar funciones relacionadas en un grupo como un espacio de nombres independiente (para el enrutamiento) y una estructura de carpetas (para las vistas).
ms.author: riande
ms.date: 02/14/2017
uid: mvc/controllers/areas
ms.openlocfilehash: 3e998af42cd6209271495dd8dd97a8aed35717a4
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274832"
---
# <a name="areas-in-aspnet-core"></a>Áreas de ASP.NET Core

Por [Dhananjay Kumar](https://twitter.com/debug_mode) y [Rick Anderson](https://twitter.com/RickAndMSFT)

Las áreas son una característica de MVC de ASP.NET que se usa para organizar funciones relacionadas en un grupo como un espacio de nombres independiente (para el enrutamiento) y una estructura de carpetas (para las vistas). El uso de áreas crea una jerarquía para el enrutamiento mediante la adición de otro parámetro de ruta, `area`, a `controller` y `action`.

Las áreas ofrecen una manera de dividir una aplicación web ASP.NET Core MVC de gran tamaño en agrupaciones funcionales más pequeñas. Un área es en realidad una estructura de MVC dentro de una aplicación. En un proyecto de MVC, los componentes lógicos como el modelo, el controlador y la vista se guardan en carpetas diferentes, y MVC usa las convenciones de nomenclatura para crear la relación entre estos componentes. Para una aplicación grande, puede ser conveniente dividir la aplicación en distintas áreas de funciones de alto nivel. Por ejemplo, una aplicación de comercio electrónico con varias unidades de negocio, como la finalización de la compra, la facturación, la búsqueda, etc. Cada una de estas unidades tiene sus propias vistas, controladores y modelos de componentes lógicos. En este escenario, puede usar las áreas para dividir físicamente los componentes empresariales del mismo proyecto.

Un área puede definirse como unidades funcionales más pequeñas en un proyecto de ASP.NET Core MVC con su propio conjunto de modelos, vistas y controladores.

Considere el uso de áreas en un proyecto de MVC en los casos siguientes:

* La aplicación está formada por varios componentes funcionales de alto nivel que deben separarse lógicamente.

* Le interesa dividir el proyecto de MVC para que se pueda trabajar en cada área funcional de forma independiente.

Características de las áreas:

* Una aplicación ASP.NET Core MVC puede tener cualquier número de áreas.

* Cada área tiene sus propios controladores, modelos y vistas.

* Permiten organizar proyectos de MVC de gran tamaño en varios componentes de alto nivel en los que se puede trabajar de forma independiente.

* Admiten varios controladores con el mismo nombre, siempre y cuando tengan *áreas* diferentes.

Veamos un ejemplo para ilustrar cómo se crean y se usan las áreas. Supongamos que tiene una aplicación de tienda con dos grupos distintos de controladores y vistas: Productos y Servicios. Una estructura de carpetas típica para dicha aplicación con áreas de MVC tendría un aspecto similar al siguiente:

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

Cuando MVC intenta representar una vista en un área, de forma predeterminada, busca en las ubicaciones siguientes:

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
   /Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
   /Views/Shared/<Action-Name>.cshtml
   ```

Estas son las ubicaciones predeterminadas que se pueden cambiar mediante `AreaViewLocationFormats` en `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.

Por ejemplo, en el código siguiente, el nombre de carpeta "Areas" se ha cambiado a "Categories".

```csharp
services.Configure<RazorViewEngineOptions>(options =>
   {
       options.AreaViewLocationFormats.Clear();
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/{1}/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/Shared/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Views/Shared/{0}.cshtml");
   });
   ```

Hay que tener en cuenta que la estructura de la carpeta *Views* es la única que se considera importante aquí; el contenido de las demás carpetas, como *Controllers* y *Models*, **no** importa. Por ejemplo, no hace falta tener una carpeta *Controllers* y *Models*. Esto funciona porque el contenido de *Controllers* y *Models* solo es código que se compila en una .dll, mientras que el contenido de *Views* no lo es mientras no se realice una solicitud a esa vista.

Una vez que haya definido la jerarquía de carpetas, debe indicar a MVC que cada controlador está asociado a un área. Para hacerlo, decore el nombre del controlador con el atributo `[Area]`.

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

Establezca una definición de ruta que funcione con las áreas recién creadas. En el artículo [Enrutamiento a acciones del controlador](routing.md) se explica en detalle cómo crear definiciones de ruta, incluido el uso de rutas convencionales frente a rutas de atributo. En este ejemplo usaremos una ruta convencional. Para ello, abra el archivo *Startup.cs* y modifíquelo mediante la adición de la definición de ruta con nombre `areaRoute` que se indica a continuación.

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

Si va a `http://<yourApp>/products`, se invocará el método de acción `Index` de `HomeController` en el área `Products`.

## <a name="link-generation"></a>Generación de vínculos

* Generación de vínculos desde una acción dentro de un controlador basado en área en otra acción dentro del mismo controlador.

  Supongamos que la ruta de la solicitud actual es `/Products/Home/Create`.

  Sintaxis de HtmlHelper: `@Html.ActionLink("Go to Product's Home Page", "Index")`

  Sintaxis de TagHelper: `<a asp-action="Index">Go to Product's Home Page</a>`

  Tenga en cuenta que es necesario proporcionar aquí los valores de área y controlador, dado que ya están disponibles en el contexto de la solicitud actual. Estos tipos de valores se denominan valores `ambient`.

* Generación de vínculos desde una acción dentro de un controlador basado en área en otra acción en un controlador diferente.

  Supongamos que la ruta de la solicitud actual es `/Products/Home/Create`.

  Sintaxis de HtmlHelper: `@Html.ActionLink("Go to Manage Products Home Page", "Index", "Manage")`

  Sintaxis de TagHelper: `<a asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`

  Tenga en cuenta que aquí se usa el valor de ambiente de área, pero el valor de controlador se especifica explícitamente más arriba.

* Generación de vínculos desde una acción dentro de un controlador basado en área en otra acción en un controlador diferente y un área diferente.

  Supongamos que la ruta de la solicitud actual es `/Products/Home/Create`.

  Sintaxis de HtmlHelper: `@Html.ActionLink("Go to Services Home Page", "Index", "Home", new { area = "Services" })`

  Sintaxis de TagHelper: `<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services Home Page</a>`

  Tenga en cuenta que aquí no se usan valores de ambiente.

* Generación de vínculos desde una acción dentro de un controlador basado en área en otra acción en un controlador diferente y **no** en un área.

  Sintaxis de HtmlHelper: `@Html.ActionLink("Go to Manage Products  Home Page", "Index", "Home", new { area = "" })`

  Sintaxis de TagHelper: `<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`

  Dado que queremos generar vínculos en una acción de controlador que no está basada en un área, vaciaremos aquí el valor de ambiente para el área.

## <a name="publishing-areas"></a>Publicación de áreas

Todos los archivos `*.cshtml` y `wwwroot/**` se publican en la salida cuando se incluye `<Project Sdk="Microsoft.NET.Sdk.Web">` en el archivo *.csproj*.
