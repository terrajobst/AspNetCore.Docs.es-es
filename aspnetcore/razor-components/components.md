---
title: Crear y usar componentes de Razor
author: guardrex
description: Obtenga información sobre cómo crear y usar componentes de Razor, incluida la forma de enlazar a datos, controlar los eventos y administrar los ciclos de vida del componente.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/08/2019
uid: razor-components/components
ms.openlocfilehash: f8ac7f3844b94a162e8d1c45f80ae153d89536ee
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/10/2019
ms.locfileid: "59515697"
---
# <a name="create-and-use-razor-components"></a>Crear y usar componentes de Razor

Por [Halter](https://github.com/guardrex), [Daniel Roth](https://github.com/danroth27), y [Morné Zaayman](https://github.com/MorneZaayman)

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) ([cómo descargarlo](xref:index#how-to-download-a-sample)). Consulte el tema [Introducción](xref:razor-components/get-started) para ver los requisitos previos.

Las aplicaciones de componentes de Razor se compilan mediante *componentes*. Un componente es un fragmento independiente de la interfaz de usuario (IU), como una página, cuadro de diálogo o formulario. Un componente incluye marcado HTML y la lógica de procesamiento necesario para insertar datos o responder a eventos de interfaz de usuario. Los componentes son flexibles y ligera. Puede anidar, volver a usar y compartirse entre proyectos.

## <a name="component-classes"></a>Clases de componentes

Los componentes se implementan normalmente en archivos de componentes de Razor (*.razor*) mediante una combinación de C# y marcado HTML (*.cshtml* archivos se usan en aplicaciones Blazor).

Los componentes se pueden crear en las aplicaciones de componentes de Razor con el *.cshtml* extensión de archivo, siempre se identifican los archivos como archivos de Razor componente mediante el `_RazorComponentInclude` propiedad de MSBuild. Por ejemplo, una aplicación creada con la plantilla de Razor componente especifica que todos los *.cshtml* archivos bajo la *componentes* carpeta debe tratarse como archivos de componentes de Razor:

```xml
<_RazorComponentInclude>Components\**\*.cshtml</_RazorComponentInclude>
```

La interfaz de usuario para un componente se define mediante HTML. La lógica de la representación dinámica (por ejemplo, bucles, instrucciones condicionales, expresiones) se agrega mediante una sintaxis de C# insertada denominada [Razor](xref:mvc/views/razor). Cuando se compila una aplicación de los componentes de Razor, el marcado HTML y C# lógica de representación se convierten en una clase de componente. El nombre de la clase generada coincide con el nombre del archivo.

Los miembros de la clase de componente se definen en un `@functions` bloque (más de un `@functions` bloque está permitido). En el `@functions` bloque, el estado de componente (propiedades, campos) se especifica con los métodos de control de eventos o para definir otra lógica de componente.

Los miembros del componente, a continuación, se pueden usar como parte del componente de la representación lógica usando C# expresiones que empiezan por `@`. Por ejemplo, un C# campo se representa con el prefijo `@` para el nombre del campo. En el ejemplo siguiente, se evalúa y se representa:

* `_headingFontStyle` el valor de propiedad CSS para `font-style`.
* `_headingText` el contenido de la `<h1>` elemento.

```cshtml
<h1 style="font-style:@_headingFontStyle">@_headingText</h1>

@functions {
    private string _headingFontStyle = "italic";
    private string _headingText = "Put on your new Blazor!";
}
```

Después de que el componente se representa inicialmente, el componente vuelve a generar su árbol de representación en respuesta a eventos. Componentes de Razor, a continuación, compara el nuevo árbol de representación en el anterior y se aplica ninguna modificación a Document Object Model (DOM) del explorador.

## <a name="integrate-components-into-razor-pages-and-mvc-apps"></a>Integrar componentes en aplicaciones de las páginas de Razor y MVC

Utilizar componentes con las aplicaciones existentes de las páginas de Razor y MVC. No hay ninguna necesidad de volver a escribir las páginas existentes o vistas a usar los componentes de Razor. Cuando se representa la página o vista, los componentes son representados&dagger; al mismo tiempo. 

> [!NOTE]
> &dagger;Procesamiento previo del lado servidor está habilitada para las aplicaciones de componentes de Razor de forma predeterminada. Las aplicaciones de cliente Blazor será compatible con procesamiento previo de la próxima versión Preview 4. Para obtener más información, consulte [actualizar plantillas/middleware para que use el archivo/MapFallbackToPage](https://github.com/aspnet/AspNetCore/issues/8852).

Para representar un componente de una página o vista, use el `RenderComponentAsync<TComponent>` método auxiliar HTML:

```cshtml
<div id="Counter">
    @(await Html.RenderComponentAsync<Counter>(new { IncrementAmount = 10 }))
</div>
```

Todavía no son interactivos en la versión Preview 3 componentes procesados a partir de las páginas y las vistas. Por ejemplo, la selección de un botón no desencadena una llamada al método. Una vista previa futura se solucione esta limitación y agregan compatibilidad para componentes de representación mediante la sintaxis normal de elementos y atributos.

Aunque las páginas y las vistas pueden usar los componentes, lo contrario no es cierto. Los componentes no pueden usar en escenarios específicos de página y vista, como las vistas parciales y secciones. Para utilizar lógica de vista parcial en un componente, descarte la lógica de vista parcial en un componente.

## <a name="using-components"></a>Uso de componentes

Los componentes pueden incluir otros componentes declarándolas mediante la sintaxis de elemento HTML. El marcado para utilizar un componente se parece a una etiqueta HTML en la que el nombre de la etiqueta es el tipo de componente.

El marcado siguiente representa un `HeadingComponent` (*HeadingComponent.cshtml*) instancia:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/Index.cshtml?name=snippet_HeadingComponent)]

## <a name="component-parameters"></a>Parámetros del componente

Pueden tener componentes *parámetros del componente*, que se definen mediante *no públicos* propiedades en la clase de componente decoran con `[Parameter]`. Use atributos para especificar argumentos para un componente en el marcado.

En el ejemplo siguiente, la `ParentComponent` establece el valor de la `Title` propiedad de la `ChildComponent`:

*ParentComponent.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.cshtml?name=snippet_ParentComponent&highlight=5)]

