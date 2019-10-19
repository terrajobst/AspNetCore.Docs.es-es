---
title: Enrutamiento de Blazor de ASP.NET Core
author: guardrex
description: Aprenda a enrutar las solicitudes en aplicaciones y el componente NavLink.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
no-loc:
- Blazor
uid: blazor/routing
ms.openlocfilehash: d4b76c00f79f333884fa7e30b27eadc6e36de287
ms.sourcegitcommit: a166291c6708f5949c417874108332856b53b6a9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2019
ms.locfileid: "72589935"
---
# <a name="aspnet-core-opno-locblazor-routing"></a><span data-ttu-id="62f7d-103">Enrutamiento de Blazor de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="62f7d-103">ASP.NET Core Blazor routing</span></span>

<span data-ttu-id="62f7d-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="62f7d-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="62f7d-105">Obtenga información acerca de cómo enrutar las solicitudes y cómo usar el componente `NavLink` para crear vínculos de navegación en Blazor aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="62f7d-105">Learn how to route requests and how to use the `NavLink` component to create navigation links in Blazor apps.</span></span>

## <a name="aspnet-core-endpoint-routing-integration"></a><span data-ttu-id="62f7d-106">Integración del enrutamiento de puntos de conexión de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="62f7d-106">ASP.NET Core endpoint routing integration</span></span>

