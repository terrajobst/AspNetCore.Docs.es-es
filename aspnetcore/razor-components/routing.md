---
title: Enrutamiento de componentes de Razor
author: guardrex
description: Aprenda a enrutar las solicitudes en las aplicaciones y sobre el componente NavLink.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: razor-components/routing
ms.openlocfilehash: ef82fa7e0d571979a43fd8ce712bf4f22c06f9a7
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/10/2019
ms.locfileid: "59515676"
---
# <a name="razor-components-routing"></a><span data-ttu-id="547d8-103">Enrutamiento de componentes de Razor</span><span class="sxs-lookup"><span data-stu-id="547d8-103">Razor Components routing</span></span>

<span data-ttu-id="547d8-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="547d8-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="547d8-105">Aprenda a enrutar las solicitudes en las aplicaciones y sobre el componente NavLink.</span><span class="sxs-lookup"><span data-stu-id="547d8-105">Learn how to route requests in apps and about the NavLink component.</span></span>

## <a name="aspnet-core-endpoint-routing-integration"></a><span data-ttu-id="547d8-106">Integración de enrutamiento de punto de conexión de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="547d8-106">ASP.NET Core endpoint routing integration</span></span>

<span data-ttu-id="547d8-107">Componentes de Razor están integrados en [enrutamiento de ASP.NET Core](xref:fundamentals/routing).</span><span class="sxs-lookup"><span data-stu-id="547d8-107">Razor Components are integrated into [ASP.NET Core routing](xref:fundamentals/routing).</span></span> <span data-ttu-id="547d8-108">Una aplicación ASP.NET Core está configurada para aceptar las conexiones entrantes para los componentes de Razor interactivos con `MapComponentHub<TComponent>` en `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="547d8-108">An ASP.NET Core app is configured to accept incoming connections for interactive Razor Components with `MapComponentHub<TComponent>` in `Startup.Configure`.</span></span> `MapComponentHub` <span data-ttu-id="547d8-109">Especifica que el componente raíz `App` deben representar dentro de un elemento de DOM coincidentes en el selector de `app`:</span><span class="sxs-lookup"><span data-stu-id="547d8-109">specifies that the root component `App` should be rendered within a DOM element matching the selector `app`:</span></span>

```csharp
app.UseRouting(routes =>
{
    routes.MapRazorPages();
    routes.MapComponentHub<App>("app");
});
```

## <a name="route-templates"></a><span data-ttu-id="547d8-110">Plantillas de ruta</span><span class="sxs-lookup"><span data-stu-id="547d8-110">Route templates</span></span>

<span data-ttu-id="547d8-111">El `<Router>` componente habilita el enrutamiento, y se proporciona una plantilla de ruta para cada componente puede tener acceso.</span><span class="sxs-lookup"><span data-stu-id="547d8-111">The `<Router>` component enables routing, and a route template is provided to each accessible component.</span></span> <span data-ttu-id="547d8-112">El `<Router>` componente aparecerá en el *Components/App.razor* archivo:</span><span class="sxs-lookup"><span data-stu-id="547d8-112">The `<Router>` component appears in the *Components/App.razor* file:</span></span>

```cshtml
<Router AppAssembly="typeof(Program).Assembly" />
```

<span data-ttu-id="547d8-113">Cuando un *.razor* o *.cshtml* de archivos con un `@page` directiva se compila, se proporciona la clase generada una <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> especificar la plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="547d8-113">When a *.razor* or *.cshtml* file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="547d8-114">En tiempo de ejecución, el enrutador busca las clases de componente con un `RouteAttribute` y representa el componente tiene una plantilla de ruta que coincida con la dirección URL solicitada.</span><span class="sxs-lookup"><span data-stu-id="547d8-114">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="547d8-115">Varias plantillas de ruta se pueden aplicar a un componente.</span><span class="sxs-lookup"><span data-stu-id="547d8-115">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="547d8-116">El siguiente componente responde a las solicitudes de `/BlazorRoute` y `/DifferentBlazorRoute`:</span><span class="sxs-lookup"><span data-stu-id="547d8-116">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.cshtml?name=snippet_BlazorRoute)]

`<Router>` <span data-ttu-id="547d8-117">admite la configuración de un componente de reserva para el procesamiento cuando una ruta solicitada no se resuelve.</span><span class="sxs-lookup"><span data-stu-id="547d8-117">supports setting a fallback component for rendering when a requested route isn't resolved.</span></span> <span data-ttu-id="547d8-118">Habilitar este escenario de participación en estableciendo el `FallbackComponent` parámetro al tipo de la clase de componente de reserva.</span><span class="sxs-lookup"><span data-stu-id="547d8-118">Enable this opt-in scenario by setting the `FallbackComponent` parameter to the type of the fallback component class.</span></span>

<span data-ttu-id="547d8-119">En el ejemplo siguiente se establece un componente definido en *Pages/MyFallbackRazorComponent.cshtml* como el componente de reserva para un `<Router>`:</span><span class="sxs-lookup"><span data-stu-id="547d8-119">The following example sets a component defined in *Pages/MyFallbackRazorComponent.cshtml* as the fallback component for a `<Router>`:</span></span>

```cshtml
<Router ... FallbackComponent="typeof(Pages.MyFallbackRazorComponent)" />
```

> [!IMPORTANT]
> <span data-ttu-id="547d8-120">Para generar rutas correctamente, la aplicación debe incluir un `<base>` etiqueta en su *wwwroot/index.HTML* archivo con la ruta de la base de aplicación especificada en el `href` atributo (`<base href="/">`).</span><span class="sxs-lookup"><span data-stu-id="547d8-120">To generate routes properly, the app must include a `<base>` tag in its *wwwroot/index.html* file with the app base path specified in the `href` attribute (`<base href="/">`).</span></span> <span data-ttu-id="547d8-121">Para obtener más información, consulta <xref:host-and-deploy/razor-components-blazor/blazor#app-base-path>.</span><span class="sxs-lookup"><span data-stu-id="547d8-121">For more information, see <xref:host-and-deploy/razor-components-blazor/blazor#app-base-path>.</span></span>

## <a name="route-parameters"></a><span data-ttu-id="547d8-122">Parámetros de ruta</span><span class="sxs-lookup"><span data-stu-id="547d8-122">Route parameters</span></span>

<span data-ttu-id="547d8-123">El enrutador usa los parámetros de ruta para rellenar los parámetros correspondientes del componente con el mismo nombre (no distingue mayúsculas de minúsculas):</span><span class="sxs-lookup"><span data-stu-id="547d8-123">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive):</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.cshtml?name=snippet_RouteParameter&highlight=2,7-8)]

<span data-ttu-id="547d8-124">Los parámetros opcionales no se admiten aún, por lo que dos `@page` directivas se aplican en el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="547d8-124">Optional parameters aren't supported yet, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="547d8-125">La primera permite la navegación al componente sin un parámetro.</span><span class="sxs-lookup"><span data-stu-id="547d8-125">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="547d8-126">El segundo `@page` directiva toma el `{text}` parámetro de ruta y asigna el valor para el `Text` propiedad.</span><span class="sxs-lookup"><span data-stu-id="547d8-126">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="547d8-127">Restricciones de ruta</span><span class="sxs-lookup"><span data-stu-id="547d8-127">Route constraints</span></span>

<span data-ttu-id="547d8-128">Una restricción de ruta exige la búsqueda de coincidencias en un segmento de ruta a un componente de tipo.</span><span class="sxs-lookup"><span data-stu-id="547d8-128">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="547d8-129">En el ejemplo siguiente, la ruta para el componente de los usuarios coincide solo si:</span><span class="sxs-lookup"><span data-stu-id="547d8-129">In the following example, the route to the Users component only matches if:</span></span>

* <span data-ttu-id="547d8-130">Un `Id` segmento de ruta está presente en la dirección URL de solicitud.</span><span class="sxs-lookup"><span data-stu-id="547d8-130">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="547d8-131">El `Id` segmento es un entero (`int`).</span><span class="sxs-lookup"><span data-stu-id="547d8-131">The `Id` segment is an integer (`int`).</span></span>

[!code-cshtml[](routing/samples_snapshot/3.x/Constraint.cshtml?highlight=1)]

<span data-ttu-id="547d8-132">Las restricciones de ruta que se muestra en la tabla siguiente están disponibles.</span><span class="sxs-lookup"><span data-stu-id="547d8-132">The route constraints shown in the following table are available.</span></span> <span data-ttu-id="547d8-133">Para las restricciones de ruta que coinciden con la referencia cultural invariable, vea la advertencia debajo de la tabla para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="547d8-133">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="547d8-134">Restricción</span><span class="sxs-lookup"><span data-stu-id="547d8-134">Constraint</span></span> | <span data-ttu-id="547d8-135">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="547d8-135">Example</span></span>           | <span data-ttu-id="547d8-136">Coincidencias de ejemplo</span><span class="sxs-lookup"><span data-stu-id="547d8-136">Example Matches</span></span>                                                                  | <span data-ttu-id="547d8-137">Invariable</span><span class="sxs-lookup"><span data-stu-id="547d8-137">Invariant</span></span><br><span data-ttu-id="547d8-138">referencia cultural</span><span class="sxs-lookup"><span data-stu-id="547d8-138">culture</span></span><br><span data-ttu-id="547d8-139">coincidencia</span><span class="sxs-lookup"><span data-stu-id="547d8-139">matching</span></span> |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | `true`<span data-ttu-id="547d8-140">,</span><span class="sxs-lookup"><span data-stu-id="547d8-140">,</span></span> `FALSE`                                                                  | <span data-ttu-id="547d8-141">No</span><span class="sxs-lookup"><span data-stu-id="547d8-141">No</span></span>                               |
| `datetime` | `{dob:datetime}`  | `2016-12-31`<span data-ttu-id="547d8-142">,</span><span class="sxs-lookup"><span data-stu-id="547d8-142">,</span></span> `2016-12-31 7:32pm`                                                | <span data-ttu-id="547d8-143">Sí</span><span class="sxs-lookup"><span data-stu-id="547d8-143">Yes</span></span>                              |
| `decimal`  | `{price:decimal}` | `49.99`<span data-ttu-id="547d8-144">,</span><span class="sxs-lookup"><span data-stu-id="547d8-144">,</span></span> `-1,000.01`                                                             | <span data-ttu-id="547d8-145">Sí</span><span class="sxs-lookup"><span data-stu-id="547d8-145">Yes</span></span>                              |
| `double`   | `{weight:double}` | `1.234`<span data-ttu-id="547d8-146">,</span><span class="sxs-lookup"><span data-stu-id="547d8-146">,</span></span> `-1,001.01e8`                                                           | <span data-ttu-id="547d8-147">Sí</span><span class="sxs-lookup"><span data-stu-id="547d8-147">Yes</span></span>                              |
| `float`    | `{weight:float}`  | `1.234`<span data-ttu-id="547d8-148">,</span><span class="sxs-lookup"><span data-stu-id="547d8-148">,</span></span> `-1,001.01e8`                                                           | <span data-ttu-id="547d8-149">Sí</span><span class="sxs-lookup"><span data-stu-id="547d8-149">Yes</span></span>                              |
| `guid`     | `{id:guid}`       | `CD2C1638-1638-72D5-1638-DEADBEEF1638`<span data-ttu-id="547d8-150">,</span><span class="sxs-lookup"><span data-stu-id="547d8-150">,</span></span> `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | <span data-ttu-id="547d8-151">No</span><span class="sxs-lookup"><span data-stu-id="547d8-151">No</span></span>                               |
| `int`      | `{id:int}`        | `123456789`<span data-ttu-id="547d8-152">,</span><span class="sxs-lookup"><span data-stu-id="547d8-152">,</span></span> `-123456789`                                                        | <span data-ttu-id="547d8-153">Sí</span><span class="sxs-lookup"><span data-stu-id="547d8-153">Yes</span></span>                              |
| `long`     | `{ticks:long}`    | `123456789`<span data-ttu-id="547d8-154">,</span><span class="sxs-lookup"><span data-stu-id="547d8-154">,</span></span> `-123456789`                                                        | <span data-ttu-id="547d8-155">Sí</span><span class="sxs-lookup"><span data-stu-id="547d8-155">Yes</span></span>                              |

