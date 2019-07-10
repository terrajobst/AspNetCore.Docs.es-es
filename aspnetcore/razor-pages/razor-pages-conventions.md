---
title: Convenciones de aplicación y de ruta de páginas de Razor en ASP.NET Core
author: guardrex
description: Vea cómo las convenciones de proveedor de modelos de aplicación y de ruta sirven para controlar el enrutamiento, la detección y el procesamiento de páginas.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/07/2019
uid: razor-pages/razor-pages-conventions
ms.openlocfilehash: 59c8af648b50deb51f3762c14348d08acd48886e
ms.sourcegitcommit: bee530454ae2b3c25dc7ffebf93536f479a14460
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2019
ms.locfileid: "67724451"
---
# <a name="razor-pages-route-and-app-conventions-in-aspnet-core"></a><span data-ttu-id="0fbd5-103">Convenciones de aplicación y de ruta de páginas de Razor en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0fbd5-103">Razor Pages route and app conventions in ASP.NET Core</span></span>

<span data-ttu-id="0fbd5-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0fbd5-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="0fbd5-105">Aquí encontrará información para saber cómo usar las [convenciones de proveedor de modelos de aplicación y de ruta](xref:mvc/controllers/application-model#conventions) con objeto de controlar el enrutamiento, la detección y el procesamiento en aplicaciones de páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-105">Learn how to use page [route and app model provider conventions](xref:mvc/controllers/application-model#conventions) to control page routing, discovery, and processing in Razor Pages apps.</span></span>

<span data-ttu-id="0fbd5-106">Si necesita configurar rutas de una página personalizadas para páginas concretas, configure el enrutamiento a esas páginas con la [convención AddPageRoute](#configure-a-page-route), descrita más adelante en este tema.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-106">When you need to configure custom page routes for individual pages, configure routing to pages with the [AddPageRoute convention](#configure-a-page-route) described later in this topic.</span></span>

<span data-ttu-id="0fbd5-107">Para especificar una ruta de página, agregue segmentos de ruta o agregar parámetros a una ruta, use la página `@page` directiva.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-107">To specify a page route, add route segments, or add parameters to a route, use the page's `@page` directive.</span></span> <span data-ttu-id="0fbd5-108">Para obtener más información, consulte [rutas personalizadas](xref:razor-pages/index#custom-routes).</span><span class="sxs-lookup"><span data-stu-id="0fbd5-108">For more information, see [Custom routes](xref:razor-pages/index#custom-routes).</span></span>

<span data-ttu-id="0fbd5-109">Hay palabras reservadas que no se puede usar como segmentos de ruta o nombres de parámetro.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-109">There are reserved words that can't be used as route segments or parameter names.</span></span> <span data-ttu-id="0fbd5-110">Para obtener más información, consulte [enrutamiento: Enrutamientos nombres reservados](xref:fundamentals/routing#reserved-routing-names).</span><span class="sxs-lookup"><span data-stu-id="0fbd5-110">For more information, see [Routing: Reserved routing names](xref:fundamentals/routing#reserved-routing-names).</span></span>

<span data-ttu-id="0fbd5-111">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0fbd5-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

| <span data-ttu-id="0fbd5-112">Escenario</span><span class="sxs-lookup"><span data-stu-id="0fbd5-112">Scenario</span></span> | <span data-ttu-id="0fbd5-113">El ejemplo explica cómo...</span><span class="sxs-lookup"><span data-stu-id="0fbd5-113">The sample demonstrates ...</span></span> |
| -------- | --------------------------- |
| [<span data-ttu-id="0fbd5-114">Convenciones de modelo</span><span class="sxs-lookup"><span data-stu-id="0fbd5-114">Model conventions</span></span>](#model-conventions)<br><br><span data-ttu-id="0fbd5-115">Conventions.Add</span><span class="sxs-lookup"><span data-stu-id="0fbd5-115">Conventions.Add</span></span><ul><li><span data-ttu-id="0fbd5-116">IPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="0fbd5-116">IPageRouteModelConvention</span></span></li><li><span data-ttu-id="0fbd5-117">IPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="0fbd5-117">IPageApplicationModelConvention</span></span></li><li><span data-ttu-id="0fbd5-118">IPageHandlerModelConvention</span><span class="sxs-lookup"><span data-stu-id="0fbd5-118">IPageHandlerModelConvention</span></span></li></ul> | <span data-ttu-id="0fbd5-119">Agregar un encabezado y una plantilla de ruta a las páginas de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-119">Add a route template and header to an app's pages.</span></span> |
| [<span data-ttu-id="0fbd5-120">Convenciones de acción de ruta de página</span><span class="sxs-lookup"><span data-stu-id="0fbd5-120">Page route action conventions</span></span>](#page-route-action-conventions)<ul><li><span data-ttu-id="0fbd5-121">AddFolderRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="0fbd5-121">AddFolderRouteModelConvention</span></span></li><li><span data-ttu-id="0fbd5-122">AddPageRouteModelConvention</span><span class="sxs-lookup"><span data-stu-id="0fbd5-122">AddPageRouteModelConvention</span></span></li><li><span data-ttu-id="0fbd5-123">AddPageRoute</span><span class="sxs-lookup"><span data-stu-id="0fbd5-123">AddPageRoute</span></span></li></ul> | <span data-ttu-id="0fbd5-124">Agregar una plantilla de ruta a las páginas de una carpeta y a una sola página.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-124">Add a route template to pages in a folder and to a single page.</span></span> |
| [<span data-ttu-id="0fbd5-125">Convenciones de acción del modelo de página</span><span class="sxs-lookup"><span data-stu-id="0fbd5-125">Page model action conventions</span></span>](#page-model-action-conventions)<ul><li><span data-ttu-id="0fbd5-126">AddFolderApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="0fbd5-126">AddFolderApplicationModelConvention</span></span></li><li><span data-ttu-id="0fbd5-127">AddPageApplicationModelConvention</span><span class="sxs-lookup"><span data-stu-id="0fbd5-127">AddPageApplicationModelConvention</span></span></li><li><span data-ttu-id="0fbd5-128">ConfigureFilter (clase de filtro, expresión lambda o fábrica de filtros)</span><span class="sxs-lookup"><span data-stu-id="0fbd5-128">ConfigureFilter (filter class, lambda expression, or filter factory)</span></span></li></ul> | <span data-ttu-id="0fbd5-129">Agregar un encabezado a las páginas de una carpeta, agregar un encabezado a una sola página y configurar una [fábrica de filtros](xref:mvc/controllers/filters#ifilterfactory) para agregar un encabezado a las páginas de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-129">Add a header to pages in a folder, add a header to a single page, and configure a [filter factory](xref:mvc/controllers/filters#ifilterfactory) to add a header to an app's pages.</span></span> |

<span data-ttu-id="0fbd5-130">Convenciones de páginas de Razor se agregan y configuran mediante el <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> método de extensión para <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> en la colección de servicios en la `Startup` clase.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-130">Razor Pages conventions are added and configured using the <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> extension method to <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> on the service collection in the `Startup` class.</span></span> <span data-ttu-id="0fbd5-131">Más avanzado el tema veremos los siguientes ejemplos de convención:</span><span class="sxs-lookup"><span data-stu-id="0fbd5-131">The following convention examples are explained later in this topic:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddRazorPagesOptions(options =>
            {
                options.Conventions.Add( ... );
                options.Conventions.AddFolderRouteModelConvention("/OtherPages", model => { ... });
                options.Conventions.AddPageRouteModelConvention("/About", model => { ... });
                options.Conventions.AddPageRoute("/Contact", "TheContactPage/{text?}");
                options.Conventions.AddFolderApplicationModelConvention("/OtherPages", model => { ... });
                options.Conventions.AddPageApplicationModelConvention("/About", model => { ... });
                options.Conventions.ConfigureFilter(model => { ... });
                options.Conventions.ConfigureFilter( ... );
            });
}
```

## <a name="route-order"></a><span data-ttu-id="0fbd5-132">Orden de la ruta</span><span class="sxs-lookup"><span data-stu-id="0fbd5-132">Route order</span></span>

<span data-ttu-id="0fbd5-133">Rutas especifican una <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> para procesar (coincidencia de ruta).</span><span class="sxs-lookup"><span data-stu-id="0fbd5-133">Routes specify an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> for processing (route matching).</span></span>

| <span data-ttu-id="0fbd5-134">Ordenar</span><span class="sxs-lookup"><span data-stu-id="0fbd5-134">Order</span></span>            | <span data-ttu-id="0fbd5-135">Comportamiento</span><span class="sxs-lookup"><span data-stu-id="0fbd5-135">Behavior</span></span> |
| :--------------: | -------- |
| <span data-ttu-id="0fbd5-136">-1</span><span class="sxs-lookup"><span data-stu-id="0fbd5-136">-1</span></span>               | <span data-ttu-id="0fbd5-137">La ruta se procesa antes de otras rutas se procesan.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-137">The route is processed before other routes are processed.</span></span> |
| <span data-ttu-id="0fbd5-138">0</span><span class="sxs-lookup"><span data-stu-id="0fbd5-138">0</span></span>                | <span data-ttu-id="0fbd5-139">No se especifica el orden (valor predeterminado).</span><span class="sxs-lookup"><span data-stu-id="0fbd5-139">Order isn't specified (default value).</span></span> <span data-ttu-id="0fbd5-140">No asignar `Order` (`Order = null`) el valor predeterminado es la ruta `Order` en 0 (cero) para su procesamiento.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-140">Not assigning `Order` (`Order = null`) defaults the route `Order` to 0 (zero) for processing.</span></span> |
| <span data-ttu-id="0fbd5-141">1, 2, &hellip; n</span><span class="sxs-lookup"><span data-stu-id="0fbd5-141">1, 2, &hellip; n</span></span> | <span data-ttu-id="0fbd5-142">Especifica el orden de procesamiento de la ruta.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-142">Specifies the route processing order.</span></span> |

<span data-ttu-id="0fbd5-143">Procesamiento de la ruta se establece mediante la convención:</span><span class="sxs-lookup"><span data-stu-id="0fbd5-143">Route processing is established by convention:</span></span>

* <span data-ttu-id="0fbd5-144">Las rutas se procesan en orden secuencial (-1, 0, 1, 2, &hellip; n).</span><span class="sxs-lookup"><span data-stu-id="0fbd5-144">Routes are processed in sequential order (-1, 0, 1, 2, &hellip; n).</span></span>
* <span data-ttu-id="0fbd5-145">Cuando las rutas tienen el mismo `Order`, más ruta específica se compara primero, seguido por rutas menos específicas.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-145">When routes have the same `Order`, the most specific route is matched first followed by less specific routes.</span></span>
* <span data-ttu-id="0fbd5-146">Cuando las rutas con el mismo `Order` y el mismo número de parámetros coincide con una dirección URL de solicitud, las rutas se procesan en el orden en que se agreguen a la <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-146">When routes with the same `Order` and the same number of parameters match a request URL, routes are processed in the order that they're added to the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>.</span></span>

<span data-ttu-id="0fbd5-147">Si es posible, evite según un orden de procesamiento de la ruta establecida.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-147">If possible, avoid depending on an established route processing order.</span></span> <span data-ttu-id="0fbd5-148">Por lo general, selecciona la ruta correcta con coincidencia de dirección URL de enrutamiento.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-148">Generally, routing selects the correct route with URL matching.</span></span> <span data-ttu-id="0fbd5-149">Si se debe establecer ruta `Order` propiedades para enrutar las solicitudes correctamente, esquema de enrutamiento de la aplicación es probablemente confuso para los clientes y frágil mantener.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-149">If you must set route `Order` properties to route requests correctly, the app's routing scheme is probably confusing to clients and fragile to maintain.</span></span> <span data-ttu-id="0fbd5-150">Buscar simplificar el esquema de enrutamiento de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-150">Seek to simplify the app's routing scheme.</span></span> <span data-ttu-id="0fbd5-151">La aplicación de ejemplo requiere una ruta explicita de procesamiento de orden en que se muestran varios escenarios de enrutamientos mediante una sola aplicación.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-151">The sample app requires an explicit route processing order to demonstrate several routing scenarios using a single app.</span></span> <span data-ttu-id="0fbd5-152">Sin embargo, debe intentar evitar la práctica de la ruta de configuración `Order` en aplicaciones de producción.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-152">However, you should attempt to avoid the practice of setting route `Order` in production apps.</span></span>

<span data-ttu-id="0fbd5-153">El enrutamiento del controlador de MVC y el enrutamiento de Razor Pages comparten una implementación.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-153">Razor Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="0fbd5-154">Información sobre el orden de la ruta en los temas MVC está disponible en [enrutamiento a acciones del controlador: Orden de las rutas de atributo](xref:mvc/controllers/routing#ordering-attribute-routes).</span><span class="sxs-lookup"><span data-stu-id="0fbd5-154">Information on route order in the MVC topics is available at [Routing to controller actions: Ordering attribute routes](xref:mvc/controllers/routing#ordering-attribute-routes).</span></span>

## <a name="model-conventions"></a><span data-ttu-id="0fbd5-155">Convenciones de modelo</span><span class="sxs-lookup"><span data-stu-id="0fbd5-155">Model conventions</span></span>

<span data-ttu-id="0fbd5-156">Agregar un delegado para <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> para agregar [convenciones del modelo](xref:mvc/controllers/application-model#conventions) que se aplican a las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-156">Add a delegate for <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> to add [model conventions](xref:mvc/controllers/application-model#conventions) that apply to Razor Pages.</span></span>

### <a name="add-a-route-model-convention-to-all-pages"></a><span data-ttu-id="0fbd5-157">Agregar una convención de modelo de ruta a todas las páginas</span><span class="sxs-lookup"><span data-stu-id="0fbd5-157">Add a route model convention to all pages</span></span>

<span data-ttu-id="0fbd5-158">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> para crear y agregar un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> a la colección de <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instancias que se aplican durante la ruta de página construcción del modelo.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-158">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page route model construction.</span></span>

<span data-ttu-id="0fbd5-159">En la aplicación de ejemplo se agrega una plantilla de ruta `{globalTemplate?}` a todas las páginas de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="0fbd5-159">The sample app adds a `{globalTemplate?}` route template to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalTemplatePageRouteModelConvention.cs?name=snippet1)]

<span data-ttu-id="0fbd5-160">La propiedad <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> de <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> está establecida en `1`.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-160">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `1`.</span></span> <span data-ttu-id="0fbd5-161">Esto garantiza que la ruta siguiente que coincida con el comportamiento de la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="0fbd5-161">This ensures the following route matching behavior in the sample app:</span></span>

* <span data-ttu-id="0fbd5-162">Una plantilla de ruta para `TheContactPage/{text?}` se agrega más adelante en el tema.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-162">A route template for `TheContactPage/{text?}` is added later in the topic.</span></span> <span data-ttu-id="0fbd5-163">La ruta de la página Contact tiene un orden predeterminado de `null` (`Order = 0`), para que coincida con antes el `{globalTemplate?}` plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-163">The Contact Page route has a default order of `null` (`Order = 0`), so it matches before the `{globalTemplate?}` route template.</span></span>
* <span data-ttu-id="0fbd5-164">Un `{aboutTemplate?}` plantilla de ruta se agrega más adelante en el tema.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-164">An `{aboutTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="0fbd5-165">La plantilla `{aboutTemplate?}` tiene un `Order` con un valor de `2`.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-165">The `{aboutTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="0fbd5-166">Cuando la página About se solicita en `/About/RouteDataValue`, "RouteDataValue" se carga en `RouteData.Values["globalTemplate"]` (`Order = 1`) y no `RouteData.Values["aboutTemplate"]` (`Order = 2`), debido a la configuración de la propiedad `Order`.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-166">When the About page is requested at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>
* <span data-ttu-id="0fbd5-167">Un `{otherPagesTemplate?}` plantilla de ruta se agrega más adelante en el tema.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-167">An `{otherPagesTemplate?}` route template is added later in the topic.</span></span> <span data-ttu-id="0fbd5-168">La plantilla `{otherPagesTemplate?}` tiene un `Order` con un valor de `2`.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-168">The `{otherPagesTemplate?}` template is given an `Order` of `2`.</span></span> <span data-ttu-id="0fbd5-169">Cuando cualquier página en el *OtherPages/páginas* carpeta se solicita con un parámetro de ruta (por ejemplo, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" se carga en `RouteData.Values["globalTemplate"]` (`Order = 1`) y no `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) debido a la configuración del `Order` propiedad.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-169">When any page in the *Pages/OtherPages* folder is requested with a route parameter (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="0fbd5-170">Siempre que sea posible, no establezca la `Order`, que da como resultado `Order = 0`.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-170">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="0fbd5-171">Se basan en enrutamiento para seleccionar la ruta correcta.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-171">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="0fbd5-172">Opciones de páginas de Razor, como agregar <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, se agregan cuando MVC se agrega a la colección de servicios en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-172">Razor Pages options, such as adding <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions>, are added when MVC is added to the service collection in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="0fbd5-173">Para obtener un ejemplo, vea la [aplicación de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span><span class="sxs-lookup"><span data-stu-id="0fbd5-173">For an example, see the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/razor-pages-conventions/samples/).</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet1)]

<span data-ttu-id="0fbd5-174">Solicite la página About del ejemplo en `localhost:5000/About/GlobalRouteValue` y revise el resultado:</span><span class="sxs-lookup"><span data-stu-id="0fbd5-174">Request the sample's About page at `localhost:5000/About/GlobalRouteValue` and inspect the result:</span></span>

![La página About se solicita con un segmento de ruta de GlobalRouteValue.](razor-pages-conventions/_static/about-page-global-template.png)

### <a name="add-an-app-model-convention-to-all-pages"></a><span data-ttu-id="0fbd5-177">Agregar una convención de modelo de aplicación a todas las páginas</span><span class="sxs-lookup"><span data-stu-id="0fbd5-177">Add an app model convention to all pages</span></span>

<span data-ttu-id="0fbd5-178">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> para crear y agregar un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> a la colección de <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instancias que se aplican durante la construcción del modelo de aplicación de página.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-178">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page app model construction.</span></span>

<span data-ttu-id="0fbd5-179">Para demostrar esta y otras convenciones más adelante en este tema, la aplicación de ejemplo incluye una clase `AddHeaderAttribute`.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-179">To demonstrate this and other conventions later in the topic, the sample app includes an `AddHeaderAttribute` class.</span></span> <span data-ttu-id="0fbd5-180">El constructor de esta clase acepta una cadena `name` y una matriz de cadenas `values`.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-180">The class constructor accepts a `name` string and a `values` string array.</span></span> <span data-ttu-id="0fbd5-181">Estos valores se usan en su método `OnResultExecuting` para establecer un encabezado de respuesta.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-181">These values are used in its `OnResultExecuting` method to set a response header.</span></span> <span data-ttu-id="0fbd5-182">La clase completa se muestra en la sección [Convenciones de acción del modelo de página](#page-model-action-conventions) más adelante en este tema.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-182">The full class is shown in the [Page model action conventions](#page-model-action-conventions) section later in the topic.</span></span>

<span data-ttu-id="0fbd5-183">En la aplicación de ejemplo se usa la clase `AddHeaderAttribute` para agregar un encabezado (`GlobalHeader`) a todas las páginas de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="0fbd5-183">The sample app uses the `AddHeaderAttribute` class to add a header, `GlobalHeader`, to all of the pages in the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalHeaderPageApplicationModelConvention.cs?name=snippet1)]

<span data-ttu-id="0fbd5-184">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0fbd5-184">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet2)]

<span data-ttu-id="0fbd5-185">Solicite la página About del ejemplo en `localhost:5000/About` y revise los encabezados para ver el resultado:</span><span class="sxs-lookup"><span data-stu-id="0fbd5-185">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Los encabezados de respuesta de la página About ponen de manifiesto que GlobalHeader se ha agregado.](razor-pages-conventions/_static/about-page-global-header.png)

### <a name="add-a-handler-model-convention-to-all-pages"></a><span data-ttu-id="0fbd5-187">Agregar una convención de modelo de controlador a todas las páginas</span><span class="sxs-lookup"><span data-stu-id="0fbd5-187">Add a handler model convention to all pages</span></span>

<span data-ttu-id="0fbd5-188">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> para crear y agregar un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> a la colección de <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instancias que se aplican durante la construcción del modelo de controlador de página.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-188">Use <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.Conventions> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageHandlerModelConvention> to the collection of <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageConvention> instances that are applied during page handler model construction.</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Conventions/GlobalPageHandlerModelConvention.cs?name=snippet1)]

<span data-ttu-id="0fbd5-189">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="0fbd5-189">*Startup.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet10)]

## <a name="page-route-action-conventions"></a><span data-ttu-id="0fbd5-190">Convenciones de acción de ruta de página</span><span class="sxs-lookup"><span data-stu-id="0fbd5-190">Page route action conventions</span></span>

<span data-ttu-id="0fbd5-191">El proveedor de modelo de ruta predeterminado que se deriva de <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> invoca convenciones que están diseñadas para proporcionar puntos de extensibilidad para configurar las rutas de la página.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-191">The default route model provider that derives from <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelProvider> invokes conventions which are designed to provide extensibility points for configuring page routes.</span></span>

### <a name="folder-route-model-convention"></a><span data-ttu-id="0fbd5-192">Convención de modelo de ruta de carpeta</span><span class="sxs-lookup"><span data-stu-id="0fbd5-192">Folder route model convention</span></span>

<span data-ttu-id="0fbd5-193">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> para crear y agregar un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> que invoque una acción en el <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> para todas las páginas en la carpeta especificada.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-193">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for all of the pages under the specified folder.</span></span>

<span data-ttu-id="0fbd5-194">En la aplicación de ejemplo se usa <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> para agregar una plantilla de ruta `{otherPagesTemplate?}` a las páginas de la carpeta *OtherPages*:</span><span class="sxs-lookup"><span data-stu-id="0fbd5-194">The sample app uses <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderRouteModelConvention*> to add an `{otherPagesTemplate?}` route template to the pages in the *OtherPages* folder:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet3)]