*ChildComponent.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ChildComponent.cshtml?highlight=7-8)]

## <a name="child-content"></a>Contenido secundario

Los componentes pueden establecer el contenido de otro componente. El componente de asignación proporciona el contenido entre las etiquetas que especifican el componente receptor. Por ejemplo, un `ParentComponent` puede proporcionar contenido para la representación por un componente secundario colocando el contenido dentro de `<ChildComponent>` etiquetas.

*ParentComponent.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ParentComponent.cshtml?name=snippet_ParentComponent&highlight=6-7)]

El componente secundario tiene un `ChildContent` propiedad que representa un `RenderFragment`. El valor de `ChildContent` se coloca en el marcado del componente secundario donde se debe representar el contenido. En el ejemplo siguiente, el valor de `ChildContent` se recibe desde el componente primario y se representa dentro del panel de Bootstrap `panel-body`.

*ChildComponent.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/ChildComponent.cshtml?highlight=3,10-11)]

> [!NOTE]
> La recepción de la propiedad la `RenderFragment` contenido debe denominarse `ChildContent` por convención.

## <a name="data-binding"></a>Enlace de datos

Enlace de datos a los componentes y los elementos DOM se logra con la `bind` atributo. En el ejemplo siguiente se enlaza el `ItalicsCheck` comprueba la propiedad a la casilla de verificación Estado:

```cshtml
<input type="checkbox" class="form-check-input" id="italicsCheck" 
    bind="@_italicsCheck">
```

Cuando se activa y se desactiva la casilla de verificación, el valor de propiedad se actualiza a `true` y `false`, respectivamente.

La casilla de verificación se actualiza en la interfaz de usuario solo cuando se procesa el componente, no en respuesta a cambiar el valor de propiedad. Puesto que los componentes de representan a sí mismos cuando se ejecuta el código del controlador de eventos, las actualizaciones de la propiedad normalmente aparecen en la interfaz de usuario inmediatamente.

Uso de `bind` con un `CurrentValue` propiedad (`<input bind="@CurrentValue">`) es esencialmente equivalente a lo siguiente:

```cshtml
<input value="@CurrentValue" 
    onchange="@((UIChangeEventArgs __e) => CurrentValue = __e.Value)">
```

Cuando se procesa el componente, el `value` del elemento de entrada procede de la `CurrentValue` propiedad. Cuando el usuario escribe en el cuadro de texto, el `onchange` desencadena el evento y el `CurrentValue` propiedad está establecida en el valor modificado. En realidad, la generación de código es un poco más complejo porque `bind` controla algunos de los casos donde se realizan las conversiones de tipos. En principio, `bind` asocia el valor actual de una expresión con un `value` cambios de atributo y controla mediante el controlador registrado.

