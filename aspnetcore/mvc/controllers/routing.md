---
title: El enrutamiento a las acciones de controlador
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 03/14/2017
ms.topic: article
ms.assetid: 26250a4d-bf62-4d45-8549-26801cf956e9
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/routing
ms.openlocfilehash: d6e230351eb2f4c8549b54d75fd8e345718e6109
ms.sourcegitcommit: 9ecd4e9fb0c40c3693dab079eab1ff94b461c922
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2017
---
# <a name="routing-to-controller-actions"></a><span data-ttu-id="d0d3f-103">El enrutamiento a las acciones de controlador</span><span class="sxs-lookup"><span data-stu-id="d0d3f-103">Routing to Controller Actions</span></span>

<span data-ttu-id="d0d3f-104">Por [Ryan Nowak](https://github.com/rynowak) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d0d3f-104">By [Ryan Nowak](https://github.com/rynowak) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d0d3f-105">Núcleo de ASP.NET MVC utiliza el enrutamiento [middleware](../../fundamentals/middleware.md) que coincida con las direcciones URL de las solicitudes entrantes y asignarlos a las acciones.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-105">ASP.NET Core MVC uses the Routing [middleware](../../fundamentals/middleware.md) to match the URLs of incoming requests and map them to actions.</span></span> <span data-ttu-id="d0d3f-106">Las rutas se definen en el código de inicio o atributos.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-106">Routes are defined in startup code or attributes.</span></span> <span data-ttu-id="d0d3f-107">Las rutas describen cómo se deberían buscar rutas de acceso de dirección URL a las acciones.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-107">Routes describe how URL paths should be matched to actions.</span></span> <span data-ttu-id="d0d3f-108">Rutas también se usan para generar direcciones URL (para vínculos) enviadas en las respuestas.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-108">Routes are also used to generate URLs (for links) sent out in responses.</span></span> 

<span data-ttu-id="d0d3f-109">Acciones o se enrutan convencional o enruta de atributo.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-109">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="d0d3f-110">Genera una ruta en el controlador o la acción hace que se enrutan de atributo.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-110">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="d0d3f-111">Vea [mixto enrutamiento](#routing-mixed-ref-label) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-111">See [Mixed routing](#routing-mixed-ref-label) for more information.</span></span>

<span data-ttu-id="d0d3f-112">Este documento explica las interacciones entre MVC y enrutamiento y asegúrese de aplicaciones MVC típica cómo el uso de características de enrutamiento.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-112">This document will explain the interactions between MVC and routing, and how typical MVC apps make use of routing features.</span></span> <span data-ttu-id="d0d3f-113">Vea [enrutamiento](xref:fundamentals/routing) para obtener más información acerca del enrutamiento avanzada.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-113">See [Routing](xref:fundamentals/routing) for details on advanced routing.</span></span>

## <a name="setting-up-routing-middleware"></a><span data-ttu-id="d0d3f-114">Configurar el enrutamiento de Middleware</span><span class="sxs-lookup"><span data-stu-id="d0d3f-114">Setting up Routing Middleware</span></span>

<span data-ttu-id="d0d3f-115">En su *configurar* método puede ver código similar al:</span><span class="sxs-lookup"><span data-stu-id="d0d3f-115">In your *Configure* method you may see code similar to:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="d0d3f-116">Dentro de la llamada a `UseMvc`, `MapRoute` se utiliza para crear una ruta única, que nos referiremos a como el `default` ruta.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-116">Inside the call to `UseMvc`, `MapRoute` is used to create a single route, which we'll refer to as the `default` route.</span></span> <span data-ttu-id="d0d3f-117">Mayoría de las aplicaciones MVC usará una ruta con una plantilla similar a la `default` ruta.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-117">Most MVC apps will use a route with a template similar to the `default` route.</span></span>

<span data-ttu-id="d0d3f-118">La plantilla de ruta `"{controller=Home}/{action=Index}/{id?}"` puede coincidir con una ruta de acceso de dirección URL como `/Products/Details/5` y va a extraer los valores de ruta `{ controller = Products, action = Details, id = 5 }` mediante encadenamiento de la ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-118">The route template `"{controller=Home}/{action=Index}/{id?}"` can match a URL path like `/Products/Details/5` and will extract the route values `{ controller = Products, action = Details, id = 5 }` by tokenizing the path.</span></span> <span data-ttu-id="d0d3f-119">MVC intentará encontrar un controlador denominado `ProductsController` y ejecutar la acción `Details`:</span><span class="sxs-lookup"><span data-stu-id="d0d3f-119">MVC will attempt to locate a controller named `ProductsController` and run the action `Details`:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Details(int id) { ... }
}
```

<span data-ttu-id="d0d3f-120">Tenga en cuenta que en este ejemplo, el enlace de modelos usarían el valor de `id = 5` para establecer el `id` parámetro `5` al invocar esta acción.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-120">Note that in this example, model binding would use the value of `id = 5` to set the `id` parameter to `5` when invoking this action.</span></span> <span data-ttu-id="d0d3f-121">Consulte la [enlace de modelos](../models/model-binding.md) para obtener más detalles.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-121">See the [Model Binding](../models/model-binding.md) for more details.</span></span>

<span data-ttu-id="d0d3f-122">Mediante el `default` ruta:</span><span class="sxs-lookup"><span data-stu-id="d0d3f-122">Using the `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="d0d3f-123">La plantilla de ruta:</span><span class="sxs-lookup"><span data-stu-id="d0d3f-123">The route template:</span></span>

* <span data-ttu-id="d0d3f-124">`{controller=Home}`define `Home` como valor predeterminado`controller`</span><span class="sxs-lookup"><span data-stu-id="d0d3f-124">`{controller=Home}` defines `Home` as the default `controller`</span></span>

* <span data-ttu-id="d0d3f-125">`{action=Index}`define `Index` como valor predeterminado`action`</span><span class="sxs-lookup"><span data-stu-id="d0d3f-125">`{action=Index}` defines `Index` as the default `action`</span></span>

* <span data-ttu-id="d0d3f-126">`{id?}`define `id` como opcionales</span><span class="sxs-lookup"><span data-stu-id="d0d3f-126">`{id?}` defines `id` as optional</span></span>

<span data-ttu-id="d0d3f-127">De forma predeterminada y los parámetros de ruta opcional no necesita estar presente en la ruta de acceso de dirección URL para una coincidencia.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-127">Default and optional route parameters do not need to be present in the URL path for a match.</span></span> <span data-ttu-id="d0d3f-128">Vea [referencia de plantilla de ruta](../../fundamentals/routing.md#route-template-reference) para obtener una descripción detallada de la sintaxis de la plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-128">See [Route Template Reference](../../fundamentals/routing.md#route-template-reference) for a detailed description of route template syntax.</span></span>

<span data-ttu-id="d0d3f-129">`"{controller=Home}/{action=Index}/{id?}"`puede coincidir con la ruta de acceso de dirección URL `/` y generará los valores de ruta `{ controller = Home, action = Index }`.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-129">`"{controller=Home}/{action=Index}/{id?}"` can match the URL path `/` and will produce the route values `{ controller = Home, action = Index }`.</span></span> <span data-ttu-id="d0d3f-130">Los valores de `controller` y `action` hacer uso de los valores predeterminados, `id` no genera un valor porque no hay ningún segmento correspondiente en la ruta de acceso de dirección URL.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-130">The values for `controller` and `action` make use of the default values, `id` does not produce a value since there is no corresponding segment in the URL path.</span></span> <span data-ttu-id="d0d3f-131">MVC utilizaría estos valores de ruta para seleccionar la `HomeController` y `Index` acción:</span><span class="sxs-lookup"><span data-stu-id="d0d3f-131">MVC would use these route values to select the `HomeController` and `Index` action:</span></span>

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

<span data-ttu-id="d0d3f-132">Utilizar esta definición de controlador y la plantilla de ruta, el `HomeController.Index` acción se ejecutaría para cualquiera de las rutas de acceso de dirección URL siguientes:</span><span class="sxs-lookup"><span data-stu-id="d0d3f-132">Using this controller definition and route template, the `HomeController.Index` action would be executed for any of the following URL paths:</span></span>

* `/Home/Index/17`

* `/Home/Index`

* `/Home`

* `/`

<span data-ttu-id="d0d3f-133">El método conveniencia `UseMvcWithDefaultRoute`:</span><span class="sxs-lookup"><span data-stu-id="d0d3f-133">The convenience method `UseMvcWithDefaultRoute`:</span></span>

```csharp
app.UseMvcWithDefaultRoute();
```

<span data-ttu-id="d0d3f-134">Se puede usar para reemplazar:</span><span class="sxs-lookup"><span data-stu-id="d0d3f-134">Can be used to replace:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="d0d3f-135">`UseMvc`y `UseMvcWithDefaultRoute` agregar una instancia de `RouterMiddleware` a la canalización de middleware.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-135">`UseMvc` and `UseMvcWithDefaultRoute` add an instance of `RouterMiddleware` to the middleware pipeline.</span></span> <span data-ttu-id="d0d3f-136">MVC no interactúa directamente con middleware y usa el enrutamiento para controlar las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-136">MVC doesn't interact directly with middleware, and uses routing to handle requests.</span></span> <span data-ttu-id="d0d3f-137">MVC está conectado a las rutas a través de una instancia de `MvcRouteHandler`.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-137">MVC is connected to the routes through an instance of `MvcRouteHandler`.</span></span> <span data-ttu-id="d0d3f-138">El código dentro de `UseMvc` es similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="d0d3f-138">The code inside of `UseMvc` is similar to the following:</span></span>

```csharp
var routes = new RouteBuilder(app);

// Add connection to MVC, will be hooked up by calls to MapRoute.
routes.DefaultHandler = new MvcRouteHandler(...);

// Execute callback to register routes.
// routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");

// Create route collection and add the middleware.
app.UseRouter(routes.Build());
```

<span data-ttu-id="d0d3f-139">`UseMvc`no se define directamente ninguna ruta, agrega un marcador de posición a la colección de rutas para la `attribute` ruta.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-139">`UseMvc` does not directly define any routes, it adds a placeholder to the route collection for the `attribute` route.</span></span> <span data-ttu-id="d0d3f-140">La sobrecarga `UseMvc(Action<IRouteBuilder>)` le permite agregar sus propias rutas y también admite el enrutamiento del atributo.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-140">The overload `UseMvc(Action<IRouteBuilder>)` lets you add your own routes and also supports attribute routing.</span></span>  <span data-ttu-id="d0d3f-141">`UseMvc`y todas sus variaciones agrega un marcador de posición para la ruta de atributo - siempre está disponible, independientemente de cómo configurar el enrutamiento de atributo `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-141">`UseMvc` and all of its variations adds a placeholder for the attribute route - attribute routing is always available regardless of how you configure `UseMvc`.</span></span> <span data-ttu-id="d0d3f-142">`UseMvcWithDefaultRoute`define una ruta predeterminada y admite el enrutamiento del atributo.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-142">`UseMvcWithDefaultRoute` defines a default route and supports attribute routing.</span></span> <span data-ttu-id="d0d3f-143">El [atributo enrutamiento](#attribute-routing-ref-label) sección incluye más detalles acerca del enrutamiento de atributo.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-143">The [Attribute Routing](#attribute-routing-ref-label) section includes more details on attribute routing.</span></span>

<a name="routing-conventional-ref-label"></a>

## <a name="conventional-routing"></a><span data-ttu-id="d0d3f-144">El enrutamiento convencional</span><span class="sxs-lookup"><span data-stu-id="d0d3f-144">Conventional routing</span></span>

<span data-ttu-id="d0d3f-145">El `default` ruta:</span><span class="sxs-lookup"><span data-stu-id="d0d3f-145">The `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="d0d3f-146">es un ejemplo de un *enrutamiento convencional*.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-146">is an example of a *conventional routing*.</span></span> <span data-ttu-id="d0d3f-147">Llamamos a este estilo *enrutamiento convencional* porque establece un *convención* para rutas de acceso de dirección URL:</span><span class="sxs-lookup"><span data-stu-id="d0d3f-147">We call this style *conventional routing* because it establishes a *convention* for URL paths:</span></span>

* <span data-ttu-id="d0d3f-148">el primer segmento de ruta de acceso asignada al nombre de controlador</span><span class="sxs-lookup"><span data-stu-id="d0d3f-148">the first path segment maps to the controller name</span></span>

* <span data-ttu-id="d0d3f-149">la segunda se asigna al nombre de acción.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-149">the second maps to the action name.</span></span>

* <span data-ttu-id="d0d3f-150">el tercer segmento se utiliza para una función opcional `id` usa para asignar a un modelo de entidad</span><span class="sxs-lookup"><span data-stu-id="d0d3f-150">the third segment is used for an optional `id` used to map to a model entity</span></span>

<span data-ttu-id="d0d3f-151">Mediante este `default` ruta, la ruta de acceso de dirección URL `/Products/List` se asigna a la `ProductsController.List` acción, y `/Blog/Article/17` se asigna a `BlogController.Article`.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-151">Using this `default` route, the URL path `/Products/List` maps to the `ProductsController.List` action, and `/Blog/Article/17` maps to `BlogController.Article`.</span></span> <span data-ttu-id="d0d3f-152">Esta asignación se basa en los nombres de acción y controlador **sólo** y no se basa en espacios de nombres, ubicaciones de archivos de origen o los parámetros de método.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-152">This mapping is based on the controller and action names **only** and is not based on namespaces, source file locations, or method parameters.</span></span>

> [!TIP]
> <span data-ttu-id="d0d3f-153">Usar el enrutamiento convencional con la ruta predeterminada, podrá generar rápidamente la aplicación sin necesidad de tener acceso a un nuevo patrón de dirección URL para cada acción que se define.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-153">Using conventional routing with the default route allows you to build the application quickly without having to come up with a new URL pattern for each action you define.</span></span> <span data-ttu-id="d0d3f-154">Para una aplicación con acciones de estilo CRUD, contenedoras de coherencia para las direcciones URL a través de los controladores pueden ayudar a simplificar el código y hacer que la interfaz de usuario más predecibles.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-154">For an application with CRUD style actions, having consistency for the URLs across your controllers can help simplify your code and make your UI more predictable.</span></span>

> [!WARNING]
> <span data-ttu-id="d0d3f-155">El `id` se define como opcionales mediante la plantilla de ruta, lo que significa que las acciones se pueden ejecutar sin el identificador proporcionado como parte de la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-155">The `id` is defined as optional by the route template, meaning that your actions can execute without the ID provided as part of the URL.</span></span> <span data-ttu-id="d0d3f-156">Normalmente lo que ocurrirá si `id` se omite de la dirección URL es la que se establecerá como `0` por el enlace de modelos y, como resultado ninguna entidad se encuentra en la base de datos de la búsqueda de coincidencias `id == 0`.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-156">Usually what will happen if `id` is omitted from the URL is that it will be set to `0` by model binding, and as a result no entity will be found in the database matching `id == 0`.</span></span> <span data-ttu-id="d0d3f-157">Ruta de atributo puede proporcionar un control específico para realizar el Id. de necesarios para algunas acciones y otros no.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-157">Attribute routing can give you fine-grained control to make the ID required for some actions and not for others.</span></span> <span data-ttu-id="d0d3f-158">Por convención incluirá la documentación de los parámetros opcionales, como `id` cuando es probables que aparezcan en el uso correcto.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-158">By convention the documentation will include optional parameters like `id` when they are likely to appear in correct usage.</span></span>

## <a name="multiple-routes"></a><span data-ttu-id="d0d3f-159">Varias rutas</span><span class="sxs-lookup"><span data-stu-id="d0d3f-159">Multiple routes</span></span>

<span data-ttu-id="d0d3f-160">Puede agregar varias rutas dentro de `UseMvc` agregando más llamadas a `MapRoute`.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-160">You can add multiple routes inside `UseMvc` by adding more calls to `MapRoute`.</span></span> <span data-ttu-id="d0d3f-161">Si lo hace, le permite definir varias convenciones, o para agregar rutas convencionales que se dedican a una acción específica, como:</span><span class="sxs-lookup"><span data-stu-id="d0d3f-161">Doing so allows you to define multiple conventions, or to add conventional routes that are dedicated to a specific action, such as:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
            defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="d0d3f-162">El `blog` ruta aquí es un *ruta convencional dedicado*, lo que significa que utiliza el sistema de enrutamiento convencional, pero está dedicado a una acción específica.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-162">The `blog` route here is a *dedicated conventional route*, meaning that it uses the conventional routing system, but is dedicated to a specific action.</span></span> <span data-ttu-id="d0d3f-163">Puesto que `controller` y `action` no aparecen en la plantilla de ruta como parámetros, solo pueden tener los valores predeterminados y, por tanto, esta ruta siempre se asignará a la acción `BlogController.Article`.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-163">Since `controller` and `action` don't appear in the route template as parameters, they can only have the default values, and thus this route will always map to the action `BlogController.Article`.</span></span>

<span data-ttu-id="d0d3f-164">Rutas de la colección de rutas se ordenan y se procesarán en el orden en que se agregaron.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-164">Routes in the route collection are ordered, and will be processed in the order they are added.</span></span> <span data-ttu-id="d0d3f-165">Por lo que en este ejemplo, el `blog` ruta se intentará antes de la `default` ruta.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-165">So in this example, the `blog` route will be tried before the `default` route.</span></span>

> [!NOTE]
> <span data-ttu-id="d0d3f-166">*Dedicado rutas convencionales* suelen usar parámetros de ruta de catch-all como `{*article}` para capturar la parte restante de la ruta de acceso de dirección URL.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-166">*Dedicated conventional routes* often use catch-all route parameters like `{*article}` to capture the remaining portion of the URL path.</span></span> <span data-ttu-id="d0d3f-167">Esto puede ser una ruta de 'demasiado expansivo' lo que significa que coincide con las direcciones URL que se pretende ser elegidas por las otras rutas.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-167">This can make a route 'too greedy' meaning that it matches URLs that you intended to be matched by other routes.</span></span> <span data-ttu-id="d0d3f-168">Coloque las rutas "expansivas" más adelante en la tabla de rutas para resolver este problema.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-168">Put the 'greedy' routes later in the route table to solve this.</span></span>

### <a name="fallback"></a><span data-ttu-id="d0d3f-169">Reserva</span><span class="sxs-lookup"><span data-stu-id="d0d3f-169">Fallback</span></span>

<span data-ttu-id="d0d3f-170">Como parte del procesamiento de la solicitud, MVC comprobará que los valores de ruta se pueden usar para buscar un controlador y la acción en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-170">As part of request processing, MVC will verify that the route values can be used to find a controller and action in your application.</span></span> <span data-ttu-id="d0d3f-171">Si los valores de ruta no coincide con una acción, a continuación, la ruta no se considera a una coincidencia y se intentará la ruta siguiente.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-171">If the route values don't match an action then the route is not considered a match, and the next route will be tried.</span></span> <span data-ttu-id="d0d3f-172">Esto se denomina *reserva*, y se ha diseñado para simplificar los casos que se superponen a rutas convencionales.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-172">This is called *fallback*, and it's intended to simplify cases where conventional routes overlap.</span></span>

### <a name="disambiguating-actions"></a><span data-ttu-id="d0d3f-173">Eliminar la ambigüedad de acciones</span><span class="sxs-lookup"><span data-stu-id="d0d3f-173">Disambiguating actions</span></span>

<span data-ttu-id="d0d3f-174">Cuando coinciden los dos acciones a través de enrutamiento, debe eliminar la ambigüedad de MVC para elegir al candidato 'mejor'; de lo contrario producen una excepción.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-174">When two actions match through routing, MVC must disambiguate to choose the 'best' candidate or else throw an exception.</span></span> <span data-ttu-id="d0d3f-175">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="d0d3f-175">For example:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Edit(int id) { ... }

   [HttpPost]
   public IActionResult Edit(int id, Product product) { ... }
}
```

<span data-ttu-id="d0d3f-176">Este controlador define dos acciones que coincidiría con la ruta de acceso de dirección URL `/Products/Edit/17` y los datos de ruta `{ controller = Products, action = Edit, id = 17 }`.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-176">This controller defines two actions that would match the URL path `/Products/Edit/17` and route data `{ controller = Products, action = Edit, id = 17 }`.</span></span> <span data-ttu-id="d0d3f-177">Se trata de un patrón típico para controladores MVC donde `Edit(int)` muestra un formulario para editar un producto, y `Edit(int, Product)` procesa el formulario expuesto.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-177">This is a typical pattern for MVC controllers where `Edit(int)` shows a form to edit a product, and `Edit(int, Product)` processes  the posted form.</span></span> <span data-ttu-id="d0d3f-178">Para hacer esto posible MVC sería necesario seleccionar `Edit(int, Product)` cuando la solicitud es un HTTP `POST` y `Edit(int)` cuando el verbo HTTP es cualquier otra cosa.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-178">To make this possible MVC would need to choose `Edit(int, Product)` when the request is an HTTP `POST` and `Edit(int)` when the HTTP verb is anything else.</span></span>

<span data-ttu-id="d0d3f-179">El `HttpPostAttribute` ( `[HttpPost]` ) es una implementación de `IActionConstraint` que solo permitirá la acción que se seleccione cuando el verbo HTTP es `POST`.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-179">The `HttpPostAttribute` ( `[HttpPost]` ) is an implementation of `IActionConstraint` that will only allow the action to be selected when the HTTP verb is `POST`.</span></span> <span data-ttu-id="d0d3f-180">La presencia de un `IActionConstraint` realiza el `Edit(int, Product)` coincide con 'mejor' que `Edit(int)`, por lo que `Edit(int, Product)` se intentará en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-180">The presence of an `IActionConstraint` makes the `Edit(int, Product)` a 'better' match than `Edit(int)`, so `Edit(int, Product)` will be tried first.</span></span>

<span data-ttu-id="d0d3f-181">Sólo necesitará escribir personalizado `IActionConstraint` implementaciones en escenarios especializados, pero la importante para comprender el rol de atributos como `HttpPostAttribute` -se definen atributos similares para otros verbos HTTP.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-181">You will only need to write custom `IActionConstraint` implementations in specialized scenarios, but it's important to understand the role of attributes like `HttpPostAttribute`  - similar attributes are defined for other HTTP verbs.</span></span> <span data-ttu-id="d0d3f-182">En el enrutamiento convencional es común para las acciones que se usará el mismo nombre de acción al que forman parte de un `show form -> submit form` flujo de trabajo.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-182">In conventional routing it's common for actions to use the same action name when they are part of a `show form -> submit form` workflow.</span></span> <span data-ttu-id="d0d3f-183">La comodidad de este patrón serán más evidente después de revisar la [IActionConstraint descripción](#understanding-iactionconstraint) sección.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-183">The convenience of this pattern will become more apparent after reviewing the [Understanding IActionConstraint](#understanding-iactionconstraint) section.</span></span>

<span data-ttu-id="d0d3f-184">Si coincide con varias rutas y MVC no encontró ninguna ruta 'recomendada', se producirá un `AmbiguousActionException`.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-184">If multiple routes match, and MVC can't find a 'best' route, it will throw an `AmbiguousActionException`.</span></span>

<a name="routing-route-name-ref-label"></a>

### <a name="route-names"></a><span data-ttu-id="d0d3f-185">Nombres de ruta</span><span class="sxs-lookup"><span data-stu-id="d0d3f-185">Route names</span></span>

<span data-ttu-id="d0d3f-186">Las cadenas `"blog"` y `"default"` en los ejemplos siguientes son nombres de ruta:</span><span class="sxs-lookup"><span data-stu-id="d0d3f-186">The strings  `"blog"` and `"default"` in the following examples are route names:</span></span>


```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
               defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="d0d3f-187">Los nombres de ruta proporcionan un nombre lógico de la ruta para que la ruta con nombre puede utilizarse para la generación de dirección URL.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-187">The route names give the route a logical name so that the named route can be used for URL generation.</span></span> <span data-ttu-id="d0d3f-188">Esto simplifica en gran medida la creación de dirección URL cuando el orden de las rutas podría realizar la generación de direcciones URL complicada.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-188">This greatly simplifies URL creation when the ordering of routes could make URL generation complicated.</span></span> <span data-ttu-id="d0d3f-189">Los nombres de ruta deben ser único en toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-189">Route names must be unique application-wide.</span></span>

<span data-ttu-id="d0d3f-190">Los nombres de ruta no tienen ningún impacto en la dirección URL que coinciden o control de solicitudes; se usan únicamente para la generación de direcciones URL.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-190">Route names have no impact on URL matching or handling of requests; they are used only for URL generation.</span></span> <span data-ttu-id="d0d3f-191">[Enrutamiento](xref:fundamentals/routing) contiene información sobre la generación de dirección URL incluyendo generación de direcciones URL en aplicaciones auxiliares de MVC específicos más detallada.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-191">[Routing](xref:fundamentals/routing) has more detailed information on URL generation including URL generation in MVC-specific helpers.</span></span>

<a name="attribute-routing-ref-label"></a>

## <a name="attribute-routing"></a><span data-ttu-id="d0d3f-192">Ruta de atributo</span><span class="sxs-lookup"><span data-stu-id="d0d3f-192">Attribute routing</span></span>

<span data-ttu-id="d0d3f-193">Ruta de atributo, utiliza un conjunto de atributos para asignar acciones directamente a las plantillas de ruta.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-193">Attribute routing uses a set of attributes to map actions directly to route templates.</span></span> <span data-ttu-id="d0d3f-194">En el ejemplo siguiente, `app.UseMvc();` se utiliza en el `Configure` se pasa el método y no hay ninguna ruta.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-194">In the following example, `app.UseMvc();` is used in the `Configure` method and no route is passed.</span></span> <span data-ttu-id="d0d3f-195">El `HomeController` coincidirá con un conjunto de direcciones URL similares a qué la ruta predeterminada `{controller=Home}/{action=Index}/{id?}` coincidiría con:</span><span class="sxs-lookup"><span data-stu-id="d0d3f-195">The `HomeController` will match a set of URLs similar to what the default route `{controller=Home}/{action=Index}/{id?}` would match:</span></span>

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

<span data-ttu-id="d0d3f-196">El `HomeController.Index()` acción se ejecutará para cualquiera de las rutas de acceso de dirección URL `/`, `/Home`, o `/Home/Index`.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-196">The `HomeController.Index()` action will be executed for any of the URL paths `/`, `/Home`, or `/Home/Index`.</span></span>

> [!NOTE]
> <span data-ttu-id="d0d3f-197">Este ejemplo resalta una diferencia clave programación entre el atributo y enrutamiento convencional.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-197">This example highlights a key programming difference between attribute routing and conventional routing.</span></span> <span data-ttu-id="d0d3f-198">Ruta de atributo requiere más entradas para especificar una ruta; la ruta predeterminada convencional controla más brevemente las rutas.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-198">Attribute routing requires more input to specify a route; the conventional default route handles routes more succinctly.</span></span> <span data-ttu-id="d0d3f-199">Sin embargo, enrutamiento de atributo permite (y necesita) un control preciso de los cuales se aplican las plantillas de ruta para cada acción.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-199">However, attribute routing allows (and requires) precise control of which route templates apply to each action.</span></span>

<span data-ttu-id="d0d3f-200">Con el nombre del controlador y los nombres de acción de enrutamiento de atributo reproducir **sin** rol en el que se selecciona la acción.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-200">With attribute routing the controller name and action names play **no** role in which action is selected.</span></span> <span data-ttu-id="d0d3f-201">Este ejemplo coincidirá con las mismas direcciones URL que el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-201">This example will match the same URLs as the previous example.</span></span>

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
> <span data-ttu-id="d0d3f-202">Las plantillas de ruta anteriores no definen parámetros de ruta para `action`, `area`, y `controller`.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-202">The route templates above don't define route parameters for `action`, `area`, and `controller`.</span></span> <span data-ttu-id="d0d3f-203">De hecho, estos parámetros de ruta no se permiten en rutas de atributo.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-203">In fact, these route parameters are not allowed in attribute routes.</span></span> <span data-ttu-id="d0d3f-204">Puesto que la plantilla de ruta ya está asociada a una acción, no tendría sentido al analizar el nombre de acción de la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-204">Since the route template is already associated with an action, it wouldn't make sense to parse the action name from the URL.</span></span>

## <a name="attribute-routing-with-httpverb-attributes"></a><span data-ttu-id="d0d3f-205">Atributo de enrutamiento con atributos de Http [verbo]</span><span class="sxs-lookup"><span data-stu-id="d0d3f-205">Attribute routing with Http[Verb] attributes</span></span>

<span data-ttu-id="d0d3f-206">Ruta de atributo también puede hacer uso de la `Http[Verb]` atributos como `HttpPostAttribute`.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-206">Attribute routing can also make use of the `Http[Verb]` attributes such as `HttpPostAttribute`.</span></span> <span data-ttu-id="d0d3f-207">Todos estos atributos pueden aceptar una plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-207">All of these attributes can accept a route template.</span></span> <span data-ttu-id="d0d3f-208">Este ejemplo muestra dos acciones que coinciden con la misma plantilla de ruta:</span><span class="sxs-lookup"><span data-stu-id="d0d3f-208">This example shows two actions that match the same route template:</span></span>

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

<span data-ttu-id="d0d3f-209">Para una ruta de acceso de dirección URL como `/products` el `ProductsApi.ListProducts` acción se ejecutará cuando el verbo HTTP es `GET` y `ProductsApi.CreateProduct` se ejecutará cuando el verbo HTTP es `POST`.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-209">For a URL path like `/products` the `ProductsApi.ListProducts` action will be executed when the HTTP verb is `GET` and `ProductsApi.CreateProduct` will be executed when the HTTP verb is `POST`.</span></span> <span data-ttu-id="d0d3f-210">Atributo enrutamiento primero coincide con la dirección URL en el conjunto de plantillas de ruta que se definen mediante atributos de ruta.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-210">Attribute routing first matches the URL against the set of route templates defined by route attributes.</span></span> <span data-ttu-id="d0d3f-211">Una vez que coincide con una plantilla de ruta, `IActionConstraint` las restricciones se aplican para determinar qué acciones se pueden ejecutar.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-211">Once a route template matches, `IActionConstraint` constraints are applied to determine which actions can be executed.</span></span>

> [!TIP]
> <span data-ttu-id="d0d3f-212">Al compilar una API de REST, es poco frecuente que desea usar `[Route(...)]` en un método de acción.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-212">When building a REST API, it's rare that you will want to use `[Route(...)]` on an action method.</span></span> <span data-ttu-id="d0d3f-213">Es mejor usar más específica `Http*Verb*Attributes` para ser precisos sobre lo que es compatible con la API.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-213">It's better to use the more specific `Http*Verb*Attributes` to be precise about what your API supports.</span></span> <span data-ttu-id="d0d3f-214">Se esperan que los clientes de API de REST para saber qué rutas de acceso y verbos HTTP que se asignan a determinadas operaciones lógicas.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-214">Clients of REST APIs are expected to know what paths and HTTP verbs map to specific logical operations.</span></span>

<span data-ttu-id="d0d3f-215">Puesto que una ruta de atributo se aplica a una acción específica, es fácil realizar parámetros necesarios como parte de la definición de plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-215">Since an attribute route applies to a specific action, it's easy to make parameters required as part of the route template definition.</span></span> <span data-ttu-id="d0d3f-216">En este ejemplo, `id` es necesario como parte de la ruta de acceso de dirección URL.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-216">In this example, `id` is required as part of the URL path.</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="d0d3f-217">El `ProductsApi.GetProduct(int)` acción se ejecutará para una ruta de acceso de dirección URL como `/products/3` , pero no para una ruta de acceso de dirección URL como `/products`.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-217">The `ProductsApi.GetProduct(int)` action will be executed for a URL path like `/products/3` but not for a URL path like `/products`.</span></span> <span data-ttu-id="d0d3f-218">Vea [enrutamiento](../../fundamentals/routing.md) para obtener una descripción completa de plantillas de ruta y opciones relacionadas.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-218">See [Routing](../../fundamentals/routing.md) for a full description of route templates and related options.</span></span>

## <a name="route-name"></a><span data-ttu-id="d0d3f-219">Nombre de ruta</span><span class="sxs-lookup"><span data-stu-id="d0d3f-219">Route Name</span></span>

<span data-ttu-id="d0d3f-220">El código siguiente define un *nombre de ruta* de `Products_List`:</span><span class="sxs-lookup"><span data-stu-id="d0d3f-220">The following code  defines a *route name* of `Products_List`:</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="d0d3f-221">Los nombres de ruta se pueden utilizar para generar una dirección URL basada en una ruta específica.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-221">Route names can be used to generate a URL based on a specific route.</span></span> <span data-ttu-id="d0d3f-222">Los nombres de ruta no tienen ningún impacto en la dirección URL del comportamiento de enrutamiento de la coincidencia y solo se usan para la generación de direcciones URL.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-222">Route names have no impact on the URL matching behavior of routing and are only used for URL generation.</span></span> <span data-ttu-id="d0d3f-223">Los nombres de ruta deben ser único en toda la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-223">Route names must be unique application-wide.</span></span>

> [!NOTE]
> <span data-ttu-id="d0d3f-224">Compare esto con el convencional *ruta predeterminada*, que define la `id` parámetro como opcional (`{id?}`).</span><span class="sxs-lookup"><span data-stu-id="d0d3f-224">Contrast this with the conventional *default route*, which defines the `id` parameter as optional (`{id?}`).</span></span> <span data-ttu-id="d0d3f-225">Esta capacidad de especificar con precisión las API tiene sus ventajas, como permitir que los `/products` y `/products/5` va a enviar a acciones diferentes.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-225">This ability to precisely specify APIs has advantages, such as  allowing `/products` and `/products/5` to be dispatched to different actions.</span></span>

<a name="routing-combining-ref-label"></a>

### <a name="combining-routes"></a><span data-ttu-id="d0d3f-226">Rutas de combinación</span><span class="sxs-lookup"><span data-stu-id="d0d3f-226">Combining routes</span></span>

<span data-ttu-id="d0d3f-227">Para realizar el enrutamiento de atributo menos repetitivos, atributos de ruta en el controlador se combinan con los atributos de ruta en las acciones individuales.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-227">To make attribute routing less repetitive, route attributes on the controller are combined with route attributes on the individual actions.</span></span> <span data-ttu-id="d0d3f-228">Las plantillas de ruta definidas en el controlador se anteponen a plantillas de ruta en las acciones.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-228">Any route templates defined on the controller are prepended to route templates on the actions.</span></span> <span data-ttu-id="d0d3f-229">Colocación de un atributo de ruta en el controlador hace **todos los** acciones en el controlador use el enrutamiento de atributo.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-229">Placing a route attribute on the controller makes **all** actions in the controller use attribute routing.</span></span>

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

<span data-ttu-id="d0d3f-230">En este ejemplo, la ruta de acceso de dirección URL `/products` puede coincidir con `ProductsApi.ListProducts`y la ruta de acceso de dirección URL `/products/5` puede coincidir con `ProductsApi.GetProduct(int)`.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-230">In this example the URL path `/products` can match `ProductsApi.ListProducts`, and the URL path `/products/5` can match `ProductsApi.GetProduct(int)`.</span></span> <span data-ttu-id="d0d3f-231">Ambas acciones coincide solo con HTTP `GET` porque se decoran con el `HttpGetAttribute`.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-231">Both of these actions only match HTTP `GET` because they are decorated with the `HttpGetAttribute`.</span></span>

<span data-ttu-id="d0d3f-232">Enrutar ninguna plantilla aplicada a una acción que comienzan por un `/` no se combinen con plantillas de ruta que se aplica al controlador.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-232">Route templates applied to an action that begin with a `/` do not get combined with route templates applied to the controller.</span></span> <span data-ttu-id="d0d3f-233">En este ejemplo coinciden con un conjunto de rutas de acceso de dirección URL similar a la *ruta predeterminada*.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-233">This example matches a set of URL paths similar to the *default route*.</span></span>

```csharp
[Route("Home")]
public class HomeController : Controller
{
    [Route("")]      // Combines to define the route template "Home"
    [Route("Index")] // Combines to define the route template "Home/Index"
    [Route("/")]     // Does not combine, defines the route template ""
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

### <a name="ordering-attribute-routes"></a><span data-ttu-id="d0d3f-234">Ordenación de las rutas de atributo</span><span class="sxs-lookup"><span data-stu-id="d0d3f-234">Ordering attribute routes</span></span>

<span data-ttu-id="d0d3f-235">A diferencia de las rutas convencionales que se ejecutan en un orden definido, el enrutamiento de atributo genera un árbol y coincide con todas las rutas al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-235">In contrast to conventional routes which execute in a defined order, attribute routing builds a tree and matches all routes simultaneously.</span></span> <span data-ttu-id="d0d3f-236">Esto se comporta como-si las entradas de ruta se colocaron en un orden ideal; las rutas más específicas tienen una oportunidad de ejecutarse antes de las rutas más generales.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-236">This behaves as-if the route entries were placed in an ideal ordering; the most specific routes have a chance to execute before the more general routes.</span></span>

<span data-ttu-id="d0d3f-237">Por ejemplo, una ruta como `blog/search/{topic}` es más específico que una ruta como `blog/{*article}`.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-237">For example, a route like `blog/search/{topic}` is more specific than a route like `blog/{*article}`.</span></span> <span data-ttu-id="d0d3f-238">Habla lógicamente el `blog/search/{topic}` ruta 'ejecuta' en primer lugar, de forma predeterminada, ya que ese es el orden solo significativo.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-238">Logically speaking the `blog/search/{topic}` route 'runs' first, by default, because that's the only sensible ordering.</span></span> <span data-ttu-id="d0d3f-239">Con el enrutamiento convencional, el programador es responsable de colocación de las rutas en el orden deseado.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-239">Using conventional routing, the developer is  responsible for placing routes in the desired order.</span></span>

<span data-ttu-id="d0d3f-240">Rutas de atributo pueden configurar un pedido, mediante el `Order` propiedad de todos los atributos de ruta del marco.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-240">Attribute routes can configure an order, using the `Order` property of all of the framework provided route attributes.</span></span> <span data-ttu-id="d0d3f-241">Las rutas se procesan según un orden ascendente de la `Order` propiedad.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-241">Routes are processed according to an ascending sort of the `Order` property.</span></span> <span data-ttu-id="d0d3f-242">El orden predeterminado es `0`.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-242">The default order is `0`.</span></span> <span data-ttu-id="d0d3f-243">Configurar una ruta mediante `Order = -1` se ejecutará antes que rutas que no establece un pedido.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-243">Setting a route using `Order = -1` will run before routes that don't set an order.</span></span> <span data-ttu-id="d0d3f-244">Configurar una ruta mediante `Order = 1` se ejecutará después de la ordenación de ruta predeterminada.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-244">Setting a route using `Order = 1` will run after default route ordering.</span></span>

> [!TIP]
> <span data-ttu-id="d0d3f-245">Evitar según `Order`.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-245">Avoid depending on `Order`.</span></span> <span data-ttu-id="d0d3f-246">Si el espacio de direcciones URL requiere que los valores de orden explícito para enrutar correctamente, es probable que confuso para los clientes, así.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-246">If your URL-space requires explicit order values to route correctly, then it's likely confusing to clients as well.</span></span> <span data-ttu-id="d0d3f-247">Por lo general la ruta de atributo seleccionará la ruta correcta con la coincidencia de dirección URL.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-247">In general attribute routing will select the correct route with URL matching.</span></span> <span data-ttu-id="d0d3f-248">Si el orden predeterminado que se usa para la generación de dirección URL no funciona, usando el nombre de ruta como una invalidación es normalmente más sencilla que aplicar la `Order` propiedad.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-248">If the default order used for URL generation isn't working, using route name as an override is usually simpler than applying the `Order` property.</span></span>

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a><span data-ttu-id="d0d3f-249">Símbolo (token) de sustitución en plantillas de ruta ([controlador], [acción] [area])</span><span class="sxs-lookup"><span data-stu-id="d0d3f-249">Token replacement in route templates ([controller], [action], [area])</span></span>

<span data-ttu-id="d0d3f-250">Para mayor comodidad, se admiten rutas de atributo *reemplazo del token* incluyendo un token entre llaves cuadrado (`[`, `]`).</span><span class="sxs-lookup"><span data-stu-id="d0d3f-250">For convenience, attribute routes support *token replacement* by enclosing a token in square-braces (`[`, `]`).</span></span> <span data-ttu-id="d0d3f-251">Los tokens `[action]`, `[area]`, y `[controller]` se sustituirán por los valores del nombre de la acción, el nombre de área y nombre del controlador de la acción donde se define la ruta.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-251">The tokens `[action]`, `[area]`, and `[controller]` will be replaced with the values of the action name, area name, and controller name from the action where the route is defined.</span></span> <span data-ttu-id="d0d3f-252">En este ejemplo las acciones pueden coincidir con las rutas de acceso de dirección URL como se describe en los comentarios:</span><span class="sxs-lookup"><span data-stu-id="d0d3f-252">In this example the actions can match URL paths as described in the comments:</span></span>

[!code-csharp[Main](routing/sample/main/Controllers/ProductsController.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="d0d3f-253">Reemplazo del token se produce cuando el último paso de la creación de las rutas de atributo.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-253">Token replacement occurs as the last step of building the attribute routes.</span></span> <span data-ttu-id="d0d3f-254">El ejemplo anterior comportará igual que el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="d0d3f-254">The above example will behave the same as the following code:</span></span>

[!code-csharp[Main](routing/sample/main/Controllers/ProductsController2.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="d0d3f-255">Rutas de atributo también pueden combinarse con la herencia.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-255">Attribute routes can also be combined with inheritance.</span></span> <span data-ttu-id="d0d3f-256">Esto es especialmente eficaz a combinar con el reemplazo del token.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-256">This is particularly powerful combined with token replacement.</span></span>

```csharp
[Route("api/[controller]")]
public abstract class MyBaseController : Controller { ... }

public class ProductsController : MyBaseController
{
   [HttpGet] // Matches '/api/Products'
   public IActionResult List() { ... }

   [HttpPost("{id}")] // Matches '/api/Products/{id}'
   public IActionResult Edit(int id) { ... }
}
```

<span data-ttu-id="d0d3f-257">Reemplazo del token también se aplica a los nombres de ruta definidos por las rutas de atributo.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-257">Token replacement also applies to route names defined by attribute routes.</span></span> <span data-ttu-id="d0d3f-258">`[Route("[controller]/[action]", Name="[controller]_[action]")]`se generará un nombre de ruta único para cada acción.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-258">`[Route("[controller]/[action]", Name="[controller]_[action]")]` will generate a unique route name for each action.</span></span>

<span data-ttu-id="d0d3f-259">Para que coincida con el delimitador de reemplazo del token literal `[` o `]`, escape repitiendo el carácter (`[[` o `]]`).</span><span class="sxs-lookup"><span data-stu-id="d0d3f-259">To match the literal token replacement delimiter `[` or  `]`, escape it by repeating the character (`[[` or `]]`).</span></span>

<a name="routing-multiple-routes-ref-label"></a>

### <a name="multiple-routes"></a><span data-ttu-id="d0d3f-260">Varias rutas</span><span class="sxs-lookup"><span data-stu-id="d0d3f-260">Multiple Routes</span></span>

<span data-ttu-id="d0d3f-261">Atributo enrutamiento permite definir varias rutas que llegan a la misma acción.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-261">Attribute routing supports defining multiple routes that reach the same action.</span></span> <span data-ttu-id="d0d3f-262">El uso más común de esto es imitar el comportamiento de la *ruta convencional de predeterminada* tal como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="d0d3f-262">The most common usage of this is to mimic the behavior of the *default conventional route* as shown in the following example:</span></span>

```csharp
[Route("[controller]")]
public class ProductsController : Controller
{
   [Route("")]     // Matches 'Products'
   [Route("Index")] // Matches 'Products/Index'
   public IActionResult Index()
}
```

<span data-ttu-id="d0d3f-263">Colocar varios atributos de ruta en el controlador significa que cada uno de ellos se combinará con cada uno de los atributos de ruta en los métodos de acción.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-263">Putting multiple route attributes on the controller means that each one will combine with each of the route attributes on the action methods.</span></span>

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

<span data-ttu-id="d0d3f-264">Cuando varios atributos de ruta (que implementan `IActionConstraint`) se colocan en una acción, a continuación, cada acción de restricción se combina con la plantilla de ruta del atributo que lo define.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-264">When multiple route attributes (that implement `IActionConstraint`) are placed on an action, then each action constraint combines with the route template from the attribute that defined it.</span></span>

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
> <span data-ttu-id="d0d3f-265">Aunque puede parecer eficaz utilizando varias rutas en acciones, es mejor mantener el espacio de direcciones URL de la aplicación simple y bien definidos.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-265">While using multiple routes on actions can seem powerful, it's better to keep your application's URL space simple and well-defined.</span></span> <span data-ttu-id="d0d3f-266">Utilice varias rutas de acciones solo cuando sea necesario, por ejemplo, para admitir a los clientes existentes.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-266">Use multiple routes on actions only where needed, for example to support existing clients.</span></span>

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a><span data-ttu-id="d0d3f-267">Especificación de parámetros opcionales de ruta de atributo, valores predeterminados y restricciones</span><span class="sxs-lookup"><span data-stu-id="d0d3f-267">Specifying attribute route optional parameters, default values, and constraints</span></span>

<span data-ttu-id="d0d3f-268">Rutas de atributo admiten la misma sintaxis en línea como rutas convencionales para especificar los parámetros opcionales, valores predeterminados y restricciones.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-268">Attribute routes support the same inline syntax as conventional routes to specify optional parameters, default values, and constraints.</span></span>

```csharp
[HttpPost("product/{id:int}")]
public IActionResult ShowProduct(int id)
{
   // ...
}
```

<span data-ttu-id="d0d3f-269">Vea [referencia de plantilla de ruta](../../fundamentals/routing.md#route-template-reference) para obtener una descripción detallada de la sintaxis de la plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-269">See [Route Template Reference](../../fundamentals/routing.md#route-template-reference) for a detailed description of route template syntax.</span></span>

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a><span data-ttu-id="d0d3f-270">Atributos de ruta personalizados utilizando`IRouteTemplateProvider`</span><span class="sxs-lookup"><span data-stu-id="d0d3f-270">Custom route attributes using `IRouteTemplateProvider`</span></span>

<span data-ttu-id="d0d3f-271">Todos los atributos de la ruta proporcionados en el marco de trabajo ( `[Route(...)]`, `[HttpGet(...)]` , etc.) implementan la `IRouteTemplateProvider` interfaz.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-271">All of the route attributes provided in the framework ( `[Route(...)]`, `[HttpGet(...)]` , etc.) implement the `IRouteTemplateProvider` interface.</span></span> <span data-ttu-id="d0d3f-272">MVC busca atributos en las clases de controlador y los métodos de acción cuando se inicia la aplicación y usa los que implementarán `IRouteTemplateProvider` para generar el conjunto inicial de rutas.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-272">MVC looks for attributes on controller classes and action methods when the app starts and uses the ones that implement `IRouteTemplateProvider` to build the initial set of routes.</span></span>

<span data-ttu-id="d0d3f-273">Puede implementar `IRouteTemplateProvider` para definir sus propios atributos de ruta.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-273">You can implement `IRouteTemplateProvider` to define your own route attributes.</span></span> <span data-ttu-id="d0d3f-274">Cada `IRouteTemplateProvider` le permite definir una única ruta con una plantilla de ruta personalizados, ordenar y el nombre:</span><span class="sxs-lookup"><span data-stu-id="d0d3f-274">Each `IRouteTemplateProvider` allows you to define a single route with a custom route template, order, and name:</span></span>

```csharp
public class MyApiControllerAttribute : Attribute, IRouteTemplateProvider
{
   public string Template => "api/[controller]";

   public int? Order { get; set; }

   public string Name { get; set; }
}
```

<span data-ttu-id="d0d3f-275">El atributo por el ejemplo anterior se establece automáticamente el `Template` a `"api/[controller]"` cuando `[MyApiController]` se aplica.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-275">The attribute from the above example automatically sets the `Template` to `"api/[controller]"` when `[MyApiController]` is applied.</span></span>

<a name="routing-app-model-ref-label"></a>

### <a name="using-application-model-to-customize-attribute-routes"></a><span data-ttu-id="d0d3f-276">Uso de modelo de aplicación para personalizar las rutas de atributo</span><span class="sxs-lookup"><span data-stu-id="d0d3f-276">Using Application Model to customize attribute routes</span></span>

<span data-ttu-id="d0d3f-277">El *modelo de aplicación* es un modelo de objetos creado durante el inicio con todos los metadatos de MVC utilizado para enrutar y ejecutar las acciones.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-277">The *application model* is an object model created at startup with all of the metadata used by MVC to route and execute your actions.</span></span> <span data-ttu-id="d0d3f-278">El *modelo de aplicación* incluye todos los datos recopilados de los atributos de ruta (a través de `IRouteTemplateProvider`).</span><span class="sxs-lookup"><span data-stu-id="d0d3f-278">The *application model* includes all of the data gathered from route attributes (through `IRouteTemplateProvider`).</span></span> <span data-ttu-id="d0d3f-279">Puede escribir *convenciones* para modificar el modelo de aplicación en tiempo de inicio para personalizar el comportamiento de enrutamiento.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-279">You can write *conventions* to modify the application model at startup time to customize how routing behaves.</span></span> <span data-ttu-id="d0d3f-280">Esta sección muestra un ejemplo sencillo de personalizar el enrutamiento mediante el modelo de aplicación.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-280">This section shows a simple example of customizing routing using application model.</span></span>

[!code-csharp[Main](routing/sample/main/NamespaceRoutingConvention.cs)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a><span data-ttu-id="d0d3f-281">Mixto enrutamiento: atributo enrutamiento enrutamiento convencional de vs</span><span class="sxs-lookup"><span data-stu-id="d0d3f-281">Mixed routing: Attribute routing vs conventional routing</span></span>

<span data-ttu-id="d0d3f-282">Las aplicaciones MVC pueden mezclar el uso de convencional y atributo de enrutamiento.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-282">MVC applications can mix the use of conventional routing and attribute routing.</span></span> <span data-ttu-id="d0d3f-283">Es habitual para usar las rutas de convencionales para controladores de servir páginas HTML para los exploradores y el atributo de enrutamiento para los controladores que sirve al API de REST.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-283">It's typical to use conventional routes for controllers serving HTML pages for browsers, and attribute routing for controllers serving REST APIs.</span></span>

<span data-ttu-id="d0d3f-284">Acciones o se enrutan convencional o enruta de atributo.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-284">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="d0d3f-285">Genera una ruta en el controlador o la acción hace que se enrutan de atributo.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-285">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="d0d3f-286">Las acciones que definen rutas de atributo no se puede alcanzar a través de las rutas convencionales y viceversa.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-286">Actions that define attribute routes cannot be reached through the conventional routes and vice-versa.</span></span> <span data-ttu-id="d0d3f-287">**Cualquier** atributo de ruta en el controlador hace que todas las acciones en el atributo de controlador enrutando.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-287">**Any** route attribute on the controller makes all actions in the controller attribute routed.</span></span>

> [!NOTE]
> <span data-ttu-id="d0d3f-288">Lo que distingue entre los dos tipos de sistemas de enrutamientos es el proceso que se aplica después de que una dirección URL coincide con una plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-288">What distinguishes the two types of routing systems is the process applied after a URL matches a route template.</span></span> <span data-ttu-id="d0d3f-289">En el enrutamiento convencional, se usan los valores de ruta de la búsqueda de coincidencias para elegir la acción y el controlador de una tabla de búsqueda de todas las acciones enrutadas convencionales.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-289">In conventional routing, the route values from the match are used to choose the action and controller from a lookup table of all conventional routed actions.</span></span> <span data-ttu-id="d0d3f-290">En el enrutamiento de atributo, cada plantilla ya está asociado a una acción y no es necesaria ninguna otra búsqueda.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-290">In attribute routing, each template is already associated with an action, and no further lookup is needed.</span></span>

<a name="routing-url-gen-ref-label"></a>

## <a name="url-generation"></a><span data-ttu-id="d0d3f-291">Generación de direcciones URL</span><span class="sxs-lookup"><span data-stu-id="d0d3f-291">URL Generation</span></span>

<span data-ttu-id="d0d3f-292">Las aplicaciones de MVC pueden usar características de generación de dirección URL de enrutamiento para generar vínculos de dirección URL a las acciones.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-292">MVC applications can use routing's URL generation features to generate URL links to actions.</span></span> <span data-ttu-id="d0d3f-293">Generar direcciones URL elimina las direcciones URL de codificar, hacer que el código sea más compacto y fácil de mantener.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-293">Generating URLs eliminates hardcoding URLs, making your code more robust and maintainable.</span></span> <span data-ttu-id="d0d3f-294">En esta sección se centra en las características de generación de dirección URL proporcionadas por MVC y sólo aborda los conceptos básicos de cómo funciona la generación de direcciones URL.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-294">This section focuses on the URL generation features provided by MVC and will only cover basics of how URL generation works.</span></span> <span data-ttu-id="d0d3f-295">Vea [enrutamiento](../../fundamentals/routing.md) para obtener una descripción detallada de generación de direcciones URL.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-295">See [Routing](../../fundamentals/routing.md) for a detailed description of URL generation.</span></span>

<span data-ttu-id="d0d3f-296">La `IUrlHelper` interfaz es la pieza subyacente de la infraestructura entre MVC y enrutamiento de generación de direcciones URL.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-296">The `IUrlHelper` interface is the underlying piece of infrastructure between MVC and routing for URL generation.</span></span> <span data-ttu-id="d0d3f-297">Puede encontrar una instancia de `IUrlHelper` disponibles a través de la `Url` propiedad en componentes de la vista, vistas y controladores.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-297">You'll find an instance of `IUrlHelper` available through the `Url` property in controllers, views, and view components.</span></span>

<span data-ttu-id="d0d3f-298">En este ejemplo, el `IUrlHelper` interfaz se usa a través de la `Controller.Url` propiedad que se va a generar una dirección URL a otra acción.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-298">In this example, the `IUrlHelper` interface is used through the `Controller.Url` property to generate a URL to another action.</span></span>

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

<span data-ttu-id="d0d3f-299">Si la aplicación está usando el valor predeterminado convencional de ruta, el valor de la `url` variable será la cadena de ruta de acceso de dirección URL `/UrlGeneration/Destination`.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-299">If the application is using the default conventional route, the value of the `url` variable will be the URL path string `/UrlGeneration/Destination`.</span></span> <span data-ttu-id="d0d3f-300">Esta ruta de acceso de dirección URL se crea mediante el enrutamiento mediante la combinación de los valores de ruta de la solicitud actual (valores de ambiente), con los valores pasados a `Url.Action` y sustituir los valores en la plantilla de ruta:</span><span class="sxs-lookup"><span data-stu-id="d0d3f-300">This URL path is created by routing by combining the route values from the current request (ambient values), with the values passed to `Url.Action` and substituting those values into the route template:</span></span>

```
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

<span data-ttu-id="d0d3f-301">Cada parámetro de ruta en la plantilla de ruta tiene su valor sustituido por nombres coincidentes con los valores y los valores de ambiente.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-301">Each route parameter in the route template has its value substituted by matching names with the values and ambient values.</span></span> <span data-ttu-id="d0d3f-302">Un parámetro de ruta que no tiene un valor puede utilizar un valor predeterminado si tiene uno, o se pueden omitir si es opcional (al igual que en el caso de `id` en este ejemplo).</span><span class="sxs-lookup"><span data-stu-id="d0d3f-302">A route parameter that does not have a value can use a default value if it has one, or be skipped if it is optional (as in the case of `id` in this example).</span></span> <span data-ttu-id="d0d3f-303">Se producirá un error en la generación de direcciones URL si cualquier parámetro de ruta necesaria no tiene un valor correspondiente.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-303">URL generation will fail if any required route parameter doesn't have a corresponding value.</span></span> <span data-ttu-id="d0d3f-304">Si se produce un error en la generación de direcciones URL para una ruta, se prueba con la ruta siguiente hasta que se hayan probado todas las rutas o se encuentra una coincidencia.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-304">If URL generation fails for a route, the next route is tried until all routes have been tried or a match is found.</span></span>

<span data-ttu-id="d0d3f-305">En el ejemplo de `Url.Action` anteriormente se supone que el enrutamiento convencional, pero funciona de la generación de dirección URL del mismo modo con el enrutamiento de atributo, aunque los conceptos son diferentes.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-305">The example of `Url.Action` above assumes conventional routing, but URL generation works similarly with attribute routing, though the concepts are different.</span></span> <span data-ttu-id="d0d3f-306">Con el enrutamiento convencional, se usan los valores de ruta para expandir una plantilla y los valores de ruta de `controller` y `action` suelen aparecer en esa plantilla - esto funciona porque las direcciones URL que coinciden con el enrutamiento adhieren a un *convención*.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-306">With conventional routing, the route values are used to expand a template, and the route values for `controller` and `action` usually appear in that template - this works because the URLs matched by routing adhere to a *convention*.</span></span> <span data-ttu-id="d0d3f-307">En el enrutamiento de atributo, los valores de la ruta de `controller` y `action` no se permite que aparezca en la plantilla - en su lugar, se usan para buscar qué plantilla utilizar.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-307">In attribute routing, the route values for `controller` and `action` are not allowed to appear in the template - they are instead used to look up which template to use.</span></span>

<span data-ttu-id="d0d3f-308">Este ejemplo utiliza la ruta de atributo:</span><span class="sxs-lookup"><span data-stu-id="d0d3f-308">This example uses attribute routing:</span></span>

[!code-csharp[Main](routing/sample/main/StartupUseMvc.cs?name=snippet_1)]

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationControllerAttr.cs?name=snippet_1)]