<span data-ttu-id="0fbd5-195">La propiedad <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> de <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> está establecida en `2`.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-195">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="0fbd5-196">Esto garantiza que la plantilla para `{globalTemplate?}` (establecido anteriormente en este tema para `1`) tiene prioridad para la posición de valor de datos de la primera ruta cuando se proporciona un solo valor de ruta.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-196">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="0fbd5-197">Si una página en el *OtherPages/páginas* carpeta se solicita con un valor de parámetro de ruta (por ejemplo, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" se carga en `RouteData.Values["globalTemplate"]` (`Order = 1`) y no `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) debido a la configuración del `Order` propiedad.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-197">If a page in the *Pages/OtherPages* folder is requested with a route parameter value (for example, `/OtherPages/Page1/RouteDataValue`), "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["otherPagesTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="0fbd5-198">Siempre que sea posible, no establezca la `Order`, que da como resultado `Order = 0`.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-198">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="0fbd5-199">Se basan en enrutamiento para seleccionar la ruta correcta.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-199">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="0fbd5-200">Solicite la página Page1 del ejemplo en `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` y revise el resultado:</span><span class="sxs-lookup"><span data-stu-id="0fbd5-200">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1/GlobalRouteValue/OtherPagesRouteValue` and inspect the result:</span></span>

![Page1 en la carpeta OtherPages solicitada con un segmento de ruta de GlobalRouteValue y OtherPagesRouteValue](razor-pages-conventions/_static/otherpages-page1-global-and-otherpages-templates.png)

### <a name="page-route-model-convention"></a><span data-ttu-id="0fbd5-203">Convención de modelo de ruta de página</span><span class="sxs-lookup"><span data-stu-id="0fbd5-203">Page route model convention</span></span>

<span data-ttu-id="0fbd5-204">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> para crear y agregar un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> que invoque una acción en el <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> para la página con el nombre especificado.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-204">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageRouteModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageRouteModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageRouteModel> for the page with the specified name.</span></span>

<span data-ttu-id="0fbd5-205">En la aplicación de ejemplo se usa `AddPageRouteModelConvention` para agregar una plantilla de ruta `{aboutTemplate?}` a la página About:</span><span class="sxs-lookup"><span data-stu-id="0fbd5-205">The sample app uses `AddPageRouteModelConvention` to add an `{aboutTemplate?}` route template to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet4)]

<span data-ttu-id="0fbd5-206">La propiedad <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> de <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> está establecida en `2`.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-206">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel.Order*> property for the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.AttributeRouteModel> is set to `2`.</span></span> <span data-ttu-id="0fbd5-207">Esto garantiza que la plantilla para `{globalTemplate?}` (establecido anteriormente en este tema para `1`) tiene prioridad para la posición de valor de datos de la primera ruta cuando se proporciona un solo valor de ruta.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-207">This ensures that the template for `{globalTemplate?}` (set earlier in the topic to `1`) is given priority for the first route data value position when a single route value is provided.</span></span> <span data-ttu-id="0fbd5-208">Si la página About se solicita con un valor de parámetro de ruta en `/About/RouteDataValue`, "RouteDataValue" se carga en `RouteData.Values["globalTemplate"]` (`Order = 1`) y no `RouteData.Values["aboutTemplate"]` (`Order = 2`) debido a la configuración del `Order` propiedad.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-208">If the About page is requested with a route parameter value at `/About/RouteDataValue`, "RouteDataValue" is loaded into `RouteData.Values["globalTemplate"]` (`Order = 1`) and not `RouteData.Values["aboutTemplate"]` (`Order = 2`) due to setting the `Order` property.</span></span>

<span data-ttu-id="0fbd5-209">Siempre que sea posible, no establezca la `Order`, que da como resultado `Order = 0`.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-209">Wherever possible, don't set the `Order`, which results in `Order = 0`.</span></span> <span data-ttu-id="0fbd5-210">Se basan en enrutamiento para seleccionar la ruta correcta.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-210">Rely on routing to select the correct route.</span></span>

<span data-ttu-id="0fbd5-211">Solicite la página About del ejemplo en `localhost:5000/About/GlobalRouteValue/AboutRouteValue` y revise el resultado:</span><span class="sxs-lookup"><span data-stu-id="0fbd5-211">Request the sample's About page at `localhost:5000/About/GlobalRouteValue/AboutRouteValue` and inspect the result:</span></span>

![La página About se solicita con los segmentos de ruta de GlobalRouteValue y AboutRouteValue.](razor-pages-conventions/_static/about-page-global-and-about-templates.png)

::: moniker range=">= aspnetcore-2.2"

## <a name="use-a-parameter-transformer-to-customize-page-routes"></a><span data-ttu-id="0fbd5-214">Usar un transformador de parámetro para personalizar las rutas de página</span><span class="sxs-lookup"><span data-stu-id="0fbd5-214">Use a parameter transformer to customize page routes</span></span>

<span data-ttu-id="0fbd5-215">Las rutas de página generadas por ASP.NET Core pueden personalizarse mediante un transformador de parámetro.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-215">Page routes generated by ASP.NET Core can be customized using a parameter transformer.</span></span> <span data-ttu-id="0fbd5-216">Un transformador de parámetro implementa `IOutboundParameterTransformer` y transforma el valor de parámetros.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-216">A parameter transformer implements `IOutboundParameterTransformer` and transforms the value of parameters.</span></span> <span data-ttu-id="0fbd5-217">Por ejemplo, un transformador de parámetros `SlugifyParameterTransformer` personalizado cambia el valor de ruta `SubscriptionManagement` a `subscription-management`.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-217">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`.</span></span>

<span data-ttu-id="0fbd5-218">El `PageRouteTransformerConvention` convención de modelo de ruta de página aplica un transformador de parámetro a los segmentos de nombre de carpeta y archivo de rutas de página generada automáticamente en una aplicación.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-218">The `PageRouteTransformerConvention` page route model convention applies a parameter transformer to the folder and file name segments of automatically generated page routes in an app.</span></span> <span data-ttu-id="0fbd5-219">Por ejemplo, las páginas de Razor de archivos en */Pages/SubscriptionManagement/ViewAll.cshtml* tendría su ruta reescribe desde `/SubscriptionManagement/ViewAll` a `/subscription-management/view-all`.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-219">For example, the Razor Pages file at */Pages/SubscriptionManagement/ViewAll.cshtml* would have its route rewritten from `/SubscriptionManagement/ViewAll` to `/subscription-management/view-all`.</span></span>

<span data-ttu-id="0fbd5-220">`PageRouteTransformerConvention` solo transforma los segmentos de una ruta de página generados automáticamente que proceden de la carpeta de las páginas de Razor y el nombre de archivo.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-220">`PageRouteTransformerConvention` only transforms the automatically generated segments of a page route that come from the Razor Pages folder and file name.</span></span> <span data-ttu-id="0fbd5-221">No transforma los segmentos de ruta agregados con el `@page` directiva.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-221">It doesn't transform route segments added with the `@page` directive.</span></span> <span data-ttu-id="0fbd5-222">La convención también no transforma las rutas agregadas por <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-222">The convention also doesn't transform routes added by <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*>.</span></span>

<span data-ttu-id="0fbd5-223">El `PageRouteTransformerConvention` está registrado como una opción en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="0fbd5-223">The `PageRouteTransformerConvention` is registered as an option in `Startup.ConfigureServices`:</span></span>

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

## <a name="configure-a-page-route"></a><span data-ttu-id="0fbd5-224">Configurar una ruta de página</span><span class="sxs-lookup"><span data-stu-id="0fbd5-224">Configure a page route</span></span>

<span data-ttu-id="0fbd5-225">Use <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> para configurar una ruta a una página en la ruta de acceso de la página especificada.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-225">Use <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AddPageRoute*> to configure a route to a page at the specified page path.</span></span> <span data-ttu-id="0fbd5-226">Los vínculos generados que apuntan a esa página usan la ruta especificada.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-226">Generated links to the page use your specified route.</span></span> <span data-ttu-id="0fbd5-227">`AddPageRoute` usa `AddPageRouteModelConvention` para establecer la ruta.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-227">`AddPageRoute` uses `AddPageRouteModelConvention` to establish the route.</span></span>

<span data-ttu-id="0fbd5-228">En la aplicación de ejemplo se crea una ruta a `/TheContactPage` para *Contact.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="0fbd5-228">The sample app creates a route to `/TheContactPage` for *Contact.cshtml*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet5)]

