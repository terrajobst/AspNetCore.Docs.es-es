---
title: Crear y usar ASP.NET Core componentes de Razor
author: guardrex
description: Obtenga información sobre cómo crear y usar componentes de Razor, incluido cómo enlazar a datos, controlar eventos y administrar ciclos de vida de componentes.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- Blazor
uid: blazor/components
ms.openlocfilehash: a79202565f45b4d26e280427892ea16b33f3f853
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/09/2019
ms.locfileid: "74943867"
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

> [!NOTE]
> En las versiones preliminares anteriores de ASP.NET Core 3,0, `@functions` bloques se usaban para el mismo propósito que `@code` bloques en los componentes de Razor. `@functions` bloques continúan funcionando en los componentes de Razor, pero se recomienda usar el bloque de `@code` en ASP.NET Core 3,0 Preview 6 o posterior.

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

Los componentes son C# clases normales y se pueden colocar en cualquier parte dentro de un proyecto. Los componentes que generan páginas web normalmente residen en la carpeta *pages* . Los componentes que no son de página se colocan con frecuencia en la carpeta *compartida* o en una carpeta personalizada agregada al proyecto. Para usar una carpeta personalizada, agregue el espacio de nombres de la carpeta personalizada al componente primario o al archivo *_Imports. Razor* de la aplicación. Por ejemplo, el siguiente espacio de nombres hace que los componentes de una carpeta *Components* estén disponibles cuando se `WebApplication`el espacio de nombres raíz de la aplicación:

```razor
@using WebApplication.Components
```

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a>Integración de componentes en aplicaciones de Razor Pages y MVC

Usar componentes con las aplicaciones de Razor Pages y MVC existentes. No es necesario volver a escribir las páginas o vistas existentes para utilizar los componentes de Razor. Cuando se representa la página o la vista, los componentes se preprocesan al mismo tiempo.

::: moniker range=">= aspnetcore-3.1"

Para representar un componente de una página o vista, use la aplicación auxiliar de etiquetas `Component`:

```cshtml
<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-IncrementAmount="10" />
```

Se admite el paso de parámetros (por ejemplo, `IncrementAmount` en el ejemplo anterior).

`RenderMode` configura si el componente:

* Se representa en la página.
* Se representa como HTML estático en la página o si incluye la información necesaria para arrancar una aplicación Blazor desde el agente de usuario.

| `RenderMode`        | Descripción |
| ------------------- | ----------- |
| `ServerPrerendered` | Representa el componente en código HTML estático e incluye un marcador para una aplicación de Blazor Server. Cuando se inicia el agente de usuario, este marcador se usa para arrancar una aplicación Blazor. |
| `Server`            | Representa un marcador para una aplicación de Blazor Server. La salida del componente no está incluida. Cuando se inicia el agente de usuario, este marcador se usa para arrancar una aplicación Blazor. |
| `Static`            | Representa el componente en HTML estático. |

Mientras que las páginas y las vistas pueden utilizar componentes, el opuesto no es cierto. Los componentes no pueden usar escenarios específicos de la página y de la vista, como vistas y secciones parciales. Para usar la lógica de la vista parcial en un componente, se debe factorizar la lógica de vista parcial en un componente.

No se admite la representación de componentes de servidor desde una página HTML estática.

Para obtener más información sobre cómo se representan los componentes, el estado del componente y la aplicación auxiliar de etiquetas `Component`, vea <xref:blazor/hosting-models>.

::: moniker-end

::: moniker range="< aspnetcore-3.1"

Para representar un componente de una página o vista, use el método auxiliar HTML `RenderComponentAsync<TComponent>`:

```cshtml
@(await Html.RenderComponentAsync<MyComponent>(RenderMode.ServerPrerendered))
```

`RenderMode` configura si el componente:

* Se representa en la página.
* Se representa como HTML estático en la página o si incluye la información necesaria para arrancar una aplicación Blazor desde el agente de usuario.

| `RenderMode`        | Descripción |
| ------------------- | ----------- |
| `ServerPrerendered` | Representa el componente en código HTML estático e incluye un marcador para una aplicación de Blazor Server. Cuando se inicia el agente de usuario, este marcador se usa para arrancar una aplicación Blazor. No se admiten los parámetros. |
| `Server`            | Representa un marcador para una aplicación de Blazor Server. La salida del componente no está incluida. Cuando se inicia el agente de usuario, este marcador se usa para arrancar una aplicación Blazor. No se admiten los parámetros. |
| `Static`            | Representa el componente en HTML estático. Se admiten los parámetros. |

Mientras que las páginas y las vistas pueden utilizar componentes, el opuesto no es cierto. Los componentes no pueden usar escenarios específicos de la página y de la vista, como vistas y secciones parciales. Para usar la lógica de la vista parcial en un componente, se debe factorizar la lógica de vista parcial en un componente.

No se admite la representación de componentes de servidor desde una página HTML estática.

Para obtener más información sobre cómo se representan los componentes, el estado del componente y la aplicación auxiliar HTML `RenderComponentAsync`, vea <xref:blazor/hosting-models>.

::: moniker-end

## <a name="use-components"></a>Uso de componentes

Los componentes pueden incluir otros componentes declarándolos mediante la sintaxis de elementos HTML. El marcado para utilizar un componente se parece a una etiqueta HTML en la que el nombre de la etiqueta es el tipo de componente.

El enlace de atributo distingue mayúsculas de minúsculas. Por ejemplo, `@bind` es válido y `@Bind` no es válido.

El marcado siguiente en *index. Razor* representa una instancia de `HeadingComponent`:

```razor
<HeadingComponent />
```

*Components/HeadingComponent. Razor*:

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/HeadingComponent.razor)]

Si un componente contiene un elemento HTML con una primera letra mayúscula que no coincide con un nombre de componente, se emite una advertencia que indica que el elemento tiene un nombre inesperado. La adición de una instrucción `@using` para el espacio de nombres del componente hace que el componente esté disponible, lo que elimina la advertencia.

