---
title: Enrutar a acciones de controlador de ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo ASP.NET Core MVC usa el middleware de enrutamiento para encontrar direcciones URL de las solicitudes entrantes y asignarlas a acciones.
ms.author: riande
ms.date: 01/24/2019
uid: mvc/controllers/routing
ms.openlocfilehash: a0dbfbe60c151990581b494f81e500fe0b315f55
ms.sourcegitcommit: a166291c6708f5949c417874108332856b53b6a9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2019
ms.locfileid: "72589851"
---
# <a name="routing-to-controller-actions-in-aspnet-core"></a><span data-ttu-id="a1735-103">Enrutar a acciones de controlador de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a1735-103">Routing to controller actions in ASP.NET Core</span></span>

<span data-ttu-id="a1735-104">Por [Ryan Nowak](https://github.com/rynowak) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a1735-104">By [Ryan Nowak](https://github.com/rynowak) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a1735-105">ASP.NET Core MVC utiliza el [middleware](xref:fundamentals/middleware/index) de enrutamiento para buscar las direcciones URL de las solicitudes entrantes y asignarlas a acciones.</span><span class="sxs-lookup"><span data-stu-id="a1735-105">ASP.NET Core MVC uses the Routing [middleware](xref:fundamentals/middleware/index) to match the URLs of incoming requests and map them to actions.</span></span> <span data-ttu-id="a1735-106">Las rutas se definen en el código de inicio o los atributos.</span><span class="sxs-lookup"><span data-stu-id="a1735-106">Routes are defined in startup code or attributes.</span></span> <span data-ttu-id="a1735-107">Las rutas describen cómo se deben asociar las rutas de dirección URL a las acciones.</span><span class="sxs-lookup"><span data-stu-id="a1735-107">Routes describe how URL paths should be matched to actions.</span></span> <span data-ttu-id="a1735-108">Las rutas también se usan para generar direcciones URL (para vínculos) enviadas en las respuestas.</span><span class="sxs-lookup"><span data-stu-id="a1735-108">Routes are also used to generate URLs (for links) sent out in responses.</span></span>

<span data-ttu-id="a1735-109">Las acciones se enrutan bien mediante convención o bien mediante atributos.</span><span class="sxs-lookup"><span data-stu-id="a1735-109">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="a1735-110">Colocar una ruta en el controlador o la acción hace que se enrute mediante atributos.</span><span class="sxs-lookup"><span data-stu-id="a1735-110">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="a1735-111">Consulte [Enrutamiento mixto](#routing-mixed-ref-label) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="a1735-111">See [Mixed routing](#routing-mixed-ref-label) for more information.</span></span>

<span data-ttu-id="a1735-112">Este documento explica las interacciones entre MVC y el enrutamiento, así como el uso que las aplicaciones MVC suelen hacer de las características de enrutamiento.</span><span class="sxs-lookup"><span data-stu-id="a1735-112">This document will explain the interactions between MVC and routing, and how typical MVC apps make use of routing features.</span></span> <span data-ttu-id="a1735-113">Consulte [Enrutamiento](xref:fundamentals/routing) para obtener más información sobre enrutamiento avanzado.</span><span class="sxs-lookup"><span data-stu-id="a1735-113">See [Routing](xref:fundamentals/routing) for details on advanced routing.</span></span>

## <a name="setting-up-routing-middleware"></a><span data-ttu-id="a1735-114">Configurar el middleware de enrutamiento</span><span class="sxs-lookup"><span data-stu-id="a1735-114">Setting up Routing Middleware</span></span>

<span data-ttu-id="a1735-115">En su método *Configure* puede ver código similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="a1735-115">In your *Configure* method you may see code similar to:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="a1735-116">Dentro de la llamada a `UseMvc`, `MapRoute` se utiliza para crear una ruta única, a la que nos referiremos como la ruta `default`.</span><span class="sxs-lookup"><span data-stu-id="a1735-116">Inside the call to `UseMvc`, `MapRoute` is used to create a single route, which we'll refer to as the `default` route.</span></span> <span data-ttu-id="a1735-117">La mayoría de las aplicaciones MVC usarán una ruta con una plantilla similar a la ruta `default`.</span><span class="sxs-lookup"><span data-stu-id="a1735-117">Most MVC apps will use a route with a template similar to the `default` route.</span></span>

<span data-ttu-id="a1735-118">La plantilla de ruta `"{controller=Home}/{action=Index}/{id?}"` puede coincidir con una ruta de dirección URL como `/Products/Details/5` y extraerá los valores de ruta `{ controller = Products, action = Details, id = 5 }` mediante la conversión de la ruta en tokens.</span><span class="sxs-lookup"><span data-stu-id="a1735-118">The route template `"{controller=Home}/{action=Index}/{id?}"` can match a URL path like `/Products/Details/5` and will extract the route values `{ controller = Products, action = Details, id = 5 }` by tokenizing the path.</span></span> <span data-ttu-id="a1735-119">MVC intentará encontrar un controlador denominado `ProductsController` y ejecutar la acción `Details`:</span><span class="sxs-lookup"><span data-stu-id="a1735-119">MVC will attempt to locate a controller named `ProductsController` and run the action `Details`:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Details(int id) { ... }
}
```

<span data-ttu-id="a1735-120">Tenga en cuenta que, en este ejemplo, el enlace de modelos usaría el valor de `id = 5` para establecer el parámetro `id` en `5` al invocar esta acción.</span><span class="sxs-lookup"><span data-stu-id="a1735-120">Note that in this example, model binding would use the value of `id = 5` to set the `id` parameter to `5` when invoking this action.</span></span> <span data-ttu-id="a1735-121">Consulte [Enlace de modelos](../models/model-binding.md) para obtener más detalles.</span><span class="sxs-lookup"><span data-stu-id="a1735-121">See the [Model Binding](../models/model-binding.md) for more details.</span></span>

<span data-ttu-id="a1735-122">Utilizando la ruta `default`:</span><span class="sxs-lookup"><span data-stu-id="a1735-122">Using the `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="a1735-123">La plantilla de ruta:</span><span class="sxs-lookup"><span data-stu-id="a1735-123">The route template:</span></span>

* <span data-ttu-id="a1735-124">`{controller=Home}` define `Home` como el valor `controller` predeterminado</span><span class="sxs-lookup"><span data-stu-id="a1735-124">`{controller=Home}` defines `Home` as the default `controller`</span></span>

* <span data-ttu-id="a1735-125">`{action=Index}` define `Index` como el valor `action` predeterminado</span><span class="sxs-lookup"><span data-stu-id="a1735-125">`{action=Index}` defines `Index` as the default `action`</span></span>

* <span data-ttu-id="a1735-126">`{id?}` define `id` como opcional</span><span class="sxs-lookup"><span data-stu-id="a1735-126">`{id?}` defines `id` as optional</span></span>

<span data-ttu-id="a1735-127">No es necesario que los parámetros de ruta opcionales y predeterminados estén presentes en la ruta de dirección URL para una coincidencia.</span><span class="sxs-lookup"><span data-stu-id="a1735-127">Default and optional route parameters don't need to be present in the URL path for a match.</span></span> <span data-ttu-id="a1735-128">Consulte [Referencia de plantilla de ruta](../../fundamentals/routing.md#route-template-reference) para obtener una descripción detallada de la sintaxis de la plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="a1735-128">See [Route Template Reference](../../fundamentals/routing.md#route-template-reference) for a detailed description of route template syntax.</span></span>

<span data-ttu-id="a1735-129">`"{controller=Home}/{action=Index}/{id?}"` puede coincidir con la ruta de dirección URL `/` y generará los valores de ruta `{ controller = Home, action = Index }`.</span><span class="sxs-lookup"><span data-stu-id="a1735-129">`"{controller=Home}/{action=Index}/{id?}"` can match the URL path `/` and will produce the route values `{ controller = Home, action = Index }`.</span></span> <span data-ttu-id="a1735-130">Los valores de `controller` y `action` utilizan los valores predeterminados, `id` no genera un valor porque no hay ningún segmento correspondiente en la ruta de dirección URL.</span><span class="sxs-lookup"><span data-stu-id="a1735-130">The values for `controller` and `action` make use of the default values, `id` doesn't produce a value since there's no corresponding segment in the URL path.</span></span> <span data-ttu-id="a1735-131">MVC utilizaría estos valores de ruta para seleccionar `HomeController` y la acción `Index`:</span><span class="sxs-lookup"><span data-stu-id="a1735-131">MVC would use these route values to select the `HomeController` and `Index` action:</span></span>

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

<span data-ttu-id="a1735-132">Usando esta definición de controlador y la plantilla de ruta, la acción `HomeController.Index` se ejecutaría para cualquiera de las rutas de dirección URL siguientes:</span><span class="sxs-lookup"><span data-stu-id="a1735-132">Using this controller definition and route template, the `HomeController.Index` action would be executed for any of the following URL paths:</span></span>

* `/Home/Index/17`

* `/Home/Index`

* `/Home`

* `/`

<span data-ttu-id="a1735-133">El método de conveniencia `UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="a1735-133">The convenience method `UseMvcWithDefaultRoute`:</span></span>

```csharp
app.UseMvcWithDefaultRoute();
```

<span data-ttu-id="a1735-134">Se puede usar para reemplazar:</span><span class="sxs-lookup"><span data-stu-id="a1735-134">Can be used to replace:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="a1735-135">`UseMvc` y `UseMvcWithDefaultRoute` agregan una instancia de `RouterMiddleware` a la canalización de middleware.</span><span class="sxs-lookup"><span data-stu-id="a1735-135">`UseMvc` and `UseMvcWithDefaultRoute` add an instance of `RouterMiddleware` to the middleware pipeline.</span></span> <span data-ttu-id="a1735-136">MVC no interactúa directamente con middleware y usa el enrutamiento para controlar las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="a1735-136">MVC doesn't interact directly with middleware, and uses routing to handle requests.</span></span> <span data-ttu-id="a1735-137">MVC está conectado a las rutas a través de una instancia de `MvcRouteHandler`.</span><span class="sxs-lookup"><span data-stu-id="a1735-137">MVC is connected to the routes through an instance of `MvcRouteHandler`.</span></span> <span data-ttu-id="a1735-138">El código dentro de `UseMvc` es similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="a1735-138">The code inside of `UseMvc` is similar to the following:</span></span>

```csharp
var routes = new RouteBuilder(app);

// Add connection to MVC, will be hooked up by calls to MapRoute.
routes.DefaultHandler = new MvcRouteHandler(...);

// Execute callback to register routes.
// routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");

// Create route collection and add the middleware.
app.UseRouter(routes.Build());
```

<span data-ttu-id="a1735-139">`UseMvc` no define directamente ninguna ruta, sino que agrega un marcador de posición a la colección de rutas para la ruta `attribute`.</span><span class="sxs-lookup"><span data-stu-id="a1735-139">`UseMvc` doesn't directly define any routes, it adds a placeholder to the route collection for the `attribute` route.</span></span> <span data-ttu-id="a1735-140">La sobrecarga `UseMvc(Action<IRouteBuilder>)` le permite agregar sus propias rutas y también admite el enrutamiento mediante atributos.</span><span class="sxs-lookup"><span data-stu-id="a1735-140">The overload `UseMvc(Action<IRouteBuilder>)` lets you add your own routes and also supports attribute routing.</span></span>  <span data-ttu-id="a1735-141">`UseMvc` y todas sus variaciones agregan un marcador de posición para la ruta de atributo (el enrutamiento mediante atributos siempre está disponible, independientemente de cómo configure `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="a1735-141">`UseMvc` and all of its variations adds a placeholder for the attribute route - attribute routing is always available regardless of how you configure `UseMvc`.</span></span> <span data-ttu-id="a1735-142">`UseMvcWithDefaultRoute` define una ruta predeterminada y admite el enrutamiento mediante atributos.</span><span class="sxs-lookup"><span data-stu-id="a1735-142">`UseMvcWithDefaultRoute` defines a default route and supports attribute routing.</span></span> <span data-ttu-id="a1735-143">La sección [Enrutamiento mediante atributos](#attribute-routing-ref-label) incluye más detalles sobre este tipo de enrutamiento.</span><span class="sxs-lookup"><span data-stu-id="a1735-143">The [Attribute Routing](#attribute-routing-ref-label) section includes more details on attribute routing.</span></span>

<a name="routing-conventional-ref-label"></a>

## <a name="conventional-routing"></a><span data-ttu-id="a1735-144">Enrutamiento convencional</span><span class="sxs-lookup"><span data-stu-id="a1735-144">Conventional routing</span></span>

<span data-ttu-id="a1735-145">La ruta `default`:</span><span class="sxs-lookup"><span data-stu-id="a1735-145">The `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="a1735-146">es un ejemplo de un *enrutamiento convencional*.</span><span class="sxs-lookup"><span data-stu-id="a1735-146">is an example of a *conventional routing*.</span></span> <span data-ttu-id="a1735-147">Llamamos a este estilo *enrutamiento convencional* porque establece una *convención* para las rutas de dirección URL:</span><span class="sxs-lookup"><span data-stu-id="a1735-147">We call this style *conventional routing* because it establishes a *convention* for URL paths:</span></span>

* <span data-ttu-id="a1735-148">El primer segmento de la ruta asigna el nombre de controlador.</span><span class="sxs-lookup"><span data-stu-id="a1735-148">the first path segment maps to the controller name</span></span>

* <span data-ttu-id="a1735-149">El segundo asigna el nombre de la acción.</span><span class="sxs-lookup"><span data-stu-id="a1735-149">the second maps to the action name.</span></span>

* <span data-ttu-id="a1735-150">El tercer segmento se utiliza para un identificador `id` opcional usado para asignar un modelo de entidad</span><span class="sxs-lookup"><span data-stu-id="a1735-150">the third segment is used for an optional `id` used to map to a model entity</span></span>

<span data-ttu-id="a1735-151">Mediante esta ruta `default`, la ruta de dirección URL `/Products/List` se asigna a la acción `ProductsController.List`, y `/Blog/Article/17` se asigna a `BlogController.Article`.</span><span class="sxs-lookup"><span data-stu-id="a1735-151">Using this `default` route, the URL path `/Products/List` maps to the `ProductsController.List` action, and `/Blog/Article/17` maps to `BlogController.Article`.</span></span> <span data-ttu-id="a1735-152">Esta asignación **solo** se basa en los nombres de acción y controlador, y no en espacios de nombres, ubicaciones de archivos de origen ni parámetros de método.</span><span class="sxs-lookup"><span data-stu-id="a1735-152">This mapping is based on the controller and action names **only** and isn't based on namespaces, source file locations, or method parameters.</span></span>

> [!TIP]
> <span data-ttu-id="a1735-153">Usar el enrutamiento convencional con la ruta predeterminada permite generar rápidamente la aplicación sin necesidad de elaborar un nuevo patrón de dirección URL para cada acción que se define.</span><span class="sxs-lookup"><span data-stu-id="a1735-153">Using conventional routing with the default route allows you to build the application quickly without having to come up with a new URL pattern for each action you define.</span></span> <span data-ttu-id="a1735-154">Para una aplicación con acciones de estilo CRUD, la coherencia en las direcciones URL en todos los controladores puede ayudar a simplificar el código y hacer que la interfaz de usuario sea más predecible.</span><span class="sxs-lookup"><span data-stu-id="a1735-154">For an application with CRUD style actions, having consistency for the URLs across your controllers can help simplify your code and make your UI more predictable.</span></span>

> [!WARNING]
> <span data-ttu-id="a1735-155">`id` se define como opcional mediante la plantilla de ruta, lo que significa que las acciones se pueden ejecutar sin el identificador proporcionado como parte de la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="a1735-155">The `id` is defined as optional by the route template, meaning that your actions can execute without the ID provided as part of the URL.</span></span> <span data-ttu-id="a1735-156">Normalmente lo que ocurrirá si `id` se omite de la dirección URL es que el enlace de modelos establecerá en `0` su valor y, como resultado, no se encontrará ninguna entidad en la base de datos que coincida con `id == 0`.</span><span class="sxs-lookup"><span data-stu-id="a1735-156">Usually what will happen if `id` is omitted from the URL is that it will be set to `0` by model binding, and as a result no entity will be found in the database matching `id == 0`.</span></span> <span data-ttu-id="a1735-157">El enrutamiento mediante atributos proporciona un mayor control que permite requerir el identificador para algunas acciones y para otras no.</span><span class="sxs-lookup"><span data-stu-id="a1735-157">Attribute routing can give you fine-grained control to make the ID required for some actions and not for others.</span></span> <span data-ttu-id="a1735-158">Por convención, la documentación incluirá parámetros opcionales como `id` cuando sea más probable que su uso sea correcto.</span><span class="sxs-lookup"><span data-stu-id="a1735-158">By convention the documentation will include optional parameters like `id` when they're likely to appear in correct usage.</span></span>

## <a name="multiple-routes"></a><span data-ttu-id="a1735-159">Varias rutas</span><span class="sxs-lookup"><span data-stu-id="a1735-159">Multiple routes</span></span>

<span data-ttu-id="a1735-160">Para agregar varias rutas dentro de `UseMvc`, agregue más llamadas a `MapRoute`.</span><span class="sxs-lookup"><span data-stu-id="a1735-160">You can add multiple routes inside `UseMvc` by adding more calls to `MapRoute`.</span></span> <span data-ttu-id="a1735-161">Hacerlo le permite definir varias convenciones o agregar rutas convencionales que se dedican a una acción específica, como en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="a1735-161">Doing so allows you to define multiple conventions, or to add conventional routes that are dedicated to a specific action, such as:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
            defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="a1735-162">Aquí, la ruta `blog` es una *ruta convencional dedicada*, lo que significa que utiliza el sistema de enrutamiento convencional, pero que está dedicada a una acción específica.</span><span class="sxs-lookup"><span data-stu-id="a1735-162">The `blog` route here is a *dedicated conventional route*, meaning that it uses the conventional routing system, but is dedicated to a specific action.</span></span> <span data-ttu-id="a1735-163">Puesto que `controller` y `action` no aparecen en la plantilla de ruta como parámetros, solo pueden tener los valores predeterminados y, por tanto, esta ruta siempre se asignará a la acción `BlogController.Article`.</span><span class="sxs-lookup"><span data-stu-id="a1735-163">Since `controller` and `action` don't appear in the route template as parameters, they can only have the default values, and thus this route will always map to the action `BlogController.Article`.</span></span>

<span data-ttu-id="a1735-164">Las rutas de la colección de rutas están ordenadas y se procesan en el orden en que se hayan agregado.</span><span class="sxs-lookup"><span data-stu-id="a1735-164">Routes in the route collection are ordered, and will be processed in the order they're added.</span></span> <span data-ttu-id="a1735-165">Por tanto, la ruta `blog` de este ejemplo se intentará antes que la ruta `default`.</span><span class="sxs-lookup"><span data-stu-id="a1735-165">So in this example, the `blog` route will be tried before the `default` route.</span></span>

> [!NOTE]
> <span data-ttu-id="a1735-166">Las *rutas convencionales dedicadas* suelen usar parámetros de ruta comodín como `{*article}` para capturar la parte restante de la ruta de dirección URL.</span><span class="sxs-lookup"><span data-stu-id="a1735-166">*Dedicated conventional routes* often use catch-all route parameters like `{*article}` to capture the remaining portion of the URL path.</span></span> <span data-ttu-id="a1735-167">Esto puede hacer que la ruta sea "demasiado expansiva", lo que significa que coincidirá con direcciones URL que se pretendía que coincidieran con otras rutas.</span><span class="sxs-lookup"><span data-stu-id="a1735-167">This can make a route 'too greedy' meaning that it matches URLs that you intended to be matched by other routes.</span></span> <span data-ttu-id="a1735-168">Coloque las rutas "expansivas" más adelante en la tabla de rutas para resolver este problema.</span><span class="sxs-lookup"><span data-stu-id="a1735-168">Put the 'greedy' routes later in the route table to solve this.</span></span>

### <a name="fallback"></a><span data-ttu-id="a1735-169">Reserva</span><span class="sxs-lookup"><span data-stu-id="a1735-169">Fallback</span></span>

<span data-ttu-id="a1735-170">Como parte del procesamiento de la solicitud, MVC comprobará que los valores de ruta se pueden usar para buscar un controlador y la acción en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a1735-170">As part of request processing, MVC will verify that the route values can be used to find a controller and action in your application.</span></span> <span data-ttu-id="a1735-171">Si los valores de ruta no coinciden con una acción, entonces la ruta no se considera una coincidencia y se intentará la ruta siguiente.</span><span class="sxs-lookup"><span data-stu-id="a1735-171">If the route values don't match an action then the route isn't considered a match, and the next route will be tried.</span></span> <span data-ttu-id="a1735-172">Esto se denomina *reserva*, y se ha diseñado para simplificar los casos donde se superponen rutas convencionales.</span><span class="sxs-lookup"><span data-stu-id="a1735-172">This is called *fallback*, and it's intended to simplify cases where conventional routes overlap.</span></span>

### <a name="disambiguating-actions"></a><span data-ttu-id="a1735-173">Eliminar la ambigüedad de acciones</span><span class="sxs-lookup"><span data-stu-id="a1735-173">Disambiguating actions</span></span>

<span data-ttu-id="a1735-174">Cuando dos acciones coinciden en el enrutamiento, MVC debe eliminar la ambigüedad para elegir el candidato "recomendado", o de lo contrario se produce una excepción.</span><span class="sxs-lookup"><span data-stu-id="a1735-174">When two actions match through routing, MVC must disambiguate to choose the 'best' candidate or else throw an exception.</span></span> <span data-ttu-id="a1735-175">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="a1735-175">For example:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Edit(int id) { ... }

   [HttpPost]
   public IActionResult Edit(int id, Product product) { ... }
}
```

<span data-ttu-id="a1735-176">Este controlador define dos acciones que coincidirían con la ruta de dirección URL `/Products/Edit/17` y los datos de ruta `{ controller = Products, action = Edit, id = 17 }`.</span><span class="sxs-lookup"><span data-stu-id="a1735-176">This controller defines two actions that would match the URL path `/Products/Edit/17` and route data `{ controller = Products, action = Edit, id = 17 }`.</span></span> <span data-ttu-id="a1735-177">Se trata de un patrón típico para controladores MVC donde `Edit(int)` muestra un formulario para editar un producto, y `Edit(int, Product)` procesa el formulario expuesto.</span><span class="sxs-lookup"><span data-stu-id="a1735-177">This is a typical pattern for MVC controllers where `Edit(int)` shows a form to edit a product, and `Edit(int, Product)` processes  the posted form.</span></span> <span data-ttu-id="a1735-178">Para hacer esto posible, MVC necesita seleccionar `Edit(int, Product)` cuando la solicitud es HTTP `POST` y `Edit(int)` cuando el verbo HTTP es cualquier otra cosa.</span><span class="sxs-lookup"><span data-stu-id="a1735-178">To make this possible MVC would need to choose `Edit(int, Product)` when the request is an HTTP `POST` and `Edit(int)` when the HTTP verb is anything else.</span></span>

<span data-ttu-id="a1735-179">`HttpPostAttribute` ( `[HttpPost]` ) es una implementación de `IActionConstraint` que solo permitirá que la acción se seleccione cuando el verbo HTTP sea `POST`.</span><span class="sxs-lookup"><span data-stu-id="a1735-179">The `HttpPostAttribute` ( `[HttpPost]` ) is an implementation of `IActionConstraint` that will only allow the action to be selected when the HTTP verb is `POST`.</span></span> <span data-ttu-id="a1735-180">La presencia de `IActionConstraint` hace que `Edit(int, Product)` sea una mejor coincidencia que `Edit(int)`, por lo que `Edit(int, Product)` se intentará en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="a1735-180">The presence of an `IActionConstraint` makes the `Edit(int, Product)` a 'better' match than `Edit(int)`, so `Edit(int, Product)` will be tried first.</span></span>

<span data-ttu-id="a1735-181">Solo necesitará escribir implementaciones de `IActionConstraint` personalizadas en escenarios especializados, pero es importante comprender el rol de atributos como `HttpPostAttribute` (para otros verbos HTTP se definen atributos similares).</span><span class="sxs-lookup"><span data-stu-id="a1735-181">You will only need to write custom `IActionConstraint` implementations in specialized scenarios, but it's important to understand the role of attributes like `HttpPostAttribute`  - similar attributes are defined for other HTTP verbs.</span></span> <span data-ttu-id="a1735-182">En el enrutamiento convencional es común que las acciones utilicen el mismo nombre de acción cuando son parte de un flujo de trabajo `show form -> submit form`.</span><span class="sxs-lookup"><span data-stu-id="a1735-182">In conventional routing it's common for actions to use the same action name when they're part of a `show form -> submit form` workflow.</span></span> <span data-ttu-id="a1735-183">La comodidad de este patrón será más evidente después de revisar la sección [Descripción de IActionConstraint](#understanding-iactionconstraint).</span><span class="sxs-lookup"><span data-stu-id="a1735-183">The convenience of this pattern will become more apparent after reviewing the [Understanding IActionConstraint](#understanding-iactionconstraint) section.</span></span>

<span data-ttu-id="a1735-184">Si coinciden varias rutas y MVC no encuentra una ruta "recomendada", se produce una excepción `AmbiguousActionException`.</span><span class="sxs-lookup"><span data-stu-id="a1735-184">If multiple routes match, and MVC can't find a 'best' route, it will throw an `AmbiguousActionException`.</span></span>

<a name="routing-route-name-ref-label"></a>

### <a name="route-names"></a><span data-ttu-id="a1735-185">Nombres de ruta</span><span class="sxs-lookup"><span data-stu-id="a1735-185">Route names</span></span>

<span data-ttu-id="a1735-186">Las cadenas `"blog"` y `"default"` de los ejemplos siguientes son nombres de ruta:</span><span class="sxs-lookup"><span data-stu-id="a1735-186">The strings  `"blog"` and `"default"` in the following examples are route names:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
               defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="a1735-187">Los nombres de ruta proporcionan un nombre lógico a la ruta para que la ruta con nombre pueda utilizarse en la generación de direcciones URL.</span><span class="sxs-lookup"><span data-stu-id="a1735-187">The route names give the route a logical name so that the named route can be used for URL generation.</span></span> <span data-ttu-id="a1735-188">Esto simplifica en gran medida la creación de direcciones URL en los casos en que el orden de las rutas podría complicar la generación de direcciones URL.</span><span class="sxs-lookup"><span data-stu-id="a1735-188">This greatly simplifies URL creation when the ordering of routes could make URL generation complicated.</span></span> <span data-ttu-id="a1735-189">Los nombres de ruta deben ser únicos en toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a1735-189">Route names must be unique application-wide.</span></span>

<span data-ttu-id="a1735-190">Los nombres de ruta no tienen ningún impacto en la coincidencia de direcciones URL ni en el control de las solicitudes; se utilizan únicamente para la generación de direcciones URL.</span><span class="sxs-lookup"><span data-stu-id="a1735-190">Route names have no impact on URL matching or handling of requests; they're used only for URL generation.</span></span> <span data-ttu-id="a1735-191">En [Enrutamiento](xref:fundamentals/routing) se incluye información más detallada sobre la generación de direcciones URL, como la generación de direcciones URL en asistentes específicos de MVC.</span><span class="sxs-lookup"><span data-stu-id="a1735-191">[Routing](xref:fundamentals/routing) has more detailed information on URL generation including URL generation in MVC-specific helpers.</span></span>

<a name="attribute-routing-ref-label"></a>

## <a name="attribute-routing"></a><span data-ttu-id="a1735-192">Enrutamiento mediante atributos</span><span class="sxs-lookup"><span data-stu-id="a1735-192">Attribute routing</span></span>

<span data-ttu-id="a1735-193">El enrutamiento mediante atributos utiliza un conjunto de atributos para asignar acciones directamente a las plantillas de ruta.</span><span class="sxs-lookup"><span data-stu-id="a1735-193">Attribute routing uses a set of attributes to map actions directly to route templates.</span></span> <span data-ttu-id="a1735-194">En el ejemplo siguiente, `app.UseMvc();` se utiliza en el método `Configure` y no se pasa ninguna ruta.</span><span class="sxs-lookup"><span data-stu-id="a1735-194">In the following example, `app.UseMvc();` is used in the `Configure` method and no route is passed.</span></span> <span data-ttu-id="a1735-195">`HomeController` coincidirá con un conjunto de direcciones URL similares a las que coincidirían con la ruta predeterminada `{controller=Home}/{action=Index}/{id?}`:</span><span class="sxs-lookup"><span data-stu-id="a1735-195">The `HomeController` will match a set of URLs similar to what the default route `{controller=Home}/{action=Index}/{id?}` would match:</span></span>

```csharp
public class HomeController : Controller
{
   [Route("")]
   [Route("Home")]
   [Route("Home/Index")]
   public IActionResult Index()
   {
      return View();
   }
   [Route("Home/About")]
   public IActionResult About()
   {
      return View();
   }
   [Route("Home/Contact")]
   public IActionResult Contact()
   {
      return View();
   }
}
```

<span data-ttu-id="a1735-196">La acción `HomeController.Index()` se ejecutará para cualquiera de las rutas de dirección URL `/`, `/Home` o `/Home/Index`.</span><span class="sxs-lookup"><span data-stu-id="a1735-196">The `HomeController.Index()` action will be executed for any of the URL paths `/`, `/Home`, or `/Home/Index`.</span></span>

> [!NOTE]
> <span data-ttu-id="a1735-197">Este ejemplo resalta una diferencia clave de programación entre el enrutamiento mediante atributos y el enrutamiento convencional.</span><span class="sxs-lookup"><span data-stu-id="a1735-197">This example highlights a key programming difference between attribute routing and conventional routing.</span></span> <span data-ttu-id="a1735-198">El enrutamiento mediante atributos requiere más entradas para especificar una ruta; la ruta predeterminada convencional controla las rutas de manera más sucinta.</span><span class="sxs-lookup"><span data-stu-id="a1735-198">Attribute routing requires more input to specify a route; the conventional default route handles routes more succinctly.</span></span> <span data-ttu-id="a1735-199">Pero el enrutamiento mediante atributos permite (y requiere) un control preciso de las plantillas de ruta que se aplicarán a cada acción.</span><span class="sxs-lookup"><span data-stu-id="a1735-199">However, attribute routing allows (and requires) precise control of which route templates apply to each action.</span></span>

<span data-ttu-id="a1735-200">Con el enrutamiento mediante atributos, el nombre del controlador y los nombres de acción **no** juegan ningún papel en la selección de las acciones.</span><span class="sxs-lookup"><span data-stu-id="a1735-200">With attribute routing the controller name and action names play **no** role in which action is selected.</span></span> <span data-ttu-id="a1735-201">Este ejemplo coincidirá con las mismas direcciones URL que el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="a1735-201">This example will match the same URLs as the previous example.</span></span>

```csharp
public class MyDemoController : Controller
{
   [Route("")]
   [Route("Home")]
   [Route("Home/Index")]
   public IActionResult MyIndex()
   {
      return View("Index");
   }
   [Route("Home/About")]
   public IActionResult MyAbout()
   {
      return View("About");
   }
   [Route("Home/Contact")]
   public IActionResult MyContact()
   {
      return View("Contact");
   }
}
```

> [!NOTE]
> <span data-ttu-id="a1735-202">Las plantillas de ruta anteriores no definen parámetros de ruta para `action`, `area` y `controller`.</span><span class="sxs-lookup"><span data-stu-id="a1735-202">The route templates above don't define route parameters for `action`, `area`, and `controller`.</span></span> <span data-ttu-id="a1735-203">De hecho, estos parámetros de ruta no se permiten en rutas de atributo.</span><span class="sxs-lookup"><span data-stu-id="a1735-203">In fact, these route parameters are not allowed in attribute routes.</span></span> <span data-ttu-id="a1735-204">Puesto que la plantilla de ruta ya está asociada a una acción, no tendría sentido analizar el nombre de acción de la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="a1735-204">Since the route template is already associated with an action, it wouldn't make sense to parse the action name from the URL.</span></span>

## <a name="attribute-routing-with-httpverb-attributes"></a><span data-ttu-id="a1735-205">Enrutamiento mediante atributos con atributos Http[Verb]</span><span class="sxs-lookup"><span data-stu-id="a1735-205">Attribute routing with Http[Verb] attributes</span></span>

<span data-ttu-id="a1735-206">El enrutamiento mediante atributos también puede hacer uso de los atributos `Http[Verb]` como `HttpPostAttribute`.</span><span class="sxs-lookup"><span data-stu-id="a1735-206">Attribute routing can also make use of the `Http[Verb]` attributes such as `HttpPostAttribute`.</span></span> <span data-ttu-id="a1735-207">Todos estos atributos pueden aceptar una plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="a1735-207">All of these attributes can accept a route template.</span></span> <span data-ttu-id="a1735-208">Este ejemplo muestra dos acciones que coinciden con la misma plantilla de ruta:</span><span class="sxs-lookup"><span data-stu-id="a1735-208">This example shows two actions that match the same route template:</span></span>

```csharp
[HttpGet("/products")]
public IActionResult ListProducts()
{
   // ...
}

[HttpPost("/products")]
public IActionResult CreateProduct(...)
{
   // ...
}
```

<span data-ttu-id="a1735-209">Para una ruta de dirección URL como `/products`, la acción `ProductsApi.ListProducts` se ejecutará cuando el verbo HTTP sea `GET` y `ProductsApi.CreateProduct` se ejecutará cuando el verbo HTTP sea `POST`.</span><span class="sxs-lookup"><span data-stu-id="a1735-209">For a URL path like `/products` the `ProductsApi.ListProducts` action will be executed when the HTTP verb is `GET` and `ProductsApi.CreateProduct` will be executed when the HTTP verb is `POST`.</span></span> <span data-ttu-id="a1735-210">El enrutamiento mediante atributos primero busca una coincidencia de la dirección URL en el conjunto de plantillas de ruta que se definen mediante atributos de ruta.</span><span class="sxs-lookup"><span data-stu-id="a1735-210">Attribute routing first matches the URL against the set of route templates defined by route attributes.</span></span> <span data-ttu-id="a1735-211">Una vez que se encuentra una plantilla de ruta que coincide, las restricciones `IActionConstraint` se aplican para determinar qué acciones se pueden ejecutar.</span><span class="sxs-lookup"><span data-stu-id="a1735-211">Once a route template matches, `IActionConstraint` constraints are applied to determine which actions can be executed.</span></span>

> [!TIP]
> <span data-ttu-id="a1735-212">Cuando se compila una API de REST, es poco frecuente que se quiera usar `[Route(...)]` en un método de acción, ya que la acción aceptará todos los métodos HTTP.</span><span class="sxs-lookup"><span data-stu-id="a1735-212">When building a REST API, it's rare that you will want to use `[Route(...)]` on an action method as the action will accept all HTTP methods.</span></span> <span data-ttu-id="a1735-213">Es mejor usar `Http*Verb*Attributes` más específicos para precisar lo que es compatible con la API.</span><span class="sxs-lookup"><span data-stu-id="a1735-213">It's better to use the more specific `Http*Verb*Attributes` to be precise about what your API supports.</span></span> <span data-ttu-id="a1735-214">Se espera que los clientes de API de REST sepan qué rutas y verbos HTTP se asignan a determinadas operaciones lógicas.</span><span class="sxs-lookup"><span data-stu-id="a1735-214">Clients of REST APIs are expected to know what paths and HTTP verbs map to specific logical operations.</span></span>

<span data-ttu-id="a1735-215">Puesto que una ruta de atributo se aplica a una acción específica, es fácil crear parámetros necesarios como parte de la definición de plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="a1735-215">Since an attribute route applies to a specific action, it's easy to make parameters required as part of the route template definition.</span></span> <span data-ttu-id="a1735-216">En este ejemplo, se requiere `id` como parte de la ruta de dirección URL.</span><span class="sxs-lookup"><span data-stu-id="a1735-216">In this example, `id` is required as part of the URL path.</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="a1735-217">La acción `ProductsApi.GetProduct(int)` se ejecutará para una ruta de dirección URL como `/products/3`, pero no para una ruta de dirección URL como `/products`.</span><span class="sxs-lookup"><span data-stu-id="a1735-217">The `ProductsApi.GetProduct(int)` action will be executed for a URL path like `/products/3` but not for a URL path like `/products`.</span></span> <span data-ttu-id="a1735-218">Consulte [Enrutamiento](../../fundamentals/routing.md) para obtener una descripción completa de las plantillas de ruta y las opciones relacionadas.</span><span class="sxs-lookup"><span data-stu-id="a1735-218">See [Routing](../../fundamentals/routing.md) for a full description of route templates and related options.</span></span>

## <a name="route-name"></a><span data-ttu-id="a1735-219">Nombre de ruta</span><span class="sxs-lookup"><span data-stu-id="a1735-219">Route Name</span></span>

<span data-ttu-id="a1735-220">El código siguiente define un *nombre de ruta* de `Products_List`:</span><span class="sxs-lookup"><span data-stu-id="a1735-220">The following code  defines a *route name* of `Products_List`:</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="a1735-221">Los nombres de ruta se pueden utilizar para generar una dirección URL basada en una ruta específica.</span><span class="sxs-lookup"><span data-stu-id="a1735-221">Route names can be used to generate a URL based on a specific route.</span></span> <span data-ttu-id="a1735-222">Los nombres de ruta no tienen ningún impacto en el comportamiento de coincidencia de direcciones URL del enrutamiento, y solo se usan para la generación de direcciones URL.</span><span class="sxs-lookup"><span data-stu-id="a1735-222">Route names have no impact on the URL matching behavior of routing and are only used for URL generation.</span></span> <span data-ttu-id="a1735-223">Los nombres de ruta deben ser únicos en toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a1735-223">Route names must be unique application-wide.</span></span>

> [!NOTE]
> <span data-ttu-id="a1735-224">Compare esto con la *ruta predeterminada* convencional, que define el parámetro `id` como opcional (`{id?}`).</span><span class="sxs-lookup"><span data-stu-id="a1735-224">Contrast this with the conventional *default route*, which defines the `id` parameter as optional (`{id?}`).</span></span> <span data-ttu-id="a1735-225">Esta capacidad de especificar con precisión las API tiene sus ventajas, como permitir que `/products` y `/products/5` se envíen a acciones diferentes.</span><span class="sxs-lookup"><span data-stu-id="a1735-225">This ability to precisely specify APIs has advantages, such as  allowing `/products` and `/products/5` to be dispatched to different actions.</span></span>

<a name="routing-combining-ref-label"></a>

### <a name="combining-routes"></a><span data-ttu-id="a1735-226">Combinación de rutas</span><span class="sxs-lookup"><span data-stu-id="a1735-226">Combining routes</span></span>

<span data-ttu-id="a1735-227">Para que el enrutamiento mediante atributos sea menos repetitivo, los atributos de ruta del controlador se combinan con los atributos de ruta de las acciones individuales.</span><span class="sxs-lookup"><span data-stu-id="a1735-227">To make attribute routing less repetitive, route attributes on the controller are combined with route attributes on the individual actions.</span></span> <span data-ttu-id="a1735-228">Las plantillas de ruta definidas en el controlador se anteponen a las plantillas de ruta de las acciones.</span><span class="sxs-lookup"><span data-stu-id="a1735-228">Any route templates defined on the controller are prepended to route templates on the actions.</span></span> <span data-ttu-id="a1735-229">La colocación de un atributo de ruta en el controlador hace que **todas** las acciones del controlador usen el enrutamiento mediante atributos.</span><span class="sxs-lookup"><span data-stu-id="a1735-229">Placing a route attribute on the controller makes **all** actions in the controller use attribute routing.</span></span>

```csharp
[Route("products")]
public class ProductsApiController : Controller
{
   [HttpGet]
   public IActionResult ListProducts() { ... }

   [HttpGet("{id}")]
   public ActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="a1735-230">En este ejemplo, la ruta de dirección URL `/products` puede coincidir con `ProductsApi.ListProducts` y la ruta de dirección URL `/products/5` puede coincidir con `ProductsApi.GetProduct(int)`.</span><span class="sxs-lookup"><span data-stu-id="a1735-230">In this example the URL path `/products` can match `ProductsApi.ListProducts`, and the URL path `/products/5` can match `ProductsApi.GetProduct(int)`.</span></span> <span data-ttu-id="a1735-231">Ambas acciones coinciden solo con HTTP `GET` porque incluyen `HttpGetAttribute`.</span><span class="sxs-lookup"><span data-stu-id="a1735-231">Both of these actions only match HTTP `GET` because they're decorated with the `HttpGetAttribute`.</span></span>

<span data-ttu-id="a1735-232">Las plantillas de ruta aplicadas a una acción que comienzan por `/` o `~/` no se combinan con las plantillas de ruta que se aplican al controlador.</span><span class="sxs-lookup"><span data-stu-id="a1735-232">Route templates applied to an action that begin with `/` or `~/` don't get combined with route templates applied to the controller.</span></span> <span data-ttu-id="a1735-233">En este ejemplo coinciden un conjunto de rutas de dirección URL similares a la *ruta predeterminada*.</span><span class="sxs-lookup"><span data-stu-id="a1735-233">This example matches a set of URL paths similar to the *default route*.</span></span>

```csharp
[Route("Home")]
public class HomeController : Controller
{
    [Route("")]      // Combines to define the route template "Home"
    [Route("Index")] // Combines to define the route template "Home/Index"
    [Route("/")]     // Doesn't combine, defines the route template ""
    public IActionResult Index()
    {
        ViewData["Message"] = "Home index";
        var url = Url.Action("Index", "Home");
        ViewData["Message"] = "Home index" + "var url = Url.Action; =  " + url;
        return View();
    }

    [Route("About")] // Combines to define the route template "Home/About"
    public IActionResult About()
    {
        return View();
    }   
}
```

<a name="routing-ordering-ref-label"></a>

### <a name="ordering-attribute-routes"></a><span data-ttu-id="a1735-234">Ordenación de rutas de atributo</span><span class="sxs-lookup"><span data-stu-id="a1735-234">Ordering attribute routes</span></span>

<span data-ttu-id="a1735-235">A diferencia de las rutas convencionales que se ejecutan en un orden definido, el enrutamiento mediante atributos genera un árbol y busca coincidir con todas las rutas al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="a1735-235">In contrast to conventional routes which execute in a defined order, attribute routing builds a tree and matches all routes simultaneously.</span></span> <span data-ttu-id="a1735-236">Es como si las entradas de ruta se colocasen en un orden ideal; las rutas más específicas tienen la oportunidad de ejecutarse antes que las rutas más generales.</span><span class="sxs-lookup"><span data-stu-id="a1735-236">This behaves as-if the route entries were placed in an ideal ordering; the most specific routes have a chance to execute before the more general routes.</span></span>

<span data-ttu-id="a1735-237">Por ejemplo, una ruta como `blog/search/{topic}` es más específica que una ruta como `blog/{*article}`.</span><span class="sxs-lookup"><span data-stu-id="a1735-237">For example, a route like `blog/search/{topic}` is more specific than a route like `blog/{*article}`.</span></span> <span data-ttu-id="a1735-238">Desde un punto de vista lógico, la ruta `blog/search/{topic}` se ejecuta en primer lugar de forma predeterminada, ya que ese es el único orden significativo.</span><span class="sxs-lookup"><span data-stu-id="a1735-238">Logically speaking the `blog/search/{topic}` route 'runs' first, by default, because that's the only sensible ordering.</span></span> <span data-ttu-id="a1735-239">En el enrutamiento convencional, el desarrollador es responsable de colocar las rutas en el orden deseado.</span><span class="sxs-lookup"><span data-stu-id="a1735-239">Using conventional routing, the developer is  responsible for placing routes in the desired order.</span></span>

<span data-ttu-id="a1735-240">Las rutas de atributo pueden configurar un orden mediante la propiedad `Order` de todos los atributos de ruta proporcionados por el marco.</span><span class="sxs-lookup"><span data-stu-id="a1735-240">Attribute routes can configure an order, using the `Order` property of all of the framework provided route attributes.</span></span> <span data-ttu-id="a1735-241">Las rutas se procesan de acuerdo con el orden ascendente de la propiedad `Order`.</span><span class="sxs-lookup"><span data-stu-id="a1735-241">Routes are processed according to an ascending sort of the `Order` property.</span></span> <span data-ttu-id="a1735-242">El orden predeterminado es `0`.</span><span class="sxs-lookup"><span data-stu-id="a1735-242">The default order is `0`.</span></span> <span data-ttu-id="a1735-243">Si una ruta se configura con `Order = -1`, se ejecutará antes que las rutas que establecen un orden.</span><span class="sxs-lookup"><span data-stu-id="a1735-243">Setting a route using `Order = -1` will run before routes that don't set an order.</span></span> <span data-ttu-id="a1735-244">Si una ruta se configura con `Order = 1`, se ejecutará después del orden de rutas predeterminado.</span><span class="sxs-lookup"><span data-stu-id="a1735-244">Setting a route using `Order = 1` will run after default route ordering.</span></span>

> [!TIP]
> <span data-ttu-id="a1735-245">Evite depender de `Order`.</span><span class="sxs-lookup"><span data-stu-id="a1735-245">Avoid depending on `Order`.</span></span> <span data-ttu-id="a1735-246">Si su espacio de direcciones URL requiere unos valores de orden explícitos para un enrutamiento correcto, es probable que también sea confuso para los clientes.</span><span class="sxs-lookup"><span data-stu-id="a1735-246">If your URL-space requires explicit order values to route correctly, then it's likely confusing to clients as well.</span></span> <span data-ttu-id="a1735-247">Por lo general, el enrutamiento mediante atributos seleccionará la ruta correcta con la coincidencia de dirección URL.</span><span class="sxs-lookup"><span data-stu-id="a1735-247">In general attribute routing will select the correct route with URL matching.</span></span> <span data-ttu-id="a1735-248">Si el orden predeterminado que se usa para la generación de direcciones URL no funciona, normalmente es más sencillo utilizar el nombre de ruta como una invalidación que aplicar la propiedad `Order`.</span><span class="sxs-lookup"><span data-stu-id="a1735-248">If the default order used for URL generation isn't working, using route name as an override is usually simpler than applying the `Order` property.</span></span>

<span data-ttu-id="a1735-249">El enrutamiento del controlador de MVC y el enrutamiento de Razor Pages comparten una implementación.</span><span class="sxs-lookup"><span data-stu-id="a1735-249">Razor Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="a1735-250">La información sobre el orden de la ruta de los temas de Razor Pages se encuentra disponible en [Convenciones de aplicación y de ruta de Razor Pages: Orden de la ruta](xref:razor-pages/razor-pages-conventions#route-order).</span><span class="sxs-lookup"><span data-stu-id="a1735-250">Information on route order in the Razor Pages topics is available at [Razor Pages route and app conventions: Route order](xref:razor-pages/razor-pages-conventions#route-order).</span></span>

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a><span data-ttu-id="a1735-251">Reemplazo de tokens en plantillas de ruta ([controller], [action], [area])</span><span class="sxs-lookup"><span data-stu-id="a1735-251">Token replacement in route templates ([controller], [action], [area])</span></span>

<span data-ttu-id="a1735-252">Para mayor comodidad, las rutas de atributo admiten *reemplazo de token*. Para ello, incluyen un token entre corchetes (`[`, `]`).</span><span class="sxs-lookup"><span data-stu-id="a1735-252">For convenience, attribute routes support *token replacement* by enclosing a token in square-braces (`[`, `]`).</span></span> <span data-ttu-id="a1735-253">Los tokens `[action]`, `[area]` y `[controller]` se reemplazan con los valores del nombre de la acción, el nombre del área y el nombre del controlador de la acción donde se define la ruta.</span><span class="sxs-lookup"><span data-stu-id="a1735-253">The tokens `[action]`, `[area]`, and `[controller]` are replaced with the values of the action name, area name, and controller name from the action where the route is defined.</span></span> <span data-ttu-id="a1735-254">En este ejemplo, las acciones coinciden con las rutas de dirección URL, tal como se describe en los comentarios:</span><span class="sxs-lookup"><span data-stu-id="a1735-254">In the following example, the actions match URL paths as described in the comments:</span></span>

[!code-csharp[](routing/sample/main/Controllers/ProductsController.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="a1735-255">El reemplazo del token se produce como último paso de la creación de las rutas de atributo.</span><span class="sxs-lookup"><span data-stu-id="a1735-255">Token replacement occurs as the last step of building the attribute routes.</span></span> <span data-ttu-id="a1735-256">El ejemplo anterior se comportará igual que el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="a1735-256">The above example will behave the same as the following code:</span></span>

[!code-csharp[](routing/sample/main/Controllers/ProductsController2.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="a1735-257">Las rutas de atributo también se pueden combinar con la herencia.</span><span class="sxs-lookup"><span data-stu-id="a1735-257">Attribute routes can also be combined with inheritance.</span></span> <span data-ttu-id="a1735-258">Esto es especialmente eficaz si se combina con el reemplazo de token.</span><span class="sxs-lookup"><span data-stu-id="a1735-258">This is particularly powerful combined with token replacement.</span></span>

```csharp
[Route("api/[controller]")]
public abstract class MyBaseController : Controller { ... }

public class ProductsController : MyBaseController
{
   [HttpGet] // Matches '/api/Products'
   public IActionResult List() { ... }

   [HttpPut("{id}")] // Matches '/api/Products/{id}'
   public IActionResult Edit(int id) { ... }
}
```

<span data-ttu-id="a1735-259">El reemplazo de token también se aplica a los nombres de ruta definidos por las rutas de atributo.</span><span class="sxs-lookup"><span data-stu-id="a1735-259">Token replacement also applies to route names defined by attribute routes.</span></span> <span data-ttu-id="a1735-260">`[Route("[controller]/[action]", Name="[controller]_[action]")]` genera un nombre de ruta único para cada acción.</span><span class="sxs-lookup"><span data-stu-id="a1735-260">`[Route("[controller]/[action]", Name="[controller]_[action]")]` generates a unique route name for each action.</span></span>

<span data-ttu-id="a1735-261">Para que el delimitador de reemplazo de token `[` o `]` coincida, repita el carácter (`[[` o `]]`) para usarlo como secuencia de escape.</span><span class="sxs-lookup"><span data-stu-id="a1735-261">To match the literal token replacement delimiter `[` or  `]`, escape it by repeating the character (`[[` or `]]`).</span></span>

::: moniker range=">= aspnetcore-2.2"

<a name="routing-token-replacement-transformers-ref-label"></a>

### <a name="use-a-parameter-transformer-to-customize-token-replacement"></a><span data-ttu-id="a1735-262">Usar un transformador de parámetro para personalizar el reemplazo de tokens</span><span class="sxs-lookup"><span data-stu-id="a1735-262">Use a parameter transformer to customize token replacement</span></span>

<span data-ttu-id="a1735-263">El reemplazo de tokens se puede personalizarse mediante un transformador de parámetro.</span><span class="sxs-lookup"><span data-stu-id="a1735-263">Token replacement can be customized using a parameter transformer.</span></span> <span data-ttu-id="a1735-264">Un transformador de parámetro implementa `IOutboundParameterTransformer` y transforma el valor de parámetros.</span><span class="sxs-lookup"><span data-stu-id="a1735-264">A parameter transformer implements `IOutboundParameterTransformer` and transforms the value of parameters.</span></span> <span data-ttu-id="a1735-265">Por ejemplo, un transformador de parámetros `SlugifyParameterTransformer` personalizado cambia el valor de ruta `SubscriptionManagement` a `subscription-management`.</span><span class="sxs-lookup"><span data-stu-id="a1735-265">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`.</span></span>

<span data-ttu-id="a1735-266">`RouteTokenTransformerConvention` es una convención de modelo de aplicación que:</span><span class="sxs-lookup"><span data-stu-id="a1735-266">The `RouteTokenTransformerConvention` is an application model convention that:</span></span>

* <span data-ttu-id="a1735-267">Aplica un transformador de parámetro a todas las rutas de atributo en una aplicación.</span><span class="sxs-lookup"><span data-stu-id="a1735-267">Applies a parameter transformer to all attribute routes in an application.</span></span>
* <span data-ttu-id="a1735-268">Personaliza los valores de token de ruta de atributo que se reemplazan.</span><span class="sxs-lookup"><span data-stu-id="a1735-268">Customizes the attribute route token values as they are replaced.</span></span>

```csharp
public class SubscriptionManagementController : Controller
{
    [HttpGet("[controller]/[action]")] // Matches '/subscription-management/list-all'
    public IActionResult ListAll() { ... }
}
```

<span data-ttu-id="a1735-269">`RouteTokenTransformerConvention` está registrado como una opción en `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a1735-269">The `RouteTokenTransformerConvention` is registered as an option in `ConfigureServices`.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc(options =>
    {
        options.Conventions.Add(new RouteTokenTransformerConvention(
                                     new SlugifyParameterTransformer()));
    });
}

public class SlugifyParameterTransformer : IOutboundParameterTransformer
{
    public string TransformOutbound(object value)
    {
        if (value == null) { return null; }

        // Slugify value
        return Regex.Replace(value.ToString(), "([a-z])([A-Z])", "$1-$2").ToLower();
    }
}
```

::: moniker-end

<a name="routing-multiple-routes-ref-label"></a>

### <a name="multiple-routes"></a><span data-ttu-id="a1735-270">Varias rutas</span><span class="sxs-lookup"><span data-stu-id="a1735-270">Multiple Routes</span></span>

<span data-ttu-id="a1735-271">El enrutamiento mediante atributos permite definir varias rutas que llegan a la misma acción.</span><span class="sxs-lookup"><span data-stu-id="a1735-271">Attribute routing supports defining multiple routes that reach the same action.</span></span> <span data-ttu-id="a1735-272">El uso más común de esto es imitar el comportamiento de la *ruta convencional predeterminada* tal como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="a1735-272">The most common usage of this is to mimic the behavior of the *default conventional route* as shown in the following example:</span></span>

```csharp
[Route("[controller]")]
public class ProductsController : Controller
{
   [Route("")]     // Matches 'Products'
   [Route("Index")] // Matches 'Products/Index'
   public IActionResult Index()
}
```

<span data-ttu-id="a1735-273">La colocación de varios atributos de ruta en el controlador significa que cada uno de ellos se combinará con cada uno de los atributos de ruta en los métodos de acción.</span><span class="sxs-lookup"><span data-stu-id="a1735-273">Putting multiple route attributes on the controller means that each one will combine with each of the route attributes on the action methods.</span></span>

```csharp
[Route("Store")]
[Route("[controller]")]
public class ProductsController : Controller
{
   [HttpPost("Buy")]     // Matches 'Products/Buy' and 'Store/Buy'
   [HttpPost("Checkout")] // Matches 'Products/Checkout' and 'Store/Checkout'
   public IActionResult Buy()
}
```

<span data-ttu-id="a1735-274">Cuando en una acción se colocan varios atributos de ruta (que implementan `IActionConstraint`), cada restricción de acción se combina con la plantilla de ruta del atributo que lo define.</span><span class="sxs-lookup"><span data-stu-id="a1735-274">When multiple route attributes (that implement `IActionConstraint`) are placed on an action, then each action constraint combines with the route template from the attribute that defined it.</span></span>

```csharp
[Route("api/[controller]")]
public class ProductsController : Controller
{
   [HttpPut("Buy")]      // Matches PUT 'api/Products/Buy'
   [HttpPost("Checkout")] // Matches POST 'api/Products/Checkout'
   public IActionResult Buy()
}
```

> [!TIP]
> <span data-ttu-id="a1735-275">Si bien el uso de varias rutas en acciones puede parecer eficaz, es mejor que el espacio de direcciones URL de la aplicación sea sencillo y esté bien definido.</span><span class="sxs-lookup"><span data-stu-id="a1735-275">While using multiple routes on actions can seem powerful, it's better to keep your application's URL space simple and well-defined.</span></span> <span data-ttu-id="a1735-276">Utilice varias rutas en acciones solo cuando sea necesario, por ejemplo, para admitir a clientes existentes.</span><span class="sxs-lookup"><span data-stu-id="a1735-276">Use multiple routes on actions only where needed, for example to support existing clients.</span></span>

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a><span data-ttu-id="a1735-277">Especificación de parámetros opcionales de ruta de atributo, valores predeterminados y restricciones</span><span class="sxs-lookup"><span data-stu-id="a1735-277">Specifying attribute route optional parameters, default values, and constraints</span></span>

<span data-ttu-id="a1735-278">Las rutas de atributo admiten la misma sintaxis en línea que las rutas convencionales para especificar parámetros opcionales, valores predeterminados y restricciones.</span><span class="sxs-lookup"><span data-stu-id="a1735-278">Attribute routes support the same inline syntax as conventional routes to specify optional parameters, default values, and constraints.</span></span>

```csharp
[HttpPost("product/{id:int}")]
public IActionResult ShowProduct(int id)
{
   // ...
}
```

<span data-ttu-id="a1735-279">Consulte [Referencia de plantilla de ruta](../../fundamentals/routing.md#route-template-reference) para obtener una descripción detallada de la sintaxis de la plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="a1735-279">See [Route Template Reference](../../fundamentals/routing.md#route-template-reference) for a detailed description of route template syntax.</span></span>

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a><span data-ttu-id="a1735-280">Atributos de ruta personalizados con `IRouteTemplateProvider`</span><span class="sxs-lookup"><span data-stu-id="a1735-280">Custom route attributes using `IRouteTemplateProvider`</span></span>

<span data-ttu-id="a1735-281">Todos los atributos de ruta proporcionados en el marco (`[Route(...)]`, `[HttpGet(...)]`, etc.) implementan la interfaz `IRouteTemplateProvider`.</span><span class="sxs-lookup"><span data-stu-id="a1735-281">All of the route attributes provided in the framework ( `[Route(...)]`, `[HttpGet(...)]` , etc.) implement the `IRouteTemplateProvider` interface.</span></span> <span data-ttu-id="a1735-282">Al iniciarse la aplicación, MVC busca atributos en las clases de controlador y los métodos de acción, y usa los que implementan `IRouteTemplateProvider` para generar el conjunto inicial de rutas.</span><span class="sxs-lookup"><span data-stu-id="a1735-282">MVC looks for attributes on controller classes and action methods when the app starts and uses the ones that implement `IRouteTemplateProvider` to build the initial set of routes.</span></span>

<span data-ttu-id="a1735-283">Implemente `IRouteTemplateProvider` para definir sus propios atributos de ruta.</span><span class="sxs-lookup"><span data-stu-id="a1735-283">You can implement `IRouteTemplateProvider` to define your own route attributes.</span></span> <span data-ttu-id="a1735-284">Cada `IRouteTemplateProvider` le permite definir una única ruta con una plantilla de ruta, un orden y un nombre personalizados:</span><span class="sxs-lookup"><span data-stu-id="a1735-284">Each `IRouteTemplateProvider` allows you to define a single route with a custom route template, order, and name:</span></span>

```csharp
public class MyApiControllerAttribute : Attribute, IRouteTemplateProvider
{
   public string Template => "api/[controller]";

   public int? Order { get; set; }

   public string Name { get; set; }
}
```

<span data-ttu-id="a1735-285">El atributo del ejemplo anterior establece automáticamente `Template` en `"api/[controller]"` cuando se aplica `[MyApiController]`.</span><span class="sxs-lookup"><span data-stu-id="a1735-285">The attribute from the above example automatically sets the `Template` to `"api/[controller]"` when `[MyApiController]` is applied.</span></span>

<a name="routing-app-model-ref-label"></a>

### <a name="using-application-model-to-customize-attribute-routes"></a><span data-ttu-id="a1735-286">Uso del modelo de aplicación para personalizar las rutas de atributo</span><span class="sxs-lookup"><span data-stu-id="a1735-286">Using Application Model to customize attribute routes</span></span>

<span data-ttu-id="a1735-287">El *modelo de aplicación* es un modelo de objetos creado durante el inicio con todos los metadatos utilizados por MVC para enrutar y ejecutar las acciones.</span><span class="sxs-lookup"><span data-stu-id="a1735-287">The *application model* is an object model created at startup with all of the metadata used by MVC to route and execute your actions.</span></span> <span data-ttu-id="a1735-288">El *modelo de aplicación* incluye todos los datos recopilados de los atributos de ruta (a través de `IRouteTemplateProvider`).</span><span class="sxs-lookup"><span data-stu-id="a1735-288">The *application model* includes all of the data gathered from route attributes (through `IRouteTemplateProvider`).</span></span> <span data-ttu-id="a1735-289">Puede escribir *convenciones* para modificar el modelo de aplicación en el momento del inicio y personalizar el comportamiento del enrutamiento.</span><span class="sxs-lookup"><span data-stu-id="a1735-289">You can write *conventions* to modify the application model at startup time to customize how routing behaves.</span></span> <span data-ttu-id="a1735-290">En esta sección se muestra un ejemplo sencillo de personalización del enrutamiento mediante el modelo de aplicación.</span><span class="sxs-lookup"><span data-stu-id="a1735-290">This section shows a simple example of customizing routing using application model.</span></span>

[!code-csharp[](routing/sample/main/NamespaceRoutingConvention.cs)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a><span data-ttu-id="a1735-291">Enrutamiento mixto: enrutamiento mediante atributos frente a enrutamiento convencional</span><span class="sxs-lookup"><span data-stu-id="a1735-291">Mixed routing: Attribute routing vs conventional routing</span></span>

<span data-ttu-id="a1735-292">Las aplicaciones MVC pueden combinar el uso de enrutamiento convencional y enrutamiento mediante atributos.</span><span class="sxs-lookup"><span data-stu-id="a1735-292">MVC applications can mix the use of conventional routing and attribute routing.</span></span> <span data-ttu-id="a1735-293">Es habitual usar las rutas convencionales para controladores que sirven páginas HTML para los exploradores, y el enrutamiento mediante atributos para los controladores que sirven las API de REST.</span><span class="sxs-lookup"><span data-stu-id="a1735-293">It's typical to use conventional routes for controllers serving HTML pages for browsers, and attribute routing for controllers serving REST APIs.</span></span>

<span data-ttu-id="a1735-294">Las acciones se enrutan bien mediante convención o bien mediante atributos.</span><span class="sxs-lookup"><span data-stu-id="a1735-294">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="a1735-295">Colocar una ruta en el controlador o la acción hace que se enrute mediante atributos.</span><span class="sxs-lookup"><span data-stu-id="a1735-295">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="a1735-296">Las acciones que definen rutas de atributo no se pueden alcanzar a través de las rutas convencionales y viceversa.</span><span class="sxs-lookup"><span data-stu-id="a1735-296">Actions that define attribute routes cannot be reached through the conventional routes and vice-versa.</span></span> <span data-ttu-id="a1735-297">**Cualquier** atributo de ruta en el controlador hace que todas las acciones del controlador se enruten mediante atributos.</span><span class="sxs-lookup"><span data-stu-id="a1735-297">**Any** route attribute on the controller makes all actions in the controller attribute routed.</span></span>

> [!NOTE]
> <span data-ttu-id="a1735-298">Lo que distingue los dos tipos de sistemas de enrutamiento es el proceso que se aplica después de que una dirección URL coincida con una plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="a1735-298">What distinguishes the two types of routing systems is the process applied after a URL matches a route template.</span></span> <span data-ttu-id="a1735-299">En el enrutamiento convencional, los valores de ruta de la coincidencia se usan para elegir la acción y el controlador en una tabla de búsqueda de todas las acciones enrutadas convencionales.</span><span class="sxs-lookup"><span data-stu-id="a1735-299">In conventional routing, the route values from the match are used to choose the action and controller from a lookup table of all conventional routed actions.</span></span> <span data-ttu-id="a1735-300">En el enrutamiento mediante atributos, cada plantilla ya está asociada a una acción y no es necesaria ninguna otra búsqueda.</span><span class="sxs-lookup"><span data-stu-id="a1735-300">In attribute routing, each template is already associated with an action, and no further lookup is needed.</span></span>

## <a name="complex-segments"></a><span data-ttu-id="a1735-301">Segmentos complejos</span><span class="sxs-lookup"><span data-stu-id="a1735-301">Complex segments</span></span>

<span data-ttu-id="a1735-302">Los segmentos complejos (por ejemplo, `[Route("/dog{token}cat")]`), se procesan buscando coincidencias de literales de derecha a izquierda de un modo no expansivo.</span><span class="sxs-lookup"><span data-stu-id="a1735-302">Complex segments (for example, `[Route("/dog{token}cat")]`), are processed by matching up literals from right to left in a non-greedy way.</span></span> <span data-ttu-id="a1735-303">Para ver una descripción, eche un vistazo al [código fuente](https://github.com/aspnet/Routing/blob/9cea167cfac36cf034dbb780e3f783114ef94780/src/Microsoft.AspNetCore.Routing/Patterns/RoutePatternMatcher.cs#L296).</span><span class="sxs-lookup"><span data-stu-id="a1735-303">See [the source code](https://github.com/aspnet/Routing/blob/9cea167cfac36cf034dbb780e3f783114ef94780/src/Microsoft.AspNetCore.Routing/Patterns/RoutePatternMatcher.cs#L296) for a description.</span></span> <span data-ttu-id="a1735-304">Para más información, vea [este problema](https://github.com/aspnet/AspNetCore.Docs/issues/8197).</span><span class="sxs-lookup"><span data-stu-id="a1735-304">For more information, see [this issue](https://github.com/aspnet/AspNetCore.Docs/issues/8197).</span></span>

<a name="routing-url-gen-ref-label"></a>

## <a name="url-generation"></a><span data-ttu-id="a1735-305">Generación de direcciones URL</span><span class="sxs-lookup"><span data-stu-id="a1735-305">URL Generation</span></span>

<span data-ttu-id="a1735-306">Las aplicaciones MVC pueden usar características de generación de direcciones URL de enrutamiento para generar vínculos URL a las acciones.</span><span class="sxs-lookup"><span data-stu-id="a1735-306">MVC applications can use routing's URL generation features to generate URL links to actions.</span></span> <span data-ttu-id="a1735-307">La generación de direcciones URL elimina las direcciones URL codificadas de forma rígida, por lo que el código es más compacto y fácil de mantener.</span><span class="sxs-lookup"><span data-stu-id="a1735-307">Generating URLs eliminates hardcoding URLs, making your code more robust and maintainable.</span></span> <span data-ttu-id="a1735-308">Esta sección se centra en las características de generación de direcciones URL proporcionadas por MVC y solo aborda los conceptos básicos de su funcionamiento.</span><span class="sxs-lookup"><span data-stu-id="a1735-308">This section focuses on the URL generation features provided by MVC and will only cover basics of how URL generation works.</span></span> <span data-ttu-id="a1735-309">Consulte [Enrutamiento](../../fundamentals/routing.md) para obtener una descripción detallada de la generación de direcciones URL.</span><span class="sxs-lookup"><span data-stu-id="a1735-309">See [Routing](../../fundamentals/routing.md) for a detailed description of URL generation.</span></span>

<span data-ttu-id="a1735-310">La interfaz `IUrlHelper` es la pieza subyacente de la infraestructura entre MVC y el enrutamiento para la generación de direcciones URL.</span><span class="sxs-lookup"><span data-stu-id="a1735-310">The `IUrlHelper` interface is the underlying piece of infrastructure between MVC and routing for URL generation.</span></span> <span data-ttu-id="a1735-311">Encontrará una instancia de `IUrlHelper` disponible a través de la propiedad `Url` en los controladores, las vistas y los componentes de vista.</span><span class="sxs-lookup"><span data-stu-id="a1735-311">You'll find an instance of `IUrlHelper` available through the `Url` property in controllers, views, and view components.</span></span>

<span data-ttu-id="a1735-312">En este ejemplo, la interfaz `IUrlHelper` se usa a través de la propiedad `Controller.Url` para generar una dirección URL a otra acción.</span><span class="sxs-lookup"><span data-stu-id="a1735-312">In this example, the `IUrlHelper` interface is used through the `Controller.Url` property to generate a URL to another action.</span></span>

[!code-csharp[](routing/sample/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

<span data-ttu-id="a1735-313">Si la aplicación está usando la ruta convencional predeterminada, el valor de la variable `url` será la cadena de ruta de dirección URL `/UrlGeneration/Destination`.</span><span class="sxs-lookup"><span data-stu-id="a1735-313">If the application is using the default conventional route, the value of the `url` variable will be the URL path string `/UrlGeneration/Destination`.</span></span> <span data-ttu-id="a1735-314">Esta ruta de dirección URL se crea por enrutamiento mediante la combinación de los valores de ruta de la solicitud actual (valores de ambiente) con los valores pasados a `Url.Action`, y sustituyendo esos valores en la plantilla de ruta:</span><span class="sxs-lookup"><span data-stu-id="a1735-314">This URL path is created by routing by combining the route values from the current request (ambient values), with the values passed to `Url.Action` and substituting those values into the route template:</span></span>

```
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

<span data-ttu-id="a1735-315">El valor de cada uno de los parámetros de ruta incluidos en la plantilla de ruta se sustituye por nombres que coincidan con los valores y los valores de ambiente.</span><span class="sxs-lookup"><span data-stu-id="a1735-315">Each route parameter in the route template has its value substituted by matching names with the values and ambient values.</span></span> <span data-ttu-id="a1735-316">Si un parámetro de ruta no tiene un valor, puede utilizar un valor predeterminado en caso de tenerlo, o se puede omitir si es opcional (como en el caso de `id` en este ejemplo).</span><span class="sxs-lookup"><span data-stu-id="a1735-316">A route parameter that doesn't have a value can use a default value if it has one, or be skipped if it's optional (as in the case of `id` in this example).</span></span> <span data-ttu-id="a1735-317">La generación de direcciones URL producirá un error si cualquiera de los parámetros de ruta necesarios no tiene un valor correspondiente.</span><span class="sxs-lookup"><span data-stu-id="a1735-317">URL generation will fail if any required route parameter doesn't have a corresponding value.</span></span> <span data-ttu-id="a1735-318">Si se produce un error en la generación de direcciones URL para una ruta, se prueba con la ruta siguiente hasta que se hayan probado todas las rutas o se encuentra una coincidencia.</span><span class="sxs-lookup"><span data-stu-id="a1735-318">If URL generation fails for a route, the next route is tried until all routes have been tried or a match is found.</span></span>

<span data-ttu-id="a1735-319">En el ejemplo anterior de `Url.Action` se supone que el enrutamiento es convencional, pero la generación de direcciones URL funciona del mismo modo con el enrutamiento mediante atributos, si bien los conceptos son diferentes.</span><span class="sxs-lookup"><span data-stu-id="a1735-319">The example of `Url.Action` above assumes conventional routing, but URL generation works similarly with attribute routing, though the concepts are different.</span></span> <span data-ttu-id="a1735-320">En el enrutamiento convencional, los valores de ruta se usan para expandir una plantilla; los valores de ruta de `controller` y `action` suelen aparecer en esa plantilla. Esto es válido porque las direcciones URL que coinciden con el enrutamiento se adhieren a una *convención*.</span><span class="sxs-lookup"><span data-stu-id="a1735-320">With conventional routing, the route values are used to expand a template, and the route values for `controller` and `action` usually appear in that template - this works because the URLs matched by routing adhere to a *convention*.</span></span> <span data-ttu-id="a1735-321">En el enrutamiento mediante atributos, no está permitido que los valores de ruta de `controller` y `action` aparezcan en la plantilla. En vez de eso, se utilizan para buscar la plantilla que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="a1735-321">In attribute routing, the route values for `controller` and `action` are not allowed to appear in the template - they're instead used to look up which template to use.</span></span>

<span data-ttu-id="a1735-322">Este ejemplo utiliza la enrutamiento mediante atributos:</span><span class="sxs-lookup"><span data-stu-id="a1735-322">This example uses attribute routing:</span></span>

[!code-csharp[](routing/sample/main/StartupUseMvc.cs?name=snippet_1)]

[!code-csharp[](routing/sample/main/Controllers/UrlGenerationControllerAttr.cs?name=snippet_1)]

<span data-ttu-id="a1735-323">MVC genera una tabla de búsqueda de todas las acciones enrutadas mediante atributos y hará coincidir los valores `controller` y `action` para seleccionar la plantilla de ruta que se usará para la generación de direcciones URL.</span><span class="sxs-lookup"><span data-stu-id="a1735-323">MVC builds a lookup table of all attribute routed actions and will match the `controller` and `action` values to select the route template to use for URL generation.</span></span> <span data-ttu-id="a1735-324">En el ejemplo anterior se genera `custom/url/to/destination`.</span><span class="sxs-lookup"><span data-stu-id="a1735-324">In the sample above,   `custom/url/to/destination` is generated.</span></span>

### <a name="generating-urls-by-action-name"></a><span data-ttu-id="a1735-325">Generación de direcciones URL por nombre de acción</span><span class="sxs-lookup"><span data-stu-id="a1735-325">Generating URLs by action name</span></span>

<span data-ttu-id="a1735-326">`Url.Action` (`IUrlHelper` .</span><span class="sxs-lookup"><span data-stu-id="a1735-326">`Url.Action` (`IUrlHelper` .</span></span> <span data-ttu-id="a1735-327">`Action`) y todas las sobrecargas relacionadas se basan en la idea de especificar el destino del vínculo mediante un nombre de controlador y un nombre de acción.</span><span class="sxs-lookup"><span data-stu-id="a1735-327">`Action`) and all related overloads all are based on that idea that you want to specify what you're linking to by specifying a controller name and action name.</span></span>

> [!NOTE]
> <span data-ttu-id="a1735-328">Cuando se usa `Url.Action`, se especifican los valores actuales de la ruta para `controller` y `action`. Los valores de `controller` y `action` forman parte tanto de los *valores de ambiente* **y**como de los *valores*.</span><span class="sxs-lookup"><span data-stu-id="a1735-328">When using `Url.Action`, the current route values for `controller` and `action` are specified for you - the value of `controller` and `action` are part of both *ambient values* **and** *values*.</span></span> <span data-ttu-id="a1735-329">El método `Url.Action` siempre utiliza los valores actuales de `action` y `controller` y genera una ruta de dirección URL que dirige a la acción actual.</span><span class="sxs-lookup"><span data-stu-id="a1735-329">The method `Url.Action`, always uses the current values of `action` and `controller` and will generate a URL path that routes to the current action.</span></span>

<span data-ttu-id="a1735-330">Intentos de enrutamiento para utilizar los valores en los valores de ambiente para rellenar la información que no se proporcionó al generar una dirección URL.</span><span class="sxs-lookup"><span data-stu-id="a1735-330">Routing attempts to use the values in ambient values to fill in information that you didn't provide when generating a URL.</span></span> <span data-ttu-id="a1735-331">Al utilizar una ruta como `{a}/{b}/{c}/{d}` y los valores de ambiente `{ a = Alice, b = Bob, c = Carol, d = David }`, el enrutamiento tiene suficiente información para generar una dirección URL sin ningún valor adicional, puesto que todos los parámetros de ruta tienen un valor.</span><span class="sxs-lookup"><span data-stu-id="a1735-331">Using a route like `{a}/{b}/{c}/{d}` and ambient values `{ a = Alice, b = Bob, c = Carol, d = David }`, routing has enough information to generate a URL without any additional values - since all route parameters have a value.</span></span> <span data-ttu-id="a1735-332">Si se agregó el valor `{ d = Donovan }`, el valor `{ d = David }` se ignorará y la ruta de dirección URL generada será `Alice/Bob/Carol/Donovan`.</span><span class="sxs-lookup"><span data-stu-id="a1735-332">If you added the value `{ d = Donovan }`, the value `{ d = David }` would be ignored, and the generated URL path would be `Alice/Bob/Carol/Donovan`.</span></span>

> [!WARNING]
> <span data-ttu-id="a1735-333">Las rutas de dirección URL son jerárquicas.</span><span class="sxs-lookup"><span data-stu-id="a1735-333">URL paths are hierarchical.</span></span> <span data-ttu-id="a1735-334">En el ejemplo anterior, si se agrega el valor `{ c = Cheryl }`, ambos valores `{ c = Carol, d = David }` se ignorarán.</span><span class="sxs-lookup"><span data-stu-id="a1735-334">In the example above, if you added the value `{ c = Cheryl }`, both of the values `{ c = Carol, d = David }` would be ignored.</span></span> <span data-ttu-id="a1735-335">En este caso ya no tenemos un valor para `d` y se producirá un error en la generación de direcciones URL.</span><span class="sxs-lookup"><span data-stu-id="a1735-335">In this case we no longer have a value for `d` and URL generation will fail.</span></span> <span data-ttu-id="a1735-336">Debe especificar el valor deseado de `c` y `d`.</span><span class="sxs-lookup"><span data-stu-id="a1735-336">You would need to specify the desired value of `c` and `d`.</span></span>  <span data-ttu-id="a1735-337">Es probable que este problema se produzca con la ruta predeterminada (`{controller}/{action}/{id?}`), pero raramente encontrará este comportamiento en la práctica, dado que `Url.Action` especificará siempre de manera explícita un valor `controller` y `action`.</span><span class="sxs-lookup"><span data-stu-id="a1735-337">You might expect to hit this problem with the default route (`{controller}/{action}/{id?}`) - but you will rarely encounter this behavior in practice as `Url.Action` will always explicitly specify a `controller` and `action` value.</span></span>

<span data-ttu-id="a1735-338">Las sobrecargas más largas de `Url.Action` también toman un objeto de *valores de ruta* adicional para proporcionar valores para parámetros de ruta distintos de `controller` y `action`.</span><span class="sxs-lookup"><span data-stu-id="a1735-338">Longer overloads of `Url.Action` also take an additional *route values* object to provide values for route parameters other than `controller` and `action`.</span></span> <span data-ttu-id="a1735-339">Normalmente verá esto utilizado con `id`, como `Url.Action("Buy", "Products", new { id = 17 })`.</span><span class="sxs-lookup"><span data-stu-id="a1735-339">You will most commonly see this used with `id` like `Url.Action("Buy", "Products", new { id = 17 })`.</span></span> <span data-ttu-id="a1735-340">Por convención, el objeto de *valores de ruta* normalmente es un objeto de tipo anónimo, pero también puede ser `IDictionary<>` o un *objeto .NET estándar*.</span><span class="sxs-lookup"><span data-stu-id="a1735-340">By convention the *route values* object is usually an object of anonymous type, but it can also be an `IDictionary<>` or a *plain old .NET object*.</span></span> <span data-ttu-id="a1735-341">Los valores de ruta adicionales que no coinciden con los parámetros de ruta se colocan en la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="a1735-341">Any additional route values that don't match route parameters are put in the query string.</span></span>

[!code-csharp[](routing/sample/main/Controllers/TestController.cs)]

> [!TIP]
> <span data-ttu-id="a1735-342">Para crear una dirección URL absoluta, use una sobrecarga que acepte `protocol`: `Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span><span class="sxs-lookup"><span data-stu-id="a1735-342">To create an absolute URL, use an overload that accepts a `protocol`: `Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span></span>

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generating-urls-by-route"></a><span data-ttu-id="a1735-343">Generación de direcciones URL por ruta</span><span class="sxs-lookup"><span data-stu-id="a1735-343">Generating URLs by route</span></span>

<span data-ttu-id="a1735-344">En el código anterior se pasó el nombre de acción y de controlador para generar una dirección URL.</span><span class="sxs-lookup"><span data-stu-id="a1735-344">The code above demonstrated generating a URL by passing in the controller and action name.</span></span> <span data-ttu-id="a1735-345">`IUrlHelper` también proporciona la familia de métodos `Url.RouteUrl`.</span><span class="sxs-lookup"><span data-stu-id="a1735-345">`IUrlHelper` also provides the `Url.RouteUrl` family of methods.</span></span> <span data-ttu-id="a1735-346">Estos métodos son similares a `Url.Action`, pero no copian los valores actuales de `action` y `controller` en los valores de ruta.</span><span class="sxs-lookup"><span data-stu-id="a1735-346">These methods are similar to `Url.Action`, but they don't copy the current values of `action` and `controller` to the route values.</span></span> <span data-ttu-id="a1735-347">Lo más común es especificar un nombre de ruta para utilizar una ruta específica y generar la dirección URL, por lo general *sin* especificar un nombre de acción o controlador.</span><span class="sxs-lookup"><span data-stu-id="a1735-347">The most common usage is to specify a route name to use a specific route to generate the URL, generally *without* specifying a controller or action name.</span></span>

[!code-csharp[](routing/sample/main/Controllers/UrlGenerationControllerRouting.cs?name=snippet_1)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generating-urls-in-html"></a><span data-ttu-id="a1735-348">Generación de direcciones URL en HTML</span><span class="sxs-lookup"><span data-stu-id="a1735-348">Generating URLs in HTML</span></span>

<span data-ttu-id="a1735-349">`IHtmlHelper` proporciona los métodos de `HtmlHelper` `Html.BeginForm` y `Html.ActionLink` para generar elementos `<form>` y `<a>`, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="a1735-349">`IHtmlHelper` provides the `HtmlHelper` methods `Html.BeginForm` and `Html.ActionLink` to generate `<form>` and `<a>` elements respectively.</span></span> <span data-ttu-id="a1735-350">Estos métodos utilizan el método `Url.Action` para generar una dirección URL y aceptan argumentos similares.</span><span class="sxs-lookup"><span data-stu-id="a1735-350">These methods use the `Url.Action` method to generate a URL and they accept similar arguments.</span></span> <span data-ttu-id="a1735-351">Los métodos `Url.RouteUrl` complementarios de `HtmlHelper` son `Html.BeginRouteForm` y `Html.RouteLink`, cuya funcionalidad es similar.</span><span class="sxs-lookup"><span data-stu-id="a1735-351">The `Url.RouteUrl` companions for `HtmlHelper` are `Html.BeginRouteForm` and `Html.RouteLink` which have similar functionality.</span></span>

<span data-ttu-id="a1735-352">Las TagHelper generan direcciones URL a través de la TagHelper `form` y la TagHelper `<a>`.</span><span class="sxs-lookup"><span data-stu-id="a1735-352">TagHelpers generate URLs through the `form` TagHelper and the `<a>` TagHelper.</span></span> <span data-ttu-id="a1735-353">Ambos usan `IUrlHelper` para su implementación.</span><span class="sxs-lookup"><span data-stu-id="a1735-353">Both of these use `IUrlHelper` for their implementation.</span></span> <span data-ttu-id="a1735-354">Consulte [Trabajar con formularios](../views/working-with-forms.md) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="a1735-354">See [Working with Forms](../views/working-with-forms.md) for more information.</span></span>

<span data-ttu-id="a1735-355">Dentro de las vistas, `IUrlHelper` está disponible a través de la propiedad `Url` para una generación de direcciones URL ad hoc no cubierta por los pasos anteriores.</span><span class="sxs-lookup"><span data-stu-id="a1735-355">Inside views, the `IUrlHelper` is available through the `Url` property for any ad-hoc URL generation not covered by the above.</span></span>

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="generating-urls-in-action-results"></a><span data-ttu-id="a1735-356">Generar direcciones URL en los resultados de acción</span><span class="sxs-lookup"><span data-stu-id="a1735-356">Generating URLS in Action Results</span></span>

<span data-ttu-id="a1735-357">Los ejemplos anteriores han demostrado el uso de `IUrlHelper` en un controlador, aunque el uso más común de un controlador consiste en generar una dirección URL como parte de un resultado de acción.</span><span class="sxs-lookup"><span data-stu-id="a1735-357">The examples above have shown using `IUrlHelper` in a controller, while the most common usage in a controller is to generate a URL as part of an action result.</span></span>

<span data-ttu-id="a1735-358">Las clases base `ControllerBase` y `Controller` proporcionan métodos de conveniencia para los resultados de acción que hacen referencia a otra acción.</span><span class="sxs-lookup"><span data-stu-id="a1735-358">The `ControllerBase` and `Controller` base classes provide convenience methods for action results that reference another action.</span></span> <span data-ttu-id="a1735-359">Un uso típico es redirigir después de aceptar la entrada del usuario.</span><span class="sxs-lookup"><span data-stu-id="a1735-359">One typical usage is to redirect after accepting user input.</span></span>

```csharp
public IActionResult Edit(int id, Customer customer)
{
    if (ModelState.IsValid)
    {
        // Update DB with new details.
        return RedirectToAction("Index");
    }
    return View(customer);
}
```

<span data-ttu-id="a1735-360">Los patrones de diseño Factory Method de resultados de acción siguen un patrón similar a los métodos en `IUrlHelper`.</span><span class="sxs-lookup"><span data-stu-id="a1735-360">The action results factory methods follow a similar pattern to the methods on `IUrlHelper`.</span></span>

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a><span data-ttu-id="a1735-361">Caso especial para rutas convencionales dedicadas</span><span class="sxs-lookup"><span data-stu-id="a1735-361">Special case for dedicated conventional routes</span></span>

<span data-ttu-id="a1735-362">El enrutamiento convencional puede usar un tipo especial de definición de ruta denominada *ruta convencional dedicada*.</span><span class="sxs-lookup"><span data-stu-id="a1735-362">Conventional routing can use a special kind of route definition called a *dedicated conventional route*.</span></span> <span data-ttu-id="a1735-363">En el ejemplo siguiente, la ruta llamada `blog` es una ruta convencional dedicada.</span><span class="sxs-lookup"><span data-stu-id="a1735-363">In the example below, the route named `blog` is a dedicated conventional route.</span></span>

```csharp
app.UseMvc(routes =>
{
    routes.MapRoute("blog", "blog/{*article}",
        defaults: new { controller = "Blog", action = "Article" });
    routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="a1735-364">Con estas definiciones de ruta, `Url.Action("Index", "Home")` generará la ruta de dirección URL `/` con la ruta `default`. Pero, ¿por qué?</span><span class="sxs-lookup"><span data-stu-id="a1735-364">Using these route definitions, `Url.Action("Index", "Home")` will generate the URL path `/` with the `default` route, but why?</span></span> <span data-ttu-id="a1735-365">Se puede suponer que los valores de ruta `{ controller = Home, action = Index }` son suficientes para generar una dirección URL utilizando `blog`, con el resultado `/blog?action=Index&controller=Home`.</span><span class="sxs-lookup"><span data-stu-id="a1735-365">You might guess the route values `{ controller = Home, action = Index }` would be enough to generate a URL using `blog`, and the result would be `/blog?action=Index&controller=Home`.</span></span>

<span data-ttu-id="a1735-366">Las rutas convencionales dedicadas se basan en un comportamiento especial de los valores predeterminados que no tienen un parámetro de ruta correspondiente que impida que la ruta sea "demasiado expansiva" con la generación de direcciones URL.</span><span class="sxs-lookup"><span data-stu-id="a1735-366">Dedicated conventional routes rely on a special behavior of default values that don't have a corresponding route parameter that prevents the route from being "too greedy" with URL generation.</span></span> <span data-ttu-id="a1735-367">En este caso, los valores predeterminados son `{ controller = Blog, action = Article }`, y ni `controller` ni `action` aparecen como un parámetro de ruta.</span><span class="sxs-lookup"><span data-stu-id="a1735-367">In this case the default values are `{ controller = Blog, action = Article }`, and neither `controller` nor `action` appears as a route parameter.</span></span> <span data-ttu-id="a1735-368">Cuando el enrutamiento realiza la generación de direcciones URL, los valores proporcionados deben coincidir con los valores predeterminados.</span><span class="sxs-lookup"><span data-stu-id="a1735-368">When routing performs URL generation, the values provided must match the default values.</span></span> <span data-ttu-id="a1735-369">La generación de direcciones URL con `blog` producirá un error porque los valores `{ controller = Home, action = Index }` no coinciden con `{ controller = Blog, action = Article }`.</span><span class="sxs-lookup"><span data-stu-id="a1735-369">URL generation using `blog` will fail because the values `{ controller = Home, action = Index }` don't match `{ controller = Blog, action = Article }`.</span></span> <span data-ttu-id="a1735-370">Después, el enrutamiento vuelve para probar `default`, operación que se realiza correctamente.</span><span class="sxs-lookup"><span data-stu-id="a1735-370">Routing then falls back to try `default`, which succeeds.</span></span>

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a><span data-ttu-id="a1735-371">Áreas</span><span class="sxs-lookup"><span data-stu-id="a1735-371">Areas</span></span>

<span data-ttu-id="a1735-372">Las [áreas](areas.md) son una característica de MVC que se usa para organizar funciones relacionadas en un grupo como un espacio de nombres de enrutamiento independiente (para acciones de controlador) y una estructura de carpetas (para las vistas).</span><span class="sxs-lookup"><span data-stu-id="a1735-372">[Areas](areas.md) are an MVC feature used to organize related functionality into a group as a separate routing-namespace (for controller actions) and folder structure (for views).</span></span> <span data-ttu-id="a1735-373">La utilización de áreas permite que una aplicación tenga varios controladores con el mismo nombre, siempre y cuando tengan *áreas* diferentes.</span><span class="sxs-lookup"><span data-stu-id="a1735-373">Using areas allows an application to have multiple controllers with the same name - as long as they have different *areas*.</span></span> <span data-ttu-id="a1735-374">El uso de áreas crea una jerarquía para el enrutamiento mediante la adición de otro parámetro de ruta, `area`, a `controller` y `action`.</span><span class="sxs-lookup"><span data-stu-id="a1735-374">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area` to `controller` and `action`.</span></span> <span data-ttu-id="a1735-375">En esta sección veremos la interacción entre el enrutamiento y las áreas. Consulte [Áreas](areas.md) para obtener más información sobre cómo se utilizan las áreas con las vistas.</span><span class="sxs-lookup"><span data-stu-id="a1735-375">This section will discuss how routing interacts with areas - see [Areas](areas.md) for details about how areas are used with views.</span></span>

<span data-ttu-id="a1735-376">En el ejemplo siguiente se configura MVC para usar la ruta predeterminada convencional y una *ruta de área* para un área denominada `Blog`:</span><span class="sxs-lookup"><span data-stu-id="a1735-376">The following example configures MVC to use the default conventional route and an *area route* for an area named `Blog`:</span></span>

[!code-csharp[](routing/sample/AreasRouting/Startup.cs?name=snippet1)]

<span data-ttu-id="a1735-377">Cuando coincide con una ruta de dirección URL como `/Manage/Users/AddUser`, la primera ruta generará los valores de ruta `{ area = Blog, controller = Users, action = AddUser }`.</span><span class="sxs-lookup"><span data-stu-id="a1735-377">When matching a URL path like `/Manage/Users/AddUser`, the first route will produce the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span> <span data-ttu-id="a1735-378">El valor de ruta `area` se genera con un valor predeterminado para `area`. De hecho, la ruta creada por `MapAreaRoute` es equivalente a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="a1735-378">The `area` route value is produced by a default value for `area`, in fact the route created by `MapAreaRoute` is equivalent to the following:</span></span>

[!code-csharp[](routing/sample/AreasRouting/Startup.cs?name=snippet2)]

<span data-ttu-id="a1735-379">`MapAreaRoute` utiliza el nombre de área proporcionado, que en este caso es `Blog`, para crear una ruta con un valor predeterminado y una restricción para `area`.</span><span class="sxs-lookup"><span data-stu-id="a1735-379">`MapAreaRoute` creates a route using both a default value and constraint for `area` using the provided area name, in this case `Blog`.</span></span> <span data-ttu-id="a1735-380">El valor predeterminado garantiza que la ruta siempre produce `{ area = Blog, ... }`; la restricción requiere el valor `{ area = Blog, ... }` para la generación de la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="a1735-380">The default value ensures that the route always produces `{ area = Blog, ... }`, the constraint requires the value `{ area = Blog, ... }` for URL generation.</span></span>

> [!TIP]
> <span data-ttu-id="a1735-381">El enrutamiento convencional depende del orden.</span><span class="sxs-lookup"><span data-stu-id="a1735-381">Conventional routing is order-dependent.</span></span> <span data-ttu-id="a1735-382">En general, las rutas con áreas deben colocarse antes en la tabla de rutas, ya que son más específicas que las rutas sin un área.</span><span class="sxs-lookup"><span data-stu-id="a1735-382">In general, routes with areas should be placed earlier in the route table as they're more specific than routes without an area.</span></span>

<span data-ttu-id="a1735-383">En el ejemplo anterior, los valores de ruta coincidirían con la acción siguiente:</span><span class="sxs-lookup"><span data-stu-id="a1735-383">Using the above example, the route values would match the following action:</span></span>

[!code-csharp[](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

<span data-ttu-id="a1735-384">`AreaAttribute` es lo que denota un controlador como parte de un área. Se dice que este controlador está en el área `Blog`.</span><span class="sxs-lookup"><span data-stu-id="a1735-384">The `AreaAttribute` is what denotes a controller as part of an area, we say that this controller is in the `Blog` area.</span></span> <span data-ttu-id="a1735-385">Los controladores sin un atributo `[Area]` no son miembros de ningún área y **no** coincidirán cuando el enrutamiento proporcione el valor de ruta `area`.</span><span class="sxs-lookup"><span data-stu-id="a1735-385">Controllers without an `[Area]` attribute are not members of any area, and will **not** match when the `area` route value is provided by routing.</span></span> <span data-ttu-id="a1735-386">En el ejemplo siguiente, solo el primer controlador enumerado puede coincidir con los valores de ruta `{ area = Blog, controller = Users, action = AddUser }`.</span><span class="sxs-lookup"><span data-stu-id="a1735-386">In the following example, only the first controller listed can match the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span>

[!code-csharp[](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[](routing/sample/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[](routing/sample/AreasRouting/Controllers/UsersController.cs)]

> [!NOTE]
> <span data-ttu-id="a1735-387">En aras de la exhaustividad, aquí se muestra el espacio de nombres de cada controlador; en caso contrario, los controladores tendrían un conflicto de nomenclatura y generarían un error del compilador.</span><span class="sxs-lookup"><span data-stu-id="a1735-387">The namespace of each controller is shown here for completeness - otherwise the controllers would have a naming conflict and generate a compiler error.</span></span> <span data-ttu-id="a1735-388">Los espacios de nombres de clase no tienen ningún efecto en el enrutamiento de MVC.</span><span class="sxs-lookup"><span data-stu-id="a1735-388">Class namespaces have no effect on MVC's routing.</span></span>

<span data-ttu-id="a1735-389">Los dos primeros controladores son miembros de las áreas y solo coinciden cuando el valor de ruta `area` proporciona su respectivo nombre de área.</span><span class="sxs-lookup"><span data-stu-id="a1735-389">The first two controllers are members of areas, and only match when their respective area name is provided by the `area` route value.</span></span> <span data-ttu-id="a1735-390">El tercer controlador no es miembro de ningún área y solo puede coincidir cuando el enrutamiento no proporciona ningún valor para `area`.</span><span class="sxs-lookup"><span data-stu-id="a1735-390">The third controller isn't a member of any area, and can only match when no value for `area` is provided by routing.</span></span>

> [!NOTE]
> <span data-ttu-id="a1735-391">En términos de búsqueda de coincidencias de *ningún valor*, la ausencia del valor `area` es igual que si el valor de `area` fuese null o una cadena vacía.</span><span class="sxs-lookup"><span data-stu-id="a1735-391">In terms of matching *no value*, the absence of the `area` value is the same as if the value for `area` were null or the empty string.</span></span>

<span data-ttu-id="a1735-392">Al ejecutar una acción en un área, el valor de ruta para `area` estará disponible como un *valor de ambiente* para que el enrutamiento pueda usarlo en la generación de direcciones URL.</span><span class="sxs-lookup"><span data-stu-id="a1735-392">When executing an action inside an area, the route value for `area` will be available as an *ambient value* for routing to use for URL generation.</span></span> <span data-ttu-id="a1735-393">Esto significa que, de forma predeterminada, las áreas actúan de forma *adhesiva* para la generación de direcciones URL, tal como se muestra en el ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="a1735-393">This means that by default areas act *sticky* for URL generation as demonstrated by the following sample.</span></span>

[!code-csharp[](routing/sample/AreasRouting/Startup.cs?name=snippet3)]

[!code-csharp[](routing/sample/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<a name="iactionconstraint-ref-label"></a>

## <a name="understanding-iactionconstraint"></a><span data-ttu-id="a1735-394">Descripción de IActionConstraint</span><span class="sxs-lookup"><span data-stu-id="a1735-394">Understanding IActionConstraint</span></span>

> [!NOTE]
> <span data-ttu-id="a1735-395">En esta sección se analiza con mayor profundidad el funcionamiento interno del marco y la elección por parte de MVC de la acción que se va a ejecutar.</span><span class="sxs-lookup"><span data-stu-id="a1735-395">This section is a deep-dive on framework internals and how MVC chooses an action to execute.</span></span> <span data-ttu-id="a1735-396">Una aplicación típica no necesitará un `IActionConstraint` personalizado.</span><span class="sxs-lookup"><span data-stu-id="a1735-396">A typical application won't need a custom `IActionConstraint`</span></span>

<span data-ttu-id="a1735-397">Es probable que ya haya usado `IActionConstraint` incluso si no está familiarizado con la interfaz.</span><span class="sxs-lookup"><span data-stu-id="a1735-397">You have likely already used `IActionConstraint` even if you're not familiar with the interface.</span></span> <span data-ttu-id="a1735-398">El atributo `[HttpGet]` y los atributos `[Http-VERB]` similares implementan `IActionConstraint` con el fin de limitar la ejecución de un método de acción.</span><span class="sxs-lookup"><span data-stu-id="a1735-398">The `[HttpGet]` Attribute and similar `[Http-VERB]` attributes implement `IActionConstraint` in order to limit the execution of an action method.</span></span>

```csharp
public class ProductsController : Controller
{
    [HttpGet]
    public IActionResult Edit() { }

    public IActionResult Edit(...) { }
}
```

<span data-ttu-id="a1735-399">Adoptando la ruta convencional predeterminada, la ruta de dirección URL `/Products/Edit` generaría los valores `{ controller = Products, action = Edit }`, que coincidirían con **ambas** acciones que se muestran aquí.</span><span class="sxs-lookup"><span data-stu-id="a1735-399">Assuming the default conventional route, the URL path `/Products/Edit` would produce the values `{ controller = Products, action = Edit }`, which would match **both** of the actions shown here.</span></span> <span data-ttu-id="a1735-400">En terminología de `IActionConstraint`, diríamos que ambas acciones se consideran candidatas, puesto que las dos coinciden con los datos de ruta.</span><span class="sxs-lookup"><span data-stu-id="a1735-400">In `IActionConstraint` terminology we would say that both of these actions are considered candidates - as they both match the route data.</span></span>

<span data-ttu-id="a1735-401">Cuando `HttpGetAttribute` se ejecute, dirá que *Edit()* es una coincidencia para *GET*, pero no para cualquier otro verbo HTTP.</span><span class="sxs-lookup"><span data-stu-id="a1735-401">When the `HttpGetAttribute` executes, it will say that *Edit()* is a match for *GET* and isn't a match for any other HTTP verb.</span></span> <span data-ttu-id="a1735-402">La acción `Edit(...)` no tiene ninguna restricción definida, por lo que coincidirá con cualquier verbo HTTP.</span><span class="sxs-lookup"><span data-stu-id="a1735-402">The `Edit(...)` action doesn't have any constraints defined, and so will match any HTTP verb.</span></span> <span data-ttu-id="a1735-403">Con `POST`, solamente `Edit(...)` coincide.</span><span class="sxs-lookup"><span data-stu-id="a1735-403">So assuming a `POST` - only `Edit(...)` matches.</span></span> <span data-ttu-id="a1735-404">Pero con `GET` ambas acciones pueden coincidir. No obstante, una acción con `IActionConstraint` siempre se considera *mejor* que una acción sin dicha restricción.</span><span class="sxs-lookup"><span data-stu-id="a1735-404">But, for a `GET` both actions can still match - however, an action with an `IActionConstraint` is always considered *better* than an action without.</span></span> <span data-ttu-id="a1735-405">Por tanto, como `Edit()` tiene `[HttpGet]`, se considera más específica y se seleccionará si ambas acciones pueden coincidir.</span><span class="sxs-lookup"><span data-stu-id="a1735-405">So because `Edit()` has `[HttpGet]` it's considered more specific, and will be selected if both actions can match.</span></span>

<span data-ttu-id="a1735-406">Conceptualmente, `IActionConstraint` es una forma de *sobrecarga*, pero en lugar de sobrecargar métodos con el mismo nombre, la sobrecarga se produce entre acciones que coinciden con la misma dirección URL.</span><span class="sxs-lookup"><span data-stu-id="a1735-406">Conceptually, `IActionConstraint` is a form of *overloading*, but instead of overloading methods with the same name, it's overloading between actions that match the same URL.</span></span> <span data-ttu-id="a1735-407">El enrutamiento mediante atributos también utiliza `IActionConstraint` y puede dar lugar a que acciones de diferentes controladores se consideren candidatas.</span><span class="sxs-lookup"><span data-stu-id="a1735-407">Attribute routing also uses `IActionConstraint` and can result in actions from different controllers both being considered candidates.</span></span>

<a name="iactionconstraint-impl-ref-label"></a>

### <a name="implementing-iactionconstraint"></a><span data-ttu-id="a1735-408">Implementación de IActionConstraint</span><span class="sxs-lookup"><span data-stu-id="a1735-408">Implementing IActionConstraint</span></span>

<span data-ttu-id="a1735-409">La manera más sencilla de implementar `IActionConstraint` consiste en crear una clase derivada de `System.Attribute` y colocarla en las acciones y los controladores.</span><span class="sxs-lookup"><span data-stu-id="a1735-409">The simplest way to implement an `IActionConstraint` is to create a class derived from `System.Attribute` and place it on your actions and controllers.</span></span> <span data-ttu-id="a1735-410">MVC detectará automáticamente cualquier `IActionConstraint` que se aplique como atributo.</span><span class="sxs-lookup"><span data-stu-id="a1735-410">MVC will automatically discover any `IActionConstraint` that are applied as attributes.</span></span> <span data-ttu-id="a1735-411">El modelo de aplicaciones es quizá el enfoque más flexible para la aplicación de restricciones, puesto que permite metaprogramar cómo se aplican.</span><span class="sxs-lookup"><span data-stu-id="a1735-411">You can use the application model to apply constraints, and this is probably the most flexible approach as it allows you to metaprogram how they're applied.</span></span>

<span data-ttu-id="a1735-412">En el ejemplo siguiente, una restricción elige una acción según un *código de país* de los datos de ruta.</span><span class="sxs-lookup"><span data-stu-id="a1735-412">In the following example a constraint chooses an action based on a *country code* from the route data.</span></span> <span data-ttu-id="a1735-413">El [ejemplo completo se encuentra en GitHub](https://github.com/aspnet/Entropy/blob/master/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).</span><span class="sxs-lookup"><span data-stu-id="a1735-413">The [full sample on GitHub](https://github.com/aspnet/Entropy/blob/master/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).</span></span>

```csharp
public class CountrySpecificAttribute : Attribute, IActionConstraint
{
    private readonly string _countryCode;

    public CountrySpecificAttribute(string countryCode)
    {
        _countryCode = countryCode;
    }

    public int Order
    {
        get
        {
            return 0;
        }
    }

    public bool Accept(ActionConstraintContext context)
    {
        return string.Equals(
            context.RouteContext.RouteData.Values["country"].ToString(),
            _countryCode,
            StringComparison.OrdinalIgnoreCase);
    }
}
```

<span data-ttu-id="a1735-414">Usted es responsable de implementar el método `Accept` y elegir un valor para "Order" para que la restricción se ejecute.</span><span class="sxs-lookup"><span data-stu-id="a1735-414">You are responsible for implementing the `Accept` method and choosing an 'Order' for the constraint to execute.</span></span> <span data-ttu-id="a1735-415">En este caso, el método `Accept` devuelve `true` para denotar que la acción es una coincidencia cuando el valor de ruta `country` coincide.</span><span class="sxs-lookup"><span data-stu-id="a1735-415">In this case, the `Accept` method returns `true` to denote the action is a match when the `country` route value matches.</span></span> <span data-ttu-id="a1735-416">Esto difiere de `RouteValueAttribute` en que permite volver a una acción sin atributos.</span><span class="sxs-lookup"><span data-stu-id="a1735-416">This is different from a `RouteValueAttribute` in that it allows fallback to a non-attributed action.</span></span> <span data-ttu-id="a1735-417">El ejemplo muestra que si se define una acción `en-US`, entonces un código de país como `fr-FR` recurriría a un controlador más genérico que no tenga `[CountrySpecific(...)]` aplicado.</span><span class="sxs-lookup"><span data-stu-id="a1735-417">The sample shows that if you define an `en-US` action then a country code like `fr-FR` will fall back to a more generic controller that doesn't have `[CountrySpecific(...)]` applied.</span></span>

<span data-ttu-id="a1735-418">La propiedad `Order` decide de qué *fase* forma parte la restricción.</span><span class="sxs-lookup"><span data-stu-id="a1735-418">The `Order` property decides which *stage* the constraint is part of.</span></span> <span data-ttu-id="a1735-419">Restricciones de acciones que se ejecutan en grupos basados en la `Order`.</span><span class="sxs-lookup"><span data-stu-id="a1735-419">Action constraints run in groups based on the `Order`.</span></span> <span data-ttu-id="a1735-420">Por ejemplo, todos los atributos del método HTTP proporcionados por el marco utilizan el mismo valor `Order` para que se ejecuten en la misma fase.</span><span class="sxs-lookup"><span data-stu-id="a1735-420">For example, all of the framework provided HTTP method attributes use the same `Order` value so that they run in the same stage.</span></span> <span data-ttu-id="a1735-421">Puede tener las fases que sean necesarias para implementar las directivas deseadas.</span><span class="sxs-lookup"><span data-stu-id="a1735-421">You can have as many stages as you need to implement your desired policies.</span></span>

> [!TIP]
> <span data-ttu-id="a1735-422">Para decidir el valor de `Order`, piense si la restricción se debería o no aplicar antes que los métodos HTTP.</span><span class="sxs-lookup"><span data-stu-id="a1735-422">To decide on a value for `Order` think about whether or not your constraint should be applied before HTTP methods.</span></span> <span data-ttu-id="a1735-423">Los números más bajos se ejecutan primero.</span><span class="sxs-lookup"><span data-stu-id="a1735-423">Lower numbers run first.</span></span>
