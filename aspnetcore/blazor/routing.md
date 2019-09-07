---
title: ASP.NET Core el enrutamiento más brillante
author: guardrex
description: Aprenda a enrutar las solicitudes en aplicaciones y el componente NavLink.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/06/2019
uid: blazor/routing
ms.openlocfilehash: d348908261c51b477aa698a407266d05c0df5a33
ms.sourcegitcommit: 43c6335b5859282f64d66a7696c5935a2bcdf966
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2019
ms.locfileid: "70800346"
---
# <a name="aspnet-core-blazor-routing"></a><span data-ttu-id="a559d-103">ASP.NET Core el enrutamiento más brillante</span><span class="sxs-lookup"><span data-stu-id="a559d-103">ASP.NET Core Blazor routing</span></span>

<span data-ttu-id="a559d-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a559d-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="a559d-105">Obtenga información acerca de cómo enrutar las solicitudes `NavLink` y cómo usar el componente para crear vínculos de navegación en aplicaciones increíbles.</span><span class="sxs-lookup"><span data-stu-id="a559d-105">Learn how to route requests and how to use the `NavLink` component to create navigation links in Blazor apps.</span></span>

## <a name="aspnet-core-endpoint-routing-integration"></a><span data-ttu-id="a559d-106">Integración del enrutamiento de puntos de conexión de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a559d-106">ASP.NET Core endpoint routing integration</span></span>