<span data-ttu-id="0fbd5-229">A la página Contact también se puede llegar en `/Contact` a través de su ruta predeterminada.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-229">The Contact page can also be reached at `/Contact` via its default route.</span></span>

<span data-ttu-id="0fbd5-230">La ruta personalizada de la aplicación de ejemplo que lleva a la página Contact tiene cabida para un segmento de ruta `text` opcional (`{text?}`).</span><span class="sxs-lookup"><span data-stu-id="0fbd5-230">The sample app's custom route to the Contact page allows for an optional `text` route segment (`{text?}`).</span></span> <span data-ttu-id="0fbd5-231">La página también incluye este segmento opcional en su directiva `@page` por si el usuario que la visita tiene acceso a la página en su ruta `/Contact`:</span><span class="sxs-lookup"><span data-stu-id="0fbd5-231">The page also includes this optional segment in its `@page` directive in case the visitor accesses the page at its `/Contact` route:</span></span>

[!code-cshtml[](razor-pages-conventions/samples/2.x/SampleApp/Pages/Contact.cshtml?highlight=1)]

<span data-ttu-id="0fbd5-232">Observe que la dirección URL generada para el vínculo **Contact** en la página presentada refleja la ruta actualizada:</span><span class="sxs-lookup"><span data-stu-id="0fbd5-232">Note that the URL generated for the **Contact** link in the rendered page reflects the updated route:</span></span>