<span data-ttu-id="d0d3f-309">MVC genera una tabla de búsqueda de todas las acciones de atributo que se enrutan y se corresponderá con el `controller` y `action` valores para seleccionar la plantilla de ruta que se usará para la generación de direcciones URL.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-309">MVC builds a lookup table of all attribute routed actions and will match the `controller` and `action` values to select the route template to use for URL generation.</span></span> <span data-ttu-id="d0d3f-310">En el ejemplo anterior, `custom/url/to/destination` se genera.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-310">In the sample above,   `custom/url/to/destination` is generated.</span></span>

### <a name="generating-urls-by-action-name"></a><span data-ttu-id="d0d3f-311">Generar direcciones URL al nombre de acción</span><span class="sxs-lookup"><span data-stu-id="d0d3f-311">Generating URLs by action name</span></span>

<span data-ttu-id="d0d3f-312">`Url.Action` (`IUrlHelper` .</span><span class="sxs-lookup"><span data-stu-id="d0d3f-312">`Url.Action` (`IUrlHelper` .</span></span> <span data-ttu-id="d0d3f-313">`Action`) y todos los relacionados con las sobrecargas todos se basan en esa idea que desea especificar lo que está vinculando a especificando un nombre de acción y controlador.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-313">`Action`) and all related overloads all are based on that idea that you want to specify what you're linking to by specifying a controller name and action name.</span></span>

> [!NOTE]
> <span data-ttu-id="d0d3f-314">Cuando se usa `Url.Action`, los valores de la ruta actual para `controller` y `action` se especifican para usted, el valor de `controller` y `action` forman parte de ambos *valores ambiente* **y** *valores*.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-314">When using `Url.Action`, the current route values for `controller` and `action` are specified for you - the value of `controller` and `action` are part of both *ambient values* **and** *values*.</span></span> <span data-ttu-id="d0d3f-315">El método `Url.Action`, siempre utiliza los valores actuales de `action` y `controller` y generará una ruta de acceso de dirección URL que se enruta a la acción actual.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-315">The method `Url.Action`, always uses the current values of `action` and `controller` and will generate a URL path that routes to the current action.</span></span>

<span data-ttu-id="d0d3f-316">Intenta utilizar los valores en los valores de ambiente para rellenar la información que no proporcionan al generar una dirección URL de enrutamiento.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-316">Routing attempts to use the values in ambient values to fill in information that you didn't provide when generating a URL.</span></span> <span data-ttu-id="d0d3f-317">Uso de una ruta como `{a}/{b}/{c}/{d}` y valores de ambiente `{ a = Alice, b = Bob, c = Carol, d = David }`, enrutamiento tiene suficiente información para generar una dirección URL sin ningún valor adicional: puesto que en todas las rutas parámetros tienen un valor.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-317">Using a route like `{a}/{b}/{c}/{d}` and ambient values `{ a = Alice, b = Bob, c = Carol, d = David }`, routing has enough information to generate a URL without any additional values - since all route parameters have a value.</span></span> <span data-ttu-id="d0d3f-318">Si agrega el valor `{ d = Donovan }`, el valor `{ d = David }` se ignorará y la ruta de acceso de dirección URL generada sería `Alice/Bob/Carol/Donovan`.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-318">If you added the value `{ d = Donovan }`, the value `{ d = David }` would be ignored, and the generated URL path would be `Alice/Bob/Carol/Donovan`.</span></span>

> [!WARNING]
> <span data-ttu-id="d0d3f-319">Las rutas de acceso de dirección URL son jerárquicos.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-319">URL paths are hierarchical.</span></span> <span data-ttu-id="d0d3f-320">En el ejemplo anterior, si se agrega el valor `{ c = Cheryl }`, ambos de los valores `{ c = Carol, d = David }` se ignoraría.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-320">In the example above, if you added the value `{ c = Cheryl }`, both of the values `{ c = Carol, d = David }` would be ignored.</span></span> <span data-ttu-id="d0d3f-321">En este caso ya no tenemos un valor para `d` y se producirá un error en la generación de direcciones URL.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-321">In this case we no longer have a value for `d` and URL generation will fail.</span></span> <span data-ttu-id="d0d3f-322">Debe especificar el valor deseado de `c` y `d`.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-322">You would need to specify the desired value of `c` and `d`.</span></span>  <span data-ttu-id="d0d3f-323">Puede esperar a que se alcanza este problema con la ruta predeterminada (`{controller}/{action}/{id?}`)- pero raramente encontrará este comportamiento en la práctica como `Url.Action` especificará siempre explícitamente una `controller` y `action` valor.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-323">You might expect to hit this problem with the default route (`{controller}/{action}/{id?}`) - but you will rarely encounter this behavior in practice as `Url.Action` will always explicitly specify a `controller` and `action` value.</span></span>

<span data-ttu-id="d0d3f-324">Más sobrecargas de `Url.Action` tener más *valores de ruta* objeto para proporcionar valores para parámetros de ruta distinto de `controller` y `action`.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-324">Longer overloads of `Url.Action` also take an additional *route values* object to provide values for route parameters other than `controller` and `action`.</span></span> <span data-ttu-id="d0d3f-325">Normalmente verá esto utiliza con `id` como `Url.Action("Buy", "Products", new { id = 17 })`.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-325">You will most commonly see this used with `id` like `Url.Action("Buy", "Products", new { id = 17 })`.</span></span> <span data-ttu-id="d0d3f-326">Por convención la *valores de ruta* normalmente es un objeto de tipo anónimo, pero también puede ser un `IDictionary<>` o un *objeto plain old de .NET*.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-326">By convention the *route values* object is usually an object of anonymous type, but it can also be an `IDictionary<>` or a *plain old .NET object*.</span></span> <span data-ttu-id="d0d3f-327">Los valores de ruta adicionales que no coinciden con los parámetros de ruta se colocan en la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-327">Any additional route values that don't match route parameters are put in the query string.</span></span>

[!code-csharp[Main](routing/sample/main/Controllers/TestController.cs)]

> [!TIP]
> <span data-ttu-id="d0d3f-328">Para crear una dirección URL absoluta, use una sobrecarga que acepta un `protocol`:`Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span><span class="sxs-lookup"><span data-stu-id="d0d3f-328">To create an absolute URL, use an overload that accepts a `protocol`: `Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span></span>

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generating-urls-by-route"></a><span data-ttu-id="d0d3f-329">Generación de direcciones URL de ruta</span><span class="sxs-lookup"><span data-stu-id="d0d3f-329">Generating URLs by route</span></span>

<span data-ttu-id="d0d3f-330">El código anterior muestra generando una dirección URL al pasar el nombre de acción y controlador.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-330">The code above demonstrated generating a URL by passing in the controller and action name.</span></span> <span data-ttu-id="d0d3f-331">`IUrlHelper`También proporciona la `Url.RouteUrl` familia de métodos.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-331">`IUrlHelper` also provides the `Url.RouteUrl` family of methods.</span></span> <span data-ttu-id="d0d3f-332">Estos métodos son similares a `Url.Action`, pero no que se copian los valores actuales de `action` y `controller` a los valores de ruta.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-332">These methods are similar to `Url.Action`, but they do not copy the current values of `action` and `controller` to the route values.</span></span> <span data-ttu-id="d0d3f-333">El uso más común es especificar un nombre de ruta que utilice una ruta específica para generar la dirección URL, por lo general *sin* especificando un nombre de acción o controlador.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-333">The most common usage is to specify a route name to use a specific route to generate the URL, generally *without* specifying a controller or action name.</span></span>

[!code-csharp[Main](routing/sample/main/Controllers/UrlGenerationControllerRouting.cs?name=snippet_1)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generating-urls-in-html"></a><span data-ttu-id="d0d3f-334">Generar direcciones URL en HTML</span><span class="sxs-lookup"><span data-stu-id="d0d3f-334">Generating URLs in HTML</span></span>

<span data-ttu-id="d0d3f-335">`IHtmlHelper`proporciona el `HtmlHelper` métodos `Html.BeginForm` y `Html.ActionLink` para generar `<form>` y `<a>` elementos respectivamente.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-335">`IHtmlHelper` provides the `HtmlHelper` methods `Html.BeginForm` and `Html.ActionLink` to generate `<form>` and `<a>` elements respectively.</span></span> <span data-ttu-id="d0d3f-336">Estos métodos utilizan el `Url.Action` método para generar una dirección URL y aceptan argumentos similares.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-336">These methods use the `Url.Action` method to generate a URL and they accept similar arguments.</span></span> <span data-ttu-id="d0d3f-337">El `Url.RouteUrl` compañeros para `HtmlHelper` son `Html.BeginRouteForm` y `Html.RouteLink` que tienen una funcionalidad similar.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-337">The `Url.RouteUrl` companions for `HtmlHelper` are `Html.BeginRouteForm` and `Html.RouteLink` which have similar functionality.</span></span>

<span data-ttu-id="d0d3f-338">TagHelpers generar direcciones URL a través de la `form` TagHelper y `<a>` TagHelper.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-338">TagHelpers generate URLs through the `form` TagHelper and the `<a>` TagHelper.</span></span> <span data-ttu-id="d0d3f-339">Ambos usan `IUrlHelper` para su implementación.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-339">Both of these use `IUrlHelper` for their implementation.</span></span> <span data-ttu-id="d0d3f-340">Vea [trabajar con formularios](../views/working-with-forms.md) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-340">See [Working with Forms](../views/working-with-forms.md) for more information.</span></span>

<span data-ttu-id="d0d3f-341">Dentro de las vistas, la `IUrlHelper` está disponible a través de la `Url` propiedad para una generación de dirección URL de ad hoc no cubierta por los pasos anteriores.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-341">Inside views, the `IUrlHelper` is available through the `Url` property for any ad-hoc URL generation not covered by the above.</span></span>

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="generating-urls-in-action-results"></a><span data-ttu-id="d0d3f-342">Generar direcciones URL en los resultados de acción</span><span class="sxs-lookup"><span data-stu-id="d0d3f-342">Generating URLS in Action Results</span></span>

<span data-ttu-id="d0d3f-343">Los ejemplos anteriores han demostrado utilizando `IUrlHelper` en un controlador, mientras que el uso de un controlador más común consiste en generar una dirección URL como parte de un resultado de acción.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-343">The examples above have shown using `IUrlHelper` in a controller, while the most common usage in a controller is to generate a URL as part of an action result.</span></span>

<span data-ttu-id="d0d3f-344">El `ControllerBase` y `Controller` clases base proporcionan métodos de conveniencia para los resultados de la acción que hacen referencia a otra acción.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-344">The `ControllerBase` and `Controller` base classes provide convenience methods for action results that reference another action.</span></span> <span data-ttu-id="d0d3f-345">Un uso típico es redirigir después de Aceptar proporcionados por el usuario.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-345">One typical usage is to redirect after accepting user input.</span></span>

```csharp
public Task<IActionResult> Edit(int id, Customer customer)
{
    if (ModelState.IsValid)
    {
        // Update DB with new details.
        return RedirectToAction("Index");
    }
}
```

<span data-ttu-id="d0d3f-346">Los métodos de generador de resultados de acción siguen un patrón similar a los métodos `IUrlHelper`.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-346">The action results factory methods follow a similar pattern to the methods on `IUrlHelper`.</span></span>

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a><span data-ttu-id="d0d3f-347">Caso especial para rutas convencionales dedicadas</span><span class="sxs-lookup"><span data-stu-id="d0d3f-347">Special case for dedicated conventional routes</span></span>

<span data-ttu-id="d0d3f-348">El enrutamiento convencional puede usar un tipo especial de definición de ruta denominada una *dedicado ruta convencional*.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-348">Conventional routing can use a special kind of route definition called a *dedicated conventional route*.</span></span> <span data-ttu-id="d0d3f-349">En el ejemplo siguiente, la ruta con el nombre `blog` es una ruta convencional dedicada.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-349">In the example below, the route named `blog` is a dedicated conventional route.</span></span>

```csharp
app.UseMvc(routes =>
{
    routes.MapRoute("blog", "blog/{*article}",
        defaults: new { controller = "Blog", action = "Article" });
    routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="d0d3f-350">¿Con estas definiciones de route, `Url.Action("Index", "Home")` generará la ruta de acceso de dirección URL `/` con el `default` ruta, pero por qué?</span><span class="sxs-lookup"><span data-stu-id="d0d3f-350">Using these route definitions, `Url.Action("Index", "Home")` will generate the URL path `/` with the `default` route, but why?</span></span> <span data-ttu-id="d0d3f-351">Se pueden suponer que los valores de ruta `{ controller = Home, action = Index }` sería suficiente para generar una dirección URL utilizar `blog`, y el resultado sería `/blog?action=Index&controller=Home`.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-351">You might guess the route values `{ controller = Home, action = Index }` would be enough to generate a URL using `blog`, and the result would be `/blog?action=Index&controller=Home`.</span></span>

<span data-ttu-id="d0d3f-352">Rutas convencionales dedicadas que se basan en un comportamiento especial de los valores predeterminados que no tienen un parámetro de ruta correspondiente que impide que la ruta se "demasiado expansiva" con la generación de direcciones URL.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-352">Dedicated conventional routes rely on a special behavior of default values that don't have a corresponding route parameter that prevents the route from being "too greedy" with URL generation.</span></span> <span data-ttu-id="d0d3f-353">En este caso, los valores predeterminados son `{ controller = Blog, action = Article }`y ni `controller` ni `action` aparece como un parámetro de ruta.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-353">In this case the default values are `{ controller = Blog, action = Article }`, and neither `controller` nor `action` appears as a route parameter.</span></span> <span data-ttu-id="d0d3f-354">Cuando el enrutamiento realiza la generación de direcciones URL, los valores proporcionados deben coincidir con los valores predeterminados.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-354">When routing performs URL generation, the values provided must match the default values.</span></span> <span data-ttu-id="d0d3f-355">Generación de dirección URL mediante `blog` se producirá un error porque los valores `{ controller = Home, action = Index }` no coincide con `{ controller = Blog, action = Article }`.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-355">URL generation using `blog` will fail because the values `{ controller = Home, action = Index }` don't match `{ controller = Blog, action = Article }`.</span></span> <span data-ttu-id="d0d3f-356">Enrutamiento, a continuación, vuelve para probar `default`, que se realiza correctamente.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-356">Routing then falls back to try `default`, which succeeds.</span></span>

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a><span data-ttu-id="d0d3f-357">Áreas</span><span class="sxs-lookup"><span data-stu-id="d0d3f-357">Areas</span></span>

<span data-ttu-id="d0d3f-358">[Áreas](areas.md) son una característica MVC que se usan para organizar funcionalidad relacionada en un grupo como un enrutamiento-espacio de nombres independiente (para las acciones de controlador) y la estructura de carpetas (para vistas).</span><span class="sxs-lookup"><span data-stu-id="d0d3f-358">[Areas](areas.md) are an MVC feature used to organize related functionality into a group as a separate routing-namespace (for controller actions) and folder structure (for views).</span></span> <span data-ttu-id="d0d3f-359">Usar áreas de permite que una aplicación que tiene varios controladores con el mismo nombre - siempre que tienen diferentes *áreas*.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-359">Using areas allows an application to have multiple controllers with the same name - as long as they have different *areas*.</span></span> <span data-ttu-id="d0d3f-360">El uso de áreas crea una jerarquía con el fin de enrutamiento mediante la adición de otro parámetro de ruta, `area` a `controller` y `action`.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-360">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area` to `controller` and `action`.</span></span> <span data-ttu-id="d0d3f-361">En esta sección veremos cómo enrutamiento interactúa con áreas - vea [áreas](areas.md) para obtener más información sobre cómo se utilizan las áreas con las vistas.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-361">This section will discuss how routing interacts with areas - see [Areas](areas.md) for details about how areas are used with views.</span></span>

<span data-ttu-id="d0d3f-362">En el ejemplo siguiente se configura MVC para usar la ruta predeterminada convencional y un *ruta de área* para un área denominada `Blog`:</span><span class="sxs-lookup"><span data-stu-id="d0d3f-362">The following example configures MVC to use the default conventional route and an *area route* for an area named `Blog`:</span></span>

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet1)]

