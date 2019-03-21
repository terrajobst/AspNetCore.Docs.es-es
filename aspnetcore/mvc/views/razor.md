---
title: Referencia de sintaxis de Razor para ASP.NET Core
author: rick-anderson
description: Obtenga información sobre la sintaxis de marcado de Razor para insertar código basado en servidor en páginas web.
ms.author: riande
ms.date: 10/26/2018
uid: mvc/views/razor
ms.openlocfilehash: 254c85ee9e74dc72170b19d27fbc5f1ae7ccd3dc
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58264754"
---
# <a name="razor-syntax-reference-for-aspnet-core"></a><span data-ttu-id="f6090-103">Referencia de sintaxis de Razor para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f6090-103">Razor syntax reference for ASP.NET Core</span></span>

<span data-ttu-id="f6090-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen) y [Dan Vicarel](https://github.com/Rabadash8820)</span><span class="sxs-lookup"><span data-stu-id="f6090-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen), and [Dan Vicarel](https://github.com/Rabadash8820)</span></span>

<span data-ttu-id="f6090-105">Razor es una sintaxis de marcado para insertar código basado en servidor en páginas web.</span><span class="sxs-lookup"><span data-stu-id="f6090-105">Razor is a markup syntax for embedding server-based code into webpages.</span></span> <span data-ttu-id="f6090-106">La sintaxis de Razor combina marcado de Razor, C# y HTML.</span><span class="sxs-lookup"><span data-stu-id="f6090-106">The Razor syntax consists of Razor markup, C#, and HTML.</span></span> <span data-ttu-id="f6090-107">Los archivos que contienen sintaxis de Razor suelen tener la extensión de archivo *.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f6090-107">Files containing Razor generally have a *.cshtml* file extension.</span></span>

## <a name="rendering-html"></a><span data-ttu-id="f6090-108">Representación de HTML</span><span class="sxs-lookup"><span data-stu-id="f6090-108">Rendering HTML</span></span>

<span data-ttu-id="f6090-109">El lenguaje de Razor predeterminado es HTML.</span><span class="sxs-lookup"><span data-stu-id="f6090-109">The default Razor language is HTML.</span></span> <span data-ttu-id="f6090-110">Representar el HTML del marcado de Razor no difiere mucho de representar el HTML de un archivo HTML.</span><span class="sxs-lookup"><span data-stu-id="f6090-110">Rendering HTML from Razor markup is no different than rendering HTML from an HTML file.</span></span> <span data-ttu-id="f6090-111">El marcado HTML de los archivos de Razor *.cshtml* se representa en el servidor sin cambios.</span><span class="sxs-lookup"><span data-stu-id="f6090-111">HTML markup in *.cshtml* Razor files is rendered by the server unchanged.</span></span>

## <a name="razor-syntax"></a><span data-ttu-id="f6090-112">Sintaxis de Razor</span><span class="sxs-lookup"><span data-stu-id="f6090-112">Razor syntax</span></span>

<span data-ttu-id="f6090-113">Razor admite C# y usa el símbolo `@` para realizar la transición de HTML a C#.</span><span class="sxs-lookup"><span data-stu-id="f6090-113">Razor supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> <span data-ttu-id="f6090-114">Razor evalúa las expresiones de C# y las representa en la salida HTML.</span><span class="sxs-lookup"><span data-stu-id="f6090-114">Razor evaluates C# expressions and renders them in the HTML output.</span></span>

<span data-ttu-id="f6090-115">Cuando el símbolo `@` va seguido de una [palabra clave reservada de Razor](#razor-reserved-keywords), realiza una transición a un marcado específico de Razor;</span><span class="sxs-lookup"><span data-stu-id="f6090-115">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords), it transitions into Razor-specific markup.</span></span> <span data-ttu-id="f6090-116">en caso contrario, realiza la transición a C# simple.</span><span class="sxs-lookup"><span data-stu-id="f6090-116">Otherwise, it transitions into plain C#.</span></span>

<span data-ttu-id="f6090-117">Para hacer escape en un símbolo `@` en el marcado de Razor, use un segundo símbolo `@`:</span><span class="sxs-lookup"><span data-stu-id="f6090-117">To escape an `@` symbol in Razor markup, use a second `@` symbol:</span></span>

```cshtml
<p>@@Username</p>
```

<span data-ttu-id="f6090-118">El código aparecerá en HTML con un solo símbolo `@`:</span><span class="sxs-lookup"><span data-stu-id="f6090-118">The code is rendered in HTML with a single `@` symbol:</span></span>

```html
<p>@Username</p>
```

<span data-ttu-id="f6090-119">El contenido y los atributos HTML que tienen direcciones de correo electrónico no tratan el símbolo `@` como un carácter de transición.</span><span class="sxs-lookup"><span data-stu-id="f6090-119">HTML attributes and content containing email addresses don't treat the `@` symbol as a transition character.</span></span> <span data-ttu-id="f6090-120">El análisis de Razor no se detiene en las direcciones de correo electrónico del siguiente ejemplo:</span><span class="sxs-lookup"><span data-stu-id="f6090-120">The email addresses in the following example are untouched by Razor parsing:</span></span>

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a><span data-ttu-id="f6090-121">Expresiones de Razor implícitas</span><span class="sxs-lookup"><span data-stu-id="f6090-121">Implicit Razor expressions</span></span>

<span data-ttu-id="f6090-122">Las expresiones de Razor implícitas comienzan por `@`, seguido de código C#:</span><span class="sxs-lookup"><span data-stu-id="f6090-122">Implicit Razor expressions start with `@` followed by C# code:</span></span>

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="f6090-123">Con la excepción de la palabra clave de C# `await`, las expresiones implícitas no deben contener espacios.</span><span class="sxs-lookup"><span data-stu-id="f6090-123">With the exception of the C# `await` keyword, implicit expressions must not contain spaces.</span></span> <span data-ttu-id="f6090-124">Si la instrucción de C# tiene un final claro, se pueden entremezclar espacios:</span><span class="sxs-lookup"><span data-stu-id="f6090-124">If the C# statement has a clear ending, spaces can be intermingled:</span></span>

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

<span data-ttu-id="f6090-125">Las expresiones implícitas **no pueden** contener tipos genéricos de C#, ya que los caracteres dentro de los corchetes (`<>`) se interpretan como una etiqueta HTML.</span><span class="sxs-lookup"><span data-stu-id="f6090-125">Implicit expressions **cannot** contain C# generics, as the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="f6090-126">El siguiente código **no** es válido:</span><span class="sxs-lookup"><span data-stu-id="f6090-126">The following code is **not** valid:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="f6090-127">El código anterior genera un error del compilador similar a uno de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="f6090-127">The preceding code generates a compiler error similar to one of the following:</span></span>

* <span data-ttu-id="f6090-128">El elemento "int" no estaba cerrado.</span><span class="sxs-lookup"><span data-stu-id="f6090-128">The "int" element wasn't closed.</span></span> <span data-ttu-id="f6090-129">Todos los elementos deben ser de autocierre o tener una etiqueta de fin coincidente.</span><span class="sxs-lookup"><span data-stu-id="f6090-129">All elements must be either self-closing or have a matching end tag.</span></span>
* <span data-ttu-id="f6090-130">No se puede convertir el grupo de métodos "GenericMethod" en el tipo no delegado "object".</span><span class="sxs-lookup"><span data-stu-id="f6090-130">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="f6090-131">¿Intentó invocar el método?</span><span class="sxs-lookup"><span data-stu-id="f6090-131">Did you intend to invoke the method?\`</span></span>

<span data-ttu-id="f6090-132">Las llamadas a método genéricas deben estar incluidas en una [expresión de Razor explícita](#explicit-razor-expressions) o en un [bloque de código de Razor](#razor-code-blocks).</span><span class="sxs-lookup"><span data-stu-id="f6090-132">Generic method calls must be wrapped in an [explicit Razor expression](#explicit-razor-expressions) or a [Razor code block](#razor-code-blocks).</span></span>

## <a name="explicit-razor-expressions"></a><span data-ttu-id="f6090-133">Expresiones de Razor explícitas</span><span class="sxs-lookup"><span data-stu-id="f6090-133">Explicit Razor expressions</span></span>

<span data-ttu-id="f6090-134">Las expresiones explícitas de Razor constan de un símbolo `@` y paréntesis de apertura y de cierre.</span><span class="sxs-lookup"><span data-stu-id="f6090-134">Explicit Razor expressions consist of an `@` symbol with balanced parenthesis.</span></span> <span data-ttu-id="f6090-135">Para representar la hora de la semana pasada, se usaría el siguiente marcado de Razor:</span><span class="sxs-lookup"><span data-stu-id="f6090-135">To render last week's time, the following Razor markup is used:</span></span>

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="f6090-136">El contenido que haya entre paréntesis `@()` se evalúa y se representa en la salida.</span><span class="sxs-lookup"><span data-stu-id="f6090-136">Any content within the `@()` parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="f6090-137">Por lo general, las expresiones implícitas (que explicamos en la sección anterior) no pueden contener espacios.</span><span class="sxs-lookup"><span data-stu-id="f6090-137">Implicit expressions, described in the previous section, generally can't contain spaces.</span></span> <span data-ttu-id="f6090-138">En el siguiente código, una semana no se resta de la hora actual:</span><span class="sxs-lookup"><span data-stu-id="f6090-138">In the following code, one week isn't subtracted from the current time:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact.cshtml?range=17)]

<span data-ttu-id="f6090-139">El código representa el siguiente HTML:</span><span class="sxs-lookup"><span data-stu-id="f6090-139">The code renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

<span data-ttu-id="f6090-140">Se pueden usar expresiones explícitas para concatenar texto con un resultado de la expresión:</span><span class="sxs-lookup"><span data-stu-id="f6090-140">Explicit expressions can be used to concatenate text with an expression result:</span></span>

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="f6090-141">Sin la expresión explícita, `<p>Age@joe.Age</p>` se trataría como una dirección de correo electrónico y se mostraría como `<p>Age@joe.Age</p>`.</span><span class="sxs-lookup"><span data-stu-id="f6090-141">Without the explicit expression, `<p>Age@joe.Age</p>` is treated as an email address, and `<p>Age@joe.Age</p>` is rendered.</span></span> <span data-ttu-id="f6090-142">Pero, si se escribe como una expresión explícita, se representa `<p>Age33</p>`.</span><span class="sxs-lookup"><span data-stu-id="f6090-142">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>

<span data-ttu-id="f6090-143">Se pueden usar expresiones explícitas para representar la salida de métodos genéricos en los archivos *.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f6090-143">Explicit expressions can be used to render output from generic methods in *.cshtml* files.</span></span> <span data-ttu-id="f6090-144">En el siguiente marcado se muestra cómo corregir el error mostrado anteriormente provocado por el uso de corchetes en un código C# genérico.</span><span class="sxs-lookup"><span data-stu-id="f6090-144">The following markup shows how to correct the error shown earlier caused by the brackets of a C# generic.</span></span> <span data-ttu-id="f6090-145">El código se escribe como una expresión explícita:</span><span class="sxs-lookup"><span data-stu-id="f6090-145">The code is written as an explicit expression:</span></span>

```cshtml
<p>@(GenericMethod<int>())</p>
```

## <a name="expression-encoding"></a><span data-ttu-id="f6090-146">Codificación de expresiones</span><span class="sxs-lookup"><span data-stu-id="f6090-146">Expression encoding</span></span>

<span data-ttu-id="f6090-147">Las expresiones de C# que se evalúan como una cadena están codificadas en HTML.</span><span class="sxs-lookup"><span data-stu-id="f6090-147">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="f6090-148">Las expresiones de C# que se evalúan como `IHtmlContent` se representan directamente a través de `IHtmlContent.WriteTo`.</span><span class="sxs-lookup"><span data-stu-id="f6090-148">C# expressions that evaluate to `IHtmlContent` are rendered directly through `IHtmlContent.WriteTo`.</span></span> <span data-ttu-id="f6090-149">Las expresiones de C# que no se evalúan como `IHtmlContent` se convierten en una cadena por medio de `ToString` y se codifican antes de que se representen.</span><span class="sxs-lookup"><span data-stu-id="f6090-149">C# expressions that don't evaluate to `IHtmlContent` are converted to a string by `ToString` and encoded before they're rendered.</span></span>

```cshtml
@("<span>Hello World</span>")
```

<span data-ttu-id="f6090-150">El código representa el siguiente HTML:</span><span class="sxs-lookup"><span data-stu-id="f6090-150">The code renders the following HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

<span data-ttu-id="f6090-151">El HTML se muestra en el explorador como:</span><span class="sxs-lookup"><span data-stu-id="f6090-151">The HTML is shown in the browser as:</span></span>

```
<span>Hello World</span>
```

<span data-ttu-id="f6090-152">La salida de `HtmlHelper.Raw` no está codificada, pero se representa como marcado HTML.</span><span class="sxs-lookup"><span data-stu-id="f6090-152">`HtmlHelper.Raw` output isn't encoded but rendered as HTML markup.</span></span>

> [!WARNING]
> <span data-ttu-id="f6090-153">Usar `HtmlHelper.Raw` en una entrada de usuario no saneada constituye un riesgo de seguridad.</span><span class="sxs-lookup"><span data-stu-id="f6090-153">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="f6090-154">Dicha entrada podría contener código JavaScript malintencionado u otras vulnerabilidades de seguridad.</span><span class="sxs-lookup"><span data-stu-id="f6090-154">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="f6090-155">Sanear una entrada de usuario es complicado.</span><span class="sxs-lookup"><span data-stu-id="f6090-155">Sanitizing user input is difficult.</span></span> <span data-ttu-id="f6090-156">Evite usar `HtmlHelper.Raw` con entradas de usuario.</span><span class="sxs-lookup"><span data-stu-id="f6090-156">Avoid using `HtmlHelper.Raw` with user input.</span></span>

```cshtml
@Html.Raw("<span>Hello World</span>")
```

<span data-ttu-id="f6090-157">El código representa el siguiente HTML:</span><span class="sxs-lookup"><span data-stu-id="f6090-157">The code renders the following HTML:</span></span>

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a><span data-ttu-id="f6090-158">Bloques de código de Razor</span><span class="sxs-lookup"><span data-stu-id="f6090-158">Razor code blocks</span></span>

<span data-ttu-id="f6090-159">Los bloques de código de Razor comienzan por `@` y se insertan entre `{}`.</span><span class="sxs-lookup"><span data-stu-id="f6090-159">Razor code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="f6090-160">A diferencia de las expresiones, el código de C# dentro de los bloques de código no se representa.</span><span class="sxs-lookup"><span data-stu-id="f6090-160">Unlike expressions, C# code inside code blocks isn't rendered.</span></span> <span data-ttu-id="f6090-161">Las expresiones y los bloques de código de una vista comparten el mismo ámbito y se definen en orden:</span><span class="sxs-lookup"><span data-stu-id="f6090-161">Code blocks and expressions in a view share the same scope and are defined in order:</span></span>

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

<span data-ttu-id="f6090-162">El código representa el siguiente HTML:</span><span class="sxs-lookup"><span data-stu-id="f6090-162">The code renders the following HTML:</span></span>

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

### <a name="implicit-transitions"></a><span data-ttu-id="f6090-163">Transiciones implícitas</span><span class="sxs-lookup"><span data-stu-id="f6090-163">Implicit transitions</span></span>

<span data-ttu-id="f6090-164">El lenguaje predeterminado de un bloque de código es C#, pero la página de Razor puede volver al HTML:</span><span class="sxs-lookup"><span data-stu-id="f6090-164">The default language in a code block is C#, but the Razor Page can transition back to HTML:</span></span>

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a><span data-ttu-id="f6090-165">Transición delimitada explícita</span><span class="sxs-lookup"><span data-stu-id="f6090-165">Explicit delimited transition</span></span>

<span data-ttu-id="f6090-166">Para definir una subsección de un bloque de código que deba representar HTML, inserte los caracteres que quiera representar entre etiquetas de Razor **\<text>**:</span><span class="sxs-lookup"><span data-stu-id="f6090-166">To define a subsection of a code block that should render HTML, surround the characters for rendering with the Razor **\<text>** tag:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="f6090-167">Emplee este método para representar HTML que no esté insertado entre etiquetas HTML.</span><span class="sxs-lookup"><span data-stu-id="f6090-167">Use this approach to render HTML that isn't surrounded by an HTML tag.</span></span> <span data-ttu-id="f6090-168">Sin una etiqueta HTML o de Razor, se produce un error de tiempo de ejecución de Razor.</span><span class="sxs-lookup"><span data-stu-id="f6090-168">Without an HTML or Razor tag, a Razor runtime error occurs.</span></span>

<span data-ttu-id="f6090-169">La etiqueta **\<text>** es útil para controlar el espacio en blanco al representar el contenido:</span><span class="sxs-lookup"><span data-stu-id="f6090-169">The **\<text>** tag is useful to control whitespace when rendering content:</span></span>

* <span data-ttu-id="f6090-170">Solo se representa el contenido entre etiquetas **\<text>**.</span><span class="sxs-lookup"><span data-stu-id="f6090-170">Only the content between the **\<text>** tag is rendered.</span></span>
* <span data-ttu-id="f6090-171">En la salida HTML no hay espacios en blanco antes o después de la etiqueta **\<text>**.</span><span class="sxs-lookup"><span data-stu-id="f6090-171">No whitespace before or after the **\<text>** tag appears in the HTML output.</span></span>

### <a name="explicit-line-transition-with-"></a><span data-ttu-id="f6090-172">Transición de línea explícita con @:</span><span class="sxs-lookup"><span data-stu-id="f6090-172">Explicit Line Transition with @:</span></span>

<span data-ttu-id="f6090-173">Para representar el resto de una línea completa como HTML dentro de un bloque de código, use la sintaxis `@:`:</span><span class="sxs-lookup"><span data-stu-id="f6090-173">To render the rest of an entire line as HTML inside a code block, use the `@:` syntax:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="f6090-174">Sin el carácter `@:` en el código, se produce un error de tiempo de ejecución de Razor.</span><span class="sxs-lookup"><span data-stu-id="f6090-174">Without the `@:` in the code, a Razor runtime error is generated.</span></span>

<span data-ttu-id="f6090-175">Advertencia: Si se incluyen caracteres `@` de más en un archivo de Razor, se pueden producir errores de compilador en las instrucciones más adelante en el bloque.</span><span class="sxs-lookup"><span data-stu-id="f6090-175">Warning: Extra `@` characters in a Razor file can cause compiler errors at statements later in the block.</span></span> <span data-ttu-id="f6090-176">Estos errores de compilador pueden ser difíciles de entender porque el error real se produce antes del error notificado.</span><span class="sxs-lookup"><span data-stu-id="f6090-176">These compiler errors can be difficult to understand because the actual error occurs before the reported error.</span></span> <span data-ttu-id="f6090-177">Este error es habitual después de combinar varias expresiones implícitas/explícitas en un mismo bloque de código.</span><span class="sxs-lookup"><span data-stu-id="f6090-177">This error is common after combining multiple implicit/explicit expressions into a single code block.</span></span>

## <a name="control-structures"></a><span data-ttu-id="f6090-178">Estructuras de control</span><span class="sxs-lookup"><span data-stu-id="f6090-178">Control structures</span></span>

<span data-ttu-id="f6090-179">Las estructuras de control son una extensión de los bloques de código.</span><span class="sxs-lookup"><span data-stu-id="f6090-179">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="f6090-180">Todos los aspectos de los bloques de código (transición a marcado, C# en línea) son válidos también en las siguientes estructuras:</span><span class="sxs-lookup"><span data-stu-id="f6090-180">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures:</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="f6090-181">Los condicionales @if, else if, else y @switch</span><span class="sxs-lookup"><span data-stu-id="f6090-181">Conditionals @if, else if, else, and @switch</span></span>

<span data-ttu-id="f6090-182">`@if` controla cuándo se ejecuta el código:</span><span class="sxs-lookup"><span data-stu-id="f6090-182">`@if` controls when code runs:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

<span data-ttu-id="f6090-183">`else` y `else if` no necesitan el símbolo `@`:</span><span class="sxs-lookup"><span data-stu-id="f6090-183">`else` and `else if` don't require the `@` symbol:</span></span>

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

<span data-ttu-id="f6090-184">En el siguiente marcado se muestra cómo usar una instrucción switch:</span><span class="sxs-lookup"><span data-stu-id="f6090-184">The following markup shows how to use a switch statement:</span></span>

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

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="f6090-185">@for, @foreach, @while y @do while en bucle</span><span class="sxs-lookup"><span data-stu-id="f6090-185">Looping @for, @foreach, @while, and @do while</span></span>

<span data-ttu-id="f6090-186">El HTML con plantilla se puede representar con instrucciones de control en bucle.</span><span class="sxs-lookup"><span data-stu-id="f6090-186">Templated HTML can be rendered with looping control statements.</span></span> <span data-ttu-id="f6090-187">Para representar una lista de personas:</span><span class="sxs-lookup"><span data-stu-id="f6090-187">To render a list of people:</span></span>

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

<span data-ttu-id="f6090-188">Se permiten las siguientes instrucciones en bucle:</span><span class="sxs-lookup"><span data-stu-id="f6090-188">The following looping statements are supported:</span></span>

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

### <a name="compound-using"></a><span data-ttu-id="f6090-189">Instrucción @using compuesta</span><span class="sxs-lookup"><span data-stu-id="f6090-189">Compound @using</span></span>

<span data-ttu-id="f6090-190">En C#, las instrucciones `using` se usan para garantizar que un objeto se elimina.</span><span class="sxs-lookup"><span data-stu-id="f6090-190">In C#, a `using` statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="f6090-191">En Razor, el mismo mecanismo se emplea para crear asistentes de HTML que incluyen contenido adicional.</span><span class="sxs-lookup"><span data-stu-id="f6090-191">In Razor, the same mechanism is used to create HTML Helpers that contain additional content.</span></span> <span data-ttu-id="f6090-192">En el siguiente código, los asistentes de HTML representan una etiqueta Form con la instrucción `@using`:</span><span class="sxs-lookup"><span data-stu-id="f6090-192">In the following code, HTML Helpers render a form tag with the `@using` statement:</span></span>

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

<span data-ttu-id="f6090-193">Con los [asistentes de etiquetas](xref:mvc/views/tag-helpers/intro) se pueden realizar acciones de nivel de ámbito.</span><span class="sxs-lookup"><span data-stu-id="f6090-193">Scope-level actions can be performed with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

### <a name="try-catch-finally"></a><span data-ttu-id="f6090-194">@try, catch, finally</span><span class="sxs-lookup"><span data-stu-id="f6090-194">@try, catch, finally</span></span>

<span data-ttu-id="f6090-195">El control de excepciones es similar a C#:</span><span class="sxs-lookup"><span data-stu-id="f6090-195">Exception handling is similar to C#:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

<span data-ttu-id="f6090-196">Razor tiene la capacidad de proteger las secciones más importantes con instrucciones de bloqueo:</span><span class="sxs-lookup"><span data-stu-id="f6090-196">Razor has the capability to protect critical sections with lock statements:</span></span>

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="f6090-197">Comentarios</span><span class="sxs-lookup"><span data-stu-id="f6090-197">Comments</span></span>

<span data-ttu-id="f6090-198">Razor admite comentarios tanto de C# como HTML:</span><span class="sxs-lookup"><span data-stu-id="f6090-198">Razor supports C# and HTML comments:</span></span>

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

<span data-ttu-id="f6090-199">El código representa el siguiente HTML:</span><span class="sxs-lookup"><span data-stu-id="f6090-199">The code renders the following HTML:</span></span>

```html
<!-- HTML comment -->
```

<span data-ttu-id="f6090-200">El servidor quitará los comentarios de Razor antes de mostrar la página web.</span><span class="sxs-lookup"><span data-stu-id="f6090-200">Razor comments are removed by the server before the webpage is rendered.</span></span> <span data-ttu-id="f6090-201">Razor usa `@*  *@` para delimitar comentarios.</span><span class="sxs-lookup"><span data-stu-id="f6090-201">Razor uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="f6090-202">El siguiente código está comentado, de modo que el servidor no representa ningún marcado:</span><span class="sxs-lookup"><span data-stu-id="f6090-202">The following code is commented out, so the server doesn't render any markup:</span></span>

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a><span data-ttu-id="f6090-203">Directivas</span><span class="sxs-lookup"><span data-stu-id="f6090-203">Directives</span></span>

<span data-ttu-id="f6090-204">Las directivas de Razor se representan en las expresiones implícitas con palabras clave reservadas seguidas del símbolo `@`.</span><span class="sxs-lookup"><span data-stu-id="f6090-204">Razor directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="f6090-205">Normalmente, una directiva cambia la forma en que una vista se analiza, o bien habilita una funcionalidad diferente.</span><span class="sxs-lookup"><span data-stu-id="f6090-205">A directive typically changes the way a view is parsed or enables different functionality.</span></span>

<span data-ttu-id="f6090-206">Conocer el modo en que Razor genera el código de una vista hace que sea más fácil comprender cómo funcionan las directivas.</span><span class="sxs-lookup"><span data-stu-id="f6090-206">Understanding how Razor generates code for a view makes it easier to understand how directives work.</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact8.cshtml)]

<span data-ttu-id="f6090-207">El código genera una clase similar a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="f6090-207">The code generates a class similar to the following:</span></span>

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

<span data-ttu-id="f6090-208">Más adelante en este artículo, en la sección [Inspección de la clase C# de Razor generada por una vista](#inspect-the-razor-c-class-generated-for-a-view), se explica cómo ver esta clase generada.</span><span class="sxs-lookup"><span data-stu-id="f6090-208">Later in this article, the section [Inspect the Razor C# class generated for a view](#inspect-the-razor-c-class-generated-for-a-view) explains how to view this generated class.</span></span>

<a name="using"></a>

### <a name="using"></a>@using

<span data-ttu-id="f6090-209">La directiva `@using` agrega la directiva `using` de C# a la vista generada:</span><span class="sxs-lookup"><span data-stu-id="f6090-209">The `@using` directive adds the C# `using` directive to the generated view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

<span data-ttu-id="f6090-210">La directiva `@model` especifica el tipo del modelo que se pasa a una vista:</span><span class="sxs-lookup"><span data-stu-id="f6090-210">The `@model` directive specifies the type of the model passed to a view:</span></span>

```cshtml
@model TypeNameOfModel
```

<span data-ttu-id="f6090-211">En una aplicación ASP.NET Core MVC creada con cuentas de usuario individuales, la vista *Views/Account/Login.cshtml* contiene la siguiente declaración de modelo:</span><span class="sxs-lookup"><span data-stu-id="f6090-211">In an ASP.NET Core MVC app created with individual user accounts, the *Views/Account/Login.cshtml* view contains the following model declaration:</span></span>

```cshtml
@model LoginViewModel
```

<span data-ttu-id="f6090-212">La clase generada se hereda de `RazorPage<dynamic>`:</span><span class="sxs-lookup"><span data-stu-id="f6090-212">The class generated inherits from `RazorPage<dynamic>`:</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

<span data-ttu-id="f6090-213">Razor expone una propiedad `Model` para tener acceso al modelo que se ha pasado a la vista:</span><span class="sxs-lookup"><span data-stu-id="f6090-213">Razor exposes a `Model` property for accessing the model passed to the view:</span></span>

```cshtml
<div>The Login Email: @Model.Email</div>
```

<span data-ttu-id="f6090-214">La directiva `@model` especifica el tipo de esta propiedad.</span><span class="sxs-lookup"><span data-stu-id="f6090-214">The `@model` directive specifies the type of this property.</span></span> <span data-ttu-id="f6090-215">La directiva especifica el elemento `T` en `RazorPage<T>` de la clase generada de la que se deriva la vista.</span><span class="sxs-lookup"><span data-stu-id="f6090-215">The directive specifies the `T` in `RazorPage<T>` that the generated class that the view derives from.</span></span> <span data-ttu-id="f6090-216">Si la directiva `@model` no se especifica, la propiedad `Model` es de tipo `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="f6090-216">If the `@model` directive isn't specified, the `Model` property is of type `dynamic`.</span></span> <span data-ttu-id="f6090-217">El valor del modelo se pasa del controlador a la vista.</span><span class="sxs-lookup"><span data-stu-id="f6090-217">The value of the model is passed from the controller to the view.</span></span> <span data-ttu-id="f6090-218">Para más información, vea [Modelos fuertemente tipados y la palabra clave &commat;model](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-and-the--keyword).</span><span class="sxs-lookup"><span data-stu-id="f6090-218">For more information, see [Strongly typed models and the &commat;model keyword](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-and-the--keyword).</span></span>

### <a name="inherits"></a>@inherits

<span data-ttu-id="f6090-219">La directiva `@inherits` proporciona control total sobre la clase que la vista hereda:</span><span class="sxs-lookup"><span data-stu-id="f6090-219">The `@inherits` directive provides full control of the class the view inherits:</span></span>

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

<span data-ttu-id="f6090-220">El siguiente código es un tipo personalizado de página de Razor:</span><span class="sxs-lookup"><span data-stu-id="f6090-220">The following code is a custom Razor page type:</span></span>

[!code-csharp[](razor/sample/Classes/CustomRazorPage.cs)]

<span data-ttu-id="f6090-221">`CustomText` se muestra en una vista:</span><span class="sxs-lookup"><span data-stu-id="f6090-221">The `CustomText` is displayed in a view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact10.cshtml)]