![Vínculo Contact de la aplicación de ejemplo en la barra de navegación](razor-pages-conventions/_static/contact-link.png)

![Si revisamos el vínculo Contact en el HTML presentado, vemos que href está establecido en "/TheContactPage".](razor-pages-conventions/_static/contact-link-source.png)

<span data-ttu-id="0fbd5-235">Visite la página Contact a través de su ruta normal, `/Contact`, o de la ruta personalizada, `/TheContactPage`.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-235">Visit the Contact page at either its ordinary route, `/Contact`, or the custom route, `/TheContactPage`.</span></span> <span data-ttu-id="0fbd5-236">Si se proporciona un segmento de ruta `text` adicional, la página muestra el segmento codificado en HTML que se ha proporcionado:</span><span class="sxs-lookup"><span data-stu-id="0fbd5-236">If you supply an additional `text` route segment, the page shows the HTML-encoded segment that you provide:</span></span>

![Ejemplo del explorador Microsoft Edge en el que se proporciona un segmento de ruta opcional "text" de "TextValue" en la dirección URL](razor-pages-conventions/_static/route-segment-with-custom-route.png)

## <a name="page-model-action-conventions"></a><span data-ttu-id="0fbd5-239">Convenciones de acción del modelo de página</span><span class="sxs-lookup"><span data-stu-id="0fbd5-239">Page model action conventions</span></span>