<span data-ttu-id="d0d3f-363">Cuando coincide con una ruta de acceso de dirección URL como `/Manage/Users/AddUser`, la primera ruta generará los valores de ruta `{ area = Blog, controller = Users, action = AddUser }`.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-363">When matching a URL path like `/Manage/Users/AddUser`, the first route will produce the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span> <span data-ttu-id="d0d3f-364">El `area` valor de ruta se genera con un valor predeterminado para `area`, de hecho la ruta creada por `MapAreaRoute` es equivalente a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="d0d3f-364">The `area` route value is produced by a default value for `area`, in fact the route created by `MapAreaRoute` is equivalent to the following:</span></span>

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet2)]

<span data-ttu-id="d0d3f-365">`MapAreaRoute`crea una ruta con un valor predeterminado y la restricción para `area` utilizando el nombre del área proporcionados, en este caso `Blog`.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-365">`MapAreaRoute` creates a route using both a default value and constraint for `area` using the provided area name, in this case `Blog`.</span></span> <span data-ttu-id="d0d3f-366">El valor predeterminado garantiza que siempre produce la ruta `{ area = Blog, ... }`, la restricción requiere que el valor `{ area = Blog, ... }` para la generación de dirección URL.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-366">The default value ensures that the route always produces `{ area = Blog, ... }`, the constraint requires the value `{ area = Blog, ... }` for URL generation.</span></span>

