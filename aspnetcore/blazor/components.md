---
title: Crear y usar ASP.NET Core componentes de Razor
author: guardrex
description: Obtenga información sobre cómo crear y usar componentes de Razor, incluido cómo enlazar a datos, controlar eventos y administrar ciclos de vida de componentes.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
no-loc:
- Blazor
- SignalR
uid: blazor/components
ms.openlocfilehash: f9b4eab29fafe8113528062f57d28dadd0f57577
ms.sourcegitcommit: 6645435fc8f5092fc7e923742e85592b56e37ada
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77447105"
---
# <a name="create-and-use-aspnet-core-razor-components"></a>Crear y usar ASP.NET Core componentes de Razor

Por [Luke Latham](https://github.com/guardrex) y [Daniel Roth](https://github.com/danroth27)

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample))

Blazor aplicaciones se compilan con *componentes*de. Un componente es un fragmento independiente de la interfaz de usuario (IU), como una página, un cuadro de diálogo o un formulario. Un componente incluye el marcado HTML y la lógica de procesamiento necesaria para insertar datos o responder a los eventos de la interfaz de usuario. Los componentes son flexibles y ligeros. Se pueden anidar, reutilizar y compartir entre proyectos.

## <a name="component-classes"></a>Clases de componentes

Los componentes se implementan en archivos de componentes de [Razor](xref:mvc/views/razor) ( *. Razor*) C# mediante una combinación de y el marcado HTML. Un componente de Blazor se conoce formalmente como un *componente de Razor*.

El nombre de un componente debe empezar con un carácter en mayúsculas. Por ejemplo, *MyCoolComponent. Razor* es válido y *MyCoolComponent. Razor* no es válido.

La interfaz de usuario de un componente se define mediante HTML. La lógica de la representación dinámica (por ejemplo, bucles, instrucciones condicionales, expresiones) se agrega mediante una sintaxis de C# insertada denominada [Razor](xref:mvc/views/razor). Cuando se compila una aplicación, el marcado HTML y C# la lógica de representación se convierten en una clase de componente. El nombre de la clase generada coincide con el nombre del archivo.

Los miembros de la clase de componente se definen en un bloque `@code`. En el bloque `@code`, el estado de componente (propiedades, campos) se especifica con métodos para el control de eventos o para definir otra lógica de componentes. Se permite emplear más de un bloque `@code`.

Los miembros de componente se pueden usar como parte de la lógica de representación del C# componente mediante expresiones que comienzan con `@`. Por ejemplo, un C# campo se representa mediante el prefijo `@` al nombre del campo. En el siguiente ejemplo se evalúa y representa:

* `_headingFontStyle` al valor de la propiedad CSS para `font-style`.
* `_headingText` al contenido del elemento `<h1>`.