> [!WARNING]
> <span data-ttu-id="547d8-156">Las restricciones de ruta que comprueban la dirección URL y que se convierten en un tipo CLR (como `int` o `DateTime`) usan siempre la referencia cultural invariable.</span><span class="sxs-lookup"><span data-stu-id="547d8-156">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="547d8-157">Estas restricciones dan por supuesto que la dirección URL no es localizable.</span><span class="sxs-lookup"><span data-stu-id="547d8-157">These constraints assume that the URL is non-localizable.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="547d8-158">Componente NavLink</span><span class="sxs-lookup"><span data-stu-id="547d8-158">NavLink component</span></span>

<span data-ttu-id="547d8-159">Usar un componente NavLink en lugar de HTML `<a>` elementos al crear vínculos de navegación.</span><span class="sxs-lookup"><span data-stu-id="547d8-159">Use a NavLink component in place of HTML `<a>` elements when creating navigation links.</span></span> <span data-ttu-id="547d8-160">Un componente NavLink se comporta como un `<a>` elemento, excepto que alterna un `active` clase CSS en función de si su `href` coincide con la dirección URL actual.</span><span class="sxs-lookup"><span data-stu-id="547d8-160">A NavLink component behaves like an `<a>` element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="547d8-161">La `active` clase ayuda a un usuario a entender qué página es la página activa entre los vínculos de navegación que se muestran.</span><span class="sxs-lookup"><span data-stu-id="547d8-161">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span>