> [!TIP]
> <span data-ttu-id="d0d3f-367">El enrutamiento convencional depende del orden.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-367">Conventional routing is order-dependent.</span></span> <span data-ttu-id="d0d3f-368">En general, las rutas con áreas deben colocarse anteriormente en la tabla de rutas tal como están más específicos que las rutas sin un área.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-368">In general, routes with areas should be placed earlier in the route table as they are more specific than routes without an area.</span></span>

<span data-ttu-id="d0d3f-369">En el ejemplo anterior, los valores de ruta coincidiría con la acción siguiente:</span><span class="sxs-lookup"><span data-stu-id="d0d3f-369">Using the above example, the route values would match the following action:</span></span>

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

<span data-ttu-id="d0d3f-370">El `AreaAttribute` es lo que denota un controlador como parte de un área, se dice que este controlador está en la `Blog` área.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-370">The `AreaAttribute` is what denotes a controller as part of an area, we say that this controller is in the `Blog` area.</span></span> <span data-ttu-id="d0d3f-371">Controladores sin un `[Area]` atributo no son miembros de cualquier área y le **no** coincide con cuando el `area` ruta valor lo proporciona el enrutamiento.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-371">Controllers without an `[Area]` attribute are not members of any area, and will **not** match when the `area` route value is provided by routing.</span></span> <span data-ttu-id="d0d3f-372">En el ejemplo siguiente, solo el primer controlador aparecen puede coincidir con los valores de ruta `{ area = Blog, controller = Users, action = AddUser }`.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-372">In the following example, only the first controller listed can match the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span>

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[Main](routing/sample/AreasRouting/Controllers/UsersController.cs)]