```razor
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@code {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

Una vez que el componente se representa inicialmente, el componente regenera su árbol de representación en respuesta a los eventos. a continuación, Blazor compara el nuevo árbol de representación con el anterior y aplica cualquier modificación a la Document Object Model del explorador (DOM).

Los componentes son C# clases normales y se pueden colocar en cualquier parte dentro de un proyecto. Los componentes que generan páginas web normalmente residen en la carpeta *pages* . Los componentes que no son de página se colocan con frecuencia en la carpeta *compartida* o en una carpeta personalizada agregada al proyecto.

Normalmente, el espacio de nombres de un componente se deriva del espacio de nombres raíz de la aplicación y la ubicación del componente (carpeta) dentro de la aplicación. Si el espacio de nombres raíz de la aplicación es `BlazorApp` y el componente `Counter` reside en la carpeta *páginas* :

* El espacio de nombres del componente de `Counter` es `BlazorApp.Pages`.
* El nombre de tipo completo del componente es `BlazorApp.Pages.Counter`.

Para obtener más información, vea la sección [importar componentes](#import-components) .

Para usar una carpeta personalizada, agregue el espacio de nombres de la carpeta personalizada al componente primario o al archivo *_Imports. Razor* de la aplicación. Por ejemplo, el siguiente espacio de nombres hace que los componentes de una carpeta *Components* estén disponibles cuando se `BlazorApp`el espacio de nombres raíz de la aplicación:

```razor
@using BlazorApp.Components
```

## <a name="tag-helpers-arent-used-in-components"></a>Las aplicaciones auxiliares de etiquetas no se usan en los componentes

Las [aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro) no se admiten en los componentes de Razor (archivos *. Razor* ). Para proporcionar funcionalidad similar a la aplicación auxiliar de etiquetas en Blazor, cree un componente con la misma funcionalidad que la aplicación auxiliar de etiquetas y use el componente en su lugar.

## <a name="use-components"></a>Uso de componentes

Los componentes pueden incluir otros componentes declarándolos mediante la sintaxis de elementos HTML. El marcado para utilizar un componente se parece a una etiqueta HTML en la que el nombre de la etiqueta es el tipo de componente.

El enlace de atributo distingue mayúsculas de minúsculas. Por ejemplo, `@bind` es válido y `@Bind` no es válido.

El marcado siguiente en *index. Razor* representa una instancia de `HeadingComponent`:

```razor
<HeadingComponent />
```

*Components/HeadingComponent. Razor*:

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/HeadingComponent.razor)]

Si un componente contiene un elemento HTML con una primera letra mayúscula que no coincide con un nombre de componente, se emite una advertencia que indica que el elemento tiene un nombre inesperado. La adición de una directiva de `@using` para el espacio de nombres del componente hace que el componente esté disponible, lo que resuelve la advertencia.

## <a name="routing"></a>Enrutamiento

El enrutamiento en Blazor se consigue proporcionando una plantilla de ruta a cada componente accesible en la aplicación.

Cuando se compila un archivo de Razor con una directiva de `@page`, a la clase generada se le asigna un <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> que especifica la plantilla de ruta. En tiempo de ejecución, el enrutador busca clases de componentes con un `RouteAttribute` y representa el componente que tiene una plantilla de ruta que coincide con la dirección URL solicitada.

```razor
@page "/ParentComponent"