<span data-ttu-id="f6090-222">El código representa el siguiente HTML:</span><span class="sxs-lookup"><span data-stu-id="f6090-222">The code renders the following HTML:</span></span>

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

 <span data-ttu-id="f6090-223">`@model` y `@inherits` se pueden usar en la misma vista.</span><span class="sxs-lookup"><span data-stu-id="f6090-223">`@model` and `@inherits` can be used in the same view.</span></span> <span data-ttu-id="f6090-224">`@inherits` puede estar en un archivo *_ViewImports.cshtml* que la vista importa:</span><span class="sxs-lookup"><span data-stu-id="f6090-224">`@inherits` can be in a *_ViewImports.cshtml* file that the view imports:</span></span>

[!code-cshtml[](razor/sample/Views/_ViewImportsModel.cshtml)]

<span data-ttu-id="f6090-225">El siguiente código es un ejemplo de una vista fuertemente tipada:</span><span class="sxs-lookup"><span data-stu-id="f6090-225">The following code is an example of a strongly-typed view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Login1.cshtml)]

<span data-ttu-id="f6090-226">Si "rick@contoso.com" se pasa en el modelo, la vista genera el siguiente marcado HTML:</span><span class="sxs-lookup"><span data-stu-id="f6090-226">If "rick@contoso.com" is passed in the model, the view generates the following HTML markup:</span></span>

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject

<span data-ttu-id="f6090-227">La directiva `@inject` permite a la página de Razor insertar un servicio del [contenedor de servicios](xref:fundamentals/dependency-injection) en una vista.</span><span class="sxs-lookup"><span data-stu-id="f6090-227">The `@inject` directive enables the Razor Page to inject a service from the [service container](xref:fundamentals/dependency-injection) into a view.</span></span> <span data-ttu-id="f6090-228">Para más información, vea [Dependency injection into views](xref:mvc/views/dependency-injection) (Inserción de dependencias en vistas).</span><span class="sxs-lookup"><span data-stu-id="f6090-228">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

### <a name="functions"></a>@functions

<span data-ttu-id="f6090-229">La directiva `@functions` permite que una página de Razor agregue un bloque de código de C# a una vista:</span><span class="sxs-lookup"><span data-stu-id="f6090-229">The `@functions` directive enables a Razor Page to add a C# code block to a view:</span></span>

```cshtml
@functions { // C# Code }
```

<span data-ttu-id="f6090-230">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="f6090-230">For example:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact6.cshtml)]

<span data-ttu-id="f6090-231">El código genera el siguiente marcado HTML:</span><span class="sxs-lookup"><span data-stu-id="f6090-231">The code generates the following HTML markup:</span></span>