> [!NOTE]
> <span data-ttu-id="d0d3f-373">Aquí se muestra el espacio de nombres de cada controlador por integridad; en caso contrario, los controladores tendría una nomenclatura entran en conflicto y generar un error del compilador.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-373">The namespace of each controller is shown here for completeness - otherwise the controllers would have a naming conflict and generate a compiler error.</span></span> <span data-ttu-id="d0d3f-374">Espacios de nombres de clase no tienen ningún efecto en el enrutamiento de MVC.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-374">Class namespaces have no effect on MVC's routing.</span></span>

<span data-ttu-id="d0d3f-375">Los dos primeros controladores son miembros de las áreas y coincida solo cuando su nombre de área respectiva se proporciona por el `area` enrutar valor.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-375">The first two controllers are members of areas, and only match when their respective area name is provided by the `area` route value.</span></span> <span data-ttu-id="d0d3f-376">El controlador de terceros no es un miembro de cualquier área y puede única coincidencia cuando ningún valor para `area` proporciona el enrutamiento.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-376">The third controller is not a member of any area, and can only match when no value for `area` is provided by routing.</span></span>

> [!NOTE]
> <span data-ttu-id="d0d3f-377">En términos de búsqueda de coincidencias *ningún valor*, la ausencia de la `area` tiene el mismo valor como si el valor de `area` eran null o una cadena vacía.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-377">In terms of matching *no value*, the absence of the `area` value is the same as if the value for `area` were null or the empty string.</span></span>