...
```

Para más información, consulte <xref:blazor/routing>.

## <a name="parameters"></a>Parámetros

### <a name="route-parameters"></a>Parámetros de ruta

Los componentes pueden recibir parámetros de ruta de la plantilla de ruta proporcionada en la Directiva de `@page`. El enrutador usa parámetros de ruta para rellenar los parámetros de componente correspondientes.

*Pages/RouteParameter. Razor*:

[!code-razor[](components/samples_snapshot/RouteParameter.razor?highlight=2,7-8)]

Los parámetros opcionales no se admiten, por lo que se aplican dos directivas de `@page` en el ejemplo anterior. El primero permite la navegación al componente sin un parámetro. La segunda Directiva de `@page` recibe el parámetro `{text}` Route y asigna el valor a la propiedad `Text`.

La sintaxis de *los parámetros catch-all* (`*`/`**`), que capturan la ruta de acceso en varios límites de carpeta, **no** se admite en los componentes de Razor ( *. Razor*).

### <a name="component-parameters"></a>Parámetros del componente

Los componentes pueden tener *parámetros de componente*, que se definen mediante propiedades públicas en la clase de componente con el atributo `[Parameter]`. Use atributos para especificar argumentos para un componente en el marcado.

*Components/ChildComponent. Razor*:

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=2,11-12)]

En el ejemplo siguiente de la aplicación de ejemplo, el `ParentComponent` establece el valor de la propiedad `Title` de la `ChildComponent`.

*Pages/ParentComponent. Razor*:

[!code-razor[](components/samples_snapshot/ParentComponent.razor?highlight=5-6)]

## <a name="child-content"></a>Contenido secundario

Los componentes pueden establecer el contenido de otro componente. El componente de asignación proporciona el contenido entre las etiquetas que especifican el componente receptor.

En el ejemplo siguiente, el `ChildComponent` tiene una propiedad `ChildContent` que representa un `RenderFragment`, que representa un segmento de interfaz de usuario que se va a representar. El valor de `ChildContent` se coloca en el marcado del componente en el que se debe representar el contenido. El valor de `ChildContent` se recibe del componente primario y se representa dentro del `panel-body`del panel de arranque.

*Components/ChildComponent. Razor*:

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> La propiedad que recibe el contenido del `RenderFragment` debe denominarse `ChildContent` por Convención.

El `ParentComponent` de la aplicación de ejemplo puede proporcionar contenido para representar el `ChildComponent` colocando el contenido dentro de las etiquetas de `<ChildComponent>`.

*Pages/ParentComponent. Razor*:

[!code-razor[](components/samples_snapshot/ParentComponent.razor?highlight=7-8)]

## <a name="attribute-splatting-and-arbitrary-parameters"></a>Atributos expansión y parámetros arbitrarios

Los componentes de pueden capturar y representar atributos adicionales además de los parámetros declarados del componente. Los atributos adicionales se pueden capturar en un diccionario y, después, *splatted* en un elemento cuando el componente se representa mediante la Directiva de Razor [`@attributes`](xref:mvc/views/razor#attributes) . Este escenario es útil cuando se define un componente que genera un elemento de marcado que admite diversas personalizaciones. Por ejemplo, puede resultar tedioso definir atributos por separado para una `<input>` que admita muchos parámetros.

En el ejemplo siguiente, el primer elemento `<input>` (`id="useIndividualParams"`) utiliza parámetros de componente individuales, mientras que el segundo elemento `<input>` (`id="useAttributesDict"`) utiliza el atributo expansión:

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

El tipo del parámetro debe implementar `IEnumerable<KeyValuePair<string, object>>` con claves de cadena. El uso de `IReadOnlyDictionary<string, object>` también es una opción en este escenario.

Los elementos `<input>` representados con ambos enfoques son idénticos:

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

Para aceptar atributos arbitrarios, defina un parámetro de componente mediante el atributo `[Parameter]` con la propiedad `CaptureUnmatchedValues` establecida en `true`:

```razor
@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object> InputAttributes { get; set; }
}
```

La propiedad `CaptureUnmatchedValues` en `[Parameter]` permite que el parámetro coincida con todos los atributos que no coinciden con ningún otro parámetro. Un componente solo puede definir un parámetro con `CaptureUnmatchedValues`. El tipo de propiedad que se usa con `CaptureUnmatchedValues` se debe poder asignar desde `Dictionary<string, object>` con claves de cadena. `IEnumerable<KeyValuePair<string, object>>` o `IReadOnlyDictionary<string, object>` también son opciones en este escenario.

La posición de `@attributes` relativa a la posición de los atributos de elemento es importante. Cuando `@attributes` se splatted en el elemento, los atributos se procesan de derecha a izquierda (último a primero). Considere el siguiente ejemplo de un componente que utiliza un componente de `Child`:

*ParentComponent. Razor*:

```razor
<ChildComponent extra="10" />
```

*ChildComponent. Razor*:

```razor
<div @attributes="AdditionalAttributes" extra="5" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

El atributo `extra` del componente de `Child` se establece a la derecha de `@attributes`. El `<div>` representado del componente `Parent` contiene `extra="5"` cuando se pasa a través del atributo adicional, ya que los atributos se procesan de derecha a izquierda (último a primero):

```html
<div extra="5" />
```

En el ejemplo siguiente, el orden de `extra` y `@attributes` se invierte en el `<div>`del componente `Child`:

*ParentComponent. Razor*:

```razor
<ChildComponent extra="10" />
```

*ChildComponent. Razor*:

```razor
<div extra="5" @attributes="AdditionalAttributes" />

[Parameter(CaptureUnmatchedValues = true)]
public IDictionary<string, object> AdditionalAttributes { get; set; }
```

El `<div>` representado en el componente `Parent` contiene `extra="10"` cuando se pasa a través del atributo adicional:

```html
<div extra="10" />
```

## <a name="capture-references-to-components"></a>Capturar referencias a componentes

Las referencias de componente proporcionan una manera de hacer referencia a una instancia de componente para que pueda emitir comandos a esa instancia, como `Show` o `Reset`. Para capturar una referencia de componente:

* Agregue un atributo [`@ref`](xref:mvc/views/razor#ref) al componente secundario.
* Defina un campo con el mismo tipo que el componente secundario.

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

Cuando se representa el componente, el campo de `_loginDialog` se rellena con la instancia del componente secundario `MyLoginDialog`. Después, puede invocar métodos .NET en la instancia del componente.

> [!IMPORTANT]
> La variable `_loginDialog` solo se rellena después de que el componente se represente y su salida incluye el elemento `MyLoginDialog`. Hasta ese momento, no hay nada que hacer referencia. Para manipular las referencias de componentes una vez finalizada la representación del componente, use los [métodos OnAfterRenderAsync o OnAfterRender](xref:blazor/lifecycle#after-component-render).

Al capturar referencias de componentes, use una sintaxis similar para [capturar referencias de elemento](xref:blazor/javascript-interop#capture-references-to-elements), no es una característica de [interoperabilidad de JavaScript](xref:blazor/javascript-interop) . Las referencias a componentes no se pasan al código JavaScript&mdash;solo se usan en código .NET.

> [!NOTE]
> **No** utilice referencias de componentes para mutar el estado de los componentes secundarios. En su lugar, use parámetros declarativos normales para pasar datos a componentes secundarios. El uso de parámetros declarativos normales da como resultado componentes secundarios que se representarán automáticamente en las horas correctas.

## <a name="invoke-component-methods-externally-to-update-state"></a>Invocar métodos de componentes externamente para actualizar el estado

Blazor usa un `SynchronizationContext` para aplicar un único subproceso lógico de ejecución. [Los métodos de ciclo de vida](xref:blazor/lifecycle) de un componente y las devoluciones de llamada de eventos que se producen en Blazor se ejecutan en esta `SynchronizationContext`. En caso de que un componente deba actualizarse en función de un evento externo, como un temporizador u otras notificaciones, use el método `InvokeAsync`, que se enviará al `SynchronizationContext`de Blazor.

Por ejemplo, considere un *servicio de notificador* que puede notificar a cualquier componente de escucha del estado actualizado:

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

Registre el `NotifierService` como singletion:

* En Blazor webassembly, registre el servicio en `Program.Main`:

  ```csharp
  builder.Services.AddSingleton<NotifierService>();
  ```

* En Blazor Server, registre el servicio en `Startup.ConfigureServices`:

  ```csharp
  services.AddSingleton<NotifierService>();
  ```

Use el `NotifierService` para actualizar un componente:

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

En el ejemplo anterior, `NotifierService` invoca el método de `OnNotify` del componente fuera del `SynchronizationContext`de Blazor. `InvokeAsync` se utiliza para cambiar al contexto correcto y poner en cola una representación.

## <a name="use-key-to-control-the-preservation-of-elements-and-components"></a>Usar \@clave para controlar la preservación de elementos y componentes

Cuando se representa una lista de elementos o componentes, y los elementos o componentes cambian posteriormente, el algoritmo de comparación de Blazordebe decidir cuáles de los elementos o componentes anteriores se pueden conservar y cómo deben asignarse los objetos de modelo. Normalmente, este proceso es automático y se puede omitir, pero hay casos en los que puede que desee controlar el proceso.

Considere el ejemplo siguiente:

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

El contenido de la colección de `People` puede cambiar con entradas insertadas, eliminadas o reordenadas. Cuando se representa el componente, el componente de `<DetailsEditor>` puede cambiar para recibir diferentes valores de parámetro de `Details`. Esto puede producir una rerepresentación más compleja de lo esperado. En algunos casos, la rerepresentación puede producir diferencias de comportamiento visibles, como el foco del elemento perdido.

El proceso de asignación se puede controlar con el `@key` atributo de directiva. `@key` hace que el algoritmo de comparación garantice la preservación de elementos o componentes basados en el valor de la clave:

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

Cuando cambia la colección de `People`, el algoritmo de comparación mantiene la asociación entre `<DetailsEditor>` instancias y `person` instancias:

* Si se elimina un `Person` de la lista de `People`, solo se quita la instancia de `<DetailsEditor>` correspondiente de la interfaz de usuario. Otras instancias permanecen sin cambios.
* Si se inserta un `Person` en alguna posición de la lista, se inserta una nueva instancia de `<DetailsEditor>` en la posición correspondiente. Otras instancias permanecen sin cambios.
* Si se vuelven a ordenar `Person` entradas, se conservan y se vuelven a ordenar las instancias de `<DetailsEditor>` correspondientes en la interfaz de usuario.

En algunos escenarios, el uso de `@key` minimiza la complejidad de la rerepresentación y evita posibles problemas con las partes con estado del DOM que cambian, como la posición del foco.

> [!IMPORTANT]
> Las claves son locales para cada elemento contenedor o componente. Las claves no se comparan globalmente en todo el documento.

### <a name="when-to-use-key"></a>Cuándo usar la clave de \@

Normalmente, tiene sentido usar `@key` cada vez que se representa una lista (por ejemplo, en un bloque de `@foreach`) y existe un valor adecuado para definir el `@key`.

También puede usar `@key` para evitar que Blazor conserven un subárbol de elementos o componentes cuando cambie un objeto:

```razor
<div @key="currentPerson">
    ... content that depends on currentPerson ...
</div>
```

Si `@currentPerson` cambia, la Directiva de `@key` atributo fuerza a Blazor a descartar toda la `<div>` y sus descendientes y volver a generar el subárbol dentro de la interfaz de usuario con nuevos elementos y componentes. Esto puede ser útil si necesita garantizar que no se conserva ningún estado de la interfaz de usuario cuando `@currentPerson` cambios.

### <a name="when-not-to-use-key"></a>Cuándo no usar la clave \@

Existe un costo de rendimiento al diferenciar con `@key`. El costo de rendimiento no es grande, pero solo especifica `@key` si el control del elemento o las reglas de conservación de componentes se benefician de la aplicación.

Incluso si no se utiliza `@key`, Blazor conserva las instancias de componente y elemento secundario lo máximo posible. La única ventaja de utilizar `@key` es el control sobre *Cómo* se asignan las instancias de modelo a las instancias de componente conservadas, en lugar del algoritmo de diferenciación que selecciona la asignación.

### <a name="what-values-to-use-for-key"></a>Qué valores se deben usar para la clave de \@

Por lo general, tiene sentido proporcionar uno de los siguientes tipos de valor para `@key`:

* Instancias de objeto de modelo (por ejemplo, una instancia de `Person` como en el ejemplo anterior). Esto garantiza la conservación en función de la igualdad de la referencia de objeto.
* Los identificadores únicos (por ejemplo, los valores de clave principal de tipo `int`, `string`o `Guid`).

Asegúrese de que los valores usados para `@key` no entren en conflicto. Si se detectan valores en conflicto en el mismo elemento primario, Blazor produce una excepción porque no puede asignar de forma determinista elementos o componentes antiguos a nuevos elementos o componentes. Utilice solo valores distintos, como instancias de objeto o valores de clave principal.

## <a name="partial-class-support"></a>Compatibilidad de clases parciales

Los componentes de Razor se generan como clases parciales. Los componentes de Razor se crean mediante cualquiera de los métodos siguientes:

* C#el código se define en un bloque [`@code`](xref:mvc/views/razor#code) con marcado HTML y código Razor en un único archivo. Blazor plantillas definen sus componentes de Razor mediante este enfoque.
* C#el código se coloca en un archivo de código subyacente definido como una clase parcial.

En el ejemplo siguiente se muestra el componente de `Counter` predeterminado con un bloque de `@code` en una aplicación generada a partir de una plantilla de Blazor. El marcado HTML, el código Razor C# y el código se encuentran en el mismo archivo:

*Counter. Razor*:

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

El componente de `Counter` también se puede crear con un archivo de código subyacente con una clase parcial:

*Counter. Razor*:

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @_currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>
```

*Counter.Razor.CS*:

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

Agregue los espacios de nombres necesarios al archivo de clase parcial según sea necesario. Los espacios de nombres típicos utilizados por los componentes de Razor incluyen:

```csharp
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Components;
using Microsoft.AspNetCore.Components.Authorization;
using Microsoft.AspNetCore.Components.Forms;
using Microsoft.AspNetCore.Components.Routing;
using Microsoft.AspNetCore.Components.Web;
```

## <a name="specify-a-base-class"></a>Especificar una clase base

La directiva [`@inherits`](xref:mvc/views/razor#inherits) se puede utilizar para especificar una clase base para un componente. En el ejemplo siguiente se muestra cómo un componente puede heredar una clase base, `BlazorRocksBase`, para proporcionar las propiedades y los métodos del componente. La clase base debe derivar de `ComponentBase`.

*Pages/BlazorRocks. Razor*:

```razor
@page "/BlazorRocks"
@inherits BlazorRocksBase

<h1>@BlazorRocksText</h1>
```

*BlazorRocksBase.CS*:

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

## <a name="specify-an-attribute"></a>Especificar un atributo

Los atributos se pueden especificar en los componentes de Razor con la directiva [`@attribute`](xref:mvc/views/razor#attribute) . En el ejemplo siguiente se aplica el atributo `[Authorize]` a la clase Component:

```razor
@page "/"
@attribute [Authorize]
```

## <a name="import-components"></a>Importar componentes

El espacio de nombres de un componente creado con Razor se basa en (en orden de prioridad):

* [`@namespace`](xref:mvc/views/razor#namespace) designación en el marcado de archivos Razor ( *. Razor*) (`@namespace BlazorSample.MyNamespace`).
* `RootNamespace` del proyecto en el archivo de proyecto (`<RootNamespace>BlazorSample</RootNamespace>`).
* Nombre del proyecto, tomado del nombre de archivo del archivo de proyecto ( *. csproj*) y la ruta de acceso de la raíz del proyecto al componente. Por ejemplo, el marco de trabajo resuelve *{root Project}/pages/index.Razor* (*BlazorSample. csproj*) en el espacio de nombres `BlazorSample.Pages`. Los componentes C# siguen las reglas de enlace de nombres. En el caso del componente `Index` en este ejemplo, los componentes del ámbito son todos los componentes:
  * En la misma carpeta, *páginas*.
  * Los componentes de la raíz del proyecto que no especifican explícitamente un espacio de nombres diferente.

Los componentes definidos en un espacio de nombres diferente se incluyen en el ámbito mediante la Directiva de [`@using`](xref:mvc/views/razor#using) de Razor.

Si existe otro componente, `NavMenu.razor`, en la carpeta *BlazorSample/Shared/* , el componente se puede utilizar en `Index.razor` con la siguiente instrucción `@using`:

```razor
@using BlazorSample.Shared

This is the Index page.

<NavMenu></NavMenu>
```

También se puede hacer referencia a los componentes mediante sus nombres completos, que no requieren la directiva [`@using`](xref:mvc/views/razor#using) :

```razor
This is the Index page.

<BlazorSample.Shared.NavMenu></BlazorSample.Shared.NavMenu>
```

> [!NOTE]
> No se admite la calificación `global::`.
>
> No se admite la importación de componentes con instrucciones de `using` con alias (por ejemplo, `@using Foo = Bar`).
>
> No se admiten nombres parcialmente completos. Por ejemplo, no se admite agregar `@using BlazorSample` y hacer referencia a `NavMenu.razor` con `<Shared.NavMenu></Shared.NavMenu>`.

## <a name="conditional-html-element-attributes"></a>Atributos de elementos HTML condicionales

Los atributos de elemento HTML se representan condicionalmente según el valor de .NET. Si el valor es `false` o `null`, el atributo no se representa. Si el valor es `true`, el atributo se representa minimizado.

En el ejemplo siguiente, `IsCompleted` determina si `checked` se representa en el marcado del elemento:

```razor
<input type="checkbox" checked="@IsCompleted" />

@code {
    [Parameter]
    public bool IsCompleted { get; set; }
}
```

Si `IsCompleted` se `true`, la casilla se representa como:

```html
<input type="checkbox" checked />
```

Si `IsCompleted` se `false`, la casilla se representa como:

```html
<input type="checkbox" />
```

Para más información, consulte <xref:mvc/views/razor>.

> [!WARNING]
> Algunos atributos HTML, como [Aria-pressed](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), no funcionan correctamente cuando el tipo .net es un `bool`. En esos casos, use un tipo de `string` en lugar de un `bool`.

## <a name="raw-html"></a>HTML sin formato

Normalmente, las cadenas se representan mediante nodos de texto DOM, lo que significa que cualquier marcado que pueda contener se omite y se trata como texto literal. Para representar HTML sin formato, ajuste el contenido HTML en un valor de `MarkupString`. El valor se analiza como HTML o SVG y se inserta en el DOM.

> [!WARNING]
> La representación de HTML sin formato construido a partir de un origen que no es de confianza es un riesgo para la **seguridad** y debe evitarse.

En el ejemplo siguiente se muestra el uso del tipo de `MarkupString` para agregar un bloque de contenido HTML estático a la salida representada de un componente:

```html
@((MarkupString)_myMarkup)

@code {
    private string _myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="cascading-values-and-parameters"></a>Valores y parámetros en cascada

En algunos escenarios, no es conveniente fluir los datos de un componente antecesor a un componente descendiente mediante [parámetros de componente](#component-parameters), especialmente cuando hay varios niveles de componentes. Los valores y parámetros en cascada solucionan este problema proporcionando un método cómodo para que un componente antecesor proporcione un valor a todos sus componentes descendientes. Los valores y parámetros en cascada también proporcionan un enfoque para que los componentes se coordinen.

### <a name="theme-example"></a>Ejemplo de tema

En el ejemplo siguiente de la aplicación de ejemplo, la clase `ThemeInfo` especifica la información del tema que va a fluir hacia abajo en la jerarquía de componentes para que todos los botones de una parte determinada de la aplicación compartan el mismo estilo.

*UIThemeClasses/ThemeInfo. CS*:

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

Un componente antecesor puede proporcionar un valor en cascada mediante el componente de valor en cascada. El componente `CascadingValue` encapsula un subárbol de la jerarquía de componentes y proporciona un valor único a todos los componentes de ese subárbol.

Por ejemplo, la aplicación de ejemplo especifica información de tema (`ThemeInfo`) en uno de los diseños de la aplicación como parámetro en cascada para todos los componentes que componen el cuerpo del diseño de la propiedad `@Body`. a `ButtonClass` se le asigna un valor de `btn-success` en el componente de diseño. Cualquier componente descendiente puede consumir esta propiedad mediante el `ThemeInfo` objeto en cascada.

`CascadingValuesParametersLayout` componente:

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

Para usar valores en cascada, los componentes declaran parámetros en cascada mediante el atributo `[CascadingParameter]`. Los valores en cascada se enlazan a los parámetros en cascada por tipo.

En la aplicación de ejemplo, el componente `CascadingValuesParametersTheme` enlaza el valor en cascada `ThemeInfo` a un parámetro en cascada. El parámetro se usa para establecer la clase CSS para uno de los botones mostrados por el componente.

`CascadingValuesParametersTheme` componente:

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

Para poner en cascada varios valores del mismo tipo en el mismo subárbol, proporcione una cadena de `Name` única para cada componente `CascadingValue` y su `CascadingParameter`correspondiente. En el ejemplo siguiente, dos componentes de `CascadingValue` en cascada son instancias diferentes de `MyCascadingType` por nombre:

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

En un componente descendiente, los parámetros en cascada reciben sus valores de los valores en cascada correspondientes del componente antecesor por nombre:

```razor
...

@code {
    [CascadingParameter(Name = "CascadeParam1")]
    protected MyCascadingType ChildCascadeParameter1 { get; set; }
    
    [CascadingParameter(Name = "CascadeParam2")]
    protected MyCascadingType ChildCascadeParameter2 { get; set; }
}
```

### <a name="tabset-example"></a>Ejemplo de TabSet

Los parámetros en cascada también permiten que los componentes colaboren en la jerarquía de componentes. Por ejemplo, considere el siguiente ejemplo de *TabSet* en la aplicación de ejemplo.

La aplicación de ejemplo tiene una interfaz `ITab` que las pestañas implementan:

[!code-csharp[](common/samples/3.x/BlazorWebAssemblySample/UIInterfaces/ITab.cs)]

El componente `CascadingValuesParametersTabSet` usa el componente `TabSet`, que contiene varios componentes `Tab`:

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

Los componentes de `Tab` secundarios no se pasan explícitamente como parámetros a la `TabSet`. En su lugar, los componentes secundarios `Tab` forman parte del contenido secundario del `TabSet`. Sin embargo, el `TabSet` todavía necesita saber sobre cada componente `Tab` para que pueda representar los encabezados y la pestaña activa. Para habilitar esta coordinación sin necesidad de código adicional, el componente de `TabSet` *puede proporcionarse como un valor en cascada* que los componentes de `Tab` descendientes recogen.

`TabSet` componente:

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/TabSet.razor)]

Los componentes de `Tab` descendientes capturan el `TabSet` contenedor como un parámetro en cascada, por lo que los componentes de `Tab` se agregan a los `TabSet` y coordenadas en los que la pestaña está activa.

`Tab` componente:

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/Tab.razor)]

## <a name="razor-templates"></a>Plantillas de Razor

Los fragmentos de representación se pueden definir mediante la sintaxis de plantilla de Razor. Las plantillas de Razor son una manera de definir un fragmento de la interfaz de usuario y suponer el siguiente formato:

```razor
@<{HTML tag}>...</{HTML tag}>
```

En el ejemplo siguiente se muestra cómo especificar los valores de `RenderFragment` y `RenderFragment<T>` y las plantillas de representación directamente en un componente. Los fragmentos de representación también se pueden pasar como argumentos a [componentes con plantilla](xref:blazor/templated-components).

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

Salida representada del código anterior:

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Pet: Rex</p>
```

## <a name="scalable-vector-graphics-svg-images"></a>Imágenes de gráficos vectoriales escalables (SVG)

Dado que Blazor representa imágenes HTML, compatibles con el explorador, incluidas las imágenes SVG (Scalable Vector Graphics) ( *. svg*), se admiten a través de la etiqueta `<img>`:

```html
<img alt="Example image" src="some-image.svg" />
```

Del mismo modo, las imágenes SVG se admiten en las reglas CSS de un archivo de hoja de estilos ( *. CSS*):

```css
.my-element {
    background-image: url("some-image.svg");
}
```

Sin embargo, el marcado SVG en línea no se admite en todos los escenarios. Si coloca una etiqueta de `<svg>` directamente en un archivo de componente ( *. Razor*), se admite la representación de imágenes básica, pero aún no se admiten muchos escenarios avanzados. Por ejemplo, `<use>` etiquetas no se respetan actualmente y `@bind` no se pueden usar con algunas etiquetas SVG. Esperamos abordar estas limitaciones en una versión futura.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:security/blazor/server> &ndash; incluye instrucciones sobre la creación de aplicaciones de Blazor Server que deben competir con el agotamiento de recursos.
