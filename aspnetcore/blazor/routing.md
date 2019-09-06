---
title: ASP.NET Core el enrutamiento más brillante
author: guardrex
description: Aprenda a enrutar las solicitudes en aplicaciones y el componente NavLink.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/23/2019
uid: blazor/routing
ms.openlocfilehash: ae3d7ab01185dd6f2e8e0f59b78c2e693fe464b0
ms.sourcegitcommit: 8b36f75b8931ae3f656e2a8e63572080adc78513
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/05/2019
ms.locfileid: "70310342"
---
# <a name="aspnet-core-blazor-routing"></a><span data-ttu-id="feade-103">ASP.NET Core el enrutamiento más brillante</span><span class="sxs-lookup"><span data-stu-id="feade-103">ASP.NET Core Blazor routing</span></span>

<span data-ttu-id="feade-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="feade-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="feade-105">Obtenga información acerca de cómo enrutar las solicitudes `NavLink` y cómo usar el componente para crear vínculos de navegación en aplicaciones increíbles.</span><span class="sxs-lookup"><span data-stu-id="feade-105">Learn how to route requests and how to use the `NavLink` component to create navigation links in Blazor apps.</span></span>

## <a name="aspnet-core-endpoint-routing-integration"></a><span data-ttu-id="feade-106">Integración del enrutamiento de puntos de conexión de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="feade-106">ASP.NET Core endpoint routing integration</span></span>

