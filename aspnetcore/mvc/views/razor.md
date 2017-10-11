---
title: Referencia de la sintaxis de Razor para ASP.NET Core
author: guardrex
description: "Obtenga información acerca de la sintaxis de marcado de Razor para incrustar código basado en servidor en las páginas Web."
keywords: Directivas de ASP.NET Core, Razor, Razor
ms.author: riande
manager: wpickett
ms.date: 09/29/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/razor
ms.openlocfilehash: 532e278597a0029b5bae93068af5b7b147c35688
ms.sourcegitcommit: e45f8912ce32b4071bf7e83b8f8315cd8bba3520
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2017
---
# <a name="razor-syntax-for-aspnet-core"></a><span data-ttu-id="8f1fe-104">Sintaxis de Razor para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8f1fe-104">Razor syntax for ASP.NET Core</span></span>

<span data-ttu-id="8f1fe-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), y [Taylor Mullen](https://twitter.com/ntaylormullen)</span><span class="sxs-lookup"><span data-stu-id="8f1fe-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), and [Taylor Mullen](https://twitter.com/ntaylormullen)</span></span>

<span data-ttu-id="8f1fe-106">Razor es una sintaxis de marcado para incrustar código basado en servidor en las páginas Web.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-106">Razor is a markup syntax for embedding server-based code into webpages.</span></span> <span data-ttu-id="8f1fe-107">La sintaxis de Razor consta de Razor marcado, C# y HTML.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-107">The Razor syntax consists of Razor markup, C#, and HTML.</span></span> <span data-ttu-id="8f1fe-108">Archivos que contienen Razor generalmente tienen un *.cshtml* la extensión de archivo.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-108">Files containing Razor generally have a *.cshtml* file extension.</span></span>

## <a name="rendering-html"></a><span data-ttu-id="8f1fe-109">Representación HTML</span><span class="sxs-lookup"><span data-stu-id="8f1fe-109">Rendering HTML</span></span>

<span data-ttu-id="8f1fe-110">El idioma de Razor predeterminado es HTML.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-110">The default Razor language is HTML.</span></span> <span data-ttu-id="8f1fe-111">Representación HTML desde el marcado de Razor es similar a representar HTML desde un archivo HTML.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-111">Rendering HTML from Razor markup is no different than rendering HTML from an HTML file.</span></span> <span data-ttu-id="8f1fe-112">Si coloca el marcado HTML en una *.cshtml* archivo Razor, se representa el servidor sin cambios.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-112">If you place HTML markup into a *.cshtml* Razor file, it's rendered by the server unchanged.</span></span>

## <a name="razor-syntax"></a><span data-ttu-id="8f1fe-113">Sintaxis de Razor</span><span class="sxs-lookup"><span data-stu-id="8f1fe-113">Razor syntax</span></span>

<span data-ttu-id="8f1fe-114">Razor es compatible con C# y usa el `@` símbolo para realizar la transición de HTML para C#.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-114">Razor supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> <span data-ttu-id="8f1fe-115">Razor evalúa las expresiones de C# y los representa en la salida HTML.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-115">Razor evaluates C# expressions and renders them in the HTML output.</span></span>

<span data-ttu-id="8f1fe-116">Cuando un `@` va seguido de símbolo de un [palabra clave reservada de Razor](#razor-reserved-keywords), realiza una transición marcado específico de Razor.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-116">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords), it transitions into Razor-specific markup.</span></span> <span data-ttu-id="8f1fe-117">En caso contrario, realiza una transición sin formato C#.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-117">Otherwise, it transitions into plain C#.</span></span>

<span data-ttu-id="8f1fe-118">Escape un `@` de símbolos en el marcado de Razor, use un segundo `@` símbolo:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-118">To escape an `@` symbol in Razor markup, use a second `@` symbol:</span></span>

```cshtml
<p>@@Username</p>
```

<span data-ttu-id="8f1fe-119">El código se representa en HTML con una sola `@` símbolo:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-119">The code is rendered in HTML with a single `@` symbol:</span></span>

```html
<p>@Username</p>
```

<span data-ttu-id="8f1fe-120">Atributos HTML y el contenido que contiene direcciones de correo electrónico no tratan la `@` símbolos como un carácter de transición.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-120">HTML attributes and content containing email addresses don't treat the `@` symbol as a transition character.</span></span> <span data-ttu-id="8f1fe-121">Razor análisis no se modifica las direcciones de correo electrónico en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-121">The email addresses in the following example are untouched by Razor parsing:</span></span>

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a><span data-ttu-id="8f1fe-122">Expresiones implícitas de Razor</span><span class="sxs-lookup"><span data-stu-id="8f1fe-122">Implicit Razor expressions</span></span>

<span data-ttu-id="8f1fe-123">Las expresiones de Razor implícita empiezan con `@` seguido por el código de C#:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-123">Implicit Razor expressions start with `@` followed by C# code:</span></span>

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="8f1fe-124">Con la excepción de C# `await` palabra clave, las expresiones implícitas no deben contener espacios.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-124">With the exception of the C# `await` keyword, implicit expressions must not contain spaces.</span></span> <span data-ttu-id="8f1fe-125">Puede intermingle espacios si la instrucción de C# tiene un claro final:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-125">You can intermingle spaces if the C# statement has a clear ending:</span></span>

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

## <a name="explicit-razor-expressions"></a><span data-ttu-id="8f1fe-126">Expresiones explícitas de Razor</span><span class="sxs-lookup"><span data-stu-id="8f1fe-126">Explicit Razor expressions</span></span>

<span data-ttu-id="8f1fe-127">Expresiones de Razor explícitas constan de un `@` símbolo con paréntesis equilibrados.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-127">Explicit Razor expressions consist of an `@` symbol with balanced parenthesis.</span></span> <span data-ttu-id="8f1fe-128">Para representar la hora de la semana pasada, se utiliza el siguiente marcado de Razor:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-128">To render last week's time, the following Razor markup is used:</span></span>

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="8f1fe-129">Cualquier contenido dentro de la `@()` paréntesis se evalúa y se representa en la salida.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-129">Any content within the `@()` parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="8f1fe-130">Por lo general, las expresiones implícitas, que se describe en la sección anterior, no pueden contener espacios.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-130">Implicit expressions, described in the previous section, generally can't contain spaces.</span></span> <span data-ttu-id="8f1fe-131">En el código siguiente, una semana no resta de la hora actual:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-131">In the following code, one week isn't subtracted from the current time:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact.cshtml?range=17)]