Además `onchange`, la propiedad se puede enlazar con otros eventos tales como `oninput` ya que es más explícito acerca de qué se debe enlazar al:

```cshtml
<input type="text" bind-value-oninput="@CurrentValue">
```

A diferencia de `onchange`, `oninput` se desencadena para cada carácter que se introduce en el cuadro de texto.

**Cadenas de formato**

Enlace de datos funciona con <xref:System.DateTime> cadenas con formato. Otras expresiones de formato, como moneda o los formatos de número, no están disponibles en este momento.

```cshtml
<input bind="@StartDate" format-value="yyyy-MM-dd">

@functions {
    [Parameter]
    private DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

El `format-value` atributo especifica el formato de fecha para aplicarlo a la `value` de la `input` elemento. El formato también se usa para analizar el valor cuando un `onchange` se produce el evento.

**Parámetros del componente**

Enlace también reconoce los parámetros del componente, donde `bind-{property}` puede enlazar un valor de propiedad entre componentes.

Usa el siguiente componente `ChildComponent` y enlaza la `ParentYear` parámetro desde el elemento primario para el `Year` parámetro del componente secundario:

Componente primario:

```cshtml
@page "/ParentComponent"

<h1>Parent Component</h1>

<p>ParentYear: @ParentYear</p>

<ChildComponent bind-Year="@ParentYear" />

<button class="btn btn-primary" onclick="@ChangeTheYear">
    Change Year to 1986
</button>

@functions {
    [Parameter]
    private int ParentYear { get; set; } = 1978;

    private void ChangeTheYear()
    {
        ParentYear = 1986;
    }
}
```

Componente secundario:

```cshtml
<h2>Child Component</h2>

<p>Year: @Year</p>

@functions {
    [Parameter]
    private int Year { get; set; }

    [Parameter]
    private Action<int> YearChanged { get; set; }
}
```

Cargando el `ParentComponent` genera el marcado siguiente:

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

Si el valor de la `ParentYear` se cambia la propiedad seleccionando el botón en el `ParentComponent`, el `Year` propiedad de la `ChildComponent` se actualiza. El nuevo valor de `Year` se representa en la interfaz de usuario cuando el `ParentComponent` es el término:

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

El `Year` parámetro es enlazable porque tiene un complemento `YearChanged` evento que coincide con el tipo de la `Year` parámetro.

Por convención, `<ChildComponent bind-Year="@ParentYear" />` es esencialmente equivalente a escribir,

```cshtml
    <ChildComponent bind-Year-YearChanged="@ParentYear" />
```

En general, se puede enlazar una propiedad en un controlador de eventos correspondiente mediante `bind-property-event` atributo.

## <a name="event-handling"></a>Control de eventos

Los componentes de Razor proporcionan características de control de eventos. Para un atributo del elemento HTML denominado `on<event>` (por ejemplo, `onclick`, `onsubmit`) con un valor con tipo de delegado, los componentes de Razor trata el valor del atributo como un controlador de eventos. El nombre del atributo siempre se inicia con `on`.

El código siguiente llama el `UpdateHeading` método cuando se selecciona el botón en la interfaz de usuario:

```cshtml
<button class="btn btn-primary" onclick="@UpdateHeading">
    Update heading
</button>

