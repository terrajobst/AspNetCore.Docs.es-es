---
title: Crear y usar ASP.NET Core componentes de Razor
author: guardrex
description: Obtenga información sobre cómo crear y usar componentes de Razor, incluido cómo enlazar a datos, controlar eventos y administrar ciclos de vida de componentes.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/24/2020
no-loc:
- Blazor
- SignalR
uid: blazor/components
ms.openlocfilehash: d6ba60b20d21636c7f780a80d8fbdb152505a3a3
ms.sourcegitcommit: 0b0e485a8a6dfcc65a7a58b365622b3839f4d624
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2020
ms.locfileid: "76928257"
---
# <a name="create-and-use-aspnet-core-razor-components"></a><span data-ttu-id="64c4e-103">Crear y usar ASP.NET Core componentes de Razor</span><span class="sxs-lookup"><span data-stu-id="64c4e-103">Create and use ASP.NET Core Razor components</span></span>

<span data-ttu-id="64c4e-104">Por [Luke Latham](https://github.com/guardrex) y [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="64c4e-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="64c4e-105">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="64c4e-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="64c4e-106">Las aplicaciones increíbles se compilan con *componentes*.</span><span class="sxs-lookup"><span data-stu-id="64c4e-106">Blazor apps are built using *components*.</span></span> <span data-ttu-id="64c4e-107">Un componente es un fragmento independiente de la interfaz de usuario (IU), como una página, un cuadro de diálogo o un formulario.</span><span class="sxs-lookup"><span data-stu-id="64c4e-107">A component is a self-contained chunk of user interface (UI), such as a page, dialog, or form.</span></span> <span data-ttu-id="64c4e-108">Un componente incluye el marcado HTML y la lógica de procesamiento necesaria para insertar datos o responder a los eventos de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="64c4e-108">A component includes HTML markup and the processing logic required to inject data or respond to UI events.</span></span> <span data-ttu-id="64c4e-109">Los componentes son flexibles y ligeros.</span><span class="sxs-lookup"><span data-stu-id="64c4e-109">Components are flexible and lightweight.</span></span> <span data-ttu-id="64c4e-110">Se pueden anidar, reutilizar y compartir entre proyectos.</span><span class="sxs-lookup"><span data-stu-id="64c4e-110">They can be nested, reused, and shared among projects.</span></span>

## <a name="component-classes"></a><span data-ttu-id="64c4e-111">Clases de componentes</span><span class="sxs-lookup"><span data-stu-id="64c4e-111">Component classes</span></span>

<span data-ttu-id="64c4e-112">Los componentes se implementan en archivos de componentes de [Razor](xref:mvc/views/razor) ( *. Razor*) C# mediante una combinación de y el marcado HTML.</span><span class="sxs-lookup"><span data-stu-id="64c4e-112">Components are implemented in [Razor](xref:mvc/views/razor) component files (*.razor*) using a combination of C# and HTML markup.</span></span> <span data-ttu-id="64c4e-113">Un componente de extraordinariamente se conoce como *componente de Razor*.</span><span class="sxs-lookup"><span data-stu-id="64c4e-113">A component in Blazor is formally referred to as a *Razor component*.</span></span>

<span data-ttu-id="64c4e-114">El nombre de un componente debe empezar con un carácter en mayúsculas.</span><span class="sxs-lookup"><span data-stu-id="64c4e-114">A component's name must start with an uppercase character.</span></span> <span data-ttu-id="64c4e-115">Por ejemplo, *MyCoolComponent. Razor* es válido y *MyCoolComponent. Razor* no es válido.</span><span class="sxs-lookup"><span data-stu-id="64c4e-115">For example, *MyCoolComponent.razor* is valid, and *myCoolComponent.razor* is invalid.</span></span>

<span data-ttu-id="64c4e-116">La interfaz de usuario de un componente se define mediante HTML.</span><span class="sxs-lookup"><span data-stu-id="64c4e-116">The UI for a component is defined using HTML.</span></span> <span data-ttu-id="64c4e-117">La lógica de la representación dinámica (por ejemplo, bucles, instrucciones condicionales, expresiones) se agrega mediante una sintaxis de C# insertada denominada [Razor](xref:mvc/views/razor).</span><span class="sxs-lookup"><span data-stu-id="64c4e-117">Dynamic rendering logic (for example, loops, conditionals, expressions) is added using an embedded C# syntax called [Razor](xref:mvc/views/razor).</span></span> <span data-ttu-id="64c4e-118">Cuando se compila una aplicación, el marcado HTML y C# la lógica de representación se convierten en una clase de componente.</span><span class="sxs-lookup"><span data-stu-id="64c4e-118">When an app is compiled, the HTML markup and C# rendering logic are converted into a component class.</span></span> <span data-ttu-id="64c4e-119">El nombre de la clase generada coincide con el nombre del archivo.</span><span class="sxs-lookup"><span data-stu-id="64c4e-119">The name of the generated class matches the name of the file.</span></span>

<span data-ttu-id="64c4e-120">Los miembros de la clase de componente se definen en un bloque `@code`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-120">Members of the component class are defined in an `@code` block.</span></span> <span data-ttu-id="64c4e-121">En el bloque `@code`, el estado de componente (propiedades, campos) se especifica con métodos para el control de eventos o para definir otra lógica de componentes.</span><span class="sxs-lookup"><span data-stu-id="64c4e-121">In the `@code` block, component state (properties, fields) is specified with methods for event handling or for defining other component logic.</span></span> <span data-ttu-id="64c4e-122">Se permite emplear más de un bloque `@code`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-122">More than one `@code` block is permissible.</span></span>

<span data-ttu-id="64c4e-123">Los miembros de componente se pueden usar como parte de la lógica de representación del C# componente mediante expresiones que comienzan con `@`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-123">Component members can be used as part of the component's rendering logic using C# expressions that start with `@`.</span></span> <span data-ttu-id="64c4e-124">Por ejemplo, un C# campo se representa mediante el prefijo `@` al nombre del campo.</span><span class="sxs-lookup"><span data-stu-id="64c4e-124">For example, a C# field is rendered by prefixing `@` to the field name.</span></span> <span data-ttu-id="64c4e-125">En el siguiente ejemplo se evalúa y representa:</span><span class="sxs-lookup"><span data-stu-id="64c4e-125">The following example evaluates and renders:</span></span>

* <span data-ttu-id="64c4e-126">`_headingFontStyle` al valor de la propiedad CSS para `font-style`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-126">`_headingFontStyle` to the CSS property value for `font-style`.</span></span>
* <span data-ttu-id="64c4e-127">`_headingText` al contenido del elemento `<h1>`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-127">`_headingText` to the content of the `<h1>` element.</span></span>

```razor
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

<span data-ttu-id="64c4e-128">Una vez que el componente se representa inicialmente, el componente regenera su árbol de representación en respuesta a los eventos.</span><span class="sxs-lookup"><span data-stu-id="64c4e-128">After the component is initially rendered, the component regenerates its render tree in response to events.</span></span> <span data-ttu-id="64c4e-129">Después compara el nuevo árbol de representación con el anterior y aplica las modificaciones realizadas en el Document Object Model del explorador (DOM).</span><span class="sxs-lookup"><span data-stu-id="64c4e-129">Blazor then compares the new render tree against the previous one and applies any modifications to the browser's Document Object Model (DOM).</span></span>

<span data-ttu-id="64c4e-130">Los componentes son C# clases normales y se pueden colocar en cualquier parte dentro de un proyecto.</span><span class="sxs-lookup"><span data-stu-id="64c4e-130">Components are ordinary C# classes and can be placed anywhere within a project.</span></span> <span data-ttu-id="64c4e-131">Los componentes que generan páginas web normalmente residen en la carpeta *pages* .</span><span class="sxs-lookup"><span data-stu-id="64c4e-131">Components that produce webpages usually reside in the *Pages* folder.</span></span> <span data-ttu-id="64c4e-132">Los componentes que no son de página se colocan con frecuencia en la carpeta *compartida* o en una carpeta personalizada agregada al proyecto.</span><span class="sxs-lookup"><span data-stu-id="64c4e-132">Non-page components are frequently placed in the *Shared* folder or a custom folder added to the project.</span></span>

<span data-ttu-id="64c4e-133">Normalmente, el espacio de nombres de un componente se deriva del espacio de nombres raíz de la aplicación y la ubicación del componente (carpeta) dentro de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="64c4e-133">Typically, a component's namespace is derived from the app's root namespace and the component's location (folder) within the app.</span></span> <span data-ttu-id="64c4e-134">Si el espacio de nombres raíz de la aplicación es `BlazorApp` y el componente `Counter` reside en la carpeta *páginas* :</span><span class="sxs-lookup"><span data-stu-id="64c4e-134">If the app's root namespace is `BlazorApp` and the `Counter` component resides in the *Pages* folder:</span></span>

* <span data-ttu-id="64c4e-135">El espacio de nombres del componente de `Counter` es `BlazorApp.Pages`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-135">The `Counter` component's namespace is `BlazorApp.Pages`.</span></span>
* <span data-ttu-id="64c4e-136">El nombre de tipo completo del componente es `BlazorApp.Pages.Counter`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-136">The fully qualified type name of the component is `BlazorApp.Pages.Counter`.</span></span>

<span data-ttu-id="64c4e-137">Para obtener más información, vea la sección [importar componentes](#import-components) .</span><span class="sxs-lookup"><span data-stu-id="64c4e-137">For more information, see the [Import components](#import-components) section.</span></span>

<span data-ttu-id="64c4e-138">Para usar una carpeta personalizada, agregue el espacio de nombres de la carpeta personalizada al componente primario o al archivo *_Imports. Razor* de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="64c4e-138">To use a custom folder, add the custom folder's namespace to either the parent component or to the app's *_Imports.razor* file.</span></span> <span data-ttu-id="64c4e-139">Por ejemplo, el siguiente espacio de nombres hace que los componentes de una carpeta *Components* estén disponibles cuando se `BlazorApp`el espacio de nombres raíz de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="64c4e-139">For example, the following namespace makes components in a *Components* folder available when the app's root namespace is `BlazorApp`:</span></span>

```razor
@using BlazorApp.Components
```

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a><span data-ttu-id="64c4e-140">Integración de componentes en aplicaciones de Razor Pages y MVC</span><span class="sxs-lookup"><span data-stu-id="64c4e-140">Integrate components into Razor Pages and MVC apps</span></span>

<span data-ttu-id="64c4e-141">Los componentes de Razor se pueden integrar en aplicaciones de Razor Pages y MVC.</span><span class="sxs-lookup"><span data-stu-id="64c4e-141">Razor components can be integrated into Razor Pages and MVC apps.</span></span> <span data-ttu-id="64c4e-142">Cuando se representa la página o la vista, los componentes se pueden representar al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="64c4e-142">When the page or view is rendered, components can be prerendered at the same time.</span></span>

<span data-ttu-id="64c4e-143">Para preparar una aplicación Razor Pages o MVC para hospedar componentes de Razor, siga las instrucciones de la sección *integración de componentes de Razor en Razor pages y aplicaciones MVC* del artículo <xref:blazor/hosting-models#integrate-razor-components-into-razor-pages-and-mvc-apps>.</span><span class="sxs-lookup"><span data-stu-id="64c4e-143">To prepare a Razor Pages or MVC app to host Razor components, follow the guidance in the *Integrate Razor components into Razor Pages and MVC apps* section of the <xref:blazor/hosting-models#integrate-razor-components-into-razor-pages-and-mvc-apps> article.</span></span>

<span data-ttu-id="64c4e-144">Al usar una carpeta personalizada para contener los componentes de la aplicación, agregue el espacio de nombres que representa la carpeta a la página o vista, o al archivo *_ViewImports. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="64c4e-144">When using a custom folder to hold the app's components, add the namespace representing the folder to either the page/view or to the *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="64c4e-145">Observe el siguiente ejemplo:</span><span class="sxs-lookup"><span data-stu-id="64c4e-145">In the following example:</span></span>

* <span data-ttu-id="64c4e-146">Cambie `MyAppNamespace` al espacio de nombres de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="64c4e-146">Change `MyAppNamespace` to the app's namespace.</span></span>
* <span data-ttu-id="64c4e-147">Si no se usa una carpeta denominada *componentes* para contener los componentes, cambie `Components` a la carpeta donde residen los componentes.</span><span class="sxs-lookup"><span data-stu-id="64c4e-147">If a folder named *Components* isn't used to hold the components, change `Components` to the folder where the components reside.</span></span>

```csharp
@using MyAppNamespace.Components
```

<span data-ttu-id="64c4e-148">El archivo *_ViewImports. cshtml* se encuentra en la carpeta *pages* de una aplicación Razor pages o en la carpeta *views* de una aplicación MVC.</span><span class="sxs-lookup"><span data-stu-id="64c4e-148">The *_ViewImports.cshtml* file is located in the *Pages* folder of a Razor Pages app or the *Views* folder of an MVC app.</span></span>

<span data-ttu-id="64c4e-149">Para representar un componente de una página o vista, use la aplicación auxiliar de etiquetas `Component`:</span><span class="sxs-lookup"><span data-stu-id="64c4e-149">To render a component from a page or view, use the `Component` Tag Helper:</span></span>

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-IncrementAmount="10" />
```

<span data-ttu-id="64c4e-150">El tipo de parámetro debe ser serializable de JSON, lo que normalmente significa que el tipo debe tener un constructor predeterminado y las propiedades configurables.</span><span class="sxs-lookup"><span data-stu-id="64c4e-150">The parameter type must be JSON serializable, which typically means that the type must have a default constructor and settable properties.</span></span> <span data-ttu-id="64c4e-151">Por ejemplo, puede especificar un valor para `IncrementAmount` porque el tipo de `IncrementAmount` es un `int`, que es un tipo primitivo admitido por el serializador JSON.</span><span class="sxs-lookup"><span data-stu-id="64c4e-151">For example, you can specify a value for `IncrementAmount` because the type of `IncrementAmount` is an `int`, which is a primitive type supported by the JSON serializer.</span></span>

<span data-ttu-id="64c4e-152">`RenderMode` configura si el componente:</span><span class="sxs-lookup"><span data-stu-id="64c4e-152">`RenderMode` configures whether the component:</span></span>

* <span data-ttu-id="64c4e-153">Se representa en la página.</span><span class="sxs-lookup"><span data-stu-id="64c4e-153">Is prerendered into the page.</span></span>
* <span data-ttu-id="64c4e-154">Se representa como HTML estático en la página o si incluye la información necesaria para iniciar una aplicación extraordinaria desde el agente de usuario.</span><span class="sxs-lookup"><span data-stu-id="64c4e-154">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| `RenderMode`        | <span data-ttu-id="64c4e-155">Descripción</span><span class="sxs-lookup"><span data-stu-id="64c4e-155">Description</span></span> |
| ------------------- | ----------- |
| `ServerPrerendered` | <span data-ttu-id="64c4e-156">Representa el componente en código HTML estático e incluye un marcador para una aplicación de servidor extraordinaria.</span><span class="sxs-lookup"><span data-stu-id="64c4e-156">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="64c4e-157">Cuando se inicia el agente de usuario, este marcador se usa para arrancar una aplicación increíblemente alta.</span><span class="sxs-lookup"><span data-stu-id="64c4e-157">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Server`            | <span data-ttu-id="64c4e-158">Representa un marcador para una aplicación de servidor extraordinaria.</span><span class="sxs-lookup"><span data-stu-id="64c4e-158">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="64c4e-159">La salida del componente no está incluida.</span><span class="sxs-lookup"><span data-stu-id="64c4e-159">Output from the component isn't included.</span></span> <span data-ttu-id="64c4e-160">Cuando se inicia el agente de usuario, este marcador se usa para arrancar una aplicación increíblemente alta.</span><span class="sxs-lookup"><span data-stu-id="64c4e-160">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| `Static`            | <span data-ttu-id="64c4e-161">Representa el componente en HTML estático.</span><span class="sxs-lookup"><span data-stu-id="64c4e-161">Renders the component into static HTML.</span></span> |

<span data-ttu-id="64c4e-162">Mientras que las páginas y las vistas pueden utilizar componentes, el opuesto no es cierto.</span><span class="sxs-lookup"><span data-stu-id="64c4e-162">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="64c4e-163">Los componentes no pueden usar escenarios específicos de la página y de la vista, como vistas y secciones parciales.</span><span class="sxs-lookup"><span data-stu-id="64c4e-163">Components can't use view- and page-specific scenarios, such as partial views and sections.</span></span> <span data-ttu-id="64c4e-164">Para usar la lógica de la vista parcial en un componente, se debe factorizar la lógica de vista parcial en un componente.</span><span class="sxs-lookup"><span data-stu-id="64c4e-164">To use logic from partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="64c4e-165">No se admite la representación de componentes de servidor desde una página HTML estática.</span><span class="sxs-lookup"><span data-stu-id="64c4e-165">Rendering server components from a static HTML page isn't supported.</span></span>

<span data-ttu-id="64c4e-166">Para obtener más información sobre cómo se representan los componentes, el estado del componente y la aplicación auxiliar de etiquetas `Component`, vea <xref:blazor/hosting-models>.</span><span class="sxs-lookup"><span data-stu-id="64c4e-166">For more information on how components are rendered, component state, and the `Component` Tag Helper, see <xref:blazor/hosting-models>.</span></span>

## <a name="tag-helpers-arent-used-in-components"></a><span data-ttu-id="64c4e-167">Las aplicaciones auxiliares de etiquetas no se usan en los componentes</span><span class="sxs-lookup"><span data-stu-id="64c4e-167">Tag Helpers aren't used in components</span></span>

<span data-ttu-id="64c4e-168">Las [aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro) no se admiten en los componentes de Razor (archivos *. Razor* ).</span><span class="sxs-lookup"><span data-stu-id="64c4e-168">[Tag Helpers](xref:mvc/views/tag-helpers/intro) aren't supported in Razor components (*.razor* files).</span></span> <span data-ttu-id="64c4e-169">Para proporcionar una funcionalidad similar a la aplicación auxiliar de etiquetas, cree un componente con la misma funcionalidad que la aplicación auxiliar de etiquetas y use el componente en su lugar.</span><span class="sxs-lookup"><span data-stu-id="64c4e-169">To provide Tag Helper-like functionality in Blazor, create a component with the same functionality as the Tag Helper and use the component instead.</span></span>

## <a name="use-components"></a><span data-ttu-id="64c4e-170">Uso de componentes</span><span class="sxs-lookup"><span data-stu-id="64c4e-170">Use components</span></span>

<span data-ttu-id="64c4e-171">Los componentes pueden incluir otros componentes declarándolos mediante la sintaxis de elementos HTML.</span><span class="sxs-lookup"><span data-stu-id="64c4e-171">Components can include other components by declaring them using HTML element syntax.</span></span> <span data-ttu-id="64c4e-172">El marcado para utilizar un componente se parece a una etiqueta HTML en la que el nombre de la etiqueta es el tipo de componente.</span><span class="sxs-lookup"><span data-stu-id="64c4e-172">The markup for using a component looks like an HTML tag where the name of the tag is the component type.</span></span>

<span data-ttu-id="64c4e-173">El enlace de atributo distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="64c4e-173">Attribute binding is case sensitive.</span></span> <span data-ttu-id="64c4e-174">Por ejemplo, `@bind` es válido y `@Bind` no es válido.</span><span class="sxs-lookup"><span data-stu-id="64c4e-174">For example, `@bind` is valid, and `@Bind` is invalid.</span></span>

<span data-ttu-id="64c4e-175">El marcado siguiente en *index. Razor* representa una instancia de `HeadingComponent`:</span><span class="sxs-lookup"><span data-stu-id="64c4e-175">The following markup in *Index.razor* renders a `HeadingComponent` instance:</span></span>

```razor
<HeadingComponent />
```

<span data-ttu-id="64c4e-176">*Components/HeadingComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="64c4e-176">*Components/HeadingComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/HeadingComponent.razor)]

<span data-ttu-id="64c4e-177">Si un componente contiene un elemento HTML con una primera letra mayúscula que no coincide con un nombre de componente, se emite una advertencia que indica que el elemento tiene un nombre inesperado.</span><span class="sxs-lookup"><span data-stu-id="64c4e-177">If a component contains an HTML element with an uppercase first letter that doesn't match a component name, a warning is emitted indicating that the element has an unexpected name.</span></span> <span data-ttu-id="64c4e-178">La adición de una instrucción `@using` para el espacio de nombres del componente hace que el componente esté disponible, lo que elimina la advertencia.</span><span class="sxs-lookup"><span data-stu-id="64c4e-178">Adding an `@using` statement for the component's namespace makes the component available, which removes the warning.</span></span>

## <a name="component-parameters"></a><span data-ttu-id="64c4e-179">Parámetros del componente</span><span class="sxs-lookup"><span data-stu-id="64c4e-179">Component parameters</span></span>

<span data-ttu-id="64c4e-180">Los componentes pueden tener *parámetros de componente*, que se definen mediante propiedades públicas en la clase de componente con el atributo `[Parameter]`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-180">Components can have *component parameters*, which are defined using public properties on the component class with the `[Parameter]` attribute.</span></span> <span data-ttu-id="64c4e-181">Use atributos para especificar argumentos para un componente en el marcado.</span><span class="sxs-lookup"><span data-stu-id="64c4e-181">Use attributes to specify arguments for a component in markup.</span></span>

<span data-ttu-id="64c4e-182">*Components/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="64c4e-182">*Components/ChildComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=11-12)]

<span data-ttu-id="64c4e-183">En el ejemplo siguiente de la aplicación de ejemplo, el `ParentComponent` establece el valor de la propiedad `Title` de la `ChildComponent`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-183">In the following example from the sample app, the `ParentComponent` sets the value of the `Title` property of the `ChildComponent`.</span></span>

<span data-ttu-id="64c4e-184">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="64c4e-184">*Pages/ParentComponent.razor*:</span></span>

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

## <a name="child-content"></a><span data-ttu-id="64c4e-185">Contenido secundario</span><span class="sxs-lookup"><span data-stu-id="64c4e-185">Child content</span></span>

<span data-ttu-id="64c4e-186">Los componentes pueden establecer el contenido de otro componente.</span><span class="sxs-lookup"><span data-stu-id="64c4e-186">Components can set the content of another component.</span></span> <span data-ttu-id="64c4e-187">El componente de asignación proporciona el contenido entre las etiquetas que especifican el componente receptor.</span><span class="sxs-lookup"><span data-stu-id="64c4e-187">The assigning component provides the content between the tags that specify the receiving component.</span></span>

<span data-ttu-id="64c4e-188">En el ejemplo siguiente, el `ChildComponent` tiene una propiedad `ChildContent` que representa un `RenderFragment`, que representa un segmento de interfaz de usuario que se va a representar.</span><span class="sxs-lookup"><span data-stu-id="64c4e-188">In the following example, the `ChildComponent` has a `ChildContent` property that represents a `RenderFragment`, which represents a segment of UI to render.</span></span> <span data-ttu-id="64c4e-189">El valor de `ChildContent` se coloca en el marcado del componente en el que se debe representar el contenido.</span><span class="sxs-lookup"><span data-stu-id="64c4e-189">The value of `ChildContent` is positioned in the component's markup where the content should be rendered.</span></span> <span data-ttu-id="64c4e-190">El valor de `ChildContent` se recibe del componente primario y se representa dentro del `panel-body`del panel de arranque.</span><span class="sxs-lookup"><span data-stu-id="64c4e-190">The value of `ChildContent` is received from the parent component and rendered inside the Bootstrap panel's `panel-body`.</span></span>

<span data-ttu-id="64c4e-191">*Components/ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="64c4e-191">*Components/ChildComponent.razor*:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> <span data-ttu-id="64c4e-192">La propiedad que recibe el contenido del `RenderFragment` debe denominarse `ChildContent` por Convención.</span><span class="sxs-lookup"><span data-stu-id="64c4e-192">The property receiving the `RenderFragment` content must be named `ChildContent` by convention.</span></span>

<span data-ttu-id="64c4e-193">El `ParentComponent` de la aplicación de ejemplo puede proporcionar contenido para representar el `ChildComponent` colocando el contenido dentro de las etiquetas de `<ChildComponent>`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-193">The `ParentComponent` in the sample app can provide content for rendering the `ChildComponent` by placing the content inside the `<ChildComponent>` tags.</span></span>

<span data-ttu-id="64c4e-194">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="64c4e-194">*Pages/ParentComponent.razor*:</span></span>

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

## <a name="attribute-splatting-and-arbitrary-parameters"></a><span data-ttu-id="64c4e-195">Atributos expansión y parámetros arbitrarios</span><span class="sxs-lookup"><span data-stu-id="64c4e-195">Attribute splatting and arbitrary parameters</span></span>

<span data-ttu-id="64c4e-196">Los componentes de pueden capturar y representar atributos adicionales además de los parámetros declarados del componente.</span><span class="sxs-lookup"><span data-stu-id="64c4e-196">Components can capture and render additional attributes in addition to the component's declared parameters.</span></span> <span data-ttu-id="64c4e-197">Los atributos adicionales se pueden capturar en un diccionario y, después, *splatted* en un elemento cuando el componente se representa mediante la Directiva de Razor [`@attributes`](xref:mvc/views/razor#attributes) .</span><span class="sxs-lookup"><span data-stu-id="64c4e-197">Additional attributes can be captured in a dictionary and then *splatted* onto an element when the component is rendered using the [`@attributes`](xref:mvc/views/razor#attributes) Razor directive.</span></span> <span data-ttu-id="64c4e-198">Este escenario es útil cuando se define un componente que genera un elemento de marcado que admite diversas personalizaciones.</span><span class="sxs-lookup"><span data-stu-id="64c4e-198">This scenario is useful when defining a component that produces a markup element that supports a variety of customizations.</span></span> <span data-ttu-id="64c4e-199">Por ejemplo, puede resultar tedioso definir atributos por separado para una `<input>` que admita muchos parámetros.</span><span class="sxs-lookup"><span data-stu-id="64c4e-199">For example, it can be tedious to define attributes separately for an `<input>` that supports many parameters.</span></span>

<span data-ttu-id="64c4e-200">En el ejemplo siguiente, el primer elemento `<input>` (`id="useIndividualParams"`) utiliza parámetros de componente individuales, mientras que el segundo elemento `<input>` (`id="useAttributesDict"`) utiliza el atributo expansión:</span><span class="sxs-lookup"><span data-stu-id="64c4e-200">In the following example, the first `<input>` element (`id="useIndividualParams"`) uses individual component parameters, while the second `<input>` element (`id="useAttributesDict"`) uses attribute splatting:</span></span>

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

<span data-ttu-id="64c4e-201">El tipo del parámetro debe implementar `IEnumerable<KeyValuePair<string, object>>` con claves de cadena.</span><span class="sxs-lookup"><span data-stu-id="64c4e-201">The type of the parameter must implement `IEnumerable<KeyValuePair<string, object>>` with string keys.</span></span> <span data-ttu-id="64c4e-202">El uso de `IReadOnlyDictionary<string, object>` también es una opción en este escenario.</span><span class="sxs-lookup"><span data-stu-id="64c4e-202">Using `IReadOnlyDictionary<string, object>` is also an option in this scenario.</span></span>

<span data-ttu-id="64c4e-203">Los elementos `<input>` representados con ambos enfoques son idénticos:</span><span class="sxs-lookup"><span data-stu-id="64c4e-203">The rendered `<input>` elements using both approaches is identical:</span></span>

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

<span data-ttu-id="64c4e-204">Para aceptar atributos arbitrarios, defina un parámetro de componente mediante el atributo `[Parameter]` con la propiedad `CaptureUnmatchedValues` establecida en `true`:</span><span class="sxs-lookup"><span data-stu-id="64c4e-204">To accept arbitrary attributes, define a component parameter using the `[Parameter]` attribute with the `CaptureUnmatchedValues` property set to `true`:</span></span>

```razor
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

<span data-ttu-id="64c4e-205">La propiedad `CaptureUnmatchedValues` en `[Parameter]` permite que el parámetro coincida con todos los atributos que no coinciden con ningún otro parámetro.</span><span class="sxs-lookup"><span data-stu-id="64c4e-205">The `CaptureUnmatchedValues` property on `[Parameter]` allows the parameter to match all attributes that don't match any other parameter.</span></span> <span data-ttu-id="64c4e-206">Un componente solo puede definir un parámetro con `CaptureUnmatchedValues`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-206">A component can only define a single parameter with `CaptureUnmatchedValues`.</span></span> <span data-ttu-id="64c4e-207">El tipo de propiedad que se usa con `CaptureUnmatchedValues` se debe poder asignar desde `Dictionary<string, object>` con claves de cadena.</span><span class="sxs-lookup"><span data-stu-id="64c4e-207">The property type used with `CaptureUnmatchedValues` must be assignable from `Dictionary<string, object>` with string keys.</span></span> <span data-ttu-id="64c4e-208">`IEnumerable<KeyValuePair<string, object>>` o `IReadOnlyDictionary<string, object>` también son opciones en este escenario.</span><span class="sxs-lookup"><span data-stu-id="64c4e-208">`IEnumerable<KeyValuePair<string, object>>` or `IReadOnlyDictionary<string, object>` are also options in this scenario.</span></span>

<span data-ttu-id="64c4e-209">La posición de `@attributes` relativa a la posición de los atributos de elemento es importante.</span><span class="sxs-lookup"><span data-stu-id="64c4e-209">The position of `@attributes` relative to the position of element attributes is important.</span></span> <span data-ttu-id="64c4e-210">Cuando `@attributes` se splatted en el elemento, los atributos se procesan de derecha a izquierda (último a primero).</span><span class="sxs-lookup"><span data-stu-id="64c4e-210">When `@attributes` are splatted on the element, the attributes are processed from right to left (last to first).</span></span> <span data-ttu-id="64c4e-211">Considere el siguiente ejemplo de un componente que utiliza un componente de `Child`:</span><span class="sxs-lookup"><span data-stu-id="64c4e-211">Consider the following example of a component that consumes a `Child` component:</span></span>

<span data-ttu-id="64c4e-212">*ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="64c4e-212">*ParentComponent.razor*:</span></span>

```razor
<ChildComponent extra="10" />
```

<span data-ttu-id="64c4e-213">*ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="64c4e-213">*ChildComponent.razor*:</span></span>

```razor
<div @attributes="AdditionalAttributes" extra="5" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="64c4e-214">El atributo `extra` del componente de `Child` se establece a la derecha de `@attributes`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-214">The `Child` component's `extra` attribute is set to the right of `@attributes`.</span></span> <span data-ttu-id="64c4e-215">El `<div>` representado del componente `Parent` contiene `extra="5"` cuando se pasa a través del atributo adicional, ya que los atributos se procesan de derecha a izquierda (último a primero):</span><span class="sxs-lookup"><span data-stu-id="64c4e-215">The `Parent` component's rendered `<div>` contains `extra="5"` when passed through the additional attribute because the attributes are processed right to left (last to first):</span></span>

```html
<div extra="5" />
```

<span data-ttu-id="64c4e-216">En el ejemplo siguiente, el orden de `extra` y `@attributes` se invierte en el `<div>`del componente `Child`:</span><span class="sxs-lookup"><span data-stu-id="64c4e-216">In the following example, the order of `extra` and `@attributes` is reversed in the `Child` component's `<div>`:</span></span>

<span data-ttu-id="64c4e-217">*ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="64c4e-217">*ParentComponent.razor*:</span></span>

```razor
<ChildComponent extra="10" />
```

<span data-ttu-id="64c4e-218">*ChildComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="64c4e-218">*ChildComponent.razor*:</span></span>

```razor
<div extra="5" @attributes="AdditionalAttributes" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

<span data-ttu-id="64c4e-219">El `<div>` representado en el componente `Parent` contiene `extra="10"` cuando se pasa a través del atributo adicional:</span><span class="sxs-lookup"><span data-stu-id="64c4e-219">The rendered `<div>` in the `Parent` component contains `extra="10"` when passed through the additional attribute:</span></span>

```html
<div extra="10" />
```

## <a name="data-binding"></a><span data-ttu-id="64c4e-220">Enlace de datos</span><span class="sxs-lookup"><span data-stu-id="64c4e-220">Data binding</span></span>

<span data-ttu-id="64c4e-221">El enlace de datos tanto a componentes como a elementos DOM se realiza con el [`@bind`](xref:mvc/views/razor#bind) atributo.</span><span class="sxs-lookup"><span data-stu-id="64c4e-221">Data binding to both components and DOM elements is accomplished with the [`@bind`](xref:mvc/views/razor#bind) attribute.</span></span> <span data-ttu-id="64c4e-222">En el ejemplo siguiente se enlaza una propiedad `CurrentValue` al valor del cuadro de texto:</span><span class="sxs-lookup"><span data-stu-id="64c4e-222">The following example binds a `CurrentValue` property to the text box's value:</span></span>

```razor
<input @bind="CurrentValue" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="64c4e-223">Cuando el cuadro de texto pierde el foco, se actualiza el valor de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="64c4e-223">When the text box loses focus, the property's value is updated.</span></span>

<span data-ttu-id="64c4e-224">El cuadro de texto se actualiza en la interfaz de usuario solo cuando se representa el componente, no en respuesta a cambiar el valor de la propiedad.</span><span class="sxs-lookup"><span data-stu-id="64c4e-224">The text box is updated in the UI only when the component is rendered, not in response to changing the property's value.</span></span> <span data-ttu-id="64c4e-225">Puesto que los componentes se representan por sí solos después de que se ejecute el código del controlador de eventos, las actualizaciones de propiedades *normalmente* se reflejan en la interfaz de usuario inmediatamente después de desencadenarse un controlador de eventos.</span><span class="sxs-lookup"><span data-stu-id="64c4e-225">Since components render themselves after event handler code executes, property updates are *usually* reflected in the UI immediately after an event handler is triggered.</span></span>

<span data-ttu-id="64c4e-226">El uso de `@bind` con la propiedad `CurrentValue` (`<input @bind="CurrentValue" />`) es esencialmente equivalente a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="64c4e-226">Using `@bind` with the `CurrentValue` property (`<input @bind="CurrentValue" />`) is essentially equivalent to the following:</span></span>

```razor
<input value="@CurrentValue"
    @onchange="@((ChangeEventArgs __e) => CurrentValue = 
        __e.Value.ToString())" />
        
@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="64c4e-227">Cuando se representa el componente, el `value` del elemento de entrada procede de la propiedad `CurrentValue`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-227">When the component is rendered, the `value` of the input element comes from the `CurrentValue` property.</span></span> <span data-ttu-id="64c4e-228">Cuando el usuario escribe en el cuadro de texto y cambia el foco del elemento, se desencadena el evento `onchange` y la propiedad `CurrentValue` se establece en el valor modificado.</span><span class="sxs-lookup"><span data-stu-id="64c4e-228">When the user types in the text box and changes element focus, the `onchange` event is fired and the `CurrentValue` property is set to the changed value.</span></span> <span data-ttu-id="64c4e-229">En realidad, la generación de código es más compleja porque `@bind` administra los casos en los que se realizan las conversiones de tipos.</span><span class="sxs-lookup"><span data-stu-id="64c4e-229">In reality, the code generation is more complex because `@bind` handles cases where type conversions are performed.</span></span> <span data-ttu-id="64c4e-230">En principio, `@bind` asocia el valor actual de una expresión con un atributo `value` y controla los cambios mediante el controlador registrado.</span><span class="sxs-lookup"><span data-stu-id="64c4e-230">In principle, `@bind` associates the current value of an expression with a `value` attribute and handles changes using the registered handler.</span></span>

<span data-ttu-id="64c4e-231">Además de controlar los eventos de `onchange` con sintaxis de `@bind`, se puede enlazar una propiedad o un campo mediante otros eventos mediante la especificación de un atributo de [`@bind-value`](xref:mvc/views/razor#bind) con un parámetro de `event` ([`@bind-value:event`](xref:mvc/views/razor#bind)).</span><span class="sxs-lookup"><span data-stu-id="64c4e-231">In addition to handling `onchange` events with `@bind` syntax, a property or field can be bound using other events by specifying an [`@bind-value`](xref:mvc/views/razor#bind) attribute with an `event` parameter ([`@bind-value:event`](xref:mvc/views/razor#bind)).</span></span> <span data-ttu-id="64c4e-232">En el siguiente ejemplo se enlaza la propiedad `CurrentValue` del evento `oninput`:</span><span class="sxs-lookup"><span data-stu-id="64c4e-232">The following example binds the `CurrentValue` property for the `oninput` event:</span></span>

```razor
<input @bind-value="CurrentValue" @bind-value:event="oninput" />

@code {
    private string CurrentValue { get; set; }
}
```

<span data-ttu-id="64c4e-233">A diferencia de `onchange`, que se activa cuando el elemento pierde el foco, `oninput` se desencadena cuando cambia el valor del cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="64c4e-233">Unlike `onchange`, which fires when the element loses focus, `oninput` fires when the value of the text box changes.</span></span>

<span data-ttu-id="64c4e-234">`@bind-value` en el ejemplo anterior enlaza:</span><span class="sxs-lookup"><span data-stu-id="64c4e-234">`@bind-value` in the preceding example binds:</span></span>

* <span data-ttu-id="64c4e-235">Expresión especificada (`CurrentValue`) en el atributo de `value` del elemento.</span><span class="sxs-lookup"><span data-stu-id="64c4e-235">The specified expression (`CurrentValue`) to the element's `value` attribute.</span></span>
* <span data-ttu-id="64c4e-236">Delegado de evento de cambio para el evento especificado por `@bind-value:event`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-236">A change event delegate to the event specified by `@bind-value:event`.</span></span>

<span data-ttu-id="64c4e-237">**Valores no analizables**</span><span class="sxs-lookup"><span data-stu-id="64c4e-237">**Unparsable values**</span></span>

<span data-ttu-id="64c4e-238">Cuando un usuario proporciona un valor que no se pueda analizar a un elemento DataBound, el valor no analizable se revierte automáticamente a su valor anterior cuando se desencadena el evento de enlace.</span><span class="sxs-lookup"><span data-stu-id="64c4e-238">When a user provides an unparsable value to a databound element, the unparsable value is automatically reverted to its previous value when the bind event is triggered.</span></span>

<span data-ttu-id="64c4e-239">Considere el siguiente escenario:</span><span class="sxs-lookup"><span data-stu-id="64c4e-239">Consider the following scenario:</span></span>

* <span data-ttu-id="64c4e-240">Un elemento `<input>` se enlaza a un tipo `int` con un valor inicial de `123`:</span><span class="sxs-lookup"><span data-stu-id="64c4e-240">An `<input>` element is bound to an `int` type with an initial value of `123`:</span></span>

  ```razor
  <input @bind="MyProperty" />

  @code {
      [Parameter]
      public int MyProperty { get; set; } = 123;
  }
  ```
* <span data-ttu-id="64c4e-241">El usuario actualiza el valor del elemento a `123.45` en la página y cambia el foco del elemento.</span><span class="sxs-lookup"><span data-stu-id="64c4e-241">The user updates the value of the element to `123.45` in the page and changes the element focus.</span></span>

<span data-ttu-id="64c4e-242">En el escenario anterior, el valor del elemento se revierte a `123`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-242">In the preceding scenario, the element's value is reverted to `123`.</span></span> <span data-ttu-id="64c4e-243">Cuando el valor `123.45` se rechaza en favor del valor original de `123`, el usuario entiende que no se aceptó su valor.</span><span class="sxs-lookup"><span data-stu-id="64c4e-243">When the value `123.45` is rejected in favor of the original value of `123`, the user understands that their value wasn't accepted.</span></span>

<span data-ttu-id="64c4e-244">De forma predeterminada, el enlace se aplica al evento `onchange` del elemento (`@bind="{PROPERTY OR FIELD}"`).</span><span class="sxs-lookup"><span data-stu-id="64c4e-244">By default, binding applies to the element's `onchange` event (`@bind="{PROPERTY OR FIELD}"`).</span></span> <span data-ttu-id="64c4e-245">Use `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` para establecer un evento diferente.</span><span class="sxs-lookup"><span data-stu-id="64c4e-245">Use `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` to set a different event.</span></span> <span data-ttu-id="64c4e-246">En el caso del evento `oninput` (`@bind-value:event="oninput"`), la reversión se produce después de cualquier pulsación de tecla que introduzca un valor no analizable.</span><span class="sxs-lookup"><span data-stu-id="64c4e-246">For the `oninput` event (`@bind-value:event="oninput"`), the reversion occurs after any keystroke that introduces an unparsable value.</span></span> <span data-ttu-id="64c4e-247">Cuando el destino es el evento `oninput` con un tipo enlazado a `int`, se impide que un usuario escriba un carácter `.`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-247">When targeting the `oninput` event with an `int`-bound type, a user is prevented from typing a `.` character.</span></span> <span data-ttu-id="64c4e-248">Se quita inmediatamente un carácter `.`, por lo que el usuario recibe comentarios inmediatos que solo se permiten números enteros.</span><span class="sxs-lookup"><span data-stu-id="64c4e-248">A `.` character is immediately removed, so the user receives immediate feedback that only whole numbers are permitted.</span></span> <span data-ttu-id="64c4e-249">Hay escenarios en los que la reversión del valor del evento `oninput` no es ideal, por ejemplo, cuando se debe permitir que el usuario borre un valor `<input>` que no se puede analizar.</span><span class="sxs-lookup"><span data-stu-id="64c4e-249">There are scenarios where reverting the value on the `oninput` event isn't ideal, such as when the user should be allowed to clear an unparsable `<input>` value.</span></span> <span data-ttu-id="64c4e-250">Las alternativas incluyen:</span><span class="sxs-lookup"><span data-stu-id="64c4e-250">Alternatives include:</span></span>

* <span data-ttu-id="64c4e-251">No utilice el evento `oninput`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-251">Don't use the `oninput` event.</span></span> <span data-ttu-id="64c4e-252">Use el evento `onchange` predeterminado (`@bind="{PROPERTY OR FIELD}"`), donde no se revierte un valor no válido hasta que el elemento pierde el foco.</span><span class="sxs-lookup"><span data-stu-id="64c4e-252">Use the default `onchange` event (`@bind="{PROPERTY OR FIELD}"`), where an invalid value isn't reverted until the element loses focus.</span></span>
* <span data-ttu-id="64c4e-253">Enlazar a un tipo que acepta valores NULL, como `int?` o `string`, y proporcionar una lógica personalizada para controlar las entradas no válidas.</span><span class="sxs-lookup"><span data-stu-id="64c4e-253">Bind to a nullable type, such as `int?` or `string`, and provide custom logic to handle invalid entries.</span></span>
* <span data-ttu-id="64c4e-254">Use un [componente de validación de formulario](xref:blazor/forms-validation), como `InputNumber` o `InputDate`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-254">Use a [form validation component](xref:blazor/forms-validation), such as `InputNumber` or `InputDate`.</span></span> <span data-ttu-id="64c4e-255">Los componentes de validación de formularios tienen compatibilidad integrada para administrar entradas no válidas.</span><span class="sxs-lookup"><span data-stu-id="64c4e-255">Form validation components have built-in support to manage invalid inputs.</span></span> <span data-ttu-id="64c4e-256">Componentes de validación de formularios:</span><span class="sxs-lookup"><span data-stu-id="64c4e-256">Form validation components:</span></span>
  * <span data-ttu-id="64c4e-257">Permite que el usuario proporcione entradas no válidas y reciba errores de validación en la `EditContext`asociada.</span><span class="sxs-lookup"><span data-stu-id="64c4e-257">Permit the user to provide invalid input and receive validation errors on the associated `EditContext`.</span></span>
  * <span data-ttu-id="64c4e-258">Mostrar errores de validación en la interfaz de usuario sin interferir con el usuario al escribir datos de WebForm adicionales.</span><span class="sxs-lookup"><span data-stu-id="64c4e-258">Display validation errors in the UI without interfering with the user entering additional webform data.</span></span>

<span data-ttu-id="64c4e-259">**Globalización**</span><span class="sxs-lookup"><span data-stu-id="64c4e-259">**Globalization**</span></span>

<span data-ttu-id="64c4e-260">`@bind` se da formato a los valores para mostrarlos y analizarlos con las reglas de la referencia cultural actual.</span><span class="sxs-lookup"><span data-stu-id="64c4e-260">`@bind` values are formatted for display and parsed using the current culture's rules.</span></span>

<span data-ttu-id="64c4e-261">Se puede tener acceso a la referencia cultural actual desde la propiedad <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="64c4e-261">The current culture can be accessed from the <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName> property.</span></span>

<span data-ttu-id="64c4e-262">[CultureInfo. InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) se usa para los siguientes tipos de campo (`<input type="{TYPE}" />`):</span><span class="sxs-lookup"><span data-stu-id="64c4e-262">[CultureInfo.InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) is used for the following field types (`<input type="{TYPE}" />`):</span></span>

* `date`
* `number`

<span data-ttu-id="64c4e-263">Los tipos de campo anteriores:</span><span class="sxs-lookup"><span data-stu-id="64c4e-263">The preceding field types:</span></span>

* <span data-ttu-id="64c4e-264">Se muestran con sus reglas de formato basadas en explorador adecuadas.</span><span class="sxs-lookup"><span data-stu-id="64c4e-264">Are displayed using their appropriate browser-based formatting rules.</span></span>
* <span data-ttu-id="64c4e-265">No puede contener texto de forma libre.</span><span class="sxs-lookup"><span data-stu-id="64c4e-265">Can't contain free-form text.</span></span>
* <span data-ttu-id="64c4e-266">Proporcionar características de interacción con el usuario en función de la implementación del explorador.</span><span class="sxs-lookup"><span data-stu-id="64c4e-266">Provide user interaction characteristics based on the browser's implementation.</span></span>

<span data-ttu-id="64c4e-267">Los siguientes tipos de campo tienen requisitos de formato específicos y no se admiten en estos momentos porque no son compatibles con todos los exploradores principales:</span><span class="sxs-lookup"><span data-stu-id="64c4e-267">The following field types have specific formatting requirements and aren't currently supported by Blazor because they aren't supported by all major browsers:</span></span>

* `datetime-local`
* `month`
* `week`

<span data-ttu-id="64c4e-268">`@bind` admite el parámetro `@bind:culture` para proporcionar un <xref:System.Globalization.CultureInfo?displayProperty=fullName> para analizar y dar formato a un valor.</span><span class="sxs-lookup"><span data-stu-id="64c4e-268">`@bind` supports the `@bind:culture` parameter to provide a <xref:System.Globalization.CultureInfo?displayProperty=fullName> for parsing and formatting a value.</span></span> <span data-ttu-id="64c4e-269">No se recomienda especificar una referencia cultural al usar los tipos de campo `date` y `number`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-269">Specifying a culture isn't recommended when using the `date` and `number` field types.</span></span> <span data-ttu-id="64c4e-270">`date` y `number` tienen compatibilidad más ligera que proporciona la referencia cultural necesaria.</span><span class="sxs-lookup"><span data-stu-id="64c4e-270">`date` and `number` have built-in Blazor support that provides the required culture.</span></span>

<span data-ttu-id="64c4e-271">Para obtener información sobre cómo establecer la referencia cultural del usuario, consulte la sección [Localización](#localization).</span><span class="sxs-lookup"><span data-stu-id="64c4e-271">For information on how to set the user's culture, see the [Localization](#localization) section.</span></span>

<span data-ttu-id="64c4e-272">**Cadenas de formato**</span><span class="sxs-lookup"><span data-stu-id="64c4e-272">**Format strings**</span></span>

<span data-ttu-id="64c4e-273">El enlace de datos funciona con cadenas de formato <xref:System.DateTime> mediante [`@bind:format`](xref:mvc/views/razor#bind).</span><span class="sxs-lookup"><span data-stu-id="64c4e-273">Data binding works with <xref:System.DateTime> format strings using [`@bind:format`](xref:mvc/views/razor#bind).</span></span> <span data-ttu-id="64c4e-274">Otras expresiones de formato, como los formatos de moneda o número, no están disponibles en este momento.</span><span class="sxs-lookup"><span data-stu-id="64c4e-274">Other format expressions, such as currency or number formats, aren't available at this time.</span></span>

```razor
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

<span data-ttu-id="64c4e-275">En el código anterior, el tipo de campo del elemento de `<input>` (`type`) tiene como valor predeterminado `text`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-275">In the preceding code, the `<input>` element's field type (`type`) defaults to `text`.</span></span> <span data-ttu-id="64c4e-276">`@bind:format` es compatible con el enlace de los siguientes tipos de .NET:</span><span class="sxs-lookup"><span data-stu-id="64c4e-276">`@bind:format` is supported for binding the following .NET types:</span></span>

* <xref:System.DateTime?displayProperty=fullName>
* <span data-ttu-id="64c4e-277"><xref:System.DateTime?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="64c4e-277"><xref:System.DateTime?displayProperty=fullName>?</span></span>
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <span data-ttu-id="64c4e-278"><xref:System.DateTimeOffset?displayProperty=fullName>?</span><span class="sxs-lookup"><span data-stu-id="64c4e-278"><xref:System.DateTimeOffset?displayProperty=fullName>?</span></span>

<span data-ttu-id="64c4e-279">El atributo `@bind:format` especifica el formato de fecha que se va a aplicar al `value` del elemento `<input>`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-279">The `@bind:format` attribute specifies the date format to apply to the `value` of the `<input>` element.</span></span> <span data-ttu-id="64c4e-280">El formato también se usa para analizar el valor cuando se produce un evento `onchange`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-280">The format is also used to parse the value when an `onchange` event occurs.</span></span>

<span data-ttu-id="64c4e-281">No se recomienda especificar un formato para el tipo de campo `date` porque el increíble tiene compatibilidad integrada para dar formato a las fechas.</span><span class="sxs-lookup"><span data-stu-id="64c4e-281">Specifying a format for the `date` field type isn't recommended because Blazor has built-in support to format dates.</span></span> <span data-ttu-id="64c4e-282">A pesar de la recomendación, use solo el formato de fecha `yyyy-MM-dd` para que el enlace funcione correctamente si se proporciona un formato con el tipo de campo `date`:</span><span class="sxs-lookup"><span data-stu-id="64c4e-282">In spite of the recommendation, only use the `yyyy-MM-dd` date format for binding to work correctly if a format is supplied with the `date` field type:</span></span>

```razor
<input type="date" @bind="StartDate" @bind:format="yyyy-MM-dd">
```

<span data-ttu-id="64c4e-283">**Parámetros de componente**</span><span class="sxs-lookup"><span data-stu-id="64c4e-283">**Component parameters**</span></span>

<span data-ttu-id="64c4e-284">El enlace reconoce los parámetros de componente, donde `@bind-{property}` puede enlazar un valor de propiedad entre los componentes.</span><span class="sxs-lookup"><span data-stu-id="64c4e-284">Binding recognizes component parameters, where `@bind-{property}` can bind a property value across components.</span></span>

<span data-ttu-id="64c4e-285">El siguiente componente secundario (`ChildComponent`) tiene un parámetro de componente `Year` y una devolución de llamada `YearChanged`:</span><span class="sxs-lookup"><span data-stu-id="64c4e-285">The following child component (`ChildComponent`) has a `Year` component parameter and `YearChanged` callback:</span></span>

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

<span data-ttu-id="64c4e-286">`EventCallback<T>` se explica en la sección [EventCallback](#eventcallback) .</span><span class="sxs-lookup"><span data-stu-id="64c4e-286">`EventCallback<T>` is explained in the [EventCallback](#eventcallback) section.</span></span>

<span data-ttu-id="64c4e-287">El siguiente componente primario utiliza `ChildComponent` y enlaza el parámetro `ParentYear` desde el elemento primario al parámetro `Year` en el componente secundario:</span><span class="sxs-lookup"><span data-stu-id="64c4e-287">The following parent component uses `ChildComponent` and binds the `ParentYear` parameter from the parent to the `Year` parameter on the child component:</span></span>

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

<span data-ttu-id="64c4e-288">Al cargar el `ParentComponent` se produce el marcado siguiente:</span><span class="sxs-lookup"><span data-stu-id="64c4e-288">Loading the `ParentComponent` produces the following markup:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

<span data-ttu-id="64c4e-289">Si el valor de la propiedad `ParentYear` se cambia seleccionando el botón en el `ParentComponent`, se actualiza la propiedad `Year` del `ChildComponent`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-289">If the value of the `ParentYear` property is changed by selecting the button in the `ParentComponent`, the `Year` property of the `ChildComponent` is updated.</span></span> <span data-ttu-id="64c4e-290">El nuevo valor de `Year` se representa en la interfaz de usuario cuando se representa el `ParentComponent`:</span><span class="sxs-lookup"><span data-stu-id="64c4e-290">The new value of `Year` is rendered in the UI when the `ParentComponent` is rerendered:</span></span>

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

<span data-ttu-id="64c4e-291">El parámetro `Year` es enlazable porque tiene un evento Companion `YearChanged` que coincide con el tipo del parámetro `Year`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-291">The `Year` parameter is bindable because it has a companion `YearChanged` event that matches the type of the `Year` parameter.</span></span>

<span data-ttu-id="64c4e-292">Por Convención, `<ChildComponent @bind-Year="ParentYear" />` es esencialmente equivalente a escribir:</span><span class="sxs-lookup"><span data-stu-id="64c4e-292">By convention, `<ChildComponent @bind-Year="ParentYear" />` is essentially equivalent to writing:</span></span>

```razor
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

<span data-ttu-id="64c4e-293">En general, una propiedad se puede enlazar a un controlador de eventos correspondiente mediante `@bind-property:event` atributo.</span><span class="sxs-lookup"><span data-stu-id="64c4e-293">In general, a property can be bound to a corresponding event handler using `@bind-property:event` attribute.</span></span> <span data-ttu-id="64c4e-294">Por ejemplo, la propiedad `MyProp` se puede enlazar a `MyEventHandler` mediante los dos atributos siguientes:</span><span class="sxs-lookup"><span data-stu-id="64c4e-294">For example, the property `MyProp` can be bound to `MyEventHandler` using the following two attributes:</span></span>

```razor
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

<span data-ttu-id="64c4e-295">**Botones de radio**</span><span class="sxs-lookup"><span data-stu-id="64c4e-295">**Radio buttons**</span></span>

<span data-ttu-id="64c4e-296">Para obtener información sobre cómo enlazar a botones de radio en un formulario, vea <xref:blazor/forms-validation#work-with-radio-buttons>.</span><span class="sxs-lookup"><span data-stu-id="64c4e-296">For information on binding to radio buttons in a form, see <xref:blazor/forms-validation#work-with-radio-buttons>.</span></span>

## <a name="event-handling"></a><span data-ttu-id="64c4e-297">Control de eventos</span><span class="sxs-lookup"><span data-stu-id="64c4e-297">Event handling</span></span>

<span data-ttu-id="64c4e-298">Los componentes de Razor proporcionan características de control de eventos.</span><span class="sxs-lookup"><span data-stu-id="64c4e-298">Razor components provide event handling features.</span></span> <span data-ttu-id="64c4e-299">Para un atributo de elemento HTML denominado `on{EVENT}` (por ejemplo, `onclick` y `onsubmit`) con un valor de tipo delegado, los componentes de Razor tratan el valor del atributo como un controlador de eventos.</span><span class="sxs-lookup"><span data-stu-id="64c4e-299">For an HTML element attribute named `on{EVENT}` (for example, `onclick` and `onsubmit`) with a delegate-typed value, Razor components treats the attribute's value as an event handler.</span></span> <span data-ttu-id="64c4e-300">Siempre se da formato al nombre del atributo [`@on{EVENT}`](xref:mvc/views/razor#onevent).</span><span class="sxs-lookup"><span data-stu-id="64c4e-300">The attribute's name is always formatted [`@on{EVENT}`](xref:mvc/views/razor#onevent).</span></span>

<span data-ttu-id="64c4e-301">El código siguiente llama al método `UpdateHeading` cuando el botón está seleccionado en la interfaz de usuario:</span><span class="sxs-lookup"><span data-stu-id="64c4e-301">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

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

<span data-ttu-id="64c4e-302">El código siguiente llama al método `CheckChanged` cuando se cambia la casilla en la interfaz de usuario:</span><span class="sxs-lookup"><span data-stu-id="64c4e-302">The following code calls the `CheckChanged` method when the check box is changed in the UI:</span></span>

```razor
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

<span data-ttu-id="64c4e-303">Los controladores de eventos también pueden ser asincrónicos y devolver un <xref:System.Threading.Tasks.Task>.</span><span class="sxs-lookup"><span data-stu-id="64c4e-303">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="64c4e-304">No es necesario llamar a [StateHasChanged](xref:blazor/lifecycle#state-changes)manualmente.</span><span class="sxs-lookup"><span data-stu-id="64c4e-304">There's no need to manually call [StateHasChanged](xref:blazor/lifecycle#state-changes).</span></span> <span data-ttu-id="64c4e-305">Las excepciones se registran cuando se producen.</span><span class="sxs-lookup"><span data-stu-id="64c4e-305">Exceptions are logged when they occur.</span></span>

<span data-ttu-id="64c4e-306">En el ejemplo siguiente, se llama a `UpdateHeading` de forma asincrónica cuando se selecciona el botón:</span><span class="sxs-lookup"><span data-stu-id="64c4e-306">In the following example, `UpdateHeading` is called asynchronously when the button is selected:</span></span>

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

### <a name="event-argument-types"></a><span data-ttu-id="64c4e-307">Tipos de argumentos de evento</span><span class="sxs-lookup"><span data-stu-id="64c4e-307">Event argument types</span></span>

<span data-ttu-id="64c4e-308">En algunos eventos, se permiten los tipos de argumento de evento.</span><span class="sxs-lookup"><span data-stu-id="64c4e-308">For some events, event argument types are permitted.</span></span> <span data-ttu-id="64c4e-309">Si no es necesario el acceso a uno de estos tipos de evento, no es necesario en la llamada al método.</span><span class="sxs-lookup"><span data-stu-id="64c4e-309">If access to one of these event types isn't necessary, it isn't required in the method call.</span></span>

<span data-ttu-id="64c4e-310">Los `EventArgs` admitidos se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="64c4e-310">Supported `EventArgs` are shown in the following table.</span></span>

| <span data-ttu-id="64c4e-311">Event</span><span class="sxs-lookup"><span data-stu-id="64c4e-311">Event</span></span>            | <span data-ttu-id="64c4e-312">Clase</span><span class="sxs-lookup"><span data-stu-id="64c4e-312">Class</span></span>                | <span data-ttu-id="64c4e-313">Eventos y notas de DOM</span><span class="sxs-lookup"><span data-stu-id="64c4e-313">DOM events and notes</span></span> |
| ---------------- | -------------------- | -------------------- |
| <span data-ttu-id="64c4e-314">Portapapeles</span><span class="sxs-lookup"><span data-stu-id="64c4e-314">Clipboard</span></span>        | `ClipboardEventArgs` | <span data-ttu-id="64c4e-315">`oncut`, `oncopy`, `onpaste`</span><span class="sxs-lookup"><span data-stu-id="64c4e-315">`oncut`, `oncopy`, `onpaste`</span></span> |
| <span data-ttu-id="64c4e-316">Arrastre</span><span class="sxs-lookup"><span data-stu-id="64c4e-316">Drag</span></span>             | `DragEventArgs`      | <span data-ttu-id="64c4e-317">`ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`, `ondragend`</span><span class="sxs-lookup"><span data-stu-id="64c4e-317">`ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`, `ondragend`</span></span><br><br><span data-ttu-id="64c4e-318">`DataTransfer` y `DataTransferItem` mantener los datos de los elementos arrastrados.</span><span class="sxs-lookup"><span data-stu-id="64c4e-318">`DataTransfer` and `DataTransferItem` hold dragged item data.</span></span> |
| <span data-ttu-id="64c4e-319">Error de :</span><span class="sxs-lookup"><span data-stu-id="64c4e-319">Error</span></span>            | `ErrorEventArgs`     | `onerror` |
| <span data-ttu-id="64c4e-320">Event</span><span class="sxs-lookup"><span data-stu-id="64c4e-320">Event</span></span>            | `EventArgs`          | <span data-ttu-id="64c4e-321">*General*</span><span class="sxs-lookup"><span data-stu-id="64c4e-321">*General*</span></span><br><span data-ttu-id="64c4e-322">`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onended`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`</span><span class="sxs-lookup"><span data-stu-id="64c4e-322">`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onended`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`</span></span><br><br><span data-ttu-id="64c4e-323">*Portapapeles*</span><span class="sxs-lookup"><span data-stu-id="64c4e-323">*Clipboard*</span></span><br><span data-ttu-id="64c4e-324">`onbeforecut`, `onbeforecopy`, `onbeforepaste`</span><span class="sxs-lookup"><span data-stu-id="64c4e-324">`onbeforecut`, `onbeforecopy`, `onbeforepaste`</span></span><br><br><span data-ttu-id="64c4e-325">*Entrada*</span><span class="sxs-lookup"><span data-stu-id="64c4e-325">*Input*</span></span><br><span data-ttu-id="64c4e-326">`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart`, `onsubmit`</span><span class="sxs-lookup"><span data-stu-id="64c4e-326">`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart`, `onsubmit`</span></span><br><br><span data-ttu-id="64c4e-327">*Soporte*</span><span class="sxs-lookup"><span data-stu-id="64c4e-327">*Media*</span></span><br><span data-ttu-id="64c4e-328">`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting`</span><span class="sxs-lookup"><span data-stu-id="64c4e-328">`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting`</span></span> |
| <span data-ttu-id="64c4e-329">Foco</span><span class="sxs-lookup"><span data-stu-id="64c4e-329">Focus</span></span>            | `FocusEventArgs`     | <span data-ttu-id="64c4e-330">`onfocus`, `onblur`, `onfocusin`, `onfocusout`</span><span class="sxs-lookup"><span data-stu-id="64c4e-330">`onfocus`, `onblur`, `onfocusin`, `onfocusout`</span></span><br><br><span data-ttu-id="64c4e-331">No incluye compatibilidad con `relatedTarget`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-331">Doesn't include support for `relatedTarget`.</span></span> |
| <span data-ttu-id="64c4e-332">Input</span><span class="sxs-lookup"><span data-stu-id="64c4e-332">Input</span></span>            | `ChangeEventArgs`    | <span data-ttu-id="64c4e-333">`onchange`, `oninput`</span><span class="sxs-lookup"><span data-stu-id="64c4e-333">`onchange`, `oninput`</span></span> |
| <span data-ttu-id="64c4e-334">Teclado</span><span class="sxs-lookup"><span data-stu-id="64c4e-334">Keyboard</span></span>         | `KeyboardEventArgs`  | <span data-ttu-id="64c4e-335">`onkeydown`, `onkeypress`, `onkeyup`</span><span class="sxs-lookup"><span data-stu-id="64c4e-335">`onkeydown`, `onkeypress`, `onkeyup`</span></span> |
| <span data-ttu-id="64c4e-336">Mouse</span><span class="sxs-lookup"><span data-stu-id="64c4e-336">Mouse</span></span>            | `MouseEventArgs`     | <span data-ttu-id="64c4e-337">`onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout`</span><span class="sxs-lookup"><span data-stu-id="64c4e-337">`onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout`</span></span> |
| <span data-ttu-id="64c4e-338">Puntero del mouse</span><span class="sxs-lookup"><span data-stu-id="64c4e-338">Mouse pointer</span></span>    | `PointerEventArgs`   | <span data-ttu-id="64c4e-339">`onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture`</span><span class="sxs-lookup"><span data-stu-id="64c4e-339">`onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture`</span></span> |
| <span data-ttu-id="64c4e-340">Rueda del mouse</span><span class="sxs-lookup"><span data-stu-id="64c4e-340">Mouse wheel</span></span>      | `WheelEventArgs`     | <span data-ttu-id="64c4e-341">`onwheel`, `onmousewheel`</span><span class="sxs-lookup"><span data-stu-id="64c4e-341">`onwheel`, `onmousewheel`</span></span> |
| <span data-ttu-id="64c4e-342">Progreso</span><span class="sxs-lookup"><span data-stu-id="64c4e-342">Progress</span></span>         | `ProgressEventArgs`  | <span data-ttu-id="64c4e-343">`onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress`, `ontimeout`</span><span class="sxs-lookup"><span data-stu-id="64c4e-343">`onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress`, `ontimeout`</span></span> |
| <span data-ttu-id="64c4e-344">Entrada táctil</span><span class="sxs-lookup"><span data-stu-id="64c4e-344">Touch</span></span>            | `TouchEventArgs`     | <span data-ttu-id="64c4e-345">`ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave`, `ontouchcancel`</span><span class="sxs-lookup"><span data-stu-id="64c4e-345">`ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave`, `ontouchcancel`</span></span><br><br><span data-ttu-id="64c4e-346">`TouchPoint` representa un único punto de contacto en un dispositivo con distinción de toque.</span><span class="sxs-lookup"><span data-stu-id="64c4e-346">`TouchPoint` represents a single contact point on a touch-sensitive device.</span></span> |

<span data-ttu-id="64c4e-347">Para obtener información sobre las propiedades y el comportamiento de control de eventos de los eventos de la tabla anterior, vea [clases EventArgs en el origen de referencia (rama dotnet/aspnetcore Release/3.1)](https://github.com/dotnet/aspnetcore/tree/release/3.1/src/Components/Web/src/Web).</span><span class="sxs-lookup"><span data-stu-id="64c4e-347">For information on the properties and event handling behavior of the events in the preceding table, see [EventArgs classes in the reference source (dotnet/aspnetcore release/3.1 branch)](https://github.com/dotnet/aspnetcore/tree/release/3.1/src/Components/Web/src/Web).</span></span>

### <a name="lambda-expressions"></a><span data-ttu-id="64c4e-348">Expresiones lambda</span><span class="sxs-lookup"><span data-stu-id="64c4e-348">Lambda expressions</span></span>

<span data-ttu-id="64c4e-349">También se pueden usar expresiones lambda:</span><span class="sxs-lookup"><span data-stu-id="64c4e-349">Lambda expressions can also be used:</span></span>

```razor
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="64c4e-350">A menudo resulta cómodo cerrar los valores adicionales, como al recorrer en iteración un conjunto de elementos.</span><span class="sxs-lookup"><span data-stu-id="64c4e-350">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="64c4e-351">En el ejemplo siguiente se crean tres botones, cada uno de los cuales llama a `UpdateHeading` pasar un argumento de evento (`MouseEventArgs`) y su número de botón (`buttonNumber`) cuando se selecciona en la interfaz de usuario:</span><span class="sxs-lookup"><span data-stu-id="64c4e-351">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`MouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

```razor
<h2>@_message</h2>

@for (var i = 1; i < 4; i++)
{
    var buttonNumber = i;

    <button class="btn btn-primary"
            @onclick="@(e => UpdateHeading(e, buttonNumber))">
        Button #@i
    </button>
}

@code {
    private string _message = "Select a button to learn its position.";

    private void UpdateHeading(MouseEventArgs e, int buttonNumber)
    {
        _message = $"You selected Button #{buttonNumber} at " +
            $"mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> <span data-ttu-id="64c4e-352">**No** utilice la variable de bucle (`i`) en un bucle de `for` directamente en una expresión lambda.</span><span class="sxs-lookup"><span data-stu-id="64c4e-352">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="64c4e-353">De lo contrario, todas las expresiones lambda usan la misma variable, lo que hace que el valor de `i`sea el mismo en todas las expresiones lambda.</span><span class="sxs-lookup"><span data-stu-id="64c4e-353">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="64c4e-354">Capture siempre su valor en una variable local (`buttonNumber` en el ejemplo anterior) y, a continuación, úsela.</span><span class="sxs-lookup"><span data-stu-id="64c4e-354">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

### <a name="eventcallback"></a><span data-ttu-id="64c4e-355">EventCallback</span><span class="sxs-lookup"><span data-stu-id="64c4e-355">EventCallback</span></span>

<span data-ttu-id="64c4e-356">Un escenario común con los componentes anidados es el deseo de ejecutar el método de un componente primario cuando se produce un evento de componente secundario&mdash;por ejemplo, cuando se produce un evento de `onclick` en el elemento secundario.</span><span class="sxs-lookup"><span data-stu-id="64c4e-356">A common scenario with nested components is the desire to run a parent component's method when a child component event occurs&mdash;for example, when an `onclick` event occurs in the child.</span></span> <span data-ttu-id="64c4e-357">Para exponer eventos entre componentes, use una `EventCallback`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-357">To expose events across components, use an `EventCallback`.</span></span> <span data-ttu-id="64c4e-358">Un componente primario puede asignar un método de devolución de llamada al `EventCallback`de un componente secundario.</span><span class="sxs-lookup"><span data-stu-id="64c4e-358">A parent component can assign a callback method to a child component's `EventCallback`.</span></span>

<span data-ttu-id="64c4e-359">En el `ChildComponent` de la aplicación de ejemplo (*Components/ChildComponent. Razor*) se muestra cómo se configura el controlador de `onclick` de un botón para recibir un `EventCallback` delegado de la `ParentComponent`del ejemplo.</span><span class="sxs-lookup"><span data-stu-id="64c4e-359">The `ChildComponent` in the sample app (*Components/ChildComponent.razor*) demonstrates how a button's `onclick` handler is set up to receive an `EventCallback` delegate from the sample's `ParentComponent`.</span></span> <span data-ttu-id="64c4e-360">El `EventCallback` se escribe con `MouseEventArgs`, que es adecuado para un evento de `onclick` desde un dispositivo periférico:</span><span class="sxs-lookup"><span data-stu-id="64c4e-360">The `EventCallback` is typed with `MouseEventArgs`, which is appropriate for an `onclick` event from a peripheral device:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=5-7,17-18)]

<span data-ttu-id="64c4e-361">El `ParentComponent` establece el `EventCallback<T>` (`OnClick`) del elemento secundario en su método `ShowMessage`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-361">The `ParentComponent` sets the child's `EventCallback<T>` (`OnClick`) to its `ShowMessage` method.</span></span>

<span data-ttu-id="64c4e-362">*Pages/ParentComponent. Razor*:</span><span class="sxs-lookup"><span data-stu-id="64c4e-362">*Pages/ParentComponent.razor*:</span></span>

```razor
@page "/ParentComponent"

<h1>Parent-child example</h1>

<ChildComponent Title="Panel Title from Parent"
                OnClick="@ShowMessage">
    Content of the child component is supplied
    by the parent component.
</ChildComponent>

<p><b>@_messageText</b></p>

@code {
    private string _messageText;

    private void ShowMessage(MouseEventArgs e)
    {
        _messageText = $"Blaze a new trail with Blazor! ({e.ScreenX}, {e.ScreenY})";
    }
}
```

<span data-ttu-id="64c4e-363">Cuando el botón está seleccionado en el `ChildComponent`:</span><span class="sxs-lookup"><span data-stu-id="64c4e-363">When the button is selected in the `ChildComponent`:</span></span>

* <span data-ttu-id="64c4e-364">Se llama al método `ShowMessage` del `ParentComponent`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-364">The `ParentComponent`'s `ShowMessage` method is called.</span></span> <span data-ttu-id="64c4e-365">`_messageText` se actualiza y se muestra en el `ParentComponent`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-365">`_messageText` is updated and displayed in the `ParentComponent`.</span></span>
* <span data-ttu-id="64c4e-366">No se requiere una llamada a [StateHasChanged](xref:blazor/lifecycle#state-changes) en el método de la devolución de llamada (`ShowMessage`).</span><span class="sxs-lookup"><span data-stu-id="64c4e-366">A call to [StateHasChanged](xref:blazor/lifecycle#state-changes) isn't required in the callback's method (`ShowMessage`).</span></span> <span data-ttu-id="64c4e-367">se llama a `StateHasChanged` automáticamente para rerepresentar el `ParentComponent`, del mismo modo que los eventos secundarios desencadenan la rerepresentación de componentes en los controladores de eventos que se ejecutan dentro del elemento secundario.</span><span class="sxs-lookup"><span data-stu-id="64c4e-367">`StateHasChanged` is called automatically to rerender the `ParentComponent`, just as child events trigger component rerendering in event handlers that execute within the child.</span></span>

<span data-ttu-id="64c4e-368">`EventCallback` y `EventCallback<T>` permiten los delegados asincrónicos.</span><span class="sxs-lookup"><span data-stu-id="64c4e-368">`EventCallback` and `EventCallback<T>` permit asynchronous delegates.</span></span> <span data-ttu-id="64c4e-369">`EventCallback<T>` tiene un tipo seguro y requiere un tipo de argumento específico.</span><span class="sxs-lookup"><span data-stu-id="64c4e-369">`EventCallback<T>` is strongly typed and requires a specific argument type.</span></span> <span data-ttu-id="64c4e-370">`EventCallback` tiene un tipo débil y permite cualquier tipo de argumento.</span><span class="sxs-lookup"><span data-stu-id="64c4e-370">`EventCallback` is weakly typed and allows any argument type.</span></span>

```razor
<ChildComponent 
    OnClick="@(async () => { await Task.Yield(); _messageText = "Blaze It!"; })" />
```

<span data-ttu-id="64c4e-371">Invocar un `EventCallback` o `EventCallback<T>` con `InvokeAsync` y esperar el <xref:System.Threading.Tasks.Task>:</span><span class="sxs-lookup"><span data-stu-id="64c4e-371">Invoke an `EventCallback` or `EventCallback<T>` with `InvokeAsync` and await the <xref:System.Threading.Tasks.Task>:</span></span>

```csharp
await callback.InvokeAsync(arg);
```

<span data-ttu-id="64c4e-372">Use `EventCallback` y `EventCallback<T>` para el control de eventos y los parámetros de componente de enlace.</span><span class="sxs-lookup"><span data-stu-id="64c4e-372">Use `EventCallback` and `EventCallback<T>` for event handling and binding component parameters.</span></span>

<span data-ttu-id="64c4e-373">Prefiera el `EventCallback<T>` fuertemente tipado en `EventCallback`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-373">Prefer the strongly typed `EventCallback<T>` over `EventCallback`.</span></span> <span data-ttu-id="64c4e-374">`EventCallback<T>` proporciona mejores comentarios de errores a los usuarios del componente.</span><span class="sxs-lookup"><span data-stu-id="64c4e-374">`EventCallback<T>` provides better error feedback to users of the component.</span></span> <span data-ttu-id="64c4e-375">Al igual que otros controladores de eventos de la interfaz de usuario, la especificación del parámetro Event es opcional.</span><span class="sxs-lookup"><span data-stu-id="64c4e-375">Similar to other UI event handlers, specifying the event parameter is optional.</span></span> <span data-ttu-id="64c4e-376">Use `EventCallback` cuando no haya ningún valor pasado a la devolución de llamada.</span><span class="sxs-lookup"><span data-stu-id="64c4e-376">Use `EventCallback` when there's no value passed to the callback.</span></span>

### <a name="prevent-default-actions"></a><span data-ttu-id="64c4e-377">Impedir acciones predeterminadas</span><span class="sxs-lookup"><span data-stu-id="64c4e-377">Prevent default actions</span></span>

<span data-ttu-id="64c4e-378">Use el atributo de directiva de [`@on{EVENT}:preventDefault`](xref:mvc/views/razor#oneventpreventdefault) para evitar la acción predeterminada de un evento.</span><span class="sxs-lookup"><span data-stu-id="64c4e-378">Use the [`@on{EVENT}:preventDefault`](xref:mvc/views/razor#oneventpreventdefault) directive attribute to prevent the default action for an event.</span></span>

<span data-ttu-id="64c4e-379">Cuando se selecciona una clave en un dispositivo de entrada y el foco del elemento está en un cuadro de texto, un explorador muestra normalmente el carácter de la tecla en el cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="64c4e-379">When a key is selected on an input device and the element focus is on a text box, a browser normally displays the key's character in the text box.</span></span> <span data-ttu-id="64c4e-380">En el ejemplo siguiente, el comportamiento predeterminado se evita especificando el atributo de directiva de `@onkeypress:preventDefault`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-380">In the following example, the default behavior is prevented by specifying the `@onkeypress:preventDefault` directive attribute.</span></span> <span data-ttu-id="64c4e-381">El contador se incrementa y la clave **+** no se captura en el valor del elemento `<input>`:</span><span class="sxs-lookup"><span data-stu-id="64c4e-381">The counter increments, and the **+** key isn't captured into the `<input>` element's value:</span></span>

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

<span data-ttu-id="64c4e-382">Especificar el `@on{EVENT}:preventDefault` atributo sin un valor es equivalente a `@on{EVENT}:preventDefault="true"`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-382">Specifying the `@on{EVENT}:preventDefault` attribute without a value is equivalent to `@on{EVENT}:preventDefault="true"`.</span></span>

<span data-ttu-id="64c4e-383">El valor del atributo también puede ser una expresión.</span><span class="sxs-lookup"><span data-stu-id="64c4e-383">The value of the attribute can also be an expression.</span></span> <span data-ttu-id="64c4e-384">En el ejemplo siguiente, `_shouldPreventDefault` es un campo `bool` establecido en `true` o `false`:</span><span class="sxs-lookup"><span data-stu-id="64c4e-384">In the following example, `_shouldPreventDefault` is a `bool` field set to either `true` or `false`:</span></span>

```razor
<input @onkeypress:preventDefault="_shouldPreventDefault" />
```

<span data-ttu-id="64c4e-385">No es necesario un controlador de eventos para evitar la acción predeterminada.</span><span class="sxs-lookup"><span data-stu-id="64c4e-385">An event handler isn't required to prevent the default action.</span></span> <span data-ttu-id="64c4e-386">El controlador de eventos y evitar escenarios de acción predeterminados se pueden usar de forma independiente.</span><span class="sxs-lookup"><span data-stu-id="64c4e-386">The event handler and prevent default action scenarios can be used independently.</span></span>

### <a name="stop-event-propagation"></a><span data-ttu-id="64c4e-387">Detener propagación de eventos</span><span class="sxs-lookup"><span data-stu-id="64c4e-387">Stop event propagation</span></span>

<span data-ttu-id="64c4e-388">Use el atributo de directiva de [`@on{EVENT}:stopPropagation`](xref:mvc/views/razor#oneventstoppropagation) para detener la propagación de eventos.</span><span class="sxs-lookup"><span data-stu-id="64c4e-388">Use the [`@on{EVENT}:stopPropagation`](xref:mvc/views/razor#oneventstoppropagation) directive attribute to stop event propagation.</span></span>

<span data-ttu-id="64c4e-389">En el ejemplo siguiente, al activar la casilla se impide que los eventos de clic de la segunda `<div>` secundaria se propaguen al `<div>`primario:</span><span class="sxs-lookup"><span data-stu-id="64c4e-389">In the following example, selecting the check box prevents click events from the second child `<div>` from propagating to the parent `<div>`:</span></span>

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

## <a name="chained-bind"></a><span data-ttu-id="64c4e-390">Enlace encadenado</span><span class="sxs-lookup"><span data-stu-id="64c4e-390">Chained bind</span></span>

<span data-ttu-id="64c4e-391">Un escenario común es encadenar un parámetro enlazado a datos a un elemento Page en la salida del componente.</span><span class="sxs-lookup"><span data-stu-id="64c4e-391">A common scenario is chaining a data-bound parameter to a page element in the component's output.</span></span> <span data-ttu-id="64c4e-392">Este escenario se denomina *enlace encadenado* porque se producen varios niveles de enlace simultáneamente.</span><span class="sxs-lookup"><span data-stu-id="64c4e-392">This scenario is called a *chained bind* because multiple levels of binding occur simultaneously.</span></span>

<span data-ttu-id="64c4e-393">No se puede implementar un enlace encadenado con `@bind` sintaxis en el elemento de la página.</span><span class="sxs-lookup"><span data-stu-id="64c4e-393">A chained bind can't be implemented with `@bind` syntax in the page's element.</span></span> <span data-ttu-id="64c4e-394">El controlador de eventos y el valor se deben especificar por separado.</span><span class="sxs-lookup"><span data-stu-id="64c4e-394">The event handler and value must be specified separately.</span></span> <span data-ttu-id="64c4e-395">Sin embargo, un componente primario puede usar la sintaxis de `@bind` con el parámetro del componente.</span><span class="sxs-lookup"><span data-stu-id="64c4e-395">A parent component, however, can use `@bind` syntax with the component's parameter.</span></span>

<span data-ttu-id="64c4e-396">El siguiente componente de `PasswordField` (*PasswordField. Razor*):</span><span class="sxs-lookup"><span data-stu-id="64c4e-396">The following `PasswordField` component (*PasswordField.razor*):</span></span>

* <span data-ttu-id="64c4e-397">Establece el valor de un elemento de `<input>` en una propiedad `Password`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-397">Sets an `<input>` element's value to a `Password` property.</span></span>
* <span data-ttu-id="64c4e-398">Expone los cambios de la propiedad `Password` a un componente primario con un [EventCallback](#eventcallback).</span><span class="sxs-lookup"><span data-stu-id="64c4e-398">Exposes changes of the `Password` property to a parent component with an [EventCallback](#eventcallback).</span></span>

```razor
Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(_showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

@code {
    private bool _showPassword;

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
        _showPassword = !_showPassword;
    }
}
```

<span data-ttu-id="64c4e-399">El componente de `PasswordField` se usa en otro componente:</span><span class="sxs-lookup"><span data-stu-id="64c4e-399">The `PasswordField` component is used in another component:</span></span>

```razor
<PasswordField @bind-Password="_password" />

@code {
    private string _password;
}
```

<span data-ttu-id="64c4e-400">Para realizar comprobaciones o errores de captura en la contraseña en el ejemplo anterior:</span><span class="sxs-lookup"><span data-stu-id="64c4e-400">To perform checks or trap errors on the password in the preceding example:</span></span>

* <span data-ttu-id="64c4e-401">Cree un campo de respaldo para `Password` (`_password` en el siguiente código de ejemplo).</span><span class="sxs-lookup"><span data-stu-id="64c4e-401">Create a backing field for `Password` (`_password` in the following example code).</span></span>
* <span data-ttu-id="64c4e-402">Realice las comprobaciones o errores de captura en el establecedor de `Password`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-402">Perform the checks or trap errors in the `Password` setter.</span></span>

<span data-ttu-id="64c4e-403">En el ejemplo siguiente se proporcionan comentarios inmediatos al usuario si se usa un espacio en el valor de la contraseña:</span><span class="sxs-lookup"><span data-stu-id="64c4e-403">The following example provides immediate feedback to the user if a space is used in the password's value:</span></span>

```razor
Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(_showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

<span class="text-danger">@_validationMessage</span>

@code {
    private bool _showPassword;
    private string _password;
    private string _validationMessage;

    [Parameter]
    public string Password
    {
        get { return _password ?? string.Empty; }
        set
        {
            if (_password != value)
            {
                if (value.Contains(' '))
                {
                    _validationMessage = "Spaces not allowed!";
                }
                else
                {
                    _password = value;
                    _validationMessage = string.Empty;
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
        _showPassword = !_showPassword;
    }
}
```

## <a name="capture-references-to-components"></a><span data-ttu-id="64c4e-404">Capturar referencias a componentes</span><span class="sxs-lookup"><span data-stu-id="64c4e-404">Capture references to components</span></span>

<span data-ttu-id="64c4e-405">Las referencias de componente proporcionan una manera de hacer referencia a una instancia de componente para que pueda emitir comandos a esa instancia, como `Show` o `Reset`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-405">Component references provide a way to reference a component instance so that you can issue commands to that instance, such as `Show` or `Reset`.</span></span> <span data-ttu-id="64c4e-406">Para capturar una referencia de componente:</span><span class="sxs-lookup"><span data-stu-id="64c4e-406">To capture a component reference:</span></span>

* <span data-ttu-id="64c4e-407">Agregue un atributo [`@ref`](xref:mvc/views/razor#ref) al componente secundario.</span><span class="sxs-lookup"><span data-stu-id="64c4e-407">Add an [`@ref`](xref:mvc/views/razor#ref) attribute to the child component.</span></span>
* <span data-ttu-id="64c4e-408">Defina un campo con el mismo tipo que el componente secundario.</span><span class="sxs-lookup"><span data-stu-id="64c4e-408">Define a field with the same type as the child component.</span></span>

```razor
<MyLoginDialog @ref="_loginDialog" ... />

@code {
    private MyLoginDialog _loginDialog;

    private void OnSomething()
    {
        _loginDialog.Show();
    }
}
```

<span data-ttu-id="64c4e-409">Cuando se representa el componente, el campo de `_loginDialog` se rellena con la instancia del componente secundario `MyLoginDialog`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-409">When the component is rendered, the `_loginDialog` field is populated with the `MyLoginDialog` child component instance.</span></span> <span data-ttu-id="64c4e-410">Después, puede invocar métodos .NET en la instancia del componente.</span><span class="sxs-lookup"><span data-stu-id="64c4e-410">You can then invoke .NET methods on the component instance.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="64c4e-411">La variable `_loginDialog` solo se rellena después de que el componente se represente y su salida incluye el elemento `MyLoginDialog`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-411">The `_loginDialog` variable is only populated after the component is rendered and its output includes the `MyLoginDialog` element.</span></span> <span data-ttu-id="64c4e-412">Hasta ese momento, no hay nada que hacer referencia.</span><span class="sxs-lookup"><span data-stu-id="64c4e-412">Until that point, there's nothing to reference.</span></span> <span data-ttu-id="64c4e-413">Para manipular las referencias de componentes una vez finalizada la representación del componente, use los [métodos OnAfterRenderAsync o OnAfterRender](xref:blazor/lifecycle#after-component-render).</span><span class="sxs-lookup"><span data-stu-id="64c4e-413">To manipulate components references after the component has finished rendering, use the [OnAfterRenderAsync or OnAfterRender methods](xref:blazor/lifecycle#after-component-render).</span></span>

<span data-ttu-id="64c4e-414">Al capturar referencias de componentes, use una sintaxis similar para [capturar referencias de elemento](xref:blazor/javascript-interop#capture-references-to-elements), no es una característica de [interoperabilidad de JavaScript](xref:blazor/javascript-interop) .</span><span class="sxs-lookup"><span data-stu-id="64c4e-414">While capturing component references use a similar syntax to [capturing element references](xref:blazor/javascript-interop#capture-references-to-elements), it isn't a [JavaScript interop](xref:blazor/javascript-interop) feature.</span></span> <span data-ttu-id="64c4e-415">Las referencias a componentes no se pasan al código JavaScript&mdash;solo se usan en código .NET.</span><span class="sxs-lookup"><span data-stu-id="64c4e-415">Component references aren't passed to JavaScript code&mdash;they're only used in .NET code.</span></span>

> [!NOTE]
> <span data-ttu-id="64c4e-416">**No** utilice referencias de componentes para mutar el estado de los componentes secundarios.</span><span class="sxs-lookup"><span data-stu-id="64c4e-416">Do **not** use component references to mutate the state of child components.</span></span> <span data-ttu-id="64c4e-417">En su lugar, use parámetros declarativos normales para pasar datos a componentes secundarios.</span><span class="sxs-lookup"><span data-stu-id="64c4e-417">Instead, use normal declarative parameters to pass data to child components.</span></span> <span data-ttu-id="64c4e-418">El uso de parámetros declarativos normales da como resultado componentes secundarios que se representarán automáticamente en las horas correctas.</span><span class="sxs-lookup"><span data-stu-id="64c4e-418">Use of normal declarative parameters result in child components that rerender at the correct times automatically.</span></span>

## <a name="invoke-component-methods-externally-to-update-state"></a><span data-ttu-id="64c4e-419">Invocar métodos de componentes externamente para actualizar el estado</span><span class="sxs-lookup"><span data-stu-id="64c4e-419">Invoke component methods externally to update state</span></span>

<span data-ttu-id="64c4e-420">Increíble utiliza un `SynchronizationContext` para aplicar un único subproceso lógico de ejecución.</span><span class="sxs-lookup"><span data-stu-id="64c4e-420">Blazor uses a `SynchronizationContext` to enforce a single logical thread of execution.</span></span> <span data-ttu-id="64c4e-421">[Los métodos de ciclo de vida](xref:blazor/lifecycle) de un componente y todas las devoluciones de llamada de evento que se producen con el método increíblemente se ejecutan en este `SynchronizationContext`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-421">A component's [lifecycle methods](xref:blazor/lifecycle) and any event callbacks that are raised by Blazor are executed on this `SynchronizationContext`.</span></span> <span data-ttu-id="64c4e-422">En caso de que un componente deba actualizarse en función de un evento externo, como un temporizador u otras notificaciones, use el método `InvokeAsync`, que se enviará al `SynchronizationContext`de la increíble.</span><span class="sxs-lookup"><span data-stu-id="64c4e-422">In the event a component must be updated based on an external event, such as a timer or other notifications, use the `InvokeAsync` method, which will dispatch to Blazor's `SynchronizationContext`.</span></span>

<span data-ttu-id="64c4e-423">Por ejemplo, considere un *servicio de notificador* que puede notificar a cualquier componente de escucha del estado actualizado:</span><span class="sxs-lookup"><span data-stu-id="64c4e-423">For example, consider a *notifier service* that can notify any listening component of the updated state:</span></span>

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

<span data-ttu-id="64c4e-424">Uso de la `NotifierService` para actualizar un componente:</span><span class="sxs-lookup"><span data-stu-id="64c4e-424">Usage of the `NotifierService` to update a component:</span></span>

```razor
@page "/"
@inject NotifierService Notifier
@implements IDisposable

<p>Last update: @_lastNotification.key = @_lastNotification.value</p>

@code {
    private (string key, int value) _lastNotification;

    protected override void OnInitialized()
    {
        Notifier.Notify += OnNotify;
    }

    public async Task OnNotify(string key, int value)
    {
        await InvokeAsync(() =>
        {
            _lastNotification = (key, value);
            StateHasChanged();
        });
    }

    public void Dispose()
    {
        Notifier.Notify -= OnNotify;
    }
}
```

<span data-ttu-id="64c4e-425">En el ejemplo anterior, `NotifierService` invoca el método de `OnNotify` del componente fuera del `SynchronizationContext`de la extraordinaria.</span><span class="sxs-lookup"><span data-stu-id="64c4e-425">In the preceding example, `NotifierService` invokes the component's `OnNotify` method outside of Blazor's `SynchronizationContext`.</span></span> <span data-ttu-id="64c4e-426">`InvokeAsync` se utiliza para cambiar al contexto correcto y poner en cola una representación.</span><span class="sxs-lookup"><span data-stu-id="64c4e-426">`InvokeAsync` is used to switch to the correct context and queue a render.</span></span>

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a><span data-ttu-id="64c4e-427">Usar \@clave para controlar la preservación de elementos y componentes</span><span class="sxs-lookup"><span data-stu-id="64c4e-427">Use \@key to control the preservation of elements and components</span></span>

<span data-ttu-id="64c4e-428">Cuando se representa una lista de elementos o componentes, y los elementos o componentes cambian posteriormente, el algoritmo de diferenciación de más increíble debe decidir cuáles de los elementos o componentes anteriores se pueden conservar y cómo deben asignarse los objetos de modelo.</span><span class="sxs-lookup"><span data-stu-id="64c4e-428">When rendering a list of elements or components and the elements or components subsequently change, Blazor's diffing algorithm must decide which of the previous elements or components can be retained and how model objects should map to them.</span></span> <span data-ttu-id="64c4e-429">Normalmente, este proceso es automático y se puede omitir, pero hay casos en los que puede que desee controlar el proceso.</span><span class="sxs-lookup"><span data-stu-id="64c4e-429">Normally, this process is automatic and can be ignored, but there are cases where you may want to control the process.</span></span>

<span data-ttu-id="64c4e-430">Considere el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="64c4e-430">Consider the following example:</span></span>

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

<span data-ttu-id="64c4e-431">El contenido de la colección de `People` puede cambiar con entradas insertadas, eliminadas o reordenadas.</span><span class="sxs-lookup"><span data-stu-id="64c4e-431">The contents of the `People` collection may change with inserted, deleted, or re-ordered entries.</span></span> <span data-ttu-id="64c4e-432">Cuando se representa el componente, el componente de `<DetailsEditor>` puede cambiar para recibir diferentes valores de parámetro de `Details`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-432">When the component rerenders, the `<DetailsEditor>` component may change to receive different `Details` parameter values.</span></span> <span data-ttu-id="64c4e-433">Esto puede producir una rerepresentación más compleja de lo esperado.</span><span class="sxs-lookup"><span data-stu-id="64c4e-433">This may cause more complex rerendering than expected.</span></span> <span data-ttu-id="64c4e-434">En algunos casos, la rerepresentación puede producir diferencias de comportamiento visibles, como el foco del elemento perdido.</span><span class="sxs-lookup"><span data-stu-id="64c4e-434">In some cases, rerendering can lead to visible behavior differences, such as lost element focus.</span></span>

<span data-ttu-id="64c4e-435">El proceso de asignación se puede controlar con el `@key` atributo de directiva.</span><span class="sxs-lookup"><span data-stu-id="64c4e-435">The mapping process can be controlled with the `@key` directive attribute.</span></span> <span data-ttu-id="64c4e-436">`@key` hace que el algoritmo de comparación garantice la preservación de elementos o componentes basados en el valor de la clave:</span><span class="sxs-lookup"><span data-stu-id="64c4e-436">`@key` causes the diffing algorithm to guarantee preservation of elements or components based on the key's value:</span></span>

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

<span data-ttu-id="64c4e-437">Cuando cambia la colección de `People`, el algoritmo de comparación mantiene la asociación entre `<DetailsEditor>` instancias y `person` instancias:</span><span class="sxs-lookup"><span data-stu-id="64c4e-437">When the `People` collection changes, the diffing algorithm retains the association between `<DetailsEditor>` instances and `person` instances:</span></span>

* <span data-ttu-id="64c4e-438">Si se elimina un `Person` de la lista de `People`, solo se quita la instancia de `<DetailsEditor>` correspondiente de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="64c4e-438">If a `Person` is deleted from the `People` list, only the corresponding `<DetailsEditor>` instance is removed from the UI.</span></span> <span data-ttu-id="64c4e-439">Otras instancias permanecen sin cambios.</span><span class="sxs-lookup"><span data-stu-id="64c4e-439">Other instances are left unchanged.</span></span>
* <span data-ttu-id="64c4e-440">Si se inserta un `Person` en alguna posición de la lista, se inserta una nueva instancia de `<DetailsEditor>` en la posición correspondiente.</span><span class="sxs-lookup"><span data-stu-id="64c4e-440">If a `Person` is inserted at some position in the list, one new `<DetailsEditor>` instance is inserted at that corresponding position.</span></span> <span data-ttu-id="64c4e-441">Otras instancias permanecen sin cambios.</span><span class="sxs-lookup"><span data-stu-id="64c4e-441">Other instances are left unchanged.</span></span>
* <span data-ttu-id="64c4e-442">Si se vuelven a ordenar `Person` entradas, se conservan y se vuelven a ordenar las instancias de `<DetailsEditor>` correspondientes en la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="64c4e-442">If `Person` entries are re-ordered, the corresponding `<DetailsEditor>` instances are preserved and re-ordered in the UI.</span></span>

<span data-ttu-id="64c4e-443">En algunos escenarios, el uso de `@key` minimiza la complejidad de la rerepresentación y evita posibles problemas con las partes con estado del DOM que cambian, como la posición del foco.</span><span class="sxs-lookup"><span data-stu-id="64c4e-443">In some scenarios, use of `@key` minimizes the complexity of rerendering and avoids potential issues with stateful parts of the DOM changing, such as focus position.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="64c4e-444">Las claves son locales para cada elemento contenedor o componente.</span><span class="sxs-lookup"><span data-stu-id="64c4e-444">Keys are local to each container element or component.</span></span> <span data-ttu-id="64c4e-445">Las claves no se comparan globalmente en todo el documento.</span><span class="sxs-lookup"><span data-stu-id="64c4e-445">Keys aren't compared globally across the document.</span></span>

### <a name="when-to-use-key"></a><span data-ttu-id="64c4e-446">Cuándo usar la clave de \@</span><span class="sxs-lookup"><span data-stu-id="64c4e-446">When to use \@key</span></span>

<span data-ttu-id="64c4e-447">Normalmente, tiene sentido usar `@key` cada vez que se representa una lista (por ejemplo, en un bloque de `@foreach`) y existe un valor adecuado para definir el `@key`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-447">Typically, it makes sense to use `@key` whenever a list is rendered (for example, in a `@foreach` block) and a suitable value exists to define the `@key`.</span></span>

<span data-ttu-id="64c4e-448">También puede usar `@key` para evitar que un elemento o subárbol de componentes se conserven cuando cambia un objeto:</span><span class="sxs-lookup"><span data-stu-id="64c4e-448">You can also use `@key` to prevent Blazor from preserving an element or component subtree when an object changes:</span></span>

```razor
<div @key="currentPerson">
    ... content that depends on currentPerson ...
</div>
```

<span data-ttu-id="64c4e-449">Si `@currentPerson` cambia, la Directiva de atributo de `@key` fuerza a increíblemente a descartar todo el `<div>` y sus descendientes y volver a generar el subárbol dentro de la interfaz de usuario con nuevos elementos y componentes.</span><span class="sxs-lookup"><span data-stu-id="64c4e-449">If `@currentPerson` changes, the `@key` attribute directive forces Blazor to discard the entire `<div>` and its descendants and rebuild the subtree within the UI with new elements and components.</span></span> <span data-ttu-id="64c4e-450">Esto puede ser útil si necesita garantizar que no se conserva ningún estado de la interfaz de usuario cuando `@currentPerson` cambios.</span><span class="sxs-lookup"><span data-stu-id="64c4e-450">This can be useful if you need to guarantee that no UI state is preserved when `@currentPerson` changes.</span></span>

### <a name="when-not-to-use-key"></a><span data-ttu-id="64c4e-451">Cuándo no usar la clave \@</span><span class="sxs-lookup"><span data-stu-id="64c4e-451">When not to use \@key</span></span>

<span data-ttu-id="64c4e-452">Existe un costo de rendimiento al diferenciar con `@key`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-452">There's a performance cost when diffing with `@key`.</span></span> <span data-ttu-id="64c4e-453">El costo de rendimiento no es grande, pero solo especifica `@key` si el control del elemento o las reglas de conservación de componentes se benefician de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="64c4e-453">The performance cost isn't large, but only specify `@key` if controlling the element or component preservation rules benefit the app.</span></span>

<span data-ttu-id="64c4e-454">Incluso si no se utiliza `@key`, Increíblementeer conserva las instancias de elementos y componentes secundarios lo máximo posible.</span><span class="sxs-lookup"><span data-stu-id="64c4e-454">Even if `@key` isn't used, Blazor preserves child element and component instances as much as possible.</span></span> <span data-ttu-id="64c4e-455">La única ventaja de utilizar `@key` es el control sobre *Cómo* se asignan las instancias de modelo a las instancias de componente conservadas, en lugar del algoritmo de diferenciación que selecciona la asignación.</span><span class="sxs-lookup"><span data-stu-id="64c4e-455">The only advantage to using `@key` is control over *how* model instances are mapped to the preserved component instances, instead of the diffing algorithm selecting the mapping.</span></span>

### <a name="what-values-to-use-for-key"></a><span data-ttu-id="64c4e-456">Qué valores se deben usar para la clave de \@</span><span class="sxs-lookup"><span data-stu-id="64c4e-456">What values to use for \@key</span></span>

<span data-ttu-id="64c4e-457">Por lo general, tiene sentido proporcionar uno de los siguientes tipos de valor para `@key`:</span><span class="sxs-lookup"><span data-stu-id="64c4e-457">Generally, it makes sense to supply one of the following kinds of value for `@key`:</span></span>

* <span data-ttu-id="64c4e-458">Instancias de objeto de modelo (por ejemplo, una instancia de `Person` como en el ejemplo anterior).</span><span class="sxs-lookup"><span data-stu-id="64c4e-458">Model object instances (for example, a `Person` instance as in the earlier example).</span></span> <span data-ttu-id="64c4e-459">Esto garantiza la conservación en función de la igualdad de la referencia de objeto.</span><span class="sxs-lookup"><span data-stu-id="64c4e-459">This ensures preservation based on object reference equality.</span></span>
* <span data-ttu-id="64c4e-460">Los identificadores únicos (por ejemplo, los valores de clave principal de tipo `int`, `string`o `Guid`).</span><span class="sxs-lookup"><span data-stu-id="64c4e-460">Unique identifiers (for example, primary key values of type `int`, `string`, or `Guid`).</span></span>

<span data-ttu-id="64c4e-461">Asegúrese de que los valores usados para `@key` no entren en conflicto.</span><span class="sxs-lookup"><span data-stu-id="64c4e-461">Ensure that values used for `@key` don't clash.</span></span> <span data-ttu-id="64c4e-462">Si se detectan valores en conflicto en el mismo elemento primario, el valor de la excepción extraordinaria produce una excepción porque no puede asignar de forma determinista los elementos o componentes antiguos a los nuevos elementos o componentes.</span><span class="sxs-lookup"><span data-stu-id="64c4e-462">If clashing values are detected within the same parent element, Blazor throws an exception because it can't deterministically map old elements or components to new elements or components.</span></span> <span data-ttu-id="64c4e-463">Utilice solo valores distintos, como instancias de objeto o valores de clave principal.</span><span class="sxs-lookup"><span data-stu-id="64c4e-463">Only use distinct values, such as object instances or primary key values.</span></span>

## <a name="routing"></a><span data-ttu-id="64c4e-464">Enrutamiento</span><span class="sxs-lookup"><span data-stu-id="64c4e-464">Routing</span></span>

<span data-ttu-id="64c4e-465">El enrutamiento en extraordinaria se consigue proporcionando una plantilla de ruta a cada componente accesible en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="64c4e-465">Routing in Blazor is achieved by providing a route template to each accessible component in the app.</span></span>

<span data-ttu-id="64c4e-466">Cuando se compila un archivo de Razor con una directiva de `@page`, a la clase generada se le asigna un <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> que especifica la plantilla de ruta.</span><span class="sxs-lookup"><span data-stu-id="64c4e-466">When a Razor file with an `@page` directive is compiled, the generated class is given a <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> specifying the route template.</span></span> <span data-ttu-id="64c4e-467">En tiempo de ejecución, el enrutador busca clases de componentes con un `RouteAttribute` y representa el componente que tiene una plantilla de ruta que coincide con la dirección URL solicitada.</span><span class="sxs-lookup"><span data-stu-id="64c4e-467">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="64c4e-468">Se pueden aplicar varias plantillas de ruta a un componente.</span><span class="sxs-lookup"><span data-stu-id="64c4e-468">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="64c4e-469">El siguiente componente responde a las solicitudes de `/BlazorRoute` y `/DifferentBlazorRoute`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-469">The following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`.</span></span>

<span data-ttu-id="64c4e-470">*Pages/BlazorRoute. Razor*:</span><span class="sxs-lookup"><span data-stu-id="64c4e-470">*Pages/BlazorRoute.razor*:</span></span>

```razor
@page "/BlazorRoute"
@page "/DifferentBlazorRoute"

<h1>Blazor routing</h1>
```

## <a name="route-parameters"></a><span data-ttu-id="64c4e-471">Parámetros de ruta</span><span class="sxs-lookup"><span data-stu-id="64c4e-471">Route parameters</span></span>

<span data-ttu-id="64c4e-472">Los componentes pueden recibir parámetros de ruta de la plantilla de ruta proporcionada en la Directiva de `@page`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-472">Components can receive route parameters from the route template provided in the `@page` directive.</span></span> <span data-ttu-id="64c4e-473">El enrutador usa parámetros de ruta para rellenar los parámetros de componente correspondientes.</span><span class="sxs-lookup"><span data-stu-id="64c4e-473">The router uses route parameters to populate the corresponding component parameters.</span></span>

<span data-ttu-id="64c4e-474">*Pages/RouteParameter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="64c4e-474">*Pages/RouteParameter.razor*:</span></span>

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

<span data-ttu-id="64c4e-475">Los parámetros opcionales no se admiten, por lo que se aplican dos directivas de `@page` en el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="64c4e-475">Optional parameters aren't supported, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="64c4e-476">El primero permite la navegación al componente sin un parámetro.</span><span class="sxs-lookup"><span data-stu-id="64c4e-476">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="64c4e-477">La segunda Directiva de `@page` toma el parámetro `{text}` Route y asigna el valor a la propiedad `Text`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-477">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

<span data-ttu-id="64c4e-478">La sintaxis de *los parámetros catch-all* (`*`/`**`), que capturan la ruta de acceso en varios límites de carpeta, **no** se admite en los componentes de Razor ( *. Razor*).</span><span class="sxs-lookup"><span data-stu-id="64c4e-478">*Catch-all* parameter syntax (`*`/`**`), which captures the path across multiple folder boundaries, is **not** supported in Razor components (*.razor*).</span></span>

## <a name="partial-class-support"></a><span data-ttu-id="64c4e-479">Compatibilidad de clases parciales</span><span class="sxs-lookup"><span data-stu-id="64c4e-479">Partial class support</span></span>

<span data-ttu-id="64c4e-480">Los componentes de Razor se generan como clases parciales.</span><span class="sxs-lookup"><span data-stu-id="64c4e-480">Razor components are generated as partial classes.</span></span> <span data-ttu-id="64c4e-481">Los componentes de Razor se crean mediante cualquiera de los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="64c4e-481">Razor components are authored using either of the following approaches:</span></span>

* <span data-ttu-id="64c4e-482">C#el código se define en un bloque [`@code`](xref:mvc/views/razor#code) con marcado HTML y código Razor en un único archivo.</span><span class="sxs-lookup"><span data-stu-id="64c4e-482">C# code is defined in an [`@code`](xref:mvc/views/razor#code) block with HTML markup and Razor code in a single file.</span></span> <span data-ttu-id="64c4e-483">Las plantillas extraordinarias definen sus componentes de Razor mediante este enfoque.</span><span class="sxs-lookup"><span data-stu-id="64c4e-483">Blazor templates define their Razor components using this approach.</span></span>
* <span data-ttu-id="64c4e-484">C#el código se coloca en un archivo de código subyacente definido como una clase parcial.</span><span class="sxs-lookup"><span data-stu-id="64c4e-484">C# code is placed in a code-behind file defined as a partial class.</span></span>

<span data-ttu-id="64c4e-485">En el siguiente ejemplo se muestra el componente de `Counter` predeterminado con un bloque de `@code` en una aplicación generada a partir de una plantilla extraordinaria.</span><span class="sxs-lookup"><span data-stu-id="64c4e-485">The following example shows the default `Counter` component with an `@code` block in an app generated from a Blazor template.</span></span> <span data-ttu-id="64c4e-486">El marcado HTML, el código Razor C# y el código se encuentran en el mismo archivo:</span><span class="sxs-lookup"><span data-stu-id="64c4e-486">HTML markup, Razor code, and C# code are in the same file:</span></span>

<span data-ttu-id="64c4e-487">*Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="64c4e-487">*Counter.razor*:</span></span>

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @_currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>

@code {
    private int _currentCount = 0;

    void IncrementCount()
    {
        _currentCount++;
    }
}
```

<span data-ttu-id="64c4e-488">El componente de `Counter` también se puede crear con un archivo de código subyacente con una clase parcial:</span><span class="sxs-lookup"><span data-stu-id="64c4e-488">The `Counter` component can also be created using a code-behind file with a partial class:</span></span>

<span data-ttu-id="64c4e-489">*Counter. Razor*:</span><span class="sxs-lookup"><span data-stu-id="64c4e-489">*Counter.razor*:</span></span>

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @_currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>
```

<span data-ttu-id="64c4e-490">*Counter.Razor.CS*:</span><span class="sxs-lookup"><span data-stu-id="64c4e-490">*Counter.razor.cs*:</span></span>

```csharp
namespace BlazorApp.Pages
{
    public partial class Counter
    {
        private int _currentCount = 0;

        void IncrementCount()
        {
            _currentCount++;
        }
    }
}
```

<span data-ttu-id="64c4e-491">Agregue los espacios de nombres necesarios al archivo de clase parcial según sea necesario.</span><span class="sxs-lookup"><span data-stu-id="64c4e-491">Add any required namespaces to the partial class file as needed.</span></span> <span data-ttu-id="64c4e-492">Los espacios de nombres típicos utilizados por los componentes de Razor incluyen:</span><span class="sxs-lookup"><span data-stu-id="64c4e-492">Typical namespaces used by Razor components include:</span></span>

```csharp
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Components;
using Microsoft.AspNetCore.Components.Authorization;
using Microsoft.AspNetCore.Components.Forms;
using Microsoft.AspNetCore.Components.Routing;
using Microsoft.AspNetCore.Components.Web;
```

## <a name="specify-a-base-class"></a><span data-ttu-id="64c4e-493">Especificar una clase base</span><span class="sxs-lookup"><span data-stu-id="64c4e-493">Specify a base class</span></span>

<span data-ttu-id="64c4e-494">La directiva [`@inherits`](xref:mvc/views/razor#inherits) se puede utilizar para especificar una clase base para un componente.</span><span class="sxs-lookup"><span data-stu-id="64c4e-494">The [`@inherits`](xref:mvc/views/razor#inherits) directive can be used to specify a base class for a component.</span></span> <span data-ttu-id="64c4e-495">En el ejemplo siguiente se muestra cómo un componente puede heredar una clase base, `BlazorRocksBase`, para proporcionar las propiedades y los métodos del componente.</span><span class="sxs-lookup"><span data-stu-id="64c4e-495">The following example shows how a component can inherit a base class, `BlazorRocksBase`, to provide the component's properties and methods.</span></span> <span data-ttu-id="64c4e-496">La clase base debe derivar de `ComponentBase`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-496">The base class should derive from `ComponentBase`.</span></span>

<span data-ttu-id="64c4e-497">*Pages/BlazorRocks. Razor*:</span><span class="sxs-lookup"><span data-stu-id="64c4e-497">*Pages/BlazorRocks.razor*:</span></span>

```razor
@page "/BlazorRocks"
@inherits BlazorRocksBase

<h1>@BlazorRocksText</h1>
```

<span data-ttu-id="64c4e-498">*BlazorRocksBase.cs*:</span><span class="sxs-lookup"><span data-stu-id="64c4e-498">*BlazorRocksBase.cs*:</span></span>

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

## <a name="specify-an-attribute"></a><span data-ttu-id="64c4e-499">Especificar un atributo</span><span class="sxs-lookup"><span data-stu-id="64c4e-499">Specify an attribute</span></span>

<span data-ttu-id="64c4e-500">Los atributos se pueden especificar en los componentes de Razor con la directiva [`@attribute`](xref:mvc/views/razor#attribute) .</span><span class="sxs-lookup"><span data-stu-id="64c4e-500">Attributes can be specified in Razor components with the [`@attribute`](xref:mvc/views/razor#attribute) directive.</span></span> <span data-ttu-id="64c4e-501">En el ejemplo siguiente se aplica el atributo `[Authorize]` a la clase Component:</span><span class="sxs-lookup"><span data-stu-id="64c4e-501">The following example applies the `[Authorize]` attribute to the component class:</span></span>

```razor
@page "/"
@attribute [Authorize]
```

## <a name="import-components"></a><span data-ttu-id="64c4e-502">Importar componentes</span><span class="sxs-lookup"><span data-stu-id="64c4e-502">Import components</span></span>

<span data-ttu-id="64c4e-503">El espacio de nombres de un componente creado con Razor se basa en (en orden de prioridad):</span><span class="sxs-lookup"><span data-stu-id="64c4e-503">The namespace of a component authored with Razor is based on (in priority order):</span></span>

* <span data-ttu-id="64c4e-504">[`@namespace`](xref:mvc/views/razor#namespace) designación en el marcado de archivos Razor ( *. Razor*) (`@namespace BlazorSample.MyNamespace`).</span><span class="sxs-lookup"><span data-stu-id="64c4e-504">[`@namespace`](xref:mvc/views/razor#namespace) designation in Razor file (*.razor*) markup (`@namespace BlazorSample.MyNamespace`).</span></span>
* <span data-ttu-id="64c4e-505">`RootNamespace` del proyecto en el archivo de proyecto (`<RootNamespace>BlazorSample</RootNamespace>`).</span><span class="sxs-lookup"><span data-stu-id="64c4e-505">The project's `RootNamespace` in the project file (`<RootNamespace>BlazorSample</RootNamespace>`).</span></span>
* <span data-ttu-id="64c4e-506">Nombre del proyecto, tomado del nombre de archivo del archivo de proyecto ( *. csproj*) y la ruta de acceso de la raíz del proyecto al componente.</span><span class="sxs-lookup"><span data-stu-id="64c4e-506">The project name, taken from the project file's file name (*.csproj*), and the path from the project root to the component.</span></span> <span data-ttu-id="64c4e-507">Por ejemplo, el marco de trabajo resuelve *{root Project}/pages/index.Razor* (*BlazorSample. csproj*) en el espacio de nombres `BlazorSample.Pages`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-507">For example, the framework resolves *{PROJECT ROOT}/Pages/Index.razor* (*BlazorSample.csproj*) to the namespace `BlazorSample.Pages`.</span></span> <span data-ttu-id="64c4e-508">Los componentes C# siguen las reglas de enlace de nombres.</span><span class="sxs-lookup"><span data-stu-id="64c4e-508">Components follow C# name binding rules.</span></span> <span data-ttu-id="64c4e-509">En el caso del componente `Index` en este ejemplo, los componentes del ámbito son todos los componentes:</span><span class="sxs-lookup"><span data-stu-id="64c4e-509">For the `Index` component in this example, the components in scope are all of the components:</span></span>
  * <span data-ttu-id="64c4e-510">En la misma carpeta, *páginas*.</span><span class="sxs-lookup"><span data-stu-id="64c4e-510">In the same folder, *Pages*.</span></span>
  * <span data-ttu-id="64c4e-511">Los componentes de la raíz del proyecto que no especifican explícitamente un espacio de nombres diferente.</span><span class="sxs-lookup"><span data-stu-id="64c4e-511">The components in the project's root that don't explicitly specify a different namespace.</span></span>

<span data-ttu-id="64c4e-512">Los componentes definidos en un espacio de nombres diferente se incluyen en el ámbito mediante la Directiva de [`@using`](xref:mvc/views/razor#using) de Razor.</span><span class="sxs-lookup"><span data-stu-id="64c4e-512">Components defined in a different namespace are brought into scope using Razor's [`@using`](xref:mvc/views/razor#using) directive.</span></span>

<span data-ttu-id="64c4e-513">Si existe otro componente, `NavMenu.razor`, en la carpeta *BlazorSample/Shared/* , el componente se puede utilizar en `Index.razor` con la siguiente instrucción `@using`:</span><span class="sxs-lookup"><span data-stu-id="64c4e-513">If another component, `NavMenu.razor`, exists in the *BlazorSample/Shared/* folder, the component can be used in `Index.razor` with the following `@using` statement:</span></span>

```razor
@using BlazorSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

<span data-ttu-id="64c4e-514">También se puede hacer referencia a los componentes mediante sus nombres completos, que no requieren la directiva [`@using`](xref:mvc/views/razor#using) :</span><span class="sxs-lookup"><span data-stu-id="64c4e-514">Components can also be referenced using their fully qualified names, which doesn't require the [`@using`](xref:mvc/views/razor#using) directive:</span></span>

```razor
This is the Index page.

<BlazorSample.Shared.NavMenu></BlazorSample.Shared.NavMenu>
```

> [!NOTE]
> <span data-ttu-id="64c4e-515">No se admite la calificación `global::`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-515">The `global::` qualification isn't supported.</span></span>
>
> <span data-ttu-id="64c4e-516">No se admite la importación de componentes con instrucciones de `using` con alias (por ejemplo, `@using Foo = Bar`).</span><span class="sxs-lookup"><span data-stu-id="64c4e-516">Importing components with aliased `using` statements (for example, `@using Foo = Bar`) isn't supported.</span></span>
>
> <span data-ttu-id="64c4e-517">No se admiten nombres parcialmente completos.</span><span class="sxs-lookup"><span data-stu-id="64c4e-517">Partially qualified names aren't supported.</span></span> <span data-ttu-id="64c4e-518">Por ejemplo, no se admite agregar `@using BlazorSample` y hacer referencia a `NavMenu.razor` con `<Shared.NavMenu></Shared.NavMenu>`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-518">For example, adding `@using BlazorSample` and referencing `NavMenu.razor` with `<Shared.NavMenu></Shared.NavMenu>` isn't supported.</span></span>

## <a name="conditional-html-element-attributes"></a><span data-ttu-id="64c4e-519">Atributos de elementos HTML condicionales</span><span class="sxs-lookup"><span data-stu-id="64c4e-519">Conditional HTML element attributes</span></span>

<span data-ttu-id="64c4e-520">Los atributos de elemento HTML se representan condicionalmente según el valor de .NET.</span><span class="sxs-lookup"><span data-stu-id="64c4e-520">HTML element attributes are conditionally rendered based on the .NET value.</span></span> <span data-ttu-id="64c4e-521">Si el valor es `false` o `null`, el atributo no se representa.</span><span class="sxs-lookup"><span data-stu-id="64c4e-521">If the value is `false` or `null`, the attribute isn't rendered.</span></span> <span data-ttu-id="64c4e-522">Si el valor es `true`, el atributo se representa minimizado.</span><span class="sxs-lookup"><span data-stu-id="64c4e-522">If the value is `true`, the attribute is rendered minimized.</span></span>

<span data-ttu-id="64c4e-523">En el ejemplo siguiente, `IsCompleted` determina si `checked` se representa en el marcado del elemento:</span><span class="sxs-lookup"><span data-stu-id="64c4e-523">In the following example, `IsCompleted` determines if `checked` is rendered in the element's markup:</span></span>

```razor
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

<span data-ttu-id="64c4e-524">Si `IsCompleted` se `true`, la casilla se representa como:</span><span class="sxs-lookup"><span data-stu-id="64c4e-524">If `IsCompleted` is `true`, the check box is rendered as:</span></span>

```html
<input type="checkbox" checked />
```

<span data-ttu-id="64c4e-525">Si `IsCompleted` se `false`, la casilla se representa como:</span><span class="sxs-lookup"><span data-stu-id="64c4e-525">If `IsCompleted` is `false`, the check box is rendered as:</span></span>

```html
<input type="checkbox" />
```

<span data-ttu-id="64c4e-526">Para obtener más información, vea <xref:mvc/views/razor>.</span><span class="sxs-lookup"><span data-stu-id="64c4e-526">For more information, see <xref:mvc/views/razor>.</span></span>

> [!WARNING]
> <span data-ttu-id="64c4e-527">Algunos atributos HTML, como [Aria-pressed](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), no funcionan correctamente cuando el tipo .net es un `bool`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-527">Some HTML attributes, such as [aria-pressed](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), don't function properly when the .NET type is a `bool`.</span></span> <span data-ttu-id="64c4e-528">En esos casos, use un tipo de `string` en lugar de un `bool`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-528">In those cases, use a `string` type instead of a `bool`.</span></span>

## <a name="raw-html"></a><span data-ttu-id="64c4e-529">HTML sin formato</span><span class="sxs-lookup"><span data-stu-id="64c4e-529">Raw HTML</span></span>

<span data-ttu-id="64c4e-530">Normalmente, las cadenas se representan mediante nodos de texto DOM, lo que significa que cualquier marcado que pueda contener se omite y se trata como texto literal.</span><span class="sxs-lookup"><span data-stu-id="64c4e-530">Strings are normally rendered using DOM text nodes, which means that any markup they may contain is ignored and treated as literal text.</span></span> <span data-ttu-id="64c4e-531">Para representar HTML sin formato, ajuste el contenido HTML en un valor de `MarkupString`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-531">To render raw HTML, wrap the HTML content in a `MarkupString` value.</span></span> <span data-ttu-id="64c4e-532">El valor se analiza como HTML o SVG y se inserta en el DOM.</span><span class="sxs-lookup"><span data-stu-id="64c4e-532">The value is parsed as HTML or SVG and inserted into the DOM.</span></span>

> [!WARNING]
> <span data-ttu-id="64c4e-533">La representación de HTML sin formato construido a partir de un origen que no es de confianza es un riesgo para la **seguridad** y debe evitarse.</span><span class="sxs-lookup"><span data-stu-id="64c4e-533">Rendering raw HTML constructed from any untrusted source is a **security risk** and should be avoided!</span></span>

<span data-ttu-id="64c4e-534">En el ejemplo siguiente se muestra el uso del tipo de `MarkupString` para agregar un bloque de contenido HTML estático a la salida representada de un componente:</span><span class="sxs-lookup"><span data-stu-id="64c4e-534">The following example shows using the `MarkupString` type to add a block of static HTML content to the rendered output of a component:</span></span>

```html
@((MarkupString)_myMarkup)

@code {
    private string _myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a><span data-ttu-id="64c4e-535">Componentes con plantilla</span><span class="sxs-lookup"><span data-stu-id="64c4e-535">Templated components</span></span>

<span data-ttu-id="64c4e-536">Los componentes con plantilla son componentes que aceptan una o varias plantillas de interfaz de usuario como parámetros, que se pueden usar como parte de la lógica de representación del componente.</span><span class="sxs-lookup"><span data-stu-id="64c4e-536">Templated components are components that accept one or more UI templates as parameters, which can then be used as part of the component's rendering logic.</span></span> <span data-ttu-id="64c4e-537">Los componentes con plantilla permiten crear componentes de nivel superior que son más reutilizables que los componentes normales.</span><span class="sxs-lookup"><span data-stu-id="64c4e-537">Templated components allow you to author higher-level components that are more reusable than regular components.</span></span> <span data-ttu-id="64c4e-538">Algunos ejemplos son:</span><span class="sxs-lookup"><span data-stu-id="64c4e-538">A couple of examples include:</span></span>

* <span data-ttu-id="64c4e-539">Componente de tabla que permite a un usuario especificar plantillas para el encabezado, las filas y el pie de página de la tabla.</span><span class="sxs-lookup"><span data-stu-id="64c4e-539">A table component that allows a user to specify templates for the table's header, rows, and footer.</span></span>
* <span data-ttu-id="64c4e-540">Componente de lista que permite a un usuario especificar una plantilla para representar elementos en una lista.</span><span class="sxs-lookup"><span data-stu-id="64c4e-540">A list component that allows a user to specify a template for rendering items in a list.</span></span>

### <a name="template-parameters"></a><span data-ttu-id="64c4e-541">Parámetros de plantilla</span><span class="sxs-lookup"><span data-stu-id="64c4e-541">Template parameters</span></span>

<span data-ttu-id="64c4e-542">Un componente con plantilla se define especificando uno o más parámetros de componente de tipo `RenderFragment` o `RenderFragment<T>`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-542">A templated component is defined by specifying one or more component parameters of type `RenderFragment` or `RenderFragment<T>`.</span></span> <span data-ttu-id="64c4e-543">Un fragmento de representación representa un segmento de interfaz de usuario que se va a representar.</span><span class="sxs-lookup"><span data-stu-id="64c4e-543">A render fragment represents a segment of UI to render.</span></span> <span data-ttu-id="64c4e-544">`RenderFragment<T>` toma un parámetro de tipo que se puede especificar cuando se invoca el fragmento de representación.</span><span class="sxs-lookup"><span data-stu-id="64c4e-544">`RenderFragment<T>` takes a type parameter that can be specified when the render fragment is invoked.</span></span>

<span data-ttu-id="64c4e-545">`TableTemplate` componente:</span><span class="sxs-lookup"><span data-stu-id="64c4e-545">`TableTemplate` component:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/TableTemplate.razor)]

<span data-ttu-id="64c4e-546">Cuando se usa un componente con plantilla, los parámetros de plantilla se pueden especificar utilizando los elementos secundarios que coinciden con los nombres de los parámetros (`TableHeader` y `RowTemplate` en el ejemplo siguiente):</span><span class="sxs-lookup"><span data-stu-id="64c4e-546">When using a templated component, the template parameters can be specified using child elements that match the names of the parameters (`TableHeader` and `RowTemplate` in the following example):</span></span>

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

### <a name="template-context-parameters"></a><span data-ttu-id="64c4e-547">Parámetros de contexto de plantilla</span><span class="sxs-lookup"><span data-stu-id="64c4e-547">Template context parameters</span></span>

<span data-ttu-id="64c4e-548">Los argumentos de los componentes de tipo `RenderFragment<T>` pasados como elementos tienen un parámetro implícito denominado `context` (por ejemplo, en el ejemplo de código anterior, `@context.PetId`), pero puede cambiar el nombre del parámetro mediante el atributo `Context` en el elemento secundario.</span><span class="sxs-lookup"><span data-stu-id="64c4e-548">Component arguments of type `RenderFragment<T>` passed as elements have an implicit parameter named `context` (for example from the preceding code sample, `@context.PetId`), but you can change the parameter name using the `Context` attribute on the child element.</span></span> <span data-ttu-id="64c4e-549">En el ejemplo siguiente, el atributo `Context` del elemento `RowTemplate` especifica el parámetro `pet`:</span><span class="sxs-lookup"><span data-stu-id="64c4e-549">In the following example, the `RowTemplate` element's `Context` attribute specifies the `pet` parameter:</span></span>

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

<span data-ttu-id="64c4e-550">También puede especificar el atributo de `Context` en el elemento de componente.</span><span class="sxs-lookup"><span data-stu-id="64c4e-550">Alternatively, you can specify the `Context` attribute on the component element.</span></span> <span data-ttu-id="64c4e-551">El atributo de `Context` especificado se aplica a todos los parámetros de plantilla especificados.</span><span class="sxs-lookup"><span data-stu-id="64c4e-551">The specified `Context` attribute applies to all specified template parameters.</span></span> <span data-ttu-id="64c4e-552">Esto puede ser útil si desea especificar el nombre del parámetro de contenido para el contenido secundario implícito (sin ningún elemento secundario de ajuste).</span><span class="sxs-lookup"><span data-stu-id="64c4e-552">This can be useful when you want to specify the content parameter name for implicit child content (without any wrapping child element).</span></span> <span data-ttu-id="64c4e-553">En el ejemplo siguiente, el atributo `Context` aparece en el elemento `TableTemplate` y se aplica a todos los parámetros de plantilla:</span><span class="sxs-lookup"><span data-stu-id="64c4e-553">In the following example, the `Context` attribute appears on the `TableTemplate` element and applies to all template parameters:</span></span>

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

### <a name="generic-typed-components"></a><span data-ttu-id="64c4e-554">Componentes de tipo genérico</span><span class="sxs-lookup"><span data-stu-id="64c4e-554">Generic-typed components</span></span>

<span data-ttu-id="64c4e-555">Los componentes con plantilla suelen tener tipos genéricos.</span><span class="sxs-lookup"><span data-stu-id="64c4e-555">Templated components are often generically typed.</span></span> <span data-ttu-id="64c4e-556">Por ejemplo, se puede usar un componente de `ListViewTemplate` genérico para representar valores de `IEnumerable<T>`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-556">For example, a generic `ListViewTemplate` component can be used to render `IEnumerable<T>` values.</span></span> <span data-ttu-id="64c4e-557">Para definir un componente genérico, utilice la directiva [`@typeparam`](xref:mvc/views/razor#typeparam) para especificar parámetros de tipo:</span><span class="sxs-lookup"><span data-stu-id="64c4e-557">To define a generic component, use the [`@typeparam`](xref:mvc/views/razor#typeparam) directive to specify type parameters:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ListViewTemplate.razor)]

<span data-ttu-id="64c4e-558">Cuando se usan componentes de tipo genérico, el parámetro de tipo se infiere si es posible:</span><span class="sxs-lookup"><span data-stu-id="64c4e-558">When using generic-typed components, the type parameter is inferred if possible:</span></span>

```razor
<ListViewTemplate Items="pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

<span data-ttu-id="64c4e-559">De lo contrario, el parámetro de tipo se debe especificar explícitamente mediante un atributo que coincida con el nombre del parámetro de tipo.</span><span class="sxs-lookup"><span data-stu-id="64c4e-559">Otherwise, the type parameter must be explicitly specified using an attribute that matches the name of the type parameter.</span></span> <span data-ttu-id="64c4e-560">En el ejemplo siguiente, `TItem="Pet"` especifica el tipo:</span><span class="sxs-lookup"><span data-stu-id="64c4e-560">In the following example, `TItem="Pet"` specifies the type:</span></span>

```razor
<ListViewTemplate Items="pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a><span data-ttu-id="64c4e-561">Valores y parámetros en cascada</span><span class="sxs-lookup"><span data-stu-id="64c4e-561">Cascading values and parameters</span></span>

<span data-ttu-id="64c4e-562">En algunos escenarios, no es conveniente fluir los datos de un componente antecesor a un componente descendiente mediante [parámetros de componente](#component-parameters), especialmente cuando hay varios niveles de componentes.</span><span class="sxs-lookup"><span data-stu-id="64c4e-562">In some scenarios, it's inconvenient to flow data from an ancestor component to a descendent component using [component parameters](#component-parameters), especially when there are several component layers.</span></span> <span data-ttu-id="64c4e-563">Los valores y parámetros en cascada solucionan este problema proporcionando un método cómodo para que un componente antecesor proporcione un valor a todos sus componentes descendientes.</span><span class="sxs-lookup"><span data-stu-id="64c4e-563">Cascading values and parameters solve this problem by providing a convenient way for an ancestor component to provide a value to all of its descendent components.</span></span> <span data-ttu-id="64c4e-564">Los valores y parámetros en cascada también proporcionan un enfoque para que los componentes se coordinen.</span><span class="sxs-lookup"><span data-stu-id="64c4e-564">Cascading values and parameters also provide an approach for components to coordinate.</span></span>

### <a name="theme-example"></a><span data-ttu-id="64c4e-565">Ejemplo de tema</span><span class="sxs-lookup"><span data-stu-id="64c4e-565">Theme example</span></span>

<span data-ttu-id="64c4e-566">En el ejemplo siguiente de la aplicación de ejemplo, la clase `ThemeInfo` especifica la información del tema que va a fluir hacia abajo en la jerarquía de componentes para que todos los botones de una parte determinada de la aplicación compartan el mismo estilo.</span><span class="sxs-lookup"><span data-stu-id="64c4e-566">In the following example from the sample app, the `ThemeInfo` class specifies the theme information to flow down the component hierarchy so that all of the buttons within a given part of the app share the same style.</span></span>

<span data-ttu-id="64c4e-567">*UIThemeClasses/ThemeInfo.cs*:</span><span class="sxs-lookup"><span data-stu-id="64c4e-567">*UIThemeClasses/ThemeInfo.cs*:</span></span>

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

<span data-ttu-id="64c4e-568">Un componente antecesor puede proporcionar un valor en cascada mediante el componente de valor en cascada.</span><span class="sxs-lookup"><span data-stu-id="64c4e-568">An ancestor component can provide a cascading value using the Cascading Value component.</span></span> <span data-ttu-id="64c4e-569">El componente `CascadingValue` encapsula un subárbol de la jerarquía de componentes y proporciona un valor único a todos los componentes de ese subárbol.</span><span class="sxs-lookup"><span data-stu-id="64c4e-569">The `CascadingValue` component wraps a subtree of the component hierarchy and supplies a single value to all components within that subtree.</span></span>

<span data-ttu-id="64c4e-570">Por ejemplo, la aplicación de ejemplo especifica información de tema (`ThemeInfo`) en uno de los diseños de la aplicación como parámetro en cascada para todos los componentes que componen el cuerpo del diseño de la propiedad `@Body`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-570">For example, the sample app specifies theme information (`ThemeInfo`) in one of the app's layouts as a cascading parameter for all components that make up the layout body of the `@Body` property.</span></span> <span data-ttu-id="64c4e-571">a `ButtonClass` se le asigna un valor de `btn-success` en el componente de diseño.</span><span class="sxs-lookup"><span data-stu-id="64c4e-571">`ButtonClass` is assigned a value of `btn-success` in the layout component.</span></span> <span data-ttu-id="64c4e-572">Cualquier componente descendiente puede consumir esta propiedad mediante el `ThemeInfo` objeto en cascada.</span><span class="sxs-lookup"><span data-stu-id="64c4e-572">Any descendent component can consume this property through the `ThemeInfo` cascading object.</span></span>

<span data-ttu-id="64c4e-573">`CascadingValuesParametersLayout` componente:</span><span class="sxs-lookup"><span data-stu-id="64c4e-573">`CascadingValuesParametersLayout` component:</span></span>

```razor
@inherits LayoutComponentBase
@using BlazorSample.UIThemeClasses

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3">
            <NavMenu />
        </div>
        <div class="col-sm-9">
            <CascadingValue Value="_theme">
                <div class="content px-4">
                    @Body
                </div>
            </CascadingValue>
        </div>
    </div>
</div>

@code {
    private ThemeInfo _theme = new ThemeInfo { ButtonClass = "btn-success" };
}
```

<span data-ttu-id="64c4e-574">Para usar valores en cascada, los componentes declaran parámetros en cascada mediante el atributo `[CascadingParameter]`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-574">To make use of cascading values, components declare cascading parameters using the `[CascadingParameter]` attribute.</span></span> <span data-ttu-id="64c4e-575">Los valores en cascada se enlazan a los parámetros en cascada por tipo.</span><span class="sxs-lookup"><span data-stu-id="64c4e-575">Cascading values are bound to cascading parameters by type.</span></span>

<span data-ttu-id="64c4e-576">En la aplicación de ejemplo, el componente `CascadingValuesParametersTheme` enlaza el valor en cascada `ThemeInfo` a un parámetro en cascada.</span><span class="sxs-lookup"><span data-stu-id="64c4e-576">In the sample app, the `CascadingValuesParametersTheme` component binds the `ThemeInfo` cascading value to a cascading parameter.</span></span> <span data-ttu-id="64c4e-577">El parámetro se usa para establecer la clase CSS para uno de los botones mostrados por el componente.</span><span class="sxs-lookup"><span data-stu-id="64c4e-577">The parameter is used to set the CSS class for one of the buttons displayed by the component.</span></span>

<span data-ttu-id="64c4e-578">`CascadingValuesParametersTheme` componente:</span><span class="sxs-lookup"><span data-stu-id="64c4e-578">`CascadingValuesParametersTheme` component:</span></span>

```razor
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @_currentCount</p>

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
    private int _currentCount = 0;

    [CascadingParameter]
    protected ThemeInfo ThemeInfo { get; set; }

    private void IncrementCount()
    {
        _currentCount++;
    }
}
```

<span data-ttu-id="64c4e-579">Para poner en cascada varios valores del mismo tipo en el mismo subárbol, proporcione una cadena de `Name` única para cada componente `CascadingValue` y su `CascadingParameter`correspondiente.</span><span class="sxs-lookup"><span data-stu-id="64c4e-579">To cascade multiple values of the same type within the same subtree, provide a unique `Name` string to each `CascadingValue` component and its corresponding `CascadingParameter`.</span></span> <span data-ttu-id="64c4e-580">En el ejemplo siguiente, dos componentes de `CascadingValue` en cascada son instancias diferentes de `MyCascadingType` por nombre:</span><span class="sxs-lookup"><span data-stu-id="64c4e-580">In the following example, two `CascadingValue` components cascade different instances of `MyCascadingType` by name:</span></span>

```razor
<CascadingValue Value=@_parentCascadeParameter1 Name="CascadeParam1">
    <CascadingValue Value=@ParentCascadeParameter2 Name="CascadeParam2">
        ...
    </CascadingValue>
</CascadingValue>

@code {
    private MyCascadingType _parentCascadeParameter1;

    [Parameter]
    public MyCascadingType ParentCascadeParameter2 { get; set; }

    ...
}
```

<span data-ttu-id="64c4e-581">En un componente descendiente, los parámetros en cascada reciben sus valores de los valores en cascada correspondientes del componente antecesor por nombre:</span><span class="sxs-lookup"><span data-stu-id="64c4e-581">In a descendant component, the cascaded parameters receive their values from the corresponding cascaded values in the ancestor component by name:</span></span>

```razor
...

@code {
    [CascadingParameter(Name = "CascadeParam1")]
    protected MyCascadingType ChildCascadeParameter1 { get; set; }
    
    [CascadingParameter(Name = "CascadeParam2")]
    protected MyCascadingType ChildCascadeParameter2 { get; set; }
}
```

### <a name="tabset-example"></a><span data-ttu-id="64c4e-582">Ejemplo de TabSet</span><span class="sxs-lookup"><span data-stu-id="64c4e-582">TabSet example</span></span>

<span data-ttu-id="64c4e-583">Los parámetros en cascada también permiten que los componentes colaboren en la jerarquía de componentes.</span><span class="sxs-lookup"><span data-stu-id="64c4e-583">Cascading parameters also enable components to collaborate across the component hierarchy.</span></span> <span data-ttu-id="64c4e-584">Por ejemplo, considere el siguiente ejemplo de *TabSet* en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="64c4e-584">For example, consider the following *TabSet* example in the sample app.</span></span>

<span data-ttu-id="64c4e-585">La aplicación de ejemplo tiene una interfaz `ITab` que las pestañas implementan:</span><span class="sxs-lookup"><span data-stu-id="64c4e-585">The sample app has an `ITab` interface that tabs implement:</span></span>

[!code-csharp[](common/samples/3.x/BlazorWebAssemblySample/UIInterfaces/ITab.cs)]

<span data-ttu-id="64c4e-586">El componente `CascadingValuesParametersTabSet` usa el componente `TabSet`, que contiene varios componentes `Tab`:</span><span class="sxs-lookup"><span data-stu-id="64c4e-586">The `CascadingValuesParametersTabSet` component uses the `TabSet` component, which contains several `Tab` components:</span></span>

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

<span data-ttu-id="64c4e-587">Los componentes de `Tab` secundarios no se pasan explícitamente como parámetros a la `TabSet`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-587">The child `Tab` components aren't explicitly passed as parameters to the `TabSet`.</span></span> <span data-ttu-id="64c4e-588">En su lugar, los componentes secundarios `Tab` forman parte del contenido secundario del `TabSet`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-588">Instead, the child `Tab` components are part of the child content of the `TabSet`.</span></span> <span data-ttu-id="64c4e-589">Sin embargo, el `TabSet` todavía necesita saber sobre cada componente `Tab` para que pueda representar los encabezados y la pestaña activa. Para habilitar esta coordinación sin necesidad de código adicional, el componente de `TabSet` *puede proporcionarse como un valor en cascada* que los componentes de `Tab` descendientes recogen.</span><span class="sxs-lookup"><span data-stu-id="64c4e-589">However, the `TabSet` still needs to know about each `Tab` component so that it can render the headers and the active tab. To enable this coordination without requiring additional code, the `TabSet` component *can provide itself as a cascading value* that is then picked up by the descendent `Tab` components.</span></span>

<span data-ttu-id="64c4e-590">`TabSet` componente:</span><span class="sxs-lookup"><span data-stu-id="64c4e-590">`TabSet` component:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/TabSet.razor)]

<span data-ttu-id="64c4e-591">Los componentes de `Tab` descendientes capturan el `TabSet` contenedor como un parámetro en cascada, por lo que los componentes de `Tab` se agregan a los `TabSet` y coordenadas en los que la pestaña está activa.</span><span class="sxs-lookup"><span data-stu-id="64c4e-591">The descendent `Tab` components capture the containing `TabSet` as a cascading parameter, so the `Tab` components add themselves to the `TabSet` and coordinate on which tab is active.</span></span>

<span data-ttu-id="64c4e-592">`Tab` componente:</span><span class="sxs-lookup"><span data-stu-id="64c4e-592">`Tab` component:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/Tab.razor)]

## <a name="razor-templates"></a><span data-ttu-id="64c4e-593">Plantillas de Razor</span><span class="sxs-lookup"><span data-stu-id="64c4e-593">Razor templates</span></span>

<span data-ttu-id="64c4e-594">Los fragmentos de representación se pueden definir mediante la sintaxis de plantilla de Razor.</span><span class="sxs-lookup"><span data-stu-id="64c4e-594">Render fragments can be defined using Razor template syntax.</span></span> <span data-ttu-id="64c4e-595">Las plantillas de Razor son una manera de definir un fragmento de la interfaz de usuario y suponer el siguiente formato:</span><span class="sxs-lookup"><span data-stu-id="64c4e-595">Razor templates are a way to define a UI snippet and assume the following format:</span></span>

```razor
@<{HTML tag}>...</{HTML tag}>
```

<span data-ttu-id="64c4e-596">En el ejemplo siguiente se muestra cómo especificar los valores de `RenderFragment` y `RenderFragment<T>` y las plantillas de representación directamente en un componente.</span><span class="sxs-lookup"><span data-stu-id="64c4e-596">The following example illustrates how to specify `RenderFragment` and `RenderFragment<T>` values and render templates directly in a component.</span></span> <span data-ttu-id="64c4e-597">Los fragmentos de representación también se pueden pasar como argumentos a [componentes con plantilla](#templated-components).</span><span class="sxs-lookup"><span data-stu-id="64c4e-597">Render fragments can also be passed as arguments to [templated components](#templated-components).</span></span>

```razor
@_timeTemplate

@_petTemplate(new Pet { Name = "Rex" })

@code {
    private RenderFragment _timeTemplate = @<p>The time is @DateTime.Now.</p>;
    private RenderFragment<Pet> _petTemplate = (pet) => @<p>Pet: @pet.Name</p>;

    private class Pet
    {
        public string Name { get; set; }
    }
}
```

<span data-ttu-id="64c4e-598">Salida representada del código anterior:</span><span class="sxs-lookup"><span data-stu-id="64c4e-598">Rendered output of the preceding code:</span></span>

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Pet: Rex</p>
```

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="64c4e-599">Lógica de RenderTreeBuilder manual</span><span class="sxs-lookup"><span data-stu-id="64c4e-599">Manual RenderTreeBuilder logic</span></span>

<span data-ttu-id="64c4e-600">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` proporciona métodos para manipular componentes y elementos, incluida la creación manual de componentes C# en el código.</span><span class="sxs-lookup"><span data-stu-id="64c4e-600">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="64c4e-601">El uso de `RenderTreeBuilder` para crear componentes es un escenario avanzado.</span><span class="sxs-lookup"><span data-stu-id="64c4e-601">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="64c4e-602">Un componente con formato incorrecto (por ejemplo, una etiqueta de marcado sin cerrar) puede dar lugar a un comportamiento indefinido.</span><span class="sxs-lookup"><span data-stu-id="64c4e-602">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="64c4e-603">Tenga en cuenta el siguiente componente de `PetDetails`, que se puede integrar manualmente en otro componente:</span><span class="sxs-lookup"><span data-stu-id="64c4e-603">Consider the following `PetDetails` component, which can be manually built into another component:</span></span>

```razor
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="64c4e-604">En el ejemplo siguiente, el bucle del método `CreateComponent` genera tres componentes `PetDetails`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-604">In the following example, the loop in the `CreateComponent` method generates three `PetDetails` components.</span></span> <span data-ttu-id="64c4e-605">Al llamar a métodos `RenderTreeBuilder` para crear los componentes (`OpenComponent` y `AddAttribute`), los números de secuencia son números de línea de código fuente.</span><span class="sxs-lookup"><span data-stu-id="64c4e-605">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="64c4e-606">El algoritmo de diferencia más increíble se basa en los números de secuencia correspondientes a líneas de código distintas, no a invocaciones de llamada distintas.</span><span class="sxs-lookup"><span data-stu-id="64c4e-606">The Blazor difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="64c4e-607">Al crear un componente con métodos de `RenderTreeBuilder`, codifique los argumentos para los números de secuencia.</span><span class="sxs-lookup"><span data-stu-id="64c4e-607">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> <span data-ttu-id="64c4e-608">**El uso de un cálculo o un contador para generar el número de secuencia puede dar lugar a un rendimiento deficiente.**</span><span class="sxs-lookup"><span data-stu-id="64c4e-608">**Using a calculation or counter to generate the sequence number can lead to poor performance.**</span></span> <span data-ttu-id="64c4e-609">Para obtener más información, vea la sección [números de secuencia relacionados con números de línea de código y no orden de ejecución](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) .</span><span class="sxs-lookup"><span data-stu-id="64c4e-609">For more information, see the [Sequence numbers relate to code line numbers and not execution order](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) section.</span></span>

<span data-ttu-id="64c4e-610">`BuiltContent` componente:</span><span class="sxs-lookup"><span data-stu-id="64c4e-610">`BuiltContent` component:</span></span>

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
> <span data-ttu-id="64c4e-611">Los tipos de `Microsoft.AspNetCore.Components.RenderTree` permiten el procesamiento de los *resultados* de las operaciones de representación.</span><span class="sxs-lookup"><span data-stu-id="64c4e-611">The types in `Microsoft.AspNetCore.Components.RenderTree` allow processing of the *results* of rendering operations.</span></span> <span data-ttu-id="64c4e-612">Estos son los detalles internos de la implementación de la plataforma más extraordinaria.</span><span class="sxs-lookup"><span data-stu-id="64c4e-612">These are internal details of the Blazor framework implementation.</span></span> <span data-ttu-id="64c4e-613">Estos tipos se deben considerar *inestables* y estar sujetos a cambios en futuras versiones.</span><span class="sxs-lookup"><span data-stu-id="64c4e-613">These types should be considered *unstable* and subject to change in future releases.</span></span>

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a><span data-ttu-id="64c4e-614">Los números de secuencia se relacionan con los números de línea de código y no el orden de ejecución</span><span class="sxs-lookup"><span data-stu-id="64c4e-614">Sequence numbers relate to code line numbers and not execution order</span></span>

<span data-ttu-id="64c4e-615">Los archivos `.razor` más increíbles siempre se compilan.</span><span class="sxs-lookup"><span data-stu-id="64c4e-615">Blazor `.razor` files are always compiled.</span></span> <span data-ttu-id="64c4e-616">Esta es potencialmente una gran ventaja para `.razor` porque el paso de compilación se puede usar para insertar información que mejoran el rendimiento de las aplicaciones en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="64c4e-616">This is potentially a great advantage for `.razor` because the compile step can be used to inject information that improve app performance at runtime.</span></span>

<span data-ttu-id="64c4e-617">Un ejemplo clave de estas mejoras implican *los números de secuencia*.</span><span class="sxs-lookup"><span data-stu-id="64c4e-617">A key example of these improvements involve *sequence numbers*.</span></span> <span data-ttu-id="64c4e-618">Los números de secuencia indican al tiempo de ejecución los resultados de los que proceden las líneas de código distintas y ordenadas.</span><span class="sxs-lookup"><span data-stu-id="64c4e-618">Sequence numbers indicate to the runtime which outputs came from which distinct and ordered lines of code.</span></span> <span data-ttu-id="64c4e-619">El motor en tiempo de ejecución utiliza esta información para generar diferencias de árbol eficientes en el tiempo lineal, que es mucho más rápido de lo que suele ser posible para un algoritmo de comparación de árboles generales.</span><span class="sxs-lookup"><span data-stu-id="64c4e-619">The runtime uses this information to generate efficient tree diffs in linear time, which is far faster than is normally possible for a general tree diff algorithm.</span></span>

<span data-ttu-id="64c4e-620">Considere el siguiente archivo de componente de Razor ( *. Razor*):</span><span class="sxs-lookup"><span data-stu-id="64c4e-620">Consider the following Razor component (*.razor*) file:</span></span>

```razor
@if (someFlag)
{
    <text>First</text>
}

Second
```

<span data-ttu-id="64c4e-621">El código anterior se compila en algo similar a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="64c4e-621">The preceding code compiles to something like the following:</span></span>

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

<span data-ttu-id="64c4e-622">Cuando el código se ejecuta por primera vez, si se `true``someFlag`, el generador recibe:</span><span class="sxs-lookup"><span data-stu-id="64c4e-622">When the code executes for the first time, if `someFlag` is `true`, the builder receives:</span></span>

| <span data-ttu-id="64c4e-623">Secuencia</span><span class="sxs-lookup"><span data-stu-id="64c4e-623">Sequence</span></span> | <span data-ttu-id="64c4e-624">Tipo de</span><span class="sxs-lookup"><span data-stu-id="64c4e-624">Type</span></span>      | <span data-ttu-id="64c4e-625">Datos</span><span class="sxs-lookup"><span data-stu-id="64c4e-625">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="64c4e-626">0</span><span class="sxs-lookup"><span data-stu-id="64c4e-626">0</span></span>        | <span data-ttu-id="64c4e-627">Nodo de texto</span><span class="sxs-lookup"><span data-stu-id="64c4e-627">Text node</span></span> | <span data-ttu-id="64c4e-628">First</span><span class="sxs-lookup"><span data-stu-id="64c4e-628">First</span></span>  |
| <span data-ttu-id="64c4e-629">1</span><span class="sxs-lookup"><span data-stu-id="64c4e-629">1</span></span>        | <span data-ttu-id="64c4e-630">Nodo de texto</span><span class="sxs-lookup"><span data-stu-id="64c4e-630">Text node</span></span> | <span data-ttu-id="64c4e-631">Second</span><span class="sxs-lookup"><span data-stu-id="64c4e-631">Second</span></span> |

<span data-ttu-id="64c4e-632">Imagine que `someFlag` se `false`y que el marcado se representará de nuevo.</span><span class="sxs-lookup"><span data-stu-id="64c4e-632">Imagine that `someFlag` becomes `false`, and the markup is rendered again.</span></span> <span data-ttu-id="64c4e-633">Esta vez, el generador recibe:</span><span class="sxs-lookup"><span data-stu-id="64c4e-633">This time, the builder receives:</span></span>

| <span data-ttu-id="64c4e-634">Secuencia</span><span class="sxs-lookup"><span data-stu-id="64c4e-634">Sequence</span></span> | <span data-ttu-id="64c4e-635">Tipo de</span><span class="sxs-lookup"><span data-stu-id="64c4e-635">Type</span></span>       | <span data-ttu-id="64c4e-636">Datos</span><span class="sxs-lookup"><span data-stu-id="64c4e-636">Data</span></span>   |
| :------: | ---------- | :----: |
| <span data-ttu-id="64c4e-637">1</span><span class="sxs-lookup"><span data-stu-id="64c4e-637">1</span></span>        | <span data-ttu-id="64c4e-638">Nodo de texto</span><span class="sxs-lookup"><span data-stu-id="64c4e-638">Text node</span></span>  | <span data-ttu-id="64c4e-639">Second</span><span class="sxs-lookup"><span data-stu-id="64c4e-639">Second</span></span> |

<span data-ttu-id="64c4e-640">Cuando el tiempo de ejecución realiza una comparación, ve que se quitó el elemento en la secuencia `0`, por lo que genera el siguiente *script de edición*trivial:</span><span class="sxs-lookup"><span data-stu-id="64c4e-640">When the runtime performs a diff, it sees that the item at sequence `0` was removed, so it generates the following trivial *edit script*:</span></span>

* <span data-ttu-id="64c4e-641">Quitar el primer nodo de texto.</span><span class="sxs-lookup"><span data-stu-id="64c4e-641">Remove the first text node.</span></span>

#### <a name="what-goes-wrong-if-you-generate-sequence-numbers-programmatically"></a><span data-ttu-id="64c4e-642">Qué sucede si se generan números de secuencia mediante programación</span><span class="sxs-lookup"><span data-stu-id="64c4e-642">What goes wrong if you generate sequence numbers programmatically</span></span>

<span data-ttu-id="64c4e-643">Imagine que escribió la siguiente lógica del generador de árboles de representación:</span><span class="sxs-lookup"><span data-stu-id="64c4e-643">Imagine instead that you wrote the following render tree builder logic:</span></span>

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

<span data-ttu-id="64c4e-644">Ahora, el primer resultado es:</span><span class="sxs-lookup"><span data-stu-id="64c4e-644">Now, the first output is:</span></span>

| <span data-ttu-id="64c4e-645">Secuencia</span><span class="sxs-lookup"><span data-stu-id="64c4e-645">Sequence</span></span> | <span data-ttu-id="64c4e-646">Tipo de</span><span class="sxs-lookup"><span data-stu-id="64c4e-646">Type</span></span>      | <span data-ttu-id="64c4e-647">Datos</span><span class="sxs-lookup"><span data-stu-id="64c4e-647">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="64c4e-648">0</span><span class="sxs-lookup"><span data-stu-id="64c4e-648">0</span></span>        | <span data-ttu-id="64c4e-649">Nodo de texto</span><span class="sxs-lookup"><span data-stu-id="64c4e-649">Text node</span></span> | <span data-ttu-id="64c4e-650">First</span><span class="sxs-lookup"><span data-stu-id="64c4e-650">First</span></span>  |
| <span data-ttu-id="64c4e-651">1</span><span class="sxs-lookup"><span data-stu-id="64c4e-651">1</span></span>        | <span data-ttu-id="64c4e-652">Nodo de texto</span><span class="sxs-lookup"><span data-stu-id="64c4e-652">Text node</span></span> | <span data-ttu-id="64c4e-653">Second</span><span class="sxs-lookup"><span data-stu-id="64c4e-653">Second</span></span> |

<span data-ttu-id="64c4e-654">Este resultado es idéntico al caso anterior, por lo que no existe ningún problema negativo.</span><span class="sxs-lookup"><span data-stu-id="64c4e-654">This outcome is identical to the prior case, so no negative issues exist.</span></span> <span data-ttu-id="64c4e-655">`someFlag` se `false` en la segunda representación y el resultado es:</span><span class="sxs-lookup"><span data-stu-id="64c4e-655">`someFlag` is `false` on the second rendering, and the output is:</span></span>

| <span data-ttu-id="64c4e-656">Secuencia</span><span class="sxs-lookup"><span data-stu-id="64c4e-656">Sequence</span></span> | <span data-ttu-id="64c4e-657">Tipo de</span><span class="sxs-lookup"><span data-stu-id="64c4e-657">Type</span></span>      | <span data-ttu-id="64c4e-658">Datos</span><span class="sxs-lookup"><span data-stu-id="64c4e-658">Data</span></span>   |
| :------: | --------- | ------ |
| <span data-ttu-id="64c4e-659">0</span><span class="sxs-lookup"><span data-stu-id="64c4e-659">0</span></span>        | <span data-ttu-id="64c4e-660">Nodo de texto</span><span class="sxs-lookup"><span data-stu-id="64c4e-660">Text node</span></span> | <span data-ttu-id="64c4e-661">Second</span><span class="sxs-lookup"><span data-stu-id="64c4e-661">Second</span></span> |

<span data-ttu-id="64c4e-662">Esta vez, el algoritmo de comparación ve que se han producido *dos* cambios y el algoritmo genera el siguiente script de edición:</span><span class="sxs-lookup"><span data-stu-id="64c4e-662">This time, the diff algorithm sees that *two* changes have occurred, and the algorithm generates the following edit script:</span></span>

* <span data-ttu-id="64c4e-663">Cambie el valor del primer nodo de texto a `Second`.</span><span class="sxs-lookup"><span data-stu-id="64c4e-663">Change the value of the first text node to `Second`.</span></span>
* <span data-ttu-id="64c4e-664">Quite el segundo nodo de texto.</span><span class="sxs-lookup"><span data-stu-id="64c4e-664">Remove the second text node.</span></span>

<span data-ttu-id="64c4e-665">La generación de los números de secuencia ha perdido toda la información útil sobre el lugar en el que las bifurcaciones de `if/else` y los bucles estaban presentes en el código original.</span><span class="sxs-lookup"><span data-stu-id="64c4e-665">Generating the sequence numbers has lost all the useful information about where the `if/else` branches and loops were present in the original code.</span></span> <span data-ttu-id="64c4e-666">Esto da como resultado una diferencia **dos veces mayor que** antes.</span><span class="sxs-lookup"><span data-stu-id="64c4e-666">This results in a diff **twice as long** as before.</span></span>

<span data-ttu-id="64c4e-667">Este es un ejemplo trivial.</span><span class="sxs-lookup"><span data-stu-id="64c4e-667">This is a trivial example.</span></span> <span data-ttu-id="64c4e-668">En casos más realistas con estructuras complejas y profundamente anidadas, y especialmente con bucles, el costo de rendimiento es más grave.</span><span class="sxs-lookup"><span data-stu-id="64c4e-668">In more realistic cases with complex and deeply nested structures, and especially with loops, the performance cost is more severe.</span></span> <span data-ttu-id="64c4e-669">En lugar de identificar inmediatamente qué bloques o ramas de bucle se han insertado o quitado, el algoritmo de comparación tiene que recurse profundamente en los árboles de representación y, normalmente, compilar scripts de edición mucho más largos porque está informada sobre cómo las estructuras antiguas y nuevas se relacionan entre sí.</span><span class="sxs-lookup"><span data-stu-id="64c4e-669">Instead of immediately identifying which loop blocks or branches have been inserted or removed, the diff algorithm has to recurse deeply into the render trees and usually build far longer edit scripts because it is misinformed about how the old and new structures relate to each other.</span></span>

#### <a name="guidance-and-conclusions"></a><span data-ttu-id="64c4e-670">Instrucciones y conclusiones</span><span class="sxs-lookup"><span data-stu-id="64c4e-670">Guidance and conclusions</span></span>

* <span data-ttu-id="64c4e-671">El rendimiento de la aplicación se ve afectado si los números de secuencia se generan dinámicamente.</span><span class="sxs-lookup"><span data-stu-id="64c4e-671">App performance suffers if sequence numbers are generated dynamically.</span></span>
* <span data-ttu-id="64c4e-672">El marco no puede crear sus propios números de secuencia automáticamente en tiempo de ejecución porque la información necesaria no existe a menos que se Capture en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="64c4e-672">The framework can't create its own sequence numbers automatically at runtime because the necessary information doesn't exist unless it's captured at compile time.</span></span>
* <span data-ttu-id="64c4e-673">No escriba grandes bloques de lógica de `RenderTreeBuilder` implementada de forma manual.</span><span class="sxs-lookup"><span data-stu-id="64c4e-673">Don't write long blocks of manually-implemented `RenderTreeBuilder` logic.</span></span> <span data-ttu-id="64c4e-674">Prefiere `.razor` archivos y permitir que el compilador trate los números de secuencia.</span><span class="sxs-lookup"><span data-stu-id="64c4e-674">Prefer `.razor` files and allow the compiler to deal with the sequence numbers.</span></span> <span data-ttu-id="64c4e-675">Si no puede evitar la lógica de `RenderTreeBuilder` manual, divida bloques largos de código en fragmentos más pequeños encapsulados en `OpenRegion`/`CloseRegion` llamadas.</span><span class="sxs-lookup"><span data-stu-id="64c4e-675">If you're unable to avoid manual `RenderTreeBuilder` logic, split long blocks of code into smaller pieces wrapped in `OpenRegion`/`CloseRegion` calls.</span></span> <span data-ttu-id="64c4e-676">Cada región tiene su propio espacio independiente de números de secuencia, por lo que puede reiniciar desde cero (o cualquier otro número arbitrario) dentro de cada región.</span><span class="sxs-lookup"><span data-stu-id="64c4e-676">Each region has its own separate space of sequence numbers, so you can restart from zero (or any other arbitrary number) inside each region.</span></span>
* <span data-ttu-id="64c4e-677">Si los números de secuencia están codificados, el algoritmo de comparación solo requiere que los números de secuencia aumenten en valor.</span><span class="sxs-lookup"><span data-stu-id="64c4e-677">If sequence numbers are hardcoded, the diff algorithm only requires that sequence numbers increase in value.</span></span> <span data-ttu-id="64c4e-678">El valor inicial y los huecos son irrelevantes.</span><span class="sxs-lookup"><span data-stu-id="64c4e-678">The initial value and gaps are irrelevant.</span></span> <span data-ttu-id="64c4e-679">Una opción legítima es usar el número de línea de código como el número de secuencia, o comenzar a partir de cero y aumentar por unos o cientos (o cualquier intervalo preferido).</span><span class="sxs-lookup"><span data-stu-id="64c4e-679">One legitimate option is to use the code line number as the sequence number, or start from zero and increase by ones or hundreds (or any preferred interval).</span></span> 
* Blazor<span data-ttu-id="64c4e-680"> usa los números de secuencia, mientras que otros marcos de interfaz de usuario de comparación de árboles no los utilizan.</span><span class="sxs-lookup"><span data-stu-id="64c4e-680"> uses sequence numbers, while other tree-diffing UI frameworks don't use them.</span></span> <span data-ttu-id="64c4e-681">La diferenciación es mucho más rápida cuando se utilizan números de secuencia y Blazor tiene la ventaja de un paso de compilación que trata los números de secuencia automáticamente para desarrolladores que crean archivos *. Razor* .</span><span class="sxs-lookup"><span data-stu-id="64c4e-681">Diffing is far faster when sequence numbers are used, and Blazor has the advantage of a compile step that deals with sequence numbers automatically for developers authoring *.razor* files.</span></span>

## <a name="localization"></a><span data-ttu-id="64c4e-682">Localización</span><span class="sxs-lookup"><span data-stu-id="64c4e-682">Localization</span></span>

<span data-ttu-id="64c4e-683">las aplicaciones de Blazor Server se localizan mediante [middleware de localización](xref:fundamentals/localization#localization-middleware).</span><span class="sxs-lookup"><span data-stu-id="64c4e-683">Blazor Server apps are localized using [Localization Middleware](xref:fundamentals/localization#localization-middleware).</span></span> <span data-ttu-id="64c4e-684">El middleware selecciona la referencia cultural adecuada para los usuarios que solicitan recursos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="64c4e-684">The middleware selects the appropriate culture for users requesting resources from the app.</span></span>

<span data-ttu-id="64c4e-685">La referencia cultural se puede establecer utilizando uno de los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="64c4e-685">The culture can be set using one of the following approaches:</span></span>

* [<span data-ttu-id="64c4e-686">Cookies</span><span class="sxs-lookup"><span data-stu-id="64c4e-686">Cookies</span></span>](#cookies)
* [<span data-ttu-id="64c4e-687">Proporcionar la interfaz de usuario para elegir la referencia cultural</span><span class="sxs-lookup"><span data-stu-id="64c4e-687">Provide UI to choose the culture</span></span>](#provide-ui-to-choose-the-culture)

<span data-ttu-id="64c4e-688">Para obtener más información y ejemplos, vea <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="64c4e-688">For more information and examples, see <xref:fundamentals/localization>.</span></span>

### <a name="configure-the-linker-for-internationalization-opno-locblazor-webassembly"></a><span data-ttu-id="64c4e-689">Configurar el enlazador para la internacionalización (Blazor webassembly)</span><span class="sxs-lookup"><span data-stu-id="64c4e-689">Configure the linker for internationalization (Blazor WebAssembly)</span></span>

<span data-ttu-id="64c4e-690">De forma predeterminada, la configuración del enlazador de Blazor para aplicaciones WebAssembly de Blazor quita información de internacionalización, excepto para las configuraciones regionales solicitadas de forma explícita.</span><span class="sxs-lookup"><span data-stu-id="64c4e-690">By default, Blazor's linker configuration for Blazor WebAssembly apps strips out internationalization information except for locales explicitly requested.</span></span> <span data-ttu-id="64c4e-691">Para obtener más información e instrucciones sobre cómo controlar el comportamiento del enlazador, vea <xref:host-and-deploy/blazor/configure-linker#configure-the-linker-for-internationalization>.</span><span class="sxs-lookup"><span data-stu-id="64c4e-691">For more information and guidance on controlling the linker's behavior, see <xref:host-and-deploy/blazor/configure-linker#configure-the-linker-for-internationalization>.</span></span>

### <a name="cookies"></a><span data-ttu-id="64c4e-692">Cookies</span><span class="sxs-lookup"><span data-stu-id="64c4e-692">Cookies</span></span>

<span data-ttu-id="64c4e-693">Una cookie de referencia cultural de localización puede conservar la referencia cultural del usuario.</span><span class="sxs-lookup"><span data-stu-id="64c4e-693">A localization culture cookie can persist the user's culture.</span></span> <span data-ttu-id="64c4e-694">La cookie se crea mediante el método `OnGet` de la página host de la aplicación (*pages/host. cshtml. CS*).</span><span class="sxs-lookup"><span data-stu-id="64c4e-694">The cookie is created by the `OnGet` method of the app's host page (*Pages/Host.cshtml.cs*).</span></span> <span data-ttu-id="64c4e-695">El middleware de localización lee la cookie en solicitudes posteriores para establecer la referencia cultural del usuario.</span><span class="sxs-lookup"><span data-stu-id="64c4e-695">The Localization Middleware reads the cookie on subsequent requests to set the user's culture.</span></span> 

<span data-ttu-id="64c4e-696">El uso de una cookie garantiza que la conexión de WebSocket puede propagar correctamente la referencia cultural.</span><span class="sxs-lookup"><span data-stu-id="64c4e-696">Use of a cookie ensures that the WebSocket connection can correctly propagate the culture.</span></span> <span data-ttu-id="64c4e-697">Si los esquemas de localización se basan en la ruta de acceso URL o la cadena de consulta, es posible que el esquema no funcione con WebSockets, por lo que no se puede conservar la referencia cultural.</span><span class="sxs-lookup"><span data-stu-id="64c4e-697">If localization schemes are based on the URL path or query string, the scheme might not be able to work with WebSockets, thus fail to persist the culture.</span></span> <span data-ttu-id="64c4e-698">Por lo tanto, el uso de una cookie de referencia cultural de localización es el enfoque recomendado.</span><span class="sxs-lookup"><span data-stu-id="64c4e-698">Therefore, use of a localization culture cookie is the recommended approach.</span></span>

<span data-ttu-id="64c4e-699">Cualquier técnica se puede usar para asignar una referencia cultural si la referencia cultural se conserva en una cookie de localización.</span><span class="sxs-lookup"><span data-stu-id="64c4e-699">Any technique can be used to assign a culture if the culture is persisted in a localization cookie.</span></span> <span data-ttu-id="64c4e-700">Si la aplicación ya tiene un esquema de localización establecido para ASP.NET Core del lado servidor, siga usando la infraestructura de localización existente de la aplicación y establezca la cookie de la cultura de localización en el esquema de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="64c4e-700">If the app already has an established localization scheme for server-side ASP.NET Core, continue to use the app's existing localization infrastructure and set the localization culture cookie within the app's scheme.</span></span>

<span data-ttu-id="64c4e-701">En el ejemplo siguiente se muestra cómo establecer la referencia cultural actual en una cookie que puede ser leída por el middleware de localización.</span><span class="sxs-lookup"><span data-stu-id="64c4e-701">The following example shows how to set the current culture in a cookie that can be read by the Localization Middleware.</span></span> <span data-ttu-id="64c4e-702">Cree un archivo *pages/host. cshtml. CS* con el siguiente contenido en la aplicación de Blazor Server:</span><span class="sxs-lookup"><span data-stu-id="64c4e-702">Create a *Pages/Host.cshtml.cs* file with the following contents in the Blazor Server app:</span></span>

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

<span data-ttu-id="64c4e-703">La localización se controla en la aplicación:</span><span class="sxs-lookup"><span data-stu-id="64c4e-703">Localization is handled in the app:</span></span>

1. <span data-ttu-id="64c4e-704">El explorador envía una solicitud HTTP inicial a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="64c4e-704">The browser sends an initial HTTP request to the app.</span></span>
1. <span data-ttu-id="64c4e-705">El middleware de localización asigna la referencia cultural.</span><span class="sxs-lookup"><span data-stu-id="64c4e-705">The culture is assigned by the Localization Middleware.</span></span>
1. <span data-ttu-id="64c4e-706">El método `OnGet` en *_Host. cshtml. CS* conserva la referencia cultural en una cookie como parte de la respuesta.</span><span class="sxs-lookup"><span data-stu-id="64c4e-706">The `OnGet` method in *_Host.cshtml.cs* persists the culture in a cookie as part of the response.</span></span>
1. <span data-ttu-id="64c4e-707">El explorador abre una conexión WebSocket para crear una sesión interactiva de Blazor Server.</span><span class="sxs-lookup"><span data-stu-id="64c4e-707">The browser opens a WebSocket connection to create an interactive Blazor Server session.</span></span>
1. <span data-ttu-id="64c4e-708">El middleware de localización lee la cookie y asigna la referencia cultural.</span><span class="sxs-lookup"><span data-stu-id="64c4e-708">The Localization Middleware reads the cookie and assigns the culture.</span></span>
1. <span data-ttu-id="64c4e-709">La sesión del servidor de Blazor comienza con la referencia cultural correcta.</span><span class="sxs-lookup"><span data-stu-id="64c4e-709">The Blazor Server session begins with the correct culture.</span></span>

### <a name="provide-ui-to-choose-the-culture"></a><span data-ttu-id="64c4e-710">Proporcionar la interfaz de usuario para elegir la referencia cultural</span><span class="sxs-lookup"><span data-stu-id="64c4e-710">Provide UI to choose the culture</span></span>

<span data-ttu-id="64c4e-711">Para proporcionar una interfaz de usuario que permita a los usuarios seleccionar una referencia cultural, se recomienda un *enfoque basado en redirección* .</span><span class="sxs-lookup"><span data-stu-id="64c4e-711">To provide UI to allow a user to select a culture, a *redirect-based approach* is recommended.</span></span> <span data-ttu-id="64c4e-712">El proceso es similar a lo que ocurre en una aplicación web cuando un usuario intenta acceder a un recurso seguro&mdash;se redirige al usuario a una página de inicio de sesión y, a continuación, se redirige de nuevo al recurso original.</span><span class="sxs-lookup"><span data-stu-id="64c4e-712">The process is similar to what happens in a web app when a user attempts to access a secure resource&mdash;the user is redirected to a sign-in page and then redirected back to the original resource.</span></span> 

<span data-ttu-id="64c4e-713">La aplicación conserva la referencia cultural seleccionada del usuario a través de una redirección a un controlador.</span><span class="sxs-lookup"><span data-stu-id="64c4e-713">The app persists the user's selected culture via a redirect to a controller.</span></span> <span data-ttu-id="64c4e-714">El controlador establece la referencia cultural seleccionada del usuario en una cookie y redirige al usuario de nuevo al URI original.</span><span class="sxs-lookup"><span data-stu-id="64c4e-714">The controller sets the user's selected culture into a cookie and redirects the user back to the original URI.</span></span>

<span data-ttu-id="64c4e-715">Establezca un punto de conexión HTTP en el servidor para establecer la referencia cultural seleccionada del usuario en una cookie y volver a realizar la redirección al URI original:</span><span class="sxs-lookup"><span data-stu-id="64c4e-715">Establish an HTTP endpoint on the server to set the user's selected culture in a cookie and perform the redirect back to the original URI:</span></span>

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
> <span data-ttu-id="64c4e-716">Use el resultado de la acción `LocalRedirect` para evitar ataques de redireccionamiento abierto.</span><span class="sxs-lookup"><span data-stu-id="64c4e-716">Use the `LocalRedirect` action result to prevent open redirect attacks.</span></span> <span data-ttu-id="64c4e-717">Para obtener más información, vea <xref:security/preventing-open-redirects>.</span><span class="sxs-lookup"><span data-stu-id="64c4e-717">For more information, see <xref:security/preventing-open-redirects>.</span></span>

<span data-ttu-id="64c4e-718">El siguiente componente muestra un ejemplo de cómo realizar la redirección inicial cuando el usuario selecciona una referencia cultural:</span><span class="sxs-lookup"><span data-stu-id="64c4e-718">The following component shows an example of how to perform the initial redirection when the user selects a culture:</span></span>

```razor
@inject NavigationManager NavigationManager

<h3>Select your language</h3>

<select @onchange="OnSelected">
    <option>Select...</option>
    <option value="en-US">English</option>
    <option value="fr-FR">Français</option>
</select>

@code {
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

### <a name="use-net-localization-scenarios-in-opno-locblazor-apps"></a><span data-ttu-id="64c4e-719">Uso de escenarios de localización de .NET en Blazor aplicaciones</span><span class="sxs-lookup"><span data-stu-id="64c4e-719">Use .NET localization scenarios in Blazor apps</span></span>

<span data-ttu-id="64c4e-720">Dentro de Blazor aplicaciones, están disponibles los siguientes escenarios de localización y globalización de .NET:</span><span class="sxs-lookup"><span data-stu-id="64c4e-720">Inside Blazor apps, the following .NET localization and globalization scenarios are available:</span></span>

* <span data-ttu-id="64c4e-721">. Sistema de recursos de la red</span><span class="sxs-lookup"><span data-stu-id="64c4e-721">.NET's resources system</span></span>
* <span data-ttu-id="64c4e-722">Formato de fecha y número específico de la referencia cultural</span><span class="sxs-lookup"><span data-stu-id="64c4e-722">Culture-specific number and date formatting</span></span>

<span data-ttu-id="64c4e-723">la funcionalidad de `@bind` de Blazorrealiza la globalización en función de la referencia cultural actual del usuario.</span><span class="sxs-lookup"><span data-stu-id="64c4e-723">Blazor's `@bind` functionality performs globalization based on the user's current culture.</span></span> <span data-ttu-id="64c4e-724">Para obtener más información, vea la sección [enlace de datos](#data-binding) .</span><span class="sxs-lookup"><span data-stu-id="64c4e-724">For more information, see the [Data binding](#data-binding) section.</span></span>

<span data-ttu-id="64c4e-725">Actualmente se admite un conjunto limitado de escenarios de localización de ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="64c4e-725">A limited set of ASP.NET Core's localization scenarios are currently supported:</span></span>

* <span data-ttu-id="64c4e-726">`IStringLocalizer<>` *se admite* en las aplicaciones de Blazor.</span><span class="sxs-lookup"><span data-stu-id="64c4e-726">`IStringLocalizer<>` *is supported* in Blazor apps.</span></span>
* <span data-ttu-id="64c4e-727">la localización de `IHtmlLocalizer<>`, `IViewLocalizer<>`y las anotaciones de datos es ASP.NET Core escenarios MVC y **no se admiten** en Blazor aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="64c4e-727">`IHtmlLocalizer<>`, `IViewLocalizer<>`, and Data Annotations localization are ASP.NET Core MVC scenarios and **not supported** in Blazor apps.</span></span>

<span data-ttu-id="64c4e-728">Para obtener más información, vea <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="64c4e-728">For more information, see <xref:fundamentals/localization>.</span></span>

## <a name="scalable-vector-graphics-svg-images"></a><span data-ttu-id="64c4e-729">Imágenes de gráficos vectoriales escalables (SVG)</span><span class="sxs-lookup"><span data-stu-id="64c4e-729">Scalable Vector Graphics (SVG) images</span></span>

<span data-ttu-id="64c4e-730">Dado que Blazor representa imágenes HTML, compatibles con el explorador, incluidas las imágenes SVG (Scalable Vector Graphics) ( *. svg*), se admiten a través de la etiqueta `<img>`:</span><span class="sxs-lookup"><span data-stu-id="64c4e-730">Since Blazor renders HTML, browser-supported images, including Scalable Vector Graphics (SVG) images (*.svg*), are supported via the `<img>` tag:</span></span>

```html
<img alt="Example image" src="some-image.svg" />
```

<span data-ttu-id="64c4e-731">Del mismo modo, las imágenes SVG se admiten en las reglas CSS de un archivo de hoja de estilos ( *. CSS*):</span><span class="sxs-lookup"><span data-stu-id="64c4e-731">Similarly, SVG images are supported in the CSS rules of a stylesheet file (*.css*):</span></span>

```css
.my-element {
    background-image: url("some-image.svg");
}
```

<span data-ttu-id="64c4e-732">Sin embargo, el marcado SVG en línea no se admite en todos los escenarios.</span><span class="sxs-lookup"><span data-stu-id="64c4e-732">However, inline SVG markup isn't supported in all scenarios.</span></span> <span data-ttu-id="64c4e-733">Si coloca una etiqueta de `<svg>` directamente en un archivo de componente ( *. Razor*), se admite la representación de imágenes básica, pero aún no se admiten muchos escenarios avanzados.</span><span class="sxs-lookup"><span data-stu-id="64c4e-733">If you place an `<svg>` tag directly into a component file (*.razor*), basic image rendering is supported but many advanced scenarios aren't yet supported.</span></span> <span data-ttu-id="64c4e-734">Por ejemplo, `<use>` etiquetas no se respetan actualmente y `@bind` no se pueden usar con algunas etiquetas SVG.</span><span class="sxs-lookup"><span data-stu-id="64c4e-734">For example, `<use>` tags aren't currently respected, and `@bind` can't be used with some SVG tags.</span></span> <span data-ttu-id="64c4e-735">Esperamos abordar estas limitaciones en una versión futura.</span><span class="sxs-lookup"><span data-stu-id="64c4e-735">We expect to address these limitations in a future release.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="64c4e-736">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="64c4e-736">Additional resources</span></span>

* <span data-ttu-id="64c4e-737"><xref:security/blazor/server> &ndash; incluye instrucciones sobre la creación de aplicaciones de Blazor Server que deben competir con el agotamiento de recursos.</span><span class="sxs-lookup"><span data-stu-id="64c4e-737"><xref:security/blazor/server> &ndash; Includes guidance on building Blazor Server apps that must contend with resource exhaustion.</span></span>