<span data-ttu-id="0fbd5-240">El proveedor de modelo de página predeterminado que implementa <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> invoca convenciones que están diseñadas para proporcionar puntos de extensibilidad para configurar los modelos de página.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-240">The default page model provider that implements <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelProvider> invokes conventions which are designed to provide extensibility points for configuring page models.</span></span> <span data-ttu-id="0fbd5-241">Estas convenciones son útiles al crear y modificar escenarios de detección y procesamiento de páginas.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-241">These conventions are useful when building and modifying page discovery and processing scenarios.</span></span>

<span data-ttu-id="0fbd5-242">Los ejemplos en esta sección, la aplicación de ejemplo usa un `AddHeaderAttribute` (clase), que es un <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, que aplica un encabezado de respuesta:</span><span class="sxs-lookup"><span data-stu-id="0fbd5-242">For the examples in this section, the sample app uses an `AddHeaderAttribute` class, which is a <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>, that applies a response header:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Filters/AddHeader.cs?name=snippet1)]

<span data-ttu-id="0fbd5-243">Si empleamos convenciones, el ejemplo indica cómo aplicar el atributo a todas las páginas de una carpeta y a una sola página.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-243">Using conventions, the sample demonstrates how to apply the attribute to all of the pages in a folder and to a single page.</span></span>

