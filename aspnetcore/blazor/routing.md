---
title: ASP.NET Core el enrutamiento más brillante
author: guardrex
description: Aprenda a enrutar las solicitudes en aplicaciones y el componente NavLink.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/21/2019
uid: blazor/routing
ms.openlocfilehash: d6fb3f03be94ff99ac3ed434265e6cd6b752c625
ms.sourcegitcommit: 04ce94b3c1b01d167f30eed60c1c95446dfe759d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/21/2019
ms.locfileid: "71176403"
---
# <a name="aspnet-core-blazor-routing"></a><span data-ttu-id="3be52-103">ASP.NET Core el enrutamiento más brillante</span><span class="sxs-lookup"><span data-stu-id="3be52-103">ASP.NET Core Blazor routing</span></span>

<span data-ttu-id="3be52-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="3be52-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="3be52-105">Obtenga información acerca de cómo enrutar las solicitudes `NavLink` y cómo usar el componente para crear vínculos de navegación en aplicaciones increíbles.</span><span class="sxs-lookup"><span data-stu-id="3be52-105">Learn how to route requests and how to use the `NavLink` component to create navigation links in Blazor apps.</span></span>

## <a name="aspnet-core-endpoint-routing-integration"></a><span data-ttu-id="3be52-106">Integración del enrutamiento de puntos de conexión de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3be52-106">ASP.NET Core endpoint routing integration</span></span>