@functions {
    private void UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

El código siguiente llama el `CheckboxChanged` método cuando se cambia la casilla de verificación en la interfaz de usuario:

```cshtml
<input type="checkbox" class="form-check-input" onchange="@CheckboxChanged">

@functions {
    private void CheckboxChanged()
    {
        ...
    }
}
```

Controladores de eventos también pueden ser asincrónico y devolver un <xref:System.Threading.Tasks.Task>. No hay ninguna necesidad de llamar manualmente a `StateHasChanged()`. Las excepciones se registran cuando se produzcan.

```cshtml
<button class="btn btn-primary" onclick="@UpdateHeading">
    Update heading
</button>

@functions {
    private async Task UpdateHeading(UIMouseEventArgs e)
    {
        ...
    }
}
```

En algunos casos, se permiten los tipos de argumento de evento específica del evento. Si el acceso a uno de estos tipos de evento no es necesario, no es necesario en la llamada al método.

La lista de argumentos de evento compatible es:

* UIEventArgs
* UIChangeEventArgs
* UIKeyboardEventArgs
* UIMouseEventArgs

También se pueden usar expresiones lambda:

```cshtml
<button onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

A menudo es conveniente cerrar sobre valores adicionales, como al recorrer en iteración un conjunto de elementos. En el siguiente ejemplo crea tres botones, cada uno de ellos llama `UpdateHeading` pasando un argumento de evento (`UIMouseEventArgs`) y su número de botón (`buttonNumber`) cuando se selecciona en la interfaz de usuario:

```cshtml
<h2>@message</h2>

@for (var i = 1; i < 4; i++)
{
    var buttonNumber = i;

    <button class="btn btn-primary"
            onclick="@(e => UpdateHeading(e, buttonNumber))">
        Button #@i
    </button>
}

@functions {
    private string message = "Select a button to learn its position.";

    private void UpdateHeading(UIMouseEventArgs e, int buttonNumber)
    {
        message = $"You selected Button #{buttonNumber} at " +
            "mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> Hacer **no** usar la variable de bucle (`i`) en un `for` bucle directamente en una expresión lambda. En caso contrario, se usa la misma variable por todas las expresiones lambda causando `i`del valor para que sean iguales en todas las expresiones lambda. Siempre captura su valor en una variable local (`buttonNumber` en el ejemplo anterior) y, a continuación, usarla.

## <a name="capture-references-to-components"></a>Capturar las referencias a componentes

Referencias de componentes proporcionan una manera de obtener una referencia a una instancia del componente para que pueda emitir comandos a esa instancia, como `Show` o `Reset`. Para capturar una referencia de componente, agregue un `ref` de atributo para el componente secundario y, a continuación, defina un campo con el mismo nombre y el mismo tipo que el componente secundario.

```cshtml
<MyLoginDialog ref="loginDialog" ... />

@functions {
    private MyLoginDialog loginDialog;

    private void OnSomething()
    {
        loginDialog.Show();
    }
}
```

Cuando se procesa el componente, el `loginDialog` campo se rellena con el `MyLoginDialog` instancia del componente secundario. A continuación, puede invocar métodos de .NET en la instancia del componente.

> [!IMPORTANT]
> El `loginDialog` variable solo se rellena cuando se procesa el componente y su salida incluye el `MyLoginDialog` elemento. Hasta ese punto, no hay nada que hacer referencia. Para manipular las referencias de componentes después de que el componente ha terminado de procesarse, utilice el `OnAfterRenderAsync` o `OnAfterRender` métodos.

Mientras que captura las referencias de componentes usa una sintaxis similar a [captura las referencias de elemento](xref:razor-components/javascript-interop#capture-references-to-elements), no es un [interoperabilidad de JavaScript](xref:razor-components/javascript-interop) característica. Referencias de componentes no se pasan a código de JavaScript; que se usan en el código. NET.

> [!NOTE]
> Hacer **no** usar referencias de componentes para el estado de los componentes secundarios se modifique. En su lugar, use parámetros declarativos normales para pasar datos a los componentes secundarios. Esto hace que los componentes secundarios procesar automáticamente en el momento adecuado.

## <a name="lifecycle-methods"></a>Métodos de ciclo de vida

`OnInitAsync` y `OnInit` ejecutar código para inicializar el componente. Para realizar una operación asincrónica, use `OnInitAsync` y `await` palabra clave en la operación:

```csharp
protected override async Task OnInitAsync()
{
    await ...
}
```

Para una operación sincrónica, use `OnInit`:

```csharp
protected override void OnInit()
{
    ...
}
```

`OnParametersSetAsync` y `OnParametersSet` se llama cuando un componente ha recibido los parámetros de su elemento primario y los valores se asignan a las propiedades. Estos métodos se ejecutan después de la inicialización de componente y, a continuación, cada vez que el componente se representa:

```csharp
protected override async Task OnParametersSetAsync()
{
    await ...
}
```

```csharp
protected override void OnParametersSet()
{
    ...
}
```

`OnAfterRenderAsync` y `OnAfterRender` se invocan después de un componente ha terminado de procesarse. Las referencias de elemento y el componente se rellenan en este momento. Use esta fase para realizar pasos de inicialización adicionales con el contenido representado, como la activación de bibliotecas de JavaScript de terceros que operan en los elementos DOM representados.

```csharp
protected override async Task OnAfterRenderAsync()
{
    await ...
}
```

```csharp
protected override void OnAfterRender()
{
    ...
}
```

`SetParameters` se puede invalidar para ejecutar código antes de que se establecen los parámetros:

```csharp
public override void SetParameters(ParameterCollection parameters)
{
    ...

    base.SetParameters(parameters);
}
```

Si `base.SetParameters` no invoca, el código personalizado puede interpretar el valor de los parámetros entrantes en cualquier forma necesarios. Por ejemplo, los parámetros de entrada no son necesarios para asignarse a las propiedades de la clase.

`ShouldRender` se puede invalidar para suprimir la actualización de la interfaz de usuario. Si la implementación devuelve `true`, se actualiza la interfaz de usuario. Aunque `ShouldRender` es invalidar, el componente se representa siempre inicialmente.

```csharp
protected override bool ShouldRender()
{
    var renderUI = true;

    return renderUI;
}
```

## <a name="component-disposal-with-idisposable"></a>Eliminación de componentes con IDisposable

Si implementa un componente <xref:System.IDisposable>, [método Dispose](/dotnet/standard/garbage-collection/implementing-dispose) se llama cuando se quita el componente de la interfaz de usuario. El siguiente componente usa `@implements IDisposable` y `Dispose` método:

```csharp
@using System
@implements IDisposable

...

@functions {
    public void Dispose()
    {
        ...
    }
}
```

## <a name="routing"></a>Enrutamiento

Enrutamiento en los componentes de Razor se logra al proporcionar una plantilla de ruta para cada componente puede tener acceso en la aplicación.

Cuando un *.cshtml* de archivos con un `@page` directiva se compila, se proporciona la clase generada una <xref:Microsoft.AspNetCore.Mvc.RouteAttribute> especificar la plantilla de ruta. En tiempo de ejecución, el enrutador busca las clases de componente con un `RouteAttribute` y representa el componente tiene una plantilla de ruta que coincida con la dirección URL solicitada.

Varias plantillas de ruta se pueden aplicar a un componente. El siguiente componente responde a las solicitudes de `/BlazorRoute` y `/DifferentBlazorRoute`:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.cshtml?name=snippet_BlazorRoute)]

## <a name="route-parameters"></a>Parámetros de ruta

Los componentes pueden recibir parámetros de ruta de la plantilla de ruta proporcionada en el `@page` directiva. El enrutador usa los parámetros de ruta para rellenar los parámetros correspondientes de componente.

*RouteParameter.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.cshtml?name=snippet_RouteParameter)]