<span data-ttu-id="feade-107">El lado servidor increíblemente integrado se integra en [ASP.net Core enrutamiento de puntos de conexión](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="feade-107">Blazor server-side is integrated into [ASP.NET Core Endpoint Routing](xref:fundamentals/routing).</span></span> <span data-ttu-id="feade-108">Una aplicación ASP.net Core está configurada para aceptar conexiones entrantes para `MapBlazorHub` componentes `Startup.Configure`interactivos con en:</span><span class="sxs-lookup"><span data-stu-id="feade-108">An ASP.NET Core app is configured to accept incoming connections for interactive components with `MapBlazorHub` in `Startup.Configure`:</span></span>

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

## <a name="route-templates"></a><span data-ttu-id="feade-109">Plantillas de ruta</span><span class="sxs-lookup"><span data-stu-id="feade-109">Route templates</span></span>

<span data-ttu-id="feade-110">El `Router` componente habilita el enrutamiento y se proporciona una plantilla de ruta a cada componente accesible.</span><span class="sxs-lookup"><span data-stu-id="feade-110">The `Router` component enables routing, and a route template is provided to each accessible component.</span></span> <span data-ttu-id="feade-111">El `Router` componente aparece en el archivo *app. Razor* :</span><span class="sxs-lookup"><span data-stu-id="feade-111">The `Router` component appears in the *App.razor* file:</span></span>

<span data-ttu-id="feade-112">En una aplicación del lado servidor o en el lado cliente:</span><span class="sxs-lookup"><span data-stu-id="feade-112">In a Blazor server-side or client-side app:</span></span>

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

<span data-ttu-id="feade-113">Cuando se compila un archivo *. Razor* con una `@page` Directiva, se proporciona la <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> clase generada que especifica la plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="feade-113">When a *.razor* file with an `@page` directive is compiled, the generated class is provided a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="feade-114">En tiempo de ejecución, el enrutador busca clases `RouteAttribute` de componentes con y representa el componente con una plantilla de ruta que coincide con la dirección URL solicitada.</span><span class="sxs-lookup"><span data-stu-id="feade-114">At runtime, the router looks for component classes with a `RouteAttribute` and renders the component with a route template that matches the requested URL.</span></span>

<span data-ttu-id="feade-115">Se pueden aplicar varias plantillas de ruta a un componente.</span><span class="sxs-lookup"><span data-stu-id="feade-115">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="feade-116">El siguiente componente responde a las solicitudes de `/BlazorRoute` y `/DifferentBlazorRoute`:</span><span class="sxs-lookup"><span data-stu-id="feade-116">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

> [!IMPORTANT]
> <span data-ttu-id="feade-117">Para que las direcciones URL se resuelvan correctamente, la `<base>` aplicación debe incluir una etiqueta en su archivo *wwwroot/index.html* (lado cliente) o en el archivo *pages/_Host. cshtml* (servidor increíble) con la ruta de acceso base de `href` la aplicación especificada en el atributo ( `<base href="/">`).</span><span class="sxs-lookup"><span data-stu-id="feade-117">For URLs to resolve correctly, the app must include a `<base>` tag in its *wwwroot/index.html* file (Blazor client-side) or *Pages/_Host.cshtml* file (Blazor server-side) with the app base path specified in the `href` attribute (`<base href="/">`).</span></span> <span data-ttu-id="feade-118">Para obtener más información, consulta <xref:host-and-deploy/blazor/client-side#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="feade-118">For more information, see <xref:host-and-deploy/blazor/client-side#app-base-path>.</span></span>

## <a name="provide-custom-content-when-content-isnt-found"></a><span data-ttu-id="feade-119">Proporcionar contenido personalizado cuando no se encuentra el contenido</span><span class="sxs-lookup"><span data-stu-id="feade-119">Provide custom content when content isn't found</span></span>

<span data-ttu-id="feade-120">El `Router` componente permite que la aplicación especifique el contenido personalizado si no se encuentra el contenido para la ruta solicitada.</span><span class="sxs-lookup"><span data-stu-id="feade-120">The `Router` component allows the app to specify custom content if content isn't found for the requested route.</span></span>

<span data-ttu-id="feade-121">En el archivo *app. Razor* , establezca el contenido personalizado en `<NotFound>` el parámetro de `Router` plantilla del componente:</span><span class="sxs-lookup"><span data-stu-id="feade-121">In the *App.razor* file, set custom content in the `<NotFound>` template parameter of the `Router` component:</span></span>

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

<span data-ttu-id="feade-122">El contenido de `<NotFound>` puede incluir elementos arbitrarios, como otros componentes interactivos.</span><span class="sxs-lookup"><span data-stu-id="feade-122">The content of `<NotFound>` can include arbitrary items, such as other interactive components.</span></span>

## <a name="route-parameters"></a><span data-ttu-id="feade-123">Parámetros de ruta</span><span class="sxs-lookup"><span data-stu-id="feade-123">Route parameters</span></span>

<span data-ttu-id="feade-124">El enrutador usa parámetros de ruta para rellenar los parámetros de componente correspondientes con el mismo nombre (no distingue mayúsculas de minúsculas):</span><span class="sxs-lookup"><span data-stu-id="feade-124">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive):</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter&highlight=2,7-8)]

<span data-ttu-id="feade-125">Los parámetros opcionales no se admiten para las aplicaciones increíbles en ASP.NET Core vista previa de 3,0.</span><span class="sxs-lookup"><span data-stu-id="feade-125">Optional parameters aren't supported for Blazor apps in ASP.NET Core 3.0 Preview.</span></span> <span data-ttu-id="feade-126">En `@page` el ejemplo anterior se aplican dos directivas.</span><span class="sxs-lookup"><span data-stu-id="feade-126">Two `@page` directives are applied in the previous example.</span></span> <span data-ttu-id="feade-127">El primero permite la navegación al componente sin un parámetro.</span><span class="sxs-lookup"><span data-stu-id="feade-127">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="feade-128">La segunda `@page` Directiva toma el `{text}` parámetro Route y asigna el valor a la `Text` propiedad.</span><span class="sxs-lookup"><span data-stu-id="feade-128">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="feade-129">Restricciones de ruta</span><span class="sxs-lookup"><span data-stu-id="feade-129">Route constraints</span></span>