<span data-ttu-id="a559d-107">El lado servidor increíblemente integrado se integra en [ASP.net Core enrutamiento de puntos de conexión](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="a559d-107">Blazor server-side is integrated into [ASP.NET Core Endpoint Routing](xref:fundamentals/routing).</span></span> <span data-ttu-id="a559d-108">Una aplicación ASP.net Core está configurada para aceptar conexiones entrantes para `MapBlazorHub` componentes `Startup.Configure`interactivos con en:</span><span class="sxs-lookup"><span data-stu-id="a559d-108">An ASP.NET Core app is configured to accept incoming connections for interactive components with `MapBlazorHub` in `Startup.Configure`:</span></span>

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

## <a name="route-templates"></a><span data-ttu-id="a559d-109">Plantillas de ruta</span><span class="sxs-lookup"><span data-stu-id="a559d-109">Route templates</span></span>

<span data-ttu-id="a559d-110">El `Router` componente habilita el enrutamiento a cada componente con una ruta especificada.</span><span class="sxs-lookup"><span data-stu-id="a559d-110">The `Router` component enables routing to each component with a specified route.</span></span> <span data-ttu-id="a559d-111">El `Router` componente aparece en el archivo *app. Razor* :</span><span class="sxs-lookup"><span data-stu-id="a559d-111">The `Router` component appears in the *App.razor* file:</span></span>

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

<span data-ttu-id="a559d-112">Cuando se compila un archivo *. Razor* con una `@page` Directiva, se proporciona la <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> clase generada que especifica la plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="a559d-112">When a *.razor* file with an `@page` directive is compiled, the generated class is provided a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span>

<span data-ttu-id="a559d-113">En tiempo de ejecución `RouteView` , el componente:</span><span class="sxs-lookup"><span data-stu-id="a559d-113">At runtime, the `RouteView` component:</span></span>

* <span data-ttu-id="a559d-114">Recibe el `RouteData` `Router` de junto con los parámetros deseados.</span><span class="sxs-lookup"><span data-stu-id="a559d-114">Receives the `RouteData` from the `Router` along with any desired parameters.</span></span>
* <span data-ttu-id="a559d-115">Representa el componente especificado con su diseño (o un diseño predeterminado opcional) mediante los parámetros especificados.</span><span class="sxs-lookup"><span data-stu-id="a559d-115">Renders the specified component with its layout (or an optional default layout) using the specified parameters.</span></span>

<span data-ttu-id="a559d-116">Opcionalmente, puede especificar un `DefaultLayout` parámetro con una clase de diseño que se usará para los componentes que no especifican un diseño.</span><span class="sxs-lookup"><span data-stu-id="a559d-116">You can optionally specify a `DefaultLayout` parameter with a layout class to use for components that don't specify a layout.</span></span> <span data-ttu-id="a559d-117">Las plantillas increíblemente predeterminadas especifican el `MainLayout` componente.</span><span class="sxs-lookup"><span data-stu-id="a559d-117">The default Blazor templates specify the `MainLayout` component.</span></span> <span data-ttu-id="a559d-118">*MainLayout. Razor* está en la carpeta *compartida* del proyecto de plantilla.</span><span class="sxs-lookup"><span data-stu-id="a559d-118">*MainLayout.razor* is in the template project's *Shared* folder.</span></span> <span data-ttu-id="a559d-119">Para obtener más información sobre los diseños, <xref:blazor/layouts>vea.</span><span class="sxs-lookup"><span data-stu-id="a559d-119">For more information on layouts, see <xref:blazor/layouts>.</span></span>

<span data-ttu-id="a559d-120">Se pueden aplicar varias plantillas de ruta a un componente.</span><span class="sxs-lookup"><span data-stu-id="a559d-120">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="a559d-121">El siguiente componente responde a las solicitudes de `/BlazorRoute` y `/DifferentBlazorRoute`:</span><span class="sxs-lookup"><span data-stu-id="a559d-121">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

> [!IMPORTANT]
> <span data-ttu-id="a559d-122">Para que las direcciones URL se resuelvan correctamente, la `<base>` aplicación debe incluir una etiqueta en su archivo *wwwroot/index.html* (lado cliente) o en el archivo *pages/_Host. cshtml* (servidor increíble) con la ruta de acceso base de `href` la aplicación especificada en el atributo ( `<base href="/">`).</span><span class="sxs-lookup"><span data-stu-id="a559d-122">For URLs to resolve correctly, the app must include a `<base>` tag in its *wwwroot/index.html* file (Blazor client-side) or *Pages/_Host.cshtml* file (Blazor server-side) with the app base path specified in the `href` attribute (`<base href="/">`).</span></span> <span data-ttu-id="a559d-123">Para obtener más información, consulta <xref:host-and-deploy/blazor/index#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="a559d-123">For more information, see <xref:host-and-deploy/blazor/index#app-base-path>.</span></span>

## <a name="provide-custom-content-when-content-isnt-found"></a><span data-ttu-id="a559d-124">Proporcionar contenido personalizado cuando no se encuentra el contenido</span><span class="sxs-lookup"><span data-stu-id="a559d-124">Provide custom content when content isn't found</span></span>

<span data-ttu-id="a559d-125">El `Router` componente permite que la aplicación especifique el contenido personalizado si no se encuentra el contenido para la ruta solicitada.</span><span class="sxs-lookup"><span data-stu-id="a559d-125">The `Router` component allows the app to specify custom content if content isn't found for the requested route.</span></span>

<span data-ttu-id="a559d-126">En el archivo *app. Razor* , establezca el contenido personalizado en `NotFound` el parámetro de `Router` plantilla del componente:</span><span class="sxs-lookup"><span data-stu-id="a559d-126">In the *App.razor* file, set custom content in the `NotFound` template parameter of the `Router` component:</span></span>

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

<span data-ttu-id="a559d-127">El contenido de `<NotFound>` las etiquetas puede incluir elementos arbitrarios, como otros componentes interactivos.</span><span class="sxs-lookup"><span data-stu-id="a559d-127">The content of `<NotFound>` tags can include arbitrary items, such as other interactive components.</span></span> <span data-ttu-id="a559d-128">Para aplicar un diseño predeterminado al `NotFound` contenido, vea <xref:blazor/layouts>.</span><span class="sxs-lookup"><span data-stu-id="a559d-128">To apply a default layout to `NotFound` content, see <xref:blazor/layouts>.</span></span>

## <a name="route-to-components-from-multiple-assemblies"></a><span data-ttu-id="a559d-129">Enrutar a componentes de varios ensamblados</span><span class="sxs-lookup"><span data-stu-id="a559d-129">Route to components from multiple assemblies</span></span>

<span data-ttu-id="a559d-130">Use el `AdditionalAssemblies` parámetro para especificar ensamblados adicionales para `Router` que el componente tenga en cuenta al buscar componentes enrutables.</span><span class="sxs-lookup"><span data-stu-id="a559d-130">Use the `AdditionalAssemblies` parameter to specify additional assemblies for the `Router` component to consider when searching for routable components.</span></span> <span data-ttu-id="a559d-131">Los ensamblados especificados se consideran además `AppAssembly`del ensamblado especificado por.</span><span class="sxs-lookup"><span data-stu-id="a559d-131">Specified assemblies are considered in addition to the `AppAssembly`-specified assembly.</span></span> <span data-ttu-id="a559d-132">En el ejemplo siguiente, `Component1` es un componente enrutable definido en una biblioteca de clases a la que se hace referencia.</span><span class="sxs-lookup"><span data-stu-id="a559d-132">In the following example, `Component1` is a routable component defined in a referenced class library.</span></span> <span data-ttu-id="a559d-133">En el `AdditionalAssemblies` ejemplo siguiente se obtiene compatibilidad de `Component1`enrutamiento para:</span><span class="sxs-lookup"><span data-stu-id="a559d-133">The following `AdditionalAssemblies` example results in routing support for `Component1`:</span></span>

<span data-ttu-id="a559d-134">< enrutador AppAssembly = "typeof (Program). Ensamblado "AdditionalAssemblies =" New [] {typeof (Component1). Ensamblado} >...</Router></span><span class="sxs-lookup"><span data-stu-id="a559d-134"><Router AppAssembly="typeof(Program).Assembly" AdditionalAssemblies="new[] { typeof(Component1).Assembly }> ... </Router></span></span>

## <a name="route-parameters"></a><span data-ttu-id="a559d-135">Parámetros de ruta</span><span class="sxs-lookup"><span data-stu-id="a559d-135">Route parameters</span></span>

<span data-ttu-id="a559d-136">El enrutador usa parámetros de ruta para rellenar los parámetros de componente correspondientes con el mismo nombre (no distingue mayúsculas de minúsculas):</span><span class="sxs-lookup"><span data-stu-id="a559d-136">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive):</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter&highlight=2,7-8)]