<span data-ttu-id="0fbd5-244">**Convención de modelo de aplicación de carpeta**</span><span class="sxs-lookup"><span data-stu-id="0fbd5-244">**Folder app model convention**</span></span>

<span data-ttu-id="0fbd5-245">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> para crear y agregar un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> que invoque una acción en <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> instancias para todas las páginas en la carpeta especificada.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-245">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> instances for all pages under the specified folder.</span></span>

<span data-ttu-id="0fbd5-246">En el ejemplo se demuestra el uso de `AddFolderApplicationModelConvention` agregando un encabezado (`OtherPagesHeader`) a las páginas de la carpeta *OtherPages* de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="0fbd5-246">The sample demonstrates the use of `AddFolderApplicationModelConvention` by adding a header, `OtherPagesHeader`, to the pages inside the *OtherPages* folder of the app:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet6)]

<span data-ttu-id="0fbd5-247">Solicite la página Page1 del ejemplo en `localhost:5000/OtherPages/Page1` y revise los encabezados para ver el resultado:</span><span class="sxs-lookup"><span data-stu-id="0fbd5-247">Request the sample's Page1 page at `localhost:5000/OtherPages/Page1` and inspect the headers to view the result:</span></span>

![Los encabezados de respuesta de la página OtherPages/Page1 ponen de manifiesto que OtherPagesHeader se ha agregado.](razor-pages-conventions/_static/page1-otherpages-header.png)