<span data-ttu-id="feade-130">Una restricción de ruta aplica la coincidencia de tipos en un segmento de ruta a un componente.</span><span class="sxs-lookup"><span data-stu-id="feade-130">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="feade-131">En el ejemplo siguiente, la ruta al `Users` componente solo coincide con si:</span><span class="sxs-lookup"><span data-stu-id="feade-131">In the following example, the route to the `Users` component only matches if:</span></span>

* <span data-ttu-id="feade-132">Hay `Id` un segmento de ruta en la dirección URL de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="feade-132">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="feade-133">El `Id` segmento es un entero (`int`).</span><span class="sxs-lookup"><span data-stu-id="feade-133">The `Id` segment is an integer (`int`).</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.razor?highlight=1)]

<span data-ttu-id="feade-134">Están disponibles las restricciones de ruta que se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="feade-134">The route constraints shown in the following table are available.</span></span> <span data-ttu-id="feade-135">Para obtener más información sobre las restricciones de ruta que coinciden con la referencia cultural de todos los idiomas, vea la advertencia que se encuentra debajo de la tabla.</span><span class="sxs-lookup"><span data-stu-id="feade-135">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="feade-136">Restricción</span><span class="sxs-lookup"><span data-stu-id="feade-136">Constraint</span></span> | <span data-ttu-id="feade-137">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="feade-137">Example</span></span>           | <span data-ttu-id="feade-138">Coincidencias de ejemplo</span><span class="sxs-lookup"><span data-stu-id="feade-138">Example Matches</span></span>                                                                  | <span data-ttu-id="feade-139">Invariable</span><span class="sxs-lookup"><span data-stu-id="feade-139">Invariant</span></span><br><span data-ttu-id="feade-140">culture</span><span class="sxs-lookup"><span data-stu-id="feade-140">culture</span></span><br><span data-ttu-id="feade-141">coincidencia</span><span class="sxs-lookup"><span data-stu-id="feade-141">matching</span></span> |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | <span data-ttu-id="feade-142">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="feade-142">`true`, `FALSE`</span></span>                                                                  | <span data-ttu-id="feade-143">Sin</span><span class="sxs-lookup"><span data-stu-id="feade-143">No</span></span>                               |
| `datetime` | `{dob:datetime}`  | <span data-ttu-id="feade-144">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="feade-144">`2016-12-31`, `2016-12-31 7:32pm`</span></span>                                                | <span data-ttu-id="feade-145">Sí</span><span class="sxs-lookup"><span data-stu-id="feade-145">Yes</span></span>                              |
| `decimal`  | `{price:decimal}` | <span data-ttu-id="feade-146">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="feade-146">`49.99`, `-1,000.01`</span></span>                                                             | <span data-ttu-id="feade-147">Sí</span><span class="sxs-lookup"><span data-stu-id="feade-147">Yes</span></span>                              |
| `double`   | `{weight:double}` | <span data-ttu-id="feade-148">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="feade-148">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="feade-149">Sí</span><span class="sxs-lookup"><span data-stu-id="feade-149">Yes</span></span>                              |
| `float`    | `{weight:float}`  | <span data-ttu-id="feade-150">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="feade-150">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="feade-151">Sí</span><span class="sxs-lookup"><span data-stu-id="feade-151">Yes</span></span>                              |
| `guid`     | `{id:guid}`       | <span data-ttu-id="feade-152">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="feade-152">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="feade-153">Sin</span><span class="sxs-lookup"><span data-stu-id="feade-153">No</span></span>                               |
| `int`      | `{id:int}`        | <span data-ttu-id="feade-154">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="feade-154">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="feade-155">Sí</span><span class="sxs-lookup"><span data-stu-id="feade-155">Yes</span></span>                              |
| `long`     | `{ticks:long}`    | <span data-ttu-id="feade-156">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="feade-156">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="feade-157">Sí</span><span class="sxs-lookup"><span data-stu-id="feade-157">Yes</span></span>                              |