Blazor<span data-ttu-id="62f7d-107"> servidor se integra en [ASP.net Core el enrutamiento del punto de conexión](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="62f7d-107"> Server is integrated into [ASP.NET Core Endpoint Routing](xref:fundamentals/routing).</span></span> <span data-ttu-id="62f7d-108">Una aplicación ASP.NET Core está configurada para aceptar conexiones entrantes para componentes interactivos con `MapBlazorHub` en `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="62f7d-108">An ASP.NET Core app is configured to accept incoming connections for interactive components with `MapBlazorHub` in `Startup.Configure`:</span></span>

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

<span data-ttu-id="62f7d-109">La configuración más habitual consiste en enrutar todas las solicitudes a una página de Razor, que actúa como el host para la parte del lado servidor de la aplicación Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="62f7d-109">The most typical configuration is to route all requests to a Razor page, which acts as the host for the server-side part of the Blazor Server app.</span></span> <span data-ttu-id="62f7d-110">Por Convención, la página *host* se denomina normalmente *_Host. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="62f7d-110">By convention, the *host* page is usually named *_Host.cshtml*.</span></span> <span data-ttu-id="62f7d-111">La ruta especificada en el archivo host se denomina *ruta de reserva* porque funciona con una prioridad baja en la coincidencia de rutas.</span><span class="sxs-lookup"><span data-stu-id="62f7d-111">The route specified in the host file is called a *fallback route* because it operates with a low priority in route matching.</span></span> <span data-ttu-id="62f7d-112">La ruta de reserva se considera cuando otras rutas no coinciden.</span><span class="sxs-lookup"><span data-stu-id="62f7d-112">The fallback route is considered when other routes don't match.</span></span> <span data-ttu-id="62f7d-113">Esto permite que la aplicación use otros controladores y páginas sin interferir con la aplicación Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="62f7d-113">This allows the app to use others controllers and pages without interfering with the Blazor Server app.</span></span>

## <a name="route-templates"></a><span data-ttu-id="62f7d-114">Plantillas de ruta</span><span class="sxs-lookup"><span data-stu-id="62f7d-114">Route templates</span></span>

<span data-ttu-id="62f7d-115">El componente `Router` permite el enrutamiento a cada componente con una ruta especificada.</span><span class="sxs-lookup"><span data-stu-id="62f7d-115">The `Router` component enables routing to each component with a specified route.</span></span> <span data-ttu-id="62f7d-116">El componente `Router` aparece en el archivo *app. Razor* :</span><span class="sxs-lookup"><span data-stu-id="62f7d-116">The `Router` component appears in the *App.razor* file:</span></span>

```cshtml
<Router AppAssembly="typeof(Startup).Assembly">
    <Found Context="routeData">
        <RouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <p>Sorry, there's nothing at this address.</p>
    </NotFound>
</Router>
```

<span data-ttu-id="62f7d-117">Cuando se compila un archivo *. Razor* con una directiva `@page`, se proporciona a la clase generada un <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> que especifica la plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="62f7d-117">When a *.razor* file with an `@page` directive is compiled, the generated class is provided a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span>

<span data-ttu-id="62f7d-118">En tiempo de ejecución, el componente `RouteView`:</span><span class="sxs-lookup"><span data-stu-id="62f7d-118">At runtime, the `RouteView` component:</span></span>

* <span data-ttu-id="62f7d-119">Recibe el `RouteData` del `Router` junto con los parámetros deseados.</span><span class="sxs-lookup"><span data-stu-id="62f7d-119">Receives the `RouteData` from the `Router` along with any desired parameters.</span></span>
* <span data-ttu-id="62f7d-120">Representa el componente especificado con su diseño (o un diseño predeterminado opcional) mediante los parámetros especificados.</span><span class="sxs-lookup"><span data-stu-id="62f7d-120">Renders the specified component with its layout (or an optional default layout) using the specified parameters.</span></span>

<span data-ttu-id="62f7d-121">Opcionalmente, puede especificar un parámetro `DefaultLayout` con una clase de diseño que se usará para los componentes que no especifican un diseño.</span><span class="sxs-lookup"><span data-stu-id="62f7d-121">You can optionally specify a `DefaultLayout` parameter with a layout class to use for components that don't specify a layout.</span></span> <span data-ttu-id="62f7d-122">Las plantillas de Blazor predeterminadas especifican el componente `MainLayout`.</span><span class="sxs-lookup"><span data-stu-id="62f7d-122">The default Blazor templates specify the `MainLayout` component.</span></span> <span data-ttu-id="62f7d-123">*MainLayout. Razor* está en la carpeta *compartida* del proyecto de plantilla.</span><span class="sxs-lookup"><span data-stu-id="62f7d-123">*MainLayout.razor* is in the template project's *Shared* folder.</span></span> <span data-ttu-id="62f7d-124">Para obtener más información sobre los diseños, vea <xref:blazor/layouts>.</span><span class="sxs-lookup"><span data-stu-id="62f7d-124">For more information on layouts, see <xref:blazor/layouts>.</span></span>

<span data-ttu-id="62f7d-125">Se pueden aplicar varias plantillas de ruta a un componente.</span><span class="sxs-lookup"><span data-stu-id="62f7d-125">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="62f7d-126">El siguiente componente responde a las solicitudes de `/BlazorRoute` y `/DifferentBlazorRoute`:</span><span class="sxs-lookup"><span data-stu-id="62f7d-126">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

> [!IMPORTANT]
> <span data-ttu-id="62f7d-127">Para que las direcciones URL se resuelvan correctamente, la aplicación debe incluir una `<base>` etiqueta en su archivo *wwwroot/index.html* (Blazor webassembly) o el archivo *pages/_Host. cshtml* (Blazor Server) con la ruta de acceso base de la aplicación especificada en el atributo `href` (`<base href="/">`).</span><span class="sxs-lookup"><span data-stu-id="62f7d-127">For URLs to resolve correctly, the app must include a `<base>` tag in its *wwwroot/index.html* file (Blazor WebAssembly) or *Pages/_Host.cshtml* file (Blazor Server) with the app base path specified in the `href` attribute (`<base href="/">`).</span></span> <span data-ttu-id="62f7d-128">Para obtener más información, vea <xref:host-and-deploy/blazor/index#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="62f7d-128">For more information, see <xref:host-and-deploy/blazor/index#app-base-path>.</span></span>

## <a name="provide-custom-content-when-content-isnt-found"></a><span data-ttu-id="62f7d-129">Proporcionar contenido personalizado cuando no se encuentra el contenido</span><span class="sxs-lookup"><span data-stu-id="62f7d-129">Provide custom content when content isn't found</span></span>

<span data-ttu-id="62f7d-130">El componente `Router` permite a la aplicación especificar el contenido personalizado si no se encuentra el contenido para la ruta solicitada.</span><span class="sxs-lookup"><span data-stu-id="62f7d-130">The `Router` component allows the app to specify custom content if content isn't found for the requested route.</span></span>

<span data-ttu-id="62f7d-131">En el archivo *app. Razor* , establezca el contenido personalizado en el parámetro de plantilla `NotFound` del componente `Router`:</span><span class="sxs-lookup"><span data-stu-id="62f7d-131">In the *App.razor* file, set custom content in the `NotFound` template parameter of the `Router` component:</span></span>

```cshtml
<Router AppAssembly="typeof(Startup).Assembly">
    <Found Context="routeData">
        <RouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <h1>Sorry</h1>
        <p>Sorry, there's nothing at this address.</p> b
    </NotFound>
</Router>
```

<span data-ttu-id="62f7d-132">El contenido de las etiquetas `<NotFound>` puede incluir elementos arbitrarios, como otros componentes interactivos.</span><span class="sxs-lookup"><span data-stu-id="62f7d-132">The content of `<NotFound>` tags can include arbitrary items, such as other interactive components.</span></span> <span data-ttu-id="62f7d-133">Para aplicar un diseño predeterminado al contenido `NotFound`, consulte <xref:blazor/layouts>.</span><span class="sxs-lookup"><span data-stu-id="62f7d-133">To apply a default layout to `NotFound` content, see <xref:blazor/layouts>.</span></span>

## <a name="route-to-components-from-multiple-assemblies"></a><span data-ttu-id="62f7d-134">Enrutar a componentes de varios ensamblados</span><span class="sxs-lookup"><span data-stu-id="62f7d-134">Route to components from multiple assemblies</span></span>

<span data-ttu-id="62f7d-135">Use el parámetro `AdditionalAssemblies` para especificar ensamblados adicionales para que el componente `Router` tenga en cuenta al buscar componentes enrutables.</span><span class="sxs-lookup"><span data-stu-id="62f7d-135">Use the `AdditionalAssemblies` parameter to specify additional assemblies for the `Router` component to consider when searching for routable components.</span></span> <span data-ttu-id="62f7d-136">Los ensamblados especificados se consideran además del ensamblado especificado por el `AppAssembly`.</span><span class="sxs-lookup"><span data-stu-id="62f7d-136">Specified assemblies are considered in addition to the `AppAssembly`-specified assembly.</span></span> <span data-ttu-id="62f7d-137">En el ejemplo siguiente, `Component1` es un componente enrutable definido en una biblioteca de clases a la que se hace referencia.</span><span class="sxs-lookup"><span data-stu-id="62f7d-137">In the following example, `Component1` is a routable component defined in a referenced class library.</span></span> <span data-ttu-id="62f7d-138">El siguiente ejemplo `AdditionalAssemblies` da como resultado la compatibilidad con el enrutamiento de `Component1`:</span><span class="sxs-lookup"><span data-stu-id="62f7d-138">The following `AdditionalAssemblies` example results in routing support for `Component1`:</span></span>

```cshtml
<Router
    AppAssembly="typeof(Program).Assembly"
    AdditionalAssemblies="new[] { typeof(Component1).Assembly }">
    ...
</Router>
```

## <a name="route-parameters"></a><span data-ttu-id="62f7d-139">Parámetros de ruta</span><span class="sxs-lookup"><span data-stu-id="62f7d-139">Route parameters</span></span>

<span data-ttu-id="62f7d-140">El enrutador usa parámetros de ruta para rellenar los parámetros de componente correspondientes con el mismo nombre (no distingue mayúsculas de minúsculas):</span><span class="sxs-lookup"><span data-stu-id="62f7d-140">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive):</span></span>

[!code-cshtml[](common/samples/3.x/BlazorWebAssemblySample/Pages/RouteParameter.razor?name=snippet_RouteParameter&highlight=2,7-8)]

<span data-ttu-id="62f7d-141">Los parámetros opcionales no se admiten para las aplicaciones de Blazor en ASP.NET Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="62f7d-141">Optional parameters aren't supported for Blazor apps in ASP.NET Core 3.0.</span></span> <span data-ttu-id="62f7d-142">En el ejemplo anterior se aplican dos directivas `@page`.</span><span class="sxs-lookup"><span data-stu-id="62f7d-142">Two `@page` directives are applied in the previous example.</span></span> <span data-ttu-id="62f7d-143">El primero permite la navegación al componente sin un parámetro.</span><span class="sxs-lookup"><span data-stu-id="62f7d-143">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="62f7d-144">La segunda Directiva `@page` toma el parámetro de ruta `{text}` y asigna el valor a la propiedad `Text`.</span><span class="sxs-lookup"><span data-stu-id="62f7d-144">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="62f7d-145">Restricciones de ruta</span><span class="sxs-lookup"><span data-stu-id="62f7d-145">Route constraints</span></span>

<span data-ttu-id="62f7d-146">Una restricción de ruta aplica la coincidencia de tipos en un segmento de ruta a un componente.</span><span class="sxs-lookup"><span data-stu-id="62f7d-146">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="62f7d-147">En el ejemplo siguiente, la ruta al componente `Users` solo coincide con si:</span><span class="sxs-lookup"><span data-stu-id="62f7d-147">In the following example, the route to the `Users` component only matches if:</span></span>

* <span data-ttu-id="62f7d-148">Existe un segmento de ruta `Id` en la dirección URL de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="62f7d-148">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="62f7d-149">El segmento `Id` es un entero (`int`).</span><span class="sxs-lookup"><span data-stu-id="62f7d-149">The `Id` segment is an integer (`int`).</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.razor?highlight=1)]

<span data-ttu-id="62f7d-150">Están disponibles las restricciones de ruta que se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="62f7d-150">The route constraints shown in the following table are available.</span></span> <span data-ttu-id="62f7d-151">Para obtener más información sobre las restricciones de ruta que coinciden con la referencia cultural de todos los idiomas, vea la advertencia que se encuentra debajo de la tabla.</span><span class="sxs-lookup"><span data-stu-id="62f7d-151">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="62f7d-152">Restricción</span><span class="sxs-lookup"><span data-stu-id="62f7d-152">Constraint</span></span> | <span data-ttu-id="62f7d-153">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="62f7d-153">Example</span></span>           | <span data-ttu-id="62f7d-154">Coincidencias de ejemplo</span><span class="sxs-lookup"><span data-stu-id="62f7d-154">Example Matches</span></span>                                                                  | <span data-ttu-id="62f7d-155">Invariable</span><span class="sxs-lookup"><span data-stu-id="62f7d-155">Invariant</span></span><br><span data-ttu-id="62f7d-156">referencia cultural</span><span class="sxs-lookup"><span data-stu-id="62f7d-156">culture</span></span><br><span data-ttu-id="62f7d-157">coincidencia</span><span class="sxs-lookup"><span data-stu-id="62f7d-157">matching</span></span> |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | <span data-ttu-id="62f7d-158">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="62f7d-158">`true`, `FALSE`</span></span>                                                                  | <span data-ttu-id="62f7d-159">No</span><span class="sxs-lookup"><span data-stu-id="62f7d-159">No</span></span>                               |
| `datetime` | `{dob:datetime}`  | <span data-ttu-id="62f7d-160">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="62f7d-160">`2016-12-31`, `2016-12-31 7:32pm`</span></span>                                                | <span data-ttu-id="62f7d-161">Sí</span><span class="sxs-lookup"><span data-stu-id="62f7d-161">Yes</span></span>                              |
| `decimal`  | `{price:decimal}` | <span data-ttu-id="62f7d-162">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="62f7d-162">`49.99`, `-1,000.01`</span></span>                                                             | <span data-ttu-id="62f7d-163">Sí</span><span class="sxs-lookup"><span data-stu-id="62f7d-163">Yes</span></span>                              |
| `double`   | `{weight:double}` | <span data-ttu-id="62f7d-164">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="62f7d-164">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="62f7d-165">Sí</span><span class="sxs-lookup"><span data-stu-id="62f7d-165">Yes</span></span>                              |
| `float`    | `{weight:float}`  | <span data-ttu-id="62f7d-166">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="62f7d-166">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="62f7d-167">Sí</span><span class="sxs-lookup"><span data-stu-id="62f7d-167">Yes</span></span>                              |
| `guid`     | `{id:guid}`       | <span data-ttu-id="62f7d-168">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="62f7d-168">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="62f7d-169">No</span><span class="sxs-lookup"><span data-stu-id="62f7d-169">No</span></span>                               |
| `int`      | `{id:int}`        | <span data-ttu-id="62f7d-170">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="62f7d-170">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="62f7d-171">Sí</span><span class="sxs-lookup"><span data-stu-id="62f7d-171">Yes</span></span>                              |
| `long`     | `{ticks:long}`    | <span data-ttu-id="62f7d-172">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="62f7d-172">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="62f7d-173">Sí</span><span class="sxs-lookup"><span data-stu-id="62f7d-173">Yes</span></span>                              |

> [!WARNING]
> <span data-ttu-id="62f7d-174">Las restricciones de ruta que comprueban la dirección URL y que se convierten en un tipo CLR (como `int` o `DateTime`) usan siempre la referencia cultural invariable.</span><span class="sxs-lookup"><span data-stu-id="62f7d-174">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="62f7d-175">Estas restricciones dan por supuesto que la dirección URL no es localizable.</span><span class="sxs-lookup"><span data-stu-id="62f7d-175">These constraints assume that the URL is non-localizable.</span></span>

### <a name="routing-with-urls-that-contain-dots"></a><span data-ttu-id="62f7d-176">Enrutamiento con direcciones URL que contienen puntos</span><span class="sxs-lookup"><span data-stu-id="62f7d-176">Routing with URLs that contain dots</span></span>

<span data-ttu-id="62f7d-177">En Blazor aplicaciones de servidor, la ruta predeterminada en *_Host. cshtml* es `/` (`@page "/"`).</span><span class="sxs-lookup"><span data-stu-id="62f7d-177">In Blazor Server apps, the default route in *_Host.cshtml* is `/` (`@page "/"`).</span></span> <span data-ttu-id="62f7d-178">La ruta predeterminada no coincide con la dirección URL de la solicitud que contiene un punto (`.`) porque la dirección URL parece solicitar un archivo.</span><span class="sxs-lookup"><span data-stu-id="62f7d-178">A request URL that contains a dot (`.`) isn't matched by the default route because the URL appears to request a file.</span></span> <span data-ttu-id="62f7d-179">Una aplicación Blazor devuelve una respuesta *404 no encontrada* para un archivo estático que no existe.</span><span class="sxs-lookup"><span data-stu-id="62f7d-179">A Blazor app returns a *404 - Not Found* response for a static file that doesn't exist.</span></span> <span data-ttu-id="62f7d-180">Para usar rutas que contienen un punto, configure *_Host. cshtml* con la siguiente plantilla de ruta:</span><span class="sxs-lookup"><span data-stu-id="62f7d-180">To use routes that contain a dot, configure *_Host.cshtml* with the following route template:</span></span>

```cshtml
@page "/{**path}"
```

<span data-ttu-id="62f7d-181">La plantilla `"/{**path}"` incluye:</span><span class="sxs-lookup"><span data-stu-id="62f7d-181">The `"/{**path}"` template includes:</span></span>

* <span data-ttu-id="62f7d-182">Sintaxis *catch-all de* doble asterisco (`**`) para capturar la ruta de acceso en varios límites de carpeta sin codificar barras diagonales (`/`).</span><span class="sxs-lookup"><span data-stu-id="62f7d-182">Double-asterisk *catch-all* syntax (`**`) to capture the path across multiple folder boundaries without encoding forward slashes (`/`).</span></span>
* <span data-ttu-id="62f7d-183">Un nombre de parámetro de ruta `path`.</span><span class="sxs-lookup"><span data-stu-id="62f7d-183">A `path` route parameter name.</span></span>

<span data-ttu-id="62f7d-184">Para obtener más información, vea <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="62f7d-184">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="62f7d-185">Componente NavLink</span><span class="sxs-lookup"><span data-stu-id="62f7d-185">NavLink component</span></span>

<span data-ttu-id="62f7d-186">Use un componente `NavLink` en lugar de los elementos de hipervínculo HTML (`<a>`) al crear vínculos de navegación.</span><span class="sxs-lookup"><span data-stu-id="62f7d-186">Use a `NavLink` component in place of HTML hyperlink elements (`<a>`) when creating navigation links.</span></span> <span data-ttu-id="62f7d-187">Un componente de `NavLink` se comporta como un elemento `<a>`, salvo que alterna una clase `active` CSS en función de si su `href` coincide con la dirección URL actual.</span><span class="sxs-lookup"><span data-stu-id="62f7d-187">A `NavLink` component behaves like an `<a>` element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="62f7d-188">La clase `active` ayuda a los usuarios a entender qué página es la página activa entre los vínculos de navegación mostrados.</span><span class="sxs-lookup"><span data-stu-id="62f7d-188">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span>

<span data-ttu-id="62f7d-189">El siguiente componente `NavMenu` crea una barra de navegación de [arranque](https://getbootstrap.com/docs/) que muestra cómo usar los componentes de `NavLink`:</span><span class="sxs-lookup"><span data-stu-id="62f7d-189">The following `NavMenu` component creates a [Bootstrap](https://getbootstrap.com/docs/) navigation bar that demonstrates how to use `NavLink` components:</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/NavMenu.razor?highlight=4,9)]

<span data-ttu-id="62f7d-190">Hay dos opciones `NavLinkMatch` que puede asignar al atributo `Match` del elemento `<NavLink>`:</span><span class="sxs-lookup"><span data-stu-id="62f7d-190">There are two `NavLinkMatch` options that you can assign to the `Match` attribute of the `<NavLink>` element:</span></span>

* <span data-ttu-id="62f7d-191">`NavLinkMatch.All` &ndash; la `NavLink` está activa cuando coincide con toda la dirección URL actual.</span><span class="sxs-lookup"><span data-stu-id="62f7d-191">`NavLinkMatch.All` &ndash; The `NavLink` is active when it matches the entire current URL.</span></span>
* <span data-ttu-id="62f7d-192">`NavLinkMatch.Prefix` (*valor predeterminado*) &ndash; el `NavLink` está activo cuando coincide con cualquier prefijo de la dirección URL actual.</span><span class="sxs-lookup"><span data-stu-id="62f7d-192">`NavLinkMatch.Prefix` (*default*) &ndash; The `NavLink` is active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="62f7d-193">En el ejemplo anterior, el `href=""` de `NavLink` Home coincide con la dirección URL de inicio y solo recibe la clase `active` CSS en la dirección URL de la ruta de acceso base predeterminada de la aplicación (por ejemplo, `https://localhost:5001/`).</span><span class="sxs-lookup"><span data-stu-id="62f7d-193">In the preceding example, the Home `NavLink` `href=""` matches the home URL and only receives the `active` CSS class at the app's default base path URL (for example, `https://localhost:5001/`).</span></span> <span data-ttu-id="62f7d-194">La segunda `NavLink` recibe la `active` clase cuando el usuario visita cualquier dirección URL con un prefijo de `MyComponent` (por ejemplo, `https://localhost:5001/MyComponent` y `https://localhost:5001/MyComponent/AnotherSegment`).</span><span class="sxs-lookup"><span data-stu-id="62f7d-194">The second `NavLink` receives the `active` class when the user visits any URL with a `MyComponent` prefix (for example, `https://localhost:5001/MyComponent` and `https://localhost:5001/MyComponent/AnotherSegment`).</span></span>

<span data-ttu-id="62f7d-195">Los atributos adicionales del componente `NavLink` se pasan a la etiqueta delimitadora representada.</span><span class="sxs-lookup"><span data-stu-id="62f7d-195">Additional `NavLink` component attributes are passed through to the rendered anchor tag.</span></span> <span data-ttu-id="62f7d-196">En el ejemplo siguiente, el componente `NavLink` incluye el atributo `target`:</span><span class="sxs-lookup"><span data-stu-id="62f7d-196">In the following example, the `NavLink` component includes the `target` attribute:</span></span>

```cshtml
<NavLink href="my-page" target="_blank">My page</NavLink>
```

<span data-ttu-id="62f7d-197">Se representa el siguiente marcado HTML:</span><span class="sxs-lookup"><span data-stu-id="62f7d-197">The following HTML markup is rendered:</span></span>

```html
<a href="my-page" target="_blank" rel="noopener noreferrer">My page</a>
```

## <a name="uri-and-navigation-state-helpers"></a><span data-ttu-id="62f7d-198">Aplicaciones auxiliares de estado de navegación y URI</span><span class="sxs-lookup"><span data-stu-id="62f7d-198">URI and navigation state helpers</span></span>

<span data-ttu-id="62f7d-199">Use `Microsoft.AspNetCore.Components.NavigationManager` para trabajar con los URI y la C# navegación en el código.</span><span class="sxs-lookup"><span data-stu-id="62f7d-199">Use `Microsoft.AspNetCore.Components.NavigationManager` to work with URIs and navigation in C# code.</span></span> <span data-ttu-id="62f7d-200">`NavigationManager` proporciona el evento y los métodos que se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="62f7d-200">`NavigationManager` provides the event and methods shown in the following table.</span></span>

| <span data-ttu-id="62f7d-201">Miembro</span><span class="sxs-lookup"><span data-stu-id="62f7d-201">Member</span></span> | <span data-ttu-id="62f7d-202">Descripción</span><span class="sxs-lookup"><span data-stu-id="62f7d-202">Description</span></span> |
| ------ | ----------- |
| `Uri` | <span data-ttu-id="62f7d-203">Obtiene el URI absoluto actual.</span><span class="sxs-lookup"><span data-stu-id="62f7d-203">Gets the current absolute URI.</span></span> |
| `BaseUri` | <span data-ttu-id="62f7d-204">Obtiene el URI base (con una barra diagonal final) que se puede anteponer a las rutas de acceso del URI relativo para generar un URI absoluto.</span><span class="sxs-lookup"><span data-stu-id="62f7d-204">Gets the base URI (with a trailing slash) that can be prepended to relative URI paths to produce an absolute URI.</span></span> <span data-ttu-id="62f7d-205">Normalmente, `BaseUri` corresponde al atributo `href` en el elemento `<base>` del documento en *wwwroot/index.html* (Blazor webassembly) o *pages/_Host. cshtml* (Blazor Server).</span><span class="sxs-lookup"><span data-stu-id="62f7d-205">Typically, `BaseUri` corresponds to the `href` attribute on the document's `<base>` element in *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server).</span></span> |
| `NavigateTo` | <span data-ttu-id="62f7d-206">Navega al URI especificado.</span><span class="sxs-lookup"><span data-stu-id="62f7d-206">Navigates to the specified URI.</span></span> <span data-ttu-id="62f7d-207">Si `forceLoad` es `true`:</span><span class="sxs-lookup"><span data-stu-id="62f7d-207">If `forceLoad` is `true`:</span></span><ul><li><span data-ttu-id="62f7d-208">Se omite el enrutamiento del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="62f7d-208">Client-side routing is bypassed.</span></span></li><li><span data-ttu-id="62f7d-209">El explorador se ve obligado a cargar la nueva página del servidor, tanto si el identificador URI está controlado normalmente por el enrutador del lado cliente como si no.</span><span class="sxs-lookup"><span data-stu-id="62f7d-209">The browser is forced to load the new page from the server, whether or not the URI is normally handled by the client-side router.</span></span></li></ul> |
| `LocationChanged` | <span data-ttu-id="62f7d-210">Evento que se desencadena cuando ha cambiado la ubicación de navegación.</span><span class="sxs-lookup"><span data-stu-id="62f7d-210">An event that fires when the navigation location has changed.</span></span> |
| `ToAbsoluteUri` | <span data-ttu-id="62f7d-211">Convierte un URI relativo en un URI absoluto.</span><span class="sxs-lookup"><span data-stu-id="62f7d-211">Converts a relative URI into an absolute URI.</span></span> |
| `ToBaseRelativePath` | <span data-ttu-id="62f7d-212">Dado un URI base (por ejemplo, un URI previamente devuelto por `GetBaseUri`), convierte un URI absoluto en un URI relativo al prefijo de URI base.</span><span class="sxs-lookup"><span data-stu-id="62f7d-212">Given a base URI (for example, a URI previously returned by `GetBaseUri`), converts an absolute URI into a URI relative to the base URI prefix.</span></span> |

<span data-ttu-id="62f7d-213">El siguiente componente navega al componente `Counter` de la aplicación cuando se selecciona el botón:</span><span class="sxs-lookup"><span data-stu-id="62f7d-213">The following component navigates to the app's `Counter` component when the button is selected:</span></span>

```cshtml
@page "/navigate"
@inject NavigationManager NavigationManager

<h1>Navigate in Code Example</h1>

<button class="btn btn-primary" @onclick="NavigateToCounterComponent">
    Navigate to the Counter component
</button>

@code {
    private void NavigateToCounterComponent()
    {
        NavigationManager.NavigateTo("counter");
    }
}
```