<span data-ttu-id="8f1fe-132">El código representa el código HTML siguiente:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-132">The code renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

<span data-ttu-id="8f1fe-133">Puede utilizar una expresión explícita para concatenar texto con un resultado de la expresión:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-133">You can use an explicit expression to concatenate text with an expression result:</span></span>

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="8f1fe-134">Sin la expresión explícita, `<p>Age@joe.Age</p>` se trata como una dirección de correo electrónico, y `<p>Age@joe.Age</p>` se representa.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-134">Without the explicit expression, `<p>Age@joe.Age</p>` is treated as an email address, and `<p>Age@joe.Age</p>` is rendered.</span></span> <span data-ttu-id="8f1fe-135">Cuando se escriben como una expresión explícita, `<p>Age33</p>` se representa.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-135">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>

## <a name="expression-encoding"></a><span data-ttu-id="8f1fe-136">Codificación de expresión</span><span class="sxs-lookup"><span data-stu-id="8f1fe-136">Expression encoding</span></span>

<span data-ttu-id="8f1fe-137">Expresiones de C# que se evalúan como una cadena están codificado en HTML.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-137">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="8f1fe-138">Expresiones de C# que se evalúan como `IHtmlContent` se representan directamente desde `IHtmlContent.WriteTo`.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-138">C# expressions that evaluate to `IHtmlContent` are rendered directly through `IHtmlContent.WriteTo`.</span></span> <span data-ttu-id="8f1fe-139">Expresiones de C# que no se evalúan como `IHtmlContent` se convierte en una cadena por `ToString` y codificar antes de que se va a procesar.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-139">C# expressions that don't evaluate to `IHtmlContent` are converted to a string by `ToString` and encoded before they're rendered.</span></span>

```cshtml
@("<span>Hello World</span>")
```

<span data-ttu-id="8f1fe-140">El código representa el código HTML siguiente:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-140">The code renders the following HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

<span data-ttu-id="8f1fe-141">El código HTML se muestra en el explorador como:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-141">The HTML is shown in the browser as:</span></span>

```
<span>Hello World</span>
```

<span data-ttu-id="8f1fe-142">`HtmlHelper.Raw`salida no está codificada pero representar como marca HTML.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-142">`HtmlHelper.Raw` output isn't encoded but rendered as HTML markup.</span></span>

