---
title: "Información general de ASP.NET Core MVC"
author: ardalis
description: "Obtenga información acerca de cómo principales de ASP.NET MVC es un marco de trabajo para la creación de aplicaciones web y API que usan el modelo Model-View-Controller patrón de diseño."
ms.author: riande
manager: wpickett
ms.date: 01/08/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/overview
ms.openlocfilehash: 3c3279e7d25704f5e70bbd83cf9feebda8319203
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
# <a name="overview-of-aspnet-core-mvc"></a><span data-ttu-id="df3c8-103">Información general de ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="df3c8-103">Overview of ASP.NET Core MVC</span></span>

<span data-ttu-id="df3c8-104">Por [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="df3c8-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="df3c8-105">Núcleo de ASP.NET MVC es un marco completo para la creación de aplicaciones web y API que usan el modelo Model-View-Controller patrón de diseño.</span><span class="sxs-lookup"><span data-stu-id="df3c8-105">ASP.NET Core MVC is a rich framework for building web apps and APIs using the Model-View-Controller design pattern.</span></span>

## <a name="what-is-the-mvc-pattern"></a><span data-ttu-id="df3c8-106">¿Qué es el modelo de MVC?</span><span class="sxs-lookup"><span data-stu-id="df3c8-106">What is the MVC pattern?</span></span>

<span data-ttu-id="df3c8-107">El modelo de arquitectura Model-View-Controller (MVC) separa una aplicación en tres grupos principales de componentes: modelos, vistas y controladores.</span><span class="sxs-lookup"><span data-stu-id="df3c8-107">The Model-View-Controller (MVC) architectural pattern separates an application into three main groups of components: Models, Views, and Controllers.</span></span> <span data-ttu-id="df3c8-108">Este patrón ayuda a lograr [separación de intereses](http://deviq.com/separation-of-concerns/).</span><span class="sxs-lookup"><span data-stu-id="df3c8-108">This pattern helps to achieve [separation of concerns](http://deviq.com/separation-of-concerns/).</span></span> <span data-ttu-id="df3c8-109">Con este patrón, se enrutan las solicitudes de usuario a un controlador que es responsable de trabajar con el modelo para realizar las acciones del usuario o recuperar los resultados de consultas.</span><span class="sxs-lookup"><span data-stu-id="df3c8-109">Using this pattern, user requests are routed to a Controller which is responsible for working with the Model to perform user actions and/or retrieve results of queries.</span></span> <span data-ttu-id="df3c8-110">El controlador elige la vista para mostrar al usuario y le proporciona los datos del modelo que requiere.</span><span class="sxs-lookup"><span data-stu-id="df3c8-110">The Controller chooses the View to display to the user, and provides it with any Model data it requires.</span></span>

<span data-ttu-id="df3c8-111">El siguiente diagrama muestra los tres componentes principales y las que hacen referencia a los demás:</span><span class="sxs-lookup"><span data-stu-id="df3c8-111">The following diagram shows the three main components and which ones reference the others:</span></span>

![Modelo de MVC](overview/_static/mvc.png)

<span data-ttu-id="df3c8-113">Este delineación de responsabilidades le ayuda a escalar la aplicación en cuanto a complejidad porque es más fácil de codificar, depurar y probar algo (modelo, vista o controlador) que tiene un único trabajo (y sigue el [principio de responsabilidad única ](http://deviq.com/single-responsibility-principle/)).</span><span class="sxs-lookup"><span data-stu-id="df3c8-113">This delineation of responsibilities helps you scale the application in terms of complexity because it’s easier to code, debug, and test something (model, view, or controller) that has a single job (and follows the [Single Responsibility Principle](http://deviq.com/single-responsibility-principle/)).</span></span> <span data-ttu-id="df3c8-114">Es más difícil de actualización, pruebas y código de depuración que tiene dependencias que se reparten entre dos o varias de estas tres áreas.</span><span class="sxs-lookup"><span data-stu-id="df3c8-114">It's more difficult to update, test, and debug code that has dependencies spread across two or more of these three areas.</span></span> <span data-ttu-id="df3c8-115">Por ejemplo, lógica de la interfaz de usuario tiende a cambiar con mayor frecuencia que la lógica de negocios.</span><span class="sxs-lookup"><span data-stu-id="df3c8-115">For example, user interface logic tends to change more frequently than business logic.</span></span> <span data-ttu-id="df3c8-116">Si la presentación código y la lógica empresarial se combina en un único objeto, tendrá que modificar un objeto que contiene la lógica de negocios cada vez que cambie la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="df3c8-116">If presentation code and business logic are combined in a single object, you have to modify an object containing business logic every time you change the user interface.</span></span> <span data-ttu-id="df3c8-117">Esto es probable que presentan errores y requerir al volver a examinar de toda la lógica de negocios después de cambiar de cada interfaz de usuario mínima.</span><span class="sxs-lookup"><span data-stu-id="df3c8-117">This is likely to introduce errors and require the retesting of all business logic after every minimal user interface change.</span></span>

> [!NOTE]
> <span data-ttu-id="df3c8-118">La vista y el controlador dependen del modelo.</span><span class="sxs-lookup"><span data-stu-id="df3c8-118">Both the view and the controller depend on the model.</span></span> <span data-ttu-id="df3c8-119">Sin embargo, el modelo depende de la vista ni el controlador.</span><span class="sxs-lookup"><span data-stu-id="df3c8-119">However, the model depends on neither the view nor the controller.</span></span> <span data-ttu-id="df3c8-120">Esta es una de las ventajas principales de la separación.</span><span class="sxs-lookup"><span data-stu-id="df3c8-120">This is one of the key benefits of the separation.</span></span> <span data-ttu-id="df3c8-121">Esta separación permite que el modelo que se compilaron y comprobaron independiente de la presentación visual.</span><span class="sxs-lookup"><span data-stu-id="df3c8-121">This separation allows the model to be built and tested independent of the visual presentation.</span></span>

### <a name="model-responsibilities"></a><span data-ttu-id="df3c8-122">Responsabilidades de modelo</span><span class="sxs-lookup"><span data-stu-id="df3c8-122">Model Responsibilities</span></span>

<span data-ttu-id="df3c8-123">El modelo en una aplicación MVC representa el estado de la aplicación y cualquier lógica de negocios o las operaciones que se deben realizar.</span><span class="sxs-lookup"><span data-stu-id="df3c8-123">The Model in an MVC application represents the state of the application and any business logic or operations that should be performed by it.</span></span> <span data-ttu-id="df3c8-124">Lógica de negocios se debe encapsular en el modelo, junto con cualquier lógica de implementación para conservar el estado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="df3c8-124">Business logic should be encapsulated in the model, along with any implementation logic for persisting the state of the application.</span></span> <span data-ttu-id="df3c8-125">Vistas fuertemente tipadas normalmente utilizan tipos ViewModel diseñados para que contenga los datos para mostrar en esa vista.</span><span class="sxs-lookup"><span data-stu-id="df3c8-125">Strongly-typed views typically use ViewModel types designed to contain the data to display on that view.</span></span> <span data-ttu-id="df3c8-126">El controlador crea y rellena estas instancias ViewModel del modelo.</span><span class="sxs-lookup"><span data-stu-id="df3c8-126">The controller creates and populates these ViewModel instances from the model.</span></span>

> [!NOTE]
> <span data-ttu-id="df3c8-127">Hay muchas maneras de organizar el modelo en una aplicación que utiliza el modelo de arquitectura de MVC.</span><span class="sxs-lookup"><span data-stu-id="df3c8-127">There are many ways to organize the model in an app that uses the MVC architectural pattern.</span></span> <span data-ttu-id="df3c8-128">Obtener más información sobre algunos [diferentes tipos de tipos de modelo](http://deviq.com/kinds-of-models/).</span><span class="sxs-lookup"><span data-stu-id="df3c8-128">Learn more about some [different kinds of model types](http://deviq.com/kinds-of-models/).</span></span>

### <a name="view-responsibilities"></a><span data-ttu-id="df3c8-129">Responsabilidades de vista</span><span class="sxs-lookup"><span data-stu-id="df3c8-129">View Responsibilities</span></span>

<span data-ttu-id="df3c8-130">Las vistas son responsables de presentar el contenido a través de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="df3c8-130">Views are responsible for presenting content through the user interface.</span></span> <span data-ttu-id="df3c8-131">Usan el [motor de vista Razor](#razor-view-engine) para incrustar código .NET en formato HTML.</span><span class="sxs-lookup"><span data-stu-id="df3c8-131">They use the [Razor view engine](#razor-view-engine) to embed .NET code in HTML markup.</span></span> <span data-ttu-id="df3c8-132">Debería haber mínimo lógica en las vistas, y debe guardar relación con toda la lógica de ellos para presentar el contenido.</span><span class="sxs-lookup"><span data-stu-id="df3c8-132">There should be minimal logic within views, and any logic in them should relate to presenting content.</span></span> <span data-ttu-id="df3c8-133">Si se encuentra la necesidad de realizar una gran cantidad de lógica en la vista archivos con el fin de mostrar los datos de un modelo complejo, considere el uso de un [del componente vista](views/view-components.md), ViewModel, o una plantilla de vista para simplificar la vista.</span><span class="sxs-lookup"><span data-stu-id="df3c8-133">If you find the need to perform a great deal of logic in view files in order to display data from a complex model, consider using a [View Component](views/view-components.md), ViewModel, or view template to simplify the view.</span></span>

### <a name="controller-responsibilities"></a><span data-ttu-id="df3c8-134">Responsabilidades de controlador</span><span class="sxs-lookup"><span data-stu-id="df3c8-134">Controller Responsibilities</span></span>

<span data-ttu-id="df3c8-135">Los controladores son los componentes que controlen la interacción del usuario, trabajan con el modelo y por último seleccionan una vista para representar.</span><span class="sxs-lookup"><span data-stu-id="df3c8-135">Controllers are the components that handle user interaction, work with the model, and ultimately select a view to render.</span></span> <span data-ttu-id="df3c8-136">En una aplicación MVC, la vista solo muestra información; el controlador administra y responde a los proporcionados por el usuario y la interacción.</span><span class="sxs-lookup"><span data-stu-id="df3c8-136">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span> <span data-ttu-id="df3c8-137">En el modelo de MVC, el controlador es el punto de entrada inicial y es responsable de seleccionar qué modelo de tipos para trabajar con y qué vista se debe representar (por lo tanto, su nombre - que controla cómo la aplicación responde a una solicitud determinada).</span><span class="sxs-lookup"><span data-stu-id="df3c8-137">In the MVC pattern, the controller is the initial entry point, and is responsible for selecting which model types to work with and which view to render (hence its name - it controls how the app responds to a given request).</span></span>

> [!NOTE]
> <span data-ttu-id="df3c8-138">Controladores no deben demasiado complicados por demasiados responsabilidades.</span><span class="sxs-lookup"><span data-stu-id="df3c8-138">Controllers shouldn't be overly complicated by too many responsibilities.</span></span> <span data-ttu-id="df3c8-139">Para conservar la lógica de controlador sea excesivamente compleja, use la [principio de responsabilidad única](http://deviq.com/single-responsibility-principle/) a la lógica de negocios de inserción fuera el controlador y en el modelo de dominio.</span><span class="sxs-lookup"><span data-stu-id="df3c8-139">To keep controller logic from becoming overly complex, use the [Single Responsibility Principle](http://deviq.com/single-responsibility-principle/) to push business logic out of the controller and into the domain model.</span></span>

>[!TIP]
> <span data-ttu-id="df3c8-140">Si encuentra que sus acciones de controlador con frecuencia realizan los mismos tipos de acciones, puede seguir el [no repita usted mismo principio](http://deviq.com/don-t-repeat-yourself/) moviendo estas acciones comunes en [filtros](#filters).</span><span class="sxs-lookup"><span data-stu-id="df3c8-140">If you find that your controller actions frequently perform the same kinds of actions, you can follow the [Don't Repeat Yourself principle](http://deviq.com/don-t-repeat-yourself/) by moving these common actions into [filters](#filters).</span></span>

## <a name="what-is-aspnet-core-mvc"></a><span data-ttu-id="df3c8-141">¿Qué es el núcleo de ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="df3c8-141">What is ASP.NET Core MVC</span></span>

<span data-ttu-id="df3c8-142">El marco de MVC de ASP.NET Core es una ligera código abierto, marco de presentación comprobables altamente optimizado para su uso con ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="df3c8-142">The ASP.NET Core MVC framework is a lightweight, open source, highly testable presentation framework optimized for use with ASP.NET Core.</span></span>

<span data-ttu-id="df3c8-143">Núcleo de ASP.NET MVC proporciona una manera basada en modelos para crear sitios Web dinámicos que permite una separación clara de intereses.</span><span class="sxs-lookup"><span data-stu-id="df3c8-143">ASP.NET Core MVC provides a patterns-based way to build dynamic websites that enables a clean separation of concerns.</span></span> <span data-ttu-id="df3c8-144">Proporciona control total sobre el marcado, admite el desarrollo para TDD rápido y utiliza los estándares web más recientes.</span><span class="sxs-lookup"><span data-stu-id="df3c8-144">It gives you full control over markup, supports TDD-friendly development and uses the latest web standards.</span></span>

## <a name="features"></a><span data-ttu-id="df3c8-145">Características</span><span class="sxs-lookup"><span data-stu-id="df3c8-145">Features</span></span>

<span data-ttu-id="df3c8-146">Núcleo de ASP.NET MVC incluye lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="df3c8-146">ASP.NET Core MVC includes the following:</span></span>

* [<span data-ttu-id="df3c8-147">Enrutamiento</span><span class="sxs-lookup"><span data-stu-id="df3c8-147">Routing</span></span>](#routing)
* [<span data-ttu-id="df3c8-148">Enlace de modelos</span><span class="sxs-lookup"><span data-stu-id="df3c8-148">Model binding</span></span>](#model-binding)
* [<span data-ttu-id="df3c8-149">Validación de modelos</span><span class="sxs-lookup"><span data-stu-id="df3c8-149">Model validation</span></span>](#model-validation)
* [<span data-ttu-id="df3c8-150">Inyección de dependencia</span><span class="sxs-lookup"><span data-stu-id="df3c8-150">Dependency injection</span></span>](../fundamentals/dependency-injection.md)
* [<span data-ttu-id="df3c8-151">Filtros</span><span class="sxs-lookup"><span data-stu-id="df3c8-151">Filters</span></span>](#filters)
* [<span data-ttu-id="df3c8-152">Áreas</span><span class="sxs-lookup"><span data-stu-id="df3c8-152">Areas</span></span>](#areas)
* [<span data-ttu-id="df3c8-153">API Web</span><span class="sxs-lookup"><span data-stu-id="df3c8-153">Web APIs</span></span>](#web-apis)
* [<span data-ttu-id="df3c8-154">Testability</span><span class="sxs-lookup"><span data-stu-id="df3c8-154">Testability</span></span>](#testability)
* [<span data-ttu-id="df3c8-155">Motor de vista Razor</span><span class="sxs-lookup"><span data-stu-id="df3c8-155">Razor view engine</span></span>](#razor-view-engine)
* [<span data-ttu-id="df3c8-156">Vistas fuertemente tipadas</span><span class="sxs-lookup"><span data-stu-id="df3c8-156">Strongly typed views</span></span>](#strongly-typed-views)
* [<span data-ttu-id="df3c8-157">Aplicaciones auxiliares de etiquetas</span><span class="sxs-lookup"><span data-stu-id="df3c8-157">Tag Helpers</span></span>](#tag-helpers)
* [<span data-ttu-id="df3c8-158">Componentes de la vista</span><span class="sxs-lookup"><span data-stu-id="df3c8-158">View Components</span></span>](#view-components)

### <a name="routing"></a><span data-ttu-id="df3c8-159">Enrutamiento</span><span class="sxs-lookup"><span data-stu-id="df3c8-159">Routing</span></span>

<span data-ttu-id="df3c8-160">Núcleo ASP.NET MVC se basa en [enrutamiento de ASP.NET Core](../fundamentals/routing.md), un eficaz componente de asignación de dirección URL que le permite compilar aplicaciones que tienen direcciones URL comprensibles y que admiten búsquedas.</span><span class="sxs-lookup"><span data-stu-id="df3c8-160">ASP.NET Core MVC is built on top of [ASP.NET Core's routing](../fundamentals/routing.md), a powerful URL-mapping component that lets you build applications that have comprehensible and searchable URLs.</span></span> <span data-ttu-id="df3c8-161">Esto le permite definir URL modelos de nomenclatura la aplicación que funcionan bien para la optimización de motor de búsqueda (SEO) y para la generación de vínculo, sin tener en cuenta cómo se organizan los archivos en el servidor web.</span><span class="sxs-lookup"><span data-stu-id="df3c8-161">This enables you to define your application's URL naming patterns that work well for search engine optimization (SEO) and for link generation, without regard for how the files on your web server are organized.</span></span> <span data-ttu-id="df3c8-162">Puede definir las rutas mediante una sintaxis de plantilla de ruta adecuada que admite las restricciones de valores de ruta, los valores predeterminados y valores opcionales.</span><span class="sxs-lookup"><span data-stu-id="df3c8-162">You can define your routes using a convenient route template syntax that supports route value constraints, defaults and optional values.</span></span>

<span data-ttu-id="df3c8-163">*Enrutamiento basado en la convención* permite definir globalmente la dirección URL de los formatos que la aplicación acepta y cómo cada uno de esos formatos se asigna a un método de acción específica en especificado controlador.</span><span class="sxs-lookup"><span data-stu-id="df3c8-163">*Convention-based routing* enables you to globally define the URL formats that your application accepts and how each of those formats maps to a specific action method on given controller.</span></span> <span data-ttu-id="df3c8-164">Cuando se recibe una solicitud entrante, el motor de enrutamiento analiza la dirección URL coincide con a uno de los formatos de dirección URL definidos y, a continuación, llama el método de acción del controlador asociado.</span><span class="sxs-lookup"><span data-stu-id="df3c8-164">When an incoming request is received, the routing engine parses the URL and matches it to one of the defined URL formats, and then calls the associated controller's action method.</span></span>

```csharp
routes.MapRoute(name: "Default", template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="df3c8-165">*Atributo enrutamiento* le permite especificar información de enrutamiento decorando los controladores y acciones con atributos que definen las rutas de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="df3c8-165">*Attribute routing* enables you to specify routing information by decorating your controllers and actions with attributes that define your application's routes.</span></span> <span data-ttu-id="df3c8-166">Esto significa que las definiciones de ruta se colocan junto al controlador y acción con la que está asociados.</span><span class="sxs-lookup"><span data-stu-id="df3c8-166">This means that your route definitions are placed next to the controller and action with which they're associated.</span></span>

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

### <a name="model-binding"></a><span data-ttu-id="df3c8-167">Enlace de modelos</span><span class="sxs-lookup"><span data-stu-id="df3c8-167">Model binding</span></span>

<span data-ttu-id="df3c8-168">Núcleo de ASP.NET MVC [enlace de modelo](models/model-binding.md) convierte los datos de la solicitud de cliente (valores de formulario, enrutar los datos, los parámetros de cadena de consulta, encabezados HTTP) en objetos que puede controlar el controlador.</span><span class="sxs-lookup"><span data-stu-id="df3c8-168">ASP.NET Core MVC [model binding](models/model-binding.md) converts client request data  (form values, route data, query string parameters, HTTP headers) into objects that the controller can handle.</span></span> <span data-ttu-id="df3c8-169">Como resultado, la lógica de controlador no tiene que realizar el trabajo de pensar en los datos de solicitud entrante; simplemente tiene los datos como parámetros a sus métodos de acción.</span><span class="sxs-lookup"><span data-stu-id="df3c8-169">As a result, your controller logic doesn't have to do the work of figuring out the incoming request data; it simply has the data as parameters to its action methods.</span></span>

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null) { ... }
   ```

### <a name="model-validation"></a><span data-ttu-id="df3c8-170">Validación de modelos</span><span class="sxs-lookup"><span data-stu-id="df3c8-170">Model validation</span></span>

<span data-ttu-id="df3c8-171">Núcleo ASP.NET MVC admite [validación](models/validation.md) decorando su objeto de modelo con los atributos de validación de anotación de datos.</span><span class="sxs-lookup"><span data-stu-id="df3c8-171">ASP.NET Core MVC supports [validation](models/validation.md) by decorating your model object with data annotation validation attributes.</span></span> <span data-ttu-id="df3c8-172">Los atributos de validación se comprueban en el cliente antes de que los valores se registran en el servidor, así como en el servidor antes de la acción de controlador se llama.</span><span class="sxs-lookup"><span data-stu-id="df3c8-172">The validation attributes are checked on the client side before values are posted to the server, as well as on the server before the controller action is called.</span></span>

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

<span data-ttu-id="df3c8-173">Una acción de controlador:</span><span class="sxs-lookup"><span data-stu-id="df3c8-173">A controller action:</span></span>

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

<span data-ttu-id="df3c8-174">El marco de trabajo administra datos de la solicitud de validación en el cliente y en el servidor.</span><span class="sxs-lookup"><span data-stu-id="df3c8-174">The framework handles validating request data both on the client and on the server.</span></span> <span data-ttu-id="df3c8-175">Lógica de validación especificada en tipos de modelo se agrega a las vistas representadas como anotaciones discretas y se aplica en el explorador con [jQuery validación](https://jqueryvalidation.org/).</span><span class="sxs-lookup"><span data-stu-id="df3c8-175">Validation logic specified on model types is added to the rendered views as unobtrusive annotations and is enforced in the browser with [jQuery Validation](https://jqueryvalidation.org/).</span></span>

### <a name="dependency-injection"></a><span data-ttu-id="df3c8-176">Inyección de dependencia</span><span class="sxs-lookup"><span data-stu-id="df3c8-176">Dependency injection</span></span>

<span data-ttu-id="df3c8-177">ASP.NET Core tiene compatibilidad integrada para [inyección de dependencia (DI)](../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="df3c8-177">ASP.NET Core has built-in support for [dependency injection (DI)](../fundamentals/dependency-injection.md).</span></span> <span data-ttu-id="df3c8-178">En ASP.NET MVC de núcleo, [controladores](controllers/dependency-injection.md) puede solicitud necesita servicios a través de sus constructores, lo que les permite seguir el [principio de dependencias explícitas](http://deviq.com/explicit-dependencies-principle/).</span><span class="sxs-lookup"><span data-stu-id="df3c8-178">In ASP.NET Core MVC, [controllers](controllers/dependency-injection.md) can request needed services through their constructors, allowing them to follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span>

<span data-ttu-id="df3c8-179">También puede usar su aplicación [inyección de dependencia en la vista archivos](views/dependency-injection.md), usando la `@inject` directiva:</span><span class="sxs-lookup"><span data-stu-id="df3c8-179">Your app can also use [dependency injection in view files](views/dependency-injection.md), using the `@inject` directive:</span></span>

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

### <a name="filters"></a><span data-ttu-id="df3c8-180">Filtros</span><span class="sxs-lookup"><span data-stu-id="df3c8-180">Filters</span></span>

<span data-ttu-id="df3c8-181">[Filtros](controllers/filters.md) ayudan a los programadores encapsular preocupaciones transversales, como control de excepciones o autorización.</span><span class="sxs-lookup"><span data-stu-id="df3c8-181">[Filters](controllers/filters.md) help developers encapsulate cross-cutting concerns, like exception handling or authorization.</span></span> <span data-ttu-id="df3c8-182">Filtros habilitar personalizada activo lógica previa y posteriores al procesamiento para los métodos de acción y pueden configurarse para ejecutarse en ciertos puntos dentro de la canalización de ejecución de una solicitud determinada.</span><span class="sxs-lookup"><span data-stu-id="df3c8-182">Filters enable running custom pre- and post-processing logic for action methods, and can be configured to run at certain points within the execution pipeline for a given request.</span></span> <span data-ttu-id="df3c8-183">Los filtros pueden aplicarse a los controladores o acciones como atributos (o pueden realizarse globalmente).</span><span class="sxs-lookup"><span data-stu-id="df3c8-183">Filters can be applied to controllers or actions as attributes (or can be run globally).</span></span> <span data-ttu-id="df3c8-184">Varios filtros (como `Authorize`) se incluyen en el marco de trabajo.</span><span class="sxs-lookup"><span data-stu-id="df3c8-184">Several filters (such as `Authorize`) are included in the framework.</span></span>


```csharp
[Authorize]
   public class AccountController : Controller
   {
```

### <a name="areas"></a><span data-ttu-id="df3c8-185">Áreas</span><span class="sxs-lookup"><span data-stu-id="df3c8-185">Areas</span></span>

<span data-ttu-id="df3c8-186">[Áreas](controllers/areas.md) proporcionan una manera de dividir una aplicación Web de MVC de ASP.NET Core grande en agrupaciones funcionales más pequeñas.</span><span class="sxs-lookup"><span data-stu-id="df3c8-186">[Areas](controllers/areas.md) provide a way to partition a large ASP.NET Core MVC Web app into smaller functional groupings.</span></span> <span data-ttu-id="df3c8-187">Un área es una estructura MVC dentro de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="df3c8-187">An area is an MVC structure inside an application.</span></span> <span data-ttu-id="df3c8-188">En un proyecto MVC, los componentes lógicos como modelo, el controlador y la vista se guardan en carpetas diferentes y MVC usa las convenciones de nomenclatura para crear la relación entre estos componentes.</span><span class="sxs-lookup"><span data-stu-id="df3c8-188">In an MVC project, logical components like Model, Controller, and View are kept in different folders, and MVC uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="df3c8-189">Para una aplicación grande, puede ser conveniente dividir la aplicación en distintas áreas de nivel alto de funcionalidad.</span><span class="sxs-lookup"><span data-stu-id="df3c8-189">For a large app, it may be advantageous to partition the app into separate high level areas of functionality.</span></span> <span data-ttu-id="df3c8-190">Por ejemplo, una aplicación de comercio electrónico con varias unidades de negocio, como la desprotección, facturación y búsqueda etcetera. Cada una de estas unidades tienen sus propias vistas de componente lógico, controladores y modelos.</span><span class="sxs-lookup"><span data-stu-id="df3c8-190">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search etc. Each of these units have their own logical component views, controllers, and models.</span></span>

### <a name="web-apis"></a><span data-ttu-id="df3c8-191">API web</span><span class="sxs-lookup"><span data-stu-id="df3c8-191">Web APIs</span></span>

<span data-ttu-id="df3c8-192">Además de ser una plataforma excelente para la creación de sitios web, MVC de ASP.NET Core tiene mayor compatibilidad para la creación de las API Web.</span><span class="sxs-lookup"><span data-stu-id="df3c8-192">In addition to being a great platform for building web sites, ASP.NET Core MVC has great support for building Web APIs.</span></span> <span data-ttu-id="df3c8-193">Puede crear servicios que llegan a una amplia gama de clientes, incluidos los exploradores y dispositivos móviles.</span><span class="sxs-lookup"><span data-stu-id="df3c8-193">You can build services that reach a broad range of clients including browsers and mobile devices.</span></span>

<span data-ttu-id="df3c8-194">El marco incluye compatibilidad de la negociación de contenido HTTP con compatibilidad integrada para [aplicar formato a datos](models/formatting.md) como JSON o XML.</span><span class="sxs-lookup"><span data-stu-id="df3c8-194">The framework includes support for HTTP content-negotiation with built-in support for [formatting data](models/formatting.md) as JSON or XML.</span></span> <span data-ttu-id="df3c8-195">Escribir [formateadores personalizados](advanced/custom-formatters.md) para agregar compatibilidad con sus propios formatos.</span><span class="sxs-lookup"><span data-stu-id="df3c8-195">Write [custom formatters](advanced/custom-formatters.md) to add support for your own formats.</span></span>

<span data-ttu-id="df3c8-196">Utilice la generación de vínculo para habilitar la compatibilidad para hipermedia.</span><span class="sxs-lookup"><span data-stu-id="df3c8-196">Use link generation to enable support for hypermedia.</span></span> <span data-ttu-id="df3c8-197">Habilitar la compatibilidad con facilidad [recursos entre orígenes (CORS) de uso compartido](http://www.w3.org/TR/cors/) para que las API Web se pueden compartir entre varias aplicaciones Web.</span><span class="sxs-lookup"><span data-stu-id="df3c8-197">Easily enable support for [cross-origin resource sharing (CORS)](http://www.w3.org/TR/cors/) so that your Web APIs can be shared across multiple Web applications.</span></span>

### <a name="testability"></a><span data-ttu-id="df3c8-198">Capacidad de prueba</span><span class="sxs-lookup"><span data-stu-id="df3c8-198">Testability</span></span>

<span data-ttu-id="df3c8-199">Uso del marco de trabajo de inserción de dependencias e interfaces hacer adecuadas a las pruebas unitarias y el marco de trabajo incluye características (por ejemplo, un proveedor TestHost y InMemory para Entity Framework) que hacen [las pruebas de integración](../testing/integration-testing.md) rápido y fácil así.</span><span class="sxs-lookup"><span data-stu-id="df3c8-199">The framework's use of interfaces and dependency injection make it well-suited to unit testing, and the framework includes features (like a TestHost and InMemory provider for Entity Framework) that make [integration testing](../testing/integration-testing.md) quick and easy as well.</span></span> <span data-ttu-id="df3c8-200">Obtenga más información sobre [para probar la lógica de controlador](controllers/testing.md).</span><span class="sxs-lookup"><span data-stu-id="df3c8-200">Learn more about [testing controller logic](controllers/testing.md).</span></span>

### <a name="razor-view-engine"></a><span data-ttu-id="df3c8-201">Motor de vista Razor</span><span class="sxs-lookup"><span data-stu-id="df3c8-201">Razor view engine</span></span>

<span data-ttu-id="df3c8-202">[Vistas ASP.NET MVC Core](views/overview.md) usar la [motor de vista Razor](views/razor.md) para representar vistas.</span><span class="sxs-lookup"><span data-stu-id="df3c8-202">[ASP.NET Core MVC views](views/overview.md) use the [Razor view engine](views/razor.md) to render views.</span></span> <span data-ttu-id="df3c8-203">Razor es un lenguaje de marcado de plantilla compact, expresivo y fluido para definir vistas que utilizan código incrustado de C#.</span><span class="sxs-lookup"><span data-stu-id="df3c8-203">Razor is a compact, expressive and fluid template markup language for defining views using embedded C# code.</span></span> <span data-ttu-id="df3c8-204">Razor se usa para generar dinámicamente el contenido web en el servidor.</span><span class="sxs-lookup"><span data-stu-id="df3c8-204">Razor is used to dynamically generate web content on the server.</span></span> <span data-ttu-id="df3c8-205">Limpiamente puede mezclar código de servidor con el código y contenido del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="df3c8-205">You can cleanly mix server code with client side content and code.</span></span>

```text
<ul>
  @for (int i = 0; i < 5; i++) {
    <li>List item @i</li>
  }
</ul>
```

<span data-ttu-id="df3c8-206">Con el motor de vista Razor, puede definir [diseños](views/layout.md), [vistas parciales](views/partial.md) y secciones reemplazables.</span><span class="sxs-lookup"><span data-stu-id="df3c8-206">Using the Razor view engine you can define [layouts](views/layout.md), [partial views](views/partial.md) and replaceable sections.</span></span>

### <a name="strongly-typed-views"></a><span data-ttu-id="df3c8-207">Vistas fuertemente tipadas</span><span class="sxs-lookup"><span data-stu-id="df3c8-207">Strongly typed views</span></span>

<span data-ttu-id="df3c8-208">Vistas de Razor en MVC pueden estar fuertemente tipadas en función de su modelo.</span><span class="sxs-lookup"><span data-stu-id="df3c8-208">Razor views in MVC can be strongly typed based on your model.</span></span> <span data-ttu-id="df3c8-209">Controladores pueden pasar un modelo fuertemente tipado para habilitar las vistas para que IntelliSense admite y comprobación de tipos de vistas.</span><span class="sxs-lookup"><span data-stu-id="df3c8-209">Controllers can pass a strongly typed model to views enabling your views to have type checking and IntelliSense support.</span></span>

<span data-ttu-id="df3c8-210">Por ejemplo, la vista siguiente representa un modelo de tipo `IEnumerable<Product>`:</span><span class="sxs-lookup"><span data-stu-id="df3c8-210">For example, the following view renders a model of type `IEnumerable<Product>`:</span></span>

```cshtml
@model IEnumerable<Product>
<ul>
    @foreach (Product p in Model)
    {
        <li>@p.Name</li>
    }
</ul>
```

### <a name="tag-helpers"></a><span data-ttu-id="df3c8-211">Aplicaciones auxiliares de etiquetas</span><span class="sxs-lookup"><span data-stu-id="df3c8-211">Tag Helpers</span></span>

<span data-ttu-id="df3c8-212">[Aplicaciones auxiliares de etiquetas](views/tag-helpers/intro.md) habilitar el código de servidor participar en la creación y representar elementos HTML en archivos de Razor.</span><span class="sxs-lookup"><span data-stu-id="df3c8-212">[Tag Helpers](views/tag-helpers/intro.md) enable server side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="df3c8-213">Puede usar aplicaciones auxiliares de etiquetas para definir etiquetas personalizadas (por ejemplo, `<environment>`) o para modificar el comportamiento de las etiquetas existentes (por ejemplo, `<label>`).</span><span class="sxs-lookup"><span data-stu-id="df3c8-213">You can use tag helpers to define custom tags (for example, `<environment>`) or to modify the behavior of existing tags (for example, `<label>`).</span></span> <span data-ttu-id="df3c8-214">Aplicaciones auxiliares de etiquetas enlazar a elementos específicos que se basa en el nombre del elemento y sus atributos.</span><span class="sxs-lookup"><span data-stu-id="df3c8-214">Tag Helpers bind to specific elements based on the element name and its attributes.</span></span> <span data-ttu-id="df3c8-215">Proporcionan las ventajas de la representación del lado servidor al tiempo que se mantiene una experiencia de edición HTML.</span><span class="sxs-lookup"><span data-stu-id="df3c8-215">They provide the benefits of server-side rendering while still preserving an HTML editing experience.</span></span>

<span data-ttu-id="df3c8-216">Hay muchas aplicaciones auxiliares de etiquetas integradas para las tareas comunes: como la creación de formularios, vínculos, activos de carga y los paquetes más - y aún más disponibles en repositorios públicos de GitHub y como NuGet.</span><span class="sxs-lookup"><span data-stu-id="df3c8-216">There are many built-in Tag Helpers for common tasks - such as creating forms, links, loading assets and more - and even more available in public GitHub repositories and as NuGet packages.</span></span> <span data-ttu-id="df3c8-217">Aplicaciones auxiliares de etiquetas se crean en C# y se dirigen a los elementos HTML en función de nombre de elemento o atributo o etiqueta primaria.</span><span class="sxs-lookup"><span data-stu-id="df3c8-217">Tag Helpers are authored in C#, and they target HTML elements based on element name, attribute name, or parent tag.</span></span> <span data-ttu-id="df3c8-218">Por ejemplo, la integrada LinkTagHelper puede utilizarse para crear un vínculo a la `Login` acción de la `AccountsController`:</span><span class="sxs-lookup"><span data-stu-id="df3c8-218">For example, the built-in LinkTagHelper can be used to create a link to the `Login` action of the `AccountsController`:</span></span>

```cshtml
<p>
    Thank you for confirming your email.
    Please <a asp-controller="Account" asp-action="Login">Click here to Log in</a>.
</p>
```

<span data-ttu-id="df3c8-219">La `EnvironmentTagHelper` puede usarse para incluir scripts diferentes en las vistas (por ejemplo, sin formato o reducida) según el entorno en tiempo de ejecución, como desarrollo, ensayo o producción:</span><span class="sxs-lookup"><span data-stu-id="df3c8-219">The `EnvironmentTagHelper` can be used to include different scripts in your views (for example, raw or minified) based on the runtime environment, such as Development, Staging, or Production:</span></span>

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

<span data-ttu-id="df3c8-220">Aplicaciones auxiliares de etiquetas proporcionan una experiencia de desarrollo compatible con HTML y un entorno rico de IntelliSense para crear marcado HTML y Razor.</span><span class="sxs-lookup"><span data-stu-id="df3c8-220">Tag Helpers provide an HTML-friendly development experience and a rich IntelliSense environment for creating HTML and Razor markup.</span></span> <span data-ttu-id="df3c8-221">La mayoría de las aplicaciones auxiliares de etiquetas integradas elementos HTML existentes de destino y proporciona atributos de servidor para el elemento.</span><span class="sxs-lookup"><span data-stu-id="df3c8-221">Most of the built-in Tag Helpers target existing HTML elements and provide server-side attributes for the element.</span></span>

### <a name="view-components"></a><span data-ttu-id="df3c8-222">Componentes de la vista</span><span class="sxs-lookup"><span data-stu-id="df3c8-222">View Components</span></span>

<span data-ttu-id="df3c8-223">[Ver componentes](views/view-components.md) permiten empaquetar la lógica de representación y volver a lo largo de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="df3c8-223">[View Components](views/view-components.md) allow you to package rendering logic and reuse it throughout the application.</span></span> <span data-ttu-id="df3c8-224">Son similares a [vistas parciales](views/partial.md), pero con lógica asociada.</span><span class="sxs-lookup"><span data-stu-id="df3c8-224">They're similar to [partial views](views/partial.md), but with associated logic.</span></span>