<span data-ttu-id="3be52-107">El servidor más rápido está integrado en [ASP.net Core enrutamiento de puntos de conexión](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="3be52-107">Blazor Server is integrated into [ASP.NET Core Endpoint Routing](xref:fundamentals/routing).</span></span> <span data-ttu-id="3be52-108">Una aplicación ASP.net Core está configurada para aceptar conexiones entrantes para `MapBlazorHub` componentes `Startup.Configure`interactivos con en:</span><span class="sxs-lookup"><span data-stu-id="3be52-108">An ASP.NET Core app is configured to accept incoming connections for interactive components with `MapBlazorHub` in `Startup.Configure`:</span></span>

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

<span data-ttu-id="3be52-109">La configuración más habitual consiste en enrutar todas las solicitudes a una página de Razor, que actúa como el host para la parte del lado servidor de la aplicación de servidor de la extraordinaria.</span><span class="sxs-lookup"><span data-stu-id="3be52-109">The most typical configuration is to route all requests to a Razor page, which acts as the host for the server-side part of the Blazor Server app.</span></span> <span data-ttu-id="3be52-110">Por Convención, la página *host* se denomina normalmente *_Host. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="3be52-110">By convention, the *host* page is usually named *_Host.cshtml*.</span></span> <span data-ttu-id="3be52-111">La ruta especificada en el archivo host se denomina *ruta de reserva* porque funciona con una prioridad baja en la coincidencia de rutas.</span><span class="sxs-lookup"><span data-stu-id="3be52-111">The route specified in the host file is called a *fallback route* because it operates with a low priority in route matching.</span></span> <span data-ttu-id="3be52-112">La ruta de reserva se considera cuando otras rutas no coinciden.</span><span class="sxs-lookup"><span data-stu-id="3be52-112">The fallback route is considered when other routes don't match.</span></span> <span data-ttu-id="3be52-113">Esto permite que la aplicación use otros controladores y páginas sin interferir con la aplicación de servidor increíblemente.</span><span class="sxs-lookup"><span data-stu-id="3be52-113">This allows the app to use others controllers and pages without interfering with the Blazor Server app.</span></span>

## <a name="route-templates"></a><span data-ttu-id="3be52-114">Plantillas de ruta</span><span class="sxs-lookup"><span data-stu-id="3be52-114">Route templates</span></span>

<span data-ttu-id="3be52-115">El `Router` componente habilita el enrutamiento a cada componente con una ruta especificada.</span><span class="sxs-lookup"><span data-stu-id="3be52-115">The `Router` component enables routing to each component with a specified route.</span></span> <span data-ttu-id="3be52-116">El `Router` componente aparece en el archivo *app. Razor* :</span><span class="sxs-lookup"><span data-stu-id="3be52-116">The `Router` component appears in the *App.razor* file:</span></span>

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

<span data-ttu-id="3be52-117">Cuando se compila un archivo *. Razor* con una `@page` Directiva, se proporciona la <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> clase generada que especifica la plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="3be52-117">When a *.razor* file with an `@page` directive is compiled, the generated class is provided a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span>

<span data-ttu-id="3be52-118">En tiempo de ejecución `RouteView` , el componente:</span><span class="sxs-lookup"><span data-stu-id="3be52-118">At runtime, the `RouteView` component:</span></span>

* <span data-ttu-id="3be52-119">Recibe el `RouteData` `Router` de junto con los parámetros deseados.</span><span class="sxs-lookup"><span data-stu-id="3be52-119">Receives the `RouteData` from the `Router` along with any desired parameters.</span></span>
* <span data-ttu-id="3be52-120">Representa el componente especificado con su diseño (o un diseño predeterminado opcional) mediante los parámetros especificados.</span><span class="sxs-lookup"><span data-stu-id="3be52-120">Renders the specified component with its layout (or an optional default layout) using the specified parameters.</span></span>

<span data-ttu-id="3be52-121">Opcionalmente, puede especificar un `DefaultLayout` parámetro con una clase de diseño que se usará para los componentes que no especifican un diseño.</span><span class="sxs-lookup"><span data-stu-id="3be52-121">You can optionally specify a `DefaultLayout` parameter with a layout class to use for components that don't specify a layout.</span></span> <span data-ttu-id="3be52-122">Las plantillas increíblemente predeterminadas especifican el `MainLayout` componente.</span><span class="sxs-lookup"><span data-stu-id="3be52-122">The default Blazor templates specify the `MainLayout` component.</span></span> <span data-ttu-id="3be52-123">*MainLayout. Razor* está en la carpeta *compartida* del proyecto de plantilla.</span><span class="sxs-lookup"><span data-stu-id="3be52-123">*MainLayout.razor* is in the template project's *Shared* folder.</span></span> <span data-ttu-id="3be52-124">Para obtener más información sobre los diseños, <xref:blazor/layouts>vea.</span><span class="sxs-lookup"><span data-stu-id="3be52-124">For more information on layouts, see <xref:blazor/layouts>.</span></span>

<span data-ttu-id="3be52-125">Se pueden aplicar varias plantillas de ruta a un componente.</span><span class="sxs-lookup"><span data-stu-id="3be52-125">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="3be52-126">El siguiente componente responde a las solicitudes de `/BlazorRoute` y `/DifferentBlazorRoute`:</span><span class="sxs-lookup"><span data-stu-id="3be52-126">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

> [!IMPORTANT]
> <span data-ttu-id="3be52-127">Para que las direcciones URL se resuelvan correctamente, la `<base>` aplicación debe incluir una etiqueta en su archivo *wwwroot/index.html* (webassembly) o el archivo *pages/_Host. cshtml* (servidor increíble) con la ruta de `href` acceso base de la aplicación especificada en el atributo (`<base href="/">`).</span><span class="sxs-lookup"><span data-stu-id="3be52-127">For URLs to resolve correctly, the app must include a `<base>` tag in its *wwwroot/index.html* file (Blazor WebAssembly) or *Pages/_Host.cshtml* file (Blazor Server) with the app base path specified in the `href` attribute (`<base href="/">`).</span></span> <span data-ttu-id="3be52-128">Para obtener más información, consulta <xref:host-and-deploy/blazor/index#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="3be52-128">For more information, see <xref:host-and-deploy/blazor/index#app-base-path>.</span></span>

## <a name="provide-custom-content-when-content-isnt-found"></a><span data-ttu-id="3be52-129">Proporcionar contenido personalizado cuando no se encuentra el contenido</span><span class="sxs-lookup"><span data-stu-id="3be52-129">Provide custom content when content isn't found</span></span>

<span data-ttu-id="3be52-130">El `Router` componente permite que la aplicación especifique el contenido personalizado si no se encuentra el contenido para la ruta solicitada.</span><span class="sxs-lookup"><span data-stu-id="3be52-130">The `Router` component allows the app to specify custom content if content isn't found for the requested route.</span></span>

<span data-ttu-id="3be52-131">En el archivo *app. Razor* , establezca el contenido personalizado en `NotFound` el parámetro de `Router` plantilla del componente:</span><span class="sxs-lookup"><span data-stu-id="3be52-131">In the *App.razor* file, set custom content in the `NotFound` template parameter of the `Router` component:</span></span>

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

<span data-ttu-id="3be52-132">El contenido de `<NotFound>` las etiquetas puede incluir elementos arbitrarios, como otros componentes interactivos.</span><span class="sxs-lookup"><span data-stu-id="3be52-132">The content of `<NotFound>` tags can include arbitrary items, such as other interactive components.</span></span> <span data-ttu-id="3be52-133">Para aplicar un diseño predeterminado al `NotFound` contenido, vea <xref:blazor/layouts>.</span><span class="sxs-lookup"><span data-stu-id="3be52-133">To apply a default layout to `NotFound` content, see <xref:blazor/layouts>.</span></span>

## <a name="route-to-components-from-multiple-assemblies"></a><span data-ttu-id="3be52-134">Enrutar a componentes de varios ensamblados</span><span class="sxs-lookup"><span data-stu-id="3be52-134">Route to components from multiple assemblies</span></span>

<span data-ttu-id="3be52-135">Use el `AdditionalAssemblies` parámetro para especificar ensamblados adicionales para `Router` que el componente tenga en cuenta al buscar componentes enrutables.</span><span class="sxs-lookup"><span data-stu-id="3be52-135">Use the `AdditionalAssemblies` parameter to specify additional assemblies for the `Router` component to consider when searching for routable components.</span></span> <span data-ttu-id="3be52-136">Los ensamblados especificados se consideran además `AppAssembly`del ensamblado especificado por.</span><span class="sxs-lookup"><span data-stu-id="3be52-136">Specified assemblies are considered in addition to the `AppAssembly`-specified assembly.</span></span> <span data-ttu-id="3be52-137">En el ejemplo siguiente, `Component1` es un componente enrutable definido en una biblioteca de clases a la que se hace referencia.</span><span class="sxs-lookup"><span data-stu-id="3be52-137">In the following example, `Component1` is a routable component defined in a referenced class library.</span></span> <span data-ttu-id="3be52-138">En el `AdditionalAssemblies` ejemplo siguiente se obtiene compatibilidad de `Component1`enrutamiento para:</span><span class="sxs-lookup"><span data-stu-id="3be52-138">The following `AdditionalAssemblies` example results in routing support for `Component1`:</span></span>

<span data-ttu-id="3be52-139">< enrutador AppAssembly = "typeof (Program). Ensamblado "AdditionalAssemblies =" New [] {typeof (Component1). Ensamblado} >...</Router></span><span class="sxs-lookup"><span data-stu-id="3be52-139"><Router AppAssembly="typeof(Program).Assembly" AdditionalAssemblies="new[] { typeof(Component1).Assembly }> ... </Router></span></span>

## <a name="route-parameters"></a><span data-ttu-id="3be52-140">Parámetros de ruta</span><span class="sxs-lookup"><span data-stu-id="3be52-140">Route parameters</span></span>

<span data-ttu-id="3be52-141">El enrutador usa parámetros de ruta para rellenar los parámetros de componente correspondientes con el mismo nombre (no distingue mayúsculas de minúsculas):</span><span class="sxs-lookup"><span data-stu-id="3be52-141">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive):</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter&highlight=2,7-8)]

