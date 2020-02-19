---
title: ASP.NET Core escenarios avanzados de Blazor
author: guardrex
description: Obtenga información sobre escenarios avanzados en Blazor, incluido cómo incorporar la lógica de RenderTreeBuilder manual en una aplicación.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/advanced-scenarios
ms.openlocfilehash: 5e0618faa7b1b5e4cc15e30d9c16afaf7ccabaf0
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77453185"
---
# <a name="aspnet-core-blazor-advanced-scenarios"></a><span data-ttu-id="7fdaf-103">ASP.NET Core escenarios avanzados más increíbles</span><span class="sxs-lookup"><span data-stu-id="7fdaf-103">ASP.NET Core Blazor advanced scenarios</span></span>

<span data-ttu-id="7fdaf-104">Por [Luke Latham](https://github.com/guardrex) y [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="7fdaf-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="7fdaf-105">Lógica de RenderTreeBuilder manual</span><span class="sxs-lookup"><span data-stu-id="7fdaf-105">Manual RenderTreeBuilder logic</span></span>

<span data-ttu-id="7fdaf-106">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` proporciona métodos para manipular componentes y elementos, incluida la creación manual de componentes C# en el código.</span><span class="sxs-lookup"><span data-stu-id="7fdaf-106">`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="7fdaf-107">El uso de `RenderTreeBuilder` para crear componentes es un escenario avanzado.</span><span class="sxs-lookup"><span data-stu-id="7fdaf-107">Use of `RenderTreeBuilder` to create components is an advanced scenario.</span></span> <span data-ttu-id="7fdaf-108">Un componente con formato incorrecto (por ejemplo, una etiqueta de marcado sin cerrar) puede dar lugar a un comportamiento indefinido.</span><span class="sxs-lookup"><span data-stu-id="7fdaf-108">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="7fdaf-109">Tenga en cuenta el siguiente componente de `PetDetails`, que se puede integrar manualmente en otro componente:</span><span class="sxs-lookup"><span data-stu-id="7fdaf-109">Consider the following `PetDetails` component, which can be manually built into another component:</span></span>

```razor
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="7fdaf-110">En el ejemplo siguiente, el bucle del método `CreateComponent` genera tres componentes `PetDetails`.</span><span class="sxs-lookup"><span data-stu-id="7fdaf-110">In the following example, the loop in the `CreateComponent` method generates three `PetDetails` components.</span></span> <span data-ttu-id="7fdaf-111">Al llamar a métodos `RenderTreeBuilder` para crear los componentes (`OpenComponent` y `AddAttribute`), los números de secuencia son números de línea de código fuente.</span><span class="sxs-lookup"><span data-stu-id="7fdaf-111">When calling `RenderTreeBuilder` methods to create the components (`OpenComponent` and `AddAttribute`), sequence numbers are source code line numbers.</span></span> <span data-ttu-id="7fdaf-112">El algoritmo de diferencia más increíble se basa en los números de secuencia correspondientes a líneas de código distintas, no a invocaciones de llamada distintas.</span><span class="sxs-lookup"><span data-stu-id="7fdaf-112">The Blazor difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="7fdaf-113">Al crear un componente con métodos de `RenderTreeBuilder`, codifique los argumentos para los números de secuencia.</span><span class="sxs-lookup"><span data-stu-id="7fdaf-113">When creating a component with `RenderTreeBuilder` methods, hardcode the arguments for sequence numbers.</span></span> <span data-ttu-id="7fdaf-114">**El uso de un cálculo o un contador para generar el número de secuencia puede dar lugar a un rendimiento deficiente.**</span><span class="sxs-lookup"><span data-stu-id="7fdaf-114">**Using a calculation or counter to generate the sequence number can lead to poor performance.**</span></span> <span data-ttu-id="7fdaf-115">Para obtener más información, vea la sección [números de secuencia relacionados con números de línea de código y no orden de ejecución](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) .</span><span class="sxs-lookup"><span data-stu-id="7fdaf-115">For more information, see the [Sequence numbers relate to code line numbers and not execution order](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) section.</span></span>

<span data-ttu-id="7fdaf-116">`BuiltContent` componente:</span><span class="sxs-lookup"><span data-stu-id="7fdaf-116">`BuiltContent` component:</span></span>

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
> <span data-ttu-id="7fdaf-117">Los tipos de `Microsoft.AspNetCore.Components.RenderTree` permiten el procesamiento de los *resultados* de las operaciones de representación.</span><span class="sxs-lookup"><span data-stu-id="7fdaf-117">The types in `Microsoft.AspNetCore.Components.RenderTree` allow processing of the *results* of rendering operations.</span></span> <span data-ttu-id="7fdaf-118">Estos son los detalles internos de la implementación de la plataforma más extraordinaria.</span><span class="sxs-lookup"><span data-stu-id="7fdaf-118">These are internal details of the Blazor framework implementation.</span></span> <span data-ttu-id="7fdaf-119">Estos tipos se deben considerar *inestables* y estar sujetos a cambios en futuras versiones.</span><span class="sxs-lookup"><span data-stu-id="7fdaf-119">These types should be considered *unstable* and subject to change in future releases.</span></span>

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a><span data-ttu-id="7fdaf-120">Los números de secuencia se relacionan con los números de línea de código y no el orden de ejecución</span><span class="sxs-lookup"><span data-stu-id="7fdaf-120">Sequence numbers relate to code line numbers and not execution order</span></span>

<span data-ttu-id="7fdaf-121">Los archivos de componentes de Razor ( *. Razor*) siempre se compilan.</span><span class="sxs-lookup"><span data-stu-id="7fdaf-121">Razor component files (*.razor*) are always compiled.</span></span> <span data-ttu-id="7fdaf-122">La compilación es una ventaja potencial sobre la interpretación de código porque el paso de compilación se puede usar para insertar información que mejora el rendimiento de las aplicaciones en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="7fdaf-122">Compilation is a potential advantage over interpreting code because the compile step can be used to inject information that improves app performance at runtime.</span></span>

<span data-ttu-id="7fdaf-123">Un ejemplo clave de estas mejoras implica *los números de secuencia*.</span><span class="sxs-lookup"><span data-stu-id="7fdaf-123">A key example of these improvements involves *sequence numbers*.</span></span> <span data-ttu-id="7fdaf-124">Los números de secuencia indican al tiempo de ejecución los resultados de los que proceden las líneas de código distintas y ordenadas.</span><span class="sxs-lookup"><span data-stu-id="7fdaf-124">Sequence numbers indicate to the runtime which outputs came from which distinct and ordered lines of code.</span></span> <span data-ttu-id="7fdaf-125">El motor en tiempo de ejecución utiliza esta información para generar diferencias de árbol eficientes en el tiempo lineal, que es mucho más rápido de lo que suele ser posible para un algoritmo de comparación de árboles generales.</span><span class="sxs-lookup"><span data-stu-id="7fdaf-125">The runtime uses this information to generate efficient tree diffs in linear time, which is far faster than is normally possible for a general tree diff algorithm.</span></span>

<span data-ttu-id="7fdaf-126">Considere el siguiente archivo de componente de Razor ( *. Razor*):</span><span class="sxs-lookup"><span data-stu-id="7fdaf-126">Consider the following Razor component (*.razor*) file:</span></span>

```razor
@if (someFlag)
{
    <text>First</text>
}

Second
```

<span data-ttu-id="7fdaf-127">El código anterior se compila en algo similar a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="7fdaf-127">The preceding code compiles to something like the following:</span></span>

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

<span data-ttu-id="7fdaf-128">Cuando el código se ejecuta por primera vez, si se `true``someFlag`, el generador recibe:</span><span class="sxs-lookup"><span data-stu-id="7fdaf-128">When the code executes for the first time, if `someFlag` is `true`, the builder receives:</span></span>

| <span data-ttu-id="7fdaf-129">Secuencia</span><span class="sxs-lookup"><span data-stu-id="7fdaf-129">Sequence</span></span> | <span data-ttu-id="7fdaf-130">Tipo</span><span class="sxs-lookup"><span data-stu-id="7fdaf-130">Type</span></span>      | <span data-ttu-id="7fdaf-131">data</span><span class="sxs-lookup"><span data-stu-id="7fdaf-131">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="7fdaf-132">0</span><span class="sxs-lookup"><span data-stu-id="7fdaf-132">0</span></span>        | <span data-ttu-id="7fdaf-133">Nodo de texto</span><span class="sxs-lookup"><span data-stu-id="7fdaf-133">Text node</span></span> | <span data-ttu-id="7fdaf-134">Primero</span><span class="sxs-lookup"><span data-stu-id="7fdaf-134">First</span></span>  |
| <span data-ttu-id="7fdaf-135">1</span><span class="sxs-lookup"><span data-stu-id="7fdaf-135">1</span></span>        | <span data-ttu-id="7fdaf-136">Nodo de texto</span><span class="sxs-lookup"><span data-stu-id="7fdaf-136">Text node</span></span> | <span data-ttu-id="7fdaf-137">Segundo</span><span class="sxs-lookup"><span data-stu-id="7fdaf-137">Second</span></span> |

<span data-ttu-id="7fdaf-138">Imagine que `someFlag` se `false`y que el marcado se representará de nuevo.</span><span class="sxs-lookup"><span data-stu-id="7fdaf-138">Imagine that `someFlag` becomes `false`, and the markup is rendered again.</span></span> <span data-ttu-id="7fdaf-139">Esta vez, el generador recibe:</span><span class="sxs-lookup"><span data-stu-id="7fdaf-139">This time, the builder receives:</span></span>

| <span data-ttu-id="7fdaf-140">Secuencia</span><span class="sxs-lookup"><span data-stu-id="7fdaf-140">Sequence</span></span> | <span data-ttu-id="7fdaf-141">Tipo</span><span class="sxs-lookup"><span data-stu-id="7fdaf-141">Type</span></span>       | <span data-ttu-id="7fdaf-142">data</span><span class="sxs-lookup"><span data-stu-id="7fdaf-142">Data</span></span>   |
| :------: | ---------- | :----: |
| <span data-ttu-id="7fdaf-143">1</span><span class="sxs-lookup"><span data-stu-id="7fdaf-143">1</span></span>        | <span data-ttu-id="7fdaf-144">Nodo de texto</span><span class="sxs-lookup"><span data-stu-id="7fdaf-144">Text node</span></span>  | <span data-ttu-id="7fdaf-145">Segundo</span><span class="sxs-lookup"><span data-stu-id="7fdaf-145">Second</span></span> |

<span data-ttu-id="7fdaf-146">Cuando el tiempo de ejecución realiza una comparación, ve que se quitó el elemento en la secuencia `0`, por lo que genera el siguiente *script de edición*trivial:</span><span class="sxs-lookup"><span data-stu-id="7fdaf-146">When the runtime performs a diff, it sees that the item at sequence `0` was removed, so it generates the following trivial *edit script*:</span></span>

* <span data-ttu-id="7fdaf-147">Quitar el primer nodo de texto.</span><span class="sxs-lookup"><span data-stu-id="7fdaf-147">Remove the first text node.</span></span>

### <a name="the-problem-with-generating-sequence-numbers-programmatically"></a><span data-ttu-id="7fdaf-148">Problema con la generación de números de secuencia mediante programación</span><span class="sxs-lookup"><span data-stu-id="7fdaf-148">The problem with generating sequence numbers programmatically</span></span>

<span data-ttu-id="7fdaf-149">Imagine que escribió la siguiente lógica del generador de árboles de representación:</span><span class="sxs-lookup"><span data-stu-id="7fdaf-149">Imagine instead that you wrote the following render tree builder logic:</span></span>

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

<span data-ttu-id="7fdaf-150">Ahora, el primer resultado es:</span><span class="sxs-lookup"><span data-stu-id="7fdaf-150">Now, the first output is:</span></span>

| <span data-ttu-id="7fdaf-151">Secuencia</span><span class="sxs-lookup"><span data-stu-id="7fdaf-151">Sequence</span></span> | <span data-ttu-id="7fdaf-152">Tipo</span><span class="sxs-lookup"><span data-stu-id="7fdaf-152">Type</span></span>      | <span data-ttu-id="7fdaf-153">data</span><span class="sxs-lookup"><span data-stu-id="7fdaf-153">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="7fdaf-154">0</span><span class="sxs-lookup"><span data-stu-id="7fdaf-154">0</span></span>        | <span data-ttu-id="7fdaf-155">Nodo de texto</span><span class="sxs-lookup"><span data-stu-id="7fdaf-155">Text node</span></span> | <span data-ttu-id="7fdaf-156">Primero</span><span class="sxs-lookup"><span data-stu-id="7fdaf-156">First</span></span>  |
| <span data-ttu-id="7fdaf-157">1</span><span class="sxs-lookup"><span data-stu-id="7fdaf-157">1</span></span>        | <span data-ttu-id="7fdaf-158">Nodo de texto</span><span class="sxs-lookup"><span data-stu-id="7fdaf-158">Text node</span></span> | <span data-ttu-id="7fdaf-159">Segundo</span><span class="sxs-lookup"><span data-stu-id="7fdaf-159">Second</span></span> |

<span data-ttu-id="7fdaf-160">Este resultado es idéntico al caso anterior, por lo que no existe ningún problema negativo.</span><span class="sxs-lookup"><span data-stu-id="7fdaf-160">This outcome is identical to the prior case, so no negative issues exist.</span></span> <span data-ttu-id="7fdaf-161">`someFlag` se `false` en la segunda representación y el resultado es:</span><span class="sxs-lookup"><span data-stu-id="7fdaf-161">`someFlag` is `false` on the second rendering, and the output is:</span></span>

| <span data-ttu-id="7fdaf-162">Secuencia</span><span class="sxs-lookup"><span data-stu-id="7fdaf-162">Sequence</span></span> | <span data-ttu-id="7fdaf-163">Tipo</span><span class="sxs-lookup"><span data-stu-id="7fdaf-163">Type</span></span>      | <span data-ttu-id="7fdaf-164">data</span><span class="sxs-lookup"><span data-stu-id="7fdaf-164">Data</span></span>   |
| :------: | --------- | ------ |
| <span data-ttu-id="7fdaf-165">0</span><span class="sxs-lookup"><span data-stu-id="7fdaf-165">0</span></span>        | <span data-ttu-id="7fdaf-166">Nodo de texto</span><span class="sxs-lookup"><span data-stu-id="7fdaf-166">Text node</span></span> | <span data-ttu-id="7fdaf-167">Segundo</span><span class="sxs-lookup"><span data-stu-id="7fdaf-167">Second</span></span> |

<span data-ttu-id="7fdaf-168">Esta vez, el algoritmo de comparación ve que se han producido *dos* cambios y el algoritmo genera el siguiente script de edición:</span><span class="sxs-lookup"><span data-stu-id="7fdaf-168">This time, the diff algorithm sees that *two* changes have occurred, and the algorithm generates the following edit script:</span></span>

* <span data-ttu-id="7fdaf-169">Cambie el valor del primer nodo de texto a `Second`.</span><span class="sxs-lookup"><span data-stu-id="7fdaf-169">Change the value of the first text node to `Second`.</span></span>
* <span data-ttu-id="7fdaf-170">Quite el segundo nodo de texto.</span><span class="sxs-lookup"><span data-stu-id="7fdaf-170">Remove the second text node.</span></span>

<span data-ttu-id="7fdaf-171">La generación de los números de secuencia ha perdido toda la información útil sobre el lugar en el que las bifurcaciones de `if/else` y los bucles estaban presentes en el código original.</span><span class="sxs-lookup"><span data-stu-id="7fdaf-171">Generating the sequence numbers has lost all the useful information about where the `if/else` branches and loops were present in the original code.</span></span> <span data-ttu-id="7fdaf-172">Esto da como resultado una diferencia **dos veces mayor que** antes.</span><span class="sxs-lookup"><span data-stu-id="7fdaf-172">This results in a diff **twice as long** as before.</span></span>

<span data-ttu-id="7fdaf-173">Este es un ejemplo trivial.</span><span class="sxs-lookup"><span data-stu-id="7fdaf-173">This is a trivial example.</span></span> <span data-ttu-id="7fdaf-174">En casos más realistas con estructuras complejas y profundamente anidadas, y especialmente con bucles, el costo de rendimiento suele ser mayor.</span><span class="sxs-lookup"><span data-stu-id="7fdaf-174">In more realistic cases with complex and deeply nested structures, and especially with loops, the performance cost is usually higher.</span></span> <span data-ttu-id="7fdaf-175">En lugar de identificar inmediatamente qué bloques o ramas de bucle se han insertado o quitado, el algoritmo de comparación tiene que recurse profundamente en los árboles de representación.</span><span class="sxs-lookup"><span data-stu-id="7fdaf-175">Instead of immediately identifying which loop blocks or branches have been inserted or removed, the diff algorithm has to recurse deeply into the render trees.</span></span> <span data-ttu-id="7fdaf-176">Esto suele dar lugar a tener que compilar scripts de edición más largos, ya que el algoritmo de comparación está informado de forma informada sobre cómo se relacionan entre sí las estructuras antiguas y nuevas.</span><span class="sxs-lookup"><span data-stu-id="7fdaf-176">This usually results in having to build longer edit scripts because the diff algorithm is misinformed about how the old and new structures relate to each other.</span></span>

### <a name="guidance-and-conclusions"></a><span data-ttu-id="7fdaf-177">Instrucciones y conclusiones</span><span class="sxs-lookup"><span data-stu-id="7fdaf-177">Guidance and conclusions</span></span>

* <span data-ttu-id="7fdaf-178">El rendimiento de la aplicación se ve afectado si los números de secuencia se generan dinámicamente.</span><span class="sxs-lookup"><span data-stu-id="7fdaf-178">App performance suffers if sequence numbers are generated dynamically.</span></span>
* <span data-ttu-id="7fdaf-179">El marco no puede crear sus propios números de secuencia automáticamente en tiempo de ejecución porque la información necesaria no existe a menos que se Capture en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="7fdaf-179">The framework can't create its own sequence numbers automatically at runtime because the necessary information doesn't exist unless it's captured at compile time.</span></span>
* <span data-ttu-id="7fdaf-180">No escriba grandes bloques de lógica de `RenderTreeBuilder` implementada de forma manual.</span><span class="sxs-lookup"><span data-stu-id="7fdaf-180">Don't write long blocks of manually-implemented `RenderTreeBuilder` logic.</span></span> <span data-ttu-id="7fdaf-181">Prefieren los archivos *. Razor* y permiten al compilador tratar con los números de secuencia.</span><span class="sxs-lookup"><span data-stu-id="7fdaf-181">Prefer *.razor* files and allow the compiler to deal with the sequence numbers.</span></span> <span data-ttu-id="7fdaf-182">Si no puede evitar la lógica de `RenderTreeBuilder` manual, divida bloques largos de código en fragmentos más pequeños encapsulados en `OpenRegion`/`CloseRegion` llamadas.</span><span class="sxs-lookup"><span data-stu-id="7fdaf-182">If you're unable to avoid manual `RenderTreeBuilder` logic, split long blocks of code into smaller pieces wrapped in `OpenRegion`/`CloseRegion` calls.</span></span> <span data-ttu-id="7fdaf-183">Cada región tiene su propio espacio independiente de números de secuencia, por lo que puede reiniciar desde cero (o cualquier otro número arbitrario) dentro de cada región.</span><span class="sxs-lookup"><span data-stu-id="7fdaf-183">Each region has its own separate space of sequence numbers, so you can restart from zero (or any other arbitrary number) inside each region.</span></span>
* <span data-ttu-id="7fdaf-184">Si los números de secuencia están codificados, el algoritmo de comparación solo requiere que los números de secuencia aumenten en valor.</span><span class="sxs-lookup"><span data-stu-id="7fdaf-184">If sequence numbers are hardcoded, the diff algorithm only requires that sequence numbers increase in value.</span></span> <span data-ttu-id="7fdaf-185">El valor inicial y los huecos son irrelevantes.</span><span class="sxs-lookup"><span data-stu-id="7fdaf-185">The initial value and gaps are irrelevant.</span></span> <span data-ttu-id="7fdaf-186">Una opción legítima es usar el número de línea de código como el número de secuencia, o comenzar a partir de cero y aumentar por unos o cientos (o cualquier intervalo preferido).</span><span class="sxs-lookup"><span data-stu-id="7fdaf-186">One legitimate option is to use the code line number as the sequence number, or start from zero and increase by ones or hundreds (or any preferred interval).</span></span> 
* Blazor<span data-ttu-id="7fdaf-187"> usa los números de secuencia, mientras que otros marcos de interfaz de usuario de comparación de árboles no los utilizan.</span><span class="sxs-lookup"><span data-stu-id="7fdaf-187"> uses sequence numbers, while other tree-diffing UI frameworks don't use them.</span></span> <span data-ttu-id="7fdaf-188">La diferenciación es mucho más rápida cuando se utilizan números de secuencia y Blazor tiene la ventaja de un paso de compilación que trata los números de secuencia automáticamente para desarrolladores que crean archivos *. Razor* .</span><span class="sxs-lookup"><span data-stu-id="7fdaf-188">Diffing is far faster when sequence numbers are used, and Blazor has the advantage of a compile step that deals with sequence numbers automatically for developers authoring *.razor* files.</span></span>
