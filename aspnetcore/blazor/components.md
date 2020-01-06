---
title: Crear y usar ASP.NET Core componentes de Razor
author: guardrex
description: Obtenga información sobre cómo crear y usar componentes de Razor, incluido cómo enlazar a datos, controlar eventos y administrar ciclos de vida de componentes.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/28/2019
no-loc:
- Blazor
uid: blazor/components
ms.openlocfilehash: 87f21d84c17e5bbd1247bb955acee81384b890e7
ms.sourcegitcommit: 47d453f34b6fd0179119c572cb8be64c5365cbb6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/02/2020
ms.locfileid: "75597907"
---
# <a name="create-and-use-aspnet-core-razor-components"></a><span data-ttu-id="9f81b-103">Crear y usar ASP.NET Core componentes de Razor</span><span class="sxs-lookup"><span data-stu-id="9f81b-103">Create and use ASP.NET Core Razor components</span></span>

<span data-ttu-id="9f81b-104">Por [Luke Latham](https://github.com/guardrex) y [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="9f81b-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="9f81b-105">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9f81b-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

Blazor<span data-ttu-id="9f81b-106"> aplicaciones se compilan con *componentes*de.</span><span class="sxs-lookup"><span data-stu-id="9f81b-106"> apps are built using *components*.</span></span> <span data-ttu-id="9f81b-107">Un componente es un fragmento independiente de la interfaz de usuario (IU), como una página, un cuadro de diálogo o un formulario.</span><span class="sxs-lookup"><span data-stu-id="9f81b-107">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="9f81b-108">Un componente incluye el marcado HTML y la lógica de procesamiento necesaria para insertar datos o responder a los eventos de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="9f81b-108">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="9f81b-109">Los componentes son flexibles y ligeros.</span><span class="sxs-lookup"><span data-stu-id="9f81b-109">Components are flexible and lightweight.</span></span> <span data-ttu-id="9f81b-110">Se pueden anidar, reutilizar y compartir entre proyectos.</span><span class="sxs-lookup"><span data-stu-id="9f81b-110">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="9f81b-111">Clases de componentes</span><span class="sxs-lookup"><span data-stu-id="9f81b-111">Component classes</span></span>

<span data-ttu-id="9f81b-112">Los componentes se implementan en archivos de componentes de [Razor](xref:mvc/views/razor) ( *. Razor*) C# mediante una combinación de y el marcado HTML.</span><span class="sxs-lookup"><span data-stu-id="9f81b-112">Components are implemented in [Razor](xref:mvc/views/razor) component files (*.razor*) using a combination of C# and HTML markup.</span></span> <span data-ttu-id="9f81b-113">Un componente de Blazor se conoce formalmente como un *componente de Razor*.</span><span class="sxs-lookup"><span data-stu-id="9f81b-113">A component in Blazor is formally referred to as a *Razor component*.</span></span>

<span data-ttu-id="9f81b-114">El nombre de un componente debe empezar con un carácter en mayúsculas.</span><span class="sxs-lookup"><span data-stu-id="9f81b-114">A component's name must start with an uppercase character.</span></span> <span data-ttu-id="9f81b-115">Por ejemplo, *MyCoolComponent. Razor* es válido y *MyCoolComponent. Razor* no es válido.</span><span class="sxs-lookup"><span data-stu-id="9f81b-115">For example, *MyCoolComponent.razor* is valid, and *myCoolComponent.razor* is invalid.</span></span>

<span data-ttu-id="9f81b-116">La interfaz de usuario de un componente se define mediante HTML.</span><span class="sxs-lookup"><span data-stu-id="9f81b-116">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="9f81b-117">La lógica de la representación dinámica (por ejemplo, bucles, instrucciones condicionales, expresiones) se agrega mediante una sintaxis de C# insertada denominada [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="9f81b-117">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="9f81b-118">Cuando se compila una aplicación, el marcado HTML y C# la lógica de representación se convierten en una clase de componente.</span><span class="sxs-lookup"><span data-stu-id="9f81b-118">When an app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="9f81b-119">El nombre de la clase generada coincide con el nombre del archivo.</span><span class="sxs-lookup"><span data-stu-id="9f81b-119">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="9f81b-120">Los miembros de la clase de componente se definen en un bloque `@code`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-120">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="9f81b-121">En el bloque `@code`, el estado de componente (propiedades, campos) se especifica con métodos para el control de eventos o para definir otra lógica de componentes.</span><span class="sxs-lookup"><span data-stu-id="9f81b-121">In the `@code` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span> <span data-ttu-id="9f81b-122">Se permite emplear más de un bloque `@code`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-122">More than one `@code` block is permissible.</span></span>

> [!NOTE]
> <span data-ttu-id="9f81b-123">En las versiones preliminares anteriores de ASP.NET Core 3,0, `@functions` bloques se usaban para el mismo propósito que `@code` bloques en los componentes de Razor.</span><span class="sxs-lookup"><span data-stu-id="9f81b-123">In prior previews of ASP.NET Core 3.0, `@functions` blocks were used for the same purpose as `@code` blocks in Razor components.</span></span> <span data-ttu-id="9f81b-124">`@functions` bloques continúan funcionando en los componentes de Razor, pero se recomienda usar el bloque de `@code` en ASP.NET Core 3,0 Preview 6 o posterior.</span><span class="sxs-lookup"><span data-stu-id="9f81b-124">`@functions` blocks continue to function in Razor components, but we recommend using the `@code` block in ASP.NET Core 3.0 Preview 6 or later.</span></span>

<span data-ttu-id="9f81b-125">Los miembros de componente se pueden usar como parte de la lógica de representación del C# componente mediante expresiones que comienzan con `@`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-125">Component members can be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="9f81b-126">Por ejemplo, un C# campo se representa mediante el prefijo `@` al nombre del campo.</span><span class="sxs-lookup"><span data-stu-id="9f81b-126">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="9f81b-127">En el siguiente ejemplo se evalúa y representa:</span><span class="sxs-lookup"><span data-stu-id="9f81b-127">The following example evaluates and renders:</span></span>

* <span data-ttu-id="9f81b-128">`_headingFontStyle` al valor de la propiedad CSS para `font-style`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-128">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="9f81b-129">`_headingText` al contenido del elemento `<h1>`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-129">`_headingText` to the content of the `<h1>` element.</span></span>

```razor
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="9f81b-130">Una vez que el componente se representa inicialmente, el componente regenera su árbol de representación en respuesta a los eventos.</span><span class="sxs-lookup"><span data-stu-id="9f81b-130">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> <span data-ttu-id="9f81b-131">a continuación, Blazor compara el nuevo árbol de representación con el anterior y aplica cualquier modificación a la Document Object Model del explorador (DOM).</span><span class="sxs-lookup"><span data-stu-id="9f81b-131">Blazor then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

<span data-ttu-id="9f81b-132">Los componentes son C# clases normales y se pueden colocar en cualquier parte dentro de un proyecto.</span><span class="sxs-lookup"><span data-stu-id="9f81b-132">Components are ordinary C# classes and can be placed anywhere within a project.</span></span> <span data-ttu-id="9f81b-133">Los componentes que generan páginas web normalmente residen en la carpeta *pages* .</span><span class="sxs-lookup"><span data-stu-id="9f81b-133">Components that produce webpages usually reside in the *Pages* folder.</span></span> <span data-ttu-id="9f81b-134">Los componentes que no son de página se colocan con frecuencia en la carpeta *compartida* o en una carpeta personalizada agregada al proyecto.</span><span class="sxs-lookup"><span data-stu-id="9f81b-134">Non-page components are frequently placed in the *Shared* folder or a custom folder added to the project.</span></span> <span data-ttu-id="9f81b-135">Para usar una carpeta personalizada, agregue el espacio de nombres de la carpeta personalizada al componente primario o al archivo *_Imports. Razor* de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9f81b-135">To use a custom folder, add the custom folder's namespace to either the parent component or to the app's *_Imports.razor* file.</span></span> <span data-ttu-id="9f81b-136">Por ejemplo, el siguiente espacio de nombres hace que los componentes de una carpeta *Components* estén disponibles cuando se `WebApplication`el espacio de nombres raíz de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="9f81b-136">For example, the following namespace makes components in a *Components* folder available when the app's root namespace is `WebApplication`:</span></span>

```razor
@using WebApplication.Components
```

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="9f81b-137">Integración de componentes en aplicaciones de Razor Pages y MVC</span><span class="sxs-lookup"><span data-stu-id="9f81b-137">Integrate components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="9f81b-138">Usar componentes con las aplicaciones de Razor Pages y MVC existentes.</span><span class="sxs-lookup"><span data-stu-id="9f81b-138">Use components with existing Razor Pages and MVC apps.</span></span> <span data-ttu-id="9f81b-139">No es necesario volver a escribir las páginas o vistas existentes para utilizar los componentes de Razor.</span><span class="sxs-lookup"><span data-stu-id="9f81b-139">There's no need to rewrite existing pages or views to use Razor components.</span></span> <span data-ttu-id="9f81b-140">Cuando se representa la página o la vista, los componentes se preprocesan al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="9f81b-140">When the page or view is rendered, components are prerendered at the same time.</span></span>

::: moniker range=">= aspnetcore-3.1"

<span data-ttu-id="9f81b-141">Para representar un componente de una página o vista, use la aplicación auxiliar de etiquetas `Component`:</span><span class="sxs-lookup"><span data-stu-id="9f81b-141">To render a component from a page or view, use the `Component` Tag Helper:</span></span>

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-IncrementAmount="10" />
```

<span data-ttu-id="9f81b-142">Se admite el paso de parámetros (por ejemplo, `IncrementAmount` en el ejemplo anterior).</span><span class="sxs-lookup"><span data-stu-id="9f81b-142">Passing parameters (for example, `IncrementAmount` in the preceding example) is supported.</span></span>

<span data-ttu-id="9f81b-143">`RenderMode` configura si el componente:</span><span class="sxs-lookup"><span data-stu-id="9f81b-143">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="9f81b-144">Se representa en la página.</span><span class="sxs-lookup"><span data-stu-id="9f81b-144">Is prerendered into the page.</span></span>
* <span data-ttu-id="9f81b-145">Se representa como HTML estático en la página o si incluye la información necesaria para arrancar una aplicación Blazor desde el agente de usuario.</span><span class="sxs-lookup"><span data-stu-id="9f81b-145">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="9f81b-146">Descripción</span><span class="sxs-lookup"><span data-stu-id="9f81b-146">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="9f81b-147">Representa el componente en código HTML estático e incluye un marcador para una aplicación de Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="9f81b-147">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="9f81b-148">Cuando se inicia el agente de usuario, este marcador se usa para arrancar una aplicación Blazor.</span><span class="sxs-lookup"><span data-stu-id="9f81b-148">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="9f81b-149">Representa un marcador para una aplicación de Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="9f81b-149">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="9f81b-150">La salida del componente no está incluida.</span><span class="sxs-lookup"><span data-stu-id="9f81b-150">Output from the component isn't included.</span></span> <span data-ttu-id="9f81b-151">Cuando se inicia el agente de usuario, este marcador se usa para arrancar una aplicación Blazor.</span><span class="sxs-lookup"><span data-stu-id="9f81b-151">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="9f81b-152">Representa el componente en HTML estático.</span><span class="sxs-lookup"><span data-stu-id="9f81b-152">Renders the component into static HTML.</span></span> |

<span data-ttu-id="9f81b-153">Mientras que las páginas y las vistas pueden utilizar componentes, el opuesto no es cierto.</span><span class="sxs-lookup"><span data-stu-id="9f81b-153">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="9f81b-154">Los componentes no pueden usar escenarios específicos de la página y de la vista, como vistas y secciones parciales.</span><span class="sxs-lookup"><span data-stu-id="9f81b-154">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="9f81b-155">Para usar la lógica de la vista parcial en un componente, se debe factorizar la lógica de vista parcial en un componente.</span><span class="sxs-lookup"><span data-stu-id="9f81b-155">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="9f81b-156">No se admite la representación de componentes de servidor desde una página HTML estática.</span><span class="sxs-lookup"><span data-stu-id="9f81b-156">Rendering server components from a static HTML page isn't supported.</span></span>

<span data-ttu-id="9f81b-157">Para obtener más información sobre cómo se representan los componentes, el estado del componente y la aplicación auxiliar de etiquetas `Component`, vea <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="9f81b-157">For more information on how components are rendered, component state, and the `Component` Tag Helper, see <xref:blazor/hosting-models>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.1"

<span data-ttu-id="9f81b-158">Para representar un componente de una página o vista, use el método auxiliar HTML `RenderComponentAsync<TComponent>`:</span><span class="sxs-lookup"><span data-stu-id="9f81b-158">To render a component from a page or view, use the `RenderComponentAsync<TComponent>` HTML helper method:</span></span>

```cshtml
@(await Html.RenderComponentAsync<MyComponent>(RenderMode.ServerPrerendered))
```

<span data-ttu-id="9f81b-159">`RenderMode` configura si el componente:</span><span class="sxs-lookup"><span data-stu-id="9f81b-159">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="9f81b-160">Se representa en la página.</span><span class="sxs-lookup"><span data-stu-id="9f81b-160">Is prerendered into the page.</span></span>
* <span data-ttu-id="9f81b-161">Se representa como HTML estático en la página o si incluye la información necesaria para arrancar una aplicación Blazor desde el agente de usuario.</span><span class="sxs-lookup"><span data-stu-id="9f81b-161">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="9f81b-162">Descripción</span><span class="sxs-lookup"><span data-stu-id="9f81b-162">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="9f81b-163">Representa el componente en código HTML estático e incluye un marcador para una aplicación de Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="9f81b-163">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="9f81b-164">Cuando se inicia el agente de usuario, este marcador se usa para arrancar una aplicación Blazor.</span><span class="sxs-lookup"><span data-stu-id="9f81b-164">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> <span data-ttu-id="9f81b-165">No se admiten los parámetros.</span><span class="sxs-lookup"><span data-stu-id="9f81b-165">Parameters aren't supported.</span></span> |
| `Server`            | <span data-ttu-id="9f81b-166">Representa un marcador para una aplicación de Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="9f81b-166">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="9f81b-167">La salida del componente no está incluida.</span><span class="sxs-lookup"><span data-stu-id="9f81b-167">Output from the component isn't included.</span></span> <span data-ttu-id="9f81b-168">Cuando se inicia el agente de usuario, este marcador se usa para arrancar una aplicación Blazor.</span><span class="sxs-lookup"><span data-stu-id="9f81b-168">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> <span data-ttu-id="9f81b-169">No se admiten los parámetros.</span><span class="sxs-lookup"><span data-stu-id="9f81b-169">Parameters aren't supported.</span></span> |
| `Static`            | <span data-ttu-id="9f81b-170">Representa el componente en HTML estático.</span><span class="sxs-lookup"><span data-stu-id="9f81b-170">Renders the component into static HTML.</span></span> <span data-ttu-id="9f81b-171">Se admiten los parámetros.</span><span class="sxs-lookup"><span data-stu-id="9f81b-171">Parameters are supported.</span></span> |

<span data-ttu-id="9f81b-172">Mientras que las páginas y las vistas pueden utilizar componentes, el opuesto no es cierto.</span><span class="sxs-lookup"><span data-stu-id="9f81b-172">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="9f81b-173">Los componentes no pueden usar escenarios específicos de la página y de la vista, como vistas y secciones parciales.</span><span class="sxs-lookup"><span data-stu-id="9f81b-173">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="9f81b-174">Para usar la lógica de la vista parcial en un componente, se debe factorizar la lógica de vista parcial en un componente.</span><span class="sxs-lookup"><span data-stu-id="9f81b-174">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="9f81b-175">No se admite la representación de componentes de servidor desde una página HTML estática.</span><span class="sxs-lookup"><span data-stu-id="9f81b-175">Rendering server components from a static HTML page isn't supported.</span></span>

<span data-ttu-id="9f81b-176">Para obtener más información sobre cómo se representan los componentes, el estado del componente y la aplicación auxiliar HTML `RenderComponentAsync`, vea <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="9f81b-176">For more information on how components are rendered, component state, and the `RenderComponentAsync` HTML Helper, see <xref:blazor/hosting-models>.</span></span>

::: moniker-end

## <a name="use-components"></a><span data-ttu-id="9f81b-177">Uso de componentes</span><span class="sxs-lookup"><span data-stu-id="9f81b-177">Use components</span></span>

<span data-ttu-id="9f81b-178">Los componentes pueden incluir otros componentes declarándolos mediante la sintaxis de elementos HTML.</span><span class="sxs-lookup"><span data-stu-id="9f81b-178">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="9f81b-179">El marcado para utilizar un componente se parece a una etiqueta HTML en la que el nombre de la etiqueta es el tipo de componente.</span><span class="sxs-lookup"><span data-stu-id="9f81b-179">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="9f81b-180">El enlace de atributo distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="9f81b-180">Attribute binding is case sensitive.</span></span> <span data-ttu-id="9f81b-181">Por ejemplo, `@bind` es válido y `@Bind` no es válido.</span><span class="sxs-lookup"><span data-stu-id="9f81b-181">For example, `@bind` is valid, and `@Bind` is invalid.</span></span>

<span data-ttu-id="9f81b-182">El marcado siguiente en *index. Razor* representa una instancia de `HeadingComponent`:</span><span class="sxs-lookup"><span data-stu-id="9f81b-182">The following markup in *Index.razor* renders a `HeadingComponent` instance:</span></span>

```razor
<HeadingComponent />
```

<span data-ttu-id="9f81b-183">*Components/HeadingComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="9f81b-183">*Components/HeadingComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/HeadingComponent.razor)]

<span data-ttu-id="9f81b-184">Si un componente contiene un elemento HTML con una primera letra mayúscula que no coincide con un nombre de componente, se emite una advertencia que indica que el elemento tiene un nombre inesperado.</span><span class="sxs-lookup"><span data-stu-id="9f81b-184">If a component contains an HTML element with an uppercase first letter that doesn't match a component name, a warning is emitted indicating that the element has an unexpected name.</span></span> <span data-ttu-id="9f81b-185">La adición de una instrucción `@using` para el espacio de nombres del componente hace que el componente esté disponible, lo que elimina la advertencia.</span><span class="sxs-lookup"><span data-stu-id="9f81b-185">Adding an `@using` statement for the component's namespace makes the component available, which removes the warning.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="9f81b-186">Parámetros del componente</span><span class="sxs-lookup"><span data-stu-id="9f81b-186">Component parameters</span></span>

<span data-ttu-id="9f81b-187">Los componentes pueden tener *parámetros de componente*, que se definen mediante propiedades públicas en la clase de componente con el atributo `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-187">Components can have *component parameters*, which are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="9f81b-188">Use atributos para especificar argumentos para un componente en el marcado.</span><span class="sxs-lookup"><span data-stu-id="9f81b-188">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="9f81b-189">*Components/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="9f81b-189">*Components/ChildComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=11-12)]

<span data-ttu-id="9f81b-190">En el ejemplo siguiente de la aplicación de ejemplo, el `ParentComponent` establece el valor de la propiedad `Title` de la `ChildComponent`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-190">In the following example from the sample app, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`.</span></span>

<span data-ttu-id="9f81b-191">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="9f81b-191">*Pages/ParentComponent.razor*:</span></span>

```razor
@page "/ParentComponent"

<h1>Parent-child example</h1>

<ChildComponent Title="Panel Title from Parent"
                OnClick="@ShowMessage">
    Content of the child component is supplied
    by the parent component.
</ChildComponent>

...
```

## <a name="child-content"></a><span data-ttu-id="9f81b-192">Contenido secundario</span><span class="sxs-lookup"><span data-stu-id="9f81b-192">Child content</span></span>

<span data-ttu-id="9f81b-193">Los componentes pueden establecer el contenido de otro componente.</span><span class="sxs-lookup"><span data-stu-id="9f81b-193">Components can set the content of another component.</span></span> <span data-ttu-id="9f81b-194">El componente de asignación proporciona el contenido entre las etiquetas que especifican el componente receptor.</span><span class="sxs-lookup"><span data-stu-id="9f81b-194">The assigning component provides the content between the tags that specify the receiving component.</span></span>

<span data-ttu-id="9f81b-195">En el ejemplo siguiente, el `ChildComponent` tiene una propiedad `ChildContent` que representa un `RenderFragment`, que representa un segmento de interfaz de usuario que se va a representar.</span><span class="sxs-lookup"><span data-stu-id="9f81b-195">In the following example, the `ChildComponent` has a `ChildContent` property that represents a `RenderFragment`, which represents a segment of UI to render.</span></span> <span data-ttu-id="9f81b-196">El valor de `ChildContent` se coloca en el marcado del componente en el que se debe representar el contenido.</span><span class="sxs-lookup"><span data-stu-id="9f81b-196">The value of `ChildContent` is positioned in the component's markup where the content should be rendered.</span></span> <span data-ttu-id="9f81b-197">El valor de `ChildContent` se recibe del componente primario y se representa dentro del `panel-body`del panel de arranque.</span><span class="sxs-lookup"><span data-stu-id="9f81b-197">The value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="9f81b-198">*Components/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="9f81b-198">*Components/ChildComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> <span data-ttu-id="9f81b-199">La propiedad que recibe el contenido del `RenderFragment` debe denominarse `ChildContent` por Convención.</span><span class="sxs-lookup"><span data-stu-id="9f81b-199">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

<span data-ttu-id="9f81b-200">El `ParentComponent` de la aplicación de ejemplo puede proporcionar contenido para representar el `ChildComponent` colocando el contenido dentro de las etiquetas de `<ChildComponent>`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-200">The `ParentComponent` in the sample app can provide content for rendering the `ChildComponent` by placing the content inside the `<ChildComponent>` tags.</span></span>