<span data-ttu-id="a559d-137">Los parámetros opcionales no se admiten para las aplicaciones increíbles en ASP.NET Core vista previa de 3,0.</span><span class="sxs-lookup"><span data-stu-id="a559d-137">Optional parameters aren't supported for Blazor apps in ASP.NET Core 3.0 Preview.</span></span> <span data-ttu-id="a559d-138">En `@page` el ejemplo anterior se aplican dos directivas.</span><span class="sxs-lookup"><span data-stu-id="a559d-138">Two `@page` directives are applied in the previous example.</span></span> <span data-ttu-id="a559d-139">El primero permite la navegación al componente sin un parámetro.</span><span class="sxs-lookup"><span data-stu-id="a559d-139">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="a559d-140">La segunda `@page` Directiva toma el `{text}` parámetro Route y asigna el valor a la `Text` propiedad.</span><span class="sxs-lookup"><span data-stu-id="a559d-140">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="a559d-141">Restricciones de ruta</span><span class="sxs-lookup"><span data-stu-id="a559d-141">Route constraints</span></span>

<span data-ttu-id="a559d-142">Una restricción de ruta aplica la coincidencia de tipos en un segmento de ruta a un componente.</span><span class="sxs-lookup"><span data-stu-id="a559d-142">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="a559d-143">En el ejemplo siguiente, la ruta al `Users` componente solo coincide con si:</span><span class="sxs-lookup"><span data-stu-id="a559d-143">In the following example, the route to the `Users` component only matches if:</span></span>