No se admiten los parámetros opcionales, por lo que dos `@page` directivas se aplican en el ejemplo anterior. La primera permite la navegación al componente sin un parámetro. El segundo `@page` directiva toma el `{text}` parámetro de ruta y asigna el valor para el `Text` propiedad.

## <a name="base-class-inheritance-for-a-code-behind-experience"></a>Herencia de clase base para una experiencia de "código subyacente"

Los archivos de componente (*.cshtml*) mezclar código HTML y C# código en el mismo archivo de procesamiento. El `@inherits` directiva puede usarse para proporcionar una experiencia de "código" que separa el marcado del componente de código de procesamiento de las aplicaciones de componentes de Razor.

El [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) muestra cómo un componente puede heredar una clase base, `BlazorRocksBase`, para proporcionar las propiedades y los métodos del componente.

*BlazorRocks.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRocks.cshtml?name=snippet_BlazorRocks)]

*BlazorRocksBase.cs*:

[!code-csharp[](common/samples/3.x/BlazorSample/Pages/BlazorRocksBase.cs)]

La clase base debe derivarse de `ComponentBase`.

## <a name="razor-support"></a>Compatibilidad con Razor

**Directivas de Razor**

Directivas de Razor se muestran en la tabla siguiente.

| Directiva | Descripción |
| --------- | ----------- |
| [\@Funciones](xref:mvc/views/razor#section-5) | Agrega un C# bloque de código para un componente. |
| `@implements` | Implementa una interfaz para la clase de componente generado. |
| [\@Hereda](xref:mvc/views/razor#section-3) | Proporciona control total sobre la clase que hereda el componente. |
| [\@Insertar](xref:mvc/views/razor#section-4) | Permite servicios de inserción desde el [contenedor de servicios](xref:fundamentals/dependency-injection). Para más información, vea [Dependency injection into views](xref:mvc/views/dependency-injection) (Inserción de dependencias en vistas). |
| `@layout` | Especifica un componente de diseño. Componentes de diseño se utilizan para evitar la incoherencia y duplicación de código. |
| [\@Página](xref:razor-pages/index#razor-pages) | Especifica que el componente debe controlar las solicitudes directamente. El `@page` directiva se puede especificar con una ruta y los parámetros opcionales. A diferencia de las páginas de Razor, el `@page` directiva no tiene que ser la primera directiva en la parte superior del archivo. Para más información, vea [Enrutamiento](xref:razor-components/routing). |
| [\@Uso de](xref:mvc/views/razor#using) | Agrega el C# `using` la directiva a la clase de componente generado. |
| [\@addTagHelper](xref:mvc/views/razor#tag-helpers) | Usar `@addTagHelper` para utilizar un componente en un ensamblado diferente al ensamblado de la aplicación. |

**Atributos condicionales**

Los atributos se representan condicionalmente en función del valor. NET. Si el valor es `false` o `null`, no se representa el atributo. Si el valor es `true`, el atributo se representa minimizada.

En el ejemplo siguiente, `IsCompleted` determina si `checked` se representa en el marcado del control:

```cshtml
<input type="checkbox" checked="@IsCompleted">

@functions {
    [Parameter]
    private bool IsCompleted { get; set; }
}
```

Si `IsCompleted` es `true`, la casilla de verificación se representa como:

```html
<input type="checkbox" checked>
```

Si `IsCompleted` es `false`, la casilla de verificación se representa como:

```html
<input type="checkbox">
```

**Información adicional en Razor**

Para obtener más información sobre Razor, consulte el [referencia de sintaxis de Razor](xref:mvc/views/razor).

## <a name="raw-html"></a>Código HTML sin procesar

Las cadenas se normalmente representan mediante nodos de texto de DOM, lo que significa que cualquier marcado pueden contener se omite y se tratan como texto literal. Para representar HTML sin formato, ajustar el contenido HTML en un `MarkupString` valor. El valor se puede analizar como HTML o SVG y se inserta en el DOM.

> [!WARNING]
> Representación HTML sin formato que se construye a partir de cualquiera de no confianza origen es un **riesgos de seguridad** y debe evitarse.

El ejemplo siguiente muestra utilizando el `MarkupString` tipo para agregar un bloque de contenido HTML estático a la salida representada de un componente:

```html
@((MarkupString)myMarkup)

@functions {
    private string myMarkup = 
        "<p class='markup'>This is a <em>markup string</em>.</p>";
}
```

## <a name="templated-components"></a>Componentes con plantilla

Los componentes con plantilla son componentes que acepten una o varias plantillas de interfaz de usuario como parámetros, que pueden usarse como parte de la lógica de representación del componente. Los componentes con plantilla permiten crear componentes de nivel superior que son más reutilizables que componentes normales. Algunos ejemplos incluyen:

* Un componente de tabla que permite al usuario especificar plantillas para filas, encabezado y pie de la tabla.
* Un componente de lista que permite al usuario especificar una plantilla para representar los elementos en una lista.

### <a name="template-parameters"></a>Parámetros de plantilla

Un componente basado en plantilla se define mediante la especificación de uno o varios parámetros del componente de tipo `RenderFragment` o `RenderFragment<T>`. Un fragmento de representación representa un segmento de la interfaz de usuario que representa el componente. Opcionalmente, un fragmento de representación toma un parámetro que se puede especificar cuando se invoca el fragmento de representación.

*Components/TableTemplate.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TableTemplate.cshtml)]

Cuando se usa un componente con plantilla, se pueden especificar los parámetros de plantilla utilizando los elementos secundarios que coinciden con los nombres de los parámetros (`TableHeader` y `RowTemplate` en el ejemplo siguiente):

```cshtml
<TableTemplate Items="@pets">
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

### <a name="template-context-parameters"></a>Parámetros de contexto de la plantilla

Argumentos de componente de tipo `RenderFragment<T>` pasa como elementos tienen un parámetro implícito denominado `context` (por ejemplo, de ejemplo de código anterior, `@context.PetId`), pero se puede cambiar el nombre de parámetro mediante el `Context` atributo en el elemento secundario elemento. En el ejemplo siguiente, la `RowTemplate` del elemento `Context` atributo especifica el `pet` parámetro:

```cshtml
<TableTemplate Items="@pets">
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

Como alternativa, puede especificar el `Context` atributo del elemento de componente. Especificado `Context` atributo se aplica a todos los parámetros de plantilla especificada. Esto puede ser útil cuando desee especificar el nombre del parámetro de contenido para el contenido secundario implícita (sin ajuste de cualquier elemento secundario). En el ejemplo siguiente, la `Context` atributo aparece en el `TableTemplate` elemento y se aplica a todos los parámetros de plantilla:

```cshtml
<TableTemplate Items="@pets" Context="pet">
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

A menudo genéricamente se escriben los componentes con plantilla. Por ejemplo, se puede usar un componente de la plantilla de vista de lista genérico para presentar `IEnumerable<T>` valores. Para definir un componente genérico, utilice el `@typeparam` directiva para especificar los parámetros de tipo.

*Components/ListViewTemplate.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/ListViewTemplate.cshtml)]

Al usar los componentes de tipo genérico, el parámetro de tipo se deduce si es posible:

```cshtml
<ListViewTemplate Items="@pets">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

En caso contrario, el parámetro de tipo debe especificarse explícitamente mediante un atributo que coincida con el nombre del parámetro de tipo. En el ejemplo siguiente, `TItem="Pet"` especifica el tipo:

```cshtml
<ListViewTemplate Items="@pets" TItem="Pet">
    <ItemTemplate Context="pet">
        <li>@pet.Name</li>
    </ItemTemplate>
</ListViewTemplate>
```

## <a name="cascading-values-and-parameters"></a>Parámetros y valores en cascada

En algunos escenarios, es conveniente a los datos de flujo de un componente de antecesor en un componente descendiente mediante [parámetros del componente](#component-parameters), especialmente cuando hay varios niveles de componente. Los valores y parámetros en cascada solucionan este problema proporcionando una manera conveniente para un componente de antecesor que proporcione un valor para todos sus componentes descendientes. Los valores y parámetros en cascada también proporcionan un enfoque para coordinar los componentes.

### <a name="theme-example"></a>Ejemplo de tema

En la siguiente *tema* ejemplo desde la aplicación de ejemplo, el `ThemeInfo` clase especifica la información del tema para que fluyan hacia abajo de la jerarquía de componentes para que todos los botones dentro de una parte determinada de la aplicación comparten el mismo estilo.

*UIThemeClasses/ThemeInfo.cs*:

```csharp
public class ThemeInfo
{
    public string ButtonClass { get; set; }
}
```

Un componente de antecesor puede proporcionar un valor en cascada mediante el componente de valor en cascada. El componente de valor en cascada ajusta un subárbol de la jerarquía de componentes y proporciona un valor único a todos los componentes dentro de ese subárbol.

Por ejemplo, la aplicación de ejemplo especifica la información del tema (`ThemeInfo`) en uno de los diseños de la aplicación como un parámetro en cascada para todos los componentes que constituyen el cuerpo de diseño de la `@Body` propiedad. `ButtonClass` se asigna un valor de `btn-success` en el componente de diseño. Cualquier componente descendiente puede consumir esta propiedad mediante la `ThemeInfo` objeto en cascada.

*Shared/CascadingValuesParametersLayout.cshtml*:

```cshtml
@inherits LayoutComponentBase
@using BlazorSample.UIThemeClasses

<div class="container-fluid">
    <div class="row">
        <div class="col-sm-3">
            <NavMenu />
        </div>
        <div class="col-sm-9">
            <CascadingValue Value="@theme">
                <div class="content px-4">
                    @Body
                </div>
            </CascadingValue>
        </div>
    </div>
</div>

@functions {
    private ThemeInfo theme = new ThemeInfo { ButtonClass = "btn-success" };
}
```

Para hacer uso de valores en cascada, los componentes declarar parámetros en cascada mediante el `[CascadingParameter]` de atributo o según un valor de nombre de cadena:

```cshtml
<CascadingValue Value=@PermInfo Name="UserPermissions">...</CascadingValue>

[CascadingParameter(Name = "UserPermissions")]
private PermInfo Permissions { get; set; }
```

Enlace con un valor de nombre de la cadena es relevante si tiene varios valores en cascada del mismo tipo y necesita diferenciarlas dentro del mismo subárbol.

Los valores en cascada se enlazan a los parámetros en cascada por tipo.

En la aplicación de ejemplo, el componente de tema de los parámetros de valores en cascada se enlaza a la `ThemeInfo` valor en cascada a un parámetro en cascada. El parámetro se usa para establecer la clase CSS para uno de los botones mostrados por el componente.

*Pages/CascadingValuesParametersTheme.cshtml*:

```cshtml
@page "/cascadingvaluesparameterstheme"
@layout CascadingValuesParametersLayout
@using BlazorSample.UIThemeClasses

<h1>Cascading Values & Parameters</h1>

<p>Current count: @currentCount</p>

<p>
    <button class="btn" onclick="@IncrementCount">
        Increment Counter (Unthemed)
    </button>
</p>

<p>
    <button class="btn @ThemeInfo.ButtonClass" onclick="@IncrementCount">
        Increment Counter (Themed)
    </button>
</p>

@functions {
    private int currentCount = 0;

    [CascadingParameter] protected ThemeInfo ThemeInfo { get; set; }

    private void IncrementCount()
    {
        currentCount++;
    }
}
```

### <a name="tabset-example"></a>Ejemplo de TabSet

Los parámetros en cascada también habilitar componentes colaborar a través de la jerarquía de componentes. Por ejemplo, considere la siguiente *TabSet* ejemplo en la aplicación de ejemplo.

La aplicación de ejemplo tiene un `ITab` interfaz que implemente de pestañas:

[!code-cs[](common/samples/3.x/BlazorSample/UIInterfaces/ITab.cs)]

El componente de TabSet de parámetros de valores en cascada utiliza el componente de conjunto de pestañas, que contiene varios componentes de la pestaña:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/CascadingValuesParametersTabSet.cshtml?name=snippet_TabSet)]

Los componentes de la pestaña secundarios explícitamente no se pasan como parámetros para el conjunto de pestañas. En su lugar, los componentes de la pestaña secundarios forman parte del contenido secundario de la pestaña configurar. Sin embargo, el conjunto de pestañas todavía necesita saber acerca de cada componente de la pestaña para que pueda representar los encabezados y la pestaña activa. Para habilitar esta coordinación sin necesidad de código adicional, el componente de conjunto de pestañas *puede proporcionar como un valor en cascada* que, a continuación, recoge los componentes de la pestaña descendientes.

*Components/TabSet.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/TabSet.cshtml)]

La captura de los componentes de pestaña descendiente el conjunto de pestañas que lo contiene como un parámetro en cascada, por lo que los componentes de la ficha que se agreguen a la coordenada y de conjunto de pestañas en la ficha que está activa.

*Components/Tab.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Components/Tab.cshtml)]