> [!WARNING]
> <span data-ttu-id="feade-158">Las restricciones de ruta que comprueban la dirección URL y que se convierten en un tipo CLR (como `int` o `DateTime`) usan siempre la referencia cultural invariable.</span><span class="sxs-lookup"><span data-stu-id="feade-158">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="feade-159">Estas restricciones dan por supuesto que la dirección URL no es localizable.</span><span class="sxs-lookup"><span data-stu-id="feade-159">These constraints assume that the URL is non-localizable.</span></span>

### <a name="routing-with-urls-that-contain-dots"></a><span data-ttu-id="feade-160">Enrutamiento con direcciones URL que contienen puntos</span><span class="sxs-lookup"><span data-stu-id="feade-160">Routing with URLs that contain dots</span></span>

<span data-ttu-id="feade-161">En las aplicaciones de servidor increíbles, la ruta predeterminada en *_Host. cshtml* es `/` (`@page "/"`).</span><span class="sxs-lookup"><span data-stu-id="feade-161">In Blazor server-side apps, the default route in *_Host.cshtml* is `/` (`@page "/"`).</span></span> <span data-ttu-id="feade-162">Una dirección URL de solicitud que contiene un`.`punto () no coincide con la ruta predeterminada porque la dirección URL parece solicitar un archivo.</span><span class="sxs-lookup"><span data-stu-id="feade-162">A request URL that contains a dot (`.`) isn't matched by the default route because the URL appears to request a file.</span></span> <span data-ttu-id="feade-163">Una aplicación increíblemente alta devuelve una respuesta *404 no encontrada* para un archivo estático que no existe.</span><span class="sxs-lookup"><span data-stu-id="feade-163">A Blazor app returns a *404 - Not Found* response for a static file that doesn't exist.</span></span> <span data-ttu-id="feade-164">Para usar rutas que contienen un punto, configure *_Host. cshtml* con la siguiente plantilla de ruta:</span><span class="sxs-lookup"><span data-stu-id="feade-164">To use routes that contain a dot, configure *_Host.cshtml* with the following route template:</span></span>

```cshtml
@page "/{**path}"
```

<span data-ttu-id="feade-165">La `"/{**path}"` plantilla incluye:</span><span class="sxs-lookup"><span data-stu-id="feade-165">The `"/{**path}"` template includes:</span></span>

* <span data-ttu-id="feade-166">Sintaxis *catch-all de* doble asterisco (`**`) para capturar la ruta de acceso en varios límites de carpeta sin codificar barras`/`diagonales ().</span><span class="sxs-lookup"><span data-stu-id="feade-166">Double-asterisk *catch-all* syntax (`**`) to capture the path across multiple folder boundaries without encoding forward slashes (`/`).</span></span>
* <span data-ttu-id="feade-167">Nombre `path` del parámetro de ruta.</span><span class="sxs-lookup"><span data-stu-id="feade-167">A `path` route parameter name.</span></span>

<span data-ttu-id="feade-168">Para obtener más información, consulta <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="feade-168">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="feade-169">Componente NavLink</span><span class="sxs-lookup"><span data-stu-id="feade-169">NavLink component</span></span>

<span data-ttu-id="feade-170">Use un `NavLink` componente en lugar de los elementos de hipervínculo HTML (`<a>`) al crear vínculos de navegación.</span><span class="sxs-lookup"><span data-stu-id="feade-170">Use a `NavLink` component in place of HTML hyperlink elements (`<a>`) when creating navigation links.</span></span> <span data-ttu-id="feade-171">Un `NavLink` componente se comporta como un `<a>` elemento, salvo que alterna una `active` clase CSS en función de si `href` coincide con la dirección URL actual.</span><span class="sxs-lookup"><span data-stu-id="feade-171">A `NavLink` component behaves like an `<a>` element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="feade-172">La `active` clase ayuda a un usuario a entender qué página es la página activa entre los vínculos de navegación mostrados.</span><span class="sxs-lookup"><span data-stu-id="feade-172">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span>

