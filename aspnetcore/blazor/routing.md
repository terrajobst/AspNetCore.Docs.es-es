---
title: ASP.NET Core el enrutamiento más brillante
author: guardrex
description: Aprenda a enrutar las solicitudes en aplicaciones y el componente NavLink.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/23/2019
uid: blazor/routing
ms.openlocfilehash: 067dad657c1e89a31fac45fdfa095cce4b10798d
ms.sourcegitcommit: e6bd2bbe5683e9a7dbbc2f2eab644986e6dc8a87
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/03/2019
ms.locfileid: "70238066"
---
# <a name="aspnet-core-blazor-routing"></a><span data-ttu-id="81322-103">ASP.NET Core el enrutamiento más brillante</span><span class="sxs-lookup"><span data-stu-id="81322-103">ASP.NET Core Blazor routing</span></span>

<span data-ttu-id="81322-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="81322-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="81322-105">Obtenga información acerca de cómo enrutar las solicitudes `NavLink` y cómo usar el componente para crear vínculos de navegación en aplicaciones increíbles.</span><span class="sxs-lookup"><span data-stu-id="81322-105">Learn how to route requests and how to use the `NavLink` component to create navigation links in Blazor apps.</span></span>

## <a name="aspnet-core-endpoint-routing-integration"></a><span data-ttu-id="81322-106">Integración del enrutamiento de puntos de conexión de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="81322-106">ASP.NET Core endpoint routing integration</span></span>

<span data-ttu-id="81322-107">El lado servidor increíblemente integrado se integra en [ASP.net Core enrutamiento de puntos de conexión](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="81322-107">Blazor server-side is integrated into [ASP.NET Core Endpoint Routing](xref:fundamentals/routing).</span></span> <span data-ttu-id="81322-108">Una aplicación ASP.net Core está configurada para aceptar conexiones entrantes para `MapBlazorHub` componentes `Startup.Configure`interactivos con en:</span><span class="sxs-lookup"><span data-stu-id="81322-108">An ASP.NET Core app is configured to accept incoming connections for interactive components with `MapBlazorHub` in `Startup.Configure`:</span></span>

[!code-csharp[](routing/samples_snapshot/3.x/Startup.cs?highlight=5)]

## <a name="route-templates"></a><span data-ttu-id="81322-109">Plantillas de ruta</span><span class="sxs-lookup"><span data-stu-id="81322-109">Route templates</span></span>

<span data-ttu-id="81322-110">El `Router` componente habilita el enrutamiento y se proporciona una plantilla de ruta a cada componente accesible.</span><span class="sxs-lookup"><span data-stu-id="81322-110">The `Router` component enables routing, and a route template is provided to each accessible component.</span></span> <span data-ttu-id="81322-111">El `Router` componente aparece en el archivo *app. Razor* :</span><span class="sxs-lookup"><span data-stu-id="81322-111">The `Router` component appears in the *App.razor* file:</span></span>

<span data-ttu-id="81322-112">En una aplicación del lado servidor increíblemente alta:</span><span class="sxs-lookup"><span data-stu-id="81322-112">In a Blazor server-side app:</span></span>

```cshtml
<Router AppAssembly="typeof(Startup).Assembly" />
```

<span data-ttu-id="81322-113">En una aplicación de cliente increíblemente alta:</span><span class="sxs-lookup"><span data-stu-id="81322-113">In a Blazor client-side app:</span></span>

```cshtml
<Router AppAssembly="typeof(Program).Assembly" />
```

<span data-ttu-id="81322-114">Cuando se compila un archivo *. Razor* con una `@page` Directiva, se proporciona la <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> clase generada que especifica la plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="81322-114">When a *.razor* file with an `@page` directive is compiled, the generated class is provided a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="81322-115">En tiempo de ejecución, el enrutador busca clases `RouteAttribute` de componentes con y representa el componente con una plantilla de ruta que coincide con la dirección URL solicitada.</span><span class="sxs-lookup"><span data-stu-id="81322-115">At runtime, the router looks for component classes with a `RouteAttribute` and renders the component with a route template that matches the requested URL.</span></span>

<span data-ttu-id="81322-116">Se pueden aplicar varias plantillas de ruta a un componente.</span><span class="sxs-lookup"><span data-stu-id="81322-116">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="81322-117">El siguiente componente responde a las solicitudes de `/BlazorRoute` y `/DifferentBlazorRoute`:</span><span class="sxs-lookup"><span data-stu-id="81322-117">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.razor?name=snippet_BlazorRoute)]

