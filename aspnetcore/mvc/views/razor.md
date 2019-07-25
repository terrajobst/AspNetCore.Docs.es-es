---
title: Referencia de sintaxis de Razor para ASP.NET Core
author: rick-anderson
description: Obtenga información sobre la sintaxis de marcado de Razor para insertar código basado en servidor en páginas web.
ms.author: riande
ms.date: 06/12/2019
uid: mvc/views/razor
ms.openlocfilehash: 634623aa6df860a2ff728da4e65d277326db37e2
ms.sourcegitcommit: 051f068c78931432e030b60094c38376d64d013e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2019
ms.locfileid: "68440367"
---
# <a name="razor-syntax-reference-for-aspnet-core"></a><span data-ttu-id="95edc-103">Referencia de sintaxis de Razor para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="95edc-103">Razor syntax reference for ASP.NET Core</span></span>

<span data-ttu-id="95edc-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen) y [Dan Vicarel](https://github.com/Rabadash8820)</span><span class="sxs-lookup"><span data-stu-id="95edc-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen), and [Dan Vicarel](https://github.com/Rabadash8820)</span></span>

<span data-ttu-id="95edc-105">Razor es una sintaxis de marcado para insertar código basado en servidor en páginas web.</span><span class="sxs-lookup"><span data-stu-id="95edc-105">Razor is a markup syntax for embedding server-based code into webpages.</span></span> <span data-ttu-id="95edc-106">La sintaxis de Razor combina marcado de Razor, C# y HTML.</span><span class="sxs-lookup"><span data-stu-id="95edc-106">The Razor syntax consists of Razor markup, C#, and HTML.</span></span> <span data-ttu-id="95edc-107">Los archivos que contienen sintaxis de Razor suelen tener la extensión de archivo *.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="95edc-107">Files containing Razor generally have a *.cshtml* file extension.</span></span>

## <a name="rendering-html"></a><span data-ttu-id="95edc-108">Representación de HTML</span><span class="sxs-lookup"><span data-stu-id="95edc-108">Rendering HTML</span></span>

<span data-ttu-id="95edc-109">El lenguaje de Razor predeterminado es HTML.</span><span class="sxs-lookup"><span data-stu-id="95edc-109">The default Razor language is HTML.</span></span> <span data-ttu-id="95edc-110">Representar el HTML del marcado de Razor no difiere mucho de representar el HTML de un archivo HTML.</span><span class="sxs-lookup"><span data-stu-id="95edc-110">Rendering HTML from Razor markup is no different than rendering HTML from an HTML file.</span></span> <span data-ttu-id="95edc-111">El marcado HTML de los archivos de Razor *.cshtml* se representa en el servidor sin cambios.</span><span class="sxs-lookup"><span data-stu-id="95edc-111">HTML markup in *.cshtml* Razor files is rendered by the server unchanged.</span></span>

## <a name="razor-syntax"></a><span data-ttu-id="95edc-112">Sintaxis de Razor</span><span class="sxs-lookup"><span data-stu-id="95edc-112">Razor syntax</span></span>

<span data-ttu-id="95edc-113">Razor admite C# y usa el símbolo `@` para realizar la transición de HTML a C#.</span><span class="sxs-lookup"><span data-stu-id="95edc-113">Razor supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> <span data-ttu-id="95edc-114">Razor evalúa las expresiones de C# y las representa en la salida HTML.</span><span class="sxs-lookup"><span data-stu-id="95edc-114">Razor evaluates C# expressions and renders them in the HTML output.</span></span>

<span data-ttu-id="95edc-115">Cuando el símbolo `@` va seguido de una [palabra clave reservada de Razor](#razor-reserved-keywords), realiza una transición a un marcado específico de Razor;</span><span class="sxs-lookup"><span data-stu-id="95edc-115">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords), it transitions into Razor-specific markup.</span></span> <span data-ttu-id="95edc-116">en caso contrario, realiza la transición a C# simple.</span><span class="sxs-lookup"><span data-stu-id="95edc-116">Otherwise, it transitions into plain C#.</span></span>

<span data-ttu-id="95edc-117">Para hacer escape en un símbolo `@` en el marcado de Razor, use un segundo símbolo `@`:</span><span class="sxs-lookup"><span data-stu-id="95edc-117">To escape an `@` symbol in Razor markup, use a second `@` symbol:</span></span>

```cshtml
<p>@@Username</p>
```

<span data-ttu-id="95edc-118">El código aparecerá en HTML con un solo símbolo `@`:</span><span class="sxs-lookup"><span data-stu-id="95edc-118">The code is rendered in HTML with a single `@` symbol:</span></span>

```html
<p>@Username</p>
```

<span data-ttu-id="95edc-119">El contenido y los atributos HTML que tienen direcciones de correo electrónico no tratan el símbolo `@` como un carácter de transición.</span><span class="sxs-lookup"><span data-stu-id="95edc-119">HTML attributes and content containing email addresses don't treat the `@` symbol as a transition character.</span></span> <span data-ttu-id="95edc-120">El análisis de Razor no se detiene en las direcciones de correo electrónico del siguiente ejemplo:</span><span class="sxs-lookup"><span data-stu-id="95edc-120">The email addresses in the following example are untouched by Razor parsing:</span></span>

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a><span data-ttu-id="95edc-121">Expresiones de Razor implícitas</span><span class="sxs-lookup"><span data-stu-id="95edc-121">Implicit Razor expressions</span></span>

<span data-ttu-id="95edc-122">Las expresiones de Razor implícitas comienzan por `@`, seguido de código C#:</span><span class="sxs-lookup"><span data-stu-id="95edc-122">Implicit Razor expressions start with `@` followed by C# code:</span></span>

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="95edc-123">Con la excepción de la palabra clave de C# `await`, las expresiones implícitas no deben contener espacios.</span><span class="sxs-lookup"><span data-stu-id="95edc-123">With the exception of the C# `await` keyword, implicit expressions must not contain spaces.</span></span> <span data-ttu-id="95edc-124">Si la instrucción de C# tiene un final claro, se pueden entremezclar espacios:</span><span class="sxs-lookup"><span data-stu-id="95edc-124">If the C# statement has a clear ending, spaces can be intermingled:</span></span>

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

<span data-ttu-id="95edc-125">Las expresiones implícitas **no pueden** contener tipos genéricos de C#, ya que los caracteres dentro de los corchetes (`<>`) se interpretan como una etiqueta HTML.</span><span class="sxs-lookup"><span data-stu-id="95edc-125">Implicit expressions **cannot** contain C# generics, as the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="95edc-126">El siguiente código **no** es válido:</span><span class="sxs-lookup"><span data-stu-id="95edc-126">The following code is **not** valid:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="95edc-127">El código anterior genera un error del compilador similar a uno de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="95edc-127">The preceding code generates a compiler error similar to one of the following:</span></span>

* <span data-ttu-id="95edc-128">El elemento "int" no estaba cerrado.</span><span class="sxs-lookup"><span data-stu-id="95edc-128">The "int" element wasn't closed.</span></span> <span data-ttu-id="95edc-129">Todos los elementos deben ser de autocierre o tener una etiqueta de fin coincidente.</span><span class="sxs-lookup"><span data-stu-id="95edc-129">All elements must be either self-closing or have a matching end tag.</span></span>
* <span data-ttu-id="95edc-130">No se puede convertir el grupo de métodos "GenericMethod" en el tipo no delegado "object".</span><span class="sxs-lookup"><span data-stu-id="95edc-130">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="95edc-131">¿Intentó invocar el método?</span><span class="sxs-lookup"><span data-stu-id="95edc-131">Did you intend to invoke the method?\`</span></span>

<span data-ttu-id="95edc-132">Las llamadas a método genéricas deben estar incluidas en una [expresión de Razor explícita](#explicit-razor-expressions) o en un [bloque de código de Razor](#razor-code-blocks).</span><span class="sxs-lookup"><span data-stu-id="95edc-132">Generic method calls must be wrapped in an [explicit Razor expression](#explicit-razor-expressions) or a [Razor code block](#razor-code-blocks).</span></span>

## <a name="explicit-razor-expressions"></a><span data-ttu-id="95edc-133">Expresiones de Razor explícitas</span><span class="sxs-lookup"><span data-stu-id="95edc-133">Explicit Razor expressions</span></span>

<span data-ttu-id="95edc-134">Las expresiones explícitas de Razor constan de un símbolo `@` y paréntesis de apertura y de cierre.</span><span class="sxs-lookup"><span data-stu-id="95edc-134">Explicit Razor expressions consist of an `@` symbol with balanced parenthesis.</span></span> <span data-ttu-id="95edc-135">Para representar la hora de la semana pasada, se usaría el siguiente marcado de Razor:</span><span class="sxs-lookup"><span data-stu-id="95edc-135">To render last week's time, the following Razor markup is used:</span></span>

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="95edc-136">El contenido que haya entre paréntesis `@()` se evalúa y se representa en la salida.</span><span class="sxs-lookup"><span data-stu-id="95edc-136">Any content within the `@()` parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="95edc-137">Por lo general, las expresiones implícitas (que explicamos en la sección anterior) no pueden contener espacios.</span><span class="sxs-lookup"><span data-stu-id="95edc-137">Implicit expressions, described in the previous section, generally can't contain spaces.</span></span> <span data-ttu-id="95edc-138">En el siguiente código, una semana no se resta de la hora actual:</span><span class="sxs-lookup"><span data-stu-id="95edc-138">In the following code, one week isn't subtracted from the current time:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact.cshtml?range=17)]

<span data-ttu-id="95edc-139">El código representa el siguiente HTML:</span><span class="sxs-lookup"><span data-stu-id="95edc-139">The code renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

<span data-ttu-id="95edc-140">Se pueden usar expresiones explícitas para concatenar texto con un resultado de la expresión:</span><span class="sxs-lookup"><span data-stu-id="95edc-140">Explicit expressions can be used to concatenate text with an expression result:</span></span>

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="95edc-141">Sin la expresión explícita, `<p>Age@joe.Age</p>` se trataría como una dirección de correo electrónico y se mostraría como `<p>Age@joe.Age</p>`.</span><span class="sxs-lookup"><span data-stu-id="95edc-141">Without the explicit expression, `<p>Age@joe.Age</p>` is treated as an email address, and `<p>Age@joe.Age</p>` is rendered.</span></span> <span data-ttu-id="95edc-142">Pero, si se escribe como una expresión explícita, se representa `<p>Age33</p>`.</span><span class="sxs-lookup"><span data-stu-id="95edc-142">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>

<span data-ttu-id="95edc-143">Se pueden usar expresiones explícitas para representar la salida de métodos genéricos en los archivos *.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="95edc-143">Explicit expressions can be used to render output from generic methods in *.cshtml* files.</span></span> <span data-ttu-id="95edc-144">En el siguiente marcado se muestra cómo corregir el error mostrado anteriormente provocado por el uso de corchetes en un código C# genérico.</span><span class="sxs-lookup"><span data-stu-id="95edc-144">The following markup shows how to correct the error shown earlier caused by the brackets of a C# generic.</span></span> <span data-ttu-id="95edc-145">El código se escribe como una expresión explícita:</span><span class="sxs-lookup"><span data-stu-id="95edc-145">The code is written as an explicit expression:</span></span>

```cshtml
<p>@(GenericMethod<int>())</p>
```

## <a name="expression-encoding"></a><span data-ttu-id="95edc-146">Codificación de expresiones</span><span class="sxs-lookup"><span data-stu-id="95edc-146">Expression encoding</span></span>

<span data-ttu-id="95edc-147">Las expresiones de C# que se evalúan como una cadena están codificadas en HTML.</span><span class="sxs-lookup"><span data-stu-id="95edc-147">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="95edc-148">Las expresiones de C# que se evalúan como `IHtmlContent` se representan directamente a través de `IHtmlContent.WriteTo`.</span><span class="sxs-lookup"><span data-stu-id="95edc-148">C# expressions that evaluate to `IHtmlContent` are rendered directly through `IHtmlContent.WriteTo`.</span></span> <span data-ttu-id="95edc-149">Las expresiones de C# que no se evalúan como `IHtmlContent` se convierten en una cadena por medio de `ToString` y se codifican antes de que se representen.</span><span class="sxs-lookup"><span data-stu-id="95edc-149">C# expressions that don't evaluate to `IHtmlContent` are converted to a string by `ToString` and encoded before they're rendered.</span></span>

```cshtml
@("<span>Hello World</span>")
```

<span data-ttu-id="95edc-150">El código representa el siguiente HTML:</span><span class="sxs-lookup"><span data-stu-id="95edc-150">The code renders the following HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

<span data-ttu-id="95edc-151">El HTML se muestra en el explorador como:</span><span class="sxs-lookup"><span data-stu-id="95edc-151">The HTML is shown in the browser as:</span></span>

```
<span>Hello World</span>
```

<span data-ttu-id="95edc-152">La salida de `HtmlHelper.Raw` no está codificada, pero se representa como marcado HTML.</span><span class="sxs-lookup"><span data-stu-id="95edc-152">`HtmlHelper.Raw` output isn't encoded but rendered as HTML markup.</span></span>

> [!WARNING]
> <span data-ttu-id="95edc-153">Usar `HtmlHelper.Raw` en una entrada de usuario no saneada constituye un riesgo de seguridad.</span><span class="sxs-lookup"><span data-stu-id="95edc-153">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="95edc-154">Dicha entrada podría contener código JavaScript malintencionado u otras vulnerabilidades de seguridad.</span><span class="sxs-lookup"><span data-stu-id="95edc-154">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="95edc-155">Sanear una entrada de usuario es complicado.</span><span class="sxs-lookup"><span data-stu-id="95edc-155">Sanitizing user input is difficult.</span></span> <span data-ttu-id="95edc-156">Evite usar `HtmlHelper.Raw` con entradas de usuario.</span><span class="sxs-lookup"><span data-stu-id="95edc-156">Avoid using `HtmlHelper.Raw` with user input.</span></span>

```cshtml
@Html.Raw("<span>Hello World</span>")
```

<span data-ttu-id="95edc-157">El código representa el siguiente HTML:</span><span class="sxs-lookup"><span data-stu-id="95edc-157">The code renders the following HTML:</span></span>

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a><span data-ttu-id="95edc-158">Bloques de código de Razor</span><span class="sxs-lookup"><span data-stu-id="95edc-158">Razor code blocks</span></span>

<span data-ttu-id="95edc-159">Los bloques de código de Razor comienzan por `@` y se insertan entre `{}`.</span><span class="sxs-lookup"><span data-stu-id="95edc-159">Razor code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="95edc-160">A diferencia de las expresiones, el código de C# dentro de los bloques de código no se representa.</span><span class="sxs-lookup"><span data-stu-id="95edc-160">Unlike expressions, C# code inside code blocks isn't rendered.</span></span> <span data-ttu-id="95edc-161">Las expresiones y los bloques de código de una vista comparten el mismo ámbito y se definen en orden:</span><span class="sxs-lookup"><span data-stu-id="95edc-161">Code blocks and expressions in a view share the same scope and are defined in order:</span></span>

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

<span data-ttu-id="95edc-162">El código representa el siguiente HTML:</span><span class="sxs-lookup"><span data-stu-id="95edc-162">The code renders the following HTML:</span></span>

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="95edc-163">En los bloques de código, declare las [funciones locales](/dotnet/csharp/programming-guide/classes-and-structs/local-functions) con marcado para utilizarlas como métodos en la creación de plantillas:</span><span class="sxs-lookup"><span data-stu-id="95edc-163">In code blocks, declare [local functions](/dotnet/csharp/programming-guide/classes-and-structs/local-functions) with markup to serve as templating methods:</span></span>

```cshtml
@{
    void RenderName(string name)
    {
        <p>Name: <strong>@name</strong></p>
    }

    RenderName("Mahatma Gandhi");
    RenderName("Martin Luther King, Jr.");
}
```

<span data-ttu-id="95edc-164">El código representa el siguiente HTML:</span><span class="sxs-lookup"><span data-stu-id="95edc-164">The code renders the following HTML:</span></span>

```html
<p>Name: <strong>Mahatma Gandhi</strong></p>
<p>Name: <strong>Martin Luther King, Jr.</strong></p>
```

::: moniker-end

### <a name="implicit-transitions"></a><span data-ttu-id="95edc-165">Transiciones implícitas</span><span class="sxs-lookup"><span data-stu-id="95edc-165">Implicit transitions</span></span>

<span data-ttu-id="95edc-166">El lenguaje predeterminado de un bloque de código es C#, pero la página de Razor puede volver al HTML:</span><span class="sxs-lookup"><span data-stu-id="95edc-166">The default language in a code block is C#, but the Razor Page can transition back to HTML:</span></span>

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a><span data-ttu-id="95edc-167">Transición delimitada explícita</span><span class="sxs-lookup"><span data-stu-id="95edc-167">Explicit delimited transition</span></span>

<span data-ttu-id="95edc-168">Para definir una subsección de un bloque de código que deba representar HTML, inserte los caracteres que quiera representar entre etiquetas de Razor **\<text>** :</span><span class="sxs-lookup"><span data-stu-id="95edc-168">To define a subsection of a code block that should render HTML, surround the characters for rendering with the Razor **\<text>** tag:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="95edc-169">Emplee este método para representar HTML que no esté insertado entre etiquetas HTML.</span><span class="sxs-lookup"><span data-stu-id="95edc-169">Use this approach to render HTML that isn't surrounded by an HTML tag.</span></span> <span data-ttu-id="95edc-170">Sin una etiqueta HTML o de Razor, se produce un error de tiempo de ejecución de Razor.</span><span class="sxs-lookup"><span data-stu-id="95edc-170">Without an HTML or Razor tag, a Razor runtime error occurs.</span></span>

<span data-ttu-id="95edc-171">La etiqueta **\<text>** es útil para controlar el espacio en blanco al representar el contenido:</span><span class="sxs-lookup"><span data-stu-id="95edc-171">The **\<text>** tag is useful to control whitespace when rendering content:</span></span>

* <span data-ttu-id="95edc-172">Solo se representa el contenido entre etiquetas **\<text>** .</span><span class="sxs-lookup"><span data-stu-id="95edc-172">Only the content between the **\<text>** tag is rendered.</span></span>
* <span data-ttu-id="95edc-173">En la salida HTML no hay espacios en blanco antes o después de la etiqueta **\<text>** .</span><span class="sxs-lookup"><span data-stu-id="95edc-173">No whitespace before or after the **\<text>** tag appears in the HTML output.</span></span>

### <a name="explicit-line-transition-with-"></a><span data-ttu-id="95edc-174">Transición de línea explícita con @:</span><span class="sxs-lookup"><span data-stu-id="95edc-174">Explicit Line Transition with @:</span></span>

<span data-ttu-id="95edc-175">Para representar el resto de una línea completa como HTML dentro de un bloque de código, use la sintaxis `@:`:</span><span class="sxs-lookup"><span data-stu-id="95edc-175">To render the rest of an entire line as HTML inside a code block, use the `@:` syntax:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="95edc-176">Sin el carácter `@:` en el código, se produce un error de tiempo de ejecución de Razor.</span><span class="sxs-lookup"><span data-stu-id="95edc-176">Without the `@:` in the code, a Razor runtime error is generated.</span></span>

<span data-ttu-id="95edc-177">Advertencia: Si se incluyen caracteres `@` de más en un archivo de Razor, se pueden producir errores de compilador en las instrucciones más adelante en el bloque.</span><span class="sxs-lookup"><span data-stu-id="95edc-177">Warning: Extra `@` characters in a Razor file can cause compiler errors at statements later in the block.</span></span> <span data-ttu-id="95edc-178">Estos errores de compilador pueden ser difíciles de entender porque el error real se produce antes del error notificado.</span><span class="sxs-lookup"><span data-stu-id="95edc-178">These compiler errors can be difficult to understand because the actual error occurs before the reported error.</span></span> <span data-ttu-id="95edc-179">Este error es habitual después de combinar varias expresiones implícitas/explícitas en un mismo bloque de código.</span><span class="sxs-lookup"><span data-stu-id="95edc-179">This error is common after combining multiple implicit/explicit expressions into a single code block.</span></span>

## <a name="control-structures"></a><span data-ttu-id="95edc-180">Estructuras de control</span><span class="sxs-lookup"><span data-stu-id="95edc-180">Control structures</span></span>

<span data-ttu-id="95edc-181">Las estructuras de control son una extensión de los bloques de código.</span><span class="sxs-lookup"><span data-stu-id="95edc-181">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="95edc-182">Todos los aspectos de los bloques de código (transición a marcado, C# en línea) son válidos también en las siguientes estructuras:</span><span class="sxs-lookup"><span data-stu-id="95edc-182">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures:</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="95edc-183">Los condicionales @if, else if, else y @switch</span><span class="sxs-lookup"><span data-stu-id="95edc-183">Conditionals @if, else if, else, and @switch</span></span>

<span data-ttu-id="95edc-184">`@if` controla cuándo se ejecuta el código:</span><span class="sxs-lookup"><span data-stu-id="95edc-184">`@if` controls when code runs:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

<span data-ttu-id="95edc-185">`else` y `else if` no necesitan el símbolo `@`:</span><span class="sxs-lookup"><span data-stu-id="95edc-185">`else` and `else if` don't require the `@` symbol:</span></span>

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

<span data-ttu-id="95edc-186">En el siguiente marcado se muestra cómo usar una instrucción switch:</span><span class="sxs-lookup"><span data-stu-id="95edc-186">The following markup shows how to use a switch statement:</span></span>

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

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="95edc-187">@for, @foreach, @while y @do while en bucle</span><span class="sxs-lookup"><span data-stu-id="95edc-187">Looping @for, @foreach, @while, and @do while</span></span>

<span data-ttu-id="95edc-188">El HTML con plantilla se puede representar con instrucciones de control en bucle.</span><span class="sxs-lookup"><span data-stu-id="95edc-188">Templated HTML can be rendered with looping control statements.</span></span> <span data-ttu-id="95edc-189">Para representar una lista de personas:</span><span class="sxs-lookup"><span data-stu-id="95edc-189">To render a list of people:</span></span>

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

<span data-ttu-id="95edc-190">Se permiten las siguientes instrucciones en bucle:</span><span class="sxs-lookup"><span data-stu-id="95edc-190">The following looping statements are supported:</span></span>

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

### <a name="compound-using"></a><span data-ttu-id="95edc-191">Instrucción @using compuesta</span><span class="sxs-lookup"><span data-stu-id="95edc-191">Compound @using</span></span>

<span data-ttu-id="95edc-192">En C#, las instrucciones `using` se usan para garantizar que un objeto se elimina.</span><span class="sxs-lookup"><span data-stu-id="95edc-192">In C#, a `using` statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="95edc-193">En Razor, el mismo mecanismo se emplea para crear asistentes de HTML que incluyen contenido adicional.</span><span class="sxs-lookup"><span data-stu-id="95edc-193">In Razor, the same mechanism is used to create HTML Helpers that contain additional content.</span></span> <span data-ttu-id="95edc-194">En el siguiente código, los asistentes de HTML representan una etiqueta Form con la instrucción `@using`:</span><span class="sxs-lookup"><span data-stu-id="95edc-194">In the following code, HTML Helpers render a form tag with the `@using` statement:</span></span>

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

<span data-ttu-id="95edc-195">Con los [asistentes de etiquetas](xref:mvc/views/tag-helpers/intro) se pueden realizar acciones de nivel de ámbito.</span><span class="sxs-lookup"><span data-stu-id="95edc-195">Scope-level actions can be performed with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

### <a name="try-catch-finally"></a><span data-ttu-id="95edc-196">@try, catch, finally</span><span class="sxs-lookup"><span data-stu-id="95edc-196">@try, catch, finally</span></span>

<span data-ttu-id="95edc-197">El control de excepciones es similar a C#:</span><span class="sxs-lookup"><span data-stu-id="95edc-197">Exception handling is similar to C#:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

<span data-ttu-id="95edc-198">Razor tiene la capacidad de proteger las secciones más importantes con instrucciones de bloqueo:</span><span class="sxs-lookup"><span data-stu-id="95edc-198">Razor has the capability to protect critical sections with lock statements:</span></span>

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="95edc-199">Comentarios</span><span class="sxs-lookup"><span data-stu-id="95edc-199">Comments</span></span>

<span data-ttu-id="95edc-200">Razor admite comentarios tanto de C# como HTML:</span><span class="sxs-lookup"><span data-stu-id="95edc-200">Razor supports C# and HTML comments:</span></span>

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

<span data-ttu-id="95edc-201">El código representa el siguiente HTML:</span><span class="sxs-lookup"><span data-stu-id="95edc-201">The code renders the following HTML:</span></span>

```html
<!-- HTML comment -->
```

<span data-ttu-id="95edc-202">El servidor quitará los comentarios de Razor antes de mostrar la página web.</span><span class="sxs-lookup"><span data-stu-id="95edc-202">Razor comments are removed by the server before the webpage is rendered.</span></span> <span data-ttu-id="95edc-203">Razor usa `@*  *@` para delimitar comentarios.</span><span class="sxs-lookup"><span data-stu-id="95edc-203">Razor uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="95edc-204">El siguiente código está comentado, de modo que el servidor no representa ningún marcado:</span><span class="sxs-lookup"><span data-stu-id="95edc-204">The following code is commented out, so the server doesn't render any markup:</span></span>

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a><span data-ttu-id="95edc-205">Directivas</span><span class="sxs-lookup"><span data-stu-id="95edc-205">Directives</span></span>

<span data-ttu-id="95edc-206">Las directivas de Razor se representan en las expresiones implícitas con palabras clave reservadas seguidas del símbolo `@`.</span><span class="sxs-lookup"><span data-stu-id="95edc-206">Razor directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="95edc-207">Normalmente, una directiva cambia la forma en que una vista se analiza, o bien habilita una funcionalidad diferente.</span><span class="sxs-lookup"><span data-stu-id="95edc-207">A directive typically changes the way a view is parsed or enables different functionality.</span></span>

<span data-ttu-id="95edc-208">Conocer el modo en que Razor genera el código de una vista hace que sea más fácil comprender cómo funcionan las directivas.</span><span class="sxs-lookup"><span data-stu-id="95edc-208">Understanding how Razor generates code for a view makes it easier to understand how directives work.</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact8.cshtml)]

<span data-ttu-id="95edc-209">El código genera una clase similar a la siguiente:</span><span class="sxs-lookup"><span data-stu-id="95edc-209">The code generates a class similar to the following:</span></span>

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

<span data-ttu-id="95edc-210">Más adelante en este artículo, en la sección [Inspección de la clase C# de Razor generada por una vista](#inspect-the-razor-c-class-generated-for-a-view), se explica cómo ver esta clase generada.</span><span class="sxs-lookup"><span data-stu-id="95edc-210">Later in this article, the section [Inspect the Razor C# class generated for a view](#inspect-the-razor-c-class-generated-for-a-view) explains how to view this generated class.</span></span>

<a name="using"></a>

### <a name="using"></a>@using

<span data-ttu-id="95edc-211">La directiva `@using` agrega la directiva `using` de C# a la vista generada:</span><span class="sxs-lookup"><span data-stu-id="95edc-211">The `@using` directive adds the C# `using` directive to the generated view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

<span data-ttu-id="95edc-212">La directiva `@model` especifica el tipo del modelo que se pasa a una vista:</span><span class="sxs-lookup"><span data-stu-id="95edc-212">The `@model` directive specifies the type of the model passed to a view:</span></span>

```cshtml
@model TypeNameOfModel
```

<span data-ttu-id="95edc-213">En una aplicación ASP.NET Core MVC creada con cuentas de usuario individuales, la vista *Views/Account/Login.cshtml* contiene la siguiente declaración de modelo:</span><span class="sxs-lookup"><span data-stu-id="95edc-213">In an ASP.NET Core MVC app created with individual user accounts, the *Views/Account/Login.cshtml* view contains the following model declaration:</span></span>

```cshtml
@model LoginViewModel
```

<span data-ttu-id="95edc-214">La clase generada se hereda de `RazorPage<dynamic>`:</span><span class="sxs-lookup"><span data-stu-id="95edc-214">The class generated inherits from `RazorPage<dynamic>`:</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

<span data-ttu-id="95edc-215">Razor expone una propiedad `Model` para tener acceso al modelo que se ha pasado a la vista:</span><span class="sxs-lookup"><span data-stu-id="95edc-215">Razor exposes a `Model` property for accessing the model passed to the view:</span></span>

```cshtml
<div>The Login Email: @Model.Email</div>
```

<span data-ttu-id="95edc-216">La directiva `@model` especifica el tipo de esta propiedad.</span><span class="sxs-lookup"><span data-stu-id="95edc-216">The `@model` directive specifies the type of this property.</span></span> <span data-ttu-id="95edc-217">La directiva especifica el elemento `T` en `RazorPage<T>` de la clase generada de la que se deriva la vista.</span><span class="sxs-lookup"><span data-stu-id="95edc-217">The directive specifies the `T` in `RazorPage<T>` that the generated class that the view derives from.</span></span> <span data-ttu-id="95edc-218">Si la directiva `@model` no se especifica, la propiedad `Model` es de tipo `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="95edc-218">If the `@model` directive isn't specified, the `Model` property is of type `dynamic`.</span></span> <span data-ttu-id="95edc-219">El valor del modelo se pasa del controlador a la vista.</span><span class="sxs-lookup"><span data-stu-id="95edc-219">The value of the model is passed from the controller to the view.</span></span> <span data-ttu-id="95edc-220">Para más información, vea [Modelos fuertemente tipados y la palabra clave &commat;model](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-and-the--keyword).</span><span class="sxs-lookup"><span data-stu-id="95edc-220">For more information, see [Strongly typed models and the &commat;model keyword](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-and-the--keyword).</span></span>

### <a name="inherits"></a>@inherits

<span data-ttu-id="95edc-221">La directiva `@inherits` proporciona control total sobre la clase que la vista hereda:</span><span class="sxs-lookup"><span data-stu-id="95edc-221">The `@inherits` directive provides full control of the class the view inherits:</span></span>

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

<span data-ttu-id="95edc-222">El siguiente código es un tipo personalizado de página de Razor:</span><span class="sxs-lookup"><span data-stu-id="95edc-222">The following code is a custom Razor page type:</span></span>

[!code-csharp[](razor/sample/Classes/CustomRazorPage.cs)]

<span data-ttu-id="95edc-223">`CustomText` se muestra en una vista:</span><span class="sxs-lookup"><span data-stu-id="95edc-223">The `CustomText` is displayed in a view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact10.cshtml)]