```html
<div>From method: Hello</div>
```

<span data-ttu-id="f6090-232">El siguiente código es la clase C# de Razor generada:</span><span class="sxs-lookup"><span data-stu-id="f6090-232">The following code is the generated Razor C# class:</span></span>

[!code-csharp[](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### <a name="section"></a>@section

<span data-ttu-id="f6090-233">La directiva `@section` se usa junto con el [diseño](xref:mvc/views/layout) para permitir que las vistas representen el contenido en diferentes partes de la página HTML.</span><span class="sxs-lookup"><span data-stu-id="f6090-233">The `@section` directive is used in conjunction with the [layout](xref:mvc/views/layout) to enable views to render content in different parts of the HTML page.</span></span> <span data-ttu-id="f6090-234">Para más información, vea [Sections](xref:mvc/views/layout#layout-sections-label) (Secciones).</span><span class="sxs-lookup"><span data-stu-id="f6090-234">For more information, see [Sections](xref:mvc/views/layout#layout-sections-label).</span></span>

## <a name="templated-razor-delegates"></a><span data-ttu-id="f6090-235">Delegados con plantillas de Razor</span><span class="sxs-lookup"><span data-stu-id="f6090-235">Templated Razor delegates</span></span>

<span data-ttu-id="f6090-236">Las plantillas de Razor permiten definir un fragmento de la interfaz de usuario con el formato siguiente:</span><span class="sxs-lookup"><span data-stu-id="f6090-236">Razor templates allow you to define a UI snippet with the following format:</span></span>

```cshtml
@<tag>...</tag>
```

<span data-ttu-id="f6090-237">En el ejemplo siguiente se muestra cómo especificar un delegado de Razor con plantilla como elemento <xref:System.Func`2>.</span><span class="sxs-lookup"><span data-stu-id="f6090-237">The following example illustrates how to specify a templated Razor delegate as a <xref:System.Func`2>.</span></span> <span data-ttu-id="f6090-238">El [tipo dinámico](/dotnet/csharp/programming-guide/types/using-type-dynamic) se especifica para el parámetro del método encapsulado por el delegado.</span><span class="sxs-lookup"><span data-stu-id="f6090-238">The [dynamic type](/dotnet/csharp/programming-guide/types/using-type-dynamic) is specified for the parameter of the method that the delegate encapsulates.</span></span> <span data-ttu-id="f6090-239">Se especifica un [tipo de objeto](/dotnet/csharp/language-reference/keywords/object) como el valor devuelto del delegado.</span><span class="sxs-lookup"><span data-stu-id="f6090-239">An [object type](/dotnet/csharp/language-reference/keywords/object) is specified as the return value of the delegate.</span></span> <span data-ttu-id="f6090-240">La plantilla se usa con un elemento <xref:System.Collections.Generic.List`1> de `Pet` que tiene una propiedad `Name`.</span><span class="sxs-lookup"><span data-stu-id="f6090-240">The template is used with a <xref:System.Collections.Generic.List`1> of `Pet` that has a `Name` property.</span></span>

```csharp
public class Pet
{
    public string Name { get; set; }
}
```

```cshtml
@{
    Func<dynamic, object> petTemplate = @<p>You have a pet named <strong>@item.Name</strong>.</p>;

    var pets = new List<Pet>
    {
        new Pet { Name = "Rin Tin Tin" },
        new Pet { Name = "Mr. Bigglesworth" },
        new Pet { Name = "K-9" }
    };
}
```

<span data-ttu-id="f6090-241">La plantilla se representa con el elemento `pets` proporcionado por una instrucción `foreach`:</span><span class="sxs-lookup"><span data-stu-id="f6090-241">The template is rendered with `pets` supplied by a `foreach` statement:</span></span>

```cshtml
@foreach (var pet in pets)
{
    @petTemplate(pet)
}
```

<span data-ttu-id="f6090-242">Salida representada:</span><span class="sxs-lookup"><span data-stu-id="f6090-242">Rendered output:</span></span>

```html
<p>You have a pet named <strong>Rin Tin Tin</strong>.</p>
<p>You have a pet named <strong>Mr. Bigglesworth</strong>.</p>
<p>You have a pet named <strong>K-9</strong>.</p>
```

<span data-ttu-id="f6090-243">También se puede proporcionar una plantilla de Razor insertada como un argumento para un método.</span><span class="sxs-lookup"><span data-stu-id="f6090-243">You can also supply an inline Razor template as an argument to a method.</span></span> <span data-ttu-id="f6090-244">En el ejemplo siguiente, el método `Repeat` recibe una plantilla de Razor.</span><span class="sxs-lookup"><span data-stu-id="f6090-244">In the following example, the `Repeat` method receives a Razor template.</span></span> <span data-ttu-id="f6090-245">El método usa la plantilla para generar contenido HTML con repeticiones de elementos proporcionados a partir de una lista:</span><span class="sxs-lookup"><span data-stu-id="f6090-245">The method uses the template to produce HTML content with repeats of items supplied from a list:</span></span>

```cshtml
@using Microsoft.AspNetCore.Html

@functions {
    public static IHtmlContent Repeat(IEnumerable<dynamic> items, int times,
        Func<dynamic, IHtmlContent> template)
    {
        var html = new HtmlContentBuilder();

        foreach (var item in items)
        {
            for (var i = 0; i < times; i++)
            {
                html.AppendHtml(template(item));
            }
        }

        return html;
    }
}
```

<span data-ttu-id="f6090-246">Con la lista de mascotas del ejemplo anterior, se llama al método `Repeat` con:</span><span class="sxs-lookup"><span data-stu-id="f6090-246">Using the list of pets from the prior example, the `Repeat` method is called with:</span></span>

* <span data-ttu-id="f6090-247"><xref:System.Collections.Generic.List`1> de `Pet`.</span><span class="sxs-lookup"><span data-stu-id="f6090-247"><xref:System.Collections.Generic.List`1> of `Pet`.</span></span>
* <span data-ttu-id="f6090-248">Número de veces que se repite cada mascota.</span><span class="sxs-lookup"><span data-stu-id="f6090-248">Number of times to repeat each pet.</span></span>
* <span data-ttu-id="f6090-249">Plantilla insertada que se va a usar para los elementos de una lista sin ordenar.</span><span class="sxs-lookup"><span data-stu-id="f6090-249">Inline template to use for the list items of an unordered list.</span></span>

```cshtml
<ul>
    @Repeat(pets, 3, @<li>@item.Name</li>)
</ul>
```

<span data-ttu-id="f6090-250">Salida representada:</span><span class="sxs-lookup"><span data-stu-id="f6090-250">Rendered output:</span></span>

```html
<ul>
    <li>Rin Tin Tin</li>
    <li>Rin Tin Tin</li>
    <li>Rin Tin Tin</li>
    <li>Mr. Bigglesworth</li>
    <li>Mr. Bigglesworth</li>
    <li>Mr. Bigglesworth</li>
    <li>K-9</li>
    <li>K-9</li>
    <li>K-9</li>
</ul>
```

## <a name="tag-helpers"></a><span data-ttu-id="f6090-251">Asistentes de etiquetas</span><span class="sxs-lookup"><span data-stu-id="f6090-251">Tag Helpers</span></span>

<span data-ttu-id="f6090-252">Hay tres directivas que pertenecen a los [asistentes de etiquetas](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="f6090-252">There are three directives that pertain to [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

| <span data-ttu-id="f6090-253">Directiva</span><span class="sxs-lookup"><span data-stu-id="f6090-253">Directive</span></span> | <span data-ttu-id="f6090-254">Función</span><span class="sxs-lookup"><span data-stu-id="f6090-254">Function</span></span> |
| --------- | -------- |
| [<span data-ttu-id="f6090-255">&commat;addTagHelper</span><span class="sxs-lookup"><span data-stu-id="f6090-255">&commat;addTagHelper</span></span>](xref:mvc/views/tag-helpers/intro#add-helper-label) | <span data-ttu-id="f6090-256">Pone los asistentes de etiquetas a disposición de una vista.</span><span class="sxs-lookup"><span data-stu-id="f6090-256">Makes Tag Helpers available to a view.</span></span> |
| [<span data-ttu-id="f6090-257">&commat;removeTagHelper</span><span class="sxs-lookup"><span data-stu-id="f6090-257">&commat;removeTagHelper</span></span>](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | <span data-ttu-id="f6090-258">Quita los asistentes de etiquetas agregadas anteriormente desde una vista.</span><span class="sxs-lookup"><span data-stu-id="f6090-258">Removes Tag Helpers previously added from a view.</span></span> |
| [<span data-ttu-id="f6090-259">&commat;tagHelperPrefix</span><span class="sxs-lookup"><span data-stu-id="f6090-259">&commat;tagHelperPrefix</span></span>](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | <span data-ttu-id="f6090-260">Especifica una cadena de prefijo de etiqueta para permitir la compatibilidad con el asistente de etiquetas y hacer explícito su uso.</span><span class="sxs-lookup"><span data-stu-id="f6090-260">Specifies a tag prefix to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> |

## <a name="razor-reserved-keywords"></a><span data-ttu-id="f6090-261">Palabras clave reservadas de Razor</span><span class="sxs-lookup"><span data-stu-id="f6090-261">Razor reserved keywords</span></span>

### <a name="razor-keywords"></a><span data-ttu-id="f6090-262">Palabras clave de Razor</span><span class="sxs-lookup"><span data-stu-id="f6090-262">Razor keywords</span></span>

* <span data-ttu-id="f6090-263">page (requiere ASP.NET Core 2.0 y versiones posteriores)</span><span class="sxs-lookup"><span data-stu-id="f6090-263">page (Requires ASP.NET Core 2.0 and later)</span></span>
* <span data-ttu-id="f6090-264">namespace</span><span class="sxs-lookup"><span data-stu-id="f6090-264">namespace</span></span>
* <span data-ttu-id="f6090-265">funciones</span><span class="sxs-lookup"><span data-stu-id="f6090-265">functions</span></span>
* <span data-ttu-id="f6090-266">hereda</span><span class="sxs-lookup"><span data-stu-id="f6090-266">inherits</span></span>
* <span data-ttu-id="f6090-267">modelo</span><span class="sxs-lookup"><span data-stu-id="f6090-267">model</span></span>
* <span data-ttu-id="f6090-268">section</span><span class="sxs-lookup"><span data-stu-id="f6090-268">section</span></span>
* <span data-ttu-id="f6090-269">helper (no admitida en ASP.NET Core actualmente)</span><span class="sxs-lookup"><span data-stu-id="f6090-269">helper (Not currently supported by ASP.NET Core)</span></span>

<span data-ttu-id="f6090-270">Para hacer escape en una palabra clave de Razor, se usa `@(Razor Keyword)` (por ejemplo, `@(functions)`).</span><span class="sxs-lookup"><span data-stu-id="f6090-270">Razor keywords are escaped with `@(Razor Keyword)` (for example, `@(functions)`).</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="f6090-271">Palabras clave C# de Razor</span><span class="sxs-lookup"><span data-stu-id="f6090-271">C# Razor keywords</span></span>

* <span data-ttu-id="f6090-272">mayúsculas y minúsculas</span><span class="sxs-lookup"><span data-stu-id="f6090-272">case</span></span>
* <span data-ttu-id="f6090-273">do</span><span class="sxs-lookup"><span data-stu-id="f6090-273">do</span></span>
* <span data-ttu-id="f6090-274">default</span><span class="sxs-lookup"><span data-stu-id="f6090-274">default</span></span>
* <span data-ttu-id="f6090-275">for</span><span class="sxs-lookup"><span data-stu-id="f6090-275">for</span></span>
* <span data-ttu-id="f6090-276">foreach</span><span class="sxs-lookup"><span data-stu-id="f6090-276">foreach</span></span>
* <span data-ttu-id="f6090-277">if</span><span class="sxs-lookup"><span data-stu-id="f6090-277">if</span></span>
* <span data-ttu-id="f6090-278">else</span><span class="sxs-lookup"><span data-stu-id="f6090-278">else</span></span>
* <span data-ttu-id="f6090-279">bloquear</span><span class="sxs-lookup"><span data-stu-id="f6090-279">lock</span></span>
* <span data-ttu-id="f6090-280">switch</span><span class="sxs-lookup"><span data-stu-id="f6090-280">switch</span></span>
* <span data-ttu-id="f6090-281">try</span><span class="sxs-lookup"><span data-stu-id="f6090-281">try</span></span>
* <span data-ttu-id="f6090-282">catch</span><span class="sxs-lookup"><span data-stu-id="f6090-282">catch</span></span>
* <span data-ttu-id="f6090-283">finally</span><span class="sxs-lookup"><span data-stu-id="f6090-283">finally</span></span>
* <span data-ttu-id="f6090-284">utilizar</span><span class="sxs-lookup"><span data-stu-id="f6090-284">using</span></span>
* <span data-ttu-id="f6090-285">while</span><span class="sxs-lookup"><span data-stu-id="f6090-285">while</span></span>

<span data-ttu-id="f6090-286">Las palabras clave C# de Razor deben tener doble escape con `@(@C# Razor Keyword)` (por ejemplo, `@(@case)`).</span><span class="sxs-lookup"><span data-stu-id="f6090-286">C# Razor keywords must be double-escaped with `@(@C# Razor Keyword)` (for example, `@(@case)`).</span></span> <span data-ttu-id="f6090-287">El primer carácter `@` hace escape en el analizador Razor</span><span class="sxs-lookup"><span data-stu-id="f6090-287">The first `@` escapes the Razor parser.</span></span> <span data-ttu-id="f6090-288">y el segundo `@`, en el analizador de C#.</span><span class="sxs-lookup"><span data-stu-id="f6090-288">The second `@` escapes the C# parser.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="f6090-289">Palabras clave reservadas no usadas en Razor</span><span class="sxs-lookup"><span data-stu-id="f6090-289">Reserved keywords not used by Razor</span></span>

* <span data-ttu-id="f6090-290">clase</span><span class="sxs-lookup"><span data-stu-id="f6090-290">class</span></span>

## <a name="inspect-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="f6090-291">Inspección de la clase C# de Razor generada por una vista</span><span class="sxs-lookup"><span data-stu-id="f6090-291">Inspect the Razor C# class generated for a view</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="f6090-292">Con el SDK de .NET Core 2.1 o posterior, el [SDK de Razor](xref:razor-pages/sdk) controla la compilación de los archivos de Razor.</span><span class="sxs-lookup"><span data-stu-id="f6090-292">With .NET Core SDK 2.1 or later, the [Razor SDK](xref:razor-pages/sdk) handles compilation of Razor files.</span></span> <span data-ttu-id="f6090-293">Al compilar un proyecto, el SDK de Razor genera un directorio *obj/<configuración_de_compilación>/<moniker_de_la_plataforma_de_destino>/Razor* en la raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="f6090-293">When building a project, the Razor SDK generates an *obj/<build_configuration>/<target_framework_moniker>/Razor* directory in the project root.</span></span> <span data-ttu-id="f6090-294">La estructura de directorios dentro del directorio *Razor* refleja la del proyecto.</span><span class="sxs-lookup"><span data-stu-id="f6090-294">The directory structure within the *Razor* directory mirrors the project's directory structure.</span></span>

<span data-ttu-id="f6090-295">Tenga en cuenta la estructura de directorios siguiente en un proyecto de Razor Pages de ASP.NET Core 2.1 destinado a .NET Core 2.1:</span><span class="sxs-lookup"><span data-stu-id="f6090-295">Consider the following directory structure in an ASP.NET Core 2.1 Razor Pages project targeting .NET Core 2.1:</span></span>

* <span data-ttu-id="f6090-296">**Areas/**</span><span class="sxs-lookup"><span data-stu-id="f6090-296">**Areas/**</span></span>
  * <span data-ttu-id="f6090-297">**Admin/**</span><span class="sxs-lookup"><span data-stu-id="f6090-297">**Admin/**</span></span>
    * <span data-ttu-id="f6090-298">**Pages/**</span><span class="sxs-lookup"><span data-stu-id="f6090-298">**Pages/**</span></span>
      * <span data-ttu-id="f6090-299">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="f6090-299">*Index.cshtml*</span></span>
      * <span data-ttu-id="f6090-300">*Index.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="f6090-300">*Index.cshtml.cs*</span></span>
* <span data-ttu-id="f6090-301">**Pages/**</span><span class="sxs-lookup"><span data-stu-id="f6090-301">**Pages/**</span></span>
  * <span data-ttu-id="f6090-302">**Shared/**</span><span class="sxs-lookup"><span data-stu-id="f6090-302">**Shared/**</span></span>
    * <span data-ttu-id="f6090-303">*_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="f6090-303">*_Layout.cshtml*</span></span>
  * <span data-ttu-id="f6090-304">*_ViewImports.cshtml*</span><span class="sxs-lookup"><span data-stu-id="f6090-304">*_ViewImports.cshtml*</span></span>
  * <span data-ttu-id="f6090-305">*_ViewStart.cshtml*</span><span class="sxs-lookup"><span data-stu-id="f6090-305">*_ViewStart.cshtml*</span></span>
  * <span data-ttu-id="f6090-306">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="f6090-306">*Index.cshtml*</span></span>
  * <span data-ttu-id="f6090-307">*Index.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="f6090-307">*Index.cshtml.cs*</span></span>

<span data-ttu-id="f6090-308">Al compilar el proyecto en la configuración *Depurar* se crea el directorio *obj* siguiente:</span><span class="sxs-lookup"><span data-stu-id="f6090-308">Building the project in *Debug* configuration yields the following *obj* directory:</span></span>

* <span data-ttu-id="f6090-309">**obj/**</span><span class="sxs-lookup"><span data-stu-id="f6090-309">**obj/**</span></span>
  * <span data-ttu-id="f6090-310">**Debug/**</span><span class="sxs-lookup"><span data-stu-id="f6090-310">**Debug/**</span></span>
    * <span data-ttu-id="f6090-311">**netcoreapp2.1/**</span><span class="sxs-lookup"><span data-stu-id="f6090-311">**netcoreapp2.1/**</span></span>
      * <span data-ttu-id="f6090-312">**Razor/**</span><span class="sxs-lookup"><span data-stu-id="f6090-312">**Razor/**</span></span>
        * <span data-ttu-id="f6090-313">**Areas/**</span><span class="sxs-lookup"><span data-stu-id="f6090-313">**Areas/**</span></span>
          * <span data-ttu-id="f6090-314">**Admin/**</span><span class="sxs-lookup"><span data-stu-id="f6090-314">**Admin/**</span></span>
            * <span data-ttu-id="f6090-315">**Pages/**</span><span class="sxs-lookup"><span data-stu-id="f6090-315">**Pages/**</span></span>
              * <span data-ttu-id="f6090-316">*Index.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="f6090-316">*Index.g.cshtml.cs*</span></span>
        * <span data-ttu-id="f6090-317">**Pages/**</span><span class="sxs-lookup"><span data-stu-id="f6090-317">**Pages/**</span></span>
          * <span data-ttu-id="f6090-318">**Shared/**</span><span class="sxs-lookup"><span data-stu-id="f6090-318">**Shared/**</span></span>
            * <span data-ttu-id="f6090-319">*_Layout.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="f6090-319">*_Layout.g.cshtml.cs*</span></span>
          * <span data-ttu-id="f6090-320">*_ViewImports.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="f6090-320">*_ViewImports.g.cshtml.cs*</span></span>
          * <span data-ttu-id="f6090-321">*_ViewStart.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="f6090-321">*_ViewStart.g.cshtml.cs*</span></span>
          * <span data-ttu-id="f6090-322">*Index.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="f6090-322">*Index.g.cshtml.cs*</span></span>

<span data-ttu-id="f6090-323">Para ver la clase generada para *Pages/Index.cshtml*, abra *obj/Debug/netcoreapp2.1/Razor/Pages/Index.g.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="f6090-323">To view the generated class for *Pages/Index.cshtml*, open *obj/Debug/netcoreapp2.1/Razor/Pages/Index.g.cshtml.cs*.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="f6090-324">Agregue la siguiente clase al proyecto de ASP.NET Core MVC:</span><span class="sxs-lookup"><span data-stu-id="f6090-324">Add the following class to the ASP.NET Core MVC project:</span></span>

[!code-csharp[](razor/sample/Utilities/CustomTemplateEngine.cs)]

<span data-ttu-id="f6090-325">En `Startup.ConfigureServices`, invalide el elemento `RazorTemplateEngine` agregado por MVC con la clase `CustomTemplateEngine`:</span><span class="sxs-lookup"><span data-stu-id="f6090-325">In `Startup.ConfigureServices`, override the `RazorTemplateEngine` added by MVC with the `CustomTemplateEngine` class:</span></span>

[!code-csharp[](razor/sample/Startup.cs?highlight=4&range=10-14)]

<span data-ttu-id="f6090-326">Establezca un punto de interrupción en la instrucción `return csharpDocument;` de `CustomTemplateEngine`.</span><span class="sxs-lookup"><span data-stu-id="f6090-326">Set a breakpoint on the `return csharpDocument;` statement of `CustomTemplateEngine`.</span></span> <span data-ttu-id="f6090-327">Cuando la ejecución del programa se detenga en el punto de interrupción, vea el valor de `generatedCode`.</span><span class="sxs-lookup"><span data-stu-id="f6090-327">When program execution stops at the breakpoint, view the value of `generatedCode`.</span></span>

![Vista del visualizador de texto de generatedCode](razor/_static/tvr.png)

::: moniker-end

## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="f6090-329">Búsquedas de vistas y distinción entre mayúsculas y minúsculas</span><span class="sxs-lookup"><span data-stu-id="f6090-329">View lookups and case sensitivity</span></span>

<span data-ttu-id="f6090-330">El motor de vista de Razor realiza búsquedas de vistas en las que se distingue entre mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="f6090-330">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="f6090-331">Pero la búsqueda real viene determinada por el sistema de archivos subyacente:</span><span class="sxs-lookup"><span data-stu-id="f6090-331">However, the actual lookup is determined by the underlying file system:</span></span>

* <span data-ttu-id="f6090-332">Origen basado en archivos:</span><span class="sxs-lookup"><span data-stu-id="f6090-332">File based source:</span></span>
  * <span data-ttu-id="f6090-333">En los sistemas operativos con sistemas de archivos que no distinguen entre mayúsculas y minúsculas (por ejemplo, Windows), las búsquedas de proveedor de archivos físicos no distinguirán mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="f6090-333">On operating systems with case insensitive file systems (for example, Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="f6090-334">Por ejemplo, `return View("Test")` arrojará como resultados */Views/Home/Test.cshtml*, */Views/home/test.cshtml* y cualquier otra variante de mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="f6090-334">For example, `return View("Test")` results in matches for */Views/Home/Test.cshtml*, */Views/home/test.cshtml*, and any other casing variant.</span></span>
  * <span data-ttu-id="f6090-335">En los sistemas de archivos en los que sí se distingue entre mayúsculas y minúsculas (por ejemplo, Linux, OSX y al usar `EmbeddedFileProvider`), las búsquedas distinguirán mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="f6090-335">On case-sensitive file systems (for example, Linux, OSX, and with `EmbeddedFileProvider`), lookups are case-sensitive.</span></span> <span data-ttu-id="f6090-336">Por ejemplo, `return View("Test")` mostrará el resultado */Views/Home/Test.cshtml* única y exclusivamente.</span><span class="sxs-lookup"><span data-stu-id="f6090-336">For example, `return View("Test")` specifically matches */Views/Home/Test.cshtml*.</span></span>
* <span data-ttu-id="f6090-337">Vistas precompiladas: En ASP.NET Core 2.0 y versiones posteriores, las búsquedas de vistas precompiladas no distinguen mayúsculas de minúsculas en todos los sistemas operativos.</span><span class="sxs-lookup"><span data-stu-id="f6090-337">Precompiled views: With ASP.NET Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="f6090-338">Este comportamiento es idéntico al comportamiento del proveedor de archivos físicos en Windows.</span><span class="sxs-lookup"><span data-stu-id="f6090-338">The behavior is identical to physical file provider's behavior on Windows.</span></span> <span data-ttu-id="f6090-339">Si dos vistas precompiladas difieren solo por sus mayúsculas o minúsculas, el resultado de la búsqueda será no determinante.</span><span class="sxs-lookup"><span data-stu-id="f6090-339">If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="f6090-340">Por tanto, se anima a todos los desarrolladores a intentar que las mayúsculas y minúsculas de los nombres de archivo y de directorio sean las mismas que las mayúsculas y minúsculas de:</span><span class="sxs-lookup"><span data-stu-id="f6090-340">Developers are encouraged to match the casing of file and directory names to the casing of:</span></span>

* <span data-ttu-id="f6090-341">Nombres de acciones, controladores y áreas.</span><span class="sxs-lookup"><span data-stu-id="f6090-341">Area, controller, and action names.</span></span>
* <span data-ttu-id="f6090-342">Páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="f6090-342">Razor Pages.</span></span>

<span data-ttu-id="f6090-343">La coincidencia de mayúsculas y minúsculas garantiza que las implementaciones van a encontrar sus vistas, independientemente de cuál sea el sistema de archivos subyacente.</span><span class="sxs-lookup"><span data-stu-id="f6090-343">Matching case ensures the deployments find their views regardless of the underlying file system.</span></span>