* <span data-ttu-id="a559d-144">Hay `Id` un segmento de ruta en la dirección URL de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="a559d-144">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="a559d-145">El `Id` segmento es un entero (`int`).</span><span class="sxs-lookup"><span data-stu-id="a559d-145">The `Id` segment is an integer (`int`).</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.razor?highlight=1)]

<span data-ttu-id="a559d-146">Están disponibles las restricciones de ruta que se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="a559d-146">The route constraints shown in the following table are available.</span></span> <span data-ttu-id="a559d-147">Para obtener más información sobre las restricciones de ruta que coinciden con la referencia cultural de todos los idiomas, vea la advertencia que se encuentra debajo de la tabla.</span><span class="sxs-lookup"><span data-stu-id="a559d-147">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="a559d-148">Restricción</span><span class="sxs-lookup"><span data-stu-id="a559d-148">Constraint</span></span> | <span data-ttu-id="a559d-149">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="a559d-149">Example</span></span>           | <span data-ttu-id="a559d-150">Coincidencias de ejemplo</span><span class="sxs-lookup"><span data-stu-id="a559d-150">Example Matches</span></span>                                                                  | <span data-ttu-id="a559d-151">Invariable</span><span class="sxs-lookup"><span data-stu-id="a559d-151">Invariant</span></span><br><span data-ttu-id="a559d-152">culture</span><span class="sxs-lookup"><span data-stu-id="a559d-152">culture</span></span><br><span data-ttu-id="a559d-153">coincidencia</span><span class="sxs-lookup"><span data-stu-id="a559d-153">matching</span></span> |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | <span data-ttu-id="a559d-154">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="a559d-154">`true`, `FALSE`</span></span>                                                                  | <span data-ttu-id="a559d-155">Sin</span><span class="sxs-lookup"><span data-stu-id="a559d-155">No</span></span>                               |
| `datetime` | `{dob:datetime}`  | <span data-ttu-id="a559d-156">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="a559d-156">`2016-12-31`, `2016-12-31 7:32pm`</span></span>                                                | <span data-ttu-id="a559d-157">Sí</span><span class="sxs-lookup"><span data-stu-id="a559d-157">Yes</span></span>                              |
| `decimal`  | `{price:decimal}` | <span data-ttu-id="a559d-158">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="a559d-158">`49.99`, `-1,000.01`</span></span>                                                             | <span data-ttu-id="a559d-159">Sí</span><span class="sxs-lookup"><span data-stu-id="a559d-159">Yes</span></span>                              |
| `double`   | `{weight:double}` | <span data-ttu-id="a559d-160">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="a559d-160">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="a559d-161">Sí</span><span class="sxs-lookup"><span data-stu-id="a559d-161">Yes</span></span>                              |
| `float`    | `{weight:float}`  | <span data-ttu-id="a559d-162">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="a559d-162">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="a559d-163">Sí</span><span class="sxs-lookup"><span data-stu-id="a559d-163">Yes</span></span>                              |
| `guid`     | `{id:guid}`       | <span data-ttu-id="a559d-164">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="a559d-164">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="a559d-165">Sin</span><span class="sxs-lookup"><span data-stu-id="a559d-165">No</span></span>                               |
| `int`      | `{id:int}`        | <span data-ttu-id="a559d-166">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="a559d-166">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="a559d-167">Sí</span><span class="sxs-lookup"><span data-stu-id="a559d-167">Yes</span></span>                              |
| `long`     | `{ticks:long}`    | <span data-ttu-id="a559d-168">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="a559d-168">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="a559d-169">Sí</span><span class="sxs-lookup"><span data-stu-id="a559d-169">Yes</span></span>                              |