## <a name="component-parameters"></a>Parámetros del componente

Los componentes pueden tener *parámetros de componente*, que se definen mediante propiedades públicas en la clase de componente con el atributo `[Parameter]`. Use atributos para especificar argumentos para un componente en el marcado.

*Components/ChildComponent. Razor*:

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=11-12)]

En el ejemplo siguiente de la aplicación de ejemplo, el `ParentComponent` establece el valor de la propiedad `Title` de la `ChildComponent`.

*Pages/ParentComponent. Razor*:

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

## <a name="child-content"></a>Contenido secundario

Los componentes pueden establecer el contenido de otro componente. El componente de asignación proporciona el contenido entre las etiquetas que especifican el componente receptor.

En el ejemplo siguiente, el `ChildComponent` tiene una propiedad `ChildContent` que representa un `RenderFragment`, que representa un segmento de interfaz de usuario que se va a representar. El valor de `ChildContent` se coloca en el marcado del componente en el que se debe representar el contenido. El valor de `ChildContent` se recibe del componente primario y se representa dentro del `panel-body`del panel de arranque.

*Components/ChildComponent. Razor*:

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=3,14-15)]

> [!NOTE]
> La propiedad que recibe el contenido del `RenderFragment` debe denominarse `ChildContent` por Convención.

El `ParentComponent` de la aplicación de ejemplo puede proporcionar contenido para representar el `ChildComponent` colocando el contenido dentro de las etiquetas de `<ChildComponent>`.

*Pages/ParentComponent. Razor*:

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

## <a name="data-binding"></a>Enlace de datos

