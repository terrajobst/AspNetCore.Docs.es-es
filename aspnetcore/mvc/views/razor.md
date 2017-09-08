---
title: Referencia de la sintaxis de Razor para ASP.NET Core
author: rick-anderson
description: Sintaxis de Razor de detalles
keywords: "Núcleo de ASP.NET, Razor"
ms.author: riande
manager: wpickett
ms.date: 07/4/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/razor
ms.openlocfilehash: 7648bc2ac7b9efd1653725cda749d6cd271bae77
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="razor-syntax-for-aspnet-core"></a>Sintaxis de Razor para ASP.NET Core

Por [Taylor Mullen](https://twitter.com/ntaylormullen) y [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="what-is-razor"></a>¿Qué es Razor?

Razor es una sintaxis de marcado para incrustar código de servidor basándose en las páginas web. La sintaxis de Razor consta del marcado de Razor, C# y el código HTML. Archivos que contienen Razor generalmente tienen un *.cshtml* la extensión de archivo.

## <a name="rendering-html"></a>Representación HTML

El idioma de Razor predeterminado es HTML. Es similar a representar HTML de Razor en un archivo HTML. Un archivo Razor con el siguiente marcado:

```html
<p>Hello World</p>
   ```

Se representa sin modificar como `<p>Hello World</p>` por el servidor.

## <a name="razor-syntax"></a>Sintaxis de Razor

Razor es compatible con C# y usa el `@` símbolo para realizar la transición de HTML para C#. Razor evalúa las expresiones de C# y los representa en la salida HTML. Razor puede realizar la transición de HTML en C# o en el marcado específico de Razor. Cuando un `@` va seguido de símbolo de un [palabra clave reservada de Razor](#razor-reserved-keywords) realiza la transición en el marcado específico de Razor, en caso contrario, realiza una transición sin formato C#.

<a name=escape-at-label></a>

HTML que contiene `@` símbolos que necesite ser caracteres de escape con una segunda `@` símbolos. Por ejemplo:

```html
<p>@@Username</p>
   ```

podría invalidar el código HTML siguiente:

```html
<p>@Username</p>
   ```

<a name=razor-email-ref></a>

Atributos HTML y el contenido que contiene direcciones de correo electrónico no tratan la `@` símbolos como un carácter de transición.

   `<a href="mailto:Support@contoso.com">Support@contoso.com</a>`

## <a name="implicit-razor-expressions"></a>Expresiones implícitas de Razor

Las expresiones de Razor implícita empiezan con `@` seguido por el código de C#. Por ejemplo:

```html
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

Con la excepción de C# `await` expresiones implícitas de palabra clave no deben contener espacios. Por ejemplo, puede intermingle espacios siempre y cuando la instrucción de C# tiene un claro final:

```html
<p>@await DoSomething("hello", "world")</p>
```

## <a name="explicit-razor-expressions"></a>Expresiones explícitas de Razor

Las expresiones de Razor explícitas consta de un símbolo con paréntesis equilibrados @. Por ejemplo, para representar la hora de la última semana:

```html
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

Cualquier contenido dentro de la @ paréntesis () se evalúa y se representa en la salida.

Por lo general, las expresiones implícitas no pueden contener espacios. Por ejemplo, en el código siguiente, no se restan una semana desde la hora actual:

[!code-html[Main](razor/sample/Views/Home/Contact.cshtml?range=20)]

Que representa el código HTML siguiente:

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
   ```

Puede utilizar una expresión explícita para concatenar texto con un resultado de la expresión:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "none", "highlight_args": {"hl_lines": [5]}} -->

```none
@{
    var joe = new Person("Joe", 33);
 }

<p>Age@(joe.Age)</p>
```

Sin la expresión explícita, `<p>Age@joe.Age</p>` se tratará como una dirección de correo electrónico y `<p>Age@joe.Age</p>` se representarían en. Cuando se escriben como una expresión explícita, `<p>Age33</p>` se representa.

<a name=expression-encoding-label></a>

## <a name="expression-encoding"></a>Codificación de expresión

Expresiones de C# que se evalúan como una cadena están codificado en HTML. Expresiones de C# que se evalúan como `IHtmlContent` se representan directamente desde *IHtmlContent.WriteTo*. Expresiones de C# que no se evalúan como *IHtmlContent* se convierte en una cadena (por *ToString*) y codificado antes de que se representan. Por ejemplo, el siguiente marcado de Razor:

```html
@("<span>Hello World</span>")
   ```

Esto representa HTML:

```html
&lt;span&gt;Hello World&lt;/span&gt;
   ```

Que el explorador se representa como:

`<span>Hello World</span>`

`HtmlHelper``Raw` salida no está codificada pero representar como marca HTML.

>[!WARNING]
> Usar `HtmlHelper.Raw` unsanitized usuario de la entrada es un riesgo de seguridad. Proporcionados por el usuario podrían contener código JavaScript malintencionado o seguridad de otras maneras. Inmunizar proporcionados por el usuario es difícil, evite el uso de `HtmlHelper.Raw` en proporcionados por el usuario.

El siguiente marcado de Razor:

```html
@Html.Raw("<span>Hello World</span>")
   ```

Esto representa HTML:

```html
<span>Hello World</span>
   ```

<a name=razor-code-blocks-label></a>

## <a name="razor-code-blocks"></a>Bloques de código Razor

Bloques de código Razor iniciar con `@` y encerradas entre `{}`. A diferencia de las expresiones, no se representa el código de C# dentro de bloques de código. Bloques de código y expresiones en una página de Razor comparten el mismo ámbito y están definidas en orden (es decir, las declaraciones en un bloque de código será en el ámbito de expresiones y bloques de código más adelante).

```none
@{
    var output = "Hello World";
}

<p>The rendered result: @output</p>
```

Podría invalidar:

```html
<p>The rendered result: Hello World</p>
   ```

<a name=implicit-transitions-label></a>

### <a name="implicit-transitions"></a>Transiciones implícita

Es el idioma predeterminado en un bloque de código C#, pero puede realizar la transición a HTML. HTML dentro de un bloque de código realizará la transición a presentar HTML:

```none
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

<a name=explicit-delimited-transition-label></a>

### <a name="explicit-delimited-transition"></a>Transición delimitado explícita

Para definir una subsección de un bloque de código que debería presentar HTML, rodear los caracteres que se va a representar con el código Razor `<text>` etiqueta:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "none", "highlight_args": {"hl_lines": [4]}} -->