> [!WARNING]
> <span data-ttu-id="8f1fe-143">Usar `HtmlHelper.Raw` unsanitized usuario de la entrada es un riesgo de seguridad.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-143">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="8f1fe-144">Proporcionados por el usuario podrían contener código JavaScript malintencionado o seguridad de otras maneras.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-144">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="8f1fe-145">Es difícil inmunizar proporcionados por el usuario.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-145">Sanitizing user input is difficult.</span></span> <span data-ttu-id="8f1fe-146">Evite el uso de `HtmlHelper.Raw` con proporcionados por el usuario.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-146">Avoid using `HtmlHelper.Raw` with user input.</span></span>

```cshtml
@Html.Raw("<span>Hello World</span>")
```

<span data-ttu-id="8f1fe-147">El código representa el código HTML siguiente:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-147">The code renders the following HTML:</span></span>

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a><span data-ttu-id="8f1fe-148">Bloques de código Razor</span><span class="sxs-lookup"><span data-stu-id="8f1fe-148">Razor code blocks</span></span>

<span data-ttu-id="8f1fe-149">Bloques de código Razor iniciar con `@` y encerradas entre `{}`.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-149">Razor code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="8f1fe-150">A diferencia de las expresiones, no representa el código de C# dentro de bloques de código.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-150">Unlike expressions, C# code inside code blocks isn't rendered.</span></span> <span data-ttu-id="8f1fe-151">Bloques de código y las expresiones de una vista comparten el mismo ámbito y están definidas en orden:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-151">Code blocks and expressions in a view share the same scope and are defined in order:</span></span>

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

<span data-ttu-id="8f1fe-152">El código representa el código HTML siguiente:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-152">The code renders the following HTML:</span></span>

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

### <a name="implicit-transitions"></a><span data-ttu-id="8f1fe-153">Transiciones implícita</span><span class="sxs-lookup"><span data-stu-id="8f1fe-153">Implicit transitions</span></span>

<span data-ttu-id="8f1fe-154">Es el idioma predeterminado en un bloque de código C#, pero puede realizar la transición a HTML:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-154">The default language in a code block is C#, but you can transition back to HTML:</span></span>

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a><span data-ttu-id="8f1fe-155">Transición delimitado explícita</span><span class="sxs-lookup"><span data-stu-id="8f1fe-155">Explicit delimited transition</span></span>

<span data-ttu-id="8f1fe-156">Para definir una subsección de un bloque de código que debería presentar HTML, rodear los caracteres para la representación con el código Razor  **\<texto >** etiqueta:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-156">To define a sub-section of a code block that should render HTML, surround the characters for rendering with the Razor **\<text>** tag:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="8f1fe-157">Utilice este enfoque cuando desee representar HTML que no está rodeado por una etiqueta HTML.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-157">Use this approach when you want to render HTML that isn't surrounded by an HTML tag.</span></span> <span data-ttu-id="8f1fe-158">Sin una etiqueta HTML o Razor, recibirá un error de tiempo de ejecución de Razor.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-158">Without an HTML or Razor tag, you receive a Razor runtime error.</span></span>

<span data-ttu-id="8f1fe-159">El  **\<texto >** etiqueta también es útil para controlar el espacio en blanco al representar el contenido.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-159">The **\<text>** tag is also useful to control whitespace when rendering content.</span></span> <span data-ttu-id="8f1fe-160">Solo el contenido entre el  **\<texto >** se representan las etiquetas y no hay espacio en blanco antes o después de la  **\<texto >** etiquetas aparece en la salida HTML.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-160">Only the content between the **\<text>** tags is rendered, and no whitespace before or after the **\<text>** tags appears in the HTML output.</span></span>

### <a name="explicit-line-transition-with-"></a><span data-ttu-id="8f1fe-161">Transición de línea explícita con @:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-161">Explicit Line Transition with @:</span></span>

<span data-ttu-id="8f1fe-162">Para representar el resto de una línea completa como HTML dentro de un bloque de código, use la `@:` sintaxis:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-162">To render the rest of an entire line as HTML inside a code block, use the `@:` syntax:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="8f1fe-163">Sin el `@:` en el código, recibirá un error de tiempo de ejecución de Razor.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-163">Without the `@:` in the code, you recieve a Razor runtime error.</span></span>

## <a name="control-structures"></a><span data-ttu-id="8f1fe-164">Estructuras de control</span><span class="sxs-lookup"><span data-stu-id="8f1fe-164">Control Structures</span></span>

