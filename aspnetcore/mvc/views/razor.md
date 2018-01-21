---
title: Referencia de la sintaxis de Razor para ASP.NET Core
author: rick-anderson
description: "Obtenga información acerca de la sintaxis de marcado de Razor para incrustar código basado en servidor en las páginas Web."
ms.author: riande
manager: wpickett
ms.date: 10/18/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/razor
ms.openlocfilehash: d932e28246998c60e2b3f9c77a2521fe55991e85
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="razor-syntax-for-aspnet-core"></a>Sintaxis de Razor para ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen), y [Dan Vicarel](https://github.com/Rabadash8820)

Razor es una sintaxis de marcado para incrustar código basado en servidor en las páginas Web. La sintaxis de Razor consta de Razor marcado, C# y HTML. Archivos que contienen Razor generalmente tienen un *.cshtml* la extensión de archivo.

## <a name="rendering-html"></a>Representación HTML

El idioma de Razor predeterminado es HTML. Representación HTML desde el marcado de Razor es similar a representar HTML desde un archivo HTML. El marcado HTML en *.cshtml* archivos Razor se representa en el servidor sin cambios.

## <a name="razor-syntax"></a>Sintaxis de Razor

Razor es compatible con C# y usa el `@` símbolo para realizar la transición de HTML para C#. Razor evalúa las expresiones de C# y los representa en la salida HTML.

Cuando un `@` va seguido de símbolo de un [palabra clave reservada de Razor](#razor-reserved-keywords), realiza una transición marcado específico de Razor. En caso contrario, realiza una transición sin formato C#.

Escape un `@` de símbolos en el marcado de Razor, use un segundo `@` símbolo:

```cshtml
<p>@@Username</p>
```

El código se representa en HTML con una sola `@` símbolo:

```html
<p>@Username</p>
```

Atributos HTML y el contenido que contiene direcciones de correo electrónico no tratan la `@` símbolos como un carácter de transición. Razor análisis no se modifica las direcciones de correo electrónico en el ejemplo siguiente:

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a>Expresiones implícitas de Razor

Las expresiones de Razor implícita empiezan con `@` seguido por el código de C#:

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

Con la excepción de C# `await` palabra clave, las expresiones implícitas no deben contener espacios. Si la instrucción de C# tiene un final claro, pueden ser mezclados espacios:

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

Las expresiones implícitas **no** contienen tipos genéricos de C#, como los caracteres dentro de los corchetes (`<>`) se interpretan como una etiqueta HTML. El código siguiente es **no** válido:

```cshtml
<p>@GenericMethod<int>()</p>
```

El código anterior genera un error del compilador similar a uno de los siguientes:

 * No se cerró el elemento "int". Todos los elementos deben ser de autocierre o tiene la correspondiente etiqueta de cierre.
 * No se puede convertir el grupo de métodos 'GenericMethod' a 'object' de tipo no delegado. Pretendía invocar el método?' 
 
Llamadas de método genérico deben incluirse en un [expresión explícita de Razor](#explicit-razor-expressions) o un [bloque de código Razor](#razor-code-blocks).

## <a name="explicit-razor-expressions"></a>Expresiones explícitas de Razor

Expresiones de Razor explícitas constan de un `@` símbolo con paréntesis equilibrados. Para representar la hora de la semana pasada, se utiliza el siguiente marcado de Razor:

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

Cualquier contenido dentro de la `@()` paréntesis se evalúa y se representa en la salida.

Por lo general, las expresiones implícitas, que se describe en la sección anterior, no pueden contener espacios. En el código siguiente, una semana no resta de la hora actual:

[!code-cshtml[Main](razor/sample/Views/Home/Contact.cshtml?range=17)]

El código representa el código HTML siguiente:

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

Pueden utilizarse expresiones explícitas para concatenar texto con un resultado de la expresión:

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

Sin la expresión explícita, `<p>Age@joe.Age</p>` se trata como una dirección de correo electrónico, y `<p>Age@joe.Age</p>` se representa. Cuando se escriben como una expresión explícita, `<p>Age33</p>` se representa.


Pueden utilizarse expresiones explícitas para representar el resultado de los métodos genéricos en *.cshtml* archivos. En una expresión implícita, los caracteres dentro de los corchetes (`<>`) se interpretan como una etiqueta HTML. Es el marcado siguiente **no** Razor válido:

```cshtml
<p>@GenericMethod<int>()</p>
```

El código anterior genera un error del compilador similar a uno de los siguientes:

 * No se cerró el elemento "int". Todos los elementos deben ser de autocierre o tiene la correspondiente etiqueta de cierre.
 * No se puede convertir el grupo de métodos 'GenericMethod' a 'object' de tipo no delegado. Pretendía invocar el método?' 
 
 El marcado siguiente muestra este código de la escritura de forma correcta. El código se escribe como una expresión explícita:

```cshtml
<p>@(GenericMethod<int>())</p>
```

## <a name="expression-encoding"></a>Codificación de expresión

Expresiones de C# que se evalúan como una cadena están codificado en HTML. Expresiones de C# que se evalúan como `IHtmlContent` se representan directamente desde `IHtmlContent.WriteTo`. Expresiones de C# que no se evalúan como `IHtmlContent` se convierte en una cadena por `ToString` y codificar antes de que se va a procesar.

```cshtml
@("<span>Hello World</span>")
```

El código representa el código HTML siguiente:

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

El código HTML se muestra en el explorador como:

```
<span>Hello World</span>
```

`HtmlHelper.Raw`salida no está codificada pero representar como marca HTML.

> [!WARNING]
> Usar `HtmlHelper.Raw` unsanitized usuario de la entrada es un riesgo de seguridad. Proporcionados por el usuario podrían contener código JavaScript malintencionado o seguridad de otras maneras. Es difícil inmunizar proporcionados por el usuario. Evite el uso de `HtmlHelper.Raw` con proporcionados por el usuario.

```cshtml
@Html.Raw("<span>Hello World</span>")
```

El código representa el código HTML siguiente:

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a>Bloques de código Razor

Bloques de código Razor iniciar con `@` y encerradas entre `{}`. A diferencia de las expresiones, no representa el código de C# dentro de bloques de código. Bloques de código y las expresiones de una vista comparten el mismo ámbito y están definidas en orden:

```cshtml
@{
    var quote = "The future depends on what you do today. - Mahatma Gandhi";
}

<p>@quote</p>

@{
    quote = "Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.";
}

<p>@quote</p>
```

El código representa el código HTML siguiente:

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

### <a name="implicit-transitions"></a>Transiciones implícita

Es el idioma predeterminado en un bloque de código C#, pero la página de Razor puede realizar la transición a HTML:

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a>Transición delimitado explícita

Para definir una subsección de un bloque de código que debería presentar HTML, rodear los caracteres para la representación con el código Razor  **\<texto >** etiqueta:

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

Utilice este enfoque para representar HTML que no está rodeado por una etiqueta HTML. Sin una etiqueta HTML o Razor, se produce un error de tiempo de ejecución de Razor.

El  **\<texto >** etiqueta es útil para controlar el espacio en blanco al representar el contenido:

* Solo el contenido entre el  **\<texto >** se representa la etiqueta. 
* No hay espacio en blanco antes o después de la  **\<texto >** la etiqueta aparece en la salida HTML.

### <a name="explicit-line-transition-with-"></a>Transición de línea explícita con @:

Para representar el resto de una línea completa como HTML dentro de un bloque de código, use la `@:` sintaxis:

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

Sin el `@:` en el código, se genera un error de tiempo de ejecución de Razor.

Advertencia: Adicional `@` caracteres en un archivo Razor pueden producir errores del compilador causa en instrucciones más adelante en el bloque. Estos errores del compilador pueden ser difíciles de entender porque el error real se produce antes del error notificado. Este error es habitual después de combinar varias expresiones implícito o explícito en un único bloque de código.

## <a name="control-structures"></a>Estructuras de control

Estructuras de control son una extensión de bloques de código. Todos los aspectos de bloques de código (transición a marcado, C# en línea) también se aplican a las estructuras siguientes:

### <a name="conditionals-if-else-if-else-and-switch"></a>Instrucciones condicionales @if, else if, else, y@switch

`@if`controles cuando se ejecuta el código:

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

`else`y `else if` no requieren la `@` símbolo:

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
else if (value >= 1337)
{
    <p>The value is large.</p>
}
else
{
    <p>The value is odd and small.</p>
}
```

El marcado siguiente muestra cómo utilizar una instrucción switch:

```cshtml
@switch (value)
{
    case 1:
        <p>The value is 1!</p>
        break;
    case 1337:
        <p>Your number is 1337!</p>
        break;
    default:
        <p>Your number wasn't 1 or 1337.</p>
        break;
}
```

### <a name="looping-for-foreach-while-and-do-while"></a>Bucle @for, @foreach, @while, y @do mientras

HTML con plantilla se puede representar con las instrucciones de control de bucle. Para presentar una lista de personas:

```cshtml
@{
    var people = new Person[]
    {
          new Person("Weston", 33),
          new Person("Johnathon", 41),
          ...
    };
}
```

Se admiten las siguientes instrucciones bucles:

`@for`

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@foreach`

```cshtml
@foreach (var person in people)
{
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>
}
```

`@while`

```cshtml
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

```cshtml
@{ var i = 0; }
@do
{
    var person = people[i];
    <p>Name: @person.Name</p>
    <p>Age: @person.Age</p>

    i++;
} while (i < people.Length);
```

### <a name="compound-using"></a>Compuesta@using

En C#, un `using` instrucción se utiliza para asegurarse de que se elimina un objeto. En Razor, el mismo mecanismo se utiliza para crear aplicaciones auxiliares de HTML que contienen contenido adicional. En el código siguiente, las aplicaciones auxiliares HTML presentar una etiqueta de formulario con el `@using` instrucción:


```cshtml
@using (Html.BeginForm())
{
    <div>
        email:
        <input type="email" id="Email" value="">
        <button>Register</button>
    </div>
}
```

Se pueden realizar acciones de nivel de ámbito con [aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro).

### <a name="try-catch-finally"></a>@try, catch y finally

Control de excepciones es similar a C#:

[!code-cshtml[Main](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

Razor tiene la capacidad de proteger las secciones críticas con instrucciones de bloqueo:

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a>Comentarios

Razor admite comentarios de C# y el código HTML:

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

El código representa el código HTML siguiente:

```html
<!-- HTML comment -->
```

Comentarios de Razor se quitan mediante el servidor antes de presenta la página Web. Razor usa `@*  *@` para delimitar los comentarios. El código siguiente se hace referencia a, por lo que el servidor no representa ningún otro marcado:

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a>Directivas

Directivas de Razor se representan mediante expresiones implícitas con las siguientes palabras clave reservadas el `@` símbolos. Normalmente, una directiva cambia la forma en que una vista se analiza o habilita una funcionalidad diferente.

Descripción de cómo Razor genera código para una vista resulta más fácil comprender cómo funcionan las directivas.

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

El código genera una clase similar al siguiente:

```csharp
public class _Views_Something_cshtml : RazorPage<dynamic>
{
    public override async Task ExecuteAsync()
    {
        var output = "Getting old ain't for wimps! - Anonymous";

        WriteLiteral("/r/n<div>Quote of the Day: ");
        Write(output);
        WriteLiteral("</div>");
    }
}
```

Más adelante en este artículo, la sección [ver la clase de C# de Razor generada por una vista](#viewing-the-razor-c-class-generated-for-a-view) explica cómo ver esta clase generada.

### <a name="using"></a>@using

El `@using` directiva agrega C# `using` la directiva a la vista generada:

[!code-cshtml[Main](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

El `@model` directiva especifica el tipo del modelo que se pasan a una vista:

```cshtml
@model TypeNameOfModel
```

En una aplicación de MVC de ASP.NET Core creada con cuentas de usuario individuales, el *Views/Account/Login.cshtml* vista contiene la siguiente declaración de modelo:

```cshtml
@model LoginViewModel
```

La clase generada se hereda de `RazorPage<dynamic>`:

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

Razor expone un `Model` propiedad para tener acceso al modelo que se pasa a la vista:

```cshtml
<div>The Login Email: @Model.Email</div>
```

El `@model` directiva especifica el tipo de esta propiedad. La directiva especifica la `T` en `RazorPage<T>` que generado clase que deriva de la vista de. Si el `@model` iisn't directiva se especifica, el `Model` propiedad es de tipo `dynamic`. El valor del modelo se pasa desde el controlador a la vista. Para obtener más información, consulte [fuertemente tipadas modelos y la @model (palabra clave).

### <a name="inherits"></a>@inherits

El `@inherits` directiva proporciona el control completo de la clase que hereda de la vista:

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

El código siguiente es un tipo de página de Razor personalizado:

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

El `CustomText` se muestran en una vista:

[!code-cshtml[Main](razor/sample/Views/Home/Contact10.cshtml)]

El código representa el código HTML siguiente:

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

 `@model`y `@inherits` puede utilizarse en la misma vista. `@inherits`puede estar en un *_ViewImports.cshtml* archivo que se importa de la vista:

[!code-cshtml[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

El código siguiente es un ejemplo de una vista fuertemente tipada:

[!code-cshtml[Main](razor/sample/Views/Home/Login1.cshtml)]

Si "rick@contoso.com" se pasa en el modelo, la vista genera el siguiente marcado HTML:

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject


El `@inject` directiva permite a la página de Razor insertar un servicio desde el [contenedor de servicios](xref:fundamentals/dependency-injection) en una vista. Para obtener más información, consulte [inyección de dependencia en las vistas](xref:mvc/views/dependency-injection).

### <a name="functions"></a>@functions

El `@functions` directiva permite que una página de Razor agregar contenido de nivel de función a una vista:

```cshtml
@functions { // C# Code }
```

Por ejemplo:

[!code-cshtml[Main](razor/sample/Views/Home/Contact6.cshtml)]

El código genera el siguiente marcado HTML:

```html
<div>From method: Hello</div>
```

El código siguiente es la clase generada de Razor C#:

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### <a name="section"></a>@section

El `@section` directiva se usa junto con el [diseño](xref:mvc/views/layout) para habilitar las vistas representar el contenido en diferentes partes de la página HTML. Para obtener más información, consulte [secciones](xref:mvc/views/layout#layout-sections-label).

## <a name="tag-helpers"></a>Aplicaciones auxiliares de etiquetas

Hay tres directivas que pertenecen a [aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro).

| Directiva | Función |
| --------- | -------- |
| [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) | Pone a disposición a una vista de aplicaciones auxiliares de etiquetas. |
| [@removeTagHelper](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | Quita las aplicaciones auxiliares de etiquetas que agregó anteriormente desde una vista. |
| [@tagHelperPrefix](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | Especifica un prefijo de etiqueta para habilitar la compatibilidad de la aplicación auxiliar de etiqueta y hacer uso de la aplicación auxiliar de etiqueta explícita. |

## <a name="razor-reserved-keywords"></a>Palabras clave reservada de Razor

### <a name="razor-keywords"></a>Palabras clave de Razor

* página (requiere el núcleo ASP.NET 2.0 y versiones posterior)
* funciones
* hereda
* modelo
* section
* aplicación auxiliar (actualmente no admitida ASP.NET Core)

Palabras clave de Razor se escapan con `@(Razor Keyword)` (por ejemplo, `@(functions)`).

### <a name="c-razor-keywords"></a>Palabras clave de C# Razor

* mayúsculas y minúsculas
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

Palabras clave de C# Razor debe ser un carácter de escape doble, con `@(@C# Razor Keyword)` (por ejemplo, `@(@case)`). La primera `@` antepone el analizador Razor. El segundo `@` antepone el analizador de C#.

### <a name="reserved-keywords-not-used-by-razor"></a>Palabras clave reservadas no utilizadas Razor

* namespace
* clase

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a>Visualización de la clase de C# de Razor generada por una vista

Agregue la siguiente clase al proyecto de MVC de ASP.NET Core:

[!code-csharp[Main](razor/sample/Utilities/CustomTemplateEngine.cs)]

Invalidar el `RazorTemplateEngine` agregado MVC con la `CustomTemplateEngine` clase:

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=10-14)]

Establecer un punto de interrupción en la `return csharpDocument` instrucción de `CustomTemplateEngine`. Cuando la ejecución del programa se detiene en el punto de interrupción, ver el valor de `generatedCode`.

![Vista de generatedCode visualizador de texto](razor/_static/tvr.png)

## <a name="view-lookups-and-case-sensitivity"></a>Búsquedas de vista y entre mayúsculas y minúsculas

El motor de vista Razor realiza búsquedas entre mayúsculas y minúsculas para las vistas. Sin embargo, la búsqueda real se determina por el sistema de archivos subyacente:

* En función del origen del archivo: 
  * En sistemas operativos con sistemas de archivos entre mayúsculas y minúsculas (por ejemplo, Windows), búsquedas de proveedor de archivo físico distinguen entre mayúsculas y minúsculas. Por ejemplo, `return View("Test")` da como resultado que coincidan con la */Views/Home/Test.cshtml*, */Views/home/test.cshtml*y cualquier otra variante de mayúsculas y minúsculas.
  * En sistemas de archivos entre mayúsculas y minúsculas (por ejemplo, Linux y OSX y con `EmbeddedFileProvider`), las búsquedas distinguen mayúsculas de minúsculas. Por ejemplo, `return View("Test")` específicamente coincidencias */Views/Home/Test.cshtml*.
* Precompilado vistas: con núcleo ASP.NET 2.0 y versiones posterior, buscar vistas precompiladas distingue mayúsculas de minúsculas en todos los sistemas operativos. El comportamiento es idéntico al comportamiento del proveedor del archivo físico en Windows. Si dos vistas precompiladas difieren solo en caso de que el resultado de búsqueda es no determinista.

Los desarrolladores pueden hacer coincidir las mayúsculas y minúsculas de los nombres de archivo y directorio para las mayúsculas y minúsculas de:

    * Nombres de área, acción y controlador. 
    * Páginas de Razor.
    
Coincidencia de mayúsculas garantiza que las implementaciones encontrar sus vistas sin tener en cuenta el sistema de archivos subyacente.