## <a name="razor-templates"></a>Plantillas de Razor

Representar fragmentos se pueden definir mediante la sintaxis de la plantilla de Razor. Las plantillas de Razor son una manera de definir un fragmento de código de interfaz de usuario y asumir el formato siguiente:

```cshtml
@<tag>...</tag>
```

El ejemplo siguiente muestra cómo especificar `RenderFragment` y `RenderFragment<T>` valores.

*RazorTemplates.cshtml*:

```cshtml
@{
    RenderFragment template = @<p>The time is @DateTime.Now.</p>;
    RenderFragment<Pet> petTemplate = (pet) => @<p>Your pet's name is @pet.Name.</p>;
}
```

Representar fragmentos definidos con Razor plantillas se pueden pasar como argumentos a los componentes con plantilla o presentar directamente. Por ejemplo, las plantillas anteriores se representan directamente con el siguiente marcado de Razor:

```cshtml
@template

@petTemplate(new Pet { Name = "Rex" })
```

Salida representada:

```
The time is 10/04/2018 01:26:52.

Your pet's name is Rex.
```

## <a name="manual-rendertreebuilder-logic"></a>Lógica de RenderTreeBuilder manual

`Microsoft.AspNetCore.Components.RenderTree` Proporciona métodos para manipular los componentes y elementos, incluida la creación de componentes manualmente en C# código.