> [!WARNING]
> <span data-ttu-id="a559d-170">Las restricciones de ruta que comprueban la dirección URL y que se convierten en un tipo CLR (como `int` o `DateTime`) usan siempre la referencia cultural invariable.</span><span class="sxs-lookup"><span data-stu-id="a559d-170">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="a559d-171">Estas restricciones dan por supuesto que la dirección URL no es localizable.</span><span class="sxs-lookup"><span data-stu-id="a559d-171">These constraints assume that the URL is non-localizable.</span></span>

### <a name="routing-with-urls-that-contain-dots"></a><span data-ttu-id="a559d-172">Enrutamiento con direcciones URL que contienen puntos</span><span class="sxs-lookup"><span data-stu-id="a559d-172">Routing with URLs that contain dots</span></span>

<span data-ttu-id="a559d-173">En las aplicaciones de servidor increíbles, la ruta predeterminada en *_Host. cshtml* es `/` (`@page "/"`).</span><span class="sxs-lookup"><span data-stu-id="a559d-173">In Blazor server-side apps, the default route in *_Host.cshtml* is `/` (`@page "/"`).</span></span> <span data-ttu-id="a559d-174">Una dirección URL de solicitud que contiene un`.`punto () no coincide con la ruta predeterminada porque la dirección URL parece solicitar un archivo.</span><span class="sxs-lookup"><span data-stu-id="a559d-174">A request URL that contains a dot (`.`) isn't matched by the default route because the URL appears to request a file.</span></span> <span data-ttu-id="a559d-175">Una aplicación increíblemente alta devuelve una respuesta *404 no encontrada* para un archivo estático que no existe.</span><span class="sxs-lookup"><span data-stu-id="a559d-175">A Blazor app returns a *404 - Not Found* response for a static file that doesn't exist.</span></span> <span data-ttu-id="a559d-176">Para usar rutas que contienen un punto, configure *_Host. cshtml* con la siguiente plantilla de ruta:</span><span class="sxs-lookup"><span data-stu-id="a559d-176">To use routes that contain a dot, configure *_Host.cshtml* with the following route template:</span></span>

```cshtml
@page "/{**path}"
```

<span data-ttu-id="a559d-177">La `"/{**path}"` plantilla incluye:</span><span class="sxs-lookup"><span data-stu-id="a559d-177">The `"/{**path}"` template includes:</span></span>

* <span data-ttu-id="a559d-178">Sintaxis *catch-all de* doble asterisco (`**`) para capturar la ruta de acceso en varios límites de carpeta sin codificar barras`/`diagonales ().</span><span class="sxs-lookup"><span data-stu-id="a559d-178">Double-asterisk *catch-all* syntax (`**`) to capture the path across multiple folder boundaries without encoding forward slashes (`/`).</span></span>
* <span data-ttu-id="a559d-179">Nombre `path` del parámetro de ruta.</span><span class="sxs-lookup"><span data-stu-id="a559d-179">A `path` route parameter name.</span></span>

<span data-ttu-id="a559d-180">Para obtener más información, consulta <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="a559d-180">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="a559d-181">Componente NavLink</span><span class="sxs-lookup"><span data-stu-id="a559d-181">NavLink component</span></span>

<span data-ttu-id="a559d-182">Use un `NavLink` componente en lugar de los elementos de hipervínculo HTML (`<a>`) al crear vínculos de navegación.</span><span class="sxs-lookup"><span data-stu-id="a559d-182">Use a `NavLink` component in place of HTML hyperlink elements (`<a>`) when creating navigation links.</span></span> <span data-ttu-id="a559d-183">Un `NavLink` componente se comporta como un `<a>` elemento, salvo que alterna una `active` clase CSS en función de si `href` coincide con la dirección URL actual.</span><span class="sxs-lookup"><span data-stu-id="a559d-183">A `NavLink` component behaves like an `<a>` element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="a559d-184">La `active` clase ayuda a un usuario a entender qué página es la página activa entre los vínculos de navegación mostrados.</span><span class="sxs-lookup"><span data-stu-id="a559d-184">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span>