<span data-ttu-id="feade-173">El componente `NavMenu` siguiente crea una barra de navegación de [bootstrap](https://getbootstrap.com/docs/) que muestra cómo `NavLink` utilizar los componentes de:</span><span class="sxs-lookup"><span data-stu-id="feade-173">The following `NavMenu` component creates a [Bootstrap](https://getbootstrap.com/docs/) navigation bar that demonstrates how to use `NavLink` components:</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/NavMenu.razor?highlight=4,9)]

<span data-ttu-id="feade-174">Hay dos `NavLinkMatch` opciones que puede asignar `Match` al atributo del `<NavLink>` elemento:</span><span class="sxs-lookup"><span data-stu-id="feade-174">There are two `NavLinkMatch` options that you can assign to the `Match` attribute of the `<NavLink>` element:</span></span>

* <span data-ttu-id="feade-175">`NavLinkMatch.All`&ndash; El`NavLink` está activo cuando coincide con toda la dirección URL actual.</span><span class="sxs-lookup"><span data-stu-id="feade-175">`NavLinkMatch.All` &ndash; The `NavLink` is active when it matches the entire current URL.</span></span>
* <span data-ttu-id="feade-176">`NavLinkMatch.Prefix`(*valor predeterminado*) &ndash; El`NavLink` está activo cuando coincide con cualquier prefijo de la dirección URL actual.</span><span class="sxs-lookup"><span data-stu-id="feade-176">`NavLinkMatch.Prefix` (*default*) &ndash; The `NavLink` is active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="feade-177">En el ejemplo anterior, el inicio `NavLink` `href=""` coincide con la dirección URL de inicio y `active` solo recibe la clase CSS en la dirección URL de la ruta de acceso `https://localhost:5001/`base predeterminada de la aplicación (por ejemplo,).</span><span class="sxs-lookup"><span data-stu-id="feade-177">In the preceding example, the Home `NavLink` `href=""` matches the home URL and only receives the `active` CSS class at the app's default base path URL (for example, `https://localhost:5001/`).</span></span> <span data-ttu-id="feade-178">El segundo `NavLink` recibe la `active` clase cuando el usuario visita cualquier dirección URL con `MyComponent` un prefijo (por `https://localhost:5001/MyComponent` ejemplo `https://localhost:5001/MyComponent/AnotherSegment`, y).</span><span class="sxs-lookup"><span data-stu-id="feade-178">The second `NavLink` receives the `active` class when the user visits any URL with a `MyComponent` prefix (for example, `https://localhost:5001/MyComponent` and `https://localhost:5001/MyComponent/AnotherSegment`).</span></span>

<span data-ttu-id="feade-179">Los `NavLink` atributos de componente adicionales se pasan a la etiqueta delimitadora representada.</span><span class="sxs-lookup"><span data-stu-id="feade-179">Additional `NavLink` component attributes are passed through to the rendered anchor tag.</span></span> <span data-ttu-id="feade-180">En el ejemplo siguiente, el `NavLink` componente incluye el `target` atributo:</span><span class="sxs-lookup"><span data-stu-id="feade-180">In the following example, the `NavLink` component includes the `target` attribute:</span></span>

```cshtml
<NavLink href="my-page" target="_blank">My page</NavLink>
```

<span data-ttu-id="feade-181">Se representa el siguiente marcado HTML:</span><span class="sxs-lookup"><span data-stu-id="feade-181">The following HTML markup is rendered:</span></span>

```html
<a href="my-page" target="_blank" rel="noopener noreferrer">My page</a>
```

## <a name="uri-and-navigation-state-helpers"></a><span data-ttu-id="feade-182">Aplicaciones auxiliares de estado de navegación y URI</span><span class="sxs-lookup"><span data-stu-id="feade-182">URI and navigation state helpers</span></span>

<span data-ttu-id="feade-183">Use `Microsoft.AspNetCore.Components.NavigationManager` para trabajar con los URI y la C# navegación en el código.</span><span class="sxs-lookup"><span data-stu-id="feade-183">Use `Microsoft.AspNetCore.Components.NavigationManager` to work with URIs and navigation in C# code.</span></span> <span data-ttu-id="feade-184">`NavigationManager`proporciona el evento y los métodos que se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="feade-184">`NavigationManager` provides the event and methods shown in the following table.</span></span>

| <span data-ttu-id="feade-185">Member</span><span class="sxs-lookup"><span data-stu-id="feade-185">Member</span></span> | <span data-ttu-id="feade-186">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="feade-186">Description</span></span> |
| ------ | ----------- |
| `Uri` | <span data-ttu-id="feade-187">Obtiene el URI absoluto actual.</span><span class="sxs-lookup"><span data-stu-id="feade-187">Gets the current absolute URI.</span></span> |
| `BaseUri` | <span data-ttu-id="feade-188">Obtiene el URI base (con una barra diagonal final) que se puede anteponer a las rutas de acceso del URI relativo para generar un URI absoluto.</span><span class="sxs-lookup"><span data-stu-id="feade-188">Gets the base URI (with a trailing slash) that can be prepended to relative URI paths to produce an absolute URI.</span></span> <span data-ttu-id="feade-189">Normalmente, `BaseUri` se corresponde con `href` el atributo en el elemento `<base>` del documento en *wwwroot/index.html* (cliente de increíble parte) o *pages/_Host. cshtml* (servidor más increíble).</span><span class="sxs-lookup"><span data-stu-id="feade-189">Typically, `BaseUri` corresponds to the `href` attribute on the document's `<base>` element in *wwwroot/index.html* (Blazor client-side) or *Pages/_Host.cshtml* (Blazor server-side).</span></span> |
| `NavigateTo` | <span data-ttu-id="feade-190">Navega al URI especificado.</span><span class="sxs-lookup"><span data-stu-id="feade-190">Navigates to the specified URI.</span></span> <span data-ttu-id="feade-191">Si `forceLoad` es `true`:</span><span class="sxs-lookup"><span data-stu-id="feade-191">If `forceLoad` is `true`:</span></span><ul><li><span data-ttu-id="feade-192">Se omite el enrutamiento del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="feade-192">Client-side routing is bypassed.</span></span></li><li><span data-ttu-id="feade-193">El explorador se ve obligado a cargar la nueva página del servidor, tanto si el identificador URI está controlado normalmente por el enrutador del lado cliente como si no.</span><span class="sxs-lookup"><span data-stu-id="feade-193">The browser is forced to load the new page from the server, whether or not the URI is normally handled by the client-side router.</span></span></li></ul> |
| `LocationChanged` | <span data-ttu-id="feade-194">Evento que se desencadena cuando ha cambiado la ubicación de navegación.</span><span class="sxs-lookup"><span data-stu-id="feade-194">An event that fires when the navigation location has changed.</span></span> |
| `ToAbsoluteUri` | <span data-ttu-id="feade-195">Convierte un URI relativo en un URI absoluto.</span><span class="sxs-lookup"><span data-stu-id="feade-195">Converts a relative URI into an absolute URI.</span></span> |
| `ToBaseRelativePath` | <span data-ttu-id="feade-196">Dado un URI base (por ejemplo, un URI previamente devuelto `GetBaseUri`por), convierte un URI absoluto en un URI relativo al prefijo de URI base.</span><span class="sxs-lookup"><span data-stu-id="feade-196">Given a base URI (for example, a URI previously returned by `GetBaseUri`), converts an absolute URI into a URI relative to the base URI prefix.</span></span> |

<span data-ttu-id="feade-197">El siguiente componente navega al componente de `Counter` la aplicación cuando se selecciona el botón:</span><span class="sxs-lookup"><span data-stu-id="feade-197">The following component navigates to the app's `Counter` component when the button is selected:</span></span>

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

