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
# <a name="aspnet-core-blazor-advanced-scenarios"></a>ASP.NET Core escenarios avanzados más increíbles

Por [Luke Latham](https://github.com/guardrex) y [Daniel Roth](https://github.com/danroth27)

## <a name="manual-rendertreebuilder-logic"></a>Lógica de RenderTreeBuilder manual

`Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder` proporciona métodos para manipular componentes y elementos, incluida la creación manual de componentes C# en el código.

> [!NOTE]
> El uso de `RenderTreeBuilder` para crear componentes es un escenario avanzado. Un componente con formato incorrecto (por ejemplo, una etiqueta de marcado sin cerrar) puede dar lugar a un comportamiento indefinido.

Tenga en cuenta el siguiente componente de `PetDetails`, que se puede integrar manualmente en otro componente:

```razor
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

En el ejemplo siguiente, el bucle del método `CreateComponent` genera tres componentes `PetDetails`. Al llamar a métodos `RenderTreeBuilder` para crear los componentes (`OpenComponent` y `AddAttribute`), los números de secuencia son números de línea de código fuente. El algoritmo de diferencia más increíble se basa en los números de secuencia correspondientes a líneas de código distintas, no a invocaciones de llamada distintas. Al crear un componente con métodos de `RenderTreeBuilder`, codifique los argumentos para los números de secuencia. **El uso de un cálculo o un contador para generar el número de secuencia puede dar lugar a un rendimiento deficiente.** Para obtener más información, vea la sección [números de secuencia relacionados con números de línea de código y no orden de ejecución](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) .

`BuiltContent` componente:

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
> Los tipos de `Microsoft.AspNetCore.Components.RenderTree` permiten el procesamiento de los *resultados* de las operaciones de representación. Estos son los detalles internos de la implementación de la plataforma más extraordinaria. Estos tipos se deben considerar *inestables* y estar sujetos a cambios en futuras versiones.

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a>Los números de secuencia se relacionan con los números de línea de código y no el orden de ejecución

Los archivos de componentes de Razor ( *. Razor*) siempre se compilan. La compilación es una ventaja potencial sobre la interpretación de código porque el paso de compilación se puede usar para insertar información que mejora el rendimiento de las aplicaciones en tiempo de ejecución.

Un ejemplo clave de estas mejoras implica *los números de secuencia*. Los números de secuencia indican al tiempo de ejecución los resultados de los que proceden las líneas de código distintas y ordenadas. El motor en tiempo de ejecución utiliza esta información para generar diferencias de árbol eficientes en el tiempo lineal, que es mucho más rápido de lo que suele ser posible para un algoritmo de comparación de árboles generales.

Considere el siguiente archivo de componente de Razor ( *. Razor*):

```razor
@if (someFlag)
{
    <text>First</text>
}

Second
```

El código anterior se compila en algo similar a lo siguiente:

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

Cuando el código se ejecuta por primera vez, si se `true``someFlag`, el generador recibe:

| Secuencia | Tipo      | data   |
| :------: | --------- | :----: |
| 0        | Nodo de texto | Primero  |
| 1        | Nodo de texto | Segundo |

Imagine que `someFlag` se `false`y que el marcado se representará de nuevo. Esta vez, el generador recibe:

| Secuencia | Tipo       | data   |
| :------: | ---------- | :----: |
| 1        | Nodo de texto  | Segundo |

Cuando el tiempo de ejecución realiza una comparación, ve que se quitó el elemento en la secuencia `0`, por lo que genera el siguiente *script de edición*trivial:

* Quitar el primer nodo de texto.

### <a name="the-problem-with-generating-sequence-numbers-programmatically"></a>Problema con la generación de números de secuencia mediante programación

Imagine que escribió la siguiente lógica del generador de árboles de representación:

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

Ahora, el primer resultado es:

| Secuencia | Tipo      | data   |
| :------: | --------- | :----: |
| 0        | Nodo de texto | Primero  |
| 1        | Nodo de texto | Segundo |

Este resultado es idéntico al caso anterior, por lo que no existe ningún problema negativo. `someFlag` se `false` en la segunda representación y el resultado es:

| Secuencia | Tipo      | data   |
| :------: | --------- | ------ |
| 0        | Nodo de texto | Segundo |

Esta vez, el algoritmo de comparación ve que se han producido *dos* cambios y el algoritmo genera el siguiente script de edición:

* Cambie el valor del primer nodo de texto a `Second`.
* Quite el segundo nodo de texto.

La generación de los números de secuencia ha perdido toda la información útil sobre el lugar en el que las bifurcaciones de `if/else` y los bucles estaban presentes en el código original. Esto da como resultado una diferencia **dos veces mayor que** antes.

Este es un ejemplo trivial. En casos más realistas con estructuras complejas y profundamente anidadas, y especialmente con bucles, el costo de rendimiento suele ser mayor. En lugar de identificar inmediatamente qué bloques o ramas de bucle se han insertado o quitado, el algoritmo de comparación tiene que recurse profundamente en los árboles de representación. Esto suele dar lugar a tener que compilar scripts de edición más largos, ya que el algoritmo de comparación está informado de forma informada sobre cómo se relacionan entre sí las estructuras antiguas y nuevas.

### <a name="guidance-and-conclusions"></a>Instrucciones y conclusiones

* El rendimiento de la aplicación se ve afectado si los números de secuencia se generan dinámicamente.
* El marco no puede crear sus propios números de secuencia automáticamente en tiempo de ejecución porque la información necesaria no existe a menos que se Capture en tiempo de compilación.
* No escriba grandes bloques de lógica de `RenderTreeBuilder` implementada de forma manual. Prefieren los archivos *. Razor* y permiten al compilador tratar con los números de secuencia. Si no puede evitar la lógica de `RenderTreeBuilder` manual, divida bloques largos de código en fragmentos más pequeños encapsulados en `OpenRegion`/`CloseRegion` llamadas. Cada región tiene su propio espacio independiente de números de secuencia, por lo que puede reiniciar desde cero (o cualquier otro número arbitrario) dentro de cada región.
* Si los números de secuencia están codificados, el algoritmo de comparación solo requiere que los números de secuencia aumenten en valor. El valor inicial y los huecos son irrelevantes. Una opción legítima es usar el número de línea de código como el número de secuencia, o comenzar a partir de cero y aumentar por unos o cientos (o cualquier intervalo preferido). 
* Blazor usa los números de secuencia, mientras que otros marcos de interfaz de usuario de comparación de árboles no los utilizan. La diferenciación es mucho más rápida cuando se utilizan números de secuencia y Blazor tiene la ventaja de un paso de compilación que trata los números de secuencia automáticamente para desarrolladores que crean archivos *. Razor* .