```none
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

Generalmente, este enfoque se utiliza cuando desee presentar HTML que no está rodeado por una etiqueta HTML. Sin una etiqueta HTML o Razor, obtendrá un error de tiempo de ejecución de Razor.

<a name=explicit-line-transition-with-label></a>

### <a name="explicit-line-transition-with-"></a>Transición de línea explícita con`@:`

Para representar el resto de una línea completa como HTML dentro de un bloque de código, use la `@:` sintaxis:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "none", "highlight_args": {"hl_lines": [4]}} -->

```none
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

Sin el `@:` en el código anterior, obtendrá un error en tiempo de ejecución de Razor.

<a name=control-structures-razor-label></a>

## <a name="control-structures"></a>Estructuras de control

Estructuras de control son una extensión de bloques de código. Todos los aspectos de bloques de código (transición a marcado, C# en línea) también se aplican a las siguientes estructuras.

### <a name="conditionals-if-else-if-else-and-switch"></a>Instrucciones condicionales `@if`, `else if`, `else` y`@switch`

El `@if` familia controla cuándo se ejecuta el código:

```none
@if (value % 2 == 0)
{
    <p>The value was even</p>
}
```

`else`y `else if` no requieren la `@` símbolo:

```none
@if (value % 2 == 0)
{
    <p>The value was even</p>
}
else if (value >= 1337)
{
    <p>The value is large.</p>
}
else
{
    <p>The value was not large and is odd.</p>
}
```

Puede usar una instrucción switch similar al siguiente:

```none
@switch (value)
{
    case 1:
        <p>The value is 1!</p>
        break;
    case 1337:
        <p>Your number is 1337!</p>
        break;
    default:
        <p>Your number was not 1 or 1337.</p>
        break;
}
```

### <a name="looping-for-foreach-while-and-do-while"></a>Bucle `@for`, `@foreach`, `@while`, y`@do while`

Puede representar HTML mediante plantillas con las instrucciones de control de bucle. Por ejemplo, para presentar una lista de personas:

```none
@{
    var people = new Person[]
    {
          new Person("John", 33),
          new Person("Doe", 41),
    };
}
```

Puede usar cualquiera de las siguientes instrucciones bucles:

`@for`

```none
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@foreach`

```none
@foreach (var person in people)
{
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@while`

```none
@{ var i = 0; }
@while (i < people.Length)
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>

    i++;
}
```

`@do while`

```none
@{ var i = 0; }
@do
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>

    i++;
} while (i < people.Length);
```

### <a name="compound-using"></a>Compuesta`@using`

En C# un uso de la instrucción se utiliza para asegurarse de que se elimina un objeto. En Razor este mismo mecanismo se puede utilizar para crear aplicaciones auxiliares HTML que contienen contenido adicional. Por ejemplo, se puede utilizar aplicaciones auxiliares de HTML para presentar una etiqueta de formulario con el `@using` instrucción:

```none
@using (Html.BeginForm())
{
    <div>
        email:
        <input type="email" id="Email" name="Email" value="" />
        <button type="submit"> Register </button>
    </div>
}
```

También puede realizar acciones de nivel de ámbito como los pasos anteriores con [aplicaciones auxiliares de etiquetas](tag-helpers/index.md).

### <a name="try-catch-finally"></a>`@try`, `catch`, `finally`

Control de excepciones es similar a C#:

[!code-html[Main](razor/sample/Views/Home/Contact7.cshtml)]

### `@lock`

Razor tiene la capacidad de proteger las secciones críticas con instrucciones de bloqueo:

```none
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a>Comentarios

Razor admite comentarios de C# y el código HTML. El siguiente marcado:

```none
@{
    /* C# comment. */
    // Another C# comment.
}
<!-- HTML comment -->
```

Se representa como por el servidor:

```none
<!-- HTML comment -->
```

Comentarios de Razor se quitan mediante el servidor antes de presentar la página. Razor usa `@*  *@` para delimitar los comentarios. El código siguiente se hace referencia a, por lo que el servidor no se puede representar cualquier marcado:

```none
 @*
 @{
     /* C# comment. */
     // Another C# comment.
 }
 <!-- HTML comment -->
*@
```

<a name=razor-directives-label></a>

## <a name="directives"></a>Directivas

Directivas de Razor se representan mediante expresiones implícitas con las siguientes palabras clave reservadas el `@` símbolos. Una directiva normalmente se cambiar la manera en que se analiza una página o habilitar una funcionalidad diferente dentro de la página de Razor.

Descripción de cómo Razor genera código para una vista le resultará más fácil de entender cómo funcionan las directivas. Una página de Razor se usa para generar un archivo de C#. Por ejemplo, esta página Razor:

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

Genera una clase similar al siguiente:

```csharp
public class _Views_Something_cshtml : RazorPage<dynamic>
{
    public override async Task ExecuteAsync()
    {
        var output = "Hello World";

        WriteLiteral("/r/n<div>Output: ");
        Write(output);
        WriteLiteral("</div>");
    }
}
```

[Visualización de la clase de C# de Razor generada por una vista](#razor-customcompilationservice-label) explica cómo ver esta clase generada.

### `@using`

El `@using` directiva agregará c# `using` la directiva a la página de razor generado:

[!code-html[Main](razor/sample/Views/Home/Contact9.cshtml)]

### `@model`

El `@model` directiva especifica el tipo de modelo que se pasa a la página de Razor. Utiliza la siguiente sintaxis:

```none
@model TypeNameOfModel
   ```

Por ejemplo, si crea una aplicación de MVC de ASP.NET Core con cuentas de usuario individuales, el *Views/Account/Login.cshtml* vista Razor contiene la siguiente declaración de modelo:

```csharp
@model LoginViewModel
   ```

En el ejemplo anterior de la clase, la clase generada se hereda de `RazorPage<dynamic>`. Agregando un `@model` controlar lo que se hereda. Por ejemplo

```csharp
@model LoginViewModel
   ```

Genera la clase siguiente

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
   ```

Las páginas de Razor exponen un `Model` propiedad para tener acceso al modelo que se pasa a la página.

```html
<div>The Login Email: @Model.Email</div>
   ```

El `@model` el tipo de esta propiedad especifica la directiva (especificando la `T` en `RazorPage<T>` que se deriva de la clase generada para la página). Si no se especifica la `@model` directiva la `Model` propiedad será de tipo `dynamic`. El valor del modelo se pasa desde el controlador a la vista. Vea [fuertemente tipadas modelos y la @model palabra clave](../../tutorials/first-mvc-app/adding-model.md#strongly-typed-models-keyword-label) para obtener más información.

### `@inherits`

El `@inherits` directiva proporciona control total de la clase que hereda de la página de Razor:

```none
@inherits TypeNameOfClassToInheritFrom
   ```

Por ejemplo, supongamos que ha surgido del siguiente tipo de página Razor personalizado:

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

El siguiente código de Razor generaría `<div>Custom text: Hello World</div>`.

[!code-html[Main](razor/sample/Views/Home/Contact10.cshtml)]

No se puede utilizar `@model` y `@inherits` en la misma página. Puede tener `@inherits` en un *_ViewImports.cshtml* archivo que se importa en la página de Razor. Por ejemplo, si la vista Razor importado los siguientes *_ViewImports.cshtml* archivo:

[!code-html[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

La siguiente página de Razor fuertemente tipada

[!code-html[Main](razor/sample/Views/Home/Login1.cshtml)]

Se genera este marcado HTML:

```none
<div>The Login Email: Rick@contoso.com</div>
<div>Custom text: Hello World</div>
```

Cuando se pasan "[Rick@contoso.com](mailto:Rick@contoso.com)" en el modelo:

   Vea [diseño](layout.md) para obtener más información.

### `@inject`

El `@inject` directiva permite insertar un servicio desde el [contenedor de servicios](../../fundamentals/dependency-injection.md) en la página de Razor para su uso. Vea [inyección de dependencia en las vistas](dependency-injection.md).

<a name="functions"></a>

### `@functions`

El `@functions` directiva le permite agregar contenido de nivel de función a la página de Razor. La sintaxis es la siguiente:

```none
@functions { // C# Code }
   ```

Por ejemplo:

[!code-html[Main](razor/sample/Views/Home/Contact6.cshtml)]

Genera el siguiente marcado HTML:

```none
<div>From method: Hello</div>
   ```

Generado Razor C# es similar:

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### `@section`

El `@section` directiva se usa junto con el [página de diseño](layout.md) para habilitar las vistas representar el contenido en diferentes partes de la página HTML representada. Vea [secciones](layout.md#layout-sections-label) para obtener más información.

## <a name="tag-helpers"></a>Aplicaciones auxiliares de etiquetas

El siguiente [aplicaciones auxiliares de etiquetas](tag-helpers/index.md) directivas se detallan en los vínculos proporcionados.

* [@addTagHelper](tag-helpers/intro.md#add-helper-label)
* [@removeTagHelper](tag-helpers/intro.md#remove-razor-directives-label)
* [@tagHelperPrefix](tag-helpers/intro.md#prefix-razor-directives-label)

<a name=razor-reserved-keywords-label></a>

## <a name="razor-reserved-keywords"></a>Palabras clave reservada de Razor

### <a name="razor-keywords"></a>Palabras clave de Razor

* página (requiere el núcleo ASP.NET 2.0 y versiones posterior)
* funciones
* hereda
* modelo
* section
* aplicación auxiliar (no compatible con ASP.NET Core).

Palabras clave de Razor se pueden escapar con `@(Razor Keyword)`, por ejemplo `@(functions)`. Vea el ejemplo completo siguiente.

### <a name="c-razor-keywords"></a>Palabras clave de C# Razor

* case
* do
* default
* for
* foreach
* if
* else
* bloquear
* switch
* try
* catch
* finally
* utilizar
* while

Palabras clave de C# Razor deba ser dobles de escape `@(@C# Razor Keyword)`, por ejemplo `@(@case)`. La primera `@` antepone el analizador Razor, el segundo `@` antepone el analizador de C#. Vea el ejemplo completo siguiente.

### <a name="reserved-keywords-not-used-by-razor"></a>Palabras clave reservadas no utilizadas Razor

* namespace
* clase

<a name=razor-customcompilationservice-label></a>

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a>Visualización de la clase de C# de Razor generada por una vista

Agregue la siguiente clase al proyecto de MVC de ASP.NET Core:

[!code-csharp[Main](razor/sample/Services/CustomCompilationService.cs)]

Invalidar el `ICompilationService` agrega MVC con la clase anterior;

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=29-33)]

Establecer un punto de interrupción en la `Compile` método `CustomCompilationService` y vista `compilationContent`.

![Vista de compilationContent visualizador de texto](razor/_static/tvr.png)

<a name="case"></a>
## <a name="view-lookups-and-case-sensitivity"></a>Búsquedas de vista y entre mayúsculas y minúsculas

El motor de vista Razor realiza búsquedas entre mayúsculas y minúsculas para las vistas. Sin embargo, la búsqueda real se determina por el origen subyacente:

* En función del origen del archivo: 

    * En sistemas operativos con sistemas de archivos entre mayúsculas y minúsculas (por ejemplo, Windows), búsquedas de proveedor de archivo físico distinguen entre mayúsculas y minúsculas. Por ejemplo `return View("Test")` , se crearán `/Views/Home/Test.cshtml`, `/Views/home/test.cshtml` y otras variantes de mayúsculas y minúsculas podrían ser detectados.
    * En sistemas de archivos entre mayúsculas y minúsculas, lo que incluye Linux, OSX y `EmbeddedFileProvider`, las búsquedas distinguen mayúsculas de minúsculas. Por ejemplo, `return View("Test")` sería específicamente `/Views/Home/Test.cshtml`.
        
* Vistas precompiladas:

   * Con ASP.Net Core 2.0 y versiones posteriores, buscar vistas precompiladas distingue mayúsculas de minúsculas en todos los sistemas operativos. El comportamiento es idéntico al comportamiento del proveedor del archivo físico en Windows. 
   Nota: Si dos vistas precompiladas difieren solo en el caso, el resultado de búsqueda es no determinista.

Los desarrolladores pueden hacer coincidir las mayúsculas y minúsculas de los nombres de archivo y directorio para las mayúsculas y minúsculas de los nombres de área, acción y controlador. Esto garantizará que las implementaciones de permanecen independientes del sistema de archivos subyacente.