<span data-ttu-id="3be52-142">Los parámetros opcionales no se admiten para las aplicaciones increíbles en ASP.NET Core vista previa de 3,0.</span><span class="sxs-lookup"><span data-stu-id="3be52-142">Optional parameters aren't supported for Blazor apps in ASP.NET Core 3.0 Preview.</span></span> <span data-ttu-id="3be52-143">En `@page` el ejemplo anterior se aplican dos directivas.</span><span class="sxs-lookup"><span data-stu-id="3be52-143">Two `@page` directives are applied in the previous example.</span></span> <span data-ttu-id="3be52-144">El primero permite la navegación al componente sin un parámetro.</span><span class="sxs-lookup"><span data-stu-id="3be52-144">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="3be52-145">La segunda `@page` Directiva toma el `{text}` parámetro Route y asigna el valor a la `Text` propiedad.</span><span class="sxs-lookup"><span data-stu-id="3be52-145">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="3be52-146">Restricciones de ruta</span><span class="sxs-lookup"><span data-stu-id="3be52-146">Route constraints</span></span>

<span data-ttu-id="3be52-147">Una restricción de ruta aplica la coincidencia de tipos en un segmento de ruta a un componente.</span><span class="sxs-lookup"><span data-stu-id="3be52-147">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="3be52-148">En el ejemplo siguiente, la ruta al `Users` componente solo coincide con si:</span><span class="sxs-lookup"><span data-stu-id="3be52-148">In the following example, the route to the `Users` component only matches if:</span></span>