<span data-ttu-id="0fbd5-249">**Convención de modelo de aplicación de página**</span><span class="sxs-lookup"><span data-stu-id="0fbd5-249">**Page app model convention**</span></span>

<span data-ttu-id="0fbd5-250">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> para crear y agregar un <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> que invoque una acción en el <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> para la página con el nombre especificado.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-250">Use <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddPageApplicationModelConvention*> to create and add an <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.IPageApplicationModelConvention> that invokes an action on the <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageApplicationModel> for the page with the specified name.</span></span>

<span data-ttu-id="0fbd5-251">En el ejemplo se demuestra el uso de `AddPageApplicationModelConvention` agregando un encabezado (`AboutHeader`) a la página About:</span><span class="sxs-lookup"><span data-stu-id="0fbd5-251">The sample demonstrates the use of `AddPageApplicationModelConvention` by adding a header, `AboutHeader`, to the About page:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet7)]

<span data-ttu-id="0fbd5-252">Solicite la página About del ejemplo en `localhost:5000/About` y revise los encabezados para ver el resultado:</span><span class="sxs-lookup"><span data-stu-id="0fbd5-252">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Los encabezados de respuesta de la página About ponen de manifiesto que AboutHeader se ha agregado.](razor-pages-conventions/_static/about-page-about-header.png)

<span data-ttu-id="0fbd5-254">**Configurar un filtro**</span><span class="sxs-lookup"><span data-stu-id="0fbd5-254">**Configure a filter**</span></span>

<span data-ttu-id="0fbd5-255"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configura el filtro que se aplican.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-255"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified filter to apply.</span></span> <span data-ttu-id="0fbd5-256">Se puede implementar una clase de filtro, pero en la aplicación de ejemplo se muestra cómo implementar un filtro en una expresión lambda, cosa que sucede en segundo plano como una fábrica que devuelve un filtro:</span><span class="sxs-lookup"><span data-stu-id="0fbd5-256">You can implement a filter class, but the sample app shows how to implement a filter in a lambda expression, which is implemented behind-the-scenes as a factory that returns a filter:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet8)]

<span data-ttu-id="0fbd5-257">El modelo de páginas de aplicación se usa para comprobar la ruta de acceso relativa de los segmentos que conducen a la página Page2 en la carpeta *OtherPages*.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-257">The page app model is used to check the relative path for segments that lead to the Page2 page in the *OtherPages* folder.</span></span> <span data-ttu-id="0fbd5-258">Si la condición se supera, se agrega un encabezado.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-258">If the condition passes, a header is added.</span></span> <span data-ttu-id="0fbd5-259">Si no, se aplica `EmptyFilter`.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-259">If not, the `EmptyFilter` is applied.</span></span>