El enlace de datos tanto a componentes como a elementos DOM se realiza con el [`@bind`](xref:mvc/views/razor#bind) atributo. En el ejemplo siguiente se enlaza una propiedad `CurrentValue` al valor del cuadro de texto:

```razor
<input @bind="CurrentValue" />

@code {
    private string CurrentValue { get; set; }
}
```

Cuando el cuadro de texto pierde el foco, se actualiza el valor de la propiedad.

El cuadro de texto se actualiza en la interfaz de usuario solo cuando se representa el componente, no en respuesta a cambiar el valor de la propiedad. Puesto que los componentes se representan por sí solos después de que se ejecute el código del controlador de eventos, las actualizaciones de propiedades *normalmente* se reflejan en la interfaz de usuario inmediatamente después de desencadenarse un controlador de eventos.

El uso de `@bind` con la propiedad `CurrentValue` (`<input @bind="CurrentValue" />`) es esencialmente equivalente a lo siguiente:

```razor
<input value="@CurrentValue"
    @onchange="@((ChangeEventArgs __e) => CurrentValue = 
        __e.Value.ToString())" />
        
@code {
    private string CurrentValue { get; set; }
}
```

Cuando se representa el componente, el `value` del elemento de entrada procede de la propiedad `CurrentValue`. Cuando el usuario escribe en el cuadro de texto y cambia el foco del elemento, se desencadena el evento `onchange` y la propiedad `CurrentValue` se establece en el valor modificado. En realidad, la generación de código es más compleja porque `@bind` administra los casos en los que se realizan las conversiones de tipos. En principio, `@bind` asocia el valor actual de una expresión con un atributo `value` y controla los cambios mediante el controlador registrado.

Además de controlar los eventos de `onchange` con sintaxis de `@bind`, se puede enlazar una propiedad o un campo mediante otros eventos mediante la especificación de un atributo de [`@bind-value`](xref:mvc/views/razor#bind) con un parámetro de `event` ([`@bind-value:event`](xref:mvc/views/razor#bind)). En el siguiente ejemplo se enlaza la propiedad `CurrentValue` del evento `oninput`:

```razor
<input @bind-value="CurrentValue" @bind-value:event="oninput" />

@code {
    private string CurrentValue { get; set; }
}
```

A diferencia de `onchange`, que se activa cuando el elemento pierde el foco, `oninput` se desencadena cuando cambia el valor del cuadro de texto.

**Valores no analizables**

Cuando un usuario proporciona un valor que no se pueda analizar a un elemento DataBound, el valor no analizable se revierte automáticamente a su valor anterior cuando se desencadena el evento de enlace.

Considere el caso siguiente:

* Un elemento `<input>` se enlaza a un tipo `int` con un valor inicial de `123`:

  ```razor
  <input @bind="MyProperty" />

  @code {
      [Parameter]
      public int MyProperty { get; set; } = 123;
  }
  ```
* El usuario actualiza el valor del elemento a `123.45` en la página y cambia el foco del elemento.

En el escenario anterior, el valor del elemento se revierte a `123`. Cuando el valor `123.45` se rechaza en favor del valor original de `123`, el usuario entiende que no se aceptó su valor.

De forma predeterminada, el enlace se aplica al evento `onchange` del elemento (`@bind="{PROPERTY OR FIELD}"`). Use `@bind-value="{PROPERTY OR FIELD}" @bind-value:event={EVENT}` para establecer un evento diferente. En el caso del evento `oninput` (`@bind-value:event="oninput"`), la reversión se produce después de cualquier pulsación de tecla que introduzca un valor no analizable. Cuando el destino es el evento `oninput` con un tipo enlazado a `int`, se impide que un usuario escriba un carácter `.`. Se quita inmediatamente un carácter `.`, por lo que el usuario recibe comentarios inmediatos que solo se permiten números enteros. Hay escenarios en los que la reversión del valor del evento `oninput` no es ideal, por ejemplo, cuando se debe permitir que el usuario borre un valor `<input>` que no se puede analizar. Las alternativas incluyen:

* No utilice el evento `oninput`. Use el evento `onchange` predeterminado (`@bind="{PROPERTY OR FIELD}"`), donde no se revierte un valor no válido hasta que el elemento pierde el foco.
* Enlazar a un tipo que acepta valores NULL, como `int?` o `string`, y proporcionar una lógica personalizada para controlar las entradas no válidas.
* Use un [componente de validación de formulario](xref:blazor/forms-validation), como `InputNumber` o `InputDate`. Los componentes de validación de formularios tienen compatibilidad integrada para administrar entradas no válidas. Componentes de validación de formularios:
  * Permite que el usuario proporcione entradas no válidas y reciba errores de validación en la `EditContext`asociada.
  * Mostrar errores de validación en la interfaz de usuario sin interferir con el usuario al escribir datos de WebForm adicionales.

**Globalización**

`@bind` se da formato a los valores para mostrarlos y analizarlos con las reglas de la referencia cultural actual.

Se puede tener acceso a la referencia cultural actual desde la propiedad <xref:System.Globalization.CultureInfo.CurrentCulture?displayProperty=fullName>.

[CultureInfo. InvariantCulture](xref:System.Globalization.CultureInfo.InvariantCulture) se usa para los siguientes tipos de campo (`<input type="{TYPE}" />`):

* `date`
* `number`

Los tipos de campo anteriores:

* Se muestran con sus reglas de formato basadas en explorador adecuadas.
* No puede contener texto de forma libre.
* Proporcionar características de interacción con el usuario en función de la implementación del explorador.

Los siguientes tipos de campo tienen requisitos de formato específicos y no se admiten actualmente en Blazor porque no son compatibles con todos los exploradores principales:

* `datetime-local`
* `month`
* `week`

`@bind` admite el parámetro `@bind:culture` para proporcionar un <xref:System.Globalization.CultureInfo?displayProperty=fullName> para analizar y dar formato a un valor. No se recomienda especificar una referencia cultural al usar los tipos de campo `date` y `number`. `date` y `number` tienen compatibilidad integrada con Blazor que proporciona la referencia cultural necesaria.

Para obtener información sobre cómo establecer la referencia cultural del usuario, consulte la sección [Localización](#localization).

**Cadenas de formato**

El enlace de datos funciona con cadenas de formato <xref:System.DateTime> mediante [`@bind:format`](xref:mvc/views/razor#bind). Otras expresiones de formato, como los formatos de moneda o número, no están disponibles en este momento.

```razor
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

En el código anterior, el tipo de campo del elemento de `<input>` (`type`) tiene como valor predeterminado `text`. `@bind:format` es compatible con el enlace de los siguientes tipos de .NET:

* <xref:System.DateTime?displayProperty=fullName>
* <xref:System.DateTime?displayProperty=fullName>?
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <xref:System.DateTimeOffset?displayProperty=fullName>?

El atributo `@bind:format` especifica el formato de fecha que se va a aplicar al `value` del elemento `<input>`. El formato también se usa para analizar el valor cuando se produce un evento `onchange`.

No se recomienda especificar un formato para el tipo de campo `date` porque Blazor tiene compatibilidad integrada para dar formato a las fechas. A pesar de la recomendación, use solo el formato de fecha `yyyy-MM-dd` para que el enlace funcione correctamente si se proporciona un formato con el tipo de campo `date`:

```razor
<input type="date" @bind="StartDate" @bind:format="yyyy-MM-dd">
```

**Parámetros de componente**

El enlace reconoce los parámetros de componente, donde `@bind-{property}` puede enlazar un valor de propiedad entre los componentes.

El siguiente componente secundario (`ChildComponent`) tiene un parámetro de componente `Year` y una devolución de llamada `YearChanged`:

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

`EventCallback<T>` se explica en la sección [EventCallback](#eventcallback) .

El siguiente componente primario utiliza `ChildComponent` y enlaza el parámetro `ParentYear` desde el elemento primario al parámetro `Year` en el componente secundario:

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

Al cargar el `ParentComponent` se produce el marcado siguiente:

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

Si el valor de la propiedad `ParentYear` se cambia seleccionando el botón en el `ParentComponent`, se actualiza la propiedad `Year` del `ChildComponent`. El nuevo valor de `Year` se representa en la interfaz de usuario cuando se representa el `ParentComponent`:

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

El parámetro `Year` es enlazable porque tiene un evento Companion `YearChanged` que coincide con el tipo del parámetro `Year`.

Por Convención, `<ChildComponent @bind-Year="ParentYear" />` es esencialmente equivalente a escribir:

```razor
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

En general, una propiedad se puede enlazar a un controlador de eventos correspondiente mediante `@bind-property:event` atributo. Por ejemplo, la propiedad `MyProp` se puede enlazar a `MyEventHandler` mediante los dos atributos siguientes:

```razor
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

## <a name="event-handling"></a>Control de eventos

Los componentes de Razor proporcionan características de control de eventos. Para un atributo de elemento HTML denominado `on{EVENT}` (por ejemplo, `onclick` y `onsubmit`) con un valor de tipo delegado, los componentes de Razor tratan el valor del atributo como un controlador de eventos. Siempre se da formato al nombre del atributo [`@on{EVENT}`](xref:mvc/views/razor#onevent).

El código siguiente llama al método `UpdateHeading` cuando el botón está seleccionado en la interfaz de usuario:

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

El código siguiente llama al método `CheckChanged` cuando se cambia la casilla en la interfaz de usuario:

```razor
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

Los controladores de eventos también pueden ser asincrónicos y devolver un <xref:System.Threading.Tasks.Task>. No es necesario llamar a [StateHasChanged](xref:blazor/lifecycle#state-changes)manualmente. Las excepciones se registran cuando se producen.

En el ejemplo siguiente, se llama a `UpdateHeading` de forma asincrónica cuando se selecciona el botón:

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

### <a name="event-argument-types"></a>Tipos de argumentos de evento

En algunos eventos, se permiten los tipos de argumento de evento. Si no es necesario el acceso a uno de estos tipos de evento, no es necesario en la llamada al método.

Los `EventArgs` admitidos se muestran en la tabla siguiente.

| Event            | Clase                | Eventos y notas de DOM |
| ---------------- | -------------------- | -------------------- |
| Portapapeles        | `ClipboardEventArgs` | `oncut`, `oncopy`, `onpaste` |
| Arrastrar             | `DragEventArgs`      | `ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`, `ondragend`<br><br>`DataTransfer` y `DataTransferItem` mantener los datos de los elementos arrastrados. |
| Error de :            | `ErrorEventArgs`     | `onerror` |
| Event            | `EventArgs`          | *General*<br>`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onended`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`<br><br>*Portapapeles*<br>`onbeforecut`, `onbeforecopy`, `onbeforepaste`<br><br>*Entrada*<br>`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart`, `onsubmit`<br><br>*Medios*<br>`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting` |
| Foco            | `FocusEventArgs`     | `onfocus`, `onblur`, `onfocusin`, `onfocusout`<br><br>No incluye compatibilidad con `relatedTarget`. |
| Input            | `ChangeEventArgs`    | `onchange`, `oninput` |
| Teclado         | `KeyboardEventArgs`  | `onkeydown`, `onkeypress`, `onkeyup` |
| Mouse            | `MouseEventArgs`     | `onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout` |
| Puntero    | `PointerEventArgs`   | `onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture` |
| Rueda del ratón      | `WheelEventArgs`     | `onwheel`, `onmousewheel` |
| Progreso         | `ProgressEventArgs`  | `onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress`, `ontimeout` |
| Entrada táctil            | `TouchEventArgs`     | `ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave`, `ontouchcancel`<br><br>`TouchPoint` representa un único punto de contacto en un dispositivo con distinción de toque. |

Para obtener información sobre las propiedades y el comportamiento de control de eventos de los eventos de la tabla anterior, vea [clases EventArgs en el origen de referencia (rama ASPNET/AspNetCore Release/3.0)](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Components/Web/src/Web).

### <a name="lambda-expressions"></a>Expresiones lambda

También se pueden usar expresiones lambda:

```razor
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

A menudo resulta cómodo cerrar los valores adicionales, como al recorrer en iteración un conjunto de elementos. En el ejemplo siguiente se crean tres botones, cada uno de los cuales llama a `UpdateHeading` pasar un argumento de evento (`MouseEventArgs`) y su número de botón (`buttonNumber`) cuando se selecciona en la interfaz de usuario:

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
> **No** utilice la variable de bucle (`i`) en un bucle de `for` directamente en una expresión lambda. De lo contrario, todas las expresiones lambda usan la misma variable, lo que hace que el valor de `i`sea el mismo en todas las expresiones lambda. Capture siempre su valor en una variable local (`buttonNumber` en el ejemplo anterior) y, a continuación, úsela.

### <a name="eventcallback"></a>EventCallback

Un escenario común con los componentes anidados es el deseo de ejecutar el método de un componente primario cuando se produce un evento de componente secundario&mdash;por ejemplo, cuando se produce un evento de `onclick` en el elemento secundario. Para exponer eventos entre componentes, use una `EventCallback`. Un componente primario puede asignar un método de devolución de llamada al `EventCallback`de un componente secundario.

En el `ChildComponent` de la aplicación de ejemplo (*Components/ChildComponent. Razor*) se muestra cómo se configura el controlador de `onclick` de un botón para recibir un `EventCallback` delegado de la `ParentComponent`del ejemplo. El `EventCallback` se escribe con `MouseEventArgs`, que es adecuado para un evento de `onclick` desde un dispositivo periférico:

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=5-7,17-18)]

El `ParentComponent` establece el `EventCallback<T>` del elemento secundario en su método `ShowMessage`.

*Pages/ParentComponent. Razor*:

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

Cuando el botón está seleccionado en el `ChildComponent`:

* Se llama al método `ShowMessage` del `ParentComponent`. `messageText` se actualiza y se muestra en el `ParentComponent`.
* No se requiere una llamada a [StateHasChanged](xref:blazor/lifecycle#state-changes) en el método de la devolución de llamada (`ShowMessage`). se llama a `StateHasChanged` automáticamente para rerepresentar el `ParentComponent`, del mismo modo que los eventos secundarios desencadenan la rerepresentación de componentes en los controladores de eventos que se ejecutan dentro del elemento secundario.

`EventCallback` y `EventCallback<T>` permiten los delegados asincrónicos. `EventCallback<T>` tiene un tipo seguro y requiere un tipo de argumento específico. `EventCallback` tiene un tipo débil y permite cualquier tipo de argumento.

```razor
<p><b>@messageText</b></p>

@{ var message = "Default Text"; }

<ChildComponent 
    OnClick="@(async () => { await Task.Yield(); messageText = "Blaze It!"; })" />

@code {
    private string messageText;
}
```

Invocar un `EventCallback` o `EventCallback<T>` con `InvokeAsync` y esperar el <xref:System.Threading.Tasks.Task>:

```csharp
await callback.InvokeAsync(arg);
```

Use `EventCallback` y `EventCallback<T>` para el control de eventos y los parámetros de componente de enlace.

Prefiera el `EventCallback<T>` fuertemente tipado en `EventCallback`. `EventCallback<T>` proporciona mejores comentarios de errores a los usuarios del componente. Al igual que otros controladores de eventos de la interfaz de usuario, la especificación del parámetro Event es opcional. Use `EventCallback` cuando no haya ningún valor pasado a la devolución de llamada.

::: moniker range=">= aspnetcore-3.1"

### <a name="prevent-default-actions"></a>Impedir acciones predeterminadas

Use el atributo de directiva de [`@on{EVENT}:preventDefault`](xref:mvc/views/razor#oneventpreventdefault) para evitar la acción predeterminada de un evento.

Cuando se selecciona una clave en un dispositivo de entrada y el foco del elemento está en un cuadro de texto, un explorador muestra normalmente el carácter de la tecla en el cuadro de texto. En el ejemplo siguiente, el comportamiento predeterminado se evita especificando el atributo de directiva de `@onkeypress:preventDefault`. El contador se incrementa y la clave **+** no se captura en el valor del elemento `<input>`:

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

Especificar el `@on{EVENT}:preventDefault` atributo sin un valor es equivalente a `@on{EVENT}:preventDefault="true"`.

El valor del atributo también puede ser una expresión. En el ejemplo siguiente, `_shouldPreventDefault` es un campo `bool` establecido en `true` o `false`:

```razor
<input @onkeypress:preventDefault="_shouldPreventDefault" />
```

No es necesario un controlador de eventos para evitar la acción predeterminada. El controlador de eventos y evitar escenarios de acción predeterminados se pueden usar de forma independiente.

### <a name="stop-event-propagation"></a>Detener propagación de eventos

Use el atributo de directiva de [`@on{EVENT}:stopPropagation`](xref:mvc/views/razor#oneventstoppropagation) para detener la propagación de eventos.

En el ejemplo siguiente, al activar la casilla se impide que los eventos de clic de la segunda `<div>` secundaria se propaguen al `<div>`primario:

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

## <a name="chained-bind"></a>Enlace encadenado

Un escenario común es encadenar un parámetro enlazado a datos a un elemento Page en la salida del componente. Este escenario se denomina *enlace encadenado* porque se producen varios niveles de enlace simultáneamente.

No se puede implementar un enlace encadenado con `@bind` sintaxis en el elemento de la página. El controlador de eventos y el valor se deben especificar por separado. Sin embargo, un componente primario puede usar la sintaxis de `@bind` con el parámetro del componente.

El siguiente componente de `PasswordField` (*PasswordField. Razor*):

* Establece el valor de un elemento de `<input>` en una propiedad `Password`.
* Expone los cambios de la propiedad `Password` a un componente primario con un [EventCallback](#eventcallback).

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

El componente de `PasswordField` se usa en otro componente:

```razor
<PasswordField @bind-Password="password" />

@code {
    private string password;
}
```

Para realizar comprobaciones o errores de captura en la contraseña en el ejemplo anterior:

* Cree un campo de respaldo para `Password` (`password` en el siguiente código de ejemplo).
* Realice las comprobaciones o errores de captura en el establecedor de `Password`.

En el ejemplo siguiente se proporcionan comentarios inmediatos al usuario si se usa un espacio en el valor de la contraseña:

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

## <a name="capture-references-to-components"></a>Capturar referencias a componentes

Las referencias de componente proporcionan una manera de hacer referencia a una instancia de componente para que pueda emitir comandos a esa instancia, como `Show` o `Reset`. Para capturar una referencia de componente:

* Agregue un atributo [`@ref`](xref:mvc/views/razor#ref) al componente secundario.
* Defina un campo con el mismo tipo que el componente secundario.

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

Cuando se representa el componente, el campo de `loginDialog` se rellena con la instancia del componente secundario `MyLoginDialog`. Después, puede invocar métodos .NET en la instancia del componente.

> [!IMPORTANT]
> La variable `loginDialog` solo se rellena después de que el componente se represente y su salida incluye el elemento `MyLoginDialog`. Hasta ese momento, no hay nada que hacer referencia. Para manipular las referencias de componentes una vez finalizada la representación del componente, use los [métodos OnAfterRenderAsync o OnAfterRender](xref:blazor/lifecycle#after-component-render).

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

Uso de la `NotifierService` para actualizar un componente:

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

## <a name="routing"></a>Enrutamiento

El enrutamiento en Blazor se consigue proporcionando una plantilla de ruta a cada componente accesible en la aplicación.

Cuando se compila un archivo de Razor con una directiva de `@page`, a la clase generada se le asigna un <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> que especifica la plantilla de ruta. En tiempo de ejecución, el enrutador busca clases de componentes con un `RouteAttribute` y representa el componente que tiene una plantilla de ruta que coincide con la dirección URL solicitada.

Se pueden aplicar varias plantillas de ruta a un componente. El siguiente componente responde a las solicitudes de `/BlazorRoute` y `/DifferentBlazorRoute`.

*Pages/BlazorRoute. Razor*:

```razor
@page "/BlazorRoute"
@page "/DifferentBlazorRoute"

<h1>Blazor routing</h1>
```

## <a name="route-parameters"></a>Parámetros de ruta

Los componentes pueden recibir parámetros de ruta de la plantilla de ruta proporcionada en la Directiva de `@page`. El enrutador usa parámetros de ruta para rellenar los parámetros de componente correspondientes.

*Pages/RouteParameter. Razor*:

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

Los parámetros opcionales no se admiten, por lo que se aplican dos directivas de `@page` en el ejemplo anterior. El primero permite la navegación al componente sin un parámetro. La segunda Directiva de `@page` toma el parámetro `{text}` Route y asigna el valor a la propiedad `Text`.

La sintaxis de *los parámetros catch-all* (`*`/`**`), que capturan la ruta de acceso en varios límites de carpeta, **no** se admite en los componentes de Razor ( *. Razor*).

::: moniker range=">= aspnetcore-3.1"

## <a name="partial-class-support"></a>Compatibilidad de clases parciales

Los componentes de Razor se generan como clases parciales. Los componentes de Razor se crean mediante cualquiera de los métodos siguientes:

* C#el código se define en un bloque [`@code`](xref:mvc/views/razor#code) con marcado HTML y código Razor en un único archivo. Blazor plantillas definen sus componentes de Razor mediante este enfoque.
* C#el código se coloca en un archivo de código subyacente definido como una clase parcial.

En el ejemplo siguiente se muestra el componente de `Counter` predeterminado con un bloque de `@code` en una aplicación generada a partir de una plantilla de Blazor. El marcado HTML, el código Razor C# y el código se encuentran en el mismo archivo:

*Counter. Razor*:

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

El componente de `Counter` también se puede crear con un archivo de código subyacente con una clase parcial:

*Counter. Razor*:

```razor
@page "/counter"

<h1>Counter</h1>

<p>Current count: @currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>
```

*Counter.Razor.CS*:

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

## <a name="specify-a-component-base-class"></a>Especificar una clase base de componente

La Directiva `@inherits` se puede utilizar para especificar una clase base para un componente.

La [aplicación de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) muestra cómo un componente puede heredar una clase base, `BlazorRocksBase`, para proporcionar las propiedades y los métodos del componente.

*Pages/BlazorRocks. Razor*:

```razor
@page "/BlazorRocks"
@inherits BlazorRocksBase

<h1>@BlazorRocksText</h1>
```

*BlazorRocksBase.cs*:

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

La clase base debe derivar de `ComponentBase`.

::: moniker-end

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

Para obtener más información, vea <xref:mvc/views/razor>.

> [!WARNING]
> Algunos atributos HTML, como [Aria-pressed](https://developer.mozilla.org/docs/Web/Accessibility/ARIA/Roles/button_role#Toggle_buttons), no funcionan correctamente cuando el tipo .net es un `bool`. En esos casos, use un tipo de `string` en lugar de un `bool`.

## <a name="raw-html"></a>HTML sin formato

Normalmente, las cadenas se representan mediante nodos de texto DOM, lo que significa que cualquier marcado que pueda contener se omite y se trata como texto literal. Para representar HTML sin formato, ajuste el contenido HTML en un valor de `MarkupString`. El valor se analiza como HTML o SVG y se inserta en el DOM.

> [!WARNING]
> La representación de HTML sin formato construido a partir de un origen que no es de confianza es un riesgo para la **seguridad** y debe evitarse.

En el ejemplo siguiente se muestra el uso del tipo de `MarkupString` para agregar un bloque de contenido HTML estático a la salida representada de un componente:

```html
@((MarkupString)myMarkup)

@code {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a>Componentes con plantilla

Los componentes con plantilla son componentes que aceptan una o varias plantillas de interfaz de usuario como parámetros, que se pueden usar como parte de la lógica de representación del componente. Los componentes con plantilla permiten crear componentes de nivel superior que son más reutilizables que los componentes normales. Algunos ejemplos son:

* Componente de tabla que permite a un usuario especificar plantillas para el encabezado, las filas y el pie de página de la tabla.
* Componente de lista que permite a un usuario especificar una plantilla para representar elementos en una lista.

### <a name="template-parameters"></a>Parámetros de plantilla

Un componente con plantilla se define especificando uno o más parámetros de componente de tipo `RenderFragment` o `RenderFragment<T>`. Un fragmento de representación representa un segmento de interfaz de usuario que se va a representar. `RenderFragment<T>` toma un parámetro de tipo que se puede especificar cuando se invoca el fragmento de representación.

`TableTemplate` componente:

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/TableTemplate.razor)]

Cuando se usa un componente con plantilla, los parámetros de plantilla se pueden especificar utilizando los elementos secundarios que coinciden con los nombres de los parámetros (`TableHeader` y `RowTemplate` en el ejemplo siguiente):

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

### <a name="template-context-parameters"></a>Parámetros de contexto de plantilla

Los argumentos de los componentes de tipo `RenderFragment<T>` pasados como elementos tienen un parámetro implícito denominado `context` (por ejemplo, en el ejemplo de código anterior, `@context.PetId`), pero puede cambiar el nombre del parámetro mediante el atributo `Context` en el elemento secundario. En el ejemplo siguiente, el atributo `Context` del elemento `RowTemplate` especifica el parámetro `pet`:

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

También puede especificar el atributo de `Context` en el elemento de componente. El atributo de `Context` especificado se aplica a todos los parámetros de plantilla especificados. Esto puede ser útil si desea especificar el nombre del parámetro de contenido para el contenido secundario implícito (sin ningún elemento secundario de ajuste). En el ejemplo siguiente, el atributo `Context` aparece en el elemento `TableTemplate` y se aplica a todos los parámetros de plantilla:

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

### <a name="generic-typed-components"></a>Componentes de tipo genérico

Los componentes con plantilla suelen tener tipos genéricos. Por ejemplo, se puede usar un componente de `ListViewTemplate` genérico para representar valores de `IEnumerable<T>`. Para definir un componente genérico, utilice la directiva [`@typeparam`](xref:mvc/views/razor#typeparam) para especificar parámetros de tipo:

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ListViewTemplate.razor)]

Cuando se usan componentes de tipo genérico, el parámetro de tipo se infiere si es posible:

```razor
<ListViewTemplate Items="pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

De lo contrario, el parámetro de tipo se debe especificar explícitamente mediante un atributo que coincida con el nombre del parámetro de tipo. En el ejemplo siguiente, `TItem="Pet"` especifica el tipo:

```razor
<ListViewTemplate Items="pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a>Valores y parámetros en cascada

En algunos escenarios, no es conveniente fluir los datos de un componente antecesor a un componente descendiente mediante [parámetros de componente](#component-parameters), especialmente cuando hay varios niveles de componentes. Los valores y parámetros en cascada solucionan este problema proporcionando un método cómodo para que un componente antecesor proporcione un valor a todos sus componentes descendientes. Los valores y parámetros en cascada también proporcionan un enfoque para que los componentes se coordinen.

### <a name="theme-example"></a>Ejemplo de tema

En el ejemplo siguiente de la aplicación de ejemplo, la clase `ThemeInfo` especifica la información del tema que va a fluir hacia abajo en la jerarquía de componentes para que todos los botones de una parte determinada de la aplicación compartan el mismo estilo.

*UIThemeClasses/ThemeInfo.cs*:

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

Para usar valores en cascada, los componentes declaran parámetros en cascada mediante el atributo `[CascadingParameter]`. Los valores en cascada se enlazan a los parámetros en cascada por tipo.

En la aplicación de ejemplo, el componente `CascadingValuesParametersTheme` enlaza el valor en cascada `ThemeInfo` a un parámetro en cascada. El parámetro se usa para establecer la clase CSS para uno de los botones mostrados por el componente.

`CascadingValuesParametersTheme` componente:

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

Para poner en cascada varios valores del mismo tipo en el mismo subárbol, proporcione una cadena de `Name` única para cada componente `CascadingValue` y su `CascadingParameter`correspondiente. En el ejemplo siguiente, dos componentes de `CascadingValue` en cascada son instancias diferentes de `MyCascadingType` por nombre:

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

En el ejemplo siguiente se muestra cómo especificar los valores de `RenderFragment` y `RenderFragment<T>` y las plantillas de representación directamente en un componente. Los fragmentos de representación también se pueden pasar como argumentos a [componentes con plantilla](#templated-components).

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

Salida representada del código anterior:

```html
<p>The time is 10/04/2018 01:26:52.</p>

<p>Your pet's name is Rex.</p>
```

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

En el ejemplo siguiente, el bucle del método `CreateComponent` genera tres componentes `PetDetails`. Al llamar a métodos `RenderTreeBuilder` para crear los componentes (`OpenComponent` y `AddAttribute`), los números de secuencia son números de línea de código fuente. El algoritmo de diferencia de Blazor se basa en los números de secuencia correspondientes a líneas de código distintas, no a invocaciones de llamada distintas. Al crear un componente con métodos de `RenderTreeBuilder`, codifique los argumentos para los números de secuencia. **El uso de un cálculo o un contador para generar el número de secuencia puede dar lugar a un rendimiento deficiente.** Para obtener más información, vea la sección [números de secuencia relacionados con números de línea de código y no orden de ejecución](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) .

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

> ! ATENCIÓN Los tipos de `Microsoft.AspNetCore.Components.RenderTree` permiten el procesamiento de los *resultados* de las operaciones de representación. Estos son los detalles internos de la implementación del marco de Blazor. Estos tipos se deben considerar *inestables* y estar sujetos a cambios en futuras versiones.

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a>Los números de secuencia se relacionan con los números de línea de código y no el orden de ejecución

los archivos de `.razor` de Blazor siempre se compilan. Esta es potencialmente una gran ventaja para `.razor` porque el paso de compilación se puede usar para insertar información que mejoran el rendimiento de las aplicaciones en tiempo de ejecución.

Un ejemplo clave de estas mejoras implican *los números de secuencia*. Los números de secuencia indican al tiempo de ejecución los resultados de los que proceden las líneas de código distintas y ordenadas. El motor en tiempo de ejecución utiliza esta información para generar diferencias de árbol eficientes en el tiempo lineal, que es mucho más rápido de lo que suele ser posible para un algoritmo de comparación de árboles generales.

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

| Secuencia | Tipo de      | Datos   |
| :------: | --------- | :----: |
| 0        | Nodo de texto | First  |
| 1        | Nodo de texto | Second |

Imagine que `someFlag` se `false`y que el marcado se representará de nuevo. Esta vez, el generador recibe:

| Secuencia | Tipo de       | Datos   |
| :------: | ---------- | :----: |
| 1        | Nodo de texto  | Second |

Cuando el tiempo de ejecución realiza una comparación, ve que se quitó el elemento en la secuencia `0`, por lo que genera el siguiente *script de edición*trivial:

* Quitar el primer nodo de texto.

#### <a name="what-goes-wrong-if-you-generate-sequence-numbers-programmatically"></a>Qué sucede si se generan números de secuencia mediante programación

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

| Secuencia | Tipo de      | Datos   |
| :------: | --------- | :----: |
| 0        | Nodo de texto | First  |
| 1        | Nodo de texto | Second |

Este resultado es idéntico al caso anterior, por lo que no existe ningún problema negativo. `someFlag` se `false` en la segunda representación y el resultado es:

| Secuencia | Tipo de      | Datos   |
| :------: | --------- | ------ |
| 0        | Nodo de texto | Second |

Esta vez, el algoritmo de comparación ve que se han producido *dos* cambios y el algoritmo genera el siguiente script de edición:

* Cambie el valor del primer nodo de texto a `Second`.
* Quite el segundo nodo de texto.

La generación de los números de secuencia ha perdido toda la información útil sobre el lugar en el que las bifurcaciones de `if/else` y los bucles estaban presentes en el código original. Esto da como resultado una diferencia **dos veces mayor que** antes.

Este es un ejemplo trivial. En casos más realistas con estructuras complejas y profundamente anidadas, y especialmente con bucles, el costo de rendimiento es más grave. En lugar de identificar inmediatamente qué bloques o ramas de bucle se han insertado o quitado, el algoritmo de comparación tiene que recurse profundamente en los árboles de representación y, normalmente, compilar scripts de edición mucho más largos porque está informada sobre cómo las estructuras antiguas y nuevas se relacionan entre sí.

#### <a name="guidance-and-conclusions"></a>Instrucciones y conclusiones

* El rendimiento de la aplicación se ve afectado si los números de secuencia se generan dinámicamente.
* El marco no puede crear sus propios números de secuencia automáticamente en tiempo de ejecución porque la información necesaria no existe a menos que se Capture en tiempo de compilación.
* No escriba grandes bloques de lógica de `RenderTreeBuilder` implementada de forma manual. Prefiere `.razor` archivos y permitir que el compilador trate los números de secuencia. Si no puede evitar la lógica de `RenderTreeBuilder` manual, divida bloques largos de código en fragmentos más pequeños encapsulados en `OpenRegion`/`CloseRegion` llamadas. Cada región tiene su propio espacio independiente de números de secuencia, por lo que puede reiniciar desde cero (o cualquier otro número arbitrario) dentro de cada región.
* Si los números de secuencia están codificados, el algoritmo de comparación solo requiere que los números de secuencia aumenten en valor. El valor inicial y los huecos son irrelevantes. Una opción legítima es usar el número de línea de código como el número de secuencia, o comenzar a partir de cero y aumentar por unos o cientos (o cualquier intervalo preferido). 
* Blazor usa los números de secuencia, mientras que otros marcos de interfaz de usuario de comparación de árboles no los utilizan. La diferenciación es mucho más rápida cuando se utilizan números de secuencia y Blazor tiene la ventaja de un paso de compilación que trata los números de secuencia automáticamente para desarrolladores que crean archivos *. Razor* .

## <a name="localization"></a>Localización

las aplicaciones de Blazor Server se localizan mediante [middleware de localización](xref:fundamentals/localization#localization-middleware). El middleware selecciona la referencia cultural adecuada para los usuarios que solicitan recursos de la aplicación.

La referencia cultural se puede establecer utilizando uno de los métodos siguientes:

* [Cookies](#cookies)
* [Proporcionar la interfaz de usuario para elegir la referencia cultural](#provide-ui-to-choose-the-culture)

Para obtener más información y ejemplos, vea <xref:fundamentals/localization>.

### <a name="configure-the-linker-for-internationalization-opno-locblazor-webassembly"></a>Configurar el enlazador para la internacionalización (Blazor webassembly)

De forma predeterminada, la configuración del enlazador de Blazorpara Blazor aplicaciones webassembly quita información de internacionalización, excepto para las configuraciones regionales solicitadas explícitamente. Para obtener más información e instrucciones sobre cómo controlar el comportamiento del enlazador, vea <xref:host-and-deploy/blazor/configure-linker#configure-the-linker-for-internationalization>.

### <a name="cookies"></a>Cookies

Una cookie de referencia cultural de localización puede conservar la referencia cultural del usuario. La cookie se crea mediante el método `OnGet` de la página host de la aplicación (*pages/host. cshtml. CS*). El middleware de localización lee la cookie en solicitudes posteriores para establecer la referencia cultural del usuario. 

El uso de una cookie garantiza que la conexión de WebSocket puede propagar correctamente la referencia cultural. Si los esquemas de localización se basan en la ruta de acceso URL o la cadena de consulta, es posible que el esquema no funcione con WebSockets, por lo que no se puede conservar la referencia cultural. Por lo tanto, el uso de una cookie de referencia cultural de localización es el enfoque recomendado.

Cualquier técnica se puede usar para asignar una referencia cultural si la referencia cultural se conserva en una cookie de localización. Si la aplicación ya tiene un esquema de localización establecido para ASP.NET Core del lado servidor, siga usando la infraestructura de localización existente de la aplicación y establezca la cookie de la cultura de localización en el esquema de la aplicación.

En el ejemplo siguiente se muestra cómo establecer la referencia cultural actual en una cookie que puede ser leída por el middleware de localización. Cree un archivo *pages/host. cshtml. CS* con el siguiente contenido en la aplicación de Blazor Server:

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

La localización se controla en la aplicación:

1. El explorador envía una solicitud HTTP inicial a la aplicación.
1. El middleware de localización asigna la referencia cultural.
1. El método `OnGet` en *_Host. cshtml. CS* conserva la referencia cultural en una cookie como parte de la respuesta.
1. El explorador abre una conexión WebSocket para crear una sesión interactiva de Blazor Server.
1. El middleware de localización lee la cookie y asigna la referencia cultural.
1. La sesión del servidor de Blazor comienza con la referencia cultural correcta.

### <a name="provide-ui-to-choose-the-culture"></a>Proporcionar la interfaz de usuario para elegir la referencia cultural

Para proporcionar una interfaz de usuario que permita a los usuarios seleccionar una referencia cultural, se recomienda un *enfoque basado en redirección* . El proceso es similar a lo que ocurre en una aplicación web cuando un usuario intenta acceder a un recurso seguro&mdash;se redirige al usuario a una página de inicio de sesión y, a continuación, se redirige de nuevo al recurso original. 

La aplicación conserva la referencia cultural seleccionada del usuario a través de una redirección a un controlador. El controlador establece la referencia cultural seleccionada del usuario en una cookie y redirige al usuario de nuevo al URI original.

Establezca un punto de conexión HTTP en el servidor para establecer la referencia cultural seleccionada del usuario en una cookie y volver a realizar la redirección al URI original:

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
> Use el resultado de la acción `LocalRedirect` para evitar ataques de redireccionamiento abierto. Para obtener más información, vea <xref:security/preventing-open-redirects>.

El siguiente componente muestra un ejemplo de cómo realizar la redirección inicial cuando el usuario selecciona una referencia cultural:

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

### <a name="use-net-localization-scenarios-in-opno-locblazor-apps"></a>Uso de escenarios de localización de .NET en Blazor aplicaciones

Dentro de Blazor aplicaciones, están disponibles los siguientes escenarios de localización y globalización de .NET:

* . Sistema de recursos de la red
* Formato de fecha y número específico de la referencia cultural

la funcionalidad de `@bind` de Blazorrealiza la globalización en función de la referencia cultural actual del usuario. Para obtener más información, vea la sección [enlace de datos](#data-binding) .

Actualmente se admite un conjunto limitado de escenarios de localización de ASP.NET Core:

* `IStringLocalizer<>` *se admite* en las aplicaciones de Blazor.
* la localización de `IHtmlLocalizer<>`, `IViewLocalizer<>`y las anotaciones de datos es ASP.NET Core escenarios MVC y **no se admiten** en Blazor aplicaciones.

Para obtener más información, vea <xref:fundamentals/localization>.

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