> [!IMPORTANT]
> <span data-ttu-id="81322-118">Para generar rutas correctamente, la aplicación debe incluir una `<base>` etiqueta en su archivo *wwwroot/index.html* (lado cliente) o el archivo *pages/_Host. cshtml* (lado servidor) con la ruta de acceso base de la aplicación especificada en el `href` atributo ( `<base href="/">`).</span><span class="sxs-lookup"><span data-stu-id="81322-118">To generate routes properly, the app must include a `<base>` tag in its *wwwroot/index.html* file (Blazor client-side) or *Pages/_Host.cshtml* file (Blazor server-side) with the app base path specified in the `href` attribute (`<base href="/">`).</span></span> <span data-ttu-id="81322-119">Para obtener más información, consulta <xref:host-and-deploy/blazor/client-side#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="81322-119">For more information, see <xref:host-and-deploy/blazor/client-side#app-base-path>.</span></span>

## <a name="provide-custom-content-when-content-isnt-found"></a><span data-ttu-id="81322-120">Proporcionar contenido personalizado cuando no se encuentra el contenido</span><span class="sxs-lookup"><span data-stu-id="81322-120">Provide custom content when content isn't found</span></span>

<span data-ttu-id="81322-121">El `Router` componente permite que la aplicación especifique el contenido personalizado si no se encuentra el contenido para la ruta solicitada.</span><span class="sxs-lookup"><span data-stu-id="81322-121">The `Router` component allows the app to specify custom content if content isn't found for the requested route.</span></span>

<span data-ttu-id="81322-122">En el archivo *app. Razor* , establezca el contenido personalizado en `<NotFoundContent>` el elemento del `Router` componente:</span><span class="sxs-lookup"><span data-stu-id="81322-122">In the *App.razor* file, set custom content in the `<NotFoundContent>` element of the `Router` component:</span></span>

```cshtml
<Router AppAssembly="typeof(Startup).Assembly">
    <NotFoundContent>
        <h1>Sorry</h1>
        <p>Sorry, there's nothing at this address.</p> b
    </NotFoundContent>
</Router>
```

<span data-ttu-id="81322-123">El contenido de `<NotFoundContent>` puede incluir elementos arbitrarios, como otros componentes interactivos.</span><span class="sxs-lookup"><span data-stu-id="81322-123">The content of `<NotFoundContent>` can include arbitrary items, such as other interactive components.</span></span>

## <a name="route-parameters"></a><span data-ttu-id="81322-124">Parámetros de ruta</span><span class="sxs-lookup"><span data-stu-id="81322-124">Route parameters</span></span>

<span data-ttu-id="81322-125">El enrutador usa parámetros de ruta para rellenar los parámetros de componente correspondientes con el mismo nombre (no distingue mayúsculas de minúsculas):</span><span class="sxs-lookup"><span data-stu-id="81322-125">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive):</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.razor?name=snippet_RouteParameter&highlight=2,7-8)]

<span data-ttu-id="81322-126">Los parámetros opcionales no se admiten para las aplicaciones increíbles en ASP.NET Core vista previa de 3,0.</span><span class="sxs-lookup"><span data-stu-id="81322-126">Optional parameters aren't supported for Blazor apps in ASP.NET Core 3.0 Preview.</span></span> <span data-ttu-id="81322-127">En `@page` el ejemplo anterior se aplican dos directivas.</span><span class="sxs-lookup"><span data-stu-id="81322-127">Two `@page` directives are applied in the previous example.</span></span> <span data-ttu-id="81322-128">El primero permite la navegación al componente sin un parámetro.</span><span class="sxs-lookup"><span data-stu-id="81322-128">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="81322-129">La segunda `@page` Directiva toma el `{text}` parámetro Route y asigna el valor a la `Text` propiedad.</span><span class="sxs-lookup"><span data-stu-id="81322-129">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="81322-130">Restricciones de ruta</span><span class="sxs-lookup"><span data-stu-id="81322-130">Route constraints</span></span>