<span data-ttu-id="0fbd5-260">`EmptyFilter` es un [filtro de acciones](xref:mvc/controllers/filters#action-filters).</span><span class="sxs-lookup"><span data-stu-id="0fbd5-260">`EmptyFilter` is an [Action filter](xref:mvc/controllers/filters#action-filters).</span></span> <span data-ttu-id="0fbd5-261">Puesto que los filtros de acción se omiten las páginas de Razor, el `EmptyFilter` según lo previsto si no contiene la ruta de acceso no tiene ningún efecto `OtherPages/Page2`.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-261">Since Action filters are ignored by Razor Pages, the `EmptyFilter` has no effect as intended if the path doesn't contain `OtherPages/Page2`.</span></span>

<span data-ttu-id="0fbd5-262">Solicite la página Page2 del ejemplo en `localhost:5000/OtherPages/Page2` y revise los encabezados para ver el resultado:</span><span class="sxs-lookup"><span data-stu-id="0fbd5-262">Request the sample's Page2 page at `localhost:5000/OtherPages/Page2` and inspect the headers to view the result:</span></span>

![OtherPagesPage2Header se agrega a la respuesta de Page2.](razor-pages-conventions/_static/page2-filter-header.png)

<span data-ttu-id="0fbd5-264">**Configurar una fábrica de filtros**</span><span class="sxs-lookup"><span data-stu-id="0fbd5-264">**Configure a filter factory**</span></span>

<span data-ttu-id="0fbd5-265"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configura la fábrica especificada para aplicar [filtros](xref:mvc/controllers/filters) a todas las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-265"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.ConfigureFilter*> configures the specified factory to apply [filters](xref:mvc/controllers/filters) to all Razor Pages.</span></span>

<span data-ttu-id="0fbd5-266">En la aplicación de ejemplo se proporciona un ejemplo del uso de una [fábrica de filtros](xref:mvc/controllers/filters#ifilterfactory), para lo cual se agrega un encabezado (`FilterFactoryHeader`) con dos valores a las páginas de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="0fbd5-266">The sample app provides an example of using a [filter factory](xref:mvc/controllers/filters#ifilterfactory) by adding a header, `FilterFactoryHeader`, with two values to the app's pages:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Startup.cs?name=snippet9)]

<span data-ttu-id="0fbd5-267">*AddHeaderWithFactory.cs*:</span><span class="sxs-lookup"><span data-stu-id="0fbd5-267">*AddHeaderWithFactory.cs*:</span></span>

[!code-csharp[](razor-pages-conventions/samples/2.x/SampleApp/Factories/AddHeaderWithFactory.cs?name=snippet1)]

<span data-ttu-id="0fbd5-268">Solicite la página About del ejemplo en `localhost:5000/About` y revise los encabezados para ver el resultado:</span><span class="sxs-lookup"><span data-stu-id="0fbd5-268">Request the sample's About page at `localhost:5000/About` and inspect the headers to view the result:</span></span>

![Los encabezados de respuesta de la página About ponen de manifiesto que se han agregado dos encabezados FilterFactoryHeader.](razor-pages-conventions/_static/about-page-filter-factory-header.png)

## <a name="mvc-filters-and-the-page-filter-ipagefilter"></a><span data-ttu-id="0fbd5-270">Filtros de MVC y filtro de página (IPageFilter)</span><span class="sxs-lookup"><span data-stu-id="0fbd5-270">MVC Filters and the Page filter (IPageFilter)</span></span>

<span data-ttu-id="0fbd5-271">Las páginas de Razor no tienen en cuenta los [filtros de acciones](xref:mvc/controllers/filters#action-filters) de MVC, porque en este tipo de páginas se usan métodos de control.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-271">MVC [Action filters](xref:mvc/controllers/filters#action-filters) are ignored by Razor Pages, since Razor Pages use handler methods.</span></span> <span data-ttu-id="0fbd5-272">Otros tipos de filtros de MVC están disponibles para su uso: [Autorización](xref:mvc/controllers/filters#authorization-filters), [excepción](xref:mvc/controllers/filters#exception-filters), [recursos](xref:mvc/controllers/filters#resource-filters), y [resultado](xref:mvc/controllers/filters#result-filters).</span><span class="sxs-lookup"><span data-stu-id="0fbd5-272">Other types of MVC filters are available for you to use: [Authorization](xref:mvc/controllers/filters#authorization-filters), [Exception](xref:mvc/controllers/filters#exception-filters), [Resource](xref:mvc/controllers/filters#resource-filters), and [Result](xref:mvc/controllers/filters#result-filters).</span></span> <span data-ttu-id="0fbd5-273">Para más información, vea el tema [Filters](xref:mvc/controllers/filters) (Filtros).</span><span class="sxs-lookup"><span data-stu-id="0fbd5-273">For more information, see the [Filters](xref:mvc/controllers/filters) topic.</span></span>

<span data-ttu-id="0fbd5-274">El filtro de página (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) es un filtro que se aplica a las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="0fbd5-274">The Page filter (<xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter>) is a filter that applies to Razor Pages.</span></span> <span data-ttu-id="0fbd5-275">Para más información, vea [Filter methods for Razor Pages](xref:razor-pages/filter) (Métodos de filtrado para páginas de Razor).</span><span class="sxs-lookup"><span data-stu-id="0fbd5-275">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0fbd5-276">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="0fbd5-276">Additional resources</span></span>

* <xref:security/authorization/razor-pages-authorization>
* <xref:mvc/controllers/areas#areas-with-razor-pages>