<span data-ttu-id="a559d-185">El componente `NavMenu` siguiente crea una barra de navegación de [bootstrap](https://getbootstrap.com/docs/) que muestra cómo `NavLink` utilizar los componentes de:</span><span class="sxs-lookup"><span data-stu-id="a559d-185">The following `NavMenu` component creates a [Bootstrap](https://getbootstrap.com/docs/) navigation bar that demonstrates how to use `NavLink` components:</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/NavMenu.razor?highlight=4,9)]

<span data-ttu-id="a559d-186">Hay dos `NavLinkMatch` opciones que puede asignar `Match` al atributo del `<NavLink>` elemento:</span><span class="sxs-lookup"><span data-stu-id="a559d-186">There are two `NavLinkMatch` options that you can assign to the `Match` attribute of the `<NavLink>` element:</span></span>

* <span data-ttu-id="a559d-187">`NavLinkMatch.All`&ndash; El`NavLink` está activo cuando coincide con toda la dirección URL actual.</span><span class="sxs-lookup"><span data-stu-id="a559d-187">`NavLinkMatch.All` &ndash; The `NavLink` is active when it matches the entire current URL.</span></span>
* <span data-ttu-id="a559d-188">`NavLinkMatch.Prefix`(*valor predeterminado*) &ndash; El`NavLink` está activo cuando coincide con cualquier prefijo de la dirección URL actual.</span><span class="sxs-lookup"><span data-stu-id="a559d-188">`NavLinkMatch.Prefix` (*default*) &ndash; The `NavLink` is active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="a559d-189">En el ejemplo anterior, el inicio `NavLink` `href=""` coincide con la dirección URL de inicio y `active` solo recibe la clase CSS en la dirección URL de la ruta de acceso `https://localhost:5001/`base predeterminada de la aplicación (por ejemplo,).</span><span class="sxs-lookup"><span data-stu-id="a559d-189">In the preceding example, the Home `NavLink` `href=""` matches the home URL and only receives the `active` CSS class at the app's default base path URL (for example, `https://localhost:5001/`).</span></span> <span data-ttu-id="a559d-190">El segundo `NavLink` recibe la `active` clase cuando el usuario visita cualquier dirección URL con `MyComponent` un prefijo (por `https://localhost:5001/MyComponent` ejemplo `https://localhost:5001/MyComponent/AnotherSegment`, y).</span><span class="sxs-lookup"><span data-stu-id="a559d-190">The second `NavLink` receives the `active` class when the user visits any URL with a `MyComponent` prefix (for example, `https://localhost:5001/MyComponent` and `https://localhost:5001/MyComponent/AnotherSegment`).</span></span>

<span data-ttu-id="a559d-191">Los `NavLink` atributos de componente adicionales se pasan a la etiqueta delimitadora representada.</span><span class="sxs-lookup"><span data-stu-id="a559d-191">Additional `NavLink` component attributes are passed through to the rendered anchor tag.</span></span> <span data-ttu-id="a559d-192">En el ejemplo siguiente, el `NavLink` componente incluye el `target` atributo:</span><span class="sxs-lookup"><span data-stu-id="a559d-192">In the following example, the `NavLink` component includes the `target` attribute:</span></span>

```cshtml
<NavLink href="my-page" target="_blank">My page</NavLink>
```

<span data-ttu-id="a559d-193">Se representa el siguiente marcado HTML:</span><span class="sxs-lookup"><span data-stu-id="a559d-193">The following HTML markup is rendered:</span></span>

```html
<a href="my-page" target="_blank" rel="noopener noreferrer">My page</a>
```

## <a name="uri-and-navigation-state-helpers"></a><span data-ttu-id="a559d-194">Aplicaciones auxiliares de estado de navegación y URI</span><span class="sxs-lookup"><span data-stu-id="a559d-194">URI and navigation state helpers</span></span>

<span data-ttu-id="a559d-195">Use `Microsoft.AspNetCore.Components.NavigationManager` para trabajar con los URI y la C# navegación en el código.</span><span class="sxs-lookup"><span data-stu-id="a559d-195">Use `Microsoft.AspNetCore.Components.NavigationManager` to work with URIs and navigation in C# code.</span></span> <span data-ttu-id="a559d-196">`NavigationManager`proporciona el evento y los métodos que se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="a559d-196">`NavigationManager` provides the event and methods shown in the following table.</span></span>

| <span data-ttu-id="a559d-197">Member</span><span class="sxs-lookup"><span data-stu-id="a559d-197">Member</span></span> | <span data-ttu-id="a559d-198">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="a559d-198">Description</span></span> |
| ------ | ----------- |
| `Uri` | <span data-ttu-id="a559d-199">Obtiene el URI absoluto actual.</span><span class="sxs-lookup"><span data-stu-id="a559d-199">Gets the current absolute URI.</span></span> |
| `BaseUri` | <span data-ttu-id="a559d-200">Obtiene el URI base (con una barra diagonal final) que se puede anteponer a las rutas de acceso del URI relativo para generar un URI absoluto.</span><span class="sxs-lookup"><span data-stu-id="a559d-200">Gets the base URI (with a trailing slash) that can be prepended to relative URI paths to produce an absolute URI.</span></span> <span data-ttu-id="a559d-201">Normalmente, `BaseUri` se corresponde con `href` el atributo en el elemento `<base>` del documento en *wwwroot/index.html* (cliente de increíble parte) o *pages/_Host. cshtml* (servidor más increíble).</span><span class="sxs-lookup"><span data-stu-id="a559d-201">Typically, `BaseUri` corresponds to the `href` attribute on the document's `<base>` element in *wwwroot/index.html* (Blazor client-side) or *Pages/_Host.cshtml* (Blazor server-side).</span></span> |
| `NavigateTo` | <span data-ttu-id="a559d-202">Navega al URI especificado.</span><span class="sxs-lookup"><span data-stu-id="a559d-202">Navigates to the specified URI.</span></span> <span data-ttu-id="a559d-203">Si `forceLoad` es `true`:</span><span class="sxs-lookup"><span data-stu-id="a559d-203">If `forceLoad` is `true`:</span></span><ul><li><span data-ttu-id="a559d-204">Se omite el enrutamiento del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="a559d-204">Client-side routing is bypassed.</span></span></li><li><span data-ttu-id="a559d-205">El explorador se ve obligado a cargar la nueva página del servidor, tanto si el identificador URI está controlado normalmente por el enrutador del lado cliente como si no.</span><span class="sxs-lookup"><span data-stu-id="a559d-205">The browser is forced to load the new page from the server, whether or not the URI is normally handled by the client-side router.</span></span></li></ul> |
| `LocationChanged` | <span data-ttu-id="a559d-206">Evento que se desencadena cuando ha cambiado la ubicación de navegación.</span><span class="sxs-lookup"><span data-stu-id="a559d-206">An event that fires when the navigation location has changed.</span></span> |
| `ToAbsoluteUri` | <span data-ttu-id="a559d-207">Convierte un URI relativo en un URI absoluto.</span><span class="sxs-lookup"><span data-stu-id="a559d-207">Converts a relative URI into an absolute URI.</span></span> |
| `ToBaseRelativePath` | <span data-ttu-id="a559d-208">Dado un URI base (por ejemplo, un URI previamente devuelto `GetBaseUri`por), convierte un URI absoluto en un URI relativo al prefijo de URI base.</span><span class="sxs-lookup"><span data-stu-id="a559d-208">Given a base URI (for example, a URI previously returned by `GetBaseUri`), converts an absolute URI into a URI relative to the base URI prefix.</span></span> |

<span data-ttu-id="a559d-209">El siguiente componente navega al componente de `Counter` la aplicación cuando se selecciona el botón:</span><span class="sxs-lookup"><span data-stu-id="a559d-209">The following component navigates to the app's `Counter` component when the button is selected:</span></span>

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