<span data-ttu-id="81322-131">Una restricción de ruta aplica la coincidencia de tipos en un segmento de ruta a un componente.</span><span class="sxs-lookup"><span data-stu-id="81322-131">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="81322-132">En el ejemplo siguiente, la ruta al `Users` componente solo coincide con si:</span><span class="sxs-lookup"><span data-stu-id="81322-132">In the following example, the route to the `Users` component only matches if:</span></span>

* <span data-ttu-id="81322-133">Hay `Id` un segmento de ruta en la dirección URL de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="81322-133">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="81322-134">El `Id` segmento es un entero (`int`).</span><span class="sxs-lookup"><span data-stu-id="81322-134">The `Id` segment is an integer (`int`).</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.razor?highlight=1)]

<span data-ttu-id="81322-135">Están disponibles las restricciones de ruta que se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="81322-135">The route constraints shown in the following table are available.</span></span> <span data-ttu-id="81322-136">Para obtener más información sobre las restricciones de ruta que coinciden con la referencia cultural de todos los idiomas, vea la advertencia que se encuentra debajo de la tabla.</span><span class="sxs-lookup"><span data-stu-id="81322-136">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="81322-137">Restricción</span><span class="sxs-lookup"><span data-stu-id="81322-137">Constraint</span></span> | <span data-ttu-id="81322-138">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="81322-138">Example</span></span>           | <span data-ttu-id="81322-139">Coincidencias de ejemplo</span><span class="sxs-lookup"><span data-stu-id="81322-139">Example Matches</span></span>                                                                  | <span data-ttu-id="81322-140">Invariable</span><span class="sxs-lookup"><span data-stu-id="81322-140">Invariant</span></span><br><span data-ttu-id="81322-141">culture</span><span class="sxs-lookup"><span data-stu-id="81322-141">culture</span></span><br><span data-ttu-id="81322-142">coincidencia</span><span class="sxs-lookup"><span data-stu-id="81322-142">matching</span></span> |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | <span data-ttu-id="81322-143">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="81322-143">`true`, `FALSE`</span></span>                                                                  | <span data-ttu-id="81322-144">Sin</span><span class="sxs-lookup"><span data-stu-id="81322-144">No</span></span>                               |
| `datetime` | `{dob:datetime}`  | <span data-ttu-id="81322-145">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="81322-145">`2016-12-31`, `2016-12-31 7:32pm`</span></span>                                                | <span data-ttu-id="81322-146">Sí</span><span class="sxs-lookup"><span data-stu-id="81322-146">Yes</span></span>                              |
| `decimal`  | `{price:decimal}` | <span data-ttu-id="81322-147">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="81322-147">`49.99`, `-1,000.01`</span></span>                                                             | <span data-ttu-id="81322-148">Sí</span><span class="sxs-lookup"><span data-stu-id="81322-148">Yes</span></span>                              |
| `double`   | `{weight:double}` | <span data-ttu-id="81322-149">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="81322-149">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="81322-150">Sí</span><span class="sxs-lookup"><span data-stu-id="81322-150">Yes</span></span>                              |
| `float`    | `{weight:float}`  | <span data-ttu-id="81322-151">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="81322-151">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="81322-152">Sí</span><span class="sxs-lookup"><span data-stu-id="81322-152">Yes</span></span>                              |
| `guid`     | `{id:guid}`       | <span data-ttu-id="81322-153">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="81322-153">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="81322-154">Sin</span><span class="sxs-lookup"><span data-stu-id="81322-154">No</span></span>                               |
| `int`      | `{id:int}`        | <span data-ttu-id="81322-155">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="81322-155">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="81322-156">Sí</span><span class="sxs-lookup"><span data-stu-id="81322-156">Yes</span></span>                              |
| `long`     | `{ticks:long}`    | <span data-ttu-id="81322-157">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="81322-157">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="81322-158">Sí</span><span class="sxs-lookup"><span data-stu-id="81322-158">Yes</span></span>                              |

