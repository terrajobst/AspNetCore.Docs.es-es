---
title: Información general de ASP.NET Core MVC
author: ardalis
description: Conozca ASP.NET Core MVC, un marco completo para crear aplicaciones web y varias API mediante el patrón de diseño del controlador de vista de modelos.
manager: wpickett
ms.author: riande
ms.date: 01/08/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/overview
ms.openlocfilehash: 0ebf53e0d14ffb5d9ab969e3d6e038a292f913c1
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2018
ms.locfileid: "34566911"
---
# <a name="overview-of-aspnet-core-mvc"></a><span data-ttu-id="12d18-103">Información general de ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="12d18-103">Overview of ASP.NET Core MVC</span></span>

<span data-ttu-id="12d18-104">Por [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="12d18-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="12d18-105">ASP.NET Core MVC es un completo marco de trabajo para compilar aplicaciones web y API mediante el patrón de diseño Modelo-Vista-Controlador.</span><span class="sxs-lookup"><span data-stu-id="12d18-105">ASP.NET Core MVC is a rich framework for building web apps and APIs using the Model-View-Controller design pattern.</span></span>

## <a name="what-is-the-mvc-pattern"></a><span data-ttu-id="12d18-106">¿Qué es el patrón de MVC?</span><span class="sxs-lookup"><span data-stu-id="12d18-106">What is the MVC pattern?</span></span>

<span data-ttu-id="12d18-107">El patrón de arquitectura del controlador de vista de modelos (MVC) separa una aplicación en tres grupos de componentes principales: modelos, vistas y controladores.</span><span class="sxs-lookup"><span data-stu-id="12d18-107">The Model-View-Controller (MVC) architectural pattern separates an application into three main groups of components: Models, Views, and Controllers.</span></span> <span data-ttu-id="12d18-108">Este patrón permite lograr la [separación de intereses](http://deviq.com/separation-of-concerns/).</span><span class="sxs-lookup"><span data-stu-id="12d18-108">This pattern helps to achieve [separation of concerns](http://deviq.com/separation-of-concerns/).</span></span> <span data-ttu-id="12d18-109">Con este patrón, las solicitudes del usuario se enrutan a un controlador que se encarga de trabajar con el modelo para realizar las acciones del usuario o recuperar los resultados de consultas.</span><span class="sxs-lookup"><span data-stu-id="12d18-109">Using this pattern, user requests are routed to a Controller which is responsible for working with the Model to perform user actions and/or retrieve results of queries.</span></span> <span data-ttu-id="12d18-110">El controlador elige la vista para mostrar al usuario y proporciona cualquier dato de modelo que sea necesario.</span><span class="sxs-lookup"><span data-stu-id="12d18-110">The Controller chooses the View to display to the user, and provides it with any Model data it requires.</span></span>

<span data-ttu-id="12d18-111">En el siguiente diagrama se muestran los tres componentes principales y cuál hace referencia a los demás:</span><span class="sxs-lookup"><span data-stu-id="12d18-111">The following diagram shows the three main components and which ones reference the others:</span></span>

![Patrón de MVC](overview/_static/mvc.png)

<span data-ttu-id="12d18-113">Con esta delineación de responsabilidades es más sencillo escalar la aplicación, porque resulta más fácil codificar, depurar y probar algo (modelo, vista o controlador) que tenga un solo trabajo (y siga el [principio de responsabilidad única ](http://deviq.com/single-responsibility-principle/)).</span><span class="sxs-lookup"><span data-stu-id="12d18-113">This delineation of responsibilities helps you scale the application in terms of complexity because it's easier to code, debug, and test something (model, view, or controller) that has a single job (and follows the [Single Responsibility Principle](http://deviq.com/single-responsibility-principle/)).</span></span> <span data-ttu-id="12d18-114">Es más difícil actualizar, probar y depurar código que tenga dependencias repartidas entre dos o más de estas tres áreas.</span><span class="sxs-lookup"><span data-stu-id="12d18-114">It's more difficult to update, test, and debug code that has dependencies spread across two or more of these three areas.</span></span> <span data-ttu-id="12d18-115">Por ejemplo, la lógica de la interfaz de usuario tiende a cambiar con mayor frecuencia que la lógica de negocios.</span><span class="sxs-lookup"><span data-stu-id="12d18-115">For example, user interface logic tends to change more frequently than business logic.</span></span> <span data-ttu-id="12d18-116">Si el código de presentación y la lógica de negocios se combinan en un solo objeto, un objeto que contenga lógica de negocios deberá modificarse cada vez que cambie la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="12d18-116">If presentation code and business logic are combined in a single object, an object containing business logic must be modified every time the user interface is changed.</span></span> <span data-ttu-id="12d18-117">A menudo esto genera errores y es necesario volver a probar la lógica de negocio después de cada cambio mínimo en la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="12d18-117">This often introduces errors and requires the retesting of business logic after every minimal user interface change.</span></span>

> [!NOTE]
> <span data-ttu-id="12d18-118">La vista y el controlador dependen del modelo.</span><span class="sxs-lookup"><span data-stu-id="12d18-118">Both the view and the controller depend on the model.</span></span> <span data-ttu-id="12d18-119">Sin embargo, el modelo no depende de la vista ni del controlador. </span><span class="sxs-lookup"><span data-stu-id="12d18-119">However, the model depends on neither the view nor the controller.</span></span> <span data-ttu-id="12d18-120">Esta es una de las ventajas principales de la separación.</span><span class="sxs-lookup"><span data-stu-id="12d18-120">This is one of the key benefits of the separation.</span></span> <span data-ttu-id="12d18-121">Esta separación permite compilar y probar el modelo con independencia de la presentación visual.</span><span class="sxs-lookup"><span data-stu-id="12d18-121">This separation allows the model to be built and tested independent of the visual presentation.</span></span>

### <a name="model-responsibilities"></a><span data-ttu-id="12d18-122">Responsabilidades del modelo</span><span class="sxs-lookup"><span data-stu-id="12d18-122">Model Responsibilities</span></span>

<span data-ttu-id="12d18-123">El modelo en una aplicación de MVC representa el estado de la aplicación y cualquier lógica de negocios u operaciones que esta deba realizar.</span><span class="sxs-lookup"><span data-stu-id="12d18-123">The Model in an MVC application represents the state of the application and any business logic or operations that should be performed by it.</span></span> <span data-ttu-id="12d18-124">La lógica de negocios debe encapsularse en el modelo, junto con cualquier lógica de implementación para conservar el estado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="12d18-124">Business logic should be encapsulated in the model, along with any implementation logic for persisting the state of the application.</span></span> <span data-ttu-id="12d18-125">Las vistas fuertemente tipadas normalmente usan tipos ViewModel diseñados para que contengan los datos para mostrar en esa vista.</span><span class="sxs-lookup"><span data-stu-id="12d18-125">Strongly-typed views typically use ViewModel types designed to contain the data to display on that view.</span></span> <span data-ttu-id="12d18-126">El controlador crea y rellena estas instancias de ViewModel desde el modelo.</span><span class="sxs-lookup"><span data-stu-id="12d18-126">The controller creates and populates these ViewModel instances from the model.</span></span>

> [!NOTE]
> <span data-ttu-id="12d18-127">Hay muchas maneras de organizar el modelo en una aplicación que usa el patrón de arquitectura de MVC.</span><span class="sxs-lookup"><span data-stu-id="12d18-127">There are many ways to organize the model in an app that uses the MVC architectural pattern.</span></span> <span data-ttu-id="12d18-128">Obtenga más información sobre algunos [tipos diferentes de tipos de modelo](http://deviq.com/kinds-of-models/).</span><span class="sxs-lookup"><span data-stu-id="12d18-128">Learn more about some [different kinds of model types](http://deviq.com/kinds-of-models/).</span></span>

### <a name="view-responsibilities"></a><span data-ttu-id="12d18-129">Responsabilidades de las vistas</span><span class="sxs-lookup"><span data-stu-id="12d18-129">View Responsibilities</span></span>

<span data-ttu-id="12d18-130">Las vistas se encargan de presentar el contenido a través de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="12d18-130">Views are responsible for presenting content through the user interface.</span></span> <span data-ttu-id="12d18-131">Usan el [motor de vistas de Razor](#razor-view-engine) para incrustar código .NET en formato HTML.</span><span class="sxs-lookup"><span data-stu-id="12d18-131">They use the [Razor view engine](#razor-view-engine) to embed .NET code in HTML markup.</span></span> <span data-ttu-id="12d18-132">Debería haber la mínima lógica entre las vistas y cualquier lógica en ellas debe estar relacionada con la presentación de contenido.</span><span class="sxs-lookup"><span data-stu-id="12d18-132">There should be minimal logic within views, and any logic in them should relate to presenting content.</span></span> <span data-ttu-id="12d18-133">Si ve que necesita realizar una gran cantidad de lógica en los archivos de vistas para mostrar datos de un modelo complejo, considere la opción de usar un [componente de vista](views/view-components.md), ViewModel, o una plantilla de vista para simplificar la vista.</span><span class="sxs-lookup"><span data-stu-id="12d18-133">If you find the need to perform a great deal of logic in view files in order to display data from a complex model, consider using a [View Component](views/view-components.md), ViewModel, or view template to simplify the view.</span></span>

### <a name="controller-responsibilities"></a><span data-ttu-id="12d18-134">Responsabilidades del controlador</span><span class="sxs-lookup"><span data-stu-id="12d18-134">Controller Responsibilities</span></span>

<span data-ttu-id="12d18-135">Los controladores son los componentes que controlan la interacción del usuario, trabajan con el modelo y, en última instancia, seleccionan una vista para representarla.</span><span class="sxs-lookup"><span data-stu-id="12d18-135">Controllers are the components that handle user interaction, work with the model, and ultimately select a view to render.</span></span> <span data-ttu-id="12d18-136">En una aplicación de MVC, la vista solo muestra información; el controlador controla y responde a la interacción y los datos que introducen los usuarios.</span><span class="sxs-lookup"><span data-stu-id="12d18-136">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span> <span data-ttu-id="12d18-137">En el patrón de MVC, el controlador es el punto de entrada inicial que se encarga de seleccionar con qué tipos de modelo trabajar y qué vistas representar (de ahí su nombre, ya que controla el modo en que la aplicación responde a una determinada solicitud).</span><span class="sxs-lookup"><span data-stu-id="12d18-137">In the MVC pattern, the controller is the initial entry point, and is responsible for selecting which model types to work with and which view to render (hence its name - it controls how the app responds to a given request).</span></span>

> [!NOTE]
> <span data-ttu-id="12d18-138">Los controladores no deberían complicarse con demasiadas responsabilidades.</span><span class="sxs-lookup"><span data-stu-id="12d18-138">Controllers shouldn't be overly complicated by too many responsibilities.</span></span> <span data-ttu-id="12d18-139">Para evitar que la lógica del controlador se vuelva demasiado compleja, use el [principio de responsabilidad única](http://deviq.com/single-responsibility-principle/) para sacar la lógica de negocios fuera el controlador y llevarla al modelo de dominio.</span><span class="sxs-lookup"><span data-stu-id="12d18-139">To keep controller logic from becoming overly complex, use the [Single Responsibility Principle](http://deviq.com/single-responsibility-principle/) to push business logic out of the controller and into the domain model.</span></span>

>[!TIP]
> <span data-ttu-id="12d18-140">Si piensa que el controlador realiza con frecuencia los mismos tipos de acciones, puede seguir el principio [Una vez y solo una (DRY)](http://deviq.com/don-t-repeat-yourself/) moviendo estas acciones comunes a [filtros](#filters).</span><span class="sxs-lookup"><span data-stu-id="12d18-140">If you find that your controller actions frequently perform the same kinds of actions, you can follow the [Don't Repeat Yourself principle](http://deviq.com/don-t-repeat-yourself/) by moving these common actions into [filters](#filters).</span></span>

## <a name="what-is-aspnet-core-mvc"></a><span data-ttu-id="12d18-141">Qué es ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="12d18-141">What is ASP.NET Core MVC</span></span>

<span data-ttu-id="12d18-142">El marco de ASP.NET Core MVC es un marco de presentación ligero, de código abierto y con gran capacidad de prueba, que está optimizado para usarlo con ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="12d18-142">The ASP.NET Core MVC framework is a lightweight, open source, highly testable presentation framework optimized for use with ASP.NET Core.</span></span>

<span data-ttu-id="12d18-143">ASP.NET Core MVC ofrece una manera basada en patrones de crear sitios web dinámicos que permitan una clara separación de intereses.</span><span class="sxs-lookup"><span data-stu-id="12d18-143">ASP.NET Core MVC provides a patterns-based way to build dynamic websites that enables a clean separation of concerns.</span></span> <span data-ttu-id="12d18-144">Proporciona control total sobre el marcado, admite el desarrollo controlado por pruebas (TDD) y usa los estándares web más recientes.</span><span class="sxs-lookup"><span data-stu-id="12d18-144">It gives you full control over markup, supports TDD-friendly development and uses the latest web standards.</span></span>

## <a name="features"></a><span data-ttu-id="12d18-145">Características</span><span class="sxs-lookup"><span data-stu-id="12d18-145">Features</span></span>

<span data-ttu-id="12d18-146">ASP.NET Core MVC incluye lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="12d18-146">ASP.NET Core MVC includes the following:</span></span>

* [<span data-ttu-id="12d18-147">Enrutamiento</span><span class="sxs-lookup"><span data-stu-id="12d18-147">Routing</span></span>](#routing)
* [<span data-ttu-id="12d18-148">Enlace de modelos</span><span class="sxs-lookup"><span data-stu-id="12d18-148">Model binding</span></span>](#model-binding)
* [<span data-ttu-id="12d18-149">Validación de modelos</span><span class="sxs-lookup"><span data-stu-id="12d18-149">Model validation</span></span>](#model-validation)
* [<span data-ttu-id="12d18-150">Inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="12d18-150">Dependency injection</span></span>](../fundamentals/dependency-injection.md)
* [<span data-ttu-id="12d18-151">Filtros</span><span class="sxs-lookup"><span data-stu-id="12d18-151">Filters</span></span>](#filters)
* [<span data-ttu-id="12d18-152">Áreas</span><span class="sxs-lookup"><span data-stu-id="12d18-152">Areas</span></span>](#areas)
* [<span data-ttu-id="12d18-153">API web</span><span class="sxs-lookup"><span data-stu-id="12d18-153">Web APIs</span></span>](#web-apis)
* [<span data-ttu-id="12d18-154">Capacidad de prueba</span><span class="sxs-lookup"><span data-stu-id="12d18-154">Testability</span></span>](#testability)
* [<span data-ttu-id="12d18-155">Motor de vistas de Razor</span><span class="sxs-lookup"><span data-stu-id="12d18-155">Razor view engine</span></span>](#razor-view-engine)
* [<span data-ttu-id="12d18-156">Vistas fuertemente tipadas</span><span class="sxs-lookup"><span data-stu-id="12d18-156">Strongly typed views</span></span>](#strongly-typed-views)
* [<span data-ttu-id="12d18-157">Aplicaciones auxiliares de etiquetas</span><span class="sxs-lookup"><span data-stu-id="12d18-157">Tag Helpers</span></span>](#tag-helpers)
* [<span data-ttu-id="12d18-158">Componentes de vista</span><span class="sxs-lookup"><span data-stu-id="12d18-158">View Components</span></span>](#view-components)

### <a name="routing"></a><span data-ttu-id="12d18-159">Enrutamiento</span><span class="sxs-lookup"><span data-stu-id="12d18-159">Routing</span></span>

<span data-ttu-id="12d18-160">ASP.NET Core MVC se basa en el [enrutamiento de ASP.NET Core](../fundamentals/routing.md), un eficaz componente de asignación de URL que permite compilar aplicaciones que tengan direcciones URL comprensibles y que admitan búsquedas.</span><span class="sxs-lookup"><span data-stu-id="12d18-160">ASP.NET Core MVC is built on top of [ASP.NET Core's routing](../fundamentals/routing.md), a powerful URL-mapping component that lets you build applications that have comprehensible and searchable URLs.</span></span> <span data-ttu-id="12d18-161">Esto permite definir patrones de nomenclatura de URL de la aplicación que funcionen bien para la optimización del motor de búsqueda (SEO) y para la generación de vínculos, sin tener en cuenta cómo se organizan los archivos en el servidor web.</span><span class="sxs-lookup"><span data-stu-id="12d18-161">This enables you to define your application's URL naming patterns that work well for search engine optimization (SEO) and for link generation, without regard for how the files on your web server are organized.</span></span> <span data-ttu-id="12d18-162">Puede definir las rutas mediante una sintaxis de plantilla de ruta adecuada que admita las restricciones de valores de ruta, los valores predeterminados y los valores opcionales.</span><span class="sxs-lookup"><span data-stu-id="12d18-162">You can define your routes using a convenient route template syntax that supports route value constraints, defaults and optional values.</span></span>

<span data-ttu-id="12d18-163">El *enrutamiento basado en la convención* permite definir globalmente los formatos de URL que acepta la aplicación y cómo cada uno de esos formatos se asigna a un método de acción específico en un determinado controlador.</span><span class="sxs-lookup"><span data-stu-id="12d18-163">*Convention-based routing* enables you to globally define the URL formats that your application accepts and how each of those formats maps to a specific action method on given controller.</span></span> <span data-ttu-id="12d18-164">Cuando se recibe una solicitud entrante, el motor de enrutamiento analiza la URL y la hace coincidir con uno de los formatos de URL definidos. Después, llama al método de acción del controlador asociado.</span><span class="sxs-lookup"><span data-stu-id="12d18-164">When an incoming request is received, the routing engine parses the URL and matches it to one of the defined URL formats, and then calls the associated controller's action method.</span></span>

```csharp
routes.MapRoute(name: "Default", template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="12d18-165">El *enrutamiento de atributos* permite especificar información de enrutamiento mediante la asignación de atributos a los controladores y las acciones, que permiten definir las rutas de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="12d18-165">*Attribute routing* enables you to specify routing information by decorating your controllers and actions with attributes that define your application's routes.</span></span> <span data-ttu-id="12d18-166">Esto significa que las definiciones de ruta se colocan junto al controlador y la acción con la que están asociados.</span><span class="sxs-lookup"><span data-stu-id="12d18-166">This means that your route definitions are placed next to the controller and action with which they're associated.</span></span>

```csharp
[Route("api/[controller]")]
public class ProductsController : Controller
{
  [HttpGet("{id}")]
  public IActionResult GetProduct(int id)
  {
    ...
  }
}
```

### <a name="model-binding"></a><span data-ttu-id="12d18-167">Enlace de modelos</span><span class="sxs-lookup"><span data-stu-id="12d18-167">Model binding</span></span>

<span data-ttu-id="12d18-168">El [enlace de modelo](models/model-binding.md) de ASP.NET Core MVC convierte los datos de solicitud del cliente (valores de formulario, datos de ruta, parámetros de cadena de consulta, encabezados HTTP) en objetos que el controlador puede controlar.</span><span class="sxs-lookup"><span data-stu-id="12d18-168">ASP.NET Core MVC [model binding](models/model-binding.md) converts client request data  (form values, route data, query string parameters, HTTP headers) into objects that the controller can handle.</span></span> <span data-ttu-id="12d18-169">Como resultado, la lógica del controlador no tiene que realizar el trabajo de pensar en los datos de la solicitud entrante; simplemente tiene los datos como parámetros para los métodos de acción.</span><span class="sxs-lookup"><span data-stu-id="12d18-169">As a result, your controller logic doesn't have to do the work of figuring out the incoming request data; it simply has the data as parameters to its action methods.</span></span>

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null) { ... }
   ```

### <a name="model-validation"></a><span data-ttu-id="12d18-170">Validación de modelos</span><span class="sxs-lookup"><span data-stu-id="12d18-170">Model validation</span></span>

<span data-ttu-id="12d18-171">ASP.NET Core MVC admite la [validación](models/validation.md) decorando el objeto de modelo con los atributos de validación de anotación de datos.</span><span class="sxs-lookup"><span data-stu-id="12d18-171">ASP.NET Core MVC supports [validation](models/validation.md) by decorating your model object with data annotation validation attributes.</span></span> <span data-ttu-id="12d18-172">Los atributos de validación se comprueban en el lado cliente antes de que los valores se registren en el servidor, y también en el servidor antes de llamar a la acción del controlador.</span><span class="sxs-lookup"><span data-stu-id="12d18-172">The validation attributes are checked on the client side before values are posted to the server, as well as on the server before the controller action is called.</span></span>

```csharp
using System.ComponentModel.DataAnnotations;
public class LoginViewModel
{
    [Required]
    [EmailAddress]
    public string Email { get; set; }

    [Required]
    [DataType(DataType.Password)]
    public string Password { get; set; }

    [Display(Name = "Remember me?")]
    public bool RememberMe { get; set; }
}
```

<span data-ttu-id="12d18-173">Una acción del controlador:</span><span class="sxs-lookup"><span data-stu-id="12d18-173">A controller action:</span></span>

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null)
{
    if (ModelState.IsValid)
    {
      // work with the model
    }
    // At this point, something failed, redisplay form
    return View(model);
}
```

<span data-ttu-id="12d18-174">El marco administra los datos de la solicitud de validación en el cliente y en el servidor.</span><span class="sxs-lookup"><span data-stu-id="12d18-174">The framework handles validating request data both on the client and on the server.</span></span> <span data-ttu-id="12d18-175">La lógica de validación especificada en tipos de modelo se agrega a las vistas representadas como anotaciones discretas y se aplica en el explorador con [Validación de jQuery](https://jqueryvalidation.org/).</span><span class="sxs-lookup"><span data-stu-id="12d18-175">Validation logic specified on model types is added to the rendered views as unobtrusive annotations and is enforced in the browser with [jQuery Validation](https://jqueryvalidation.org/).</span></span>

### <a name="dependency-injection"></a><span data-ttu-id="12d18-176">Inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="12d18-176">Dependency injection</span></span>

<span data-ttu-id="12d18-177">ASP.NET Core tiene compatibilidad integrada con la [inserción de dependencias](../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="12d18-177">ASP.NET Core has built-in support for [dependency injection (DI)](../fundamentals/dependency-injection.md).</span></span> <span data-ttu-id="12d18-178">En ASP.NET Core MVC, los [controladores](controllers/dependency-injection.md) pueden solicitar los servicios que necesiten a través de sus constructores, lo que les permite seguir el [principio de dependencias explícitas](http://deviq.com/explicit-dependencies-principle/).</span><span class="sxs-lookup"><span data-stu-id="12d18-178">In ASP.NET Core MVC, [controllers](controllers/dependency-injection.md) can request needed services through their constructors, allowing them to follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span>

<span data-ttu-id="12d18-179">La aplicación también puede usar la [inserción de dependencias en archivos de vistas](views/dependency-injection.md), usando la directiva `@inject`:</span><span class="sxs-lookup"><span data-stu-id="12d18-179">Your app can also use [dependency injection in view files](views/dependency-injection.md), using the `@inject` directive:</span></span>

```cshtml
@inject SomeService ServiceName
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ServiceName.GetTitle</title>
</head>
<body>
    <h1>@ServiceName.GetTitle</h1>
</body>
</html>
```

### <a name="filters"></a><span data-ttu-id="12d18-180">Filtros</span><span class="sxs-lookup"><span data-stu-id="12d18-180">Filters</span></span>

<span data-ttu-id="12d18-181">Los [filtros](controllers/filters.md) ayudan a los desarrolladores a encapsular ciertas preocupaciones transversales, como el control de excepciones o la autorización.</span><span class="sxs-lookup"><span data-stu-id="12d18-181">[Filters](controllers/filters.md) help developers encapsulate cross-cutting concerns, like exception handling or authorization.</span></span> <span data-ttu-id="12d18-182">Los filtros habilitan la ejecución de lógica de procesamiento previo y posterior para métodos de acción y pueden configurarse para ejecutarse en ciertos puntos dentro de la canalización de ejecución para una solicitud determinada.</span><span class="sxs-lookup"><span data-stu-id="12d18-182">Filters enable running custom pre- and post-processing logic for action methods, and can be configured to run at certain points within the execution pipeline for a given request.</span></span> <span data-ttu-id="12d18-183">Los filtros pueden aplicarse a controladores o acciones como atributos (o pueden ejecutarse globalmente).</span><span class="sxs-lookup"><span data-stu-id="12d18-183">Filters can be applied to controllers or actions as attributes (or can be run globally).</span></span> <span data-ttu-id="12d18-184">En el marco se incluyen varios filtros (como `Authorize`).</span><span class="sxs-lookup"><span data-stu-id="12d18-184">Several filters (such as `Authorize`) are included in the framework.</span></span>


```csharp
[Authorize]
   public class AccountController : Controller
   {
```

### <a name="areas"></a><span data-ttu-id="12d18-185">Áreas</span><span class="sxs-lookup"><span data-stu-id="12d18-185">Areas</span></span>

<span data-ttu-id="12d18-186">Las [áreas](controllers/areas.md) ofrecen una manera de dividir una aplicación web ASP.NET Core MVC de gran tamaño en agrupaciones funcionales más pequeñas.</span><span class="sxs-lookup"><span data-stu-id="12d18-186">[Areas](controllers/areas.md) provide a way to partition a large ASP.NET Core MVC Web app into smaller functional groupings.</span></span> <span data-ttu-id="12d18-187">Un área es una estructura de MVC dentro de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="12d18-187">An area is an MVC structure inside an application.</span></span> <span data-ttu-id="12d18-188">En un proyecto de MVC, los componentes lógicos como el modelo, el controlador y la vista se guardan en carpetas diferentes, y MVC usa las convenciones de nomenclatura para crear la relación entre estos componentes.</span><span class="sxs-lookup"><span data-stu-id="12d18-188">In an MVC project, logical components like Model, Controller, and View are kept in different folders, and MVC uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="12d18-189">Para una aplicación grande, puede ser conveniente dividir la aplicación en distintas áreas de funciones de alto nivel.</span><span class="sxs-lookup"><span data-stu-id="12d18-189">For a large app, it may be advantageous to partition the app into separate high level areas of functionality.</span></span> <span data-ttu-id="12d18-190">Por ejemplo, una aplicación de comercio electrónico con varias unidades de negocio, como la finalización de la compra, la facturación, la búsqueda, etcétera. Cada una de estas unidades tiene sus propias vistas, controladores y modelos de componentes lógicos.</span><span class="sxs-lookup"><span data-stu-id="12d18-190">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search etc. Each of these units have their own logical component views, controllers, and models.</span></span>

### <a name="web-apis"></a><span data-ttu-id="12d18-191">API web</span><span class="sxs-lookup"><span data-stu-id="12d18-191">Web APIs</span></span>

<span data-ttu-id="12d18-192">Además de ser una plataforma excelente para crear sitios web, ASP.NET Core MVC es muy compatible con la creación de las API web.</span><span class="sxs-lookup"><span data-stu-id="12d18-192">In addition to being a great platform for building web sites, ASP.NET Core MVC has great support for building Web APIs.</span></span> <span data-ttu-id="12d18-193">Puede crear servicios que lleguen con facilidad a una amplia gama de clientes, incluidos exploradores y dispositivos móviles.</span><span class="sxs-lookup"><span data-stu-id="12d18-193">You can build services that reach a broad range of clients including browsers and mobile devices.</span></span>

<span data-ttu-id="12d18-194">El marco incluye compatibilidad con la negociación de contenido HTTP y compatibilidad integrada para [aplicar formato a datos](xref:web-api/advanced/formatting) como JSON o XML.</span><span class="sxs-lookup"><span data-stu-id="12d18-194">The framework includes support for HTTP content-negotiation with built-in support to [format data](xref:web-api/advanced/formatting) as JSON or XML.</span></span> <span data-ttu-id="12d18-195">Escriba [formateadores personalizados](xref:web-api/advanced/custom-formatters) para agregar compatibilidad con sus propios formatos.</span><span class="sxs-lookup"><span data-stu-id="12d18-195">Write [custom formatters](xref:web-api/advanced/custom-formatters) to add support for your own formats.</span></span>

<span data-ttu-id="12d18-196">Use la generación de vínculos para habilitar la compatibilidad con hipermedios.</span><span class="sxs-lookup"><span data-stu-id="12d18-196">Use link generation to enable support for hypermedia.</span></span> <span data-ttu-id="12d18-197">Habilite fácilmente la compatibilidad con el [uso compartido de recursos entre orígenes (CORS)](http://www.w3.org/TR/cors/) para que las API web se pueden compartir entre varias aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="12d18-197">Easily enable support for [cross-origin resource sharing (CORS)](http://www.w3.org/TR/cors/) so that your Web APIs can be shared across multiple Web applications.</span></span>

### <a name="testability"></a><span data-ttu-id="12d18-198">Capacidad de prueba</span><span class="sxs-lookup"><span data-stu-id="12d18-198">Testability</span></span>

<span data-ttu-id="12d18-199">Al usar interfaces e inserción de dependencias, el marco es adecuado para realizar pruebas unitarias. También incluye características (por ejemplo, un proveedor TestHost e InMemory para Entity Framework) con las que resulta muy fácil y rápido realizar [pruebas de integración](xref:test/integration-tests).</span><span class="sxs-lookup"><span data-stu-id="12d18-199">The framework's use of interfaces and dependency injection make it well-suited to unit testing, and the framework includes features (like a TestHost and InMemory provider for Entity Framework) that make [integration tests](xref:test/integration-tests) quick and easy as well.</span></span> <span data-ttu-id="12d18-200">Obtenga más información sobre [cómo probar la lógica del controlador](controllers/testing.md).</span><span class="sxs-lookup"><span data-stu-id="12d18-200">Learn more about [how to test controller logic](controllers/testing.md).</span></span>

### <a name="razor-view-engine"></a><span data-ttu-id="12d18-201">Motor de vistas de Razor</span><span class="sxs-lookup"><span data-stu-id="12d18-201">Razor view engine</span></span>

<span data-ttu-id="12d18-202">Las [vistas de ASP.NET Core MVC](views/overview.md) usan el [motor de vistas de Razor](views/razor.md) para representar vistas.</span><span class="sxs-lookup"><span data-stu-id="12d18-202">[ASP.NET Core MVC views](views/overview.md) use the [Razor view engine](views/razor.md) to render views.</span></span> <span data-ttu-id="12d18-203">Razor es un lenguaje de marcado de plantillas compacto, expresivo y fluido para definir vistas que usan código incrustado de C#.</span><span class="sxs-lookup"><span data-stu-id="12d18-203">Razor is a compact, expressive and fluid template markup language for defining views using embedded C# code.</span></span> <span data-ttu-id="12d18-204">Razor se usa para generar dinámicamente contenido web en el servidor.</span><span class="sxs-lookup"><span data-stu-id="12d18-204">Razor is used to dynamically generate web content on the server.</span></span> <span data-ttu-id="12d18-205">Permite combinar de manera limpia el código del servidor con el código y el contenido del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="12d18-205">You can cleanly mix server code with client side content and code.</span></span>

```text
<ul>
  @for (int i = 0; i < 5; i++) {
    <li>List item @i</li>
  }
</ul>
```

<span data-ttu-id="12d18-206">Con el motor de vistas de Razor, puede definir [diseños](views/layout.md), [vistas parciales](views/partial.md) y secciones reemplazables.</span><span class="sxs-lookup"><span data-stu-id="12d18-206">Using the Razor view engine you can define [layouts](views/layout.md), [partial views](views/partial.md) and replaceable sections.</span></span>

### <a name="strongly-typed-views"></a><span data-ttu-id="12d18-207">Vistas fuertemente tipadas</span><span class="sxs-lookup"><span data-stu-id="12d18-207">Strongly typed views</span></span>

<span data-ttu-id="12d18-208">Las vistas de Razor en MVC pueden estar fuertemente tipadas en función del modelo.</span><span class="sxs-lookup"><span data-stu-id="12d18-208">Razor views in MVC can be strongly typed based on your model.</span></span> <span data-ttu-id="12d18-209">Los controladores pueden pasar un modelo fuertemente tipado a las vistas para que estas admitan IntelliSense y la comprobación de tipos.</span><span class="sxs-lookup"><span data-stu-id="12d18-209">Controllers can pass a strongly typed model to views enabling your views to have type checking and IntelliSense support.</span></span>

<span data-ttu-id="12d18-210">Por ejemplo, en esta vista se representa un modelo de tipo `IEnumerable<Product>`:</span><span class="sxs-lookup"><span data-stu-id="12d18-210">For example, the following view renders a model of type `IEnumerable<Product>`:</span></span>

```cshtml
@model IEnumerable<Product>
<ul>
    @foreach (Product p in Model)
    {
        <li>@p.Name</li>
    }
</ul>
```

### <a name="tag-helpers"></a><span data-ttu-id="12d18-211">Aplicaciones auxiliares de etiquetas</span><span class="sxs-lookup"><span data-stu-id="12d18-211">Tag Helpers</span></span>

<span data-ttu-id="12d18-212">Las [aplicaciones auxiliares de etiquetas](views/tag-helpers/intro.md) permiten que el código del lado servidor participe en la creación y la representación de elementos HTML en archivos de Razor.</span><span class="sxs-lookup"><span data-stu-id="12d18-212">[Tag Helpers](views/tag-helpers/intro.md) enable server side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="12d18-213">Puede usar aplicaciones auxiliares de etiquetas para definir etiquetas personalizadas (por ejemplo, `<environment>`) o para modificar el comportamiento de etiquetas existentes (por ejemplo, `<label>`).</span><span class="sxs-lookup"><span data-stu-id="12d18-213">You can use tag helpers to define custom tags (for example, `<environment>`) or to modify the behavior of existing tags (for example, `<label>`).</span></span> <span data-ttu-id="12d18-214">Las aplicaciones auxiliares de etiquetas enlazan con elementos específicos, en función del nombre del elemento y sus atributos.</span><span class="sxs-lookup"><span data-stu-id="12d18-214">Tag Helpers bind to specific elements based on the element name and its attributes.</span></span> <span data-ttu-id="12d18-215">Proporcionan las ventajas de la representación del lado servidor, al tiempo que se mantiene una experiencia de edición HTML.</span><span class="sxs-lookup"><span data-stu-id="12d18-215">They provide the benefits of server-side rendering while still preserving an HTML editing experience.</span></span>

<span data-ttu-id="12d18-216">Hay muchas aplicaciones auxiliares de etiquetas integradas para tareas comunes (como la creación de formularios, vínculos, carga de activos, etc.) y existen muchas más a disposición en repositorios públicos de GitHub y como paquetes NuGet.</span><span class="sxs-lookup"><span data-stu-id="12d18-216">There are many built-in Tag Helpers for common tasks - such as creating forms, links, loading assets and more - and even more available in public GitHub repositories and as NuGet packages.</span></span> <span data-ttu-id="12d18-217">Las aplicaciones auxiliares de etiquetas se crean en C# y tienen como destino elementos HTML en función del nombre de elemento, el nombre de atributo o la etiqueta principal.</span><span class="sxs-lookup"><span data-stu-id="12d18-217">Tag Helpers are authored in C#, and they target HTML elements based on element name, attribute name, or parent tag.</span></span> <span data-ttu-id="12d18-218">Por ejemplo, la aplicación auxiliar de etiquetas integrada LinkTagHelper puede usarse para crear un vínculo a la acción `Login` de `AccountsController`:</span><span class="sxs-lookup"><span data-stu-id="12d18-218">For example, the built-in LinkTagHelper can be used to create a link to the `Login` action of the `AccountsController`:</span></span>

```cshtml
<p>
    Thank you for confirming your email.
    Please <a asp-controller="Account" asp-action="Login">Click here to Log in</a>.
</p>
```

<span data-ttu-id="12d18-219">La aplicación auxiliar de etiquetas `EnvironmentTagHelper` puede usarse para incluir distintos scripts en las vistas (por ejemplo, sin formato o reducida) según el entorno en tiempo de ejecución, como desarrollo, ensayo o producción:</span><span class="sxs-lookup"><span data-stu-id="12d18-219">The `EnvironmentTagHelper` can be used to include different scripts in your views (for example, raw or minified) based on the runtime environment, such as Development, Staging, or Production:</span></span>

```cshtml
<environment names="Development">
    <script src="~/lib/jquery/dist/jquery.js"></script>
</environment>
<environment names="Staging,Production">
    <script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-2.1.4.min.js"
            asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
            asp-fallback-test="window.jQuery">
    </script>
</environment>
```

<span data-ttu-id="12d18-220">Las aplicaciones auxiliares de etiquetas ofrecen una experiencia de desarrollo compatible con HTML y un entorno de IntelliSense enriquecido para crear formato HTML y Razor.</span><span class="sxs-lookup"><span data-stu-id="12d18-220">Tag Helpers provide an HTML-friendly development experience and a rich IntelliSense environment for creating HTML and Razor markup.</span></span> <span data-ttu-id="12d18-221">La mayoría de las aplicaciones auxiliares de etiquetas integradas tienen como destino elementos HTML existentes y proporcionan atributos del lado servidor para el elemento.</span><span class="sxs-lookup"><span data-stu-id="12d18-221">Most of the built-in Tag Helpers target existing HTML elements and provide server-side attributes for the element.</span></span>

### <a name="view-components"></a><span data-ttu-id="12d18-222">Componentes de vista</span><span class="sxs-lookup"><span data-stu-id="12d18-222">View Components</span></span>

<span data-ttu-id="12d18-223">Los [componentes de vista](views/view-components.md) permiten empaquetar la lógica de representación y reutilizarla en toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="12d18-223">[View Components](views/view-components.md) allow you to package rendering logic and reuse it throughout the application.</span></span> <span data-ttu-id="12d18-224">Son similares a las [vistas parciales](views/partial.md), pero con lógica asociada.</span><span class="sxs-lookup"><span data-stu-id="12d18-224">They're similar to [partial views](views/partial.md), but with associated logic.</span></span>