* <span data-ttu-id="3be52-149">Hay `Id` un segmento de ruta en la dirección URL de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="3be52-149">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="3be52-150">El `Id` segmento es un entero (`int`).</span><span class="sxs-lookup"><span data-stu-id="3be52-150">The `Id` segment is an integer (`int`).</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.razor?highlight=1)]

<span data-ttu-id="3be52-151">Están disponibles las restricciones de ruta que se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="3be52-151">The route constraints shown in the following table are available.</span></span> <span data-ttu-id="3be52-152">Para obtener más información sobre las restricciones de ruta que coinciden con la referencia cultural de todos los idiomas, vea la advertencia que se encuentra debajo de la tabla.</span><span class="sxs-lookup"><span data-stu-id="3be52-152">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="3be52-153">Restricción</span><span class="sxs-lookup"><span data-stu-id="3be52-153">Constraint</span></span> | <span data-ttu-id="3be52-154">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="3be52-154">Example</span></span>           | <span data-ttu-id="3be52-155">Coincidencias de ejemplo</span><span class="sxs-lookup"><span data-stu-id="3be52-155">Example Matches</span></span>                                                                  | <span data-ttu-id="3be52-156">Invariable</span><span class="sxs-lookup"><span data-stu-id="3be52-156">Invariant</span></span><br><span data-ttu-id="3be52-157">referencia cultural</span><span class="sxs-lookup"><span data-stu-id="3be52-157">culture</span></span><br><span data-ttu-id="3be52-158">coincidencia</span><span class="sxs-lookup"><span data-stu-id="3be52-158">matching</span></span> |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | <span data-ttu-id="3be52-159">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="3be52-159">`true`, `FALSE`</span></span>                                                                  | <span data-ttu-id="3be52-160">No</span><span class="sxs-lookup"><span data-stu-id="3be52-160">No</span></span>                               |
| `datetime` | `{dob:datetime}`  | <span data-ttu-id="3be52-161">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="3be52-161">`2016-12-31`, `2016-12-31 7:32pm`</span></span>                                                | <span data-ttu-id="3be52-162">Sí</span><span class="sxs-lookup"><span data-stu-id="3be52-162">Yes</span></span>                              |
| `decimal`  | `{price:decimal}` | <span data-ttu-id="3be52-163">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="3be52-163">`49.99`, `-1,000.01`</span></span>                                                             | <span data-ttu-id="3be52-164">Sí</span><span class="sxs-lookup"><span data-stu-id="3be52-164">Yes</span></span>                              |
| `double`   | `{weight:double}` | <span data-ttu-id="3be52-165">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="3be52-165">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="3be52-166">Sí</span><span class="sxs-lookup"><span data-stu-id="3be52-166">Yes</span></span>                              |
| `float`    | `{weight:float}`  | <span data-ttu-id="3be52-167">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="3be52-167">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="3be52-168">Sí</span><span class="sxs-lookup"><span data-stu-id="3be52-168">Yes</span></span>                              |
| `guid`     | `{id:guid}`       | <span data-ttu-id="3be52-169">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="3be52-169">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="3be52-170">No</span><span class="sxs-lookup"><span data-stu-id="3be52-170">No</span></span>                               |
| `int`      | `{id:int}`        | <span data-ttu-id="3be52-171">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="3be52-171">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="3be52-172">Sí</span><span class="sxs-lookup"><span data-stu-id="3be52-172">Yes</span></span>                              |
| `long`     | `{ticks:long}`    | <span data-ttu-id="3be52-173">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="3be52-173">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="3be52-174">Sí</span><span class="sxs-lookup"><span data-stu-id="3be52-174">Yes</span></span>                              |