> [!WARNING]
> <span data-ttu-id="81322-159">Las restricciones de ruta que comprueban la dirección URL y que se convierten en un tipo CLR (como `int` o `DateTime`) usan siempre la referencia cultural invariable.</span><span class="sxs-lookup"><span data-stu-id="81322-159">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="81322-160">Estas restricciones dan por supuesto que la dirección URL no es localizable.</span><span class="sxs-lookup"><span data-stu-id="81322-160">These constraints assume that the URL is non-localizable.</span></span>

### <a name="routing-with-urls-that-contain-dots"></a><span data-ttu-id="81322-161">Enrutamiento con direcciones URL que contienen puntos</span><span class="sxs-lookup"><span data-stu-id="81322-161">Routing with URLs that contain dots</span></span>

<span data-ttu-id="81322-162">En las aplicaciones de servidor increíbles, la ruta predeterminada en *_Host. cshtml* es `/` (`@page "/"`).</span><span class="sxs-lookup"><span data-stu-id="81322-162">In Blazor server-side apps, the default route in *_Host.cshtml* is `/` (`@page "/"`).</span></span> <span data-ttu-id="81322-163">Una dirección URL de solicitud que contiene un`.`punto () no coincide con la ruta predeterminada porque la dirección URL parece solicitar un archivo.</span><span class="sxs-lookup"><span data-stu-id="81322-163">A request URL that contains a dot (`.`) isn't matched by the default route because the URL appears to request a file.</span></span> <span data-ttu-id="81322-164">Una aplicación increíblemente alta devuelve una respuesta *404 no encontrada* para un archivo estático que no existe.</span><span class="sxs-lookup"><span data-stu-id="81322-164">A Blazor app returns a *404 - Not Found* response for a static file that doesn't exist.</span></span> <span data-ttu-id="81322-165">Para usar rutas que contienen un punto, configure *_Host. cshtml* con la siguiente plantilla de ruta:</span><span class="sxs-lookup"><span data-stu-id="81322-165">To use routes that contain a dot, configure *_Host.cshtml* with the following route template:</span></span>

```cshtml
@page "/{**path}"
```

<span data-ttu-id="81322-166">La `"/{**path}"` plantilla incluye:</span><span class="sxs-lookup"><span data-stu-id="81322-166">The `"/{**path}"` template includes:</span></span>

* <span data-ttu-id="81322-167">Sintaxis *catch-all de* doble asterisco (`**`) para capturar la ruta de acceso en varios límites de carpeta sin codificar barras`/`diagonales ().</span><span class="sxs-lookup"><span data-stu-id="81322-167">Double-asterisk *catch-all* syntax (`**`) to capture the path across multiple folder boundaries without encoding forward slashes (`/`).</span></span>
* <span data-ttu-id="81322-168">Nombre `path` del parámetro de ruta.</span><span class="sxs-lookup"><span data-stu-id="81322-168">A `path` route parameter name.</span></span>

<span data-ttu-id="81322-169">Para obtener más información, consulta <xref:fundamentals/routing>.</span><span class="sxs-lookup"><span data-stu-id="81322-169">For more information, see <xref:fundamentals/routing>.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="81322-170">Componente NavLink</span><span class="sxs-lookup"><span data-stu-id="81322-170">NavLink component</span></span>

<span data-ttu-id="81322-171">Use un `NavLink` componente en lugar de los elementos de hipervínculo HTML (`<a>`) al crear vínculos de navegación.</span><span class="sxs-lookup"><span data-stu-id="81322-171">Use a `NavLink` component in place of HTML hyperlink elements (`<a>`) when creating navigation links.</span></span> <span data-ttu-id="81322-172">Un `NavLink` componente se comporta como un `<a>` elemento, salvo que alterna una `active` clase CSS en función de si `href` coincide con la dirección URL actual.</span><span class="sxs-lookup"><span data-stu-id="81322-172">A `NavLink` component behaves like an `<a>` element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="81322-173">La `active` clase ayuda a un usuario a entender qué página es la página activa entre los vínculos de navegación mostrados.</span><span class="sxs-lookup"><span data-stu-id="81322-173">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span>

