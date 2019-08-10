---
title: Convenciones de aplicación y de ruta de páginas de Razor en ASP.NET Core
author: guardrex
description: Vea cómo las convenciones de proveedor de modelos de aplicación y de ruta sirven para controlar el enrutamiento, la detección y el procesamiento de páginas.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/08/2019
uid: razor-pages/razor-pages-conventions
ms.openlocfilehash: c93f169c422d260f738faba4812861521f383e51
ms.sourcegitcommit: 776367717e990bdd600cb3c9148ffb905d56862d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2019
ms.locfileid: "68914976"
---
# <a name="razor-pages-route-and-app-conventions-in-aspnet-core"></a><span data-ttu-id="f99f1-103">Convenciones de aplicación y de ruta de páginas de Razor en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f99f1-103">Razor Pages route and app conventions in ASP.NET Core</span></span>

<span data-ttu-id="f99f1-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f99f1-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="f99f1-105">Aquí encontrará información para saber cómo usar las [convenciones de proveedor de modelos de aplicación y de ruta](xref:mvc/controllers/application-model#conventions) con objeto de controlar el enrutamiento, la detección y el procesamiento en aplicaciones de páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="f99f1-105">Learn how to use page [route and app model provider conventions](xref:mvc/controllers/application-model#conventions) to control page routing, discovery, and processing in Razor Pages apps.</span></span>