<span data-ttu-id="95edc-224">El código representa el siguiente HTML:</span><span class="sxs-lookup"><span data-stu-id="95edc-224">The code renders the following HTML:</span></span>

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

 <span data-ttu-id="95edc-225">`@model` y `@inherits` se pueden usar en la misma vista.</span><span class="sxs-lookup"><span data-stu-id="95edc-225">`@model` and `@inherits` can be used in the same view.</span></span> <span data-ttu-id="95edc-226">`@inherits` puede estar en un archivo *_ViewImports.cshtml* que la vista importa:</span><span class="sxs-lookup"><span data-stu-id="95edc-226">`@inherits` can be in a *_ViewImports.cshtml* file that the view imports:</span></span>

[!code-cshtml[](razor/sample/Views/_ViewImportsModel.cshtml)]

<span data-ttu-id="95edc-227">El siguiente código es un ejemplo de una vista fuertemente tipada:</span><span class="sxs-lookup"><span data-stu-id="95edc-227">The following code is an example of a strongly-typed view:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Login1.cshtml)]

<span data-ttu-id="95edc-228">Si "rick@contoso.com" se pasa en el modelo, la vista genera el siguiente marcado HTML:</span><span class="sxs-lookup"><span data-stu-id="95edc-228">If "rick@contoso.com" is passed in the model, the view generates the following HTML markup:</span></span>

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject

<span data-ttu-id="95edc-229">La directiva `@inject` permite a la página de Razor insertar un servicio del [contenedor de servicios](xref:fundamentals/dependency-injection) en una vista.</span><span class="sxs-lookup"><span data-stu-id="95edc-229">The `@inject` directive enables the Razor Page to inject a service from the [service container](xref:fundamentals/dependency-injection) into a view.</span></span> <span data-ttu-id="95edc-230">Para más información, vea [Dependency injection into views](xref:mvc/views/dependency-injection) (Inserción de dependencias en vistas).</span><span class="sxs-lookup"><span data-stu-id="95edc-230">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

### <a name="functions"></a>@functions

<span data-ttu-id="95edc-231">La directiva `@functions` permite que una página de Razor agregue un bloque de código de C# a una vista:</span><span class="sxs-lookup"><span data-stu-id="95edc-231">The `@functions` directive enables a Razor Page to add a C# code block to a view:</span></span>

```cshtml
@functions { // C# Code }
```

<span data-ttu-id="95edc-232">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="95edc-232">For example:</span></span>

[!code-cshtml[](razor/sample/Views/Home/Contact6.cshtml)]

<span data-ttu-id="95edc-233">El código genera el siguiente marcado HTML:</span><span class="sxs-lookup"><span data-stu-id="95edc-233">The code generates the following HTML markup:</span></span>

