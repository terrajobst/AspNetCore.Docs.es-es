---
title: Ciclo de vida de ASP.NET Core Blazor
author: guardrex
description: Aprenda a usar los métodos de ciclo de vida de los componentes de Razor en ASP.NET Core aplicaciones Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 11/26/2019
no-loc:
- Blazor
uid: blazor/lifecycle
ms.openlocfilehash: 1482f6b2147c74b11836e8029401bb8bcb3cdb2d
ms.sourcegitcommit: 0dd224b2b7efca1fda0041b5c3f45080327033f6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2019
ms.locfileid: "74681378"
---
# <a name="aspnet-core-opno-locblazor-lifecycle"></a><span data-ttu-id="94f59-103">Ciclo de vida de ASP.NET Core Blazor</span><span class="sxs-lookup"><span data-stu-id="94f59-103">ASP.NET Core Blazor lifecycle</span></span>

<span data-ttu-id="94f59-104">Por [Luke Latham](https://github.com/guardrex) y [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="94f59-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="94f59-105">El marco de Blazor incluye métodos de ciclo de vida sincrónicos y asincrónicos.</span><span class="sxs-lookup"><span data-stu-id="94f59-105">The Blazor framework includes synchronous and asynchronous lifecycle methods.</span></span> <span data-ttu-id="94f59-106">Invalide los métodos del ciclo de vida para realizar operaciones adicionales en los componentes durante la inicialización y representación de componentes.</span><span class="sxs-lookup"><span data-stu-id="94f59-106">Override lifecycle methods to perform additional operations on components during component initialization and rendering.</span></span>

## <a name="lifecycle-methods"></a><span data-ttu-id="94f59-107">Métodos de ciclo de vida</span><span class="sxs-lookup"><span data-stu-id="94f59-107">Lifecycle methods</span></span>

### <a name="component-initialization-methods"></a><span data-ttu-id="94f59-108">Métodos de inicialización de componentes</span><span class="sxs-lookup"><span data-stu-id="94f59-108">Component initialization methods</span></span>

<span data-ttu-id="94f59-109"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync*> y <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized*> ejecutar código que inicializa un componente.</span><span class="sxs-lookup"><span data-stu-id="94f59-109"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync*> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized*> execute code that initializes a component.</span></span> <span data-ttu-id="94f59-110">Solo se llama a estos métodos una vez cuando se crea una instancia del componente en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="94f59-110">These methods are only called one time when the component is first instantiated.</span></span>

<span data-ttu-id="94f59-111">Para realizar una operación asincrónica, use `OnInitializedAsync` y la palabra clave `await` en la operación:</span><span class="sxs-lookup"><span data-stu-id="94f59-111">To perform an asynchronous operation, use `OnInitializedAsync` and the `await` keyword on the operation:</span></span>

```csharp
protected override async Task OnInitializedAsync()
{
    await ...
}
```

> [!NOTE]
> <span data-ttu-id="94f59-112">El trabajo asincrónico durante la inicialización del componente debe producirse durante el evento del ciclo de vida de `OnInitializedAsync`.</span><span class="sxs-lookup"><span data-stu-id="94f59-112">Asynchronous work during component initialization must occur during the `OnInitializedAsync` lifecycle event.</span></span>

<span data-ttu-id="94f59-113">En el caso de una operación sincrónica, utilice `OnInitialized`:</span><span class="sxs-lookup"><span data-stu-id="94f59-113">For a synchronous operation, use `OnInitialized`:</span></span>

```csharp
protected override void OnInitialized()
{
    ...
}
```

### <a name="before-parameters-are-set"></a><span data-ttu-id="94f59-114">Antes de establecer los parámetros</span><span class="sxs-lookup"><span data-stu-id="94f59-114">Before parameters are set</span></span>

<span data-ttu-id="94f59-115"><xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync*> establece los parámetros proporcionados por el elemento primario del componente en el árbol de representación:</span><span class="sxs-lookup"><span data-stu-id="94f59-115"><xref:Microsoft.AspNetCore.Components.ComponentBase.SetParametersAsync*> sets parameters supplied by the component's parent in the render tree:</span></span>

```csharp
public override async Task SetParametersAsync(ParameterView parameters)
{
    await ...

    await base.SetParametersAsync(parameters);
}
```

<span data-ttu-id="94f59-116"><xref:Microsoft.AspNetCore.Components.ParameterView> contiene el conjunto completo de valores de parámetro cada vez que se llama a `SetParametersAsync`.</span><span class="sxs-lookup"><span data-stu-id="94f59-116"><xref:Microsoft.AspNetCore.Components.ParameterView> contains the entire set of parameter values each time `SetParametersAsync` is called.</span></span>

<span data-ttu-id="94f59-117">La implementación predeterminada de `SetParametersAsync` establece el valor de cada propiedad decorada con el `[Parameter]` o `[CascadingParameter]` atributo que tiene un valor correspondiente en el `ParameterView`.</span><span class="sxs-lookup"><span data-stu-id="94f59-117">The default implementation of `SetParametersAsync` sets the value of each property decorated with the `[Parameter]` or `[CascadingParameter]` attribute that has a corresponding value in the `ParameterView`.</span></span> <span data-ttu-id="94f59-118">Los parámetros que no tienen un valor correspondiente en `ParameterView` se dejan sin cambios.</span><span class="sxs-lookup"><span data-stu-id="94f59-118">Parameters that don't have a corresponding value in `ParameterView` are left unchanged.</span></span>

<span data-ttu-id="94f59-119">Si no se invoca `base.SetParametersAync`, el código personalizado puede interpretar el valor de los parámetros entrantes de cualquier manera que sea necesario.</span><span class="sxs-lookup"><span data-stu-id="94f59-119">If `base.SetParametersAync` isn't invoked, the custom code can interpret the incoming parameters value in any way required.</span></span> <span data-ttu-id="94f59-120">Por ejemplo, no hay ningún requisito para asignar los parámetros de entrada a las propiedades de la clase.</span><span class="sxs-lookup"><span data-stu-id="94f59-120">For example, there's no requirement to assign the incoming parameters to the properties on the class.</span></span>

### <a name="after-parameters-are-set"></a><span data-ttu-id="94f59-121">Después de establecer los parámetros</span><span class="sxs-lookup"><span data-stu-id="94f59-121">After parameters are set</span></span>

<span data-ttu-id="94f59-122">se llama a <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync*> y <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet*> cuando un componente ha recibido parámetros de su elemento primario y los valores se asignan a las propiedades.</span><span class="sxs-lookup"><span data-stu-id="94f59-122"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSetAsync*> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnParametersSet*> are called when a component has received parameters from its parent and the values are assigned to properties.</span></span> <span data-ttu-id="94f59-123">Estos métodos se ejecutan después de la inicialización del componente y cada vez que se especifican nuevos valores de parámetro:</span><span class="sxs-lookup"><span data-stu-id="94f59-123">These methods are executed after component initialization and each time new parameter values are specified:</span></span>

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

> [!NOTE]
> <span data-ttu-id="94f59-124">El trabajo asincrónico al aplicar parámetros y valores de propiedad debe producirse durante el evento del ciclo de vida de `OnParametersSetAsync`.</span><span class="sxs-lookup"><span data-stu-id="94f59-124">Asynchronous work when applying parameters and property values must occur during the `OnParametersSetAsync` lifecycle event.</span></span>

```csharp
protected override void OnParametersSet()
{
    ...
}
```

### <a name="after-component-render"></a><span data-ttu-id="94f59-125">Después de representar el componente</span><span class="sxs-lookup"><span data-stu-id="94f59-125">After component render</span></span>

<span data-ttu-id="94f59-126">se llama a <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync*> y <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender*> después de que un componente haya terminado de representar.</span><span class="sxs-lookup"><span data-stu-id="94f59-126"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRenderAsync*> and <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender*> are called after a component has finished rendering.</span></span> <span data-ttu-id="94f59-127">En este punto se rellenan las referencias a elementos y componentes.</span><span class="sxs-lookup"><span data-stu-id="94f59-127">Element and component references are populated at this point.</span></span> <span data-ttu-id="94f59-128">Use esta fase para realizar pasos de inicialización adicionales mediante el contenido representado, como la activación de bibliotecas de JavaScript de terceros que operan en los elementos DOM representados.</span><span class="sxs-lookup"><span data-stu-id="94f59-128">Use this stage to perform additional initialization steps using the rendered content, such as activating third-party JavaScript libraries that operate on the rendered DOM elements.</span></span>

<span data-ttu-id="94f59-129">El parámetro `firstRender` para `OnAfterRenderAsync` y `OnAfterRender`:</span><span class="sxs-lookup"><span data-stu-id="94f59-129">The `firstRender` parameter for `OnAfterRenderAsync` and `OnAfterRender`:</span></span>

* <span data-ttu-id="94f59-130">Se establece en `true` la primera vez que se representa la instancia del componente.</span><span class="sxs-lookup"><span data-stu-id="94f59-130">Is set to `true` the first time that the component instance is rendered.</span></span>
* <span data-ttu-id="94f59-131">Se puede utilizar para asegurarse de que el trabajo de inicialización solo se realiza una vez.</span><span class="sxs-lookup"><span data-stu-id="94f59-131">Can be used to ensure that initialization work is only performed once.</span></span>

```csharp
protected override async Task OnAfterRenderAsync(bool firstRender)
{
    if (firstRender)
    {
        await ...
    }
}
```

> [!NOTE]
> <span data-ttu-id="94f59-132">El trabajo asincrónico inmediatamente después de la representación debe producirse durante el evento del ciclo de vida de `OnAfterRenderAsync`.</span><span class="sxs-lookup"><span data-stu-id="94f59-132">Asynchronous work immediately after rendering must occur during the `OnAfterRenderAsync` lifecycle event.</span></span>
>
> <span data-ttu-id="94f59-133">Incluso si se devuelve un <xref:System.Threading.Tasks.Task> de `OnAfterRenderAsync`, el marco de trabajo no programa un ciclo de representación adicional para el componente una vez que la tarea se completa.</span><span class="sxs-lookup"><span data-stu-id="94f59-133">Even if you return a <xref:System.Threading.Tasks.Task> from `OnAfterRenderAsync`, the framework doesn't schedule a further render cycle for your component once that task completes.</span></span> <span data-ttu-id="94f59-134">Esto se hace para evitar un bucle de representación infinito.</span><span class="sxs-lookup"><span data-stu-id="94f59-134">This is to avoid an infinite render loop.</span></span> <span data-ttu-id="94f59-135">Es diferente de los otros métodos de ciclo de vida, que programan un ciclo de representación adicional una vez completada la tarea devuelta.</span><span class="sxs-lookup"><span data-stu-id="94f59-135">It's different from the other lifecycle methods, which schedule a further render cycle once the returned task completes.</span></span>

```csharp
protected override void OnAfterRender(bool firstRender)
{
    if (firstRender)
    {
        ...
    }
}
```

<span data-ttu-id="94f59-136">no se llama a `OnAfterRender` y `OnAfterRenderAsync` *al representarse en el servidor.*</span><span class="sxs-lookup"><span data-stu-id="94f59-136">`OnAfterRender` and `OnAfterRenderAsync` *aren't called when prerendering on the server.*</span></span>

### <a name="suppress-ui-refreshing"></a><span data-ttu-id="94f59-137">Suprimir actualización de IU</span><span class="sxs-lookup"><span data-stu-id="94f59-137">Suppress UI refreshing</span></span>

<span data-ttu-id="94f59-138">Invalide <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender*> para suprimir la actualización de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="94f59-138">Override <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender*> to suppress UI refreshing.</span></span> <span data-ttu-id="94f59-139">Si la implementación devuelve `true`, la interfaz de usuario se actualiza:</span><span class="sxs-lookup"><span data-stu-id="94f59-139">If the implementation returns `true`, the UI is refreshed:</span></span>

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

<span data-ttu-id="94f59-140">se llama a `ShouldRender` cada vez que se representa el componente.</span><span class="sxs-lookup"><span data-stu-id="94f59-140">`ShouldRender` is called each time the component is rendered.</span></span>

<span data-ttu-id="94f59-141">Aunque se invalide `ShouldRender`, el componente siempre se representa inicialmente.</span><span class="sxs-lookup"><span data-stu-id="94f59-141">Even if `ShouldRender` is overridden, the component is always initially rendered.</span></span>

## <a name="state-changes"></a><span data-ttu-id="94f59-142">Cambios de estado</span><span class="sxs-lookup"><span data-stu-id="94f59-142">State changes</span></span>

<span data-ttu-id="94f59-143"><xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> notifica al componente que su estado ha cambiado.</span><span class="sxs-lookup"><span data-stu-id="94f59-143"><xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> notifies the component that its state has changed.</span></span> <span data-ttu-id="94f59-144">Cuando es aplicable, la llamada a `StateHasChanged` hace que el componente se represente.</span><span class="sxs-lookup"><span data-stu-id="94f59-144">When applicable, calling `StateHasChanged` causes the component to be rerendered.</span></span>

## <a name="handle-incomplete-async-actions-at-render"></a><span data-ttu-id="94f59-145">Controlar acciones asincrónicas incompletas en la representación</span><span class="sxs-lookup"><span data-stu-id="94f59-145">Handle incomplete async actions at render</span></span>

<span data-ttu-id="94f59-146">Es posible que las acciones asincrónicas realizadas en eventos del ciclo de vida no se hayan completado antes de que se represente el componente.</span><span class="sxs-lookup"><span data-stu-id="94f59-146">Asynchronous actions performed in lifecycle events might not have completed before the component is rendered.</span></span> <span data-ttu-id="94f59-147">Los objetos podrían estar `null` o rellenarse de forma incompleta con datos mientras se ejecuta el método de ciclo de vida.</span><span class="sxs-lookup"><span data-stu-id="94f59-147">Objects might be `null` or incompletely populated with data while the lifecycle method is executing.</span></span> <span data-ttu-id="94f59-148">Proporcione lógica de representación para confirmar que los objetos se inicializan.</span><span class="sxs-lookup"><span data-stu-id="94f59-148">Provide rendering logic to confirm that objects are initialized.</span></span> <span data-ttu-id="94f59-149">Representar los elementos de la interfaz de usuario del marcador de posición (por ejemplo, un mensaje de carga) mientras los objetos se `null`.</span><span class="sxs-lookup"><span data-stu-id="94f59-149">Render placeholder UI elements (for example, a loading message) while objects are `null`.</span></span>

<span data-ttu-id="94f59-150">En el `FetchData` componente de las plantillas de Blazor, `OnInitializedAsync` se invalida a asincrónica recibir datos de previsión (`forecasts`).</span><span class="sxs-lookup"><span data-stu-id="94f59-150">In the `FetchData` component of the Blazor templates, `OnInitializedAsync` is overridden to asychronously receive forecast data (`forecasts`).</span></span> <span data-ttu-id="94f59-151">Cuando `forecasts` se `null`, se muestra al usuario un mensaje de carga.</span><span class="sxs-lookup"><span data-stu-id="94f59-151">When `forecasts` is `null`, a loading message is displayed to the user.</span></span> <span data-ttu-id="94f59-152">Una vez que se completa el `Task` devuelto por `OnInitializedAsync`, el componente se representará con el estado actualizado.</span><span class="sxs-lookup"><span data-stu-id="94f59-152">After the `Task` returned by `OnInitializedAsync` completes, the component is rerendered with the updated state.</span></span>

<span data-ttu-id="94f59-153">*Pages/FetchData. Razor* en la plantilla de Blazor Server:</span><span class="sxs-lookup"><span data-stu-id="94f59-153">*Pages/FetchData.razor* in the Blazor Server template:</span></span>

[!code-cshtml[](lifecycle/samples_snapshot/3.x/FetchData.razor?highlight=9,21,25)]

## <a name="component-disposal-with-idisposable"></a><span data-ttu-id="94f59-154">Eliminación de componentes con IDisposable</span><span class="sxs-lookup"><span data-stu-id="94f59-154">Component disposal with IDisposable</span></span>

<span data-ttu-id="94f59-155">Si un componente implementa <xref:System.IDisposable>, se llama al [método Dispose](/dotnet/standard/garbage-collection/implementing-dispose) cuando se quita el componente de la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="94f59-155">If a component implements <xref:System.IDisposable>, the [Dispose method](/dotnet/standard/garbage-collection/implementing-dispose) is called when the component is removed from the UI.</span></span> <span data-ttu-id="94f59-156">El siguiente componente utiliza `@implements IDisposable` y el método `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="94f59-156">The following component uses `@implements IDisposable` and the `Dispose` method:</span></span>

```csharp
@using System
@implements IDisposable

...

@code {
    public void Dispose()
    {
        ...
    }
}
```

> [!NOTE]
> <span data-ttu-id="94f59-157">No se admite la llamada a <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> en `Dispose`.</span><span class="sxs-lookup"><span data-stu-id="94f59-157">Calling <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged*> in `Dispose` isn't supported.</span></span> <span data-ttu-id="94f59-158">`StateHasChanged` se puede invocar como parte de la anulación del representador, por lo que no se admite la solicitud de actualizaciones de la interfaz de usuario en ese momento.</span><span class="sxs-lookup"><span data-stu-id="94f59-158">`StateHasChanged` might be invoked as part of tearing down the renderer, so requesting UI updates at that point isn't supported.</span></span>

## <a name="handle-errors"></a><span data-ttu-id="94f59-159">Control de errores</span><span class="sxs-lookup"><span data-stu-id="94f59-159">Handle errors</span></span>

<span data-ttu-id="94f59-160">Para obtener información sobre cómo controlar los errores durante la ejecución del método de ciclo de vida, vea <xref:blazor/handle-errors#lifecycle-methods>.</span><span class="sxs-lookup"><span data-stu-id="94f59-160">For information on handling errors during lifecycle method execution, see <xref:blazor/handle-errors#lifecycle-methods>.</span></span>