<span data-ttu-id="9f81b-201">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="9f81b-201">*Pages/ParentComponent.razor*:</span></span>

```razor
@page "/ParentComponent"

<h1>Parent-child example</h1>

<ChildComponent Title="Panel Title from Parent"
                OnClick="@ShowMessage">
    Content of the child component is supplied
    by the parent component.
</ChildComponent>

...
```

## <a name="attribute-splatting-and-arbitrary-parameters"></a><span data-ttu-id="9f81b-202">Atributos expansión y parámetros arbitrarios</span><span class="sxs-lookup"><span data-stu-id="9f81b-202">Attribute splatting and arbitrary parameters</span></span>

<span data-ttu-id="9f81b-203">Los componentes de pueden capturar y representar atributos adicionales además de los parámetros declarados del componente.</span><span class="sxs-lookup"><span data-stu-id="9f81b-203">Components can capture and render additional attributes in addition to the component's declared parameters.</span></span> <span data-ttu-id="9f81b-204">Los atributos adicionales se pueden capturar en un diccionario y, después, *splatted* en un elemento cuando el componente se representa mediante la Directiva de Razor [`@attributes`](xref:mvc/views/razor#attributes) .</span><span class="sxs-lookup"><span data-stu-id="9f81b-204">Additional attributes can be captured in a dictionary and then *splatted* onto an element when the component is rendered using the [`@attributes`](xref:mvc/views/razor#attributes) Razor directive.</span></span> <span data-ttu-id="9f81b-205">Este escenario es útil cuando se define un componente que genera un elemento de marcado que admite diversas personalizaciones.</span><span class="sxs-lookup"><span data-stu-id="9f81b-205">This scenario is useful when defining a component that produces a markup element that supports a variety of customizations.</span></span> <span data-ttu-id="9f81b-206">Por ejemplo, puede resultar tedioso definir atributos por separado para una `<input>` que admita muchos parámetros.</span><span class="sxs-lookup"><span data-stu-id="9f81b-206">For example, it can be tedious to define attributes separately for an `<input>` that supports many parameters.</span></span>

<span data-ttu-id="9f81b-207">En el ejemplo siguiente, el primer elemento `<input>` (`id="useIndividualParams"`) utiliza parámetros de componente individuales, mientras que el segundo elemento `<input>` (`id="useAttributesDict"`) utiliza el atributo expansión:</span><span class="sxs-lookup"><span data-stu-id="9f81b-207">In the following example, the first `<input>` element (`id="useIndividualParams"`) uses individual component parameters, while the second `<input>` element (`id="useAttributesDict"`) uses attribute splatting:</span></span>

```razor
<input id="useIndividualParams"
       maxlength="@Maxlength"
       placeholder="@Placeholder"
       required="@Required"
       size="@Size" />

<input id="useAttributesDict"
       @attributes="InputAttributes" />

@code {
    [Parameter]
    public string Maxlength { get; set; } = "10";

    [Parameter]
    public string Placeholder { get; set; } = "Input placeholder text";

    [Parameter]
    public string Required { get; set; } = "required";

    [Parameter]
    public string Size { get; set; } = "50";

    [Parameter]
    public Dictionary<string, object> InputAttributes { get; set; } =
        new Dictionary<string, object>()
        {
            { "maxlength", "10" },
            { "placeholder", "Input placeholder text" },
            { "required", "required" },
            { "size", "50" }
        };
}
```

<span data-ttu-id="9f81b-208">El tipo del parámetro debe implementar `IEnumerable<KeyValuePair<string, object>>` con claves de cadena.</span><span class="sxs-lookup"><span data-stu-id="9f81b-208">The type of the parameter must implement `IEnumerable<KeyValuePair<string, object>>` with string keys.</span></span> <span data-ttu-id="9f81b-209">El uso de `IReadOnlyDictionary<string, object>` también es una opción en este escenario.</span><span class="sxs-lookup"><span data-stu-id="9f81b-209">Using `IReadOnlyDictionary<string, object>` is also an option in this scenario.</span></span>

<span data-ttu-id="9f81b-210">Los elementos `<input>` representados con ambos enfoques son idénticos:</span><span class="sxs-lookup"><span data-stu-id="9f81b-210">The rendered `<input>` elements using both approaches is identical:</span></span>

```html
<input id="useIndividualParams"
       maxlength="10"
       placeholder="Input placeholder text"
       required="required"
       size="50">

<input id="useAttributesDict"
       maxlength="10"
       placeholder="Input placeholder text"
       required="required"
       size="50">
```

<span data-ttu-id="9f81b-211">Para aceptar atributos arbitrarios, defina un parámetro de componente mediante el atributo `[Parameter]` con la propiedad `CaptureUnmatchedValues` establecida en `true`:</span><span class="sxs-lookup"><span data-stu-id="9f81b-211">To accept arbitrary attributes, define a component parameter using the `[Parameter]` attribute with the `CaptureUnmatchedValues` property set to `true`:</span></span>

```razor
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

<span data-ttu-id="9f81b-212">La propiedad `CaptureUnmatchedValues` en `[Parameter]` permite que el parámetro coincida con todos los atributos que no coinciden con ningún otro parámetro.</span><span class="sxs-lookup"><span data-stu-id="9f81b-212">The `CaptureUnmatchedValues` property on `[Parameter]` allows the parameter to match all attributes that don't match any other parameter.</span></span> <span data-ttu-id="9f81b-213">Un componente solo puede definir un parámetro con `CaptureUnmatchedValues`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-213">A component can only define a single parameter with `CaptureUnmatchedValues`.</span></span> <span data-ttu-id="9f81b-214">El tipo de propiedad que se usa con `CaptureUnmatchedValues` se debe poder asignar desde `Dictionary<string, object>` con claves de cadena.</span><span class="sxs-lookup"><span data-stu-id="9f81b-214">The property type used with `CaptureUnmatchedValues` must be assignable from `Dictionary<string, object>` with string keys.</span></span> <span data-ttu-id="9f81b-215">`IEnumerable<KeyValuePair<string, object>>` o `IReadOnlyDictionary<string, object>` también son opciones en este escenario.</span><span class="sxs-lookup"><span data-stu-id="9f81b-215">`IEnumerable<KeyValuePair<string, object>>` or `IReadOnlyDictionary<string, object>` are also options in this scenario.</span></span>

<span data-ttu-id="9f81b-216">La posición de `@attributes` relativa a la posición de los atributos de elemento es importante.</span><span class="sxs-lookup"><span data-stu-id="9f81b-216">The position of `@attributes` relative to the position of element attributes is important.</span></span> <span data-ttu-id="9f81b-217">Cuando `@attributes` se splatted en el elemento, los atributos se procesan de derecha a izquierda (último a primero).</span><span class="sxs-lookup"><span data-stu-id="9f81b-217">When `@attributes` are splatted on the element, the attributes are processed from right to left (last to first).</span></span> <span data-ttu-id="9f81b-218">Considere el siguiente ejemplo de un componente que utiliza un componente de `Child`:</span><span class="sxs-lookup"><span data-stu-id="9f81b-218">Consider the following example of a component that consumes a `Child` component:</span></span>

<span data-ttu-id="9f81b-219">*ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="9f81b-219">*ParentComponent.razor*:</span></span>

```razor
<ChildComponent extra="10" />
```

<span data-ttu-id="9f81b-220">*ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="9f81b-220">*ChildComponent.razor*:</span></span>

```razor
<div @attributes="AdditionalAttributes" extra="5" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="9f81b-221">El atributo `extra` del componente de `Child` se establece a la derecha de `@attributes`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-221">The `Child` component's `extra` attribute is set to the right of `@attributes`.</span></span> <span data-ttu-id="9f81b-222">El `<div>` representado del componente `Parent` contiene `extra="5"` cuando se pasa a través del atributo adicional, ya que los atributos se procesan de derecha a izquierda (último a primero):</span><span class="sxs-lookup"><span data-stu-id="9f81b-222">The `Parent` component's rendered `<div>` contains `extra="5"` when passed through the additional attribute because the attributes are processed right to left (last to first):</span></span>

```html
<div extra="5" />
```

<span data-ttu-id="9f81b-223">En el ejemplo siguiente, el orden de `extra` y `@attributes` se invierte en el `<div>`del componente `Child`:</span><span class="sxs-lookup"><span data-stu-id="9f81b-223">In the following example, the order of `extra` and `@attributes` is reversed in the `Child` component's `<div>`:</span></span>

<span data-ttu-id="9f81b-224">*ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="9f81b-224">*ParentComponent.razor*:</span></span>

```razor
<ChildComponent extra="10" />
```

<span data-ttu-id="9f81b-225">*ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="9f81b-225">*ChildComponent.razor*:</span></span>

```razor
<div extra="5" @attributes="AdditionalAttributes" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="9f81b-226">El `<div>` representado en el componente `Parent` contiene `extra="10"` cuando se pasa a través del atributo adicional:</span><span class="sxs-lookup"><span data-stu-id="9f81b-226">The rendered `<div>` in the `Parent` component contains `extra="10"` when passed through the additional attribute:</span></span>

```html
<div extra="10" />
```

## <a name="data-binding"></a><span data-ttu-id="9f81b-227">Enlace de datos</span><span class="sxs-lookup"><span data-stu-id="9f81b-227">Data binding</span></span>

<span data-ttu-id="9f81b-228">El enlace de datos tanto a componentes como a elementos DOM se realiza con el [`@bind`](xref:mvc/views/razor#bind) atributo.</span><span class="sxs-lookup"><span data-stu-id="9f81b-228">Data binding to both components and DOM elements is accomplished with the [`@bind`](xref:mvc/views/razor#bind) attribute.</span></span> <span data-ttu-id="9f81b-229">En el ejemplo siguiente se enlaza una propiedad `CurrentValue` al valor del cuadro de texto:</span><span class="sxs-lookup"><span data-stu-id="9f81b-229">The following example binds a `CurrentValue` property to the text box's value:</span></span>

```razor
<input @bind="CurrentValue" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="9f81b-230">Cuando el cuadro de texto pierde el foco, se actualiza el valor de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="9f81b-230">When the text box loses focus, the property's value is updated.</span></span>

<span data-ttu-id="9f81b-231">El cuadro de texto se actualiza en la interfaz de usuario solo cuando se representa el componente, no en respuesta a cambiar el valor de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="9f81b-231">The text box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="9f81b-232">Puesto que los componentes se representan por sí solos después de que se ejecute el código del controlador de eventos, las actualizaciones de propiedades *normalmente* se reflejan en la interfaz de usuario inmediatamente después de desencadenarse un controlador de eventos.</span><span class="sxs-lookup"><span data-stu-id="9f81b-232">Since components render themselves after event handler code executes, property updates are *usually* reflected in the UI immediately after an event handler is triggered.</span></span>

<span data-ttu-id="9f81b-233">El uso de `@bind` con la propiedad `CurrentValue` (`<input @bind="CurrentValue" />`) es esencialmente equivalente a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="9f81b-233">Using `@bind` with the `CurrentValue` property (`<input @bind="CurrentValue" />`) is essentially equivalent to the following:</span></span>