<span data-ttu-id="f99f1-106">Si necesita configurar rutas de una página personalizadas para páginas concretas, configure el enrutamiento a esas páginas con la [convención AddPageRoute](#configure-a-page-route), descrita más adelante en este tema.</span><span class="sxs-lookup"><span data-stu-id="f99f1-106">When you need to configure custom page routes for individual pages, configure routing to pages with the [AddPageRoute convention](#configure-a-page-route) described later in this topic.</span></span>

<span data-ttu-id="f99f1-107">Para especificar una ruta de página, agregar segmentos de ruta o agregar parámetros a una ruta, use la directiva `@page` de la página.</span><span class="sxs-lookup"><span data-stu-id="f99f1-107">To specify a page route, add route segments, or add parameters to a route, use the page's `@page` directive.</span></span> <span data-ttu-id="f99f1-108">Para obtener más información, vea [rutas personalizadas](xref:razor-pages/index#custom-routes).</span><span class="sxs-lookup"><span data-stu-id="f99f1-108">For more information, see [Custom routes](xref:razor-pages/index#custom-routes).</span></span>

<span data-ttu-id="f99f1-109">Hay palabras reservadas que no se pueden usar como segmentos de ruta o nombres de parámetro.</span><span class="sxs-lookup"><span data-stu-id="f99f1-109">There are reserved words that can't be used as route segments or parameter names.</span></span> <span data-ttu-id="f99f1-110">Para obtener más información, [consulte enrutamiento: Nombres](xref:fundamentals/routing#reserved-routing-names)de enrutamiento reservados.</span><span class="sxs-lookup"><span data-stu-id="f99f1-110">For more information, see [Routing: Reserved routing names](xref:fundamentals/routing#reserved-routing-names).</span></span>

<span data-ttu-id="f99f1-111">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f99f1-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

| <span data-ttu-id="f99f1-112">Escenario</span><span class="sxs-lookup"><span data-stu-id="f99f1-112">Scenario</span></span> | <span data-ttu-id="f99f1-113">El ejemplo explica cómo...</span><span class="sxs-lookup"><span data-stu-id="f99f1-113">The sample demonstrates ...</span></span> |
| -------- | --------------------------- |
| [<span data-ttu-id="f99f1-114">Convenciones de modelo</span><span class="sxs-lookup"><span data-stu-id="f99f1-114">Model conventions</span></span>](#model-conventions)<br><br><span data-ttu-id="f99f1-115">Conventions.Add</span><span class="sxs-lookup"><span data-stu-id="f99f1-115">Conventions.Add</span></span><ul><li><span data-ttu-id="f99f1-116">IPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="f99f1-116">IPageRouteModelConvention</span></span></li><li><span data-ttu-id="f99f1-117">IPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="f99f1-117">IPageApplicationModelConvention</span></span></li><li><span data-ttu-id="f99f1-118">IPageHandlerModelConvention</span><span class="sxs-lookup"><span data-stu-id="f99f1-118">IPageHandlerModelConvention</span></span></li></ul> | <span data-ttu-id="f99f1-119">Agregar un encabezado y una plantilla de ruta a las páginas de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="f99f1-119">Add a route template and header to an app's pages.</span></span> |
| [<span data-ttu-id="f99f1-120">Convenciones de acción de ruta de página</span><span class="sxs-lookup"><span data-stu-id="f99f1-120">Page route action conventions</span></span>](#page-route-action-conventions)<ul><li><span data-ttu-id="f99f1-121">AddFolderRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="f99f1-121">AddFolderRouteModelConvention</span></span></li><li><span data-ttu-id="f99f1-122">AddPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="f99f1-122">AddPageRouteModelConvention</span></span></li><li><span data-ttu-id="f99f1-123">AddPageRoute</span><span class="sxs-lookup"><span data-stu-id="f99f1-123">AddPageRoute</span></span></li></ul> | <span data-ttu-id="f99f1-124">Agregar una plantilla de ruta a las páginas de una carpeta y a una sola página.</span><span class="sxs-lookup"><span data-stu-id="f99f1-124">Add a route template to pages in a folder and to a single page.</span></span> |
| [<span data-ttu-id="f99f1-125">Convenciones de acción del modelo de página</span><span class="sxs-lookup"><span data-stu-id="f99f1-125">Page model action conventions</span></span>](#page-model-action-conventions)<ul><li><span data-ttu-id="f99f1-126">AddFolderApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="f99f1-126">AddFolderApplicationModelConvention</span></span></li><li><span data-ttu-id="f99f1-127">AddPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="f99f1-127">AddPageApplicationModelConvention</span></span></li><li><span data-ttu-id="f99f1-128">ConfigureFilter (clase de filtro, expresión lambda o fábrica de filtros)</span><span class="sxs-lookup"><span data-stu-id="f99f1-128">ConfigureFilter (filter class, lambda expression, or filter factory)</span></span></li></ul> | <span data-ttu-id="f99f1-129">Agregar un encabezado a las páginas de una carpeta, agregar un encabezado a una sola página y configurar una [fábrica de filtros](xref:mvc/controllers/filters#ifilterfactory) para agregar un encabezado a las páginas de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f99f1-129">Add a header to pages in a folder, add a header to a single page, and configure a [filter factory](xref:mvc/controllers/filters#ifilterfactory) to add a header to an app's pages.</span></span> |

<span data-ttu-id="f99f1-130">Razor pages convenciones se agregan y configuran mediante <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> el <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> método de extensión en la colección `Startup` de servicios de la clase.</span><span class="sxs-lookup"><span data-stu-id="f99f1-130">Razor Pages conventions are added and configured using the <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> extension method to <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> on the service collection in the `Startup` class.</span></span> <span data-ttu-id="f99f1-131">Más avanzado el tema veremos los siguientes ejemplos de convención:</span><span class="sxs-lookup"><span data-stu-id="f99f1-131">The following convention examples are explained later in this topic:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddRazorPagesOptions(options =>
            {
                options.Conventions.Add( ... );
                options.Conventions.AddFolderRouteModelConvention(
                    "/OtherPages", model => { ... });
                options.Conventions.AddPageRouteModelConvention(
                    "/About", model => { ... });
                options.Conventions.AddPageRoute(
                    "/Contact", "TheContactPage/{text?}");
                options.Conventions.AddFolderApplicationModelConvention(
                    "/OtherPages", model => { ... });
                options.Conventions.AddPageApplicationModelConvention(
                    "/About", model => { ... });
                options.Conventions.ConfigureFilter(model => { ... });
                options.Conventions.ConfigureFilter( ... );
            });
}
```

## <a name="route-order"></a><span data-ttu-id="f99f1-132">Orden de ruta</span><span class="sxs-lookup"><span data-stu-id="f99f1-132">Route order</span></span>

<span data-ttu-id="f99f1-133">Las rutas especifican un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> para el procesamiento (coincidencia de rutas).</span><span class="sxs-lookup"><span data-stu-id="f99f1-133">Routes specify an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> for processing (route matching).</span></span>

| <span data-ttu-id="f99f1-134">Ordenar</span><span class="sxs-lookup"><span data-stu-id="f99f1-134">Order</span></span>            | <span data-ttu-id="f99f1-135">Comportamiento</span><span class="sxs-lookup"><span data-stu-id="f99f1-135">Behavior</span></span> |
| :--------------: | -------- |
| <span data-ttu-id="f99f1-136">-1</span><span class="sxs-lookup"><span data-stu-id="f99f1-136">-1</span></span>               | <span data-ttu-id="f99f1-137">La ruta se procesa antes de que se procesen otras rutas.</span><span class="sxs-lookup"><span data-stu-id="f99f1-137">The route is processed before other routes are processed.</span></span> |
| <span data-ttu-id="f99f1-138">0</span><span class="sxs-lookup"><span data-stu-id="f99f1-138">0</span></span>                | <span data-ttu-id="f99f1-139">No se ha especificado el orden (valor predeterminado).</span><span class="sxs-lookup"><span data-stu-id="f99f1-139">Order isn't specified (default value).</span></span> <span data-ttu-id="f99f1-140">Si no se asigna `Order` (`Order = null`), el valor predeterminado `Order` de la ruta es 0 (cero) para el procesamiento.</span><span class="sxs-lookup"><span data-stu-id="f99f1-140">Not assigning `Order` (`Order = null`) defaults the route `Order` to 0 (zero) for processing.</span></span> |
| <span data-ttu-id="f99f1-141">1, 2, &hellip; n</span><span class="sxs-lookup"><span data-stu-id="f99f1-141">1, 2, &hellip; n</span></span> | <span data-ttu-id="f99f1-142">Especifica el orden de procesamiento de la ruta.</span><span class="sxs-lookup"><span data-stu-id="f99f1-142">Specifies the route processing order.</span></span> |

<span data-ttu-id="f99f1-143">El procesamiento de rutas se establece por Convención:</span><span class="sxs-lookup"><span data-stu-id="f99f1-143">Route processing is established by convention:</span></span>

* <span data-ttu-id="f99f1-144">Las rutas se procesan en orden secuencial (-1, 0, 1, &hellip; 2, n).</span><span class="sxs-lookup"><span data-stu-id="f99f1-144">Routes are processed in sequential order (-1, 0, 1, 2, &hellip; n).</span></span>
* <span data-ttu-id="f99f1-145">Cuando las rutas tienen el `Order`mismo, la ruta más específica se compara primero seguida por rutas menos específicas.</span><span class="sxs-lookup"><span data-stu-id="f99f1-145">When routes have the same `Order`, the most specific route is matched first followed by less specific routes.</span></span>
* <span data-ttu-id="f99f1-146">Cuando las rutas con el `Order` mismo número de parámetros coinciden con una dirección URL de solicitud, las rutas se procesan en el orden en que <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>se agregan a.</span><span class="sxs-lookup"><span data-stu-id="f99f1-146">When routes with the same `Order` and the same number of parameters match a request URL, routes are processed in the order that they're added to the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span></span>

<span data-ttu-id="f99f1-147">Si es posible, evite depender de un orden de procesamiento de rutas establecido.</span><span class="sxs-lookup"><span data-stu-id="f99f1-147">If possible, avoid depending on an established route processing order.</span></span> <span data-ttu-id="f99f1-148">Por lo general, el enrutamiento selecciona la ruta correcta con coincidencia de dirección URL.</span><span class="sxs-lookup"><span data-stu-id="f99f1-148">Generally, routing selects the correct route with URL matching.</span></span> <span data-ttu-id="f99f1-149">Si debe establecer las propiedades `Order` de ruta para enrutar las solicitudes correctamente, es probable que el esquema de enrutamiento de la aplicación sea confuso para los clientes y que el mantenimiento sea frágil.</span><span class="sxs-lookup"><span data-stu-id="f99f1-149">If you must set route `Order` properties to route requests correctly, the app's routing scheme is probably confusing to clients and fragile to maintain.</span></span> <span data-ttu-id="f99f1-150">Busque para simplificar el esquema de enrutamiento de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f99f1-150">Seek to simplify the app's routing scheme.</span></span> <span data-ttu-id="f99f1-151">La aplicación de ejemplo requiere un orden de procesamiento de rutas explícito para mostrar varios escenarios de enrutamiento con una sola aplicación.</span><span class="sxs-lookup"><span data-stu-id="f99f1-151">The sample app requires an explicit route processing order to demonstrate several routing scenarios using a single app.</span></span> <span data-ttu-id="f99f1-152">Sin embargo, debe intentar evitar la práctica de establecer la ruta `Order` en las aplicaciones de producción.</span><span class="sxs-lookup"><span data-stu-id="f99f1-152">However, you should attempt to avoid the practice of setting route `Order` in production apps.</span></span>

<span data-ttu-id="f99f1-153">El enrutamiento del controlador de MVC y el enrutamiento de Razor Pages comparten una implementación.</span><span class="sxs-lookup"><span data-stu-id="f99f1-153">Razor Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="f99f1-154">La información sobre el orden de las rutas en los temas [de MVC está disponible en enrutamiento a las acciones del controlador: Ordenar rutas](xref:mvc/controllers/routing#ordering-attribute-routes)de atributo.</span><span class="sxs-lookup"><span data-stu-id="f99f1-154">Information on route order in the MVC topics is available at [Routing to controller actions: Ordering attribute routes](xref:mvc/controllers/routing#ordering-attribute-routes).</span></span>

## <a name="model-conventions"></a><span data-ttu-id="f99f1-155">Convenciones de modelo</span><span class="sxs-lookup"><span data-stu-id="f99f1-155">Model conventions</span></span>

<span data-ttu-id="f99f1-156">Agregue un delegado para <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> para agregar [convenciones de modelo](xref:mvc/controllers/application-model#conventions) que se aplican a Razor pages.</span><span class="sxs-lookup"><span data-stu-id="f99f1-156">Add a delegate for <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> to add [model conventions](xref:mvc/controllers/application-model#conventions) that apply to Razor Pages.</span></span>

### <a name="add-a-route-model-convention-to-all-pages"></a><span data-ttu-id="f99f1-157">Agregar una Convención de modelo de ruta a todas las páginas</span><span class="sxs-lookup"><span data-stu-id="f99f1-157">Add a route model convention to all pages</span></span>

<span data-ttu-id="f99f1-158">Utilice <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> para crear y <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> agregar a la colección de instancias <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> de que se aplican durante la construcción del modelo de ruta de página.</span><span class="sxs-lookup"><span data-stu-id="f99f1-158">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page route model construction.</span></span>

<span data-ttu-id="f99f1-159">En la aplicación de ejemplo se agrega una plantilla de ruta `{globalTemplate?}` a todas las páginas de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="f99f1-159">The sample app adds a `{globalTemplate?}` route template to all of the pages in the app:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="f99f1-160">La propiedad <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> de <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> está establecida en `1`.</span><span class="sxs-lookup"><span data-stu-id="f99f1-160">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `1`.</span></span> <span data-ttu-id="f99f1-161">Esto garantiza el siguiente comportamiento de coincidencia de rutas en la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="f99f1-161">This ensures the following route matching behavior in the sample app:</span></span>

* <span data-ttu-id="f99f1-162">Más adelante en el `TheContactPage/{text?}` tema se agrega una plantilla de ruta para.</span><span class="sxs-lookup"><span data-stu-id="f99f1-162">A route template for `TheContactPage/{text?}` is added later in the topic.</span></span> <span data-ttu-id="f99f1-163">La ruta de la página de contacto tiene un `null` orden`Order = 0`predeterminado de (), por lo `{globalTemplate?}` que coincide antes de la plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="f99f1-163">The Contact Page route has a default order of `null` (`Order = 0`), so it matches before the `{globalTemplate?}` route template.</span></span>
* <span data-ttu-id="f99f1-164">Más `{aboutTemplate?}` adelante en el tema se agrega una plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="f99f1-164">An `{aboutTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="f99f1-165">La plantilla `{aboutTemplate?}` tiene un `Order` con un valor de `2`.</span><span class="sxs-lookup"><span data-stu-id="f99f1-165">The `{aboutTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="f99f1-166">Cuando la página About se solicita en `/About/RouteDataValue`, "RouteDataValue" se carga en `RouteData.Values["globalTemplate"]` (`Order = 1`) y no `RouteData.Values["aboutTemplate"]` (`Order = 2`), debido a la configuración de la propiedad `Order`.</span><span class="sxs-lookup"><span data-stu-id="f99f1-166">When the About page is requested at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>
* <span data-ttu-id="f99f1-167">Más `{otherPagesTemplate?}` adelante en el tema se agrega una plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="f99f1-167">An `{otherPagesTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="f99f1-168">La plantilla `{otherPagesTemplate?}` tiene un `Order` con un valor de `2`.</span><span class="sxs-lookup"><span data-stu-id="f99f1-168">The `{otherPagesTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="f99f1-169">Cuando se solicita una página de la carpeta *pages/OtherPages* con un parámetro de ruta (por `/OtherPages/Page1/RouteDataValue`ejemplo,), "RouteDataValue" se `RouteData.Values["globalTemplate"]` carga`Order = 1`en () `RouteData.Values["otherPagesTemplate"]` y`Order = 2`no () debido a la configuración del parámetro`Order` propiedad.</span><span class="sxs-lookup"><span data-stu-id="f99f1-169">When any page in the *Pages/OtherPages* folder is requested with a route parameter (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="f99f1-170">Siempre que sea posible, no `Order`establezca, lo que `Order = 0`da como resultado.</span><span class="sxs-lookup"><span data-stu-id="f99f1-170">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="f99f1-171">Confíe en el enrutamiento para seleccionar la ruta correcta.</span><span class="sxs-lookup"><span data-stu-id="f99f1-171">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="f99f1-172">Razor pages opciones, como agregar <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, se agregan cuando MVC se agrega a la colección de servicios en. `Startup.ConfigureServices`</span><span class="sxs-lookup"><span data-stu-id="f99f1-172">Razor Pages options, such as adding <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, are added when MVC is added to the service collection in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="f99f1-173">Para obtener un ejemplo, vea la [aplicación de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span><span class="sxs-lookup"><span data-stu-id="f99f1-173">For an example, see the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="f99f1-174">Solicite la página About del ejemplo en `localhost:5000/About/GlobalRouteValue` y revise el resultado:</span><span class="sxs-lookup"><span data-stu-id="f99f1-174">Request the sample's About page at `localhost:5000/About/GlobalRouteValue` and inspect the result:</span></span>

![La página About se solicita con un segmento de ruta de GlobalRouteValue.](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a><span data-ttu-id="f99f1-177">Agregar una Convención de modelo de aplicación a todas las páginas</span><span class="sxs-lookup"><span data-stu-id="f99f1-177">Add an app model convention to all pages</span></span>

<span data-ttu-id="f99f1-178">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> para crear y agregar un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> a la colección de <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instancias de que se aplican durante la construcción del modelo de aplicación de página.</span><span class="sxs-lookup"><span data-stu-id="f99f1-178">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page app model construction.</span></span>

<span data-ttu-id="f99f1-179">Para demostrar esta y otras convenciones más adelante en este tema, la aplicación de ejemplo incluye una clase `AddHeaderAttribute`.</span><span class="sxs-lookup"><span data-stu-id="f99f1-179">To demonstrate this and other conventions later in the topic, the sample app includes an `AddHeaderAttribute` class.</span></span> <span data-ttu-id="f99f1-180">El constructor de esta clase acepta una cadena `name` y una matriz de cadenas `values`.</span><span class="sxs-lookup"><span data-stu-id="f99f1-180">The class constructor accepts a `name` string and a `values` string array.</span></span> <span data-ttu-id="f99f1-181">Estos valores se usan en su método `OnResultExecuting` para establecer un encabezado de respuesta.</span><span class="sxs-lookup"><span data-stu-id="f99f1-181">These values are used in its `OnResultExecuting` method to set a response header.</span></span> <span data-ttu-id="f99f1-182">La clase completa se muestra en la sección [Convenciones de acción del modelo de página](#page-model-action-conventions) más adelante en este tema.</span><span class="sxs-lookup"><span data-stu-id="f99f1-182">The full class is shown in the [Page model action conventions](#page-model-action-conventions) section later in the topic.</span></span>

<span data-ttu-id="f99f1-183">En la aplicación de ejemplo se usa la clase `AddHeaderAttribute` para agregar un encabezado (`GlobalHeader`) a todas las páginas de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="f99f1-183">The sample app uses the `AddHeaderAttribute` class to add a header, `GlobalHeader`, to all of the pages in the app:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="f99f1-184">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="f99f1-184">*Startup.cs*:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet2)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

::: moniker-end

<span data-ttu-id="f99f1-185">Solicite la página About del ejemplo en `localhost:5000/About` y revise los encabezados para ver el resultado:</span><span class="sxs-lookup"><span data-stu-id="f99f1-185">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Los encabezados de respuesta de la página About ponen de manifiesto que GlobalHeader se ha agregado.](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a><span data-ttu-id="f99f1-187">Agregar una Convención de modelo de controlador a todas las páginas</span><span class="sxs-lookup"><span data-stu-id="f99f1-187">Add a handler model convention to all pages</span></span>

<span data-ttu-id="f99f1-188">Utilice <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> para crear y <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> agregar a la colección de instancias <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> de que se aplican durante la construcción del modelo de controlador de páginas.</span><span class="sxs-lookup"><span data-stu-id="f99f1-188">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page handler model construction.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="f99f1-189">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="f99f1-189">*Startup.cs*:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet10)]

::: moniker-end

## <a name="page-route-action-conventions"></a><span data-ttu-id="f99f1-190">Convenciones de acción de ruta de página</span><span class="sxs-lookup"><span data-stu-id="f99f1-190">Page route action conventions</span></span>

<span data-ttu-id="f99f1-191">El proveedor de modelos de ruta predeterminado que deriva <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> de las convenciones de invocaciones que están diseñadas para proporcionar puntos de extensibilidad para configurar rutas de página.</span><span class="sxs-lookup"><span data-stu-id="f99f1-191">The default route model provider that derives from <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> invokes conventions which are designed to provide extensibility points for configuring page routes.</span></span>

### <a name="folder-route-model-convention"></a><span data-ttu-id="f99f1-192">Convención de modelo de ruta de carpeta</span><span class="sxs-lookup"><span data-stu-id="f99f1-192">Folder route model convention</span></span>

<span data-ttu-id="f99f1-193">Utilice <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> para crear y agregar un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> que invoca una acción en <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> para todas las páginas de la carpeta especificada.</span><span class="sxs-lookup"><span data-stu-id="f99f1-193">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for all of the pages under the specified folder.</span></span>

<span data-ttu-id="f99f1-194">En la aplicación de ejemplo se usa <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> para agregar una plantilla de ruta `{otherPagesTemplate?}` a las páginas de la carpeta *OtherPages*:</span><span class="sxs-lookup"><span data-stu-id="f99f1-194">The sample app uses <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to add an `{otherPagesTemplate?}` route template to the pages in the *OtherPages* folder:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

::: moniker-end

<span data-ttu-id="f99f1-195">La propiedad <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> de <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> está establecida en `2`.</span><span class="sxs-lookup"><span data-stu-id="f99f1-195">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="f99f1-196">Esto garantiza que la plantilla para `{globalTemplate?}` (establecida anteriormente en el tema a `1`) tiene prioridad para la primera posición de valor de datos de ruta cuando se proporciona un valor de ruta único.</span><span class="sxs-lookup"><span data-stu-id="f99f1-196">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="f99f1-197">Si se solicita una página de la carpeta *pages/OtherPages* con un valor de parámetro de ruta ( `/OtherPages/Page1/RouteDataValue`por ejemplo,), "RouteDataValue" `RouteData.Values["globalTemplate"]` se`Order = 1`carga en ( `RouteData.Values["otherPagesTemplate"]` )`Order = 2`y no () debido a la configuración de la `Order` propiedad.</span><span class="sxs-lookup"><span data-stu-id="f99f1-197">If a page in the *Pages/OtherPages* folder is requested with a route parameter value (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="f99f1-198">Siempre que sea posible, no `Order`establezca, lo que `Order = 0`da como resultado.</span><span class="sxs-lookup"><span data-stu-id="f99f1-198">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="f99f1-199">Confíe en el enrutamiento para seleccionar la ruta correcta.</span><span class="sxs-lookup"><span data-stu-id="f99f1-199">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="f99f1-200">Solicite la página Page1 del ejemplo en `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` y revise el resultado:</span><span class="sxs-lookup"><span data-stu-id="f99f1-200">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` and inspect the result:</span></span>

![Page1 en la carpeta OtherPages solicitada con un segmento de ruta de GlobalRouteValue y OtherPagesRouteValue](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a><span data-ttu-id="f99f1-203">Convención de modelo de ruta de página</span><span class="sxs-lookup"><span data-stu-id="f99f1-203">Page route model convention</span></span>

<span data-ttu-id="f99f1-204">Utilice <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> para crear y agregar un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> que invoca una acción en <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> para la página con el nombre especificado.</span><span class="sxs-lookup"><span data-stu-id="f99f1-204">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for the page with the specified name.</span></span>

<span data-ttu-id="f99f1-205">En la aplicación de ejemplo se usa `AddPageRouteModelConvention` para agregar una plantilla de ruta `{aboutTemplate?}` a la página About:</span><span class="sxs-lookup"><span data-stu-id="f99f1-205">The sample app uses `AddPageRouteModelConvention` to add an `{aboutTemplate?}` route template to the About page:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet4)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

::: moniker-end

<span data-ttu-id="f99f1-206">La propiedad <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> de <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> está establecida en `2`.</span><span class="sxs-lookup"><span data-stu-id="f99f1-206">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="f99f1-207">Esto garantiza que la plantilla para `{globalTemplate?}` (establecida anteriormente en el tema a `1`) tiene prioridad para la primera posición de valor de datos de ruta cuando se proporciona un valor de ruta único.</span><span class="sxs-lookup"><span data-stu-id="f99f1-207">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="f99f1-208">Si la página about se solicita con un valor de parámetro de `/About/RouteDataValue`ruta en, "RouteDataValue" se `RouteData.Values["globalTemplate"]` carga`Order = 1`en () `RouteData.Values["aboutTemplate"]` y`Order = 2`no () debido a `Order` la configuración de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="f99f1-208">If the About page is requested with a route parameter value at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="f99f1-209">Siempre que sea posible, no `Order`establezca, lo que `Order = 0`da como resultado.</span><span class="sxs-lookup"><span data-stu-id="f99f1-209">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="f99f1-210">Confíe en el enrutamiento para seleccionar la ruta correcta.</span><span class="sxs-lookup"><span data-stu-id="f99f1-210">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="f99f1-211">Solicite la página About del ejemplo en `localhost:5000/About/GlobalRouteValue/AboutRouteValue` y revise el resultado:</span><span class="sxs-lookup"><span data-stu-id="f99f1-211">Request the sample's About page at `localhost:5000/About/GlobalRouteValue/AboutRouteValue` and inspect the result:</span></span>

![La página About se solicita con los segmentos de ruta de GlobalRouteValue y AboutRouteValue.](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

::: moniker range=">= aspnetcore-2.2"

## <a name="use-a-parameter-transformer-to-customize-page-routes"></a><span data-ttu-id="f99f1-214">Usar un transformador de parámetros para personalizar las rutas de página</span><span class="sxs-lookup"><span data-stu-id="f99f1-214">Use a parameter transformer to customize page routes</span></span>

<span data-ttu-id="f99f1-215">Las rutas de página generadas por ASP.NET Core pueden personalizarse mediante un transformador de parámetros.</span><span class="sxs-lookup"><span data-stu-id="f99f1-215">Page routes generated by ASP.NET Core can be customized using a parameter transformer.</span></span> <span data-ttu-id="f99f1-216">Un transformador de parámetro implementa `IOutboundParameterTransformer` y transforma el valor de parámetros.</span><span class="sxs-lookup"><span data-stu-id="f99f1-216">A parameter transformer implements `IOutboundParameterTransformer` and transforms the value of parameters.</span></span> <span data-ttu-id="f99f1-217">Por ejemplo, un transformador de parámetros `SlugifyParameterTransformer` personalizado cambia el valor de ruta `SubscriptionManagement` a `subscription-management`.</span><span class="sxs-lookup"><span data-stu-id="f99f1-217">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`.</span></span>

<span data-ttu-id="f99f1-218">La `PageRouteTransformerConvention` Convención de modelo de ruta de página aplica un transformador de parámetros a los segmentos de nombre de archivo y carpeta de las rutas de página generadas automáticamente en una aplicación.</span><span class="sxs-lookup"><span data-stu-id="f99f1-218">The `PageRouteTransformerConvention` page route model convention applies a parameter transformer to the folder and file name segments of automatically generated page routes in an app.</span></span> <span data-ttu-id="f99f1-219">Por ejemplo, el archivo Razor pages en */pages/SubscriptionManagement/ViewAll.cshtml* tendría que volver a escribir su ruta `/SubscriptionManagement/ViewAll` de `/subscription-management/view-all`en.</span><span class="sxs-lookup"><span data-stu-id="f99f1-219">For example, the Razor Pages file at */Pages/SubscriptionManagement/ViewAll.cshtml* would have its route rewritten from `/SubscriptionManagement/ViewAll` to `/subscription-management/view-all`.</span></span>

<span data-ttu-id="f99f1-220">`PageRouteTransformerConvention`solo transforma los segmentos generados automáticamente de una ruta de página que proceden de la carpeta Razor Pages y el nombre de archivo.</span><span class="sxs-lookup"><span data-stu-id="f99f1-220">`PageRouteTransformerConvention` only transforms the automatically generated segments of a page route that come from the Razor Pages folder and file name.</span></span> <span data-ttu-id="f99f1-221">No transforma segmentos de ruta agregados con `@page` la Directiva.</span><span class="sxs-lookup"><span data-stu-id="f99f1-221">It doesn't transform route segments added with the `@page` directive.</span></span> <span data-ttu-id="f99f1-222">La Convención tampoco transforma las rutas agregadas <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>por.</span><span class="sxs-lookup"><span data-stu-id="f99f1-222">The convention also doesn't transform routes added by <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>.</span></span>

<span data-ttu-id="f99f1-223">Se registra como una opción en `Startup.ConfigureServices`: `PageRouteTransformerConvention`</span><span class="sxs-lookup"><span data-stu-id="f99f1-223">The `PageRouteTransformerConvention` is registered as an option in `Startup.ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddRazorPagesOptions(options =>
            {
                options.Conventions.Add(
                    new PageRouteTransformerConvention(
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

## <a name="configure-a-page-route"></a><span data-ttu-id="f99f1-224">Configurar una ruta de página</span><span class="sxs-lookup"><span data-stu-id="f99f1-224">Configure a page route</span></span>

<span data-ttu-id="f99f1-225">Use <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> para configurar una ruta a una página en la ruta de acceso de la página especificada.</span><span class="sxs-lookup"><span data-stu-id="f99f1-225">Use <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> to configure a route to a page at the specified page path.</span></span> <span data-ttu-id="f99f1-226">Los vínculos generados que apuntan a esa página usan la ruta especificada.</span><span class="sxs-lookup"><span data-stu-id="f99f1-226">Generated links to the page use your specified route.</span></span> <span data-ttu-id="f99f1-227">`AddPageRoute` usa `AddPageRouteModelConvention` para establecer la ruta.</span><span class="sxs-lookup"><span data-stu-id="f99f1-227">`AddPageRoute` uses `AddPageRouteModelConvention` to establish the route.</span></span>

<span data-ttu-id="f99f1-228">En la aplicación de ejemplo se crea una ruta a `/TheContactPage` para *Contact.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="f99f1-228">The sample app creates a route to `/TheContactPage` for *Contact.cshtml*:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet5)]

::: moniker-end

<span data-ttu-id="f99f1-229">A la página Contact también se puede llegar en `/Contact` a través de su ruta predeterminada.</span><span class="sxs-lookup"><span data-stu-id="f99f1-229">The Contact page can also be reached at `/Contact` via its default route.</span></span>

<span data-ttu-id="f99f1-230">La ruta personalizada de la aplicación de ejemplo que lleva a la página Contact tiene cabida para un segmento de ruta `text` opcional (`{text?}`).</span><span class="sxs-lookup"><span data-stu-id="f99f1-230">The sample app's custom route to the Contact page allows for an optional `text` route segment (`{text?}`).</span></span> <span data-ttu-id="f99f1-231">La página también incluye este segmento opcional en su directiva `@page` por si el usuario que la visita tiene acceso a la página en su ruta `/Contact`:</span><span class="sxs-lookup"><span data-stu-id="f99f1-231">The page also includes this optional segment in its `@page` directive in case the visitor accesses the page at its `/Contact` route:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-cshtml[](razor-pages-conventions/samples/3.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-cshtml[](razor-pages-conventions/samples/2.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

::: moniker-end

<span data-ttu-id="f99f1-232">Observe que la dirección URL generada para el vínculo **Contact** en la página presentada refleja la ruta actualizada:</span><span class="sxs-lookup"><span data-stu-id="f99f1-232">Note that the URL generated for the **Contact** link in the rendered page reflects the updated route:</span></span>

![Vínculo Contact de la aplicación de ejemplo en la barra de navegación](razor-pages-conventions/_static/contact-link.png)

![Si revisamos el vínculo Contact en el HTML presentado, vemos que href está establecido en "/TheContactPage".](razor-pages-conventions/_static/contact-link-source.png)

<span data-ttu-id="f99f1-235">Visite la página Contact a través de su ruta normal, `/Contact`, o de la ruta personalizada, `/TheContactPage`.</span><span class="sxs-lookup"><span data-stu-id="f99f1-235">Visit the Contact page at either its ordinary route, `/Contact`, or the custom route, `/TheContactPage`.</span></span> <span data-ttu-id="f99f1-236">Si se proporciona un segmento de ruta `text` adicional, la página muestra el segmento codificado en HTML que se ha proporcionado:</span><span class="sxs-lookup"><span data-stu-id="f99f1-236">If you supply an additional `text` route segment, the page shows the HTML-encoded segment that you provide:</span></span>

![Ejemplo del explorador Microsoft Edge en el que se proporciona un segmento de ruta opcional "text" de "TextValue" en la dirección URL](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a><span data-ttu-id="f99f1-239">Convenciones de acción del modelo de página</span><span class="sxs-lookup"><span data-stu-id="f99f1-239">Page model action conventions</span></span>

<span data-ttu-id="f99f1-240">El proveedor de modelos de página predeterminado que <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> implementa invoca convenciones que están diseñadas para proporcionar puntos de extensibilidad para la configuración de modelos de página.</span><span class="sxs-lookup"><span data-stu-id="f99f1-240">The default page model provider that implements <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> invokes conventions which are designed to provide extensibility points for configuring page models.</span></span> <span data-ttu-id="f99f1-241">Estas convenciones son útiles al crear y modificar escenarios de detección y procesamiento de páginas.</span><span class="sxs-lookup"><span data-stu-id="f99f1-241">These conventions are useful when building and modifying page discovery and processing scenarios.</span></span>

<span data-ttu-id="f99f1-242">En los ejemplos de esta sección, la aplicación de ejemplo usa `AddHeaderAttribute` una clase, que es <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>un, que aplica un encabezado de respuesta:</span><span class="sxs-lookup"><span data-stu-id="f99f1-242">For the examples in this section, the sample app uses an `AddHeaderAttribute` class, which is a <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, that applies a response header:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="f99f1-243">Si empleamos convenciones, el ejemplo indica cómo aplicar el atributo a todas las páginas de una carpeta y a una sola página.</span><span class="sxs-lookup"><span data-stu-id="f99f1-243">Using conventions, the sample demonstrates how to apply the attribute to all of the pages in a folder and to a single page.</span></span>

<span data-ttu-id="f99f1-244">**Convención de modelo de aplicación de carpeta**</span><span class="sxs-lookup"><span data-stu-id="f99f1-244">**Folder app model convention**</span></span>

<span data-ttu-id="f99f1-245">Utilice <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> para crear y agregar un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> que invoca una acción en <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> las instancias de para todas las páginas de la carpeta especificada.</span><span class="sxs-lookup"><span data-stu-id="f99f1-245">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> instances for all pages under the specified folder.</span></span>

<span data-ttu-id="f99f1-246">En el ejemplo se demuestra el uso de `AddFolderApplicationModelConvention` agregando un encabezado (`OtherPagesHeader`) a las páginas de la carpeta *OtherPages* de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="f99f1-246">The sample demonstrates the use of `AddFolderApplicationModelConvention` by adding a header, `OtherPagesHeader`, to the pages inside the *OtherPages* folder of the app:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet6)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet6)]

::: moniker-end

<span data-ttu-id="f99f1-247">Solicite la página Page1 del ejemplo en `localhost:5000/OtherPages/Page1` y revise los encabezados para ver el resultado:</span><span class="sxs-lookup"><span data-stu-id="f99f1-247">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1` and inspect the headers to view the result:</span></span>

![Los encabezados de respuesta de la página OtherPages/Page1 ponen de manifiesto que OtherPagesHeader se ha agregado.](razor-pages-conventions/_static/page1-otherpages-header.png)

<span data-ttu-id="f99f1-249">**Convención de modelo de aplicación de página**</span><span class="sxs-lookup"><span data-stu-id="f99f1-249">**Page app model convention**</span></span>

<span data-ttu-id="f99f1-250">Utilice <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> para crear y agregar un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> que invoca una acción en <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> para la página con el nombre especificado.</span><span class="sxs-lookup"><span data-stu-id="f99f1-250">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> for the page with the specified name.</span></span>

<span data-ttu-id="f99f1-251">En el ejemplo se demuestra el uso de `AddPageApplicationModelConvention` agregando un encabezado (`AboutHeader`) a la página About:</span><span class="sxs-lookup"><span data-stu-id="f99f1-251">The sample demonstrates the use of `AddPageApplicationModelConvention` by adding a header, `AboutHeader`, to the About page:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet7)]

::: moniker-end

<span data-ttu-id="f99f1-252">Solicite la página About del ejemplo en `localhost:5000/About` y revise los encabezados para ver el resultado:</span><span class="sxs-lookup"><span data-stu-id="f99f1-252">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Los encabezados de respuesta de la página About ponen de manifiesto que AboutHeader se ha agregado.](razor-pages-conventions/_static/about-page-about-header.png)

<span data-ttu-id="f99f1-254">**Configurar un filtro**</span><span class="sxs-lookup"><span data-stu-id="f99f1-254">**Configure a filter**</span></span>

<span data-ttu-id="f99f1-255"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*>configura el filtro especificado que se va a aplicar.</span><span class="sxs-lookup"><span data-stu-id="f99f1-255"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified filter to apply.</span></span> <span data-ttu-id="f99f1-256">Se puede implementar una clase de filtro, pero en la aplicación de ejemplo se muestra cómo implementar un filtro en una expresión lambda, cosa que sucede en segundo plano como una fábrica que devuelve un filtro:</span><span class="sxs-lookup"><span data-stu-id="f99f1-256">You can implement a filter class, but the sample app shows how to implement a filter in a lambda expression, which is implemented behind-the-scenes as a factory that returns a filter:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet8)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet8)]

::: moniker-end

<span data-ttu-id="f99f1-257">El modelo de páginas de aplicación se usa para comprobar la ruta de acceso relativa de los segmentos que conducen a la página Page2 en la carpeta *OtherPages*.</span><span class="sxs-lookup"><span data-stu-id="f99f1-257">The page app model is used to check the relative path for segments that lead to the Page2 page in the *OtherPages* folder.</span></span> <span data-ttu-id="f99f1-258">Si la condición se supera, se agrega un encabezado.</span><span class="sxs-lookup"><span data-stu-id="f99f1-258">If the condition passes, a header is added.</span></span> <span data-ttu-id="f99f1-259">Si no, se aplica `EmptyFilter`.</span><span class="sxs-lookup"><span data-stu-id="f99f1-259">If not, the `EmptyFilter` is applied.</span></span>

<span data-ttu-id="f99f1-260">`EmptyFilter` es un [filtro de acciones](xref:mvc/controllers/filters#action-filters).</span><span class="sxs-lookup"><span data-stu-id="f99f1-260">`EmptyFilter` is an [Action filter](xref:mvc/controllers/filters#action-filters).</span></span> <span data-ttu-id="f99f1-261">Dado que Razor pages omite los filtros de acción, `EmptyFilter` el no tiene ningún efecto según lo previsto si la `OtherPages/Page2`ruta de acceso no contiene.</span><span class="sxs-lookup"><span data-stu-id="f99f1-261">Since Action filters are ignored by Razor Pages, the `EmptyFilter` has no effect as intended if the path doesn't contain `OtherPages/Page2`.</span></span>

<span data-ttu-id="f99f1-262">Solicite la página Page2 del ejemplo en `localhost:5000/OtherPages/Page2` y revise los encabezados para ver el resultado:</span><span class="sxs-lookup"><span data-stu-id="f99f1-262">Request the sample's Page2 page at `localhost:5000/OtherPages/Page2` and inspect the headers to view the result:</span></span>

![OtherPagesPage2Header se agrega a la respuesta de Page2.](razor-pages-conventions/_static/page2-filter-header.png)

<span data-ttu-id="f99f1-264">**Configurar una fábrica de filtros**</span><span class="sxs-lookup"><span data-stu-id="f99f1-264">**Configure a filter factory**</span></span>

<span data-ttu-id="f99f1-265"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*>configura el generador especificado para aplicar [filtros](xref:mvc/controllers/filters) a todos los Razor pages.</span><span class="sxs-lookup"><span data-stu-id="f99f1-265"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified factory to apply [filters](xref:mvc/controllers/filters) to all Razor Pages.</span></span>

<span data-ttu-id="f99f1-266">En la aplicación de ejemplo se proporciona un ejemplo del uso de una [fábrica de filtros](xref:mvc/controllers/filters#ifilterfactory), para lo cual se agrega un encabezado (`FilterFactoryHeader`) con dos valores a las páginas de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="f99f1-266">The sample app provides an example of using a [filter factory](xref:mvc/controllers/filters#ifilterfactory) by adding a header, `FilterFactoryHeader`, with two values to the app's pages:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Startup.cs?name=snippet9)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet9)]

::: moniker-end

<span data-ttu-id="f99f1-267">*AddHeaderWithFactory.cs*:</span><span class="sxs-lookup"><span data-stu-id="f99f1-267">*AddHeaderWithFactory.cs*:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/3.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="f99f1-268">Solicite la página About del ejemplo en `localhost:5000/About` y revise los encabezados para ver el resultado:</span><span class="sxs-lookup"><span data-stu-id="f99f1-268">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Los encabezados de respuesta de la página About ponen de manifiesto que se han agregado dos encabezados FilterFactoryHeader.](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a><span data-ttu-id="f99f1-270">Filtros de MVC y filtro de página (IPageFilter)</span><span class="sxs-lookup"><span data-stu-id="f99f1-270">MVC Filters and the Page filter (IPageFilter)</span></span>

<span data-ttu-id="f99f1-271">Las páginas de Razor no tienen en cuenta los [filtros de acciones](xref:mvc/controllers/filters#action-filters) de MVC, porque en este tipo de páginas se usan métodos de control.</span><span class="sxs-lookup"><span data-stu-id="f99f1-271">MVC [Action filters](xref:mvc/controllers/filters#action-filters) are ignored by Razor Pages, since Razor Pages use handler methods.</span></span> <span data-ttu-id="f99f1-272">Otros tipos de filtros MVC están disponibles para su uso: [Autorización](xref:mvc/controllers/filters#authorization-filters), [excepción](xref:mvc/controllers/filters#exception-filters), [recurso](xref:mvc/controllers/filters#resource-filters)y [resultado](xref:mvc/controllers/filters#result-filters).</span><span class="sxs-lookup"><span data-stu-id="f99f1-272">Other types of MVC filters are available for you to use: [Authorization](xref:mvc/controllers/filters#authorization-filters), [Exception](xref:mvc/controllers/filters#exception-filters), [Resource](xref:mvc/controllers/filters#resource-filters), and [Result](xref:mvc/controllers/filters#result-filters).</span></span> <span data-ttu-id="f99f1-273">Para más información, vea el tema [Filters](xref:mvc/controllers/filters) (Filtros).</span><span class="sxs-lookup"><span data-stu-id="f99f1-273">For more information, see the [Filters](xref:mvc/controllers/filters) topic.</span></span>

<span data-ttu-id="f99f1-274">El filtro de página<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>() es un filtro que se aplica a Razor pages.</span><span class="sxs-lookup"><span data-stu-id="f99f1-274">The Page filter (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) is a filter that applies to Razor Pages.</span></span> <span data-ttu-id="f99f1-275">Para más información, vea [Filter methods for Razor Pages](xref:razor-pages/filter) (Métodos de filtrado para páginas de Razor).</span><span class="sxs-lookup"><span data-stu-id="f99f1-275">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f99f1-276">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="f99f1-276">Additional resources</span></span>

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>