> [!WARNING]
> <span data-ttu-id="3be52-175">Las restricciones de ruta que comprueban la dirección URL y que se convierten en un tipo CLR (como `int` o `DateTime`) usan siempre la referencia cultural invariable.</span><span class="sxs-lookup"><span data-stu-id="3be52-175">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="3be52-176">Estas restricciones dan por supuesto que la dirección URL no es localizable.</span><span class="sxs-lookup"><span data-stu-id="3be52-176">These constraints assume that the URL is non-localizable.</span></span>

### <a name="routing-with-urls-that-contain-dots"></a><span data-ttu-id="3be52-177">Enrutamiento con direcciones URL que contienen puntos</span><span class="sxs-lookup"><span data-stu-id="3be52-177">Routing with URLs that contain dots</span></span>

<span data-ttu-id="3be52-178">En las aplicaciones de servidor increíbles, la ruta predeterminada en *_Host. cshtml* `/` es`@page "/"`().</span><span class="sxs-lookup"><span data-stu-id="3be52-178">In Blazor Server apps, the default route in *_Host.cshtml* is `/` (`@page "/"`).</span></span> <span data-ttu-id="3be52-179">Una dirección URL de solicitud que contiene un`.`punto () no coincide con la ruta predeterminada porque la dirección URL parece solicitar un archivo.</span><span class="sxs-lookup"><span data-stu-id="3be52-179">A request URL that contains a dot (`.`) isn't matched by the default route because the URL appears to request a file.</span></span> <span data-ttu-id="3be52-180">Una aplicación increíblemente alta devuelve una respuesta *404 no encontrada* para un archivo estático que no existe.</span><span class="sxs-lookup"><span data-stu-id="3be52-180">A Blazor app returns a *404 - Not Found* response for a static file that doesn't exist.</span></span> <span data-ttu-id="3be52-181">Para usar rutas que contienen un punto, configure *_Host. cshtml* con la siguiente plantilla de ruta:</span><span class="sxs-lookup"><span data-stu-id="3be52-181">To use routes that contain a dot, configure *_Host.cshtml* with the following route template:</span></span>

```cshtml
@page "/{**path}"
```

<span data-ttu-id="3be52-182">La `"/{**path}"` plantilla incluye:</span><span class="sxs-lookup"><span data-stu-id="3be52-182">The `"/{**path}"` template includes:</span></span>

* <span data-ttu-id="3be52-183">Sintaxis *catch-all de* doble asterisco (`**`) para capturar la ruta de acceso en varios límites de carpeta sin codificar barras`/`diagonales ().</span><span class="sxs-lookup"><span data-stu-id="3be52-183">Double-asterisk *catch-all* syntax (`**`) to capture the path across multiple folder boundaries without encoding forward slashes (`/`).</span></span>
* <span data-ttu-id="3be52-184">Nombre `path` del parámetro de ruta.</span><span class="sxs-lookup"><span data-stu-id="3be52-184">A `path` route parameter name.</span></span>

<span data-ttu-id="3be52-185">Para obtener más información, consulta <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="3be52-185">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="3be52-186">Componente NavLink</span><span class="sxs-lookup"><span data-stu-id="3be52-186">NavLink component</span></span>

<span data-ttu-id="3be52-187">Use un `NavLink` componente en lugar de los elementos de hipervínculo HTML (`<a>`) al crear vínculos de navegación.</span><span class="sxs-lookup"><span data-stu-id="3be52-187">Use a `NavLink` component in place of HTML hyperlink elements (`<a>`) when creating navigation links.</span></span> <span data-ttu-id="3be52-188">Un `NavLink` componente se comporta como un `<a>` elemento, salvo que alterna una `active` clase CSS en función de si `href` coincide con la dirección URL actual.</span><span class="sxs-lookup"><span data-stu-id="3be52-188">A `NavLink` component behaves like an `<a>` element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="3be52-189">La `active` clase ayuda a un usuario a entender qué página es la página activa entre los vínculos de navegación mostrados.</span><span class="sxs-lookup"><span data-stu-id="3be52-189">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span>