<span data-ttu-id="8f1fe-165">Estructuras de control son una extensión de bloques de código.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-165">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="8f1fe-166">Todos los aspectos de bloques de código (transición a marcado, C# en línea) también se aplican a las siguientes estructuras.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-166">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures.</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="8f1fe-167">Instrucciones condicionales @if, else if, else, y@switch</span><span class="sxs-lookup"><span data-stu-id="8f1fe-167">Conditionals @if, else if, else, and @switch</span></span>

<span data-ttu-id="8f1fe-168">`@if`controles cuando se ejecuta el código:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-168">`@if` controls when code runs:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

<span data-ttu-id="8f1fe-169">`else`y `else if` no requieren la `@` símbolo:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-169">`else` and `else if` don't require the `@` symbol:</span></span>

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

<span data-ttu-id="8f1fe-170">Puede usar una instrucción switch similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-170">You can use a switch statement like this:</span></span>

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

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="8f1fe-171">Bucle @for, @foreach, @while, y @do mientras</span><span class="sxs-lookup"><span data-stu-id="8f1fe-171">Looping @for, @foreach, @while, and @do while</span></span>

<span data-ttu-id="8f1fe-172">Puede representar HTML mediante plantillas con las instrucciones de control de bucle.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-172">You can render templated HTML with looping control statements.</span></span> <span data-ttu-id="8f1fe-173">Para presentar una lista de personas:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-173">To render a list of people:</span></span>

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

<span data-ttu-id="8f1fe-174">Puede usar cualquiera de las siguientes instrucciones bucles:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-174">You can use any of the following looping statements:</span></span>

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

### <a name="compound-using"></a><span data-ttu-id="8f1fe-175">Compuesta@using</span><span class="sxs-lookup"><span data-stu-id="8f1fe-175">Compound @using</span></span>

<span data-ttu-id="8f1fe-176">En C#, un `using` instrucción se utiliza para asegurarse de que se elimina un objeto.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-176">In C#, a `using` statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="8f1fe-177">En Razor, el mismo mecanismo se utiliza para crear aplicaciones auxiliares de HTML que contienen contenido adicional.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-177">In Razor, the same mechanism is used to create HTML Helpers that contain additional content.</span></span> <span data-ttu-id="8f1fe-178">Por ejemplo, puede usar aplicaciones auxiliares de HTML para presentar una etiqueta de formulario con el `@using` instrucción:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-178">For instance, you can utilize HTML Helpers to render a form tag with the `@using` statement:</span></span>

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