<span data-ttu-id="d0d3f-378">Al ejecutar una acción en un área, la ruta valor `area` estarán disponibles como un *valor ambiente* para el enrutamiento para que utilice para la generación de direcciones URL.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-378">When executing an action inside an area, the route value for `area` will be available as an *ambient value* for routing to use for URL generation.</span></span> <span data-ttu-id="d0d3f-379">Esto significa que de forma predeterminada las áreas actúen *rápidas* para la generación de dirección URL como se muestra en el ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-379">This means that by default areas act *sticky* for URL generation as demonstrated by the following sample.</span></span>

[!code-csharp[Main](routing/sample/AreasRouting/Startup.cs?name=snippet3)]

[!code-csharp[Main](routing/sample/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<a name="iactionconstraint-ref-label"></a>

## <a name="understanding-iactionconstraint"></a><span data-ttu-id="d0d3f-380">Descripción IActionConstraint</span><span class="sxs-lookup"><span data-stu-id="d0d3f-380">Understanding IActionConstraint</span></span>

> [!NOTE]
> <span data-ttu-id="d0d3f-381">Esta sección es un análisis más profundo en funcionamiento interno de framework y cómo MVC elige una acción que se va a ejecutar.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-381">This section is a deep-dive on framework internals and how MVC chooses an action to execute.</span></span> <span data-ttu-id="d0d3f-382">Una aplicación típica no tendrá un personalizado`IActionConstraint`</span><span class="sxs-lookup"><span data-stu-id="d0d3f-382">A typical application won't need a custom `IActionConstraint`</span></span>

<span data-ttu-id="d0d3f-383">Es probable que ya haya usado `IActionConstraint` incluso si no está familiarizado con la interfaz.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-383">You have likely already used `IActionConstraint` even if you're not familiar with the interface.</span></span> <span data-ttu-id="d0d3f-384">El `[HttpGet]` atributo y similares `[Http-VERB]` atributos implementan `IActionConstraint` con el fin de limitar la ejecución de un método de acción.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-384">The `[HttpGet]` Attribute and similar `[Http-VERB]` attributes implement `IActionConstraint` in order to limit the execution of an action method.</span></span>

```csharp
public class ProductsController : Controller
{
    [HttpGet]
    public IActionResult Edit() { }

    public IActionResult Edit(...) { }
}
```

<span data-ttu-id="d0d3f-385">Suponiendo que la ruta convencional de forma predeterminada, la ruta de acceso de dirección URL `/Products/Edit` generaría los valores `{ controller = Products, action = Edit }`, que coincidiría con **ambos** de las acciones que se muestra aquí.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-385">Assuming the default conventional route, the URL path `/Products/Edit` would produce the values `{ controller = Products, action = Edit }`, which would match **both** of the actions shown here.</span></span> <span data-ttu-id="d0d3f-386">En `IActionConstraint` terminología haría decimos que ambas de estas acciones se consideran candidatos - cuando coincidan con los datos de ruta.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-386">In `IActionConstraint` terminology we would say that both of these actions are considered candidates - as they both match the route data.</span></span>

<span data-ttu-id="d0d3f-387">Cuando el `HttpGetAttribute` se ejecuta, lo dirá que *Edit()* es una coincidencia para *obtener* y no es una coincidencia para cualquier otro verbo HTTP.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-387">When the `HttpGetAttribute` executes, it will say that *Edit()* is a match for *GET* and is not a match for any other HTTP verb.</span></span> <span data-ttu-id="d0d3f-388">El `Edit(...)` acción no tiene ninguna restricción definida y, por lo que coincidirá con cualquier verbo HTTP.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-388">The `Edit(...)` action doesn't have any constraints defined, and so will match any HTTP verb.</span></span> <span data-ttu-id="d0d3f-389">Por lo que suponiendo un `POST` : solo `Edit(...)` coincide con.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-389">So assuming a `POST` - only `Edit(...)` matches.</span></span> <span data-ttu-id="d0d3f-390">Sin embargo, para un `GET` ambas acciones pueden coincidir con los-sin embargo, una acción con un `IActionConstraint` siempre se considera *mejor* que una acción sin.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-390">But, for a `GET` both actions can still match - however, an action with an `IActionConstraint` is always considered *better* than an action without.</span></span> <span data-ttu-id="d0d3f-391">Por lo que ya `Edit()` tiene `[HttpGet]` se considera más específica y se seleccionará si pueden hacer coincidir ambas acciones.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-391">So because `Edit()` has `[HttpGet]` it is considered more specific, and will be selected if both actions can match.</span></span>

<span data-ttu-id="d0d3f-392">Conceptualmente, `IActionConstraint` es una forma de *sobrecarga*, pero en lugar de la sobrecarga de métodos con el mismo nombre, se sobrecarga entre las acciones que coinciden con la misma dirección URL.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-392">Conceptually, `IActionConstraint` is a form of *overloading*, but instead of overloading methods with the same name, it is overloading between actions that match the same URL.</span></span> <span data-ttu-id="d0d3f-393">Ruta de atributo también utiliza `IActionConstraint` y puede dar lugar a acciones de diferentes controladores ambos están considerando candidatos.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-393">Attribute routing also uses `IActionConstraint` and can result in actions from different controllers both being considered candidates.</span></span>

<a name="iactionconstraint-impl-ref-label"></a>

### <a name="implementing-iactionconstraint"></a><span data-ttu-id="d0d3f-394">Implementar IActionConstraint</span><span class="sxs-lookup"><span data-stu-id="d0d3f-394">Implementing IActionConstraint</span></span>

<span data-ttu-id="d0d3f-395">La manera más sencilla de implementar un `IActionConstraint` consiste en crear una clase derivada de `System.Attribute` y lo coloca en las acciones y los controladores.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-395">The simplest way to implement an `IActionConstraint` is to create a class derived from `System.Attribute` and place it on your actions and controllers.</span></span> <span data-ttu-id="d0d3f-396">MVC detecta automáticamente cualquier `IActionConstraint` que se aplican como atributos.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-396">MVC will automatically discover any `IActionConstraint` that are applied as attributes.</span></span> <span data-ttu-id="d0d3f-397">Puede usar el modelo de aplicaciones para aplicar restricciones y se trata probablemente el enfoque más flexible puesto que permite a metaprogram cómo se aplican.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-397">You can use the application model to apply constraints, and this is probably the most flexible approach as it allows you to metaprogram how they are applied.</span></span>

<span data-ttu-id="d0d3f-398">En el ejemplo siguiente, una restricción elige una acción según un *código de país* de los datos de ruta.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-398">In the following example a constraint chooses an action based on a *country code* from the route data.</span></span> <span data-ttu-id="d0d3f-399">El [ejemplo completo en GitHub](https://github.com/aspnet/Entropy/blob/dev/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).</span><span class="sxs-lookup"><span data-stu-id="d0d3f-399">The [full sample on GitHub](https://github.com/aspnet/Entropy/blob/dev/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).</span></span>

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

<span data-ttu-id="d0d3f-400">Usted es responsable de implementar la `Accept` método y elija un 'pedido' para la restricción para ejecutar.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-400">You are responsible for implementing the `Accept` method and choosing an 'Order' for the constraint to execute.</span></span> <span data-ttu-id="d0d3f-401">En este caso, el `Accept` método `true` para denotar la acción es una coincidencia cuando el `country` enrutar coincidencias de valor.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-401">In this case, the `Accept` method returns `true` to denote the action is a match when the `country` route value matches.</span></span> <span data-ttu-id="d0d3f-402">Esto es diferente de un `RouteValueAttribute` en que se permite el retroceso a una acción sin atributos.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-402">This is different from a `RouteValueAttribute` in that it allows fallback to a non-attributed action.</span></span> <span data-ttu-id="d0d3f-403">El ejemplo muestra que si se define un `en-US` acción, a continuación, un código de país como `fr-FR` recurra a un controlador más genérico que no tiene `[CountrySpecific(...)]` aplicado.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-403">The sample shows that if you define an `en-US` action then a country code like `fr-FR` will fall back to a more generic controller that does not have `[CountrySpecific(...)]` applied.</span></span>

<span data-ttu-id="d0d3f-404">El `Order` propiedad decide en qué *fase* la restricción es parte de.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-404">The `Order` property decides which *stage* the constraint is part of.</span></span> <span data-ttu-id="d0d3f-405">Restricciones de acciones que se ejecutan en grupos basados en la `Order`.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-405">Action constraints run in groups based on the `Order`.</span></span> <span data-ttu-id="d0d3f-406">Por ejemplo, todo el marco de trabajo proporciona los atributos de métodos HTTP utilizan el mismo `Order` valor para que se ejecutan en la misma fase.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-406">For example, all of the framework provided HTTP method attributes use the same `Order` value so that they run in the same stage.</span></span> <span data-ttu-id="d0d3f-407">Puede tener tantas fases según sea necesario implementar las directivas deseadas.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-407">You can have as many stages as you need to implement your desired policies.</span></span>

> [!TIP]
> <span data-ttu-id="d0d3f-408">Para decidir cuál es un valor para `Order` piense o no se debe aplicar la restricción antes que los métodos HTTP.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-408">To decide on a value for `Order` think about whether or not your constraint should be applied before HTTP methods.</span></span> <span data-ttu-id="d0d3f-409">Los números más bajos se ejecutan primeros.</span><span class="sxs-lookup"><span data-stu-id="d0d3f-409">Lower numbers run first.</span></span>