<span data-ttu-id="81322-174">El componente `NavMenu` siguiente crea una barra de navegación de [bootstrap](https://getbootstrap.com/docs/) que muestra cómo `NavLink` utilizar los componentes de:</span><span class="sxs-lookup"><span data-stu-id="81322-174">The following `NavMenu` component creates a [Bootstrap](https://getbootstrap.com/docs/) navigation bar that demonstrates how to use `NavLink` components:</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/NavMenu.razor?highlight=4,9)]

<span data-ttu-id="81322-175">Hay dos `NavLinkMatch` opciones que puede asignar `Match` al atributo del `<NavLink>` elemento:</span><span class="sxs-lookup"><span data-stu-id="81322-175">There are two `NavLinkMatch` options that you can assign to the `Match` attribute of the `<NavLink>` element:</span></span>

* <span data-ttu-id="81322-176">`NavLinkMatch.All`&ndash; El`NavLink` está activo cuando coincide con toda la dirección URL actual.</span><span class="sxs-lookup"><span data-stu-id="81322-176">`NavLinkMatch.All` &ndash; The `NavLink` is active when it matches the entire current URL.</span></span>
* <span data-ttu-id="81322-177">`NavLinkMatch.Prefix`(*valor predeterminado*) &ndash; El`NavLink` está activo cuando coincide con cualquier prefijo de la dirección URL actual.</span><span class="sxs-lookup"><span data-stu-id="81322-177">`NavLinkMatch.Prefix` (*default*) &ndash; The `NavLink` is active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="81322-178">En el ejemplo anterior, el inicio `NavLink` `href=""` coincide con la dirección URL de inicio y `active` solo recibe la clase CSS en la dirección URL de la ruta de acceso `https://localhost:5001/`base predeterminada de la aplicación (por ejemplo,).</span><span class="sxs-lookup"><span data-stu-id="81322-178">In the preceding example, the Home `NavLink` `href=""` matches the home URL and only receives the `active` CSS class at the app's default base path URL (for example, `https://localhost:5001/`).</span></span> <span data-ttu-id="81322-179">El segundo `NavLink` recibe la `active` clase cuando el usuario visita cualquier dirección URL con `MyComponent` un prefijo (por `https://localhost:5001/MyComponent` ejemplo `https://localhost:5001/MyComponent/AnotherSegment`, y).</span><span class="sxs-lookup"><span data-stu-id="81322-179">The second `NavLink` receives the `active` class when the user visits any URL with a `MyComponent` prefix (for example, `https://localhost:5001/MyComponent` and `https://localhost:5001/MyComponent/AnotherSegment`).</span></span>

<span data-ttu-id="81322-180">Los `NavLink` atributos de componente adicionales se pasan a la etiqueta delimitadora representada.</span><span class="sxs-lookup"><span data-stu-id="81322-180">Additional `NavLink` component attributes are passed through to the rendered anchor tag.</span></span> <span data-ttu-id="81322-181">En el ejemplo siguiente, el `NavLink` componente incluye el `target` atributo:</span><span class="sxs-lookup"><span data-stu-id="81322-181">In the following example, the `NavLink` component includes the `target` attribute:</span></span>

```cshtml
<NavLink href="my-page" target="_blank">My page</NavLink>
```

<span data-ttu-id="81322-182">Se representa el siguiente marcado HTML:</span><span class="sxs-lookup"><span data-stu-id="81322-182">The following HTML markup is rendered:</span></span>

```html
<a href="my-page" target="_blank" rel="noopener noreferrer">My page</a>
```

## <a name="uri-and-navigation-state-helpers"></a><span data-ttu-id="81322-183">Aplicaciones auxiliares de estado de navegación y URI</span><span class="sxs-lookup"><span data-stu-id="81322-183">URI and navigation state helpers</span></span>

<span data-ttu-id="81322-184">Use `Microsoft.AspNetCore.Components.IUriHelper` para trabajar con los URI y la C# navegación en el código.</span><span class="sxs-lookup"><span data-stu-id="81322-184">Use `Microsoft.AspNetCore.Components.IUriHelper` to work with URIs and navigation in C# code.</span></span> <span data-ttu-id="81322-185">`IUriHelper`proporciona el evento y los métodos que se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="81322-185">`IUriHelper` provides the event and methods shown in the following table.</span></span>

| <span data-ttu-id="81322-186">Member</span><span class="sxs-lookup"><span data-stu-id="81322-186">Member</span></span> | <span data-ttu-id="81322-187">DESCRIPCIÓN</span><span class="sxs-lookup"><span data-stu-id="81322-187">Description</span></span> |
| ------ | ----------- |
| `GetAbsoluteUri` | <span data-ttu-id="81322-188">Obtiene el URI absoluto actual.</span><span class="sxs-lookup"><span data-stu-id="81322-188">Gets the current absolute URI.</span></span> |
| `GetBaseUri` | <span data-ttu-id="81322-189">Obtiene el URI base (con una barra diagonal final) que se puede anteponer a las rutas de acceso del URI relativo para generar un URI absoluto.</span><span class="sxs-lookup"><span data-stu-id="81322-189">Gets the base URI (with a trailing slash) that can be prepended to relative URI paths to produce an absolute URI.</span></span> <span data-ttu-id="81322-190">Normalmente, `GetBaseUri` se corresponde con `href` el atributo en el elemento `<base>` del documento en *wwwroot/index.html* (cliente de increíble parte) o *pages/_Host. cshtml* (servidor más increíble).</span><span class="sxs-lookup"><span data-stu-id="81322-190">Typically, `GetBaseUri` corresponds to the `href` attribute on the document's `<base>` element in *wwwroot/index.html* (Blazor client-side) or *Pages/_Host.cshtml* (Blazor server-side).</span></span> |
| `NavigateTo` | <span data-ttu-id="81322-191">Navega al URI especificado.</span><span class="sxs-lookup"><span data-stu-id="81322-191">Navigates to the specified URI.</span></span> <span data-ttu-id="81322-192">Si `forceLoad` es `true`:</span><span class="sxs-lookup"><span data-stu-id="81322-192">If `forceLoad` is `true`:</span></span><ul><li><span data-ttu-id="81322-193">Se omite el enrutamiento del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="81322-193">Client-side routing is bypassed.</span></span></li><li><span data-ttu-id="81322-194">El explorador se ve obligado a cargar la nueva página del servidor, tanto si el identificador URI está controlado normalmente por el enrutador del lado cliente como si no.</span><span class="sxs-lookup"><span data-stu-id="81322-194">The browser is forced to load the new page from the server, whether or not the URI is normally handled by the client-side router.</span></span></li></ul> |
| `OnLocationChanged` | <span data-ttu-id="81322-195">Evento que se desencadena cuando ha cambiado la ubicación de navegación.</span><span class="sxs-lookup"><span data-stu-id="81322-195">An event that fires when the navigation location has changed.</span></span> |
| `ToAbsoluteUri` | <span data-ttu-id="81322-196">Convierte un URI relativo en un URI absoluto.</span><span class="sxs-lookup"><span data-stu-id="81322-196">Converts a relative URI into an absolute URI.</span></span> |
| `ToBaseRelativePath` | <span data-ttu-id="81322-197">Dado un URI base (por ejemplo, un URI previamente devuelto `GetBaseUri`por), convierte un URI absoluto en un URI relativo al prefijo de URI base.</span><span class="sxs-lookup"><span data-stu-id="81322-197">Given a base URI (for example, a URI previously returned by `GetBaseUri`), converts an absolute URI into a URI relative to the base URI prefix.</span></span> |

<span data-ttu-id="81322-198">El siguiente componente navega al componente de `Counter` la aplicación cuando se selecciona el botón:</span><span class="sxs-lookup"><span data-stu-id="81322-198">The following component navigates to the app's `Counter` component when the button is selected:</span></span>

```cshtml
@page "/navigate"
@using Microsoft.AspNetCore.Components
@inject IUriHelper UriHelper

<h1>Navigate in Code Example</h1>

<button class="btn btn-primary" @onclick="NavigateToCounterComponent">
    Navigate to the Counter component
</button>

@code {
    private void NavigateToCounterComponent()
    {
        UriHelper.NavigateTo("counter");
    }
}
```