> [!NOTE]
> El uso de `RenderTreeBuilder` crear componentes es un escenario avanzado. Un componente tiene un formato incorrecto (por ejemplo, una etiqueta de marcado sin cerrar) puede provocar un comportamiento indefinido.

Tenga en cuenta los siguientes componentes de mascota detalles (*PetDetails.razor* en componentes de Razor; *PetDetails.cshtml* en Blazor), que pueden crearse manualmente en otro componente:

```cshtml
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote<p>

@functions
{
    [Parameter]
    string PetDetailsQuote { get; set; }
}
```

En el ejemplo siguiente, el bucle en el `CreateComponent` método genera tres componentes de los detalles de mascota. Al llamar a `RenderTreeBuilder` métodos para crear los componentes (`OpenComponent` y `AddAttribute`), números de secuencia son números de línea de código fuente. Los componentes de Razor algoritmo de diferencias se basa en los números de secuencia correspondiente a diferentes líneas de código, no se diferencia llamar a las invocaciones. Al crear un componente con `RenderTreeBuilder` codificar los métodos, los argumentos para los números de secuencia. **Uso de un cálculo o el contador para generar el número de secuencia puede provocar un rendimiento deficiente.**

Compila el componente (*BuiltContent.razor* en componentes de Razor; *BuiltContent.cshtml* en Blazor):

```cshtml
@page "/BuiltContent"

<h1>Build a component</h1>

@CustomRender

<button type="button" onclick="@RenderComponent">
    Create three Pet Details components
</button>

@functions {
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