```html
<div>From method: Hello</div>
```

<span data-ttu-id="95edc-234">El siguiente código es la clase C# de Razor generada:</span><span class="sxs-lookup"><span data-stu-id="95edc-234">The following code is the generated Razor C# class:</span></span>

[!code-csharp[](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="95edc-235">Los métodos `@functions` se pueden usar para la creación de plantillas si están marcados:</span><span class="sxs-lookup"><span data-stu-id="95edc-235">`@functions` methods serve as templating methods when they have markup:</span></span>

```cshtml
@{
    RenderName("Mahatma Gandhi");
    RenderName("Martin Luther King, Jr.");
}

@functions {
    private void RenderName(string name)
    {
        <p>Name: <strong>@name</strong></p>
    }
}
```

<span data-ttu-id="95edc-236">El código representa el siguiente HTML:</span><span class="sxs-lookup"><span data-stu-id="95edc-236">The code renders the following HTML:</span></span>

```html
<p>Name: <strong>Mahatma Gandhi</strong></p>
<p>Name: <strong>Martin Luther King, Jr.</strong></p>
```

::: moniker-end

### <a name="attribute"></a>@attribute

<span data-ttu-id="95edc-237">La directiva `@attribute` agrega el atributo especificado a la clase de la página o vista generada.</span><span class="sxs-lookup"><span data-stu-id="95edc-237">The `@attribute` directive adds the given attribute to the class of the generated page or view.</span></span> <span data-ttu-id="95edc-238">En el ejemplo siguiente se agrega el atributo `[Authorize]`:</span><span class="sxs-lookup"><span data-stu-id="95edc-238">The following example adds the `[Authorize]` attribute:</span></span>

```cshtml
@attribute [Authorize]
```

### <a name="namespace"></a>@namespace

<span data-ttu-id="95edc-239">La directiva `@namespace` establece el espacio de nombres de la clase de la página o vista generada:</span><span class="sxs-lookup"><span data-stu-id="95edc-239">The `@namespace` directive sets the namespace of the class of the generated page or view:</span></span>

```cshtml
@namespace Your.Namespace.Here
```

<span data-ttu-id="95edc-240">Si una página o vista importa API con una directiva `@namespace`, el espacio de nombres del archivo original se establece en relación con ese espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="95edc-240">If a page or view imports API with an `@namespace` directive, the original file's namespace is set relative to that namespace.</span></span> 

<span data-ttu-id="95edc-241">Si *MyApp/Pages/\_ViewImports.cshtml* contiene `@namespace Hello.World`, el espacio de nombres de las páginas o vistas que importan el espacio de nombres `Hello.World` se establece como se muestra en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="95edc-241">If *MyApp/Pages/\_ViewImports.cshtml* contains `@namespace Hello.World`, the namespace of pages or views that import the `Hello.World` namespace is set as shown in the following table.</span></span>

| <span data-ttu-id="95edc-242">Página (o vista)</span><span class="sxs-lookup"><span data-stu-id="95edc-242">Page (or view)</span></span>                     | <span data-ttu-id="95edc-243">Espacio de nombres</span><span class="sxs-lookup"><span data-stu-id="95edc-243">Namespace</span></span>               |
| ---------------------------------- | ----------------------- |
| <span data-ttu-id="95edc-244">*MyApp/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="95edc-244">*MyApp/Pages/Index.cshtml*</span></span>         | `Hello.World`           |
| <span data-ttu-id="95edc-245">*MyApp/Pages/MorePages/Bar.cshtml*</span><span class="sxs-lookup"><span data-stu-id="95edc-245">*MyApp/Pages/MorePages/Bar.cshtml*</span></span> | `Hello.World.MorePages` |

<span data-ttu-id="95edc-246">Si varios archivos de importación tienen la directiva `@namespace`, se usa el archivo más cercano a la página o vista en la cadena del directorio.</span><span class="sxs-lookup"><span data-stu-id="95edc-246">If multiple import files have the `@namespace` directive, the file closest to the page or view in the directory chain is used.</span></span>

### <a name="section"></a>@section

<span data-ttu-id="95edc-247">La directiva `@section` se usa junto con el [diseño](xref:mvc/views/layout) para permitir que las páginas o vistas representen el contenido en otras partes de la página HTML.</span><span class="sxs-lookup"><span data-stu-id="95edc-247">The `@section` directive is used in conjunction with the [layout](xref:mvc/views/layout) to enable pages or views to render content in different parts of the HTML page.</span></span> <span data-ttu-id="95edc-248">Para más información, vea [Sections](xref:mvc/views/layout#layout-sections-label) (Secciones).</span><span class="sxs-lookup"><span data-stu-id="95edc-248">For more information, see [Sections](xref:mvc/views/layout#layout-sections-label).</span></span>

## <a name="templated-razor-delegates"></a><span data-ttu-id="95edc-249">Delegados con plantillas de Razor</span><span class="sxs-lookup"><span data-stu-id="95edc-249">Templated Razor delegates</span></span>

<span data-ttu-id="95edc-250">Las plantillas de Razor permiten definir un fragmento de la interfaz de usuario con el formato siguiente:</span><span class="sxs-lookup"><span data-stu-id="95edc-250">Razor templates allow you to define a UI snippet with the following format:</span></span>

```cshtml
@<tag>...</tag>
```

<span data-ttu-id="95edc-251">En el ejemplo siguiente se muestra cómo especificar un delegado de Razor con plantilla como elemento <xref:System.Func%602>.</span><span class="sxs-lookup"><span data-stu-id="95edc-251">The following example illustrates how to specify a templated Razor delegate as a <xref:System.Func%602>.</span></span> <span data-ttu-id="95edc-252">El [tipo dinámico](/dotnet/csharp/programming-guide/types/using-type-dynamic) se especifica para el parámetro del método encapsulado por el delegado.</span><span class="sxs-lookup"><span data-stu-id="95edc-252">The [dynamic type](/dotnet/csharp/programming-guide/types/using-type-dynamic) is specified for the parameter of the method that the delegate encapsulates.</span></span> <span data-ttu-id="95edc-253">Se especifica un [tipo de objeto](/dotnet/csharp/language-reference/keywords/object) como el valor devuelto del delegado.</span><span class="sxs-lookup"><span data-stu-id="95edc-253">An [object type](/dotnet/csharp/language-reference/keywords/object) is specified as the return value of the delegate.</span></span> <span data-ttu-id="95edc-254">La plantilla se usa con un elemento <xref:System.Collections.Generic.List%601> de `Pet` que tiene una propiedad `Name`.</span><span class="sxs-lookup"><span data-stu-id="95edc-254">The template is used with a <xref:System.Collections.Generic.List%601> of `Pet` that has a `Name` property.</span></span>

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

<span data-ttu-id="95edc-255">La plantilla se representa con el elemento `pets` proporcionado por una instrucción `foreach`:</span><span class="sxs-lookup"><span data-stu-id="95edc-255">The template is rendered with `pets` supplied by a `foreach` statement:</span></span>

```cshtml
@foreach (var pet in pets)
{
    @petTemplate(pet)
}
```

<span data-ttu-id="95edc-256">Salida representada:</span><span class="sxs-lookup"><span data-stu-id="95edc-256">Rendered output:</span></span>

```html
<p>You have a pet named <strong>Rin Tin Tin</strong>.</p>
<p>You have a pet named <strong>Mr. Bigglesworth</strong>.</p>
<p>You have a pet named <strong>K-9</strong>.</p>
```

<span data-ttu-id="95edc-257">También se puede proporcionar una plantilla de Razor insertada como un argumento para un método.</span><span class="sxs-lookup"><span data-stu-id="95edc-257">You can also supply an inline Razor template as an argument to a method.</span></span> <span data-ttu-id="95edc-258">En el ejemplo siguiente, el método `Repeat` recibe una plantilla de Razor.</span><span class="sxs-lookup"><span data-stu-id="95edc-258">In the following example, the `Repeat` method receives a Razor template.</span></span> <span data-ttu-id="95edc-259">El método usa la plantilla para generar contenido HTML con repeticiones de elementos proporcionados a partir de una lista:</span><span class="sxs-lookup"><span data-stu-id="95edc-259">The method uses the template to produce HTML content with repeats of items supplied from a list:</span></span>

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

<span data-ttu-id="95edc-260">Con la lista de mascotas del ejemplo anterior, se llama al método `Repeat` con:</span><span class="sxs-lookup"><span data-stu-id="95edc-260">Using the list of pets from the prior example, the `Repeat` method is called with:</span></span>

* <span data-ttu-id="95edc-261"><xref:System.Collections.Generic.List%601> de `Pet`.</span><span class="sxs-lookup"><span data-stu-id="95edc-261"><xref:System.Collections.Generic.List%601> of `Pet`.</span></span>
* <span data-ttu-id="95edc-262">Número de veces que se repite cada mascota.</span><span class="sxs-lookup"><span data-stu-id="95edc-262">Number of times to repeat each pet.</span></span>
* <span data-ttu-id="95edc-263">Plantilla insertada que se va a usar para los elementos de una lista sin ordenar.</span><span class="sxs-lookup"><span data-stu-id="95edc-263">Inline template to use for the list items of an unordered list.</span></span>

```cshtml
<ul>
    @Repeat(pets, 3, @<li>@item.Name</li>)
</ul>
```

<span data-ttu-id="95edc-264">Salida representada:</span><span class="sxs-lookup"><span data-stu-id="95edc-264">Rendered output:</span></span>

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

## <a name="tag-helpers"></a><span data-ttu-id="95edc-265">Asistentes de etiquetas</span><span class="sxs-lookup"><span data-stu-id="95edc-265">Tag Helpers</span></span>

<span data-ttu-id="95edc-266">Hay tres directivas que pertenecen a los [asistentes de etiquetas](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="95edc-266">There are three directives that pertain to [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

| <span data-ttu-id="95edc-267">Directiva</span><span class="sxs-lookup"><span data-stu-id="95edc-267">Directive</span></span> | <span data-ttu-id="95edc-268">Función</span><span class="sxs-lookup"><span data-stu-id="95edc-268">Function</span></span> |
| --------- | -------- |
| [<span data-ttu-id="95edc-269">&commat;addTagHelper</span><span class="sxs-lookup"><span data-stu-id="95edc-269">&commat;addTagHelper</span></span>](xref:mvc/views/tag-helpers/intro#add-helper-label) | <span data-ttu-id="95edc-270">Pone los asistentes de etiquetas a disposición de una vista.</span><span class="sxs-lookup"><span data-stu-id="95edc-270">Makes Tag Helpers available to a view.</span></span> |
| [<span data-ttu-id="95edc-271">&commat;removeTagHelper</span><span class="sxs-lookup"><span data-stu-id="95edc-271">&commat;removeTagHelper</span></span>](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | <span data-ttu-id="95edc-272">Quita los asistentes de etiquetas agregadas anteriormente desde una vista.</span><span class="sxs-lookup"><span data-stu-id="95edc-272">Removes Tag Helpers previously added from a view.</span></span> |
| [<span data-ttu-id="95edc-273">&commat;tagHelperPrefix</span><span class="sxs-lookup"><span data-stu-id="95edc-273">&commat;tagHelperPrefix</span></span>](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | <span data-ttu-id="95edc-274">Especifica una cadena de prefijo de etiqueta para permitir la compatibilidad con el asistente de etiquetas y hacer explícito su uso.</span><span class="sxs-lookup"><span data-stu-id="95edc-274">Specifies a tag prefix to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> |

## <a name="razor-reserved-keywords"></a><span data-ttu-id="95edc-275">Palabras clave reservadas de Razor</span><span class="sxs-lookup"><span data-stu-id="95edc-275">Razor reserved keywords</span></span>

### <a name="razor-keywords"></a><span data-ttu-id="95edc-276">Palabras clave de Razor</span><span class="sxs-lookup"><span data-stu-id="95edc-276">Razor keywords</span></span>

* <span data-ttu-id="95edc-277">page (requiere ASP.NET Core 2.0 y versiones posteriores)</span><span class="sxs-lookup"><span data-stu-id="95edc-277">page (Requires ASP.NET Core 2.0 and later)</span></span>
* <span data-ttu-id="95edc-278">namespace</span><span class="sxs-lookup"><span data-stu-id="95edc-278">namespace</span></span>
* <span data-ttu-id="95edc-279">funciones</span><span class="sxs-lookup"><span data-stu-id="95edc-279">functions</span></span>
* <span data-ttu-id="95edc-280">hereda</span><span class="sxs-lookup"><span data-stu-id="95edc-280">inherits</span></span>
* <span data-ttu-id="95edc-281">modelo</span><span class="sxs-lookup"><span data-stu-id="95edc-281">model</span></span>
* <span data-ttu-id="95edc-282">section</span><span class="sxs-lookup"><span data-stu-id="95edc-282">section</span></span>
* <span data-ttu-id="95edc-283">helper (no admitida en ASP.NET Core actualmente)</span><span class="sxs-lookup"><span data-stu-id="95edc-283">helper (Not currently supported by ASP.NET Core)</span></span>

<span data-ttu-id="95edc-284">Para hacer escape en una palabra clave de Razor, se usa `@(Razor Keyword)` (por ejemplo, `@(functions)`).</span><span class="sxs-lookup"><span data-stu-id="95edc-284">Razor keywords are escaped with `@(Razor Keyword)` (for example, `@(functions)`).</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="95edc-285">Palabras clave C# de Razor</span><span class="sxs-lookup"><span data-stu-id="95edc-285">C# Razor keywords</span></span>

* <span data-ttu-id="95edc-286">mayúsculas y minúsculas</span><span class="sxs-lookup"><span data-stu-id="95edc-286">case</span></span>
* <span data-ttu-id="95edc-287">do</span><span class="sxs-lookup"><span data-stu-id="95edc-287">do</span></span>
* <span data-ttu-id="95edc-288">default</span><span class="sxs-lookup"><span data-stu-id="95edc-288">default</span></span>
* <span data-ttu-id="95edc-289">for</span><span class="sxs-lookup"><span data-stu-id="95edc-289">for</span></span>
* <span data-ttu-id="95edc-290">foreach</span><span class="sxs-lookup"><span data-stu-id="95edc-290">foreach</span></span>
* <span data-ttu-id="95edc-291">if</span><span class="sxs-lookup"><span data-stu-id="95edc-291">if</span></span>
* <span data-ttu-id="95edc-292">else</span><span class="sxs-lookup"><span data-stu-id="95edc-292">else</span></span>
* <span data-ttu-id="95edc-293">bloquear</span><span class="sxs-lookup"><span data-stu-id="95edc-293">lock</span></span>
* <span data-ttu-id="95edc-294">switch</span><span class="sxs-lookup"><span data-stu-id="95edc-294">switch</span></span>
* <span data-ttu-id="95edc-295">try</span><span class="sxs-lookup"><span data-stu-id="95edc-295">try</span></span>
* <span data-ttu-id="95edc-296">catch</span><span class="sxs-lookup"><span data-stu-id="95edc-296">catch</span></span>
* <span data-ttu-id="95edc-297">finally</span><span class="sxs-lookup"><span data-stu-id="95edc-297">finally</span></span>
* <span data-ttu-id="95edc-298">utilizar</span><span class="sxs-lookup"><span data-stu-id="95edc-298">using</span></span>
* <span data-ttu-id="95edc-299">while</span><span class="sxs-lookup"><span data-stu-id="95edc-299">while</span></span>

<span data-ttu-id="95edc-300">Las palabras clave C# de Razor deben tener doble escape con `@(@C# Razor Keyword)` (por ejemplo, `@(@case)`).</span><span class="sxs-lookup"><span data-stu-id="95edc-300">C# Razor keywords must be double-escaped with `@(@C# Razor Keyword)` (for example, `@(@case)`).</span></span> <span data-ttu-id="95edc-301">El primer carácter `@` hace escape en el analizador Razor</span><span class="sxs-lookup"><span data-stu-id="95edc-301">The first `@` escapes the Razor parser.</span></span> <span data-ttu-id="95edc-302">y el segundo `@`, en el analizador de C#.</span><span class="sxs-lookup"><span data-stu-id="95edc-302">The second `@` escapes the C# parser.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="95edc-303">Palabras clave reservadas no usadas en Razor</span><span class="sxs-lookup"><span data-stu-id="95edc-303">Reserved keywords not used by Razor</span></span>

* <span data-ttu-id="95edc-304">clase</span><span class="sxs-lookup"><span data-stu-id="95edc-304">class</span></span>

## <a name="inspect-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="95edc-305">Inspección de la clase C# de Razor generada por una vista</span><span class="sxs-lookup"><span data-stu-id="95edc-305">Inspect the Razor C# class generated for a view</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="95edc-306">Con el SDK de .NET Core 2.1 o posterior, el [SDK de Razor](xref:razor-pages/sdk) controla la compilación de los archivos de Razor.</span><span class="sxs-lookup"><span data-stu-id="95edc-306">With .NET Core SDK 2.1 or later, the [Razor SDK](xref:razor-pages/sdk) handles compilation of Razor files.</span></span> <span data-ttu-id="95edc-307">Al compilar un proyecto, el SDK de Razor genera un directorio *obj/<configuración_de_compilación>/<moniker_de_la_plataforma_de_destino>/Razor* en la raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="95edc-307">When building a project, the Razor SDK generates an *obj/<build_configuration>/<target_framework_moniker>/Razor* directory in the project root.</span></span> <span data-ttu-id="95edc-308">La estructura de directorios dentro del directorio *Razor* refleja la del proyecto.</span><span class="sxs-lookup"><span data-stu-id="95edc-308">The directory structure within the *Razor* directory mirrors the project's directory structure.</span></span>

<span data-ttu-id="95edc-309">Tenga en cuenta la estructura de directorios siguiente en un proyecto de Razor Pages de ASP.NET Core 2.1 destinado a .NET Core 2.1:</span><span class="sxs-lookup"><span data-stu-id="95edc-309">Consider the following directory structure in an ASP.NET Core 2.1 Razor Pages project targeting .NET Core 2.1:</span></span>

* <span data-ttu-id="95edc-310">**Areas/**</span><span class="sxs-lookup"><span data-stu-id="95edc-310">**Areas/**</span></span>
  * <span data-ttu-id="95edc-311">**Admin/**</span><span class="sxs-lookup"><span data-stu-id="95edc-311">**Admin/**</span></span>
    * <span data-ttu-id="95edc-312">**Pages/**</span><span class="sxs-lookup"><span data-stu-id="95edc-312">**Pages/**</span></span>
      * <span data-ttu-id="95edc-313">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="95edc-313">*Index.cshtml*</span></span>
      * <span data-ttu-id="95edc-314">*Index.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="95edc-314">*Index.cshtml.cs*</span></span>
* <span data-ttu-id="95edc-315">**Pages/**</span><span class="sxs-lookup"><span data-stu-id="95edc-315">**Pages/**</span></span>
  * <span data-ttu-id="95edc-316">**Shared/**</span><span class="sxs-lookup"><span data-stu-id="95edc-316">**Shared/**</span></span>
    * <span data-ttu-id="95edc-317">*_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="95edc-317">*_Layout.cshtml*</span></span>
  * <span data-ttu-id="95edc-318">*_ViewImports.cshtml*</span><span class="sxs-lookup"><span data-stu-id="95edc-318">*_ViewImports.cshtml*</span></span>
  * <span data-ttu-id="95edc-319">*_ViewStart.cshtml*</span><span class="sxs-lookup"><span data-stu-id="95edc-319">*_ViewStart.cshtml*</span></span>
  * <span data-ttu-id="95edc-320">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="95edc-320">*Index.cshtml*</span></span>
  * <span data-ttu-id="95edc-321">*Index.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="95edc-321">*Index.cshtml.cs*</span></span>

<span data-ttu-id="95edc-322">Al compilar el proyecto en la configuración *Depurar* se crea el directorio *obj* siguiente:</span><span class="sxs-lookup"><span data-stu-id="95edc-322">Building the project in *Debug* configuration yields the following *obj* directory:</span></span>

* <span data-ttu-id="95edc-323">**obj/**</span><span class="sxs-lookup"><span data-stu-id="95edc-323">**obj/**</span></span>
  * <span data-ttu-id="95edc-324">**Debug/**</span><span class="sxs-lookup"><span data-stu-id="95edc-324">**Debug/**</span></span>
    * <span data-ttu-id="95edc-325">**netcoreapp2.1/**</span><span class="sxs-lookup"><span data-stu-id="95edc-325">**netcoreapp2.1/**</span></span>
      * <span data-ttu-id="95edc-326">**Razor/**</span><span class="sxs-lookup"><span data-stu-id="95edc-326">**Razor/**</span></span>
        * <span data-ttu-id="95edc-327">**Areas/**</span><span class="sxs-lookup"><span data-stu-id="95edc-327">**Areas/**</span></span>
          * <span data-ttu-id="95edc-328">**Admin/**</span><span class="sxs-lookup"><span data-stu-id="95edc-328">**Admin/**</span></span>
            * <span data-ttu-id="95edc-329">**Pages/**</span><span class="sxs-lookup"><span data-stu-id="95edc-329">**Pages/**</span></span>
              * <span data-ttu-id="95edc-330">*Index.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="95edc-330">*Index.g.cshtml.cs*</span></span>
        * <span data-ttu-id="95edc-331">**Pages/**</span><span class="sxs-lookup"><span data-stu-id="95edc-331">**Pages/**</span></span>
          * <span data-ttu-id="95edc-332">**Shared/**</span><span class="sxs-lookup"><span data-stu-id="95edc-332">**Shared/**</span></span>
            * <span data-ttu-id="95edc-333">*_Layout.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="95edc-333">*_Layout.g.cshtml.cs*</span></span>
          * <span data-ttu-id="95edc-334">*_ViewImports.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="95edc-334">*_ViewImports.g.cshtml.cs*</span></span>
          * <span data-ttu-id="95edc-335">*_ViewStart.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="95edc-335">*_ViewStart.g.cshtml.cs*</span></span>
          * <span data-ttu-id="95edc-336">*Index.g.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="95edc-336">*Index.g.cshtml.cs*</span></span>

<span data-ttu-id="95edc-337">Para ver la clase generada para *Pages/Index.cshtml*, abra *obj/Debug/netcoreapp2.1/Razor/Pages/Index.g.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="95edc-337">To view the generated class for *Pages/Index.cshtml*, open *obj/Debug/netcoreapp2.1/Razor/Pages/Index.g.cshtml.cs*.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="95edc-338">Agregue la siguiente clase al proyecto de ASP.NET Core MVC:</span><span class="sxs-lookup"><span data-stu-id="95edc-338">Add the following class to the ASP.NET Core MVC project:</span></span>

[!code-csharp[](razor/sample/Utilities/CustomTemplateEngine.cs)]

<span data-ttu-id="95edc-339">En `Startup.ConfigureServices`, invalide el elemento `RazorTemplateEngine` agregado por MVC con la clase `CustomTemplateEngine`:</span><span class="sxs-lookup"><span data-stu-id="95edc-339">In `Startup.ConfigureServices`, override the `RazorTemplateEngine` added by MVC with the `CustomTemplateEngine` class:</span></span>

[!code-csharp[](razor/sample/Startup.cs?highlight=4&range=10-14)]

<span data-ttu-id="95edc-340">Establezca un punto de interrupción en la instrucción `return csharpDocument;` de `CustomTemplateEngine`.</span><span class="sxs-lookup"><span data-stu-id="95edc-340">Set a breakpoint on the `return csharpDocument;` statement of `CustomTemplateEngine`.</span></span> <span data-ttu-id="95edc-341">Cuando la ejecución del programa se detenga en el punto de interrupción, vea el valor de `generatedCode`.</span><span class="sxs-lookup"><span data-stu-id="95edc-341">When program execution stops at the breakpoint, view the value of `generatedCode`.</span></span>

![Vista del visualizador de texto de generatedCode](razor/_static/tvr.png)

::: moniker-end

## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="95edc-343">Búsquedas de vistas y distinción entre mayúsculas y minúsculas</span><span class="sxs-lookup"><span data-stu-id="95edc-343">View lookups and case sensitivity</span></span>

<span data-ttu-id="95edc-344">El motor de vista de Razor realiza búsquedas de vistas en las que se distingue entre mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="95edc-344">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="95edc-345">Pero la búsqueda real viene determinada por el sistema de archivos subyacente:</span><span class="sxs-lookup"><span data-stu-id="95edc-345">However, the actual lookup is determined by the underlying file system:</span></span>

* <span data-ttu-id="95edc-346">Origen basado en archivos:</span><span class="sxs-lookup"><span data-stu-id="95edc-346">File based source:</span></span>
  * <span data-ttu-id="95edc-347">En los sistemas operativos con sistemas de archivos que no distinguen entre mayúsculas y minúsculas (por ejemplo, Windows), las búsquedas de proveedor de archivos físicos no distinguirán mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="95edc-347">On operating systems with case insensitive file systems (for example, Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="95edc-348">Por ejemplo, `return View("Test")` arrojará como resultados */Views/Home/Test.cshtml*, */Views/home/test.cshtml* y cualquier otra variante de mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="95edc-348">For example, `return View("Test")` results in matches for */Views/Home/Test.cshtml*, */Views/home/test.cshtml*, and any other casing variant.</span></span>
  * <span data-ttu-id="95edc-349">En los sistemas de archivos en los que sí se distingue entre mayúsculas y minúsculas (por ejemplo, Linux, OSX y al usar `EmbeddedFileProvider`), las búsquedas distinguirán mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="95edc-349">On case-sensitive file systems (for example, Linux, OSX, and with `EmbeddedFileProvider`), lookups are case-sensitive.</span></span> <span data-ttu-id="95edc-350">Por ejemplo, `return View("Test")` mostrará el resultado */Views/Home/Test.cshtml* única y exclusivamente.</span><span class="sxs-lookup"><span data-stu-id="95edc-350">For example, `return View("Test")` specifically matches */Views/Home/Test.cshtml*.</span></span>
* <span data-ttu-id="95edc-351">Vistas precompiladas: En ASP.NET Core 2.0 y versiones posteriores, las búsquedas de vistas precompiladas no distinguen mayúsculas de minúsculas en todos los sistemas operativos.</span><span class="sxs-lookup"><span data-stu-id="95edc-351">Precompiled views: With ASP.NET Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="95edc-352">Este comportamiento es idéntico al comportamiento del proveedor de archivos físicos en Windows.</span><span class="sxs-lookup"><span data-stu-id="95edc-352">The behavior is identical to physical file provider's behavior on Windows.</span></span> <span data-ttu-id="95edc-353">Si dos vistas precompiladas difieren solo por sus mayúsculas o minúsculas, el resultado de la búsqueda será no determinante.</span><span class="sxs-lookup"><span data-stu-id="95edc-353">If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="95edc-354">Por tanto, se anima a todos los desarrolladores a intentar que las mayúsculas y minúsculas de los nombres de archivo y de directorio sean las mismas que las mayúsculas y minúsculas de:</span><span class="sxs-lookup"><span data-stu-id="95edc-354">Developers are encouraged to match the casing of file and directory names to the casing of:</span></span>

* <span data-ttu-id="95edc-355">Nombres de acciones, controladores y áreas.</span><span class="sxs-lookup"><span data-stu-id="95edc-355">Area, controller, and action names.</span></span>
* <span data-ttu-id="95edc-356">Páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="95edc-356">Razor Pages.</span></span>

<span data-ttu-id="95edc-357">La coincidencia de mayúsculas y minúsculas garantiza que las implementaciones van a encontrar sus vistas, independientemente de cuál sea el sistema de archivos subyacente.</span><span class="sxs-lookup"><span data-stu-id="95edc-357">Matching case ensures the deployments find their views regardless of the underlying file system.</span></span>