<span data-ttu-id="547d8-162">El siguiente componente NavMenu crea un [Bootstrap](https://getbootstrap.com/docs/) barra de navegación que se muestra cómo utilizar componentes NavLink:</span><span class="sxs-lookup"><span data-stu-id="547d8-162">The following NavMenu component creates a [Bootstrap](https://getbootstrap.com/docs/) navigation bar that demonstrates how to use NavLink components:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Shared/NavMenu.cshtml?name=snippet_NavLinks&highlight=4-6,9-11)]

<span data-ttu-id="547d8-163">Hay dos `NavLinkMatch` opciones:</span><span class="sxs-lookup"><span data-stu-id="547d8-163">There are two `NavLinkMatch` options:</span></span>

* `NavLinkMatch.All` <span data-ttu-id="547d8-164">&ndash; Especifica que el NavLink debe estar activo cuando coincide con la dirección URL actual completa.</span><span class="sxs-lookup"><span data-stu-id="547d8-164">&ndash; Specifies that the NavLink should be active when it matches the entire current URL.</span></span>
* `NavLinkMatch.Prefix` <span data-ttu-id="547d8-165">&ndash; Especifica que el NavLink debe estar activo cuando coincide con cualquier prefijo de la dirección URL actual.</span><span class="sxs-lookup"><span data-stu-id="547d8-165">&ndash; Specifies that the NavLink should be active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="547d8-166">En el ejemplo anterior, el inicio NavLink (`href=""`) coincide con todas las direcciones URL y siempre recibe la `active` clase CSS.</span><span class="sxs-lookup"><span data-stu-id="547d8-166">In the preceding example, the Home NavLink (`href=""`) matches all URLs and always receives the `active` CSS class.</span></span> <span data-ttu-id="547d8-167">El segundo NavLink solo recibe la `active` clase cuando el usuario visita el componente de ruta Blazor (`href="BlazorRoute"`).</span><span class="sxs-lookup"><span data-stu-id="547d8-167">The second NavLink only receives the `active` class when the user visits the Blazor Route component (`href="BlazorRoute"`).</span></span>