<span data-ttu-id="3be52-190">El componente `NavMenu` siguiente crea una barra de navegación de [bootstrap](https://getbootstrap.com/docs/) que muestra cómo `NavLink` utilizar los componentes de:</span><span class="sxs-lookup"><span data-stu-id="3be52-190">The following `NavMenu` component creates a [Bootstrap](https://getbootstrap.com/docs/) navigation bar that demonstrates how to use `NavLink` components:</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/NavMenu.razor?highlight=4,9)]

<span data-ttu-id="3be52-191">Hay dos `NavLinkMatch` opciones que puede asignar `Match` al atributo del `<NavLink>` elemento:</span><span class="sxs-lookup"><span data-stu-id="3be52-191">There are two `NavLinkMatch` options that you can assign to the `Match` attribute of the `<NavLink>` element:</span></span>

* <span data-ttu-id="3be52-192">`NavLinkMatch.All`&ndash; El`NavLink` está activo cuando coincide con toda la dirección URL actual.</span><span class="sxs-lookup"><span data-stu-id="3be52-192">`NavLinkMatch.All` &ndash; The `NavLink` is active when it matches the entire current URL.</span></span>
* <span data-ttu-id="3be52-193">`NavLinkMatch.Prefix`(*valor predeterminado*) &ndash; El`NavLink` está activo cuando coincide con cualquier prefijo de la dirección URL actual.</span><span class="sxs-lookup"><span data-stu-id="3be52-193">`NavLinkMatch.Prefix` (*default*) &ndash; The `NavLink` is active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="3be52-194">En el ejemplo anterior, el inicio `NavLink` `href=""` coincide con la dirección URL de inicio y `active` solo recibe la clase CSS en la dirección URL de la ruta de acceso `https://localhost:5001/`base predeterminada de la aplicación (por ejemplo,).</span><span class="sxs-lookup"><span data-stu-id="3be52-194">In the preceding example, the Home `NavLink` `href=""` matches the home URL and only receives the `active` CSS class at the app's default base path URL (for example, `https://localhost:5001/`).</span></span> <span data-ttu-id="3be52-195">El segundo `NavLink` recibe la `active` clase cuando el usuario visita cualquier dirección URL con `MyComponent` un prefijo (por `https://localhost:5001/MyComponent` ejemplo `https://localhost:5001/MyComponent/AnotherSegment`, y).</span><span class="sxs-lookup"><span data-stu-id="3be52-195">The second `NavLink` receives the `active` class when the user visits any URL with a `MyComponent` prefix (for example, `https://localhost:5001/MyComponent` and `https://localhost:5001/MyComponent/AnotherSegment`).</span></span>

<span data-ttu-id="3be52-196">Los `NavLink` atributos de componente adicionales se pasan a la etiqueta delimitadora representada.</span><span class="sxs-lookup"><span data-stu-id="3be52-196">Additional `NavLink` component attributes are passed through to the rendered anchor tag.</span></span> <span data-ttu-id="3be52-197">En el ejemplo siguiente, el `NavLink` componente incluye el `target` atributo:</span><span class="sxs-lookup"><span data-stu-id="3be52-197">In the following example, the `NavLink` component includes the `target` attribute:</span></span>

```cshtml
<NavLink href="my-page" target="_blank">My page</NavLink>
```

<span data-ttu-id="3be52-198">Se representa el siguiente marcado HTML:</span><span class="sxs-lookup"><span data-stu-id="3be52-198">The following HTML markup is rendered:</span></span>

```html
<a href="my-page" target="_blank" rel="noopener noreferrer">My page</a>
```

## <a name="uri-and-navigation-state-helpers"></a><span data-ttu-id="3be52-199">Aplicaciones auxiliares de estado de navegación y URI</span><span class="sxs-lookup"><span data-stu-id="3be52-199">URI and navigation state helpers</span></span>

<span data-ttu-id="3be52-200">Use `Microsoft.AspNetCore.Components.NavigationManager` para trabajar con los URI y la C# navegación en el código.</span><span class="sxs-lookup"><span data-stu-id="3be52-200">Use `Microsoft.AspNetCore.Components.NavigationManager` to work with URIs and navigation in C# code.</span></span> <span data-ttu-id="3be52-201">`NavigationManager`proporciona el evento y los métodos que se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="3be52-201">`NavigationManager` provides the event and methods shown in the following table.</span></span>

| <span data-ttu-id="3be52-202">Miembro</span><span class="sxs-lookup"><span data-stu-id="3be52-202">Member</span></span> | <span data-ttu-id="3be52-203">Descripción</span><span class="sxs-lookup"><span data-stu-id="3be52-203">Description</span></span> |
| ------ | ----------- |
| `Uri` | <span data-ttu-id="3be52-204">Obtiene el URI absoluto actual.</span><span class="sxs-lookup"><span data-stu-id="3be52-204">Gets the current absolute URI.</span></span> |
| `BaseUri` | <span data-ttu-id="3be52-205">Obtiene el URI base (con una barra diagonal final) que se puede anteponer a las rutas de acceso del URI relativo para generar un URI absoluto.</span><span class="sxs-lookup"><span data-stu-id="3be52-205">Gets the base URI (with a trailing slash) that can be prepended to relative URI paths to produce an absolute URI.</span></span> <span data-ttu-id="3be52-206">Normalmente, `BaseUri` se corresponde con `href` el atributo del elemento del `<base>` documento en *wwwroot/index.html* (webassembler más rápido) o *pages/_Host. cshtml* (servidor increíble).</span><span class="sxs-lookup"><span data-stu-id="3be52-206">Typically, `BaseUri` corresponds to the `href` attribute on the document's `<base>` element in *wwwroot/index.html* (Blazor WebAssembly) or *Pages/_Host.cshtml* (Blazor Server).</span></span> |
| `NavigateTo` | <span data-ttu-id="3be52-207">Navega al URI especificado.</span><span class="sxs-lookup"><span data-stu-id="3be52-207">Navigates to the specified URI.</span></span> <span data-ttu-id="3be52-208">Si `forceLoad` es `true`:</span><span class="sxs-lookup"><span data-stu-id="3be52-208">If `forceLoad` is `true`:</span></span><ul><li><span data-ttu-id="3be52-209">Se omite el enrutamiento del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="3be52-209">Client-side routing is bypassed.</span></span></li><li><span data-ttu-id="3be52-210">El explorador se ve obligado a cargar la nueva página del servidor, tanto si el identificador URI está controlado normalmente por el enrutador del lado cliente como si no.</span><span class="sxs-lookup"><span data-stu-id="3be52-210">The browser is forced to load the new page from the server, whether or not the URI is normally handled by the client-side router.</span></span></li></ul> |
| `LocationChanged` | <span data-ttu-id="3be52-211">Evento que se desencadena cuando ha cambiado la ubicación de navegación.</span><span class="sxs-lookup"><span data-stu-id="3be52-211">An event that fires when the navigation location has changed.</span></span> |
| `ToAbsoluteUri` | <span data-ttu-id="3be52-212">Convierte un URI relativo en un URI absoluto.</span><span class="sxs-lookup"><span data-stu-id="3be52-212">Converts a relative URI into an absolute URI.</span></span> |
| `ToBaseRelativePath` | <span data-ttu-id="3be52-213">Dado un URI base (por ejemplo, un URI previamente devuelto `GetBaseUri`por), convierte un URI absoluto en un URI relativo al prefijo de URI base.</span><span class="sxs-lookup"><span data-stu-id="3be52-213">Given a base URI (for example, a URI previously returned by `GetBaseUri`), converts an absolute URI into a URI relative to the base URI prefix.</span></span> |

<span data-ttu-id="3be52-214">El siguiente componente navega al componente de `Counter` la aplicación cuando se selecciona el botón:</span><span class="sxs-lookup"><span data-stu-id="3be52-214">The following component navigates to the app's `Counter` component when the button is selected:</span></span>

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
