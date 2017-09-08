---
title: "Información general del núcleo de ASP.NET MVC"
author: ardalis
description: 
keywords: "Núcleo de ASP.NET,"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 89af38d1-52e0-4db7-b791-dbce909b0714
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/overview
ms.openlocfilehash: 3e95ccd8aeda54a050cd656ea0c831fd928f53a2
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="overview-of-aspnet-core-mvc"></a><span data-ttu-id="38f7b-103">Información general del núcleo de ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="38f7b-103">Overview of ASP.NET Core MVC</span></span>

<span data-ttu-id="38f7b-104">Por [Steve Smith](http://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="38f7b-104">By [Steve Smith](http://ardalis.com)</span></span>

<span data-ttu-id="38f7b-105">Núcleo de ASP.NET MVC es un marco completo para la creación de aplicaciones web y API que usan el modelo Model-View-Controller patrón de diseño.</span><span class="sxs-lookup"><span data-stu-id="38f7b-105">ASP.NET Core MVC is a rich framework for building web apps and APIs using the Model-View-Controller design pattern.</span></span>

## <a name="what-is-the-mvc-pattern"></a><span data-ttu-id="38f7b-106">¿Qué es el modelo de MVC?</span><span class="sxs-lookup"><span data-stu-id="38f7b-106">What is the MVC pattern?</span></span>

<span data-ttu-id="38f7b-107">El modelo de arquitectura Model-View-Controller (MVC) separa una aplicación en tres grupos principales de componentes: modelos, vistas y controladores.</span><span class="sxs-lookup"><span data-stu-id="38f7b-107">The Model-View-Controller (MVC) architectural pattern separates an application into three main groups of components: Models, Views, and Controllers.</span></span> <span data-ttu-id="38f7b-108">Este patrón ayuda a lograr [separación de intereses](http://deviq.com/separation-of-concerns/).</span><span class="sxs-lookup"><span data-stu-id="38f7b-108">This pattern helps to achieve [separation of concerns](http://deviq.com/separation-of-concerns/).</span></span> <span data-ttu-id="38f7b-109">Con este patrón, se enrutan las solicitudes de usuario a un controlador que es responsable de trabajar con el modelo para realizar las acciones del usuario o recuperar los resultados de consultas.</span><span class="sxs-lookup"><span data-stu-id="38f7b-109">Using this pattern, user requests are routed to a Controller which is responsible for working with the Model to perform user actions and/or retrieve results of queries.</span></span> <span data-ttu-id="38f7b-110">El controlador elige la vista para mostrar al usuario y le proporciona los datos del modelo que requiere.</span><span class="sxs-lookup"><span data-stu-id="38f7b-110">The Controller chooses the View to display to the user, and provides it with any Model data it requires.</span></span>

<span data-ttu-id="38f7b-111">El siguiente diagrama muestra los tres componentes principales y las que hacen referencia a los demás:</span><span class="sxs-lookup"><span data-stu-id="38f7b-111">The following diagram shows the three main components and which ones reference the others:</span></span>

![Modelo de MVC](overview/_static/mvc.png)

<span data-ttu-id="38f7b-113">Este delineación de responsabilidades le ayuda a escalar la aplicación en cuanto a complejidad porque es más fácil de codificar, depurar y probar algo (modelo, vista o controlador) que tiene un único trabajo (y sigue el [principio de responsabilidad única ](http://deviq.com/single-responsibility-principle/)).</span><span class="sxs-lookup"><span data-stu-id="38f7b-113">This delineation of responsibilities helps you scale the application in terms of complexity because it’s easier to code, debug, and test something (model, view, or controller) that has a single job (and follows the [Single Responsibility Principle](http://deviq.com/single-responsibility-principle/)).</span></span> <span data-ttu-id="38f7b-114">Es más difícil de actualización, pruebas y código de depuración que tiene dependencias que se reparten entre dos o varias de estas tres áreas.</span><span class="sxs-lookup"><span data-stu-id="38f7b-114">It's more difficult to update, test, and debug code that has dependencies spread across two or more of these three areas.</span></span> <span data-ttu-id="38f7b-115">Por ejemplo, lógica de la interfaz de usuario tiende a cambiar con mayor frecuencia que la lógica de negocios.</span><span class="sxs-lookup"><span data-stu-id="38f7b-115">For example, user interface logic tends to change more frequently than business logic.</span></span> <span data-ttu-id="38f7b-116">Si la presentación código y la lógica empresarial se combina en un único objeto, tendrá que modificar un objeto que contiene la lógica de negocios cada vez que cambie la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="38f7b-116">If presentation code and business logic are combined in a single object, you have to modify an object containing business logic every time you change the user interface.</span></span> <span data-ttu-id="38f7b-117">Esto es probable que presentan errores y requerir al volver a examinar de toda la lógica de negocios después de cambiar de cada interfaz de usuario mínima.</span><span class="sxs-lookup"><span data-stu-id="38f7b-117">This is likely to introduce errors and require the retesting of all business logic after every minimal user interface change.</span></span>

> [!NOTE]
> <span data-ttu-id="38f7b-118">La vista y el controlador dependen del modelo.</span><span class="sxs-lookup"><span data-stu-id="38f7b-118">Both the view and the controller depend on the model.</span></span> <span data-ttu-id="38f7b-119">Sin embargo, el modelo depende de la vista ni el controlador.</span><span class="sxs-lookup"><span data-stu-id="38f7b-119">However, the model depends on neither the view nor the controller.</span></span> <span data-ttu-id="38f7b-120">Esta es una de las ventajas principales de la separación.</span><span class="sxs-lookup"><span data-stu-id="38f7b-120">This is one of the key benefits of the separation.</span></span> <span data-ttu-id="38f7b-121">Esta separación permite que el modelo que se compilaron y comprobaron independiente de la presentación visual.</span><span class="sxs-lookup"><span data-stu-id="38f7b-121">This separation allows the model to be built and tested independent of the visual presentation.</span></span>

### <a name="model-responsibilities"></a><span data-ttu-id="38f7b-122">Responsabilidades de modelo</span><span class="sxs-lookup"><span data-stu-id="38f7b-122">Model Responsibilities</span></span>

<span data-ttu-id="38f7b-123">El modelo en una aplicación MVC representa el estado de la aplicación y cualquier lógica de negocios o las operaciones que se deben realizar.</span><span class="sxs-lookup"><span data-stu-id="38f7b-123">The Model in an MVC application represents the state of the application and any business logic or operations that should be performed by it.</span></span> <span data-ttu-id="38f7b-124">Lógica de negocios se debe encapsular en el modelo, junto con cualquier lógica de implementación para conservar el estado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="38f7b-124">Business logic should be encapsulated in the model, along with any implementation logic for persisting the state of the application.</span></span> <span data-ttu-id="38f7b-125">Vistas fuertemente tipadas normalmente utilizará tipos ViewModel diseñados específicamente para que contenga los datos para mostrar en esa vista; el controlador creará y rellenar estas instancias ViewModel del modelo.</span><span class="sxs-lookup"><span data-stu-id="38f7b-125">Strongly-typed views will typically use ViewModel types specifically designed to contain the data to display on that view; the controller will create and populate these ViewModel instances from the model.</span></span>

> [!NOTE]
> <span data-ttu-id="38f7b-126">Hay muchas maneras de organizar el modelo en una aplicación que utiliza el modelo de arquitectura de MVC.</span><span class="sxs-lookup"><span data-stu-id="38f7b-126">There are many ways to organize the model in an app that uses the MVC architectural pattern.</span></span> <span data-ttu-id="38f7b-127">Obtener más información sobre algunos [diferentes tipos de tipos de modelo](http://deviq.com/kinds-of-models/).</span><span class="sxs-lookup"><span data-stu-id="38f7b-127">Learn more about some [different kinds of model types](http://deviq.com/kinds-of-models/).</span></span>

### <a name="view-responsibilities"></a><span data-ttu-id="38f7b-128">Responsabilidades de vista</span><span class="sxs-lookup"><span data-stu-id="38f7b-128">View Responsibilities</span></span>

<span data-ttu-id="38f7b-129">Las vistas son responsables de presentar el contenido a través de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="38f7b-129">Views are responsible for presenting content through the user interface.</span></span> <span data-ttu-id="38f7b-130">Usan el [motor de vista Razor](#razor-view-engine) para incrustar código .NET en formato HTML.</span><span class="sxs-lookup"><span data-stu-id="38f7b-130">They use the [Razor view engine](#razor-view-engine) to embed .NET code in HTML markup.</span></span> <span data-ttu-id="38f7b-131">Debería haber mínimo lógica en las vistas, y debe guardar relación con toda la lógica de ellos para presentar el contenido.</span><span class="sxs-lookup"><span data-stu-id="38f7b-131">There should be minimal logic within views, and any logic in them should relate to presenting content.</span></span> <span data-ttu-id="38f7b-132">Si se encuentra la necesidad de realizar una gran cantidad de lógica en la vista archivos con el fin de mostrar los datos de un modelo complejo, considere el uso de un [del componente vista](views/view-components.md), ViewModel, o una plantilla de vista para simplificar la vista.</span><span class="sxs-lookup"><span data-stu-id="38f7b-132">If you find the need to perform a great deal of logic in view files in order to display data from a complex model, consider using a [View Component](views/view-components.md), ViewModel, or view template to simplify the view.</span></span>

### <a name="controller-responsibilities"></a><span data-ttu-id="38f7b-133">Responsabilidades de controlador</span><span class="sxs-lookup"><span data-stu-id="38f7b-133">Controller Responsibilities</span></span>

<span data-ttu-id="38f7b-134">Los controladores son los componentes que controlen la interacción del usuario, trabajan con el modelo y por último seleccionan una vista para representar.</span><span class="sxs-lookup"><span data-stu-id="38f7b-134">Controllers are the components that handle user interaction, work with the model, and ultimately select a view to render.</span></span> <span data-ttu-id="38f7b-135">En una aplicación MVC, la vista solo muestra información; el controlador administra y responde a los proporcionados por el usuario y la interacción.</span><span class="sxs-lookup"><span data-stu-id="38f7b-135">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span> <span data-ttu-id="38f7b-136">En el modelo de MVC, el controlador es el punto de entrada inicial y es responsable de seleccionar qué modelo de tipos para trabajar con y qué vista se debe representar (por lo tanto, su nombre - que controla cómo la aplicación responde a una solicitud determinada).</span><span class="sxs-lookup"><span data-stu-id="38f7b-136">In the MVC pattern, the controller is the initial entry point, and is responsible for selecting which model types to work with and which view to render (hence its name - it controls how the app responds to a given request).</span></span>

> [!NOTE]
> <span data-ttu-id="38f7b-137">Controladores no deben ser demasiado complicados por demasiados responsabilidades.</span><span class="sxs-lookup"><span data-stu-id="38f7b-137">Controllers should not be overly complicated by too many responsibilities.</span></span> <span data-ttu-id="38f7b-138">Para conservar la lógica de controlador sea excesivamente compleja, use la [principio de responsabilidad única](http://deviq.com/single-responsibility-principle/) a la lógica de negocios de inserción fuera el controlador y en el modelo de dominio.</span><span class="sxs-lookup"><span data-stu-id="38f7b-138">To keep controller logic from becoming overly complex, use the [Single Responsibility Principle](http://deviq.com/single-responsibility-principle/) to push business logic out of the controller and into the domain model.</span></span>

>[!TIP]
> <span data-ttu-id="38f7b-139">Si encuentra que sus acciones de controlador con frecuencia realizan los mismos tipos de acciones, puede seguir el [no repita usted mismo principio](http://deviq.com/don-t-repeat-yourself/) moviendo estas acciones comunes en [filtros](#filters).</span><span class="sxs-lookup"><span data-stu-id="38f7b-139">If you find that your controller actions frequently perform the same kinds of actions, you can follow the [Don't Repeat Yourself principle](http://deviq.com/don-t-repeat-yourself/) by moving these common actions into [filters](#filters).</span></span>

## <a name="what-is-aspnet-core-mvc"></a><span data-ttu-id="38f7b-140">¿Qué es el núcleo de ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="38f7b-140">What is ASP.NET Core MVC</span></span>

<span data-ttu-id="38f7b-141">El marco de MVC de ASP.NET Core es una ligera código abierto, marco de presentación comprobables altamente optimizado para su uso con ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="38f7b-141">The ASP.NET Core MVC framework is a lightweight, open source, highly testable presentation framework optimized for use with ASP.NET Core.</span></span>

<span data-ttu-id="38f7b-142">Núcleo de ASP.NET MVC proporciona una manera basada en modelos para crear sitios Web dinámicos que permite una separación clara de intereses.</span><span class="sxs-lookup"><span data-stu-id="38f7b-142">ASP.NET Core MVC provides a patterns-based way to build dynamic websites that enables a clean separation of concerns.</span></span> <span data-ttu-id="38f7b-143">Proporciona control total sobre el marcado, admite el desarrollo para TDD rápido y utiliza los estándares web más recientes.</span><span class="sxs-lookup"><span data-stu-id="38f7b-143">It gives you full control over markup, supports TDD-friendly development and uses the latest web standards.</span></span>

## <a name="features"></a><span data-ttu-id="38f7b-144">Características</span><span class="sxs-lookup"><span data-stu-id="38f7b-144">Features</span></span>

<span data-ttu-id="38f7b-145">Núcleo de ASP.NET MVC incluye lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="38f7b-145">ASP.NET Core MVC includes the following:</span></span>

* [<span data-ttu-id="38f7b-146">Enrutamiento</span><span class="sxs-lookup"><span data-stu-id="38f7b-146">Routing</span></span>](#routing)
* [<span data-ttu-id="38f7b-147">Enlace de modelos</span><span class="sxs-lookup"><span data-stu-id="38f7b-147">Model binding</span></span>](#model-binding)
* [<span data-ttu-id="38f7b-148">Validación del modelo</span><span class="sxs-lookup"><span data-stu-id="38f7b-148">Model validation</span></span>](#model-validation)
* [<span data-ttu-id="38f7b-149">Inyección de dependencia</span><span class="sxs-lookup"><span data-stu-id="38f7b-149">Dependency injection</span></span>](../fundamentals/dependency-injection.md)
* [<span data-ttu-id="38f7b-150">Filtros</span><span class="sxs-lookup"><span data-stu-id="38f7b-150">Filters</span></span>](#filters)
* [<span data-ttu-id="38f7b-151">Áreas</span><span class="sxs-lookup"><span data-stu-id="38f7b-151">Areas</span></span>](#areas)
* [<span data-ttu-id="38f7b-152">API Web</span><span class="sxs-lookup"><span data-stu-id="38f7b-152">Web APIs</span></span>](#web-apis)
* [<span data-ttu-id="38f7b-153">Capacidad de prueba</span><span class="sxs-lookup"><span data-stu-id="38f7b-153">Testability</span></span>](#testability)
* [<span data-ttu-id="38f7b-154">Motor de vista Razor</span><span class="sxs-lookup"><span data-stu-id="38f7b-154">Razor view engine</span></span>](#razor-view-engine)
* [<span data-ttu-id="38f7b-155">Vistas fuertemente tipadas</span><span class="sxs-lookup"><span data-stu-id="38f7b-155">Strongly typed views</span></span>](#strongly-typed-views)
* [<span data-ttu-id="38f7b-156">Aplicaciones auxiliares de etiquetas</span><span class="sxs-lookup"><span data-stu-id="38f7b-156">Tag Helpers</span></span>](#tag-helpers)
* [<span data-ttu-id="38f7b-157">Componentes de la vista</span><span class="sxs-lookup"><span data-stu-id="38f7b-157">View Components</span></span>](#view-components)

### <a name="routing"></a><span data-ttu-id="38f7b-158">Enrutamiento</span><span class="sxs-lookup"><span data-stu-id="38f7b-158">Routing</span></span>

<span data-ttu-id="38f7b-159">Núcleo ASP.NET MVC se basa en [enrutamiento de ASP.NET Core](../fundamentals/routing.md), un eficaz componente de asignación de dirección URL que le permite compilar aplicaciones que tienen direcciones URL comprensibles y que admiten búsquedas.</span><span class="sxs-lookup"><span data-stu-id="38f7b-159">ASP.NET Core MVC is built on top of [ASP.NET Core's routing](../fundamentals/routing.md), a powerful URL-mapping component that lets you build applications that have comprehensible and searchable URLs.</span></span> <span data-ttu-id="38f7b-160">Esto le permite definir URL modelos de nomenclatura la aplicación que funcionan bien para la optimización de motor de búsqueda (SEO) y para la generación de vínculo, sin tener en cuenta cómo se organizan los archivos en el servidor web.</span><span class="sxs-lookup"><span data-stu-id="38f7b-160">This enables you to define your application's URL naming patterns that work well for search engine optimization (SEO) and for link generation, without regard for how the files on your web server are organized.</span></span> <span data-ttu-id="38f7b-161">Puede definir las rutas mediante una sintaxis de plantilla de ruta adecuada que admite las restricciones de valores de ruta, los valores predeterminados y valores opcionales.</span><span class="sxs-lookup"><span data-stu-id="38f7b-161">You can define your routes using a convenient route template syntax that supports route value constraints, defaults and optional values.</span></span>

<span data-ttu-id="38f7b-162">*Enrutamiento basado en la convención* permite definir globalmente la dirección URL de los formatos que la aplicación acepta y cómo cada uno de esos formatos se asigna a un método de acción específica en especificado controlador.</span><span class="sxs-lookup"><span data-stu-id="38f7b-162">*Convention-based routing* enables you to globally define the URL formats that your application accepts and how each of those formats maps to a specific action method on given controller.</span></span> <span data-ttu-id="38f7b-163">Cuando se recibe una solicitud entrante, el motor de enrutamiento analiza la dirección URL coincide con a uno de los formatos de dirección URL definidos y, a continuación, llama el método de acción del controlador asociado.</span><span class="sxs-lookup"><span data-stu-id="38f7b-163">When an incoming request is received, the routing engine parses the URL and matches it to one of the defined URL formats, and then calls the associated controller's action method.</span></span>

```csharp
routes.MapRoute(name: "Default", template: "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="38f7b-164">*Atributo enrutamiento* le permite especificar información de enrutamiento decorando los controladores y acciones con atributos que definen las rutas de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="38f7b-164">*Attribute routing* enables you to specify routing information by decorating your controllers and actions with attributes that define your application's routes.</span></span> <span data-ttu-id="38f7b-165">Esto significa que las definiciones de ruta se colocan junto al controlador y acción con la que está asociados.</span><span class="sxs-lookup"><span data-stu-id="38f7b-165">This means that your route definitions are placed next to the controller and action with which they're associated.</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [1, 4]}} -->

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

### <a name="model-binding"></a><span data-ttu-id="38f7b-166">Enlace de modelos</span><span class="sxs-lookup"><span data-stu-id="38f7b-166">Model binding</span></span>

<span data-ttu-id="38f7b-167">Núcleo de ASP.NET MVC [enlace de modelo](models/model-binding.md) convierte los datos de la solicitud de cliente (valores de formulario, enrutar los datos, los parámetros de cadena de consulta, encabezados HTTP) en objetos que puede controlar el controlador.</span><span class="sxs-lookup"><span data-stu-id="38f7b-167">ASP.NET Core MVC [model binding](models/model-binding.md) converts client request data  (form values, route data, query string parameters, HTTP headers) into objects that the controller can handle.</span></span> <span data-ttu-id="38f7b-168">Como resultado, la lógica de controlador no tiene que realizar el trabajo de pensar en los datos de solicitud entrante; simplemente tiene los datos como parámetros a sus métodos de acción.</span><span class="sxs-lookup"><span data-stu-id="38f7b-168">As a result, your controller logic doesn't have to do the work of figuring out the incoming request data; it simply has the data as parameters to its action methods.</span></span>

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null) { ... }
   ```

### <a name="model-validation"></a><span data-ttu-id="38f7b-169">validación del modelo</span><span class="sxs-lookup"><span data-stu-id="38f7b-169">Model validation</span></span>

<span data-ttu-id="38f7b-170">Núcleo ASP.NET MVC admite [validación](models/validation.md) decorando su objeto de modelo con los atributos de validación de anotación de datos.</span><span class="sxs-lookup"><span data-stu-id="38f7b-170">ASP.NET Core MVC supports [validation](models/validation.md) by decorating your model object with data annotation validation attributes.</span></span> <span data-ttu-id="38f7b-171">Los atributos de validación se comprueban en el cliente antes de que los valores se registran en el servidor, así como en el servidor antes de la acción de controlador se llama.</span><span class="sxs-lookup"><span data-stu-id="38f7b-171">The validation attributes are checked on the client side before values are posted to the server, as well as on the server before the controller action is called.</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [4, 5, 8, 9]}} -->

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

<span data-ttu-id="38f7b-172">Una acción de controlador:</span><span class="sxs-lookup"><span data-stu-id="38f7b-172">A controller action:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "csharp", "highlight_args": {"hl_lines": [3]}} -->

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null)
{
    if (ModelState.IsValid)
    {
      // work with the model
    }
    // If we got this far, something failed, redisplay form
    return View(model);
}
```

<span data-ttu-id="38f7b-173">El marco de trabajo controlará la validación de datos de la solicitud en el cliente y en el servidor.</span><span class="sxs-lookup"><span data-stu-id="38f7b-173">The framework will handle validating request data both on the client and on the server.</span></span> <span data-ttu-id="38f7b-174">Lógica de validación especificada en tipos de modelo se agrega a las vistas representadas como anotaciones discretas y se aplica en el explorador con [jQuery validación](http://jqueryvalidation.org/).</span><span class="sxs-lookup"><span data-stu-id="38f7b-174">Validation logic specified on model types is added to the rendered views as unobtrusive annotations and is enforced in the browser with [jQuery Validation](http://jqueryvalidation.org/).</span></span>

### <a name="dependency-injection"></a><span data-ttu-id="38f7b-175">Inyección de dependencia</span><span class="sxs-lookup"><span data-stu-id="38f7b-175">Dependency injection</span></span>

<span data-ttu-id="38f7b-176">ASP.NET Core tiene compatibilidad integrada para [inyección de dependencia (DI)](../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="38f7b-176">ASP.NET Core has built-in support for [dependency injection (DI)](../fundamentals/dependency-injection.md).</span></span> <span data-ttu-id="38f7b-177">En ASP.NET MVC de núcleo, [controladores](controllers/dependency-injection.md) puede solicitud necesita servicios a través de sus constructores, lo que les permite seguir el [principio de dependencias explícitas](http://deviq.com/explicit-dependencies-principle/).</span><span class="sxs-lookup"><span data-stu-id="38f7b-177">In ASP.NET Core MVC, [controllers](controllers/dependency-injection.md) can request needed services through their constructors, allowing them to follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span>

<span data-ttu-id="38f7b-178">También puede usar su aplicación [inyección de dependencia en la vista archivos](views/dependency-injection.md), usando la `@inject` directiva:</span><span class="sxs-lookup"><span data-stu-id="38f7b-178">Your app can also use [dependency injection in view files](views/dependency-injection.md), using the `@inject` directive:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [1]}} -->

```html
@inject SomeService ServiceName
<!DOCTYPE html>
<html>
<head>
  <title>@ServiceName.GetTitle</title>
</head>
<body>
  <h1>@ServiceName.GetTitle</h1>
</body>
</html>
```

### <a name="filters"></a><span data-ttu-id="38f7b-179">Filtros</span><span class="sxs-lookup"><span data-stu-id="38f7b-179">Filters</span></span>

<span data-ttu-id="38f7b-180">[Filtros](controllers/filters.md) ayudan a los programadores encapsular preocupaciones transversales, como control de excepciones o autorización.</span><span class="sxs-lookup"><span data-stu-id="38f7b-180">[Filters](controllers/filters.md) help developers encapsulate cross-cutting concerns, like exception handling or authorization.</span></span> <span data-ttu-id="38f7b-181">Filtros habilitar personalizada activo lógica previa y posteriores al procesamiento para los métodos de acción y pueden configurarse para ejecutarse en ciertos puntos dentro de la canalización de ejecución de una solicitud determinada.</span><span class="sxs-lookup"><span data-stu-id="38f7b-181">Filters enable running custom pre- and post-processing logic for action methods, and can be configured to run at certain points within the execution pipeline for a given request.</span></span> <span data-ttu-id="38f7b-182">Los filtros pueden aplicarse a los controladores o acciones como atributos (o pueden realizarse globalmente).</span><span class="sxs-lookup"><span data-stu-id="38f7b-182">Filters can be applied to controllers or actions as attributes (or can be run globally).</span></span> <span data-ttu-id="38f7b-183">Varios filtros (como `Authorize`) se incluyen en el marco de trabajo.</span><span class="sxs-lookup"><span data-stu-id="38f7b-183">Several filters (such as `Authorize`) are included in the framework.</span></span>


```csharp
[Authorize]
   public class AccountController : Controller
   {

```

### <a name="areas"></a><span data-ttu-id="38f7b-184">Áreas</span><span class="sxs-lookup"><span data-stu-id="38f7b-184">Areas</span></span>

<span data-ttu-id="38f7b-185">[Áreas](controllers/areas.md) proporcionan una manera de dividir una aplicación Web de MVC de ASP.NET Core grande en agrupaciones funcionales más pequeñas.</span><span class="sxs-lookup"><span data-stu-id="38f7b-185">[Areas](controllers/areas.md) provide a way to partition a large ASP.NET Core MVC Web app into smaller functional groupings.</span></span> <span data-ttu-id="38f7b-186">Un área de forma eficaz es una estructura MVC dentro de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="38f7b-186">An area is effectively an MVC structure inside an application.</span></span> <span data-ttu-id="38f7b-187">En un proyecto MVC, los componentes lógicos como modelo, el controlador y la vista se guardan en carpetas diferentes y MVC usa las convenciones de nomenclatura para crear la relación entre estos componentes.</span><span class="sxs-lookup"><span data-stu-id="38f7b-187">In an MVC project, logical components like Model, Controller, and View are kept in different folders, and MVC uses naming conventions to create the relationship between these components.</span></span> <span data-ttu-id="38f7b-188">Para una aplicación grande, puede ser conveniente dividir la aplicación en distintas áreas de nivel alto de funcionalidad.</span><span class="sxs-lookup"><span data-stu-id="38f7b-188">For a large app, it may be advantageous to partition the app into separate high level areas of functionality.</span></span> <span data-ttu-id="38f7b-189">Por ejemplo, una aplicación de comercio electrónico con varias unidades de negocio, como la desprotección, facturación y búsqueda etcetera. Cada una de estas unidades tienen sus propias vistas de componente lógico, controladores y modelos.</span><span class="sxs-lookup"><span data-stu-id="38f7b-189">For instance, an e-commerce app with multiple business units, such as checkout, billing, and search etc. Each of these units have their own logical component views, controllers, and models.</span></span>

### <a name="web-apis"></a><span data-ttu-id="38f7b-190">API Web</span><span class="sxs-lookup"><span data-stu-id="38f7b-190">Web APIs</span></span>

<span data-ttu-id="38f7b-191">Además de ser una plataforma excelente para la creación de sitios web, MVC de ASP.NET Core tiene mayor compatibilidad para la creación de las API Web.</span><span class="sxs-lookup"><span data-stu-id="38f7b-191">In addition to being a great platform for building web sites, ASP.NET Core MVC has great support for building Web APIs.</span></span> <span data-ttu-id="38f7b-192">Puede crear servicios que pueden alcanzar una amplia gama de clientes, incluidos los exploradores y dispositivos móviles.</span><span class="sxs-lookup"><span data-stu-id="38f7b-192">You can build services that can reach a broad range of clients including browsers and mobile devices.</span></span>

<span data-ttu-id="38f7b-193">El marco incluye compatibilidad de la negociación de contenido HTTP con compatibilidad integrada para [aplicar formato a datos](models/formatting.md) como JSON o XML.</span><span class="sxs-lookup"><span data-stu-id="38f7b-193">The framework includes support for HTTP content-negotiation with built-in support for [formatting data](models/formatting.md) as JSON or XML.</span></span> <span data-ttu-id="38f7b-194">Escribir [formateadores personalizados](advanced/custom-formatters.md) para agregar compatibilidad con sus propios formatos.</span><span class="sxs-lookup"><span data-stu-id="38f7b-194">Write [custom formatters](advanced/custom-formatters.md) to add support for your own formats.</span></span>

<span data-ttu-id="38f7b-195">Utilice la generación de vínculo para habilitar la compatibilidad para hipermedia.</span><span class="sxs-lookup"><span data-stu-id="38f7b-195">Use link generation to enable support for hypermedia.</span></span> <span data-ttu-id="38f7b-196">Habilitar la compatibilidad con facilidad [recursos entre orígenes (CORS) de uso compartido](http://www.w3.org/TR/cors/) para que las API Web se pueden compartir entre varias aplicaciones Web.</span><span class="sxs-lookup"><span data-stu-id="38f7b-196">Easily enable support for [cross-origin resource sharing (CORS)](http://www.w3.org/TR/cors/) so that your Web APIs can be shared across multiple Web applications.</span></span>

### <a name="testability"></a><span data-ttu-id="38f7b-197">Capacidad de prueba</span><span class="sxs-lookup"><span data-stu-id="38f7b-197">Testability</span></span>

<span data-ttu-id="38f7b-198">Uso del marco de trabajo de inserción de dependencias e interfaces hacer adecuadas a las pruebas unitarias y el marco de trabajo incluye características (por ejemplo, un proveedor TestHost y InMemory para Entity Framework) que hacen [las pruebas de integración](../testing/integration-testing.md) rápido y fácil así.</span><span class="sxs-lookup"><span data-stu-id="38f7b-198">The framework's use of interfaces and dependency injection make it well-suited to unit testing, and the framework includes features (like a TestHost and InMemory provider for Entity Framework) that make [integration testing](../testing/integration-testing.md) quick and easy as well.</span></span> <span data-ttu-id="38f7b-199">Obtenga más información sobre [para probar la lógica de controlador](controllers/testing.md).</span><span class="sxs-lookup"><span data-stu-id="38f7b-199">Learn more about [testing controller logic](controllers/testing.md).</span></span>

### <a name="razor-view-engine"></a><span data-ttu-id="38f7b-200">Motor de vista Razor</span><span class="sxs-lookup"><span data-stu-id="38f7b-200">Razor view engine</span></span>

<span data-ttu-id="38f7b-201">[Vistas ASP.NET MVC Core](views/overview.md) usar la [motor de vista Razor](views/razor.md) para representar vistas.</span><span class="sxs-lookup"><span data-stu-id="38f7b-201">[ASP.NET Core MVC views](views/overview.md) use the [Razor view engine](views/razor.md) to render views.</span></span> <span data-ttu-id="38f7b-202">Razor es un lenguaje de marcado de plantilla compact, expresivo y fluido para definir vistas que utilizan código incrustado de C#.</span><span class="sxs-lookup"><span data-stu-id="38f7b-202">Razor is a compact, expressive and fluid template markup language for defining views using embedded C# code.</span></span> <span data-ttu-id="38f7b-203">Razor se usa para generar dinámicamente el contenido web en el servidor.</span><span class="sxs-lookup"><span data-stu-id="38f7b-203">Razor is used to dynamically generate web content on the server.</span></span> <span data-ttu-id="38f7b-204">Limpiamente puede mezclar código de servidor con el código y contenido del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="38f7b-204">You can cleanly mix server code with client side content and code.</span></span>

```text
<ul>
  @for (int i = 0; i < 5; i++) {
    <li>List item @i</li>
  }
</ul>
```

<span data-ttu-id="38f7b-205">Con el motor de vista Razor, puede definir [diseños](views/layout.md), [vistas parciales](views/partial.md) y secciones reemplazables.</span><span class="sxs-lookup"><span data-stu-id="38f7b-205">Using the Razor view engine you can define [layouts](views/layout.md), [partial views](views/partial.md) and replaceable sections.</span></span>

### <a name="strongly-typed-views"></a><span data-ttu-id="38f7b-206">Vistas fuertemente tipadas</span><span class="sxs-lookup"><span data-stu-id="38f7b-206">Strongly typed views</span></span>

<span data-ttu-id="38f7b-207">Vistas de Razor en MVC pueden estar fuertemente tipadas en función de su modelo.</span><span class="sxs-lookup"><span data-stu-id="38f7b-207">Razor views in MVC can be strongly typed based on your model.</span></span> <span data-ttu-id="38f7b-208">Controladores pueden pasar un modelo fuertemente tipado para habilitar las vistas para que IntelliSense admite y comprobación de tipos de vistas.</span><span class="sxs-lookup"><span data-stu-id="38f7b-208">Controllers can pass a strongly typed model to views enabling your views to have type checking and IntelliSense support.</span></span>

<span data-ttu-id="38f7b-209">Por ejemplo, la vista siguiente define un modelo de tipo `IEnumerable<Product>`:</span><span class="sxs-lookup"><span data-stu-id="38f7b-209">For example, the following view defines a model of type `IEnumerable<Product>`:</span></span>

```html
@model IEnumerable<Product>
<ul>
    @foreach (Product p in Model)
    {
        <li>@p.Name</li>
    }
</ul>
```

### <a name="tag-helpers"></a><span data-ttu-id="38f7b-210">Aplicaciones auxiliares de etiquetas</span><span class="sxs-lookup"><span data-stu-id="38f7b-210">Tag Helpers</span></span>

<span data-ttu-id="38f7b-211">[Aplicaciones auxiliares de etiquetas](views/tag-helpers/intro.md) habilitar el código de servidor participar en la creación y representar elementos HTML en archivos de Razor.</span><span class="sxs-lookup"><span data-stu-id="38f7b-211">[Tag Helpers](views/tag-helpers/intro.md) enable server side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="38f7b-212">Puede usar aplicaciones auxiliares de etiquetas para definir etiquetas personalizadas (por ejemplo, `<environment>`) o para modificar el comportamiento de las etiquetas existentes (por ejemplo, `<label>`).</span><span class="sxs-lookup"><span data-stu-id="38f7b-212">You can use tag helpers to define custom tags (for example, `<environment>`) or to modify the behavior of existing tags (for example, `<label>`).</span></span> <span data-ttu-id="38f7b-213">Aplicaciones auxiliares de etiquetas enlazar a elementos específicos que se basa en el nombre del elemento y sus atributos.</span><span class="sxs-lookup"><span data-stu-id="38f7b-213">Tag Helpers bind to specific elements based on the element name and its attributes.</span></span> <span data-ttu-id="38f7b-214">Proporcionan las ventajas de la representación del lado servidor al tiempo que se mantiene una experiencia de edición HTML.</span><span class="sxs-lookup"><span data-stu-id="38f7b-214">They provide the benefits of server-side rendering while still preserving an HTML editing experience.</span></span>

<span data-ttu-id="38f7b-215">Hay muchas aplicaciones auxiliares de etiquetas integradas para las tareas comunes: como la creación de formularios, vínculos, activos de carga y los paquetes más - y aún más disponibles en repositorios públicos de GitHub y como NuGet.</span><span class="sxs-lookup"><span data-stu-id="38f7b-215">There are many built-in Tag Helpers for common tasks - such as creating forms, links, loading assets and more - and even more available in public GitHub repositories and as NuGet packages.</span></span> <span data-ttu-id="38f7b-216">Aplicaciones auxiliares de etiquetas se crean en C# y se dirigen a los elementos HTML en función de nombre de elemento o atributo o etiqueta primaria.</span><span class="sxs-lookup"><span data-stu-id="38f7b-216">Tag Helpers are authored in C#, and they target HTML elements based on element name, attribute name, or parent tag.</span></span> <span data-ttu-id="38f7b-217">Por ejemplo, la integrada LinkTagHelper puede utilizarse para crear un vínculo a la `Login` acción de la `AccountsController`:</span><span class="sxs-lookup"><span data-stu-id="38f7b-217">For example, the built-in LinkTagHelper can be used to create a link to the `Login` action of the `AccountsController`:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [3]}} -->

```html
<p>
    Thank you for confirming your email.
    Please <a asp-controller="Account" asp-action="Login">Click here to Log in</a>.
</p>
```

<span data-ttu-id="38f7b-218">La `EnvironmentTagHelper` puede usarse para incluir scripts diferentes en las vistas (por ejemplo, sin formato o reducida) según el entorno en tiempo de ejecución, como desarrollo, ensayo o producción:</span><span class="sxs-lookup"><span data-stu-id="38f7b-218">The `EnvironmentTagHelper` can be used to include different scripts in your views (for example, raw or minified) based on the runtime environment, such as Development, Staging, or Production:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [1, 3, 4, 9]}} -->

```html
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

<span data-ttu-id="38f7b-219">Aplicaciones auxiliares de etiquetas proporcionan una experiencia de desarrollo compatible con HTML y un entorno rico de IntelliSense para crear marcado HTML y Razor.</span><span class="sxs-lookup"><span data-stu-id="38f7b-219">Tag Helpers provide an HTML-friendly development experience and a rich IntelliSense environment for creating HTML and Razor markup.</span></span> <span data-ttu-id="38f7b-220">La mayoría de las aplicaciones auxiliares de etiquetas integradas elementos HTML existentes de destino y proporciona atributos de servidor para el elemento.</span><span class="sxs-lookup"><span data-stu-id="38f7b-220">Most of the built-in Tag Helpers target existing HTML elements and provide server-side attributes for the element.</span></span>

### <a name="view-components"></a><span data-ttu-id="38f7b-221">Componentes de la vista</span><span class="sxs-lookup"><span data-stu-id="38f7b-221">View Components</span></span>

<span data-ttu-id="38f7b-222">[Ver componentes](views/view-components.md) permiten empaquetar la lógica de representación y volver a lo largo de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="38f7b-222">[View Components](views/view-components.md) allow you to package rendering logic and reuse it throughout the application.</span></span> <span data-ttu-id="38f7b-223">Son similares a [vistas parciales](views/partial.md), pero con lógica asociada.</span><span class="sxs-lookup"><span data-stu-id="38f7b-223">They're similar to [partial views](views/partial.md), but with associated logic.</span></span>