<span data-ttu-id="8f1fe-179">También puede realizar acciones de nivel de ámbito con [aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="8f1fe-179">You can also perform scope-level actions with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

### <a name="try-catch-finally"></a><span data-ttu-id="8f1fe-180">@try, catch y finally</span><span class="sxs-lookup"><span data-stu-id="8f1fe-180">@try, catch, finally</span></span>

<span data-ttu-id="8f1fe-181">Control de excepciones es similar a C#:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-181">Exception handling is similar to C#:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

<span data-ttu-id="8f1fe-182">Razor tiene la capacidad de proteger las secciones críticas con instrucciones de bloqueo:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-182">Razor has the capability to protect critical sections with lock statements:</span></span>

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="8f1fe-183">Comentarios</span><span class="sxs-lookup"><span data-stu-id="8f1fe-183">Comments</span></span>

<span data-ttu-id="8f1fe-184">Razor admite comentarios de C# y el código HTML:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-184">Razor supports C# and HTML comments:</span></span>

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

<span data-ttu-id="8f1fe-185">El código representa el código HTML siguiente:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-185">The code renders the following HTML:</span></span>

```html
<!-- HTML comment -->
```

<span data-ttu-id="8f1fe-186">Comentarios de Razor se quitan mediante el servidor antes de presenta la página Web.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-186">Razor comments are removed by the server before the webpage is rendered.</span></span> <span data-ttu-id="8f1fe-187">Razor usa `@*  *@` para delimitar los comentarios.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-187">Razor uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="8f1fe-188">El código siguiente se hace referencia a, por lo que el servidor no representa ningún otro marcado:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-188">The following code is commented out, so the server doesn't render any markup:</span></span>

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a><span data-ttu-id="8f1fe-189">Directivas</span><span class="sxs-lookup"><span data-stu-id="8f1fe-189">Directives</span></span>

<span data-ttu-id="8f1fe-190">Directivas de Razor se representan mediante expresiones implícitas con las siguientes palabras clave reservadas el `@` símbolos.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-190">Razor directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="8f1fe-191">Normalmente, una directiva cambia la forma en que una vista se analiza o habilita una funcionalidad diferente.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-191">A directive typically changes the way a view is parsed or enables different functionality.</span></span>

<span data-ttu-id="8f1fe-192">Descripción de cómo Razor genera código para una vista resulta más fácil comprender cómo funcionan las directivas.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-192">Understanding how Razor generates code for a view makes it easier to understand how directives work.</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

<span data-ttu-id="8f1fe-193">El código genera una clase similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-193">The code generates a class similar to the following:</span></span>

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

<span data-ttu-id="8f1fe-194">Más adelante en este artículo, la sección [ver la clase de C# de Razor generada por una vista](#viewing-the-razor-c-class-generated-for-a-view) explica cómo ver esta clase generada.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-194">Later in this article, the section [Viewing the Razor C# class generated for a view](#viewing-the-razor-c-class-generated-for-a-view) explains how to view this generated class.</span></span>

### <a name="using"></a>@using

<span data-ttu-id="8f1fe-195">El `@using` directiva agrega C# `using` la directiva a la vista generada:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-195">The `@using` directive adds the C# `using` directive to the generated view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

<span data-ttu-id="8f1fe-196">El `@model` directiva especifica el tipo del modelo que se pasan a una vista:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-196">The `@model` directive specifies the type of the model passed to a view:</span></span>

```cshtml
@model TypeNameOfModel
```

<span data-ttu-id="8f1fe-197">Si crea una aplicación de MVC de ASP.NET Core con cuentas de usuario individuales, el *Views/Account/Login.cshtml* vista contiene la siguiente declaración de modelo:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-197">If you create an ASP.NET Core MVC app with individual user accounts, the *Views/Account/Login.cshtml* view contains the following model declaration:</span></span>

```cshtml
@model LoginViewModel
```

<span data-ttu-id="8f1fe-198">La clase generada se hereda de `RazorPage<dynamic>`:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-198">The class generated inherits from `RazorPage<dynamic>`:</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

<span data-ttu-id="8f1fe-199">Razor expone un `Model` propiedad para tener acceso al modelo que se pasa a la vista:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-199">Razor exposes a `Model` property for accessing the model passed to the view:</span></span>

```cshtml
<div>The Login Email: @Model.Email</div>
```

<span data-ttu-id="8f1fe-200">El `@model` directiva especifica el tipo de esta propiedad.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-200">The `@model` directive specifies the type of this property.</span></span> <span data-ttu-id="8f1fe-201">La directiva especifica la `T` en `RazorPage<T>` que generado clase que deriva de la vista de.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-201">The directive specifies the `T` in `RazorPage<T>` that the generated class that your view derives from.</span></span> <span data-ttu-id="8f1fe-202">Si no se especifica la `@model` directiva, la `Model` propiedad es de tipo `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-202">If you don't specify the `@model` directive, the `Model` property is of type `dynamic`.</span></span> <span data-ttu-id="8f1fe-203">El valor del modelo se pasa desde el controlador a la vista.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-203">The value of the model is passed from the controller to the view.</span></span> <span data-ttu-id="8f1fe-204">Vea [fuertemente tipadas modelos y la @model palabra clave](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-keyword-label) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-204">See [Strongly typed models and the @model keyword](xref:tutorials/first-mvc-app/adding-model#strongly-typed-models-keyword-label) for more information.</span></span>

### <a name="inherits"></a>@inherits

<span data-ttu-id="8f1fe-205">El `@inherits` directiva proporciona control total de la clase que hereda de la vista:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-205">The `@inherits` directive gives you full control of the class your view inherits:</span></span>

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

<span data-ttu-id="8f1fe-206">El siguiente es un tipo de página de Razor personalizado:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-206">The following is a custom Razor page type:</span></span>

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

<span data-ttu-id="8f1fe-207">El `CustomText` se muestran en una vista:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-207">The `CustomText` is displayed in a view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact10.cshtml)]

<span data-ttu-id="8f1fe-208">El código representa el código HTML siguiente:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-208">The code renders the following HTML:</span></span>

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

<span data-ttu-id="8f1fe-209">No se puede utilizar `@model` y `@inherits` en la misma vista.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-209">You can't use `@model` and `@inherits` in the same view.</span></span> <span data-ttu-id="8f1fe-210">Puede tener `@inherits` en un *_ViewImports.cshtml* archivo que se importa de la vista:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-210">You can have `@inherits` in a *_ViewImports.cshtml* file that the view imports:</span></span>

[!code-cshtml[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

<span data-ttu-id="8f1fe-211">El siguiente es un ejemplo de una vista fuertemente tipada:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-211">The following is an example of a strongly-typed view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Login1.cshtml)]

<span data-ttu-id="8f1fe-212">Si "rick@contoso.com" se pasa en el modelo, la vista genera el siguiente marcado HTML:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-212">If "rick@contoso.com" is passed in the model, the view generates the following HTML markup:</span></span>

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject

<span data-ttu-id="8f1fe-213">El `@inject` directiva permite insertar un servicio desde el [contenedor de servicios](xref:fundamentals/dependency-injection) en la vista.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-213">The `@inject` directive enables you to inject a service from your [service container](xref:fundamentals/dependency-injection) into your view.</span></span> <span data-ttu-id="8f1fe-214">Vea [inyección de dependencia en las vistas](xref:mvc/views/dependency-injection) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-214">See [Dependency injection into views](xref:mvc/views/dependency-injection) for more information.</span></span>

### <a name="functions"></a>@functions

<span data-ttu-id="8f1fe-215">El `@functions` directiva le permite agregar contenido de nivel de función a una vista:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-215">The `@functions` directive enables you to add function-level content to a view:</span></span>

```cshtml
@functions { // C# Code }
```

<span data-ttu-id="8f1fe-216">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-216">For example:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact6.cshtml)]

<span data-ttu-id="8f1fe-217">El código genera el siguiente marcado HTML:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-217">The code generates the following HTML markup:</span></span>

```html
<div>From method: Hello</div>
```

<span data-ttu-id="8f1fe-218">El código siguiente es la clase generada de Razor C#:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-218">The following code is the generated Razor C# class:</span></span>

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### <a name="section"></a>@section

<span data-ttu-id="8f1fe-219">El `@section` directiva se usa junto con el [diseño](xref:mvc/views/layout) para habilitar las vistas representar el contenido en diferentes partes de la página HTML.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-219">The `@section` directive is used in conjunction with the [layout](xref:mvc/views/layout) to enable views to render content in different parts of the HTML page.</span></span> <span data-ttu-id="8f1fe-220">Vea [secciones](xref:mvc/views/layout#layout-sections-label) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-220">See [Sections](xref:mvc/views/layout#layout-sections-label) for more information.</span></span>

## <a name="tag-helpers"></a><span data-ttu-id="8f1fe-221">Aplicaciones auxiliares de etiquetas</span><span class="sxs-lookup"><span data-stu-id="8f1fe-221">Tag Helpers</span></span>

<span data-ttu-id="8f1fe-222">Hay tres directivas que pertenecen a [aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="8f1fe-222">There are three directives that pertain to [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

| <span data-ttu-id="8f1fe-223">Directiva</span><span class="sxs-lookup"><span data-stu-id="8f1fe-223">Directive</span></span> | <span data-ttu-id="8f1fe-224">Función</span><span class="sxs-lookup"><span data-stu-id="8f1fe-224">Function</span></span> |
| --------- | -------- |
| [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) | <span data-ttu-id="8f1fe-225">Pone a disposición a una vista de aplicaciones auxiliares de etiquetas.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-225">Makes Tag Helpers available to a view.</span></span> |
| [@removeTagHelper](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | <span data-ttu-id="8f1fe-226">Quita las aplicaciones auxiliares de etiquetas que agregó anteriormente desde una vista.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-226">Removes Tag Helpers previously added from a view.</span></span> |
| [@tagHelperPrefix](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | <span data-ttu-id="8f1fe-227">Especifica un prefijo de etiqueta para habilitar la compatibilidad de la aplicación auxiliar de etiqueta y hacer uso de la aplicación auxiliar de etiqueta explícita.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-227">Specifies a tag prefix to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> |

## <a name="razor-reserved-keywords"></a><span data-ttu-id="8f1fe-228">Palabras clave reservada de Razor</span><span class="sxs-lookup"><span data-stu-id="8f1fe-228">Razor reserved keywords</span></span>

### <a name="razor-keywords"></a><span data-ttu-id="8f1fe-229">Palabras clave de Razor</span><span class="sxs-lookup"><span data-stu-id="8f1fe-229">Razor keywords</span></span>

* <span data-ttu-id="8f1fe-230">página (requiere el núcleo ASP.NET 2.0 y versiones posterior)</span><span class="sxs-lookup"><span data-stu-id="8f1fe-230">page (Requires ASP.NET Core 2.0 and later)</span></span>
* <span data-ttu-id="8f1fe-231">funciones</span><span class="sxs-lookup"><span data-stu-id="8f1fe-231">functions</span></span>
* <span data-ttu-id="8f1fe-232">hereda</span><span class="sxs-lookup"><span data-stu-id="8f1fe-232">inherits</span></span>
* <span data-ttu-id="8f1fe-233">modelo</span><span class="sxs-lookup"><span data-stu-id="8f1fe-233">model</span></span>
* <span data-ttu-id="8f1fe-234">section</span><span class="sxs-lookup"><span data-stu-id="8f1fe-234">section</span></span>
* <span data-ttu-id="8f1fe-235">aplicación auxiliar (actualmente no admitida ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="8f1fe-235">helper (Not currently supported by ASP.NET Core)</span></span>

<span data-ttu-id="8f1fe-236">Palabras clave de Razor se escapan con `@(Razor Keyword)` (por ejemplo, `@(functions)`).</span><span class="sxs-lookup"><span data-stu-id="8f1fe-236">Razor keywords are escaped with `@(Razor Keyword)` (for example, `@(functions)`).</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="8f1fe-237">Palabras clave de C# Razor</span><span class="sxs-lookup"><span data-stu-id="8f1fe-237">C# Razor keywords</span></span>

* <span data-ttu-id="8f1fe-238">case</span><span class="sxs-lookup"><span data-stu-id="8f1fe-238">case</span></span>
* <span data-ttu-id="8f1fe-239">do</span><span class="sxs-lookup"><span data-stu-id="8f1fe-239">do</span></span>
* <span data-ttu-id="8f1fe-240">default</span><span class="sxs-lookup"><span data-stu-id="8f1fe-240">default</span></span>
* <span data-ttu-id="8f1fe-241">for</span><span class="sxs-lookup"><span data-stu-id="8f1fe-241">for</span></span>
* <span data-ttu-id="8f1fe-242">foreach</span><span class="sxs-lookup"><span data-stu-id="8f1fe-242">foreach</span></span>
* <span data-ttu-id="8f1fe-243">if</span><span class="sxs-lookup"><span data-stu-id="8f1fe-243">if</span></span>
* <span data-ttu-id="8f1fe-244">else</span><span class="sxs-lookup"><span data-stu-id="8f1fe-244">else</span></span>
* <span data-ttu-id="8f1fe-245">bloquear</span><span class="sxs-lookup"><span data-stu-id="8f1fe-245">lock</span></span>
* <span data-ttu-id="8f1fe-246">switch</span><span class="sxs-lookup"><span data-stu-id="8f1fe-246">switch</span></span>
* <span data-ttu-id="8f1fe-247">try</span><span class="sxs-lookup"><span data-stu-id="8f1fe-247">try</span></span>
* <span data-ttu-id="8f1fe-248">catch</span><span class="sxs-lookup"><span data-stu-id="8f1fe-248">catch</span></span>
* <span data-ttu-id="8f1fe-249">finally</span><span class="sxs-lookup"><span data-stu-id="8f1fe-249">finally</span></span>
* <span data-ttu-id="8f1fe-250">utilizar</span><span class="sxs-lookup"><span data-stu-id="8f1fe-250">using</span></span>
* <span data-ttu-id="8f1fe-251">while</span><span class="sxs-lookup"><span data-stu-id="8f1fe-251">while</span></span>

<span data-ttu-id="8f1fe-252">Palabras clave de C# Razor debe ser un carácter de escape doble, con `@(@C# Razor Keyword)` (por ejemplo, `@(@case)`).</span><span class="sxs-lookup"><span data-stu-id="8f1fe-252">C# Razor keywords must be double-escaped with `@(@C# Razor Keyword)` (for example, `@(@case)`).</span></span> <span data-ttu-id="8f1fe-253">La primera `@` antepone el analizador Razor.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-253">The first `@` escapes the Razor parser.</span></span> <span data-ttu-id="8f1fe-254">El segundo `@` antepone el analizador de C#.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-254">The second `@` escapes the C# parser.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="8f1fe-255">Palabras clave reservadas no utilizadas Razor</span><span class="sxs-lookup"><span data-stu-id="8f1fe-255">Reserved keywords not used by Razor</span></span>

* <span data-ttu-id="8f1fe-256">namespace</span><span class="sxs-lookup"><span data-stu-id="8f1fe-256">namespace</span></span>
* <span data-ttu-id="8f1fe-257">clase</span><span class="sxs-lookup"><span data-stu-id="8f1fe-257">class</span></span>

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="8f1fe-258">Visualización de la clase de C# de Razor generada por una vista</span><span class="sxs-lookup"><span data-stu-id="8f1fe-258">Viewing the Razor C# class generated for a view</span></span>

<span data-ttu-id="8f1fe-259">Agregue la siguiente clase al proyecto de MVC de ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-259">Add the following class to your ASP.NET Core MVC project:</span></span>

[!code-csharp[Main](razor/sample/Utilities/CustomTemplateEngine.cs)]

<span data-ttu-id="8f1fe-260">Invalidar el `RazorTemplateEngine` agregado MVC con la `CustomTemplateEngine` clase:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-260">Override the `RazorTemplateEngine` added by MVC with the `CustomTemplateEngine` class:</span></span>

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=10-14)]

<span data-ttu-id="8f1fe-261">Establecer un punto de interrupción en la `return csharpDocument` instrucción de `CustomTemplateEngine`.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-261">Set a break point on the `return csharpDocument` statement of `CustomTemplateEngine`.</span></span> <span data-ttu-id="8f1fe-262">Cuando la ejecución del programa se detiene en el punto de interrupción, ver el valor de `generatedCode`.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-262">When program execution stops at the break point, view the value of `generatedCode`.</span></span>

![Vista de generatedCode visualizador de texto](razor/_static/tvr.png)

## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="8f1fe-264">Búsquedas de vista y entre mayúsculas y minúsculas</span><span class="sxs-lookup"><span data-stu-id="8f1fe-264">View lookups and case sensitivity</span></span>

<span data-ttu-id="8f1fe-265">El motor de vista Razor realiza búsquedas entre mayúsculas y minúsculas para las vistas.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-265">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="8f1fe-266">Sin embargo, la búsqueda real se determina por el sistema de archivos subyacente:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-266">However, the actual lookup is determined by the underlying file system:</span></span>

* <span data-ttu-id="8f1fe-267">En función del origen del archivo:</span><span class="sxs-lookup"><span data-stu-id="8f1fe-267">File based source:</span></span> 
  * <span data-ttu-id="8f1fe-268">En sistemas operativos con sistemas de archivos entre mayúsculas y minúsculas (por ejemplo, Windows), búsquedas de proveedor de archivo físico distinguen entre mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-268">On operating systems with case insensitive file systems (for example, Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="8f1fe-269">Por ejemplo, `return View("Test")` da como resultado que coincidan con la */Views/Home/Test.cshtml*, */Views/home/test.cshtml*y cualquier otra variante de mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-269">For example, `return View("Test")` results in matches for */Views/Home/Test.cshtml*, */Views/home/test.cshtml*, and any other casing variant.</span></span>
  * <span data-ttu-id="8f1fe-270">En sistemas de archivos entre mayúsculas y minúsculas (por ejemplo, Linux y OSX y con `EmbeddedFileProvider`), las búsquedas distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-270">On case sensitive file systems (for example, Linux, OSX, and with `EmbeddedFileProvider`), lookups are case sensitive.</span></span> <span data-ttu-id="8f1fe-271">Por ejemplo, `return View("Test")` específicamente coincidencias */Views/Home/Test.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-271">For example, `return View("Test")` specifically matches */Views/Home/Test.cshtml*.</span></span>
* <span data-ttu-id="8f1fe-272">Precompilado vistas: con núcleo de ASP.Net 2.0 y versiones posterior, buscar vistas precompiladas distingue mayúsculas de minúsculas en todos los sistemas operativos.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-272">Precompiled views: With ASP.Net Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="8f1fe-273">El comportamiento es idéntico al comportamiento del proveedor del archivo físico en Windows.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-273">The behavior is identical to physical file provider's behavior on Windows.</span></span> <span data-ttu-id="8f1fe-274">Si dos vistas precompiladas difieren solo en caso de que el resultado de búsqueda es no determinista.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-274">If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="8f1fe-275">Los desarrolladores pueden hacer coincidir las mayúsculas y minúsculas de los nombres de archivo y directorio para las mayúsculas y minúsculas de los nombres de área, acción y controlador.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-275">Developers are encouraged to match the casing of file and directory names to the casing of area, controller, and action names.</span></span> <span data-ttu-id="8f1fe-276">Esto garantiza que las implementaciones encontrará sus vistas sin tener en cuenta el sistema de archivos subyacente.</span><span class="sxs-lookup"><span data-stu-id="8f1fe-276">This ensures your deployments will find their views regardless of the underlying file system.</span></span>