```razor
<input value="@CurrentValue"
    @onchange="@((ChangeEventArgs __e) => CurrentValue = 
        __e.Value.ToString())" />
        
@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="9f81b-234">Cuando se representa el componente, el `value` del elemento de entrada procede de la propiedad `CurrentValue`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-234">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="9f81b-235">Cuando el usuario escribe en el cuadro de texto y cambia el foco del elemento, se desencadena el evento `onchange` y la propiedad `CurrentValue` se establece en el valor modificado.</span><span class="sxs-lookup"><span data-stu-id="9f81b-235">When the user types in the text box and changes element focus, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="9f81b-236">En realidad, la generación de código es más compleja porque `@bind` administra los casos en los que se realizan las conversiones de tipos.</span><span class="sxs-lookup"><span data-stu-id="9f81b-236">In reality, the code generation is more complex because `@bind` handles cases where type conversions are performed.</span></span> <span data-ttu-id="9f81b-237">En principio, `@bind` asocia el valor actual de una expresión con un atributo `value` y controla los cambios mediante el controlador registrado.</span><span class="sxs-lookup"><span data-stu-id="9f81b-237">In principle, `@bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="9f81b-238">Además de controlar los eventos de `onchange` con sintaxis de `@bind`, se puede enlazar una propiedad o un campo mediante otros eventos mediante la especificación de un atributo de [`@bind-value`](xref:mvc/views/razor#bind) con un parámetro de `event` ([`@bind-value:event`](xref:mvc/views/razor#bind)).</span><span class="sxs-lookup"><span data-stu-id="9f81b-238">In addition to handling `onchange` events with `@bind` syntax, a property or field can be bound using other events by specifying an [`@bind-value`](xref:mvc/views/razor#bind) attribute with an `event` parameter ([`@bind-value:event`](xref:mvc/views/razor#bind)).</span></span> <span data-ttu-id="9f81b-239">En el siguiente ejemplo se enlaza la propiedad `CurrentValue` del evento `oninput`:</span><span class="sxs-lookup"><span data-stu-id="9f81b-239">The following example binds the `CurrentValue` property for the `oninput` event:</span></span>

```razor
<input @bind-value="CurrentValue" @bind-value:event="oninput" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="9f81b-240">A diferencia de `onchange`, que se activa cuando el elemento pierde el foco, `oninput` se desencadena cuando cambia el valor del cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="9f81b-240">Unlike `onchange`, which fires when the element loses focus, `oninput` fires when the value of the text box changes.</span></span>

<span data-ttu-id="9f81b-241">**Valores no analizables**</span><span class="sxs-lookup"><span data-stu-id="9f81b-241">**Unparsable values**</span></span>

<span data-ttu-id="9f81b-242">Cuando un usuario proporciona un valor que no se pueda analizar a un elemento DataBound, el valor no analizable se revierte automáticamente a su valor anterior cuando se desencadena el evento de enlace.</span><span class="sxs-lookup"><span data-stu-id="9f81b-242">When a user provides an unparsable value to a databound element, the unparsable value is automatically reverted to its previous value when the bind event is triggered.</span></span>

<span data-ttu-id="9f81b-243">Considere el siguiente escenario:</span><span class="sxs-lookup"><span data-stu-id="9f81b-243">Consider the following scenario:</span></span>

* <span data-ttu-id="9f81b-244">Un elemento `<input>` se enlaza a un tipo `int` con un valor inicial de `123`:</span><span class="sxs-lookup"><span data-stu-id="9f81b-244">An `<input>` element is bound to an `int` type with an initial value of `123`:</span></span>

  ```razor
  <input @bind="MyProperty" />

  @code {
      [Parameter]
      public int MyProperty { get; set; } = 123;
  }
  ```
* <span data-ttu-id="9f81b-245">El usuario actualiza el valor del elemento a `123.45` en la página y cambia el foco del elemento.</span><span class="sxs-lookup"><span data-stu-id="9f81b-245">The user updates the value of the element to `123.45` in the page and changes the element focus.</span></span>

<span data-ttu-id="9f81b-246">En el escenario anterior, el valor del elemento se revierte a `123`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-246">In the preceding scenario, the element's value is reverted to `123`.</span></span> <span data-ttu-id="9f81b-247">Cuando el valor `123.45` se rechaza en favor del valor original de `123`, el usuario entiende que no se aceptó su valor.</span><span class="sxs-lookup"><span data-stu-id="9f81b-247">When the value `123.45` is rejected in favor of the original value of `123`, the user understands that their value wasn't accepted.</span></span>

<span data-ttu-id="9f81b-248">De forma predeterminada, el enlace se aplica al evento `onchange` del elemento (`@bind="{PROPERTY OR FIELD}"`).</span><span class="sxs-lookup"><span data-stu-id="9f81b-248">By default, binding applies to the element's `onchange` event (`@bind="{PROPERTY OR FIELD}"`).</span></span> <span data-ttu-id="9f81b-249">Use `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` para establecer un evento diferente.</span><span class="sxs-lookup"><span data-stu-id="9f81b-249">Use `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` to set a different event.</span></span> <span data-ttu-id="9f81b-250">En el caso del evento `oninput` (`@bind-value:event="oninput"`), la reversión se produce después de cualquier pulsación de tecla que introduzca un valor no analizable.</span><span class="sxs-lookup"><span data-stu-id="9f81b-250">For the `oninput` event (`@bind-value:event="oninput"`), the reversion occurs after any keystroke that introduces an unparsable value.</span></span> <span data-ttu-id="9f81b-251">Cuando el destino es el evento `oninput` con un tipo enlazado a `int`, se impide que un usuario escriba un carácter `.`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-251">When targeting the `oninput` event with an `int`-bound type, a user is prevented from typing a `.` character.</span></span> <span data-ttu-id="9f81b-252">Se quita inmediatamente un carácter `.`, por lo que el usuario recibe comentarios inmediatos que solo se permiten números enteros.</span><span class="sxs-lookup"><span data-stu-id="9f81b-252">A `.` character is immediately removed, so the user receives immediate feedback that only whole numbers are permitted.</span></span> <span data-ttu-id="9f81b-253">Hay escenarios en los que la reversión del valor del evento `oninput` no es ideal, por ejemplo, cuando se debe permitir que el usuario borre un valor `<input>` que no se puede analizar.</span><span class="sxs-lookup"><span data-stu-id="9f81b-253">There are scenarios where reverting the value on the `oninput` event isn't ideal, such as when the user should be allowed to clear an unparsable `<input>` value.</span></span> <span data-ttu-id="9f81b-254">Las alternativas incluyen:</span><span class="sxs-lookup"><span data-stu-id="9f81b-254">Alternatives include:</span></span>

* <span data-ttu-id="9f81b-255">No utilice el evento `oninput`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-255">Don't use the `oninput` event.</span></span> <span data-ttu-id="9f81b-256">Use el evento `onchange` predeterminado (`@bind="{PROPERTY OR FIELD}"`), donde no se revierte un valor no válido hasta que el elemento pierde el foco.</span><span class="sxs-lookup"><span data-stu-id="9f81b-256">Use the default `onchange` event (`@bind="{PROPERTY OR FIELD}"`), where an invalid value isn't reverted until the element loses focus.</span></span>
* <span data-ttu-id="9f81b-257">Enlazar a un tipo que acepta valores NULL, como `int?` o `string`, y proporcionar una lógica personalizada para controlar las entradas no válidas.</span><span class="sxs-lookup"><span data-stu-id="9f81b-257">Bind to a nullable type, such as `int?` or `string`, and provide custom logic to handle invalid entries.</span></span>
* <span data-ttu-id="9f81b-258">Use un [componente de validación de formulario](xref:blazor/forms-validation), como `InputNumber` o `InputDate`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-258">Use a [form validation component](xref:blazor/forms-validation), such as `InputNumber` or `InputDate`.</span></span> <span data-ttu-id="9f81b-259">Los componentes de validación de formularios tienen compatibilidad integrada para administrar entradas no válidas.</span><span class="sxs-lookup"><span data-stu-id="9f81b-259">Form validation components have built-in support to manage invalid inputs.</span></span> <span data-ttu-id="9f81b-260">Componentes de validación de formularios:</span><span class="sxs-lookup"><span data-stu-id="9f81b-260">Form validation components:</span></span>
  * <span data-ttu-id="9f81b-261">Permite que el usuario proporcione entradas no válidas y reciba errores de validación en la `EditContext`asociada.</span><span class="sxs-lookup"><span data-stu-id="9f81b-261">Permit the user to provide invalid input and receive validation errors on the associated `EditContext`.</span></span>
  * <span data-ttu-id="9f81b-262">Mostrar errores de validación en la interfaz de usuario sin interferir con el usuario al escribir datos de WebForm adicionales.</span><span class="sxs-lookup"><span data-stu-id="9f81b-262">Display validation errors in the UI without interfering with the user entering additional webform data.</span></span>

<span data-ttu-id="9f81b-263">**Globalización**</span><span class="sxs-lookup"><span data-stu-id="9f81b-263">**Globalization**</span></span>

<span data-ttu-id="9f81b-264">`@bind` se da formato a los valores para mostrarlos y analizarlos con las reglas de la referencia cultural actual.</span><span class="sxs-lookup"><span data-stu-id="9f81b-264">`@bind` values are formatted for display and parsed using the current culture's rules.</span></span>

<span data-ttu-id="9f81b-265">Se puede tener acceso a la referencia cultural actual desde la propiedad <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="9f81b-265">The current culture can be accessed from the <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> property.</span></span>

<span data-ttu-id="9f81b-266">[CultureInfo. InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) se usa para los siguientes tipos de campo (`<input type="{TYPE}" />`):</span><span class="sxs-lookup"><span data-stu-id="9f81b-266">[CultureInfo.InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) is used for the following field types (`<input type="{TYPE}" />`):</span></span>

* `date`
* `number`

<span data-ttu-id="9f81b-267">Los tipos de campo anteriores:</span><span class="sxs-lookup"><span data-stu-id="9f81b-267">The preceding field types:</span></span>

* <span data-ttu-id="9f81b-268">Se muestran con sus reglas de formato basadas en explorador adecuadas.</span><span class="sxs-lookup"><span data-stu-id="9f81b-268">Are displayed using their appropriate browser-based formatting rules.</span></span>
* <span data-ttu-id="9f81b-269">No puede contener texto de forma libre.</span><span class="sxs-lookup"><span data-stu-id="9f81b-269">Can't contain free-form text.</span></span>
* <span data-ttu-id="9f81b-270">Proporcionar características de interacción con el usuario en función de la implementación del explorador.</span><span class="sxs-lookup"><span data-stu-id="9f81b-270">Provide user interaction characteristics based on the browser's implementation.</span></span>

<span data-ttu-id="9f81b-271">Los siguientes tipos de campo tienen requisitos de formato específicos y no se admiten actualmente en Blazor porque no son compatibles con todos los exploradores principales:</span><span class="sxs-lookup"><span data-stu-id="9f81b-271">The following field types have specific formatting requirements and aren't currently supported by Blazor because they aren't supported by all major browsers:</span></span>

* `datetime-local`
* `month`
* `week`

<span data-ttu-id="9f81b-272">`@bind` admite el parámetro `@bind:culture` para proporcionar un <xref:System.Globalization.CultureInfo?displayProperty=fullName> para analizar y dar formato a un valor.</span><span class="sxs-lookup"><span data-stu-id="9f81b-272">`@bind` supports the `@bind:culture` parameter to provide a <xref:System.Globalization.CultureInfo?displayProperty=fullName> for parsing and formatting a value.</span></span> <span data-ttu-id="9f81b-273">No se recomienda especificar una referencia cultural al usar los tipos de campo `date` y `number`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-273">Specifying a culture isn't recommended when using the `date` and `number` field types.</span></span> <span data-ttu-id="9f81b-274">`date` y `number` tienen compatibilidad integrada con Blazor que proporciona la referencia cultural necesaria.</span><span class="sxs-lookup"><span data-stu-id="9f81b-274">`date` and `number` have built-in Blazor support that provides the required culture.</span></span>

<span data-ttu-id="9f81b-275">Para obtener información sobre cómo establecer la referencia cultural del usuario, consulte la sección [Localización](#localization).</span><span class="sxs-lookup"><span data-stu-id="9f81b-275">For information on how to set the user's culture, see the [Localization](#localization) section.</span></span>

<span data-ttu-id="9f81b-276">**Cadenas de formato**</span><span class="sxs-lookup"><span data-stu-id="9f81b-276">**Format strings**</span></span>

<span data-ttu-id="9f81b-277">El enlace de datos funciona con cadenas de formato <xref:System.DateTime> mediante [`@bind:format`](xref:mvc/views/razor#bind).</span><span class="sxs-lookup"><span data-stu-id="9f81b-277">Data binding works with <xref:System.DateTime> format strings using [`@bind:format`](xref:mvc/views/razor#bind).</span></span> <span data-ttu-id="9f81b-278">Otras expresiones de formato, como los formatos de moneda o número, no están disponibles en este momento.</span><span class="sxs-lookup"><span data-stu-id="9f81b-278">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```razor
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="9f81b-279">En el código anterior, el tipo de campo del elemento de `<input>` (`type`) tiene como valor predeterminado `text`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-279">In the preceding code, the `<input>` element's field type (`type`) defaults to `text`.</span></span> <span data-ttu-id="9f81b-280">`@bind:format` es compatible con el enlace de los siguientes tipos de .NET:</span><span class="sxs-lookup"><span data-stu-id="9f81b-280">`@bind:format` is supported for binding the following .NET types:</span></span>

* <xref:System.DateTime?displayProperty=fullName>
* <span data-ttu-id="9f81b-281"><xref:System.DateTime?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="9f81b-281"><xref:System.DateTime?displayProperty=fullName>?</span></span>
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <span data-ttu-id="9f81b-282"><xref:System.DateTimeOffset?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="9f81b-282"><xref:System.DateTimeOffset?displayProperty=fullName>?</span></span>

<span data-ttu-id="9f81b-283">El atributo `@bind:format` especifica el formato de fecha que se va a aplicar al `value` del elemento `<input>`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-283">The `@bind:format` attribute specifies the date format to apply to the `value` of the `<input>` element.</span></span> <span data-ttu-id="9f81b-284">El formato también se usa para analizar el valor cuando se produce un evento `onchange`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-284">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="9f81b-285">No se recomienda especificar un formato para el tipo de campo `date` porque Blazor tiene compatibilidad integrada para dar formato a las fechas.</span><span class="sxs-lookup"><span data-stu-id="9f81b-285">Specifying a format for the `date` field type isn't recommended because Blazor has built-in support to format dates.</span></span> <span data-ttu-id="9f81b-286">A pesar de la recomendación, use solo el formato de fecha `yyyy-MM-dd` para que el enlace funcione correctamente si se proporciona un formato con el tipo de campo `date`:</span><span class="sxs-lookup"><span data-stu-id="9f81b-286">In spite of the recommendation, only use the `yyyy-MM-dd` date format for binding to work correctly if a format is supplied with the `date` field type:</span></span>

```razor
<input type="date" @bind="StartDate" @bind:format="yyyy-MM-dd">
```

<span data-ttu-id="9f81b-287">**Parámetros de componente**</span><span class="sxs-lookup"><span data-stu-id="9f81b-287">**Component parameters**</span></span>

<span data-ttu-id="9f81b-288">El enlace reconoce los parámetros de componente, donde `@bind-{property}` puede enlazar un valor de propiedad entre los componentes.</span><span class="sxs-lookup"><span data-stu-id="9f81b-288">Binding recognizes component parameters, where `@bind-{property}` can bind a property value across components.</span></span>

<span data-ttu-id="9f81b-289">El siguiente componente secundario (`ChildComponent`) tiene un parámetro de componente `Year` y una devolución de llamada `YearChanged`:</span><span class="sxs-lookup"><span data-stu-id="9f81b-289">The following child component (`ChildComponent`) has a `Year` component parameter and `YearChanged` callback:</span></span>

```razor
<h2>Child Component</h2>

<p>Year: @Year</p>

@code {
    [Parameter]
    public int Year { get; set; }

    [Parameter]
    public EventCallback<int> YearChanged { get; set; }
}
```

<span data-ttu-id="9f81b-290">`EventCallback<T>` se explica en la sección [EventCallback](#eventcallback) .</span><span class="sxs-lookup"><span data-stu-id="9f81b-290">`EventCallback<T>` is explained in the [EventCallback](#eventcallback) section.</span></span>

<span data-ttu-id="9f81b-291">El siguiente componente primario utiliza `ChildComponent` y enlaza el parámetro `ParentYear` desde el elemento primario al parámetro `Year` en el componente secundario:</span><span class="sxs-lookup"><span data-stu-id="9f81b-291">The following parent component uses `ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component:</span></span>

```razor
@page "/ParentComponent"

<h1>Parent Component</h1>

<p>ParentYear: @ParentYear</p>

<ChildComponent @bind-Year="ParentYear" />

<button class="btn btn-primary" @onclick="ChangeTheYear">
    Change Year to 1986
</button>

@code {
    [Parameter]
    public int ParentYear { get; set; } = 1978;

    private void ChangeTheYear()
    {
        ParentYear = 1986;
    }
}
```

<span data-ttu-id="9f81b-292">Al cargar el `ParentComponent` se produce el marcado siguiente:</span><span class="sxs-lookup"><span data-stu-id="9f81b-292">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="9f81b-293">Si el valor de la propiedad `ParentYear` se cambia seleccionando el botón en el `ParentComponent`, se actualiza la propiedad `Year` del `ChildComponent`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-293">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="9f81b-294">El nuevo valor de `Year` se representa en la interfaz de usuario cuando se representa el `ParentComponent`:</span><span class="sxs-lookup"><span data-stu-id="9f81b-294">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="9f81b-295">El parámetro `Year` es enlazable porque tiene un evento Companion `YearChanged` que coincide con el tipo del parámetro `Year`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-295">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="9f81b-296">Por Convención, `<ChildComponent @bind-Year="ParentYear" />` es esencialmente equivalente a escribir:</span><span class="sxs-lookup"><span data-stu-id="9f81b-296">By convention, `<ChildComponent @bind-Year="ParentYear" />` is essentially equivalent to writing:</span></span>

```razor
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

<span data-ttu-id="9f81b-297">En general, una propiedad se puede enlazar a un controlador de eventos correspondiente mediante `@bind-property:event` atributo.</span><span class="sxs-lookup"><span data-stu-id="9f81b-297">In general, a property can be bound to a corresponding event handler using `@bind-property:event` attribute.</span></span> <span data-ttu-id="9f81b-298">Por ejemplo, la propiedad `MyProp` se puede enlazar a `MyEventHandler` mediante los dos atributos siguientes:</span><span class="sxs-lookup"><span data-stu-id="9f81b-298">For example, the property `MyProp` can be bound to `MyEventHandler` using the following two attributes:</span></span>

```razor
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

## <a name="event-handling"></a><span data-ttu-id="9f81b-299">Control de eventos</span><span class="sxs-lookup"><span data-stu-id="9f81b-299">Event handling</span></span>

<span data-ttu-id="9f81b-300">Los componentes de Razor proporcionan características de control de eventos.</span><span class="sxs-lookup"><span data-stu-id="9f81b-300">Razor components provide event handling features.</span></span> <span data-ttu-id="9f81b-301">Para un atributo de elemento HTML denominado `on{EVENT}` (por ejemplo, `onclick` y `onsubmit`) con un valor de tipo delegado, los componentes de Razor tratan el valor del atributo como un controlador de eventos.</span><span class="sxs-lookup"><span data-stu-id="9f81b-301">For an HTML element attribute named `on{EVENT}` (for example, `onclick` and `onsubmit`) with a delegate-typed value, Razor components treats the attribute's value as an event handler.</span></span> <span data-ttu-id="9f81b-302">Siempre se da formato al nombre del atributo [`@on{EVENT}`](xref:mvc/views/razor#onevent).</span><span class="sxs-lookup"><span data-stu-id="9f81b-302">The attribute's name is always formatted [`@on{EVENT}`](xref:mvc/views/razor#onevent).</span></span>

<span data-ttu-id="9f81b-303">El código siguiente llama al método `UpdateHeading` cuando el botón está seleccionado en la interfaz de usuario:</span><span class="sxs-lookup"><span data-stu-id="9f81b-303">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

```razor
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private void UpdateHeading(MouseEventArgs e)
    {
        ...
    }
}
```

<span data-ttu-id="9f81b-304">El código siguiente llama al método `CheckChanged` cuando se cambia la casilla en la interfaz de usuario:</span><span class="sxs-lookup"><span data-stu-id="9f81b-304">The following code calls the `CheckChanged` method when the check box is changed in the UI:</span></span>

```razor
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

<span data-ttu-id="9f81b-305">Los controladores de eventos también pueden ser asincrónicos y devolver un <xref:System.Threading.Tasks.Task>.</span><span class="sxs-lookup"><span data-stu-id="9f81b-305">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="9f81b-306">No es necesario llamar a [StateHasChanged](xref:blazor/lifecycle#state-changes)manualmente.</span><span class="sxs-lookup"><span data-stu-id="9f81b-306">There's no need to manually call [StateHasChanged](xref:blazor/lifecycle#state-changes).</span></span> <span data-ttu-id="9f81b-307">Las excepciones se registran cuando se producen.</span><span class="sxs-lookup"><span data-stu-id="9f81b-307">Exceptions are logged when they occur.</span></span>

<span data-ttu-id="9f81b-308">En el ejemplo siguiente, se llama a `UpdateHeading` de forma asincrónica cuando se selecciona el botón:</span><span class="sxs-lookup"><span data-stu-id="9f81b-308">In the following example, `UpdateHeading` is called asynchronously when the button is selected:</span></span>

```razor
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private async Task UpdateHeading(MouseEventArgs e)
    {
        ...
    }
}
```

### <a name="event-argument-types"></a><span data-ttu-id="9f81b-309">Tipos de argumentos de evento</span><span class="sxs-lookup"><span data-stu-id="9f81b-309">Event argument types</span></span>

<span data-ttu-id="9f81b-310">En algunos eventos, se permiten los tipos de argumento de evento.</span><span class="sxs-lookup"><span data-stu-id="9f81b-310">For some events, event argument types are permitted.</span></span> <span data-ttu-id="9f81b-311">Si no es necesario el acceso a uno de estos tipos de evento, no es necesario en la llamada al método.</span><span class="sxs-lookup"><span data-stu-id="9f81b-311">If access to one of these event types isn't necessary, it isn't required in the method call.</span></span>

<span data-ttu-id="9f81b-312">Los `EventArgs` admitidos se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="9f81b-312">Supported `EventArgs` are shown in the following table.</span></span>

| <span data-ttu-id="9f81b-313">Event</span><span class="sxs-lookup"><span data-stu-id="9f81b-313">Event</span></span>            | <span data-ttu-id="9f81b-314">Clase</span><span class="sxs-lookup"><span data-stu-id="9f81b-314">Class</span></span>                | <span data-ttu-id="9f81b-315">Eventos y notas de DOM</span><span class="sxs-lookup"><span data-stu-id="9f81b-315">DOM events and notes</span></span> |
| ---------------- | -------------------- | -------------------- |
| <span data-ttu-id="9f81b-316">Portapapeles</span><span class="sxs-lookup"><span data-stu-id="9f81b-316">Clipboard</span></span>        | `ClipboardEventArgs` | <span data-ttu-id="9f81b-317">`oncut`, `oncopy`, `onpaste`</span><span class="sxs-lookup"><span data-stu-id="9f81b-317">`oncut`, `oncopy`, `onpaste`</span></span> |
| <span data-ttu-id="9f81b-318">Arrastrar</span><span class="sxs-lookup"><span data-stu-id="9f81b-318">Drag</span></span>             | `DragEventArgs`      | <span data-ttu-id="9f81b-319">`ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`, `ondragend`</span><span class="sxs-lookup"><span data-stu-id="9f81b-319">`ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`, `ondragend`</span></span><br><br><span data-ttu-id="9f81b-320">`DataTransfer` y `DataTransferItem` mantener los datos de los elementos arrastrados.</span><span class="sxs-lookup"><span data-stu-id="9f81b-320">`DataTransfer` and `DataTransferItem` hold dragged item data.</span></span> |
| <span data-ttu-id="9f81b-321">Error de :</span><span class="sxs-lookup"><span data-stu-id="9f81b-321">Error</span></span>            | `ErrorEventArgs`     | `onerror` |
| <span data-ttu-id="9f81b-322">Event</span><span class="sxs-lookup"><span data-stu-id="9f81b-322">Event</span></span>            | `EventArgs`          | <span data-ttu-id="9f81b-323">*General*</span><span class="sxs-lookup"><span data-stu-id="9f81b-323">*General*</span></span><br><span data-ttu-id="9f81b-324">`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onended`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`</span><span class="sxs-lookup"><span data-stu-id="9f81b-324">`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onended`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`</span></span><br><br><span data-ttu-id="9f81b-325">*Portapapeles*</span><span class="sxs-lookup"><span data-stu-id="9f81b-325">*Clipboard*</span></span><br><span data-ttu-id="9f81b-326">`onbeforecut`, `onbeforecopy`, `onbeforepaste`</span><span class="sxs-lookup"><span data-stu-id="9f81b-326">`onbeforecut`, `onbeforecopy`, `onbeforepaste`</span></span><br><br><span data-ttu-id="9f81b-327">*Entrada*</span><span class="sxs-lookup"><span data-stu-id="9f81b-327">*Input*</span></span><br><span data-ttu-id="9f81b-328">`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart`, `onsubmit`</span><span class="sxs-lookup"><span data-stu-id="9f81b-328">`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart`, `onsubmit`</span></span><br><br><span data-ttu-id="9f81b-329">*Medios*</span><span class="sxs-lookup"><span data-stu-id="9f81b-329">*Media*</span></span><br><span data-ttu-id="9f81b-330">`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting`</span><span class="sxs-lookup"><span data-stu-id="9f81b-330">`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting`</span></span> |
| <span data-ttu-id="9f81b-331">Foco</span><span class="sxs-lookup"><span data-stu-id="9f81b-331">Focus</span></span>            | `FocusEventArgs`     | <span data-ttu-id="9f81b-332">`onfocus`, `onblur`, `onfocusin`, `onfocusout`</span><span class="sxs-lookup"><span data-stu-id="9f81b-332">`onfocus`, `onblur`, `onfocusin`, `onfocusout`</span></span><br><br><span data-ttu-id="9f81b-333">No incluye compatibilidad con `relatedTarget`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-333">Doesn't include support for `relatedTarget`.</span></span> |
| <span data-ttu-id="9f81b-334">Input</span><span class="sxs-lookup"><span data-stu-id="9f81b-334">Input</span></span>            | `ChangeEventArgs`    | <span data-ttu-id="9f81b-335">`onchange`, `oninput`</span><span class="sxs-lookup"><span data-stu-id="9f81b-335">`onchange`, `oninput`</span></span> |
| <span data-ttu-id="9f81b-336">Teclado</span><span class="sxs-lookup"><span data-stu-id="9f81b-336">Keyboard</span></span>         | `KeyboardEventArgs`  | <span data-ttu-id="9f81b-337">`onkeydown`, `onkeypress`, `onkeyup`</span><span class="sxs-lookup"><span data-stu-id="9f81b-337">`onkeydown`, `onkeypress`, `onkeyup`</span></span> |
| <span data-ttu-id="9f81b-338">Mouse</span><span class="sxs-lookup"><span data-stu-id="9f81b-338">Mouse</span></span>            | `MouseEventArgs`     | <span data-ttu-id="9f81b-339">`onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout`</span><span class="sxs-lookup"><span data-stu-id="9f81b-339">`onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout`</span></span> |
| <span data-ttu-id="9f81b-340">Puntero</span><span class="sxs-lookup"><span data-stu-id="9f81b-340">Mouse pointer</span></span>    | `PointerEventArgs`   | <span data-ttu-id="9f81b-341">`onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture`</span><span class="sxs-lookup"><span data-stu-id="9f81b-341">`onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture`</span></span> |
| <span data-ttu-id="9f81b-342">Rueda del ratón</span><span class="sxs-lookup"><span data-stu-id="9f81b-342">Mouse wheel</span></span>      | `WheelEventArgs`     | <span data-ttu-id="9f81b-343">`onwheel`, `onmousewheel`</span><span class="sxs-lookup"><span data-stu-id="9f81b-343">`onwheel`, `onmousewheel`</span></span> |
| <span data-ttu-id="9f81b-344">Progreso</span><span class="sxs-lookup"><span data-stu-id="9f81b-344">Progress</span></span>         | `ProgressEventArgs`  | <span data-ttu-id="9f81b-345">`onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress`, `ontimeout`</span><span class="sxs-lookup"><span data-stu-id="9f81b-345">`onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress`, `ontimeout`</span></span> |
| <span data-ttu-id="9f81b-346">Entrada táctil</span><span class="sxs-lookup"><span data-stu-id="9f81b-346">Touch</span></span>            | `TouchEventArgs`     | <span data-ttu-id="9f81b-347">`ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave`, `ontouchcancel`</span><span class="sxs-lookup"><span data-stu-id="9f81b-347">`ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave`, `ontouchcancel`</span></span><br><br><span data-ttu-id="9f81b-348">`TouchPoint` representa un único punto de contacto en un dispositivo con distinción de toque.</span><span class="sxs-lookup"><span data-stu-id="9f81b-348">`TouchPoint` represents a single contact point on a touch-sensitive device.</span></span> |

<span data-ttu-id="9f81b-349">Para obtener información sobre las propiedades y el comportamiento de control de eventos de los eventos de la tabla anterior, vea [clases EventArgs en el origen de referencia (rama ASPNET/AspNetCore Release/3.0)](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Components/Web/src/Web).</span><span class="sxs-lookup"><span data-stu-id="9f81b-349">For information on the properties and event handling behavior of the events in the preceding table, see [EventArgs classes in the reference source (aspnet/AspNetCore release/3.0 branch)](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Components/Web/src/Web).</span></span>

### <a name="lambda-expressions"></a><span data-ttu-id="9f81b-350">Expresiones lambda</span><span class="sxs-lookup"><span data-stu-id="9f81b-350">Lambda expressions</span></span>

<span data-ttu-id="9f81b-351">También se pueden usar expresiones lambda:</span><span class="sxs-lookup"><span data-stu-id="9f81b-351">Lambda expressions can also be used:</span></span>

```razor
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="9f81b-352">A menudo resulta cómodo cerrar los valores adicionales, como al recorrer en iteración un conjunto de elementos.</span><span class="sxs-lookup"><span data-stu-id="9f81b-352">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="9f81b-353">En el ejemplo siguiente se crean tres botones, cada uno de los cuales llama a `UpdateHeading` pasar un argumento de evento (`MouseEventArgs`) y su número de botón (`buttonNumber`) cuando se selecciona en la interfaz de usuario:</span><span class="sxs-lookup"><span data-stu-id="9f81b-353">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`MouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

```razor
<h2>@message</h2>

@for (var i = 1; i < 4; i++)
{
    var buttonNumber = i;

    <button class="btn btn-primary"
            @onclick="@(e => UpdateHeading(e, buttonNumber))">
        Button #@i
    </button>
}

@code {
    private string message = "Select a button to learn its position.";

    private void UpdateHeading(MouseEventArgs e, int buttonNumber)
    {
        message = $"You selected Button #{buttonNumber} at " +
            $"mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> <span data-ttu-id="9f81b-354">**No** utilice la variable de bucle (`i`) en un bucle de `for` directamente en una expresión lambda.</span><span class="sxs-lookup"><span data-stu-id="9f81b-354">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="9f81b-355">De lo contrario, todas las expresiones lambda usan la misma variable, lo que hace que el valor de `i`sea el mismo en todas las expresiones lambda.</span><span class="sxs-lookup"><span data-stu-id="9f81b-355">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="9f81b-356">Capture siempre su valor en una variable local (`buttonNumber` en el ejemplo anterior) y, a continuación, úsela.</span><span class="sxs-lookup"><span data-stu-id="9f81b-356">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

### <a name="eventcallback"></a><span data-ttu-id="9f81b-357">EventCallback</span><span class="sxs-lookup"><span data-stu-id="9f81b-357">EventCallback</span></span>

<span data-ttu-id="9f81b-358">Un escenario común con los componentes anidados es el deseo de ejecutar el método de un componente primario cuando se produce un evento de componente secundario&mdash;por ejemplo, cuando se produce un evento de `onclick` en el elemento secundario.</span><span class="sxs-lookup"><span data-stu-id="9f81b-358">A common scenario with nested components is the desire to run a parent component's method when a child component event occurs&mdash;for example, when an `onclick` event occurs in the child.</span></span> <span data-ttu-id="9f81b-359">Para exponer eventos entre componentes, use una `EventCallback`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-359">To expose events across components, use an `EventCallback`.</span></span> <span data-ttu-id="9f81b-360">Un componente primario puede asignar un método de devolución de llamada al `EventCallback`de un componente secundario.</span><span class="sxs-lookup"><span data-stu-id="9f81b-360">A parent component can assign a callback method to a child component's `EventCallback`.</span></span>

<span data-ttu-id="9f81b-361">En el `ChildComponent` de la aplicación de ejemplo (*Components/ChildComponent. Razor*) se muestra cómo se configura el controlador de `onclick` de un botón para recibir un `EventCallback` delegado de la `ParentComponent`del ejemplo.</span><span class="sxs-lookup"><span data-stu-id="9f81b-361">The `ChildComponent` in the sample app (*Components/ChildComponent.razor*) demonstrates how a button's `onclick` handler is set up to receive an `EventCallback` delegate from the sample's `ParentComponent`.</span></span> <span data-ttu-id="9f81b-362">El `EventCallback` se escribe con `MouseEventArgs`, que es adecuado para un evento de `onclick` desde un dispositivo periférico:</span><span class="sxs-lookup"><span data-stu-id="9f81b-362">The `EventCallback` is typed with `MouseEventArgs`, which is appropriate for an `onclick` event from a peripheral device:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=5-7,17-18)]

<span data-ttu-id="9f81b-363">El `ParentComponent` establece el `EventCallback<T>` del elemento secundario en su método `ShowMessage`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-363">The `ParentComponent` sets the child's `EventCallback<T>` to its `ShowMessage` method.</span></span>

<span data-ttu-id="9f81b-364">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="9f81b-364">*Pages/ParentComponent.razor*:</span></span>

```razor
@page "/ParentComponent"

<h1>Parent-child example</h1>

<ChildComponent Title="Panel Title from Parent"
                OnClick="@ShowMessage">
    Content of the child component is supplied
    by the parent component.
</ChildComponent>

<p><b>@messageText</b></p>

@code {
    private string messageText;

    private void ShowMessage(MouseEventArgs e)
    {
        messageText = $"Blaze a new trail with Blazor! ({e.ScreenX}, {e.ScreenY})";
    }
}
```

<span data-ttu-id="9f81b-365">Cuando el botón está seleccionado en el `ChildComponent`:</span><span class="sxs-lookup"><span data-stu-id="9f81b-365">When the button is selected in the `ChildComponent`:</span></span>

* <span data-ttu-id="9f81b-366">Se llama al método `ShowMessage` del `ParentComponent`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-366">The `ParentComponent`'s `ShowMessage` method is called.</span></span> <span data-ttu-id="9f81b-367">`messageText` se actualiza y se muestra en el `ParentComponent`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-367">`messageText` is updated and displayed in the `ParentComponent`.</span></span>
* <span data-ttu-id="9f81b-368">No se requiere una llamada a [StateHasChanged](xref:blazor/lifecycle#state-changes) en el método de la devolución de llamada (`ShowMessage`).</span><span class="sxs-lookup"><span data-stu-id="9f81b-368">A call to [StateHasChanged](xref:blazor/lifecycle#state-changes) isn't required in the callback's method (`ShowMessage`).</span></span> <span data-ttu-id="9f81b-369">se llama a `StateHasChanged` automáticamente para rerepresentar el `ParentComponent`, del mismo modo que los eventos secundarios desencadenan la rerepresentación de componentes en los controladores de eventos que se ejecutan dentro del elemento secundario.</span><span class="sxs-lookup"><span data-stu-id="9f81b-369">`StateHasChanged` is called automatically to rerender the `ParentComponent`, just as child events trigger component rerendering in event handlers that execute within the child.</span></span>

<span data-ttu-id="9f81b-370">`EventCallback` y `EventCallback<T>` permiten los delegados asincrónicos.</span><span class="sxs-lookup"><span data-stu-id="9f81b-370">`EventCallback` and `EventCallback<T>` permit asynchronous delegates.</span></span> <span data-ttu-id="9f81b-371">`EventCallback<T>` tiene un tipo seguro y requiere un tipo de argumento específico.</span><span class="sxs-lookup"><span data-stu-id="9f81b-371">`EventCallback<T>` is strongly typed and requires a specific argument type.</span></span> <span data-ttu-id="9f81b-372">`EventCallback` tiene un tipo débil y permite cualquier tipo de argumento.</span><span class="sxs-lookup"><span data-stu-id="9f81b-372">`EventCallback` is weakly typed and allows any argument type.</span></span>

```razor
<p><b>@messageText</b></p>

@{ var message = "Default Text"; }

<ChildComponent 
    OnClick="@(async () => { await Task.Yield(); messageText = "Blaze It!"; })" />

@code {
    private string messageText;
}
```

<span data-ttu-id="9f81b-373">Invocar un `EventCallback` o `EventCallback<T>` con `InvokeAsync` y esperar el <xref:System.Threading.Tasks.Task>:</span><span class="sxs-lookup"><span data-stu-id="9f81b-373">Invoke an `EventCallback` or `EventCallback<T>` with `InvokeAsync` and await the <xref:System.Threading.Tasks.Task>:</span></span>

```csharp
await callback.InvokeAsync(arg);
```

<span data-ttu-id="9f81b-374">Use `EventCallback` y `EventCallback<T>` para el control de eventos y los parámetros de componente de enlace.</span><span class="sxs-lookup"><span data-stu-id="9f81b-374">Use `EventCallback` and `EventCallback<T>` for event handling and binding component parameters.</span></span>

<span data-ttu-id="9f81b-375">Prefiera el `EventCallback<T>` fuertemente tipado en `EventCallback`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-375">Prefer the strongly typed `EventCallback<T>` over `EventCallback`.</span></span> <span data-ttu-id="9f81b-376">`EventCallback<T>` proporciona mejores comentarios de errores a los usuarios del componente.</span><span class="sxs-lookup"><span data-stu-id="9f81b-376">`EventCallback<T>` provides better error feedback to users of the component.</span></span> <span data-ttu-id="9f81b-377">Al igual que otros controladores de eventos de la interfaz de usuario, la especificación del parámetro Event es opcional.</span><span class="sxs-lookup"><span data-stu-id="9f81b-377">Similar to other UI event handlers, specifying the event parameter is optional.</span></span> <span data-ttu-id="9f81b-378">Use `EventCallback` cuando no haya ningún valor pasado a la devolución de llamada.</span><span class="sxs-lookup"><span data-stu-id="9f81b-378">Use `EventCallback` when there's no value passed to the callback.</span></span>

::: moniker range=">= aspnetcore-3.1"

### <a name="prevent-default-actions"></a><span data-ttu-id="9f81b-379">Impedir acciones predeterminadas</span><span class="sxs-lookup"><span data-stu-id="9f81b-379">Prevent default actions</span></span>

<span data-ttu-id="9f81b-380">Use el atributo de directiva de [`@on{EVENT}:preventDefault`](xref:mvc/views/razor#oneventpreventdefault) para evitar la acción predeterminada de un evento.</span><span class="sxs-lookup"><span data-stu-id="9f81b-380">Use the [`@on{EVENT}:preventDefault`](xref:mvc/views/razor#oneventpreventdefault) directive attribute to prevent the default action for an event.</span></span>

<span data-ttu-id="9f81b-381">Cuando se selecciona una clave en un dispositivo de entrada y el foco del elemento está en un cuadro de texto, un explorador muestra normalmente el carácter de la tecla en el cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="9f81b-381">When a key is selected on an input device and the element focus is on a text box, a browser normally displays the key's character in the text box.</span></span> <span data-ttu-id="9f81b-382">En el ejemplo siguiente, el comportamiento predeterminado se evita especificando el atributo de directiva de `@onkeypress:preventDefault`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-382">In the following example, the default behavior is prevented by specifying the `@onkeypress:preventDefault` directive attribute.</span></span> <span data-ttu-id="9f81b-383">El contador se incrementa y la clave **+** no se captura en el valor del elemento `<input>`:</span><span class="sxs-lookup"><span data-stu-id="9f81b-383">The counter increments, and the **+** key isn't captured into the `<input>` element's value:</span></span>

```razor
<input value="@_count" @onkeypress="KeyHandler" @onkeypress:preventDefault />

@code {
    private int _count = 0;

    private void KeyHandler(KeyboardEventArgs e)
    {
        if (e.Key == "+")
        {
            _count++;
        }
    }
}
```

<span data-ttu-id="9f81b-384">Especificar el `@on{EVENT}:preventDefault` atributo sin un valor es equivalente a `@on{EVENT}:preventDefault="true"`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-384">Specifying the `@on{EVENT}:preventDefault` attribute without a value is equivalent to `@on{EVENT}:preventDefault="true"`.</span></span>

<span data-ttu-id="9f81b-385">El valor del atributo también puede ser una expresión.</span><span class="sxs-lookup"><span data-stu-id="9f81b-385">The value of the attribute can also be an expression.</span></span> <span data-ttu-id="9f81b-386">En el ejemplo siguiente, `_shouldPreventDefault` es un campo `bool` establecido en `true` o `false`:</span><span class="sxs-lookup"><span data-stu-id="9f81b-386">In the following example, `_shouldPreventDefault` is a `bool` field set to either `true` or `false`:</span></span>

```razor
<input @onkeypress:preventDefault="_shouldPreventDefault" />
```

<span data-ttu-id="9f81b-387">No es necesario un controlador de eventos para evitar la acción predeterminada.</span><span class="sxs-lookup"><span data-stu-id="9f81b-387">An event handler isn't required to prevent the default action.</span></span> <span data-ttu-id="9f81b-388">El controlador de eventos y evitar escenarios de acción predeterminados se pueden usar de forma independiente.</span><span class="sxs-lookup"><span data-stu-id="9f81b-388">The event handler and prevent default action scenarios can be used independently.</span></span>

### <a name="stop-event-propagation"></a><span data-ttu-id="9f81b-389">Detener propagación de eventos</span><span class="sxs-lookup"><span data-stu-id="9f81b-389">Stop event propagation</span></span>

<span data-ttu-id="9f81b-390">Use el atributo de directiva de [`@on{EVENT}:stopPropagation`](xref:mvc/views/razor#oneventstoppropagation) para detener la propagación de eventos.</span><span class="sxs-lookup"><span data-stu-id="9f81b-390">Use the [`@on{EVENT}:stopPropagation`](xref:mvc/views/razor#oneventstoppropagation) directive attribute to stop event propagation.</span></span>

<span data-ttu-id="9f81b-391">En el ejemplo siguiente, al activar la casilla se impide que los eventos de clic de la segunda `<div>` secundaria se propaguen al `<div>`primario:</span><span class="sxs-lookup"><span data-stu-id="9f81b-391">In the following example, selecting the check box prevents click events from the second child `<div>` from propagating to the parent `<div>`:</span></span>

```razor
<label>
    <input @bind="_stopPropagation" type="checkbox" />
    Stop Propagation
</label>

<div @onclick="OnSelectParentDiv">
    <h3>Parent div</h3>

    <div @onclick="OnSelectChildDiv">
        Child div that doesn't stop propagation when selected.
    </div>

    <div @onclick="OnSelectChildDiv" @onclick:stopPropagation="_stopPropagation">
        Child div that stops propagation when selected.
    </div>
</div>

@code {
    private bool _stopPropagation = false;

    private void OnSelectParentDiv() => 
        Console.WriteLine($"The parent div was selected. {DateTime.Now}");
    private void OnSelectChildDiv() => 
        Console.WriteLine($"A child div was selected. {DateTime.Now}");
}
```

::: moniker-end

## <a name="chained-bind"></a><span data-ttu-id="9f81b-392">Enlace encadenado</span><span class="sxs-lookup"><span data-stu-id="9f81b-392">Chained bind</span></span>

<span data-ttu-id="9f81b-393">Un escenario común es encadenar un parámetro enlazado a datos a un elemento Page en la salida del componente.</span><span class="sxs-lookup"><span data-stu-id="9f81b-393">A common scenario is chaining a data-bound parameter to a page element in the component's output.</span></span> <span data-ttu-id="9f81b-394">Este escenario se denomina *enlace encadenado* porque se producen varios niveles de enlace simultáneamente.</span><span class="sxs-lookup"><span data-stu-id="9f81b-394">This scenario is called a *chained bind* because multiple levels of binding occur simultaneously.</span></span>

<span data-ttu-id="9f81b-395">No se puede implementar un enlace encadenado con `@bind` sintaxis en el elemento de la página.</span><span class="sxs-lookup"><span data-stu-id="9f81b-395">A chained bind can't be implemented with `@bind` syntax in the page's element.</span></span> <span data-ttu-id="9f81b-396">El controlador de eventos y el valor se deben especificar por separado.</span><span class="sxs-lookup"><span data-stu-id="9f81b-396">The event handler and value must be specified separately.</span></span> <span data-ttu-id="9f81b-397">Sin embargo, un componente primario puede usar la sintaxis de `@bind` con el parámetro del componente.</span><span class="sxs-lookup"><span data-stu-id="9f81b-397">A parent component, however, can use `@bind` syntax with the component's parameter.</span></span>

<span data-ttu-id="9f81b-398">El siguiente componente de `PasswordField` (*PasswordField. Razor*):</span><span class="sxs-lookup"><span data-stu-id="9f81b-398">The following `PasswordField` component (*PasswordField.razor*):</span></span>

* <span data-ttu-id="9f81b-399">Establece el valor de un elemento de `<input>` en una propiedad `Password`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-399">Sets an `<input>` element's value to a `Password` property.</span></span>
* <span data-ttu-id="9f81b-400">Expone los cambios de la propiedad `Password` a un componente primario con un [EventCallback](#eventcallback).</span><span class="sxs-lookup"><span data-stu-id="9f81b-400">Exposes changes of the `Password` property to a parent component with an [EventCallback](#eventcallback).</span></span>

```razor
Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

@code {
    private bool showPassword;

    [Parameter]
    public string Password { get; set; }

    [Parameter]
    public EventCallback<string> PasswordChanged { get; set; }

    private Task OnPasswordChanged(ChangeEventArgs e)
    {
        Password = e.Value.ToString();

        return PasswordChanged.InvokeAsync(Password);
    }

    private void ToggleShowPassword()
    {
        showPassword = !showPassword;
    }
}
```

<span data-ttu-id="9f81b-401">El componente de `PasswordField` se usa en otro componente:</span><span class="sxs-lookup"><span data-stu-id="9f81b-401">The `PasswordField` component is used in another component:</span></span>

```razor
<PasswordField @bind-Password="password" />

@code {
    private string password;
}
```

<span data-ttu-id="9f81b-402">Para realizar comprobaciones o errores de captura en la contraseña en el ejemplo anterior:</span><span class="sxs-lookup"><span data-stu-id="9f81b-402">To perform checks or trap errors on the password in the preceding example:</span></span>

* <span data-ttu-id="9f81b-403">Cree un campo de respaldo para `Password` (`password` en el siguiente código de ejemplo).</span><span class="sxs-lookup"><span data-stu-id="9f81b-403">Create a backing field for `Password` (`password` in the following example code).</span></span>
* <span data-ttu-id="9f81b-404">Realice las comprobaciones o errores de captura en el establecedor de `Password`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-404">Perform the checks or trap errors in the `Password` setter.</span></span>

<span data-ttu-id="9f81b-405">En el ejemplo siguiente se proporcionan comentarios inmediatos al usuario si se usa un espacio en el valor de la contraseña:</span><span class="sxs-lookup"><span data-stu-id="9f81b-405">The following example provides immediate feedback to the user if a space is used in the password's value:</span></span>

```razor
Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

<span class="text-danger">@validationMessage</span>

@code {
    private bool showPassword;
    private string password;
    private string validationMessage;

    [Parameter]
    public string Password
    {
        get { return password ?? string.Empty; }
        set
        {
            if (password != value)
            {
                if (value.Contains(' '))
                {
                    validationMessage = "Spaces not allowed!";
                }
                else
                {
                    password = value;
                    validationMessage = string.Empty;
                }
            }
        }
    }

    [Parameter]
    public EventCallback<string> PasswordChanged { get; set; }

    private Task OnPasswordChanged(ChangeEventArgs e)
    {
        Password = e.Value.ToString();

        return PasswordChanged.InvokeAsync(Password);
    }

    private void ToggleShowPassword()
    {
        showPassword = !showPassword;
    }
}
```

## <a name="capture-references-to-components"></a><span data-ttu-id="9f81b-406">Capturar referencias a componentes</span><span class="sxs-lookup"><span data-stu-id="9f81b-406">Capture references to components</span></span>

<span data-ttu-id="9f81b-407">Las referencias de componente proporcionan una manera de hacer referencia a una instancia de componente para que pueda emitir comandos a esa instancia, como `Show` o `Reset`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-407">Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="9f81b-408">Para capturar una referencia de componente:</span><span class="sxs-lookup"><span data-stu-id="9f81b-408">To capture a component reference:</span></span>

* <span data-ttu-id="9f81b-409">Agregue un atributo [`@ref`](xref:mvc/views/razor#ref) al componente secundario.</span><span class="sxs-lookup"><span data-stu-id="9f81b-409">Add an [`@ref`](xref:mvc/views/razor#ref) attribute to the child component.</span></span>
* <span data-ttu-id="9f81b-410">Defina un campo con el mismo tipo que el componente secundario.</span><span class="sxs-lookup"><span data-stu-id="9f81b-410">Define a field with the same type as the child component.</span></span>

```razor
<MyLoginDialog @ref="loginDialog" ... />

@code {
    private MyLoginDialog loginDialog;

    private void OnSomething()
    {
        loginDialog.Show();
    }
}
```

<span data-ttu-id="9f81b-411">Cuando se representa el componente, el campo de `loginDialog` se rellena con la instancia del componente secundario `MyLoginDialog`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-411">When the component is rendered, the `loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="9f81b-412">Después, puede invocar métodos .NET en la instancia del componente.</span><span class="sxs-lookup"><span data-stu-id="9f81b-412">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9f81b-413">La variable `loginDialog` solo se rellena después de que el componente se represente y su salida incluye el elemento `MyLoginDialog`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-413">The `loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="9f81b-414">Hasta ese momento, no hay nada que hacer referencia.</span><span class="sxs-lookup"><span data-stu-id="9f81b-414">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="9f81b-415">Para manipular las referencias de componentes una vez finalizada la representación del componente, use los [métodos OnAfterRenderAsync o OnAfterRender](xref:blazor/lifecycle#after-component-render).</span><span class="sxs-lookup"><span data-stu-id="9f81b-415">To manipulate components references after the component has finished rendering, use the [OnAfterRenderAsync or OnAfterRender methods](xref:blazor/lifecycle#after-component-render).</span></span>

<span data-ttu-id="9f81b-416">Al capturar referencias de componentes, use una sintaxis similar para [capturar referencias de elemento](xref:blazor/javascript-interop#capture-references-to-elements), no es una característica de [interoperabilidad de JavaScript](xref:blazor/javascript-interop) .</span><span class="sxs-lookup"><span data-stu-id="9f81b-416">While capturing component references use a similar syntax to [capturing element references](xref:blazor/javascript-interop#capture-references-to-elements), it isn't a [JavaScript interop](xref:blazor/javascript-interop) feature.</span></span> <span data-ttu-id="9f81b-417">Las referencias a componentes no se pasan al código JavaScript&mdash;solo se usan en código .NET.</span><span class="sxs-lookup"><span data-stu-id="9f81b-417">Component references aren't passed to JavaScript code&mdash;they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="9f81b-418">**No** utilice referencias de componentes para mutar el estado de los componentes secundarios.</span><span class="sxs-lookup"><span data-stu-id="9f81b-418">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="9f81b-419">En su lugar, use parámetros declarativos normales para pasar datos a componentes secundarios.</span><span class="sxs-lookup"><span data-stu-id="9f81b-419">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="9f81b-420">El uso de parámetros declarativos normales da como resultado componentes secundarios que se representarán automáticamente en las horas correctas.</span><span class="sxs-lookup"><span data-stu-id="9f81b-420">Use of normal declarative parameters result in child components that rerender at the correct times automatically.</span></span>

## <a name="invoke-component-methods-externally-to-update-state"></a><span data-ttu-id="9f81b-421">Invocar métodos de componentes externamente para actualizar el estado</span><span class="sxs-lookup"><span data-stu-id="9f81b-421">Invoke component methods externally to update state</span></span>

Blazor<span data-ttu-id="9f81b-422"> usa un `SynchronizationContext` para aplicar un único subproceso lógico de ejecución.</span><span class="sxs-lookup"><span data-stu-id="9f81b-422"> uses a `SynchronizationContext` to enforce a single logical thread of execution.</span></span> <span data-ttu-id="9f81b-423">[Los métodos de ciclo de vida](xref:blazor/lifecycle) de un componente y las devoluciones de llamada de eventos que se producen en Blazor se ejecutan en esta `SynchronizationContext`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-423">A component's [lifecycle methods](xref:blazor/lifecycle) and any event callbacks that are raised by Blazor are executed on this `SynchronizationContext`.</span></span> <span data-ttu-id="9f81b-424">En caso de que un componente deba actualizarse en función de un evento externo, como un temporizador u otras notificaciones, use el método `InvokeAsync`, que se enviará al `SynchronizationContext`de Blazor.</span><span class="sxs-lookup"><span data-stu-id="9f81b-424">In the event a component must be updated based on an external event, such as a timer or other notifications, use the `InvokeAsync` method, which will dispatch to Blazor's `SynchronizationContext`.</span></span>

<span data-ttu-id="9f81b-425">Por ejemplo, considere un *servicio de notificador* que puede notificar a cualquier componente de escucha del estado actualizado:</span><span class="sxs-lookup"><span data-stu-id="9f81b-425">For example, consider a *notifier service* that can notify any listening component of the updated state:</span></span>

```csharp
public class NotifierService
{
    // Can be called from anywhere
    public async Task Update(string key, int value)
    {
        if (Notify != null)
        {
            await Notify.Invoke(key, value);
        }
    }

    public event Func<string, int, Task> Notify;
}
```

<span data-ttu-id="9f81b-426">Uso de la `NotifierService` para actualizar un componente:</span><span class="sxs-lookup"><span data-stu-id="9f81b-426">Usage of the `NotifierService` to update a component:</span></span>

```razor
@page "/"
@inject NotifierService Notifier
@implements IDisposable

<p>Last update: @lastNotification.key = @lastNotification.value</p>

@code {
    private (string key, int value) lastNotification;

    protected override void OnInitialized()
    {
        Notifier.Notify += OnNotify;
    }

    public async Task OnNotify(string key, int value)
    {
        await InvokeAsync(() =>
        {
            lastNotification = (key, value);
            StateHasChanged();
        });
    }

    public void Dispose()
    {
        Notifier.Notify -= OnNotify;
    }
}
```

<span data-ttu-id="9f81b-427">En el ejemplo anterior, `NotifierService` invoca el método de `OnNotify` del componente fuera del `SynchronizationContext`de Blazor.</span><span class="sxs-lookup"><span data-stu-id="9f81b-427">In the preceding example, `NotifierService` invokes the component's `OnNotify` method outside of Blazor's `SynchronizationContext`.</span></span> <span data-ttu-id="9f81b-428">`InvokeAsync` se utiliza para cambiar al contexto correcto y poner en cola una representación.</span><span class="sxs-lookup"><span data-stu-id="9f81b-428">`InvokeAsync` is used to switch to the correct context and queue a render.</span></span>

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a><span data-ttu-id="9f81b-429">Usar \@clave para controlar la preservación de elementos y componentes</span><span class="sxs-lookup"><span data-stu-id="9f81b-429">Use \@key to control the preservation of elements and components</span></span>

<span data-ttu-id="9f81b-430">Cuando se representa una lista de elementos o componentes, y los elementos o componentes cambian posteriormente, el algoritmo de comparación de Blazordebe decidir cuáles de los elementos o componentes anteriores se pueden conservar y cómo deben asignarse los objetos de modelo.</span><span class="sxs-lookup"><span data-stu-id="9f81b-430">When rendering a list of elements or components and the elements or components subsequently change, Blazor's diffing algorithm must decide which of the previous elements or components can be retained and how model objects should map to them.</span></span> <span data-ttu-id="9f81b-431">Normalmente, este proceso es automático y se puede omitir, pero hay casos en los que puede que desee controlar el proceso.</span><span class="sxs-lookup"><span data-stu-id="9f81b-431">Normally, this process is automatic and can be ignored, but there are cases where you may want to control the process.</span></span>

<span data-ttu-id="9f81b-432">Considere el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="9f81b-432">Consider the following example:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor Details="person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="9f81b-433">El contenido de la colección de `People` puede cambiar con entradas insertadas, eliminadas o reordenadas.</span><span class="sxs-lookup"><span data-stu-id="9f81b-433">The contents of the `People` collection may change with inserted, deleted, or re-ordered entries.</span></span> <span data-ttu-id="9f81b-434">Cuando se representa el componente, el componente de `<DetailsEditor>` puede cambiar para recibir diferentes valores de parámetro de `Details`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-434">When the component rerenders, the `<DetailsEditor>` component may change to receive different `Details` parameter values.</span></span> <span data-ttu-id="9f81b-435">Esto puede producir una rerepresentación más compleja de lo esperado.</span><span class="sxs-lookup"><span data-stu-id="9f81b-435">This may cause more complex rerendering than expected.</span></span> <span data-ttu-id="9f81b-436">En algunos casos, la rerepresentación puede producir diferencias de comportamiento visibles, como el foco del elemento perdido.</span><span class="sxs-lookup"><span data-stu-id="9f81b-436">In some cases, rerendering can lead to visible behavior differences, such as lost element focus.</span></span>

<span data-ttu-id="9f81b-437">El proceso de asignación se puede controlar con el `@key` atributo de directiva.</span><span class="sxs-lookup"><span data-stu-id="9f81b-437">The mapping process can be controlled with the `@key` directive attribute.</span></span> <span data-ttu-id="9f81b-438">`@key` hace que el algoritmo de comparación garantice la preservación de elementos o componentes basados en el valor de la clave:</span><span class="sxs-lookup"><span data-stu-id="9f81b-438">`@key` causes the diffing algorithm to guarantee preservation of elements or components based on the key's value:</span></span>

```csharp
@foreach (var person in People)
{
    <DetailsEditor @key="person" Details="person.Details" />
}

@code {
    [Parameter]
    public IEnumerable<Person> People { get; set; }
}
```

<span data-ttu-id="9f81b-439">Cuando cambia la colección de `People`, el algoritmo de comparación mantiene la asociación entre `<DetailsEditor>` instancias y `person` instancias:</span><span class="sxs-lookup"><span data-stu-id="9f81b-439">When the `People` collection changes, the diffing algorithm retains the association between `<DetailsEditor>` instances and `person` instances:</span></span>

* <span data-ttu-id="9f81b-440">Si se elimina un `Person` de la lista de `People`, solo se quita la instancia de `<DetailsEditor>` correspondiente de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="9f81b-440">If a `Person` is deleted from the `People` list, only the corresponding `<DetailsEditor>` instance is removed from the UI.</span></span> <span data-ttu-id="9f81b-441">Otras instancias permanecen sin cambios.</span><span class="sxs-lookup"><span data-stu-id="9f81b-441">Other instances are left unchanged.</span></span>
* <span data-ttu-id="9f81b-442">Si se inserta un `Person` en alguna posición de la lista, se inserta una nueva instancia de `<DetailsEditor>` en la posición correspondiente.</span><span class="sxs-lookup"><span data-stu-id="9f81b-442">If a `Person` is inserted at some position in the list, one new `<DetailsEditor>` instance is inserted at that corresponding position.</span></span> <span data-ttu-id="9f81b-443">Otras instancias permanecen sin cambios.</span><span class="sxs-lookup"><span data-stu-id="9f81b-443">Other instances are left unchanged.</span></span>
* <span data-ttu-id="9f81b-444">Si se vuelven a ordenar `Person` entradas, se conservan y se vuelven a ordenar las instancias de `<DetailsEditor>` correspondientes en la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="9f81b-444">If `Person` entries are re-ordered, the corresponding `<DetailsEditor>` instances are preserved and re-ordered in the UI.</span></span>

<span data-ttu-id="9f81b-445">En algunos escenarios, el uso de `@key` minimiza la complejidad de la rerepresentación y evita posibles problemas con las partes con estado del DOM que cambian, como la posición del foco.</span><span class="sxs-lookup"><span data-stu-id="9f81b-445">In some scenarios, use of `@key` minimizes the complexity of rerendering and avoids potential issues with stateful parts of the DOM changing, such as focus position.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9f81b-446">Las claves son locales para cada elemento contenedor o componente.</span><span class="sxs-lookup"><span data-stu-id="9f81b-446">Keys are local to each container element or component.</span></span> <span data-ttu-id="9f81b-447">Las claves no se comparan globalmente en todo el documento.</span><span class="sxs-lookup"><span data-stu-id="9f81b-447">Keys aren't compared globally across the document.</span></span>

### <a name="when-to-use-key"></a><span data-ttu-id="9f81b-448">Cuándo usar la clave de \@</span><span class="sxs-lookup"><span data-stu-id="9f81b-448">When to use \@key</span></span>

<span data-ttu-id="9f81b-449">Normalmente, tiene sentido usar `@key` cada vez que se representa una lista (por ejemplo, en un bloque de `@foreach`) y existe un valor adecuado para definir el `@key`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-449">Typically, it makes sense to use `@key` whenever a list is rendered (for example, in a `@foreach` block) and a suitable value exists to define the `@key`.</span></span>

<span data-ttu-id="9f81b-450">También puede usar `@key` para evitar que Blazor conserven un subárbol de elementos o componentes cuando cambie un objeto:</span><span class="sxs-lookup"><span data-stu-id="9f81b-450">You can also use `@key` to prevent Blazor from preserving an element or component subtree when an object changes:</span></span>

```razor
<div @key="currentPerson">
    ... content that depends on currentPerson ...
</div>
```

<span data-ttu-id="9f81b-451">Si `@currentPerson` cambia, la Directiva de `@key` atributo fuerza a Blazor a descartar toda la `<div>` y sus descendientes y volver a generar el subárbol dentro de la interfaz de usuario con nuevos elementos y componentes.</span><span class="sxs-lookup"><span data-stu-id="9f81b-451">If `@currentPerson` changes, the `@key` attribute directive forces Blazor to discard the entire `<div>` and its descendants and rebuild the subtree within the UI with new elements and components.</span></span> <span data-ttu-id="9f81b-452">Esto puede ser útil si necesita garantizar que no se conserva ningún estado de la interfaz de usuario cuando `@currentPerson` cambios.</span><span class="sxs-lookup"><span data-stu-id="9f81b-452">This can be useful if you need to guarantee that no UI state is preserved when `@currentPerson` changes.</span></span>

### <a name="when-not-to-use-key"></a><span data-ttu-id="9f81b-453">Cuándo no usar la clave \@</span><span class="sxs-lookup"><span data-stu-id="9f81b-453">When not to use \@key</span></span>

<span data-ttu-id="9f81b-454">Existe un costo de rendimiento al diferenciar con `@key`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-454">There's a performance cost when diffing with `@key`.</span></span> <span data-ttu-id="9f81b-455">El costo de rendimiento no es grande, pero solo especifica `@key` si el control del elemento o las reglas de conservación de componentes se benefician de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9f81b-455">The performance cost isn't large, but only specify `@key` if controlling the element or component preservation rules benefit the app.</span></span>

<span data-ttu-id="9f81b-456">Incluso si no se utiliza `@key`, Blazor conserva las instancias de componente y elemento secundario lo máximo posible.</span><span class="sxs-lookup"><span data-stu-id="9f81b-456">Even if `@key` isn't used, Blazor preserves child element and component instances as much as possible.</span></span> <span data-ttu-id="9f81b-457">La única ventaja de utilizar `@key` es el control sobre *Cómo* se asignan las instancias de modelo a las instancias de componente conservadas, en lugar del algoritmo de diferenciación que selecciona la asignación.</span><span class="sxs-lookup"><span data-stu-id="9f81b-457">The only advantage to using `@key` is control over *how* model instances are mapped to the preserved component instances, instead of the diffing algorithm selecting the mapping.</span></span>

### <a name="what-values-to-use-for-key"></a><span data-ttu-id="9f81b-458">Qué valores se deben usar para la clave de \@</span><span class="sxs-lookup"><span data-stu-id="9f81b-458">What values to use for \@key</span></span>

<span data-ttu-id="9f81b-459">Por lo general, tiene sentido proporcionar uno de los siguientes tipos de valor para `@key`:</span><span class="sxs-lookup"><span data-stu-id="9f81b-459">Generally, it makes sense to supply one of the following kinds of value for `@key`:</span></span>

* <span data-ttu-id="9f81b-460">Instancias de objeto de modelo (por ejemplo, una instancia de `Person` como en el ejemplo anterior).</span><span class="sxs-lookup"><span data-stu-id="9f81b-460">Model object instances (for example, a `Person` instance as in the earlier example).</span></span> <span data-ttu-id="9f81b-461">Esto garantiza la conservación en función de la igualdad de la referencia de objeto.</span><span class="sxs-lookup"><span data-stu-id="9f81b-461">This ensures preservation based on object reference equality.</span></span>
* <span data-ttu-id="9f81b-462">Los identificadores únicos (por ejemplo, los valores de clave principal de tipo `int`, `string`o `Guid`).</span><span class="sxs-lookup"><span data-stu-id="9f81b-462">Unique identifiers (for example, primary key values of type `int`, `string`, or `Guid`).</span></span>

<span data-ttu-id="9f81b-463">Asegúrese de que los valores usados para `@key` no entren en conflicto.</span><span class="sxs-lookup"><span data-stu-id="9f81b-463">Ensure that values used for `@key` don't clash.</span></span> <span data-ttu-id="9f81b-464">Si se detectan valores en conflicto en el mismo elemento primario, Blazor produce una excepción porque no puede asignar de forma determinista elementos o componentes antiguos a nuevos elementos o componentes.</span><span class="sxs-lookup"><span data-stu-id="9f81b-464">If clashing values are detected within the same parent element, Blazor throws an exception because it can't deterministically map old elements or components to new elements or components.</span></span> <span data-ttu-id="9f81b-465">Utilice solo valores distintos, como instancias de objeto o valores de clave principal.</span><span class="sxs-lookup"><span data-stu-id="9f81b-465">Only use distinct values, such as object instances or primary key values.</span></span>

## <a name="routing"></a><span data-ttu-id="9f81b-466">Enrutamiento</span><span class="sxs-lookup"><span data-stu-id="9f81b-466">Routing</span></span>

<span data-ttu-id="9f81b-467">El enrutamiento en Blazor se consigue proporcionando una plantilla de ruta a cada componente accesible en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9f81b-467">Routing in Blazor is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="9f81b-468">Cuando se compila un archivo de Razor con una directiva de `@page`, a la clase generada se le asigna un <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> que especifica la plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="9f81b-468">When a Razor file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="9f81b-469">En tiempo de ejecución, el enrutador busca clases de componentes con un `RouteAttribute` y representa el componente que tiene una plantilla de ruta que coincide con la dirección URL solicitada.</span><span class="sxs-lookup"><span data-stu-id="9f81b-469">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="9f81b-470">Se pueden aplicar varias plantillas de ruta a un componente.</span><span class="sxs-lookup"><span data-stu-id="9f81b-470">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="9f81b-471">El siguiente componente responde a las solicitudes de `/BlazorRoute` y `/DifferentBlazorRoute`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-471">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`.</span></span>

<span data-ttu-id="9f81b-472">*Pages/BlazorRoute. Razor*:</span><span class="sxs-lookup"><span data-stu-id="9f81b-472">*Pages/BlazorRoute.razor*:</span></span>

```razor
@page "/BlazorRoute"
@page "/DifferentBlazorRoute"

<h1>Blazor routing</h1>
```

## <a name="route-parameters"></a><span data-ttu-id="9f81b-473">Parámetros de ruta</span><span class="sxs-lookup"><span data-stu-id="9f81b-473">Route parameters</span></span>

<span data-ttu-id="9f81b-474">Los componentes pueden recibir parámetros de ruta de la plantilla de ruta proporcionada en la Directiva de `@page`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-474">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="9f81b-475">El enrutador usa parámetros de ruta para rellenar los parámetros de componente correspondientes.</span><span class="sxs-lookup"><span data-stu-id="9f81b-475">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="9f81b-476">*Pages/RouteParameter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="9f81b-476">*Pages/RouteParameter.razor*:</span></span>

```razor
@page "/RouteParameter"
@page "/RouteParameter/{text}"

<h1>Blazor is @Text!</h1>

@code {
    [Parameter]
    public string Text { get; set; }

    protected override void OnInitialized()
    {
        Text = Text ?? "fantastic";
    }
}
```

<span data-ttu-id="9f81b-477">Los parámetros opcionales no se admiten, por lo que se aplican dos directivas de `@page` en el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="9f81b-477">Optional parameters aren't supported, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="9f81b-478">El primero permite la navegación al componente sin un parámetro.</span><span class="sxs-lookup"><span data-stu-id="9f81b-478">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="9f81b-479">La segunda Directiva de `@page` toma el parámetro `{text}` Route y asigna el valor a la propiedad `Text`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-479">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

<span data-ttu-id="9f81b-480">La sintaxis de *los parámetros catch-all* (`*`/`**`), que capturan la ruta de acceso en varios límites de carpeta, **no** se admite en los componentes de Razor ( *. Razor*).</span><span class="sxs-lookup"><span data-stu-id="9f81b-480">*Catch-all* parameter syntax (`*`/`**`), which captures the path across multiple folder boundaries, is **not** supported in Razor components (*.razor*).</span></span>

::: moniker range=">= aspnetcore-3.1"

## <a name="partial-class-support"></a><span data-ttu-id="9f81b-481">Compatibilidad de clases parciales</span><span class="sxs-lookup"><span data-stu-id="9f81b-481">Partial class support</span></span>

<span data-ttu-id="9f81b-482">Los componentes de Razor se generan como clases parciales.</span><span class="sxs-lookup"><span data-stu-id="9f81b-482">Razor components are generated as partial classes.</span></span> <span data-ttu-id="9f81b-483">Los componentes de Razor se crean mediante cualquiera de los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="9f81b-483">Razor components are authored using either of the following approaches:</span></span>

* <span data-ttu-id="9f81b-484">C#el código se define en un bloque [`@code`](xref:mvc/views/razor#code) con marcado HTML y código Razor en un único archivo.</span><span class="sxs-lookup"><span data-stu-id="9f81b-484">C# code is defined in an [`@code`](xref:mvc/views/razor#code) block with HTML markup and Razor code in a single file.</span></span> Blazor<span data-ttu-id="9f81b-485"> plantillas definen sus componentes de Razor mediante este enfoque.</span><span class="sxs-lookup"><span data-stu-id="9f81b-485"> templates define their Razor components using this approach.</span></span>
* <span data-ttu-id="9f81b-486">C#el código se coloca en un archivo de código subyacente definido como una clase parcial.</span><span class="sxs-lookup"><span data-stu-id="9f81b-486">C# code is placed in a code-behind file defined as a partial class.</span></span>

<span data-ttu-id="9f81b-487">En el ejemplo siguiente se muestra el componente de `Counter` predeterminado con un bloque de `@code` en una aplicación generada a partir de una plantilla de Blazor.</span><span class="sxs-lookup"><span data-stu-id="9f81b-487">The following example shows the default `Counter` component with an `@code` block in an app generated from a Blazor template.</span></span> <span data-ttu-id="9f81b-488">El marcado HTML, el código Razor C# y el código se encuentran en el mismo archivo:</span><span class="sxs-lookup"><span data-stu-id="9f81b-488">HTML markup, Razor code, and C# code are in the same file:</span></span>

<span data-ttu-id="9f81b-489">*Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="9f81b-489">*Counter.razor*:</span></span>

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>

@code {
    int currentCount = 0;

    void IncrementCount()
    {
        currentCount++;
    }
}
```

<span data-ttu-id="9f81b-490">El componente de `Counter` también se puede crear con un archivo de código subyacente con una clase parcial:</span><span class="sxs-lookup"><span data-stu-id="9f81b-490">The `Counter` component can also be created using a code-behind file with a partial class:</span></span>

<span data-ttu-id="9f81b-491">*Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="9f81b-491">*Counter.razor*:</span></span>

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>
```

<span data-ttu-id="9f81b-492">*Counter.Razor.CS*:</span><span class="sxs-lookup"><span data-stu-id="9f81b-492">*Counter.razor.cs*:</span></span>

```csharp
namespace BlazorApp.Pages
{
    public partial class Counter
    {
        int currentCount = 0;

        void IncrementCount()
        {
            currentCount++;
        }
    }
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.1"

## <a name="specify-a-component-base-class"></a><span data-ttu-id="9f81b-493">Especificar una clase base de componente</span><span class="sxs-lookup"><span data-stu-id="9f81b-493">Specify a component base class</span></span>

<span data-ttu-id="9f81b-494">La Directiva `@inherits` se puede utilizar para especificar una clase base para un componente.</span><span class="sxs-lookup"><span data-stu-id="9f81b-494">The `@inherits` directive can be used to specify a base class for a component.</span></span>

<span data-ttu-id="9f81b-495">La [aplicación de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) muestra cómo un componente puede heredar una clase base, `BlazorRocksBase`, para proporcionar las propiedades y los métodos del componente.</span><span class="sxs-lookup"><span data-stu-id="9f81b-495">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span>

<span data-ttu-id="9f81b-496">*Pages/BlazorRocks. Razor*:</span><span class="sxs-lookup"><span data-stu-id="9f81b-496">*Pages/BlazorRocks.razor*:</span></span>

```razor
@page "/BlazorRocks"
@inherits BlazorRocksBase

<h1>@BlazorRocksText</h1>
```

<span data-ttu-id="9f81b-497">*BlazorRocksBase.cs*:</span><span class="sxs-lookup"><span data-stu-id="9f81b-497">*BlazorRocksBase.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Components;

namespace BlazorSample
{
    public class BlazorRocksBase : ComponentBase
    {
        public string BlazorRocksText { get; set; } = 
            "Blazor rocks the browser!";
    }
}
```

<span data-ttu-id="9f81b-498">La clase base debe derivar de `ComponentBase`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-498">The base class should derive from `ComponentBase`.</span></span>

::: moniker-end

## <a name="import-components"></a><span data-ttu-id="9f81b-499">Importar componentes</span><span class="sxs-lookup"><span data-stu-id="9f81b-499">Import components</span></span>

<span data-ttu-id="9f81b-500">El espacio de nombres de un componente creado con Razor se basa en (en orden de prioridad):</span><span class="sxs-lookup"><span data-stu-id="9f81b-500">The namespace of a component authored with Razor is based on (in priority order):</span></span>

* <span data-ttu-id="9f81b-501">[`@namespace`](xref:mvc/views/razor#namespace) designación en el marcado de archivos Razor ( *. Razor*) (`@namespace BlazorSample.MyNamespace`).</span><span class="sxs-lookup"><span data-stu-id="9f81b-501">[`@namespace`](xref:mvc/views/razor#namespace) designation in Razor file (*.razor*) markup (`@namespace BlazorSample.MyNamespace`).</span></span>
* <span data-ttu-id="9f81b-502">`RootNamespace` del proyecto en el archivo de proyecto (`<RootNamespace>BlazorSample</RootNamespace>`).</span><span class="sxs-lookup"><span data-stu-id="9f81b-502">The project's `RootNamespace` in the project file (`<RootNamespace>BlazorSample</RootNamespace>`).</span></span>
* <span data-ttu-id="9f81b-503">Nombre del proyecto, tomado del nombre de archivo del archivo de proyecto ( *. csproj*) y la ruta de acceso de la raíz del proyecto al componente.</span><span class="sxs-lookup"><span data-stu-id="9f81b-503">The project name, taken from the project file's file name (*.csproj*), and the path from the project root to the component.</span></span> <span data-ttu-id="9f81b-504">Por ejemplo, el marco de trabajo resuelve *{root Project}/pages/index.Razor* (*BlazorSample. csproj*) en el espacio de nombres `BlazorSample.Pages`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-504">For example, the framework resolves *{PROJECT ROOT}/Pages/Index.razor* (*BlazorSample.csproj*) to the namespace `BlazorSample.Pages`.</span></span> <span data-ttu-id="9f81b-505">Los componentes C# siguen las reglas de enlace de nombres.</span><span class="sxs-lookup"><span data-stu-id="9f81b-505">Components follow C# name binding rules.</span></span> <span data-ttu-id="9f81b-506">En el caso del componente `Index` en este ejemplo, los componentes del ámbito son todos los componentes:</span><span class="sxs-lookup"><span data-stu-id="9f81b-506">For the `Index` component in this example, the components in scope are all of the components:</span></span>
  * <span data-ttu-id="9f81b-507">En la misma carpeta, *páginas*.</span><span class="sxs-lookup"><span data-stu-id="9f81b-507">In the same folder, *Pages*.</span></span>
  * <span data-ttu-id="9f81b-508">Los componentes de la raíz del proyecto que no especifican explícitamente un espacio de nombres diferente.</span><span class="sxs-lookup"><span data-stu-id="9f81b-508">The components in the project's root that don't explicitly specify a different namespace.</span></span>

<span data-ttu-id="9f81b-509">Los componentes definidos en un espacio de nombres diferente se incluyen en el ámbito mediante la Directiva de [`@using`](xref:mvc/views/razor#using) de Razor.</span><span class="sxs-lookup"><span data-stu-id="9f81b-509">Components defined in a different namespace are brought into scope using Razor's [`@using`](xref:mvc/views/razor#using) directive.</span></span>

<span data-ttu-id="9f81b-510">Si existe otro componente, `NavMenu.razor`, en la carpeta *BlazorSample/Shared/* , el componente se puede utilizar en `Index.razor` con la siguiente instrucción `@using`:</span><span class="sxs-lookup"><span data-stu-id="9f81b-510">If another component, `NavMenu.razor`, exists in the *BlazorSample/Shared/* folder, the component can be used in `Index.razor` with the following `@using` statement:</span></span>

```razor
@using BlazorSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

<span data-ttu-id="9f81b-511">También se puede hacer referencia a los componentes mediante sus nombres completos, que no requieren la directiva [`@using`](xref:mvc/views/razor#using) :</span><span class="sxs-lookup"><span data-stu-id="9f81b-511">Components can also be referenced using their fully qualified names, which doesn't require the [`@using`](xref:mvc/views/razor#using) directive:</span></span>

```razor
This is the Index page.

<BlazorSample.Shared.NavMenu></BlazorSample.Shared.NavMenu>
```

> [!NOTE]
> <span data-ttu-id="9f81b-512">No se admite la calificación `global::`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-512">The `global::` qualification isn't supported.</span></span>
>
> <span data-ttu-id="9f81b-513">No se admite la importación de componentes con instrucciones de `using` con alias (por ejemplo, `@using Foo = Bar`).</span><span class="sxs-lookup"><span data-stu-id="9f81b-513">Importing components with aliased `using` statements (for example, `@using Foo = Bar`) isn't supported.</span></span>
>
> <span data-ttu-id="9f81b-514">No se admiten nombres parcialmente completos.</span><span class="sxs-lookup"><span data-stu-id="9f81b-514">Partially qualified names aren't supported.</span></span> <span data-ttu-id="9f81b-515">Por ejemplo, no se admite agregar `@using BlazorSample` y hacer referencia a `NavMenu.razor` con `<Shared.NavMenu></Shared.NavMenu>`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-515">For example, adding `@using BlazorSample` and referencing `NavMenu.razor` with `<Shared.NavMenu></Shared.NavMenu>` isn't supported.</span></span>

## <a name="conditional-html-element-attributes"></a><span data-ttu-id="9f81b-516">Atributos de elementos HTML condicionales</span><span class="sxs-lookup"><span data-stu-id="9f81b-516">Conditional HTML element attributes</span></span>

<span data-ttu-id="9f81b-517">Los atributos de elemento HTML se representan condicionalmente según el valor de .NET.</span><span class="sxs-lookup"><span data-stu-id="9f81b-517">HTML element attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="9f81b-518">Si el valor es `false` o `null`, el atributo no se representa.</span><span class="sxs-lookup"><span data-stu-id="9f81b-518">If the value is `false` or `null`, the attribute isn't rendered.</span></span> <span data-ttu-id="9f81b-519">Si el valor es `true`, el atributo se representa minimizado.</span><span class="sxs-lookup"><span data-stu-id="9f81b-519">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="9f81b-520">En el ejemplo siguiente, `IsCompleted` determina si `checked` se representa en el marcado del elemento:</span><span class="sxs-lookup"><span data-stu-id="9f81b-520">In the following example, `IsCompleted` determines if `checked` is rendered in the element's markup:</span></span>

```razor
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

<span data-ttu-id="9f81b-521">Si `IsCompleted` se `true`, la casilla se representa como:</span><span class="sxs-lookup"><span data-stu-id="9f81b-521">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="9f81b-522">Si `IsCompleted` se `false`, la casilla se representa como:</span><span class="sxs-lookup"><span data-stu-id="9f81b-522">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="9f81b-523">Para obtener más información, vea <xref:mvc/views/razor>.</span><span class="sxs-lookup"><span data-stu-id="9f81b-523">For more information, see <xref:mvc/views/razor>.</span></span>

> [!WARNING]
> <span data-ttu-id="9f81b-524">Algunos atributos HTML, como [Aria-pressed](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), no funcionan correctamente cuando el tipo .net es un `bool`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-524">Some HTML attributes, such as [aria-pressed](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), don't function properly when the .NET type is a `bool`.</span></span> <span data-ttu-id="9f81b-525">En esos casos, use un tipo de `string` en lugar de un `bool`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-525">In those cases, use a `string` type instead of a `bool`.</span></span>

## <a name="raw-html"></a><span data-ttu-id="9f81b-526">HTML sin formato</span><span class="sxs-lookup"><span data-stu-id="9f81b-526">Raw HTML</span></span>

<span data-ttu-id="9f81b-527">Normalmente, las cadenas se representan mediante nodos de texto DOM, lo que significa que cualquier marcado que pueda contener se omite y se trata como texto literal.</span><span class="sxs-lookup"><span data-stu-id="9f81b-527">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="9f81b-528">Para representar HTML sin formato, ajuste el contenido HTML en un valor de `MarkupString`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-528">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="9f81b-529">El valor se analiza como HTML o SVG y se inserta en el DOM.</span><span class="sxs-lookup"><span data-stu-id="9f81b-529">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="9f81b-530">La representación de HTML sin formato construido a partir de un origen que no es de confianza es un riesgo para la **seguridad** y debe evitarse.</span><span class="sxs-lookup"><span data-stu-id="9f81b-530">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="9f81b-531">En el ejemplo siguiente se muestra el uso del tipo de `MarkupString` para agregar un bloque de contenido HTML estático a la salida representada de un componente:</span><span class="sxs-lookup"><span data-stu-id="9f81b-531">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)myMarkup)

@code {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a><span data-ttu-id="9f81b-532">Componentes con plantilla</span><span class="sxs-lookup"><span data-stu-id="9f81b-532">Templated components</span></span>

<span data-ttu-id="9f81b-533">Los componentes con plantilla son componentes que aceptan una o varias plantillas de interfaz de usuario como parámetros, que se pueden usar como parte de la lógica de representación del componente.</span><span class="sxs-lookup"><span data-stu-id="9f81b-533">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="9f81b-534">Los componentes con plantilla permiten crear componentes de nivel superior que son más reutilizables que los componentes normales.</span><span class="sxs-lookup"><span data-stu-id="9f81b-534">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="9f81b-535">Algunos ejemplos son:</span><span class="sxs-lookup"><span data-stu-id="9f81b-535">A couple of examples include:</span></span>

* <span data-ttu-id="9f81b-536">Componente de tabla que permite a un usuario especificar plantillas para el encabezado, las filas y el pie de página de la tabla.</span><span class="sxs-lookup"><span data-stu-id="9f81b-536">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="9f81b-537">Componente de lista que permite a un usuario especificar una plantilla para representar elementos en una lista.</span><span class="sxs-lookup"><span data-stu-id="9f81b-537">A list component that allows a user to specify a template for rendering items in a list.</span></span>

### <a name="template-parameters"></a><span data-ttu-id="9f81b-538">Parámetros de plantilla</span><span class="sxs-lookup"><span data-stu-id="9f81b-538">Template parameters</span></span>

<span data-ttu-id="9f81b-539">Un componente con plantilla se define especificando uno o más parámetros de componente de tipo `RenderFragment` o `RenderFragment<T>`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-539">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="9f81b-540">Un fragmento de representación representa un segmento de interfaz de usuario que se va a representar.</span><span class="sxs-lookup"><span data-stu-id="9f81b-540">A render fragment represents a segment of UI to render.</span></span> <span data-ttu-id="9f81b-541">`RenderFragment<T>` toma un parámetro de tipo que se puede especificar cuando se invoca el fragmento de representación.</span><span class="sxs-lookup"><span data-stu-id="9f81b-541">`RenderFragment<T>` takes a type parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="9f81b-542">`TableTemplate` componente:</span><span class="sxs-lookup"><span data-stu-id="9f81b-542">`TableTemplate` component:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/TableTemplate.razor)]

<span data-ttu-id="9f81b-543">Cuando se usa un componente con plantilla, los parámetros de plantilla se pueden especificar utilizando los elementos secundarios que coinciden con los nombres de los parámetros (`TableHeader` y `RowTemplate` en el ejemplo siguiente):</span><span class="sxs-lookup"><span data-stu-id="9f81b-543">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

```razor
<TableTemplate Items="pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@context.PetId</td>
        <td>@context.Name</td>
    </RowTemplate>
</TableTemplate>
```

### <a name="template-context-parameters"></a><span data-ttu-id="9f81b-544">Parámetros de contexto de plantilla</span><span class="sxs-lookup"><span data-stu-id="9f81b-544">Template context parameters</span></span>

<span data-ttu-id="9f81b-545">Los argumentos de los componentes de tipo `RenderFragment<T>` pasados como elementos tienen un parámetro implícito denominado `context` (por ejemplo, en el ejemplo de código anterior, `@context.PetId`), pero puede cambiar el nombre del parámetro mediante el atributo `Context` en el elemento secundario.</span><span class="sxs-lookup"><span data-stu-id="9f81b-545">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="9f81b-546">En el ejemplo siguiente, el atributo `Context` del elemento `RowTemplate` especifica el parámetro `pet`:</span><span class="sxs-lookup"><span data-stu-id="9f81b-546">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

```razor
<TableTemplate Items="pets">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate Context="pet">
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

<span data-ttu-id="9f81b-547">También puede especificar el atributo de `Context` en el elemento de componente.</span><span class="sxs-lookup"><span data-stu-id="9f81b-547">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="9f81b-548">El atributo de `Context` especificado se aplica a todos los parámetros de plantilla especificados.</span><span class="sxs-lookup"><span data-stu-id="9f81b-548">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="9f81b-549">Esto puede ser útil si desea especificar el nombre del parámetro de contenido para el contenido secundario implícito (sin ningún elemento secundario de ajuste).</span><span class="sxs-lookup"><span data-stu-id="9f81b-549">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="9f81b-550">En el ejemplo siguiente, el atributo `Context` aparece en el elemento `TableTemplate` y se aplica a todos los parámetros de plantilla:</span><span class="sxs-lookup"><span data-stu-id="9f81b-550">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

```razor
<TableTemplate Items="pets" Context="pet">
    <TableHeader>
        <th>ID</th>
        <th>Name</th>
    </TableHeader>
    <RowTemplate>
        <td>@pet.PetId</td>
        <td>@pet.Name</td>
    </RowTemplate>
</TableTemplate>
```

### <a name="generic-typed-components"></a><span data-ttu-id="9f81b-551">Componentes de tipo genérico</span><span class="sxs-lookup"><span data-stu-id="9f81b-551">Generic-typed components</span></span>

<span data-ttu-id="9f81b-552">Los componentes con plantilla suelen tener tipos genéricos.</span><span class="sxs-lookup"><span data-stu-id="9f81b-552">Templated components are often generically typed.</span></span> <span data-ttu-id="9f81b-553">Por ejemplo, se puede usar un componente de `ListViewTemplate` genérico para representar valores de `IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-553">For example, a generic `ListViewTemplate` component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="9f81b-554">Para definir un componente genérico, utilice la directiva [`@typeparam`](xref:mvc/views/razor#typeparam) para especificar parámetros de tipo:</span><span class="sxs-lookup"><span data-stu-id="9f81b-554">To define a generic component, use the [`@typeparam`](xref:mvc/views/razor#typeparam) directive to specify type parameters:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ListViewTemplate.razor)]

<span data-ttu-id="9f81b-555">Cuando se usan componentes de tipo genérico, el parámetro de tipo se infiere si es posible:</span><span class="sxs-lookup"><span data-stu-id="9f81b-555">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```razor
<ListViewTemplate Items="pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="9f81b-556">De lo contrario, el parámetro de tipo se debe especificar explícitamente mediante un atributo que coincida con el nombre del parámetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="9f81b-556">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="9f81b-557">En el ejemplo siguiente, `TItem="Pet"` especifica el tipo:</span><span class="sxs-lookup"><span data-stu-id="9f81b-557">In the following example, `TItem="Pet"` specifies the type:</span></span>

```razor
<ListViewTemplate Items="pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="9f81b-558">Valores y parámetros en cascada</span><span class="sxs-lookup"><span data-stu-id="9f81b-558">Cascading values and parameters</span></span>

<span data-ttu-id="9f81b-559">En algunos escenarios, no es conveniente fluir los datos de un componente antecesor a un componente descendiente mediante [parámetros de componente](#component-parameters), especialmente cuando hay varios niveles de componentes.</span><span class="sxs-lookup"><span data-stu-id="9f81b-559">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="9f81b-560">Los valores y parámetros en cascada solucionan este problema proporcionando un método cómodo para que un componente antecesor proporcione un valor a todos sus componentes descendientes.</span><span class="sxs-lookup"><span data-stu-id="9f81b-560">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="9f81b-561">Los valores y parámetros en cascada también proporcionan un enfoque para que los componentes se coordinen.</span><span class="sxs-lookup"><span data-stu-id="9f81b-561">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="9f81b-562">Ejemplo de tema</span><span class="sxs-lookup"><span data-stu-id="9f81b-562">Theme example</span></span>

<span data-ttu-id="9f81b-563">En el ejemplo siguiente de la aplicación de ejemplo, la clase `ThemeInfo` especifica la información del tema que va a fluir hacia abajo en la jerarquía de componentes para que todos los botones de una parte determinada de la aplicación compartan el mismo estilo.</span><span class="sxs-lookup"><span data-stu-id="9f81b-563">In the following example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="9f81b-564">*UIThemeClasses/ThemeInfo.cs*:</span><span class="sxs-lookup"><span data-stu-id="9f81b-564">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="9f81b-565">Un componente antecesor puede proporcionar un valor en cascada mediante el componente de valor en cascada.</span><span class="sxs-lookup"><span data-stu-id="9f81b-565">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="9f81b-566">El componente `CascadingValue` encapsula un subárbol de la jerarquía de componentes y proporciona un valor único a todos los componentes de ese subárbol.</span><span class="sxs-lookup"><span data-stu-id="9f81b-566">The `CascadingValue` component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="9f81b-567">Por ejemplo, la aplicación de ejemplo especifica información de tema (`ThemeInfo`) en uno de los diseños de la aplicación como parámetro en cascada para todos los componentes que componen el cuerpo del diseño de la propiedad `@Body`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-567">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="9f81b-568">a `ButtonClass` se le asigna un valor de `btn-success` en el componente de diseño.</span><span class="sxs-lookup"><span data-stu-id="9f81b-568">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="9f81b-569">Cualquier componente descendiente puede consumir esta propiedad mediante el `ThemeInfo` objeto en cascada.</span><span class="sxs-lookup"><span data-stu-id="9f81b-569">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="9f81b-570">`CascadingValuesParametersLayout` componente:</span><span class="sxs-lookup"><span data-stu-id="9f81b-570">`CascadingValuesParametersLayout` component:</span></span>

```razor
@inherits LayoutComponentBase
@using BlazorSample.UIThemeClasses

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3">
            <NavMenu />
        </div>
        <div class="col-sm-9">
            <CascadingValue Value="theme">
                <div class="content px-4">
                    @Body
                </div>
            </CascadingValue>
        </div>
    </div>
</div>

@code {
    private ThemeInfo theme = new ThemeInfo { ButtonClass = "btn-success" };
}
```

<span data-ttu-id="9f81b-571">Para usar valores en cascada, los componentes declaran parámetros en cascada mediante el atributo `[CascadingParameter]`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-571">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute.</span></span> <span data-ttu-id="9f81b-572">Los valores en cascada se enlazan a los parámetros en cascada por tipo.</span><span class="sxs-lookup"><span data-stu-id="9f81b-572">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="9f81b-573">En la aplicación de ejemplo, el componente `CascadingValuesParametersTheme` enlaza el valor en cascada `ThemeInfo` a un parámetro en cascada.</span><span class="sxs-lookup"><span data-stu-id="9f81b-573">In the sample app, the `CascadingValuesParametersTheme` component binds the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="9f81b-574">El parámetro se usa para establecer la clase CSS para uno de los botones mostrados por el componente.</span><span class="sxs-lookup"><span data-stu-id="9f81b-574">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="9f81b-575">`CascadingValuesParametersTheme` componente:</span><span class="sxs-lookup"><span data-stu-id="9f81b-575">`CascadingValuesParametersTheme` component:</span></span>

```razor
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @currentCount</p>

<p>
    <button class="btn" @onclick="IncrementCount">
        Increment Counter (Unthemed)
    </button>
</p>

<p>
    <button class="btn @ThemeInfo.ButtonClass" @onclick="IncrementCount">
        Increment Counter (Themed)
    </button>
</p>

@code {
    private int currentCount = 0;

    [CascadingParameter]
    protected ThemeInfo ThemeInfo { get; set; }

    private void IncrementCount()
    {
        currentCount++;
    }
}
```

<span data-ttu-id="9f81b-576">Para poner en cascada varios valores del mismo tipo en el mismo subárbol, proporcione una cadena de `Name` única para cada componente `CascadingValue` y su `CascadingParameter`correspondiente.</span><span class="sxs-lookup"><span data-stu-id="9f81b-576">To cascade multiple values of the same type within the same subtree, provide a unique `Name` string to each `CascadingValue` component and its corresponding `CascadingParameter`.</span></span> <span data-ttu-id="9f81b-577">En el ejemplo siguiente, dos componentes de `CascadingValue` en cascada son instancias diferentes de `MyCascadingType` por nombre:</span><span class="sxs-lookup"><span data-stu-id="9f81b-577">In the following example, two `CascadingValue` components cascade different instances of `MyCascadingType` by name:</span></span>

```razor
<CascadingValue Value=@ParentCascadeParameter1 Name="CascadeParam1">
    <CascadingValue Value=@ParentCascadeParameter2 Name="CascadeParam2">
        ...
    </CascadingValue>
</CascadingValue>

@code {
    private MyCascadingType ParentCascadeParameter1;

    [Parameter]
    public MyCascadingType ParentCascadeParameter2 { get; set; }

    ...
}
```

<span data-ttu-id="9f81b-578">En un componente descendiente, los parámetros en cascada reciben sus valores de los valores en cascada correspondientes del componente antecesor por nombre:</span><span class="sxs-lookup"><span data-stu-id="9f81b-578">In a descendant component, the cascaded parameters receive their values from the corresponding cascaded values in the ancestor component by name:</span></span>

```razor
...

@code {
    [CascadingParameter(Name = "CascadeParam1")]
    protected MyCascadingType ChildCascadeParameter1 { get; set; }
    
    [CascadingParameter(Name = "CascadeParam2")]
    protected MyCascadingType ChildCascadeParameter2 { get; set; }
}
```

### <a name="tabset-example"></a><span data-ttu-id="9f81b-579">Ejemplo de TabSet</span><span class="sxs-lookup"><span data-stu-id="9f81b-579">TabSet example</span></span>

<span data-ttu-id="9f81b-580">Los parámetros en cascada también permiten que los componentes colaboren en la jerarquía de componentes.</span><span class="sxs-lookup"><span data-stu-id="9f81b-580">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="9f81b-581">Por ejemplo, considere el siguiente ejemplo de *TabSet* en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="9f81b-581">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="9f81b-582">La aplicación de ejemplo tiene una interfaz `ITab` que las pestañas implementan:</span><span class="sxs-lookup"><span data-stu-id="9f81b-582">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-csharp[](common/samples/3.x/BlazorWebAssemblySample/UIInterfaces/ITab.cs)]

<span data-ttu-id="9f81b-583">El componente `CascadingValuesParametersTabSet` usa el componente `TabSet`, que contiene varios componentes `Tab`:</span><span class="sxs-lookup"><span data-stu-id="9f81b-583">The `CascadingValuesParametersTabSet` component uses the `TabSet` component, which contains several `Tab` components:</span></span>

```razor
<TabSet>
    <Tab Title="First tab">
        <h4>Greetings from the first tab!</h4>

        <label>
            <input type="checkbox" @bind="showThirdTab" />
            Toggle third tab
        </label>
    </Tab>
    <Tab Title="Second tab">
        <h4>The second tab says Hello World!</h4>
    </Tab>

    @if (showThirdTab)
    {
        <Tab Title="Third tab">
            <h4>Welcome to the disappearing third tab!</h4>
            <p>Toggle this tab from the first tab.</p>
        </Tab>
    }
</TabSet>
```

<span data-ttu-id="9f81b-584">Los componentes de `Tab` secundarios no se pasan explícitamente como parámetros a la `TabSet`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-584">The child `Tab` components aren't explicitly passed as parameters to the `TabSet`.</span></span> <span data-ttu-id="9f81b-585">En su lugar, los componentes secundarios `Tab` forman parte del contenido secundario del `TabSet`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-585">Instead, the child `Tab` components are part of the child content of the `TabSet`.</span></span> <span data-ttu-id="9f81b-586">Sin embargo, el `TabSet` todavía necesita saber sobre cada componente `Tab` para que pueda representar los encabezados y la pestaña activa. Para habilitar esta coordinación sin necesidad de código adicional, el componente de `TabSet` *puede proporcionarse como un valor en cascada* que los componentes de `Tab` descendientes recogen.</span><span class="sxs-lookup"><span data-stu-id="9f81b-586">However, the `TabSet` still needs to know about each `Tab` component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the `TabSet` component *can provide itself as a cascading value* that is then picked up by the descendent `Tab` components.</span></span>

<span data-ttu-id="9f81b-587">`TabSet` componente:</span><span class="sxs-lookup"><span data-stu-id="9f81b-587">`TabSet` component:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/TabSet.razor)]

<span data-ttu-id="9f81b-588">Los componentes de `Tab` descendientes capturan el `TabSet` contenedor como un parámetro en cascada, por lo que los componentes de `Tab` se agregan a los `TabSet` y coordenadas en los que la pestaña está activa.</span><span class="sxs-lookup"><span data-stu-id="9f81b-588">The descendent `Tab` components capture the containing `TabSet` as a cascading parameter, so the `Tab` components add themselves to the `TabSet` and coordinate on which tab is active.</span></span>

<span data-ttu-id="9f81b-589">`Tab` componente:</span><span class="sxs-lookup"><span data-stu-id="9f81b-589">`Tab` component:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/Tab.razor)]

## <a name="razor-templates"></a><span data-ttu-id="9f81b-590">Plantillas de Razor</span><span class="sxs-lookup"><span data-stu-id="9f81b-590">Razor templates</span></span>

<span data-ttu-id="9f81b-591">Los fragmentos de representación se pueden definir mediante la sintaxis de plantilla de Razor.</span><span class="sxs-lookup"><span data-stu-id="9f81b-591">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="9f81b-592">Las plantillas de Razor son una manera de definir un fragmento de la interfaz de usuario y suponer el siguiente formato:</span><span class="sxs-lookup"><span data-stu-id="9f81b-592">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```razor
@<{HTML tag}>...</{HTML tag}>
```

<span data-ttu-id="9f81b-593">En el ejemplo siguiente se muestra cómo especificar los valores de `RenderFragment` y `RenderFragment<T>` y las plantillas de representación directamente en un componente.</span><span class="sxs-lookup"><span data-stu-id="9f81b-593">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values and render templates directly in a component.</span></span> <span data-ttu-id="9f81b-594">Los fragmentos de representación también se pueden pasar como argumentos a [componentes con plantilla](#templated-components).</span><span class="sxs-lookup"><span data-stu-id="9f81b-594">Render fragments can also be passed as arguments to [templated components](#templated-components).</span></span>

```razor
@timeTemplate

@petTemplate(new Pet { Name = "Rex" })

@code {
    private RenderFragment timeTemplate = @<p>The time is @DateTime.Now.</p>;
    private RenderFragment<Pet> petTemplate = 
        (pet) => @<p>Your pet's name is @pet.Name.</p>;

    private class Pet
    {
        public string Name { get; set; }
    }
}
```

<span data-ttu-id="9f81b-595">Salida representada del código anterior:</span><span class="sxs-lookup"><span data-stu-id="9f81b-595">Rendered output of the preceding code:</span></span>

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Your pet's name is Rex.</p>
```

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="9f81b-596">Lógica de RenderTreeBuilder manual</span><span class="sxs-lookup"><span data-stu-id="9f81b-596">Manual RenderTreeBuilder logic</span></span>

<span data-ttu-id="9f81b-597">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` proporciona métodos para manipular componentes y elementos, incluida la creación manual de componentes C# en el código.</span><span class="sxs-lookup"><span data-stu-id="9f81b-597">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="9f81b-598">El uso de `RenderTreeBuilder` para crear componentes es un escenario avanzado.</span><span class="sxs-lookup"><span data-stu-id="9f81b-598">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="9f81b-599">Un componente con formato incorrecto (por ejemplo, una etiqueta de marcado sin cerrar) puede dar lugar a un comportamiento indefinido.</span><span class="sxs-lookup"><span data-stu-id="9f81b-599">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="9f81b-600">Tenga en cuenta el siguiente componente de `PetDetails`, que se puede integrar manualmente en otro componente:</span><span class="sxs-lookup"><span data-stu-id="9f81b-600">Consider the following `PetDetails` component, which can be manually built into another component:</span></span>

```razor
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="9f81b-601">En el ejemplo siguiente, el bucle del método `CreateComponent` genera tres componentes `PetDetails`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-601">In the following example, the loop in the `CreateComponent` method generates three `PetDetails` components.</span></span> <span data-ttu-id="9f81b-602">Al llamar a métodos `RenderTreeBuilder` para crear los componentes (`OpenComponent` y `AddAttribute`), los números de secuencia son números de línea de código fuente.</span><span class="sxs-lookup"><span data-stu-id="9f81b-602">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="9f81b-603">El algoritmo de diferencia de Blazor se basa en los números de secuencia correspondientes a líneas de código distintas, no a invocaciones de llamada distintas.</span><span class="sxs-lookup"><span data-stu-id="9f81b-603">The Blazor difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="9f81b-604">Al crear un componente con métodos de `RenderTreeBuilder`, codifique los argumentos para los números de secuencia.</span><span class="sxs-lookup"><span data-stu-id="9f81b-604">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> <span data-ttu-id="9f81b-605">**El uso de un cálculo o un contador para generar el número de secuencia puede dar lugar a un rendimiento deficiente.**</span><span class="sxs-lookup"><span data-stu-id="9f81b-605">**Using a calculation or counter to generate the sequence number can lead to poor performance.**</span></span> <span data-ttu-id="9f81b-606">Para obtener más información, vea la sección [números de secuencia relacionados con números de línea de código y no orden de ejecución](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) .</span><span class="sxs-lookup"><span data-stu-id="9f81b-606">For more information, see the [Sequence numbers relate to code line numbers and not execution order](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) section.</span></span>

<span data-ttu-id="9f81b-607">`BuiltContent` componente:</span><span class="sxs-lookup"><span data-stu-id="9f81b-607">`BuiltContent` component:</span></span>

```razor
@page "/BuiltContent"

<h1>Build a component</h1>

@CustomRender

<button type="button" @onclick="RenderComponent">
    Create three Pet Details components
</button>

@code {
    private RenderFragment CustomRender { get; set; }
    
    private RenderFragment CreateComponent() => builder =>
    {
        for (var i = 0; i < 3; i++) 
        {
            builder.OpenComponent(0, typeof(PetDetails));
            builder.AddAttribute(1, "PetDetailsQuote", "Someone's best friend!");
            builder.CloseComponent();
        }
    };    
    
    private void RenderComponent()
    {
        CustomRender = CreateComponent();
    }
}
```

> [!WARNING]
> <span data-ttu-id="9f81b-608">Los tipos de `Microsoft.AspNetCore.Components.RenderTree` permiten el procesamiento de los *resultados* de las operaciones de representación.</span><span class="sxs-lookup"><span data-stu-id="9f81b-608">The types in `Microsoft.AspNetCore.Components.RenderTree` allow processing of the *results* of rendering operations.</span></span> <span data-ttu-id="9f81b-609">Estos son los detalles internos de la implementación del marco de Blazor.</span><span class="sxs-lookup"><span data-stu-id="9f81b-609">These are internal details of the Blazor framework implementation.</span></span> <span data-ttu-id="9f81b-610">Estos tipos se deben considerar *inestables* y estar sujetos a cambios en futuras versiones.</span><span class="sxs-lookup"><span data-stu-id="9f81b-610">These types should be considered *unstable* and subject to change in future releases.</span></span>

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a><span data-ttu-id="9f81b-611">Los números de secuencia se relacionan con los números de línea de código y no el orden de ejecución</span><span class="sxs-lookup"><span data-stu-id="9f81b-611">Sequence numbers relate to code line numbers and not execution order</span></span>

<span data-ttu-id="9f81b-612">los archivos de `.razor` de Blazor siempre se compilan.</span><span class="sxs-lookup"><span data-stu-id="9f81b-612">Blazor `.razor` files are always compiled.</span></span> <span data-ttu-id="9f81b-613">Esta es potencialmente una gran ventaja para `.razor` porque el paso de compilación se puede usar para insertar información que mejoran el rendimiento de las aplicaciones en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="9f81b-613">This is potentially a great advantage for `.razor` because the compile step can be used to inject information that improve app performance at runtime.</span></span>

<span data-ttu-id="9f81b-614">Un ejemplo clave de estas mejoras implican *los números de secuencia*.</span><span class="sxs-lookup"><span data-stu-id="9f81b-614">A key example of these improvements involve *sequence numbers*.</span></span> <span data-ttu-id="9f81b-615">Los números de secuencia indican al tiempo de ejecución los resultados de los que proceden las líneas de código distintas y ordenadas.</span><span class="sxs-lookup"><span data-stu-id="9f81b-615">Sequence numbers indicate to the runtime which outputs came from which distinct and ordered lines of code.</span></span> <span data-ttu-id="9f81b-616">El motor en tiempo de ejecución utiliza esta información para generar diferencias de árbol eficientes en el tiempo lineal, que es mucho más rápido de lo que suele ser posible para un algoritmo de comparación de árboles generales.</span><span class="sxs-lookup"><span data-stu-id="9f81b-616">The runtime uses this information to generate efficient tree diffs in linear time, which is far faster than is normally possible for a general tree diff algorithm.</span></span>

<span data-ttu-id="9f81b-617">Considere el siguiente archivo de componente de Razor ( *. Razor*):</span><span class="sxs-lookup"><span data-stu-id="9f81b-617">Consider the following Razor component (*.razor*) file:</span></span>

```razor
@if (someFlag)
{
    <text>First</text>
}

Second
```

<span data-ttu-id="9f81b-618">El código anterior se compila en algo similar a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="9f81b-618">The preceding code compiles to something like the following:</span></span>

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

<span data-ttu-id="9f81b-619">Cuando el código se ejecuta por primera vez, si se `true``someFlag`, el generador recibe:</span><span class="sxs-lookup"><span data-stu-id="9f81b-619">When the code executes for the first time, if `someFlag` is `true`, the builder receives:</span></span>

| <span data-ttu-id="9f81b-620">Secuencia</span><span class="sxs-lookup"><span data-stu-id="9f81b-620">Sequence</span></span> | <span data-ttu-id="9f81b-621">Tipo de</span><span class="sxs-lookup"><span data-stu-id="9f81b-621">Type</span></span>      | <span data-ttu-id="9f81b-622">Datos</span><span class="sxs-lookup"><span data-stu-id="9f81b-622">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="9f81b-623">0</span><span class="sxs-lookup"><span data-stu-id="9f81b-623">0</span></span>        | <span data-ttu-id="9f81b-624">Nodo de texto</span><span class="sxs-lookup"><span data-stu-id="9f81b-624">Text node</span></span> | <span data-ttu-id="9f81b-625">First</span><span class="sxs-lookup"><span data-stu-id="9f81b-625">First</span></span>  |
| <span data-ttu-id="9f81b-626">1</span><span class="sxs-lookup"><span data-stu-id="9f81b-626">1</span></span>        | <span data-ttu-id="9f81b-627">Nodo de texto</span><span class="sxs-lookup"><span data-stu-id="9f81b-627">Text node</span></span> | <span data-ttu-id="9f81b-628">Second</span><span class="sxs-lookup"><span data-stu-id="9f81b-628">Second</span></span> |

<span data-ttu-id="9f81b-629">Imagine que `someFlag` se `false`y que el marcado se representará de nuevo.</span><span class="sxs-lookup"><span data-stu-id="9f81b-629">Imagine that `someFlag` becomes `false`, and the markup is rendered again.</span></span> <span data-ttu-id="9f81b-630">Esta vez, el generador recibe:</span><span class="sxs-lookup"><span data-stu-id="9f81b-630">This time, the builder receives:</span></span>

| <span data-ttu-id="9f81b-631">Secuencia</span><span class="sxs-lookup"><span data-stu-id="9f81b-631">Sequence</span></span> | <span data-ttu-id="9f81b-632">Tipo de</span><span class="sxs-lookup"><span data-stu-id="9f81b-632">Type</span></span>       | <span data-ttu-id="9f81b-633">Datos</span><span class="sxs-lookup"><span data-stu-id="9f81b-633">Data</span></span>   |
| :------: | ---------- | :----: |
| <span data-ttu-id="9f81b-634">1</span><span class="sxs-lookup"><span data-stu-id="9f81b-634">1</span></span>        | <span data-ttu-id="9f81b-635">Nodo de texto</span><span class="sxs-lookup"><span data-stu-id="9f81b-635">Text node</span></span>  | <span data-ttu-id="9f81b-636">Second</span><span class="sxs-lookup"><span data-stu-id="9f81b-636">Second</span></span> |

<span data-ttu-id="9f81b-637">Cuando el tiempo de ejecución realiza una comparación, ve que se quitó el elemento en la secuencia `0`, por lo que genera el siguiente *script de edición*trivial:</span><span class="sxs-lookup"><span data-stu-id="9f81b-637">When the runtime performs a diff, it sees that the item at sequence `0` was removed, so it generates the following trivial *edit script*:</span></span>

* <span data-ttu-id="9f81b-638">Quitar el primer nodo de texto.</span><span class="sxs-lookup"><span data-stu-id="9f81b-638">Remove the first text node.</span></span>

#### <a name="what-goes-wrong-if-you-generate-sequence-numbers-programmatically"></a><span data-ttu-id="9f81b-639">Qué sucede si se generan números de secuencia mediante programación</span><span class="sxs-lookup"><span data-stu-id="9f81b-639">What goes wrong if you generate sequence numbers programmatically</span></span>

<span data-ttu-id="9f81b-640">Imagine que escribió la siguiente lógica del generador de árboles de representación:</span><span class="sxs-lookup"><span data-stu-id="9f81b-640">Imagine instead that you wrote the following render tree builder logic:</span></span>

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

<span data-ttu-id="9f81b-641">Ahora, el primer resultado es:</span><span class="sxs-lookup"><span data-stu-id="9f81b-641">Now, the first output is:</span></span>

| <span data-ttu-id="9f81b-642">Secuencia</span><span class="sxs-lookup"><span data-stu-id="9f81b-642">Sequence</span></span> | <span data-ttu-id="9f81b-643">Tipo de</span><span class="sxs-lookup"><span data-stu-id="9f81b-643">Type</span></span>      | <span data-ttu-id="9f81b-644">Datos</span><span class="sxs-lookup"><span data-stu-id="9f81b-644">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="9f81b-645">0</span><span class="sxs-lookup"><span data-stu-id="9f81b-645">0</span></span>        | <span data-ttu-id="9f81b-646">Nodo de texto</span><span class="sxs-lookup"><span data-stu-id="9f81b-646">Text node</span></span> | <span data-ttu-id="9f81b-647">First</span><span class="sxs-lookup"><span data-stu-id="9f81b-647">First</span></span>  |
| <span data-ttu-id="9f81b-648">1</span><span class="sxs-lookup"><span data-stu-id="9f81b-648">1</span></span>        | <span data-ttu-id="9f81b-649">Nodo de texto</span><span class="sxs-lookup"><span data-stu-id="9f81b-649">Text node</span></span> | <span data-ttu-id="9f81b-650">Second</span><span class="sxs-lookup"><span data-stu-id="9f81b-650">Second</span></span> |

<span data-ttu-id="9f81b-651">Este resultado es idéntico al caso anterior, por lo que no existe ningún problema negativo.</span><span class="sxs-lookup"><span data-stu-id="9f81b-651">This outcome is identical to the prior case, so no negative issues exist.</span></span> <span data-ttu-id="9f81b-652">`someFlag` se `false` en la segunda representación y el resultado es:</span><span class="sxs-lookup"><span data-stu-id="9f81b-652">`someFlag` is `false` on the second rendering, and the output is:</span></span>

| <span data-ttu-id="9f81b-653">Secuencia</span><span class="sxs-lookup"><span data-stu-id="9f81b-653">Sequence</span></span> | <span data-ttu-id="9f81b-654">Tipo de</span><span class="sxs-lookup"><span data-stu-id="9f81b-654">Type</span></span>      | <span data-ttu-id="9f81b-655">Datos</span><span class="sxs-lookup"><span data-stu-id="9f81b-655">Data</span></span>   |
| :------: | --------- | ------ |
| <span data-ttu-id="9f81b-656">0</span><span class="sxs-lookup"><span data-stu-id="9f81b-656">0</span></span>        | <span data-ttu-id="9f81b-657">Nodo de texto</span><span class="sxs-lookup"><span data-stu-id="9f81b-657">Text node</span></span> | <span data-ttu-id="9f81b-658">Second</span><span class="sxs-lookup"><span data-stu-id="9f81b-658">Second</span></span> |

<span data-ttu-id="9f81b-659">Esta vez, el algoritmo de comparación ve que se han producido *dos* cambios y el algoritmo genera el siguiente script de edición:</span><span class="sxs-lookup"><span data-stu-id="9f81b-659">This time, the diff algorithm sees that *two* changes have occurred, and the algorithm generates the following edit script:</span></span>

* <span data-ttu-id="9f81b-660">Cambie el valor del primer nodo de texto a `Second`.</span><span class="sxs-lookup"><span data-stu-id="9f81b-660">Change the value of the first text node to `Second`.</span></span>
* <span data-ttu-id="9f81b-661">Quite el segundo nodo de texto.</span><span class="sxs-lookup"><span data-stu-id="9f81b-661">Remove the second text node.</span></span>

<span data-ttu-id="9f81b-662">La generación de los números de secuencia ha perdido toda la información útil sobre el lugar en el que las bifurcaciones de `if/else` y los bucles estaban presentes en el código original.</span><span class="sxs-lookup"><span data-stu-id="9f81b-662">Generating the sequence numbers has lost all the useful information about where the `if/else` branches and loops were present in the original code.</span></span> <span data-ttu-id="9f81b-663">Esto da como resultado una diferencia **dos veces mayor que** antes.</span><span class="sxs-lookup"><span data-stu-id="9f81b-663">This results in a diff **twice as long** as before.</span></span>

<span data-ttu-id="9f81b-664">Este es un ejemplo trivial.</span><span class="sxs-lookup"><span data-stu-id="9f81b-664">This is a trivial example.</span></span> <span data-ttu-id="9f81b-665">En casos más realistas con estructuras complejas y profundamente anidadas, y especialmente con bucles, el costo de rendimiento es más grave.</span><span class="sxs-lookup"><span data-stu-id="9f81b-665">In more realistic cases with complex and deeply nested structures, and especially with loops, the performance cost is more severe.</span></span> <span data-ttu-id="9f81b-666">En lugar de identificar inmediatamente qué bloques o ramas de bucle se han insertado o quitado, el algoritmo de comparación tiene que recurse profundamente en los árboles de representación y, normalmente, compilar scripts de edición mucho más largos porque está informada sobre cómo las estructuras antiguas y nuevas se relacionan entre sí.</span><span class="sxs-lookup"><span data-stu-id="9f81b-666">Instead of immediately identifying which loop blocks or branches have been inserted or removed, the diff algorithm has to recurse deeply into the render trees and usually build far longer edit scripts because it is misinformed about how the old and new structures relate to each other.</span></span>

#### <a name="guidance-and-conclusions"></a><span data-ttu-id="9f81b-667">Instrucciones y conclusiones</span><span class="sxs-lookup"><span data-stu-id="9f81b-667">Guidance and conclusions</span></span>

* <span data-ttu-id="9f81b-668">El rendimiento de la aplicación se ve afectado si los números de secuencia se generan dinámicamente.</span><span class="sxs-lookup"><span data-stu-id="9f81b-668">App performance suffers if sequence numbers are generated dynamically.</span></span>
* <span data-ttu-id="9f81b-669">El marco no puede crear sus propios números de secuencia automáticamente en tiempo de ejecución porque la información necesaria no existe a menos que se Capture en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="9f81b-669">The framework can't create its own sequence numbers automatically at runtime because the necessary information doesn't exist unless it's captured at compile time.</span></span>
* <span data-ttu-id="9f81b-670">No escriba grandes bloques de lógica de `RenderTreeBuilder` implementada de forma manual.</span><span class="sxs-lookup"><span data-stu-id="9f81b-670">Don't write long blocks of manually-implemented `RenderTreeBuilder` logic.</span></span> <span data-ttu-id="9f81b-671">Prefiere `.razor` archivos y permitir que el compilador trate los números de secuencia.</span><span class="sxs-lookup"><span data-stu-id="9f81b-671">Prefer `.razor` files and allow the compiler to deal with the sequence numbers.</span></span> <span data-ttu-id="9f81b-672">Si no puede evitar la lógica de `RenderTreeBuilder` manual, divida bloques largos de código en fragmentos más pequeños encapsulados en `OpenRegion`/`CloseRegion` llamadas.</span><span class="sxs-lookup"><span data-stu-id="9f81b-672">If you're unable to avoid manual `RenderTreeBuilder` logic, split long blocks of code into smaller pieces wrapped in `OpenRegion`/`CloseRegion` calls.</span></span> <span data-ttu-id="9f81b-673">Cada región tiene su propio espacio independiente de números de secuencia, por lo que puede reiniciar desde cero (o cualquier otro número arbitrario) dentro de cada región.</span><span class="sxs-lookup"><span data-stu-id="9f81b-673">Each region has its own separate space of sequence numbers, so you can restart from zero (or any other arbitrary number) inside each region.</span></span>
* <span data-ttu-id="9f81b-674">Si los números de secuencia están codificados, el algoritmo de comparación solo requiere que los números de secuencia aumenten en valor.</span><span class="sxs-lookup"><span data-stu-id="9f81b-674">If sequence numbers are hardcoded, the diff algorithm only requires that sequence numbers increase in value.</span></span> <span data-ttu-id="9f81b-675">El valor inicial y los huecos son irrelevantes.</span><span class="sxs-lookup"><span data-stu-id="9f81b-675">The initial value and gaps are irrelevant.</span></span> <span data-ttu-id="9f81b-676">Una opción legítima es usar el número de línea de código como el número de secuencia, o comenzar a partir de cero y aumentar por unos o cientos (o cualquier intervalo preferido).</span><span class="sxs-lookup"><span data-stu-id="9f81b-676">One legitimate option is to use the code line number as the sequence number, or start from zero and increase by ones or hundreds (or any preferred interval).</span></span> 
* Blazor<span data-ttu-id="9f81b-677"> usa los números de secuencia, mientras que otros marcos de interfaz de usuario de comparación de árboles no los utilizan.</span><span class="sxs-lookup"><span data-stu-id="9f81b-677"> uses sequence numbers, while other tree-diffing UI frameworks don't use them.</span></span> <span data-ttu-id="9f81b-678">La diferenciación es mucho más rápida cuando se utilizan números de secuencia y Blazor tiene la ventaja de un paso de compilación que trata los números de secuencia automáticamente para desarrolladores que crean archivos *. Razor* .</span><span class="sxs-lookup"><span data-stu-id="9f81b-678">Diffing is far faster when sequence numbers are used, and Blazor has the advantage of a compile step that deals with sequence numbers automatically for developers authoring *.razor* files.</span></span>

## <a name="localization"></a><span data-ttu-id="9f81b-679">Localización</span><span class="sxs-lookup"><span data-stu-id="9f81b-679">Localization</span></span>

<span data-ttu-id="9f81b-680">las aplicaciones de Blazor Server se localizan mediante [middleware de localización](xref:fundamentals/localization#localization-middleware).</span><span class="sxs-lookup"><span data-stu-id="9f81b-680">Blazor Server apps are localized using [Localization Middleware](xref:fundamentals/localization#localization-middleware).</span></span> <span data-ttu-id="9f81b-681">El middleware selecciona la referencia cultural adecuada para los usuarios que solicitan recursos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9f81b-681">The middleware selects the appropriate culture for users requesting resources from the app.</span></span>

<span data-ttu-id="9f81b-682">La referencia cultural se puede establecer utilizando uno de los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="9f81b-682">The culture can be set using one of the following approaches:</span></span>

* [<span data-ttu-id="9f81b-683">Cookies</span><span class="sxs-lookup"><span data-stu-id="9f81b-683">Cookies</span></span>](#cookies)
* [<span data-ttu-id="9f81b-684">Proporcionar la interfaz de usuario para elegir la referencia cultural</span><span class="sxs-lookup"><span data-stu-id="9f81b-684">Provide UI to choose the culture</span></span>](#provide-ui-to-choose-the-culture)

<span data-ttu-id="9f81b-685">Para obtener más información y ejemplos, vea <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="9f81b-685">For more information and examples, see <xref:fundamentals/localization>.</span></span>

### <a name="configure-the-linker-for-internationalization-opno-locblazor-webassembly"></a><span data-ttu-id="9f81b-686">Configurar el enlazador para la internacionalización (Blazor webassembly)</span><span class="sxs-lookup"><span data-stu-id="9f81b-686">Configure the linker for internationalization (Blazor WebAssembly)</span></span>

<span data-ttu-id="9f81b-687">De forma predeterminada, la configuración del enlazador de Blazor para aplicaciones WebAssembly de Blazor quita información de internacionalización, excepto para las configuraciones regionales solicitadas de forma explícita.</span><span class="sxs-lookup"><span data-stu-id="9f81b-687">By default, Blazor's linker configuration for Blazor WebAssembly apps strips out internationalization information except for locales explicitly requested.</span></span> <span data-ttu-id="9f81b-688">Para obtener más información e instrucciones sobre cómo controlar el comportamiento del enlazador, vea <xref:host-and-deploy/blazor/configure-linker#configure-the-linker-for-internationalization>.</span><span class="sxs-lookup"><span data-stu-id="9f81b-688">For more information and guidance on controlling the linker's behavior, see <xref:host-and-deploy/blazor/configure-linker#configure-the-linker-for-internationalization>.</span></span>

### <a name="cookies"></a><span data-ttu-id="9f81b-689">Cookies</span><span class="sxs-lookup"><span data-stu-id="9f81b-689">Cookies</span></span>

<span data-ttu-id="9f81b-690">Una cookie de referencia cultural de localización puede conservar la referencia cultural del usuario.</span><span class="sxs-lookup"><span data-stu-id="9f81b-690">A localization culture cookie can persist the user's culture.</span></span> <span data-ttu-id="9f81b-691">La cookie se crea mediante el método `OnGet` de la página host de la aplicación (*pages/host. cshtml. CS*).</span><span class="sxs-lookup"><span data-stu-id="9f81b-691">The cookie is created by the `OnGet` method of the app's host page (*Pages/Host.cshtml.cs*).</span></span> <span data-ttu-id="9f81b-692">El middleware de localización lee la cookie en solicitudes posteriores para establecer la referencia cultural del usuario.</span><span class="sxs-lookup"><span data-stu-id="9f81b-692">The Localization Middleware reads the cookie on subsequent requests to set the user's culture.</span></span> 

<span data-ttu-id="9f81b-693">El uso de una cookie garantiza que la conexión de WebSocket puede propagar correctamente la referencia cultural.</span><span class="sxs-lookup"><span data-stu-id="9f81b-693">Use of a cookie ensures that the WebSocket connection can correctly propagate the culture.</span></span> <span data-ttu-id="9f81b-694">Si los esquemas de localización se basan en la ruta de acceso URL o la cadena de consulta, es posible que el esquema no funcione con WebSockets, por lo que no se puede conservar la referencia cultural.</span><span class="sxs-lookup"><span data-stu-id="9f81b-694">If localization schemes are based on the URL path or query string, the scheme might not be able to work with WebSockets, thus fail to persist the culture.</span></span> <span data-ttu-id="9f81b-695">Por lo tanto, el uso de una cookie de referencia cultural de localización es el enfoque recomendado.</span><span class="sxs-lookup"><span data-stu-id="9f81b-695">Therefore, use of a localization culture cookie is the recommended approach.</span></span>

<span data-ttu-id="9f81b-696">Cualquier técnica se puede usar para asignar una referencia cultural si la referencia cultural se conserva en una cookie de localización.</span><span class="sxs-lookup"><span data-stu-id="9f81b-696">Any technique can be used to assign a culture if the culture is persisted in a localization cookie.</span></span> <span data-ttu-id="9f81b-697">Si la aplicación ya tiene un esquema de localización establecido para ASP.NET Core del lado servidor, siga usando la infraestructura de localización existente de la aplicación y establezca la cookie de la cultura de localización en el esquema de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9f81b-697">If the app already has an established localization scheme for server-side ASP.NET Core, continue to use the app's existing localization infrastructure and set the localization culture cookie within the app's scheme.</span></span>

<span data-ttu-id="9f81b-698">En el ejemplo siguiente se muestra cómo establecer la referencia cultural actual en una cookie que puede ser leída por el middleware de localización.</span><span class="sxs-lookup"><span data-stu-id="9f81b-698">The following example shows how to set the current culture in a cookie that can be read by the Localization Middleware.</span></span> <span data-ttu-id="9f81b-699">Cree un archivo *pages/host. cshtml. CS* con el siguiente contenido en la aplicación de Blazor Server:</span><span class="sxs-lookup"><span data-stu-id="9f81b-699">Create a *Pages/Host.cshtml.cs* file with the following contents in the Blazor Server app:</span></span>

```csharp
public class HostModel : PageModel
{
    public void OnGet()
    {
        HttpContext.Response.Cookies.Append(
            CookieRequestCultureProvider.DefaultCookieName,
            CookieRequestCultureProvider.MakeCookieValue(
                new RequestCulture(
                    CultureInfo.CurrentCulture,
                    CultureInfo.CurrentUICulture)));
    }
}
```

<span data-ttu-id="9f81b-700">La localización se controla en la aplicación:</span><span class="sxs-lookup"><span data-stu-id="9f81b-700">Localization is handled in the app:</span></span>

1. <span data-ttu-id="9f81b-701">El explorador envía una solicitud HTTP inicial a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9f81b-701">The browser sends an initial HTTP request to the app.</span></span>
1. <span data-ttu-id="9f81b-702">El middleware de localización asigna la referencia cultural.</span><span class="sxs-lookup"><span data-stu-id="9f81b-702">The culture is assigned by the Localization Middleware.</span></span>
1. <span data-ttu-id="9f81b-703">El método `OnGet` en *_Host. cshtml. CS* conserva la referencia cultural en una cookie como parte de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="9f81b-703">The `OnGet` method in *_Host.cshtml.cs* persists the culture in a cookie as part of the response.</span></span>
1. <span data-ttu-id="9f81b-704">El explorador abre una conexión WebSocket para crear una sesión interactiva de Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="9f81b-704">The browser opens a WebSocket connection to create an interactive Blazor Server session.</span></span>
1. <span data-ttu-id="9f81b-705">El middleware de localización lee la cookie y asigna la referencia cultural.</span><span class="sxs-lookup"><span data-stu-id="9f81b-705">The Localization Middleware reads the cookie and assigns the culture.</span></span>
1. <span data-ttu-id="9f81b-706">La sesión del servidor de Blazor comienza con la referencia cultural correcta.</span><span class="sxs-lookup"><span data-stu-id="9f81b-706">The Blazor Server session begins with the correct culture.</span></span>

### <a name="provide-ui-to-choose-the-culture"></a><span data-ttu-id="9f81b-707">Proporcionar la interfaz de usuario para elegir la referencia cultural</span><span class="sxs-lookup"><span data-stu-id="9f81b-707">Provide UI to choose the culture</span></span>

<span data-ttu-id="9f81b-708">Para proporcionar una interfaz de usuario que permita a los usuarios seleccionar una referencia cultural, se recomienda un *enfoque basado en redirección* .</span><span class="sxs-lookup"><span data-stu-id="9f81b-708">To provide UI to allow a user to select a culture, a *redirect-based approach* is recommended.</span></span> <span data-ttu-id="9f81b-709">El proceso es similar a lo que ocurre en una aplicación web cuando un usuario intenta acceder a un recurso seguro&mdash;se redirige al usuario a una página de inicio de sesión y, a continuación, se redirige de nuevo al recurso original.</span><span class="sxs-lookup"><span data-stu-id="9f81b-709">The process is similar to what happens in a web app when a user attempts to access a secure resource&mdash;the user is redirected to a sign-in page and then redirected back to the original resource.</span></span> 

<span data-ttu-id="9f81b-710">La aplicación conserva la referencia cultural seleccionada del usuario a través de una redirección a un controlador.</span><span class="sxs-lookup"><span data-stu-id="9f81b-710">The app persists the user's selected culture via a redirect to a controller.</span></span> <span data-ttu-id="9f81b-711">El controlador establece la referencia cultural seleccionada del usuario en una cookie y redirige al usuario de nuevo al URI original.</span><span class="sxs-lookup"><span data-stu-id="9f81b-711">The controller sets the user's selected culture into a cookie and redirects the user back to the original URI.</span></span>

<span data-ttu-id="9f81b-712">Establezca un punto de conexión HTTP en el servidor para establecer la referencia cultural seleccionada del usuario en una cookie y volver a realizar la redirección al URI original:</span><span class="sxs-lookup"><span data-stu-id="9f81b-712">Establish an HTTP endpoint on the server to set the user's selected culture in a cookie and perform the redirect back to the original URI:</span></span>

```csharp
[Route("[controller]/[action]")]
public class CultureController : Controller
{
    public IActionResult SetCulture(string culture, string redirectUri)
    {
        if (culture != null)
        {
            HttpContext.Response.Cookies.Append(
                CookieRequestCultureProvider.DefaultCookieName,
                CookieRequestCultureProvider.MakeCookieValue(
                    new RequestCulture(culture)));
        }

        return LocalRedirect(redirectUri);
    }
}
```

> [!WARNING]
> <span data-ttu-id="9f81b-713">Use el resultado de la acción `LocalRedirect` para evitar ataques de redireccionamiento abierto.</span><span class="sxs-lookup"><span data-stu-id="9f81b-713">Use the `LocalRedirect` action result to prevent open redirect attacks.</span></span> <span data-ttu-id="9f81b-714">Para obtener más información, vea <xref:security/preventing-open-redirects>.</span><span class="sxs-lookup"><span data-stu-id="9f81b-714">For more information, see <xref:security/preventing-open-redirects>.</span></span>

<span data-ttu-id="9f81b-715">El siguiente componente muestra un ejemplo de cómo realizar la redirección inicial cuando el usuario selecciona una referencia cultural:</span><span class="sxs-lookup"><span data-stu-id="9f81b-715">The following component shows an example of how to perform the initial redirection when the user selects a culture:</span></span>

```razor
@inject NavigationManager NavigationManager

<h3>Select your language</h3>

<select @onchange="OnSelected">
    <option>Select...</option>
    <option value="en-US">English</option>
    <option value="fr-FR">Français</option>
</select>

@code {
    private double textNumber;

    private void OnSelected(ChangeEventArgs e)
    {
        var culture = (string)e.Value;
        var uri = new Uri(NavigationManager.Uri())
            .GetComponents(UriComponents.PathAndQuery, UriFormat.Unescaped);
        var query = $"?culture={Uri.EscapeDataString(culture)}&" +
            $"redirectUri={Uri.EscapeDataString(uri)}";

        NavigationManager.NavigateTo("/Culture/SetCulture" + query, forceLoad: true);
    }
}
```

### <a name="use-net-localization-scenarios-in-opno-locblazor-apps"></a><span data-ttu-id="9f81b-716">Uso de escenarios de localización de .NET en Blazor aplicaciones</span><span class="sxs-lookup"><span data-stu-id="9f81b-716">Use .NET localization scenarios in Blazor apps</span></span>

<span data-ttu-id="9f81b-717">Dentro de Blazor aplicaciones, están disponibles los siguientes escenarios de localización y globalización de .NET:</span><span class="sxs-lookup"><span data-stu-id="9f81b-717">Inside Blazor apps, the following .NET localization and globalization scenarios are available:</span></span>

* <span data-ttu-id="9f81b-718">. Sistema de recursos de la red</span><span class="sxs-lookup"><span data-stu-id="9f81b-718">.NET's resources system</span></span>
* <span data-ttu-id="9f81b-719">Formato de fecha y número específico de la referencia cultural</span><span class="sxs-lookup"><span data-stu-id="9f81b-719">Culture-specific number and date formatting</span></span>

<span data-ttu-id="9f81b-720">la funcionalidad de `@bind` de Blazorrealiza la globalización en función de la referencia cultural actual del usuario.</span><span class="sxs-lookup"><span data-stu-id="9f81b-720">Blazor's `@bind` functionality performs globalization based on the user's current culture.</span></span> <span data-ttu-id="9f81b-721">Para obtener más información, vea la sección [enlace de datos](#data-binding) .</span><span class="sxs-lookup"><span data-stu-id="9f81b-721">For more information, see the [Data binding](#data-binding) section.</span></span>

<span data-ttu-id="9f81b-722">Actualmente se admite un conjunto limitado de escenarios de localización de ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="9f81b-722">A limited set of ASP.NET Core's localization scenarios are currently supported:</span></span>

* <span data-ttu-id="9f81b-723">`IStringLocalizer<>` *se admite* en las aplicaciones de Blazor.</span><span class="sxs-lookup"><span data-stu-id="9f81b-723">`IStringLocalizer<>` *is supported* in Blazor apps.</span></span>
* <span data-ttu-id="9f81b-724">la localización de `IHtmlLocalizer<>`, `IViewLocalizer<>`y las anotaciones de datos es ASP.NET Core escenarios MVC y **no se admiten** en Blazor aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="9f81b-724">`IHtmlLocalizer<>`, `IViewLocalizer<>`, and Data Annotations localization are ASP.NET Core MVC scenarios and **not supported** in Blazor apps.</span></span>

<span data-ttu-id="9f81b-725">Para obtener más información, vea <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="9f81b-725">For more information, see <xref:fundamentals/localization>.</span></span>

## <a name="scalable-vector-graphics-svg-images"></a><span data-ttu-id="9f81b-726">Imágenes de gráficos vectoriales escalables (SVG)</span><span class="sxs-lookup"><span data-stu-id="9f81b-726">Scalable Vector Graphics (SVG) images</span></span>

<span data-ttu-id="9f81b-727">Dado que Blazor representa imágenes HTML, compatibles con el explorador, incluidas las imágenes SVG (Scalable Vector Graphics) ( *. svg*), se admiten a través de la etiqueta `<img>`:</span><span class="sxs-lookup"><span data-stu-id="9f81b-727">Since Blazor renders HTML, browser-supported images, including Scalable Vector Graphics (SVG) images (*.svg*), are supported via the `<img>` tag:</span></span>

```html
<img alt="Example image" src="some-image.svg" />
```

<span data-ttu-id="9f81b-728">Del mismo modo, las imágenes SVG se admiten en las reglas CSS de un archivo de hoja de estilos ( *. CSS*):</span><span class="sxs-lookup"><span data-stu-id="9f81b-728">Similarly, SVG images are supported in the CSS rules of a stylesheet file (*.css*):</span></span>

```css
.my-element {
    background-image: url("some-image.svg");
}
```

<span data-ttu-id="9f81b-729">Sin embargo, el marcado SVG en línea no se admite en todos los escenarios.</span><span class="sxs-lookup"><span data-stu-id="9f81b-729">However, inline SVG markup isn't supported in all scenarios.</span></span> <span data-ttu-id="9f81b-730">Si coloca una etiqueta de `<svg>` directamente en un archivo de componente ( *. Razor*), se admite la representación de imágenes básica, pero aún no se admiten muchos escenarios avanzados.</span><span class="sxs-lookup"><span data-stu-id="9f81b-730">If you place an `<svg>` tag directly into a component file (*.razor*), basic image rendering is supported but many advanced scenarios aren't yet supported.</span></span> <span data-ttu-id="9f81b-731">Por ejemplo, `<use>` etiquetas no se respetan actualmente y `@bind` no se pueden usar con algunas etiquetas SVG.</span><span class="sxs-lookup"><span data-stu-id="9f81b-731">For example, `<use>` tags aren't currently respected, and `@bind` can't be used with some SVG tags.</span></span> <span data-ttu-id="9f81b-732">Esperamos abordar estas limitaciones en una versión futura.</span><span class="sxs-lookup"><span data-stu-id="9f81b-732">We expect to address these limitations in a future release.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9f81b-733">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="9f81b-733">Additional resources</span></span>

* <span data-ttu-id="9f81b-734"><xref:security/blazor/server> &ndash; incluye instrucciones sobre la creación de aplicaciones de Blazor Server que deben competir con el agotamiento de recursos.</span><span class="sxs-lookup"><span data-stu-id="9f81b-734"><xref:security/blazor/server> &ndash; Includes guidance on building Blazor Server apps that must contend with resource exhaustion.</span></span>
