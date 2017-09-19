---
title: Referencia de la sintaxis de Razor para ASP.NET Core
author: rick-anderson
description: Sintaxis de Razor de detalles
keywords: "Núcleo de ASP.NET, Razor"
ms.author: riande
manager: wpickett
ms.date: 07/04/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/razor
ms.openlocfilehash: fff2f98592473a9baf6a2d4e360fec3026b7210d
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2017
---
# <a name="razor-syntax-for-aspnet-core"></a><span data-ttu-id="9cbdf-104">Sintaxis de Razor para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9cbdf-104">Razor syntax for ASP.NET Core</span></span>

<span data-ttu-id="9cbdf-105">Por [Taylor Mullen](https://twitter.com/ntaylormullen) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9cbdf-105">By [Taylor Mullen](https://twitter.com/ntaylormullen) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="what-is-razor"></a><span data-ttu-id="9cbdf-106">¿Qué es Razor?</span><span class="sxs-lookup"><span data-stu-id="9cbdf-106">What is Razor?</span></span>

<span data-ttu-id="9cbdf-107">Razor es una sintaxis de marcado para incrustar código de servidor basándose en las páginas web.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-107">Razor is a markup syntax for embedding server based code into web pages.</span></span> <span data-ttu-id="9cbdf-108">La sintaxis de Razor consta del marcado de Razor, C# y el código HTML.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-108">The Razor syntax consists of Razor markup, C# and HTML.</span></span> <span data-ttu-id="9cbdf-109">Archivos que contienen Razor generalmente tienen un *.cshtml* la extensión de archivo.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-109">Files containing Razor generally have a *.cshtml* file extension.</span></span>

## <a name="rendering-html"></a><span data-ttu-id="9cbdf-110">Representación HTML</span><span class="sxs-lookup"><span data-stu-id="9cbdf-110">Rendering HTML</span></span>

<span data-ttu-id="9cbdf-111">El idioma de Razor predeterminado es HTML.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-111">The default Razor language is HTML.</span></span> <span data-ttu-id="9cbdf-112">Es similar a representar HTML de Razor en un archivo HTML.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-112">Rendering HTML from Razor is no different than in an HTML file.</span></span> <span data-ttu-id="9cbdf-113">Un archivo Razor con el siguiente marcado:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-113">A Razor file with the following markup:</span></span>

```html
<p>Hello World</p>
   ```

<span data-ttu-id="9cbdf-114">Se representa sin modificar como `<p>Hello World</p>` por el servidor.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-114">Is rendered unchanged as `<p>Hello World</p>` by the server.</span></span>

## <a name="razor-syntax"></a><span data-ttu-id="9cbdf-115">Sintaxis de Razor</span><span class="sxs-lookup"><span data-stu-id="9cbdf-115">Razor syntax</span></span>

<span data-ttu-id="9cbdf-116">Razor es compatible con C# y usa el `@` símbolo para realizar la transición de HTML para C#.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-116">Razor supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> <span data-ttu-id="9cbdf-117">Razor evalúa las expresiones de C# y los representa en la salida HTML.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-117">Razor evaluates C# expressions and renders them in the HTML output.</span></span> <span data-ttu-id="9cbdf-118">Razor puede realizar la transición de HTML a C# o a un marcado específico de Razor.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-118">Razor can transition from HTML into C# or into Razor-specific markup.</span></span> <span data-ttu-id="9cbdf-119">Cuando un `@` va seguido de símbolo de un [palabra clave reservada de Razor](#razor-reserved-keywords) realiza la transición en el marcado específico de Razor, en caso contrario, realiza una transición sin formato C#.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-119">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords) it transitions into Razor-specific markup, otherwise it transitions into plain C#.</span></span>

<a name=escape-at-label></a>

<span data-ttu-id="9cbdf-120">HTML que contiene `@` símbolos que necesite ser caracteres de escape con una segunda `@` símbolos.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-120">HTML containing `@` symbols may need to be escaped with a second `@` symbol.</span></span> <span data-ttu-id="9cbdf-121">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-121">For example:</span></span>

```html
<p>@@Username</p>
   ```

<span data-ttu-id="9cbdf-122">podría invalidar el código HTML siguiente:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-122">would render the following HTML:</span></span>

```html
<p>@Username</p>
   ```

<a name=razor-email-ref></a>

<span data-ttu-id="9cbdf-123">Atributos HTML y el contenido que contiene direcciones de correo electrónico no tratan la `@` símbolos como un carácter de transición.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-123">HTML attributes and content containing email addresses don’t treat the `@` symbol as a transition character.</span></span>

   `<a href="mailto:Support@contoso.com">Support@contoso.com</a>`

## <a name="implicit-razor-expressions"></a><span data-ttu-id="9cbdf-124">Expresiones implícitas de Razor</span><span class="sxs-lookup"><span data-stu-id="9cbdf-124">Implicit Razor expressions</span></span>

<span data-ttu-id="9cbdf-125">Las expresiones de Razor implícita empiezan con `@` seguido por el código de C#.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-125">Implicit Razor expressions start with `@` followed by C# code.</span></span> <span data-ttu-id="9cbdf-126">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-126">For example:</span></span>

```html
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="9cbdf-127">Con la excepción de C# `await` expresiones implícitas de palabra clave no deben contener espacios.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-127">With the exception of the C# `await` keyword implicit expressions must not contain spaces.</span></span> <span data-ttu-id="9cbdf-128">Por ejemplo, puede intermingle espacios siempre y cuando la instrucción de C# tiene un claro final:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-128">For example, you can intermingle spaces as long as the C# statement has a clear ending:</span></span>

```html
<p>@await DoSomething("hello", "world")</p>
```

## <a name="explicit-razor-expressions"></a><span data-ttu-id="9cbdf-129">Expresiones explícitas de Razor</span><span class="sxs-lookup"><span data-stu-id="9cbdf-129">Explicit Razor expressions</span></span>

<span data-ttu-id="9cbdf-130">Las expresiones de Razor explícitas consta de un símbolo con paréntesis equilibrados @.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-130">Explicit Razor expressions consists of an @ symbol with balanced parenthesis.</span></span> <span data-ttu-id="9cbdf-131">Por ejemplo, para representar la hora de la última semana:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-131">For example, to render last week's time:</span></span>

```html
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="9cbdf-132">Cualquier contenido dentro de la @ paréntesis () se evalúa y se representa en la salida.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-132">Any content within the @() parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="9cbdf-133">Por lo general, las expresiones implícitas no pueden contener espacios.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-133">Implicit expressions generally cannot contain spaces.</span></span> <span data-ttu-id="9cbdf-134">Por ejemplo, en el código siguiente, no se restan una semana desde la hora actual:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-134">For example, in the code below, one week is not subtracted from the current time:</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact.cshtml?range=20)]

<span data-ttu-id="9cbdf-135">Que representa el código HTML siguiente:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-135">Which renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
   ```

<span data-ttu-id="9cbdf-136">Puede utilizar una expresión explícita para concatenar texto con un resultado de la expresión:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-136">You can use an explicit expression to concatenate text with an expression result:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "none", "highlight_args": {"hl_lines": [5]}} -->

```none
@{
    var joe = new Person("Joe", 33);
 }

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="9cbdf-137">Sin la expresión explícita, `<p>Age@joe.Age</p>` se tratará como una dirección de correo electrónico y `<p>Age@joe.Age</p>` se representarían en.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-137">Without the explicit expression, `<p>Age@joe.Age</p>` would be treated as an email address and `<p>Age@joe.Age</p>` would be rendered.</span></span> <span data-ttu-id="9cbdf-138">Cuando se escriben como una expresión explícita, `<p>Age33</p>` se representa.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-138">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>

<a name=expression-encoding-label></a>

## <a name="expression-encoding"></a><span data-ttu-id="9cbdf-139">Codificación de expresión</span><span class="sxs-lookup"><span data-stu-id="9cbdf-139">Expression encoding</span></span>

<span data-ttu-id="9cbdf-140">Expresiones de C# que se evalúan como una cadena están codificado en HTML.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-140">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="9cbdf-141">Expresiones de C# que se evalúan como `IHtmlContent` se representan directamente desde *IHtmlContent.WriteTo*.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-141">C# expressions that evaluate to `IHtmlContent` are rendered directly through *IHtmlContent.WriteTo*.</span></span> <span data-ttu-id="9cbdf-142">Expresiones de C# que no se evalúan como *IHtmlContent* se convierte en una cadena (por *ToString*) y codificado antes de que se representan.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-142">C# expressions that don't evaluate to *IHtmlContent* are converted to a string (by *ToString*) and encoded before they are rendered.</span></span> <span data-ttu-id="9cbdf-143">Por ejemplo, el siguiente marcado de Razor:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-143">For example, the following Razor markup:</span></span>

```html
@("<span>Hello World</span>")
   ```

<span data-ttu-id="9cbdf-144">Esto representa HTML:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-144">Renders this HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
   ```

<span data-ttu-id="9cbdf-145">Que el explorador se representa como:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-145">Which the browser renders as:</span></span>

`<span>Hello World</span>`

<span data-ttu-id="9cbdf-146">`HtmlHelper``Raw` salida no está codificada pero representar como marca HTML.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-146">`HtmlHelper` `Raw` output is not encoded but rendered as HTML markup.</span></span>

>[!WARNING]
> <span data-ttu-id="9cbdf-147">Usar `HtmlHelper.Raw` unsanitized usuario de la entrada es un riesgo de seguridad.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-147">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="9cbdf-148">Proporcionados por el usuario podrían contener código JavaScript malintencionado o seguridad de otras maneras.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-148">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="9cbdf-149">Inmunizar proporcionados por el usuario es difícil, evite el uso de `HtmlHelper.Raw` en proporcionados por el usuario.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-149">Sanitizing user input is difficult, avoid using `HtmlHelper.Raw` on user input.</span></span>

<span data-ttu-id="9cbdf-150">El siguiente marcado de Razor:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-150">The following Razor markup:</span></span>

```html
@Html.Raw("<span>Hello World</span>")
   ```

<span data-ttu-id="9cbdf-151">Esto representa HTML:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-151">Renders this HTML:</span></span>

```html
<span>Hello World</span>
   ```

<a name=razor-code-blocks-label></a>

## <a name="razor-code-blocks"></a><span data-ttu-id="9cbdf-152">Bloques de código Razor</span><span class="sxs-lookup"><span data-stu-id="9cbdf-152">Razor code blocks</span></span>

<span data-ttu-id="9cbdf-153">Bloques de código Razor iniciar con `@` y encerradas entre `{}`.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-153">Razor code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="9cbdf-154">A diferencia de las expresiones, no se representa el código de C# dentro de bloques de código.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-154">Unlike expressions, C# code inside code blocks is not rendered.</span></span> <span data-ttu-id="9cbdf-155">Bloques de código y expresiones en una página de Razor comparten el mismo ámbito y están definidas en orden (es decir, las declaraciones en un bloque de código será en el ámbito de expresiones y bloques de código más adelante).</span><span class="sxs-lookup"><span data-stu-id="9cbdf-155">Code blocks and expressions in a Razor page share the same scope and are defined in order (that is, declarations in a code block will be in scope for later code blocks and expressions).</span></span>

```none
@{
    var output = "Hello World";
}

<p>The rendered result: @output</p>
```

<span data-ttu-id="9cbdf-156">Podría invalidar:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-156">Would render:</span></span>

```html
<p>The rendered result: Hello World</p>
   ```

<a name=implicit-transitions-label></a>

### <a name="implicit-transitions"></a><span data-ttu-id="9cbdf-157">Transiciones implícita</span><span class="sxs-lookup"><span data-stu-id="9cbdf-157">Implicit transitions</span></span>

<span data-ttu-id="9cbdf-158">Es el idioma predeterminado en un bloque de código C#, pero puede realizar la transición a HTML.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-158">The default language in a code block is C#, but you can transition back to HTML.</span></span> <span data-ttu-id="9cbdf-159">HTML dentro de un bloque de código realizará la transición a presentar HTML:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-159">HTML within a code block will transition back into rendering HTML:</span></span>

```none
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

<a name=explicit-delimited-transition-label></a>

### <a name="explicit-delimited-transition"></a><span data-ttu-id="9cbdf-160">Transición delimitado explícita</span><span class="sxs-lookup"><span data-stu-id="9cbdf-160">Explicit delimited transition</span></span>

<span data-ttu-id="9cbdf-161">Para definir una subsección de un bloque de código que debería presentar HTML, rodear los caracteres que se va a representar con el código Razor `<text>` etiqueta:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-161">To define a sub-section of a code block that should render HTML, surround the characters to be rendered with the Razor `<text>` tag:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "none", "highlight_args": {"hl_lines": [4]}} -->

```none
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="9cbdf-162">Generalmente, este enfoque se utiliza cuando desee presentar HTML que no está rodeado por una etiqueta HTML.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-162">You generally use this approach when you want to render HTML that is not surrounded by an HTML tag.</span></span> <span data-ttu-id="9cbdf-163">Sin una etiqueta HTML o Razor, obtendrá un error de tiempo de ejecución de Razor.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-163">Without an HTML or Razor tag, you get a Razor runtime error.</span></span>

<a name=explicit-line-transition-with-label></a>

### <a name="explicit-line-transition-with-"></a><span data-ttu-id="9cbdf-164">Transición de línea explícita con`@:`</span><span class="sxs-lookup"><span data-stu-id="9cbdf-164">Explicit Line Transition with `@:`</span></span>

<span data-ttu-id="9cbdf-165">Para representar el resto de una línea completa como HTML dentro de un bloque de código, use la `@:` sintaxis:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-165">To render the rest of an entire line as HTML inside a code block, use the `@:` syntax:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "none", "highlight_args": {"hl_lines": [4]}} -->

```none
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="9cbdf-166">Sin el `@:` en el código anterior, obtendrá un error en tiempo de ejecución de Razor.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-166">Without the `@:` in the code above, you'd get a Razor run time error.</span></span>

<a name=control-structures-razor-label></a>

## <a name="control-structures"></a><span data-ttu-id="9cbdf-167">Estructuras de control</span><span class="sxs-lookup"><span data-stu-id="9cbdf-167">Control Structures</span></span>

<span data-ttu-id="9cbdf-168">Estructuras de control son una extensión de bloques de código.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-168">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="9cbdf-169">Todos los aspectos de bloques de código (transición a marcado, C# en línea) también se aplican a las siguientes estructuras.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-169">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures.</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="9cbdf-170">Instrucciones condicionales `@if`, `else if`, `else` y`@switch`</span><span class="sxs-lookup"><span data-stu-id="9cbdf-170">Conditionals `@if`, `else if`, `else` and `@switch`</span></span>

<span data-ttu-id="9cbdf-171">El `@if` familia controla cuándo se ejecuta el código:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-171">The `@if` family controls when code runs:</span></span>

```none
@if (value % 2 == 0)
{
    <p>The value was even</p>
}
```

<span data-ttu-id="9cbdf-172">`else`y `else if` no requieren la `@` símbolo:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-172">`else` and `else if` don't require the `@` symbol:</span></span>

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

<span data-ttu-id="9cbdf-173">Puede usar una instrucción switch similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-173">You can use a switch statement like this:</span></span>

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

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="9cbdf-174">Bucle `@for`, `@foreach`, `@while`, y`@do while`</span><span class="sxs-lookup"><span data-stu-id="9cbdf-174">Looping `@for`, `@foreach`, `@while`, and `@do while`</span></span>

<span data-ttu-id="9cbdf-175">Puede representar HTML mediante plantillas con las instrucciones de control de bucle.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-175">You can render templated HTML with looping control statements.</span></span> <span data-ttu-id="9cbdf-176">Por ejemplo, para presentar una lista de personas:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-176">For example, to render a list of people:</span></span>

```none
@{
    var people = new Person[]
    {
          new Person("John", 33),
          new Person("Doe", 41),
    };
}
```

<span data-ttu-id="9cbdf-177">Puede usar cualquiera de las siguientes instrucciones bucles:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-177">You can use any of the following looping statements:</span></span>

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

### <a name="compound-using"></a><span data-ttu-id="9cbdf-178">Compuesta`@using`</span><span class="sxs-lookup"><span data-stu-id="9cbdf-178">Compound `@using`</span></span>

<span data-ttu-id="9cbdf-179">En C# un uso de la instrucción se utiliza para asegurarse de que se elimina un objeto.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-179">In C# a using statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="9cbdf-180">En Razor este mismo mecanismo se puede utilizar para crear aplicaciones auxiliares HTML que contienen contenido adicional.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-180">In Razor this same mechanism can be used to create HTML helpers that contain additional content.</span></span> <span data-ttu-id="9cbdf-181">Por ejemplo, se puede utilizar aplicaciones auxiliares de HTML para presentar una etiqueta de formulario con el `@using` instrucción:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-181">For instance, we can utilize HTML Helpers to render a form tag with the `@using` statement:</span></span>

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

<span data-ttu-id="9cbdf-182">También puede realizar acciones de nivel de ámbito como los pasos anteriores con [aplicaciones auxiliares de etiquetas](tag-helpers/index.md).</span><span class="sxs-lookup"><span data-stu-id="9cbdf-182">You can also perform scope level actions like the above with [Tag Helpers](tag-helpers/index.md).</span></span>

### <a name="try-catch-finally"></a><span data-ttu-id="9cbdf-183">`@try`, `catch`, `finally`</span><span class="sxs-lookup"><span data-stu-id="9cbdf-183">`@try`, `catch`, `finally`</span></span>

<span data-ttu-id="9cbdf-184">Control de excepciones es similar a C#:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-184">Exception handling is similar to  C#:</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact7.cshtml)]

### `@lock`

<span data-ttu-id="9cbdf-185">Razor tiene la capacidad de proteger las secciones críticas con instrucciones de bloqueo:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-185">Razor has the capability to protect critical sections with lock statements:</span></span>

```none
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="9cbdf-186">Comentarios</span><span class="sxs-lookup"><span data-stu-id="9cbdf-186">Comments</span></span>

<span data-ttu-id="9cbdf-187">Razor admite comentarios de C# y el código HTML.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-187">Razor supports C# and HTML comments.</span></span> <span data-ttu-id="9cbdf-188">El siguiente marcado:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-188">The following markup:</span></span>

```none
@{
    /* C# comment. */
    // Another C# comment.
}
<!-- HTML comment -->
```

<span data-ttu-id="9cbdf-189">Se representa como por el servidor:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-189">Is rendered by the server as:</span></span>

```none
<!-- HTML comment -->
```

<span data-ttu-id="9cbdf-190">Comentarios de Razor se quitan mediante el servidor antes de presentar la página.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-190">Razor comments are removed by the server before the page is rendered.</span></span> <span data-ttu-id="9cbdf-191">Razor usa `@*  *@` para delimitar los comentarios.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-191">Razor uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="9cbdf-192">El código siguiente se hace referencia a, por lo que el servidor no se puede representar cualquier marcado:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-192">The following code is commented out, so the server will not render any markup:</span></span>

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

## <a name="directives"></a><span data-ttu-id="9cbdf-193">Directivas</span><span class="sxs-lookup"><span data-stu-id="9cbdf-193">Directives</span></span>

<span data-ttu-id="9cbdf-194">Directivas de Razor se representan mediante expresiones implícitas con las siguientes palabras clave reservadas el `@` símbolos.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-194">Razor directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="9cbdf-195">Una directiva normalmente se cambiar la manera en que se analiza una página o habilitar una funcionalidad diferente dentro de la página de Razor.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-195">A directive will typically change the way a page is parsed or enable different functionality within your Razor page.</span></span>

<span data-ttu-id="9cbdf-196">Descripción de cómo Razor genera código para una vista le resultará más fácil de entender cómo funcionan las directivas.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-196">Understanding how Razor generates code for a view will make it easier to understand how directives work.</span></span> <span data-ttu-id="9cbdf-197">Una página de Razor se usa para generar un archivo de C#.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-197">A Razor page is used to generate a C# file.</span></span> <span data-ttu-id="9cbdf-198">Por ejemplo, esta página Razor:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-198">For example, this Razor page:</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

<span data-ttu-id="9cbdf-199">Genera una clase similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-199">Generates a class similar to the following:</span></span>

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

<span data-ttu-id="9cbdf-200">[Visualización de la clase de C# de Razor generada por una vista](#razor-customcompilationservice-label) explica cómo ver esta clase generada.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-200">[Viewing the Razor C# class generated for a view](#razor-customcompilationservice-label) explains how to view this generated class.</span></span>

### `@using`

<span data-ttu-id="9cbdf-201">El `@using` directiva agregará c# `using` la directiva a la página de razor generado:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-201">The `@using` directive will add the c# `using` directive to the generated razor page:</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact9.cshtml)]

### `@model`

<span data-ttu-id="9cbdf-202">El `@model` directiva especifica el tipo de modelo que se pasa a la página de Razor.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-202">The `@model` directive specifies the type of the model passed to the Razor page.</span></span> <span data-ttu-id="9cbdf-203">Utiliza la siguiente sintaxis:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-203">It uses the following syntax:</span></span>

```none
@model TypeNameOfModel
   ```

<span data-ttu-id="9cbdf-204">Por ejemplo, si crea una aplicación de MVC de ASP.NET Core con cuentas de usuario individuales, el *Views/Account/Login.cshtml* vista Razor contiene la siguiente declaración de modelo:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-204">For example, if you create an ASP.NET Core MVC app with individual user accounts, the *Views/Account/Login.cshtml* Razor view contains the following model declaration:</span></span>

```csharp
@model LoginViewModel
   ```

<span data-ttu-id="9cbdf-205">En el ejemplo anterior de la clase, la clase generada se hereda de `RazorPage<dynamic>`.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-205">In the preceding class example, the class generated inherits from `RazorPage<dynamic>`.</span></span> <span data-ttu-id="9cbdf-206">Agregando un `@model` controlar lo que se hereda.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-206">By adding an `@model` you control what’s inherited.</span></span> <span data-ttu-id="9cbdf-207">Por ejemplo</span><span class="sxs-lookup"><span data-stu-id="9cbdf-207">For example</span></span>

```csharp
@model LoginViewModel
   ```

<span data-ttu-id="9cbdf-208">Genera la clase siguiente</span><span class="sxs-lookup"><span data-stu-id="9cbdf-208">Generates the following class</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
   ```

<span data-ttu-id="9cbdf-209">Las páginas de Razor exponen un `Model` propiedad para tener acceso al modelo que se pasa a la página.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-209">Razor pages expose a `Model` property for accessing the model passed to the page.</span></span>

```html
<div>The Login Email: @Model.Email</div>
   ```

<span data-ttu-id="9cbdf-210">El `@model` el tipo de esta propiedad especifica la directiva (especificando la `T` en `RazorPage<T>` que se deriva de la clase generada para la página).</span><span class="sxs-lookup"><span data-stu-id="9cbdf-210">The `@model` directive specified the type of this property (by specifying the `T` in `RazorPage<T>` that the generated class for your page derives from).</span></span> <span data-ttu-id="9cbdf-211">Si no se especifica la `@model` directiva la `Model` propiedad será de tipo `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-211">If you don't specify the `@model` directive the `Model` property will be of type `dynamic`.</span></span> <span data-ttu-id="9cbdf-212">El valor del modelo se pasa desde el controlador a la vista.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-212">The value of the model is passed from the controller to the view.</span></span> <span data-ttu-id="9cbdf-213">Vea [fuertemente tipadas modelos y la @model palabra clave](../../tutorials/first-mvc-app/adding-model.md#strongly-typed-models-keyword-label) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-213">See [Strongly typed models and the @model keyword](../../tutorials/first-mvc-app/adding-model.md#strongly-typed-models-keyword-label) for more information.</span></span>

### `@inherits`

<span data-ttu-id="9cbdf-214">El `@inherits` directiva proporciona control total de la clase que hereda de la página de Razor:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-214">The `@inherits` directive gives you full control of the class your Razor page inherits:</span></span>

```none
@inherits TypeNameOfClassToInheritFrom
   ```

<span data-ttu-id="9cbdf-215">Por ejemplo, supongamos que ha surgido del siguiente tipo de página Razor personalizado:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-215">For instance, let’s say we had the following custom Razor page type:</span></span>

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

<span data-ttu-id="9cbdf-216">El siguiente código de Razor generaría `<div>Custom text: Hello World</div>`.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-216">The following Razor would generate `<div>Custom text: Hello World</div>`.</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact10.cshtml)]

<span data-ttu-id="9cbdf-217">No se puede utilizar `@model` y `@inherits` en la misma página.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-217">You can't use `@model` and `@inherits` on the same page.</span></span> <span data-ttu-id="9cbdf-218">Puede tener `@inherits` en un *_ViewImports.cshtml* archivo que se importa en la página de Razor.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-218">You can have `@inherits` in a *_ViewImports.cshtml* file that the Razor page imports.</span></span> <span data-ttu-id="9cbdf-219">Por ejemplo, si la vista Razor importado los siguientes *_ViewImports.cshtml* archivo:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-219">For example, if your Razor view imported the following *_ViewImports.cshtml* file:</span></span>

[!code-html[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

<span data-ttu-id="9cbdf-220">La siguiente página de Razor fuertemente tipada</span><span class="sxs-lookup"><span data-stu-id="9cbdf-220">The following strongly typed Razor page</span></span>

[!code-html[Main](razor/sample/Views/Home/Login1.cshtml)]

<span data-ttu-id="9cbdf-221">Se genera este marcado HTML:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-221">Generates this HTML markup:</span></span>

```none
<div>The Login Email: Rick@contoso.com</div>
<div>Custom text: Hello World</div>
```

<span data-ttu-id="9cbdf-222">Cuando se pasan "[Rick@contoso.com](mailto:Rick@contoso.com)" en el modelo:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-222">When passed "[Rick@contoso.com](mailto:Rick@contoso.com)" in the model:</span></span>

   <span data-ttu-id="9cbdf-223">Vea [Layout](layout.md) (Diseño) para más información.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-223">See [Layout](layout.md) for more information.</span></span>

### `@inject`

<span data-ttu-id="9cbdf-224">El `@inject` directiva permite insertar un servicio desde el [contenedor de servicios](../../fundamentals/dependency-injection.md) en la página de Razor para su uso.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-224">The `@inject` directive enables you to inject a service from your [service container](../../fundamentals/dependency-injection.md)  into your Razor page for use.</span></span> <span data-ttu-id="9cbdf-225">Vea [inyección de dependencia en las vistas](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="9cbdf-225">See [Dependency injection into views](dependency-injection.md).</span></span>

<a name="functions"></a>

### `@functions`

<span data-ttu-id="9cbdf-226">El `@functions` directiva le permite agregar contenido de nivel de función a la página de Razor.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-226">The `@functions` directive enables you to add function level content to your Razor page.</span></span> <span data-ttu-id="9cbdf-227">La sintaxis es la siguiente:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-227">The syntax is:</span></span>

```none
@functions { // C# Code }
   ```

<span data-ttu-id="9cbdf-228">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-228">For example:</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact6.cshtml)]

<span data-ttu-id="9cbdf-229">Genera el siguiente marcado HTML:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-229">Generates the following HTML markup:</span></span>

```none
<div>From method: Hello</div>
   ```

<span data-ttu-id="9cbdf-230">Generado Razor C# es similar:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-230">The generated Razor C# looks like:</span></span>

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### `@section`

<span data-ttu-id="9cbdf-231">El `@section` directiva se usa junto con el [página de diseño](layout.md) para habilitar las vistas representar el contenido en diferentes partes de la página HTML representada.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-231">The `@section` directive is used in conjunction with the [layout page](layout.md) to enable views to render content in different parts of the rendered HTML page.</span></span> <span data-ttu-id="9cbdf-232">Vea [secciones](layout.md#layout-sections-label) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-232">See [Sections](layout.md#layout-sections-label) for more information.</span></span>

## <a name="tag-helpers"></a><span data-ttu-id="9cbdf-233">Aplicaciones auxiliares de etiquetas</span><span class="sxs-lookup"><span data-stu-id="9cbdf-233">Tag Helpers</span></span>

<span data-ttu-id="9cbdf-234">El siguiente [aplicaciones auxiliares de etiquetas](tag-helpers/index.md) directivas se detallan en los vínculos proporcionados.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-234">The following [Tag Helpers](tag-helpers/index.md) directives are detailed in the links provided.</span></span>

* [@addTagHelper](tag-helpers/intro.md#add-helper-label)
* [@removeTagHelper](tag-helpers/intro.md#remove-razor-directives-label)
* [@tagHelperPrefix](tag-helpers/intro.md#prefix-razor-directives-label)

<a name=razor-reserved-keywords-label></a>

## <a name="razor-reserved-keywords"></a><span data-ttu-id="9cbdf-235">Palabras clave reservada de Razor</span><span class="sxs-lookup"><span data-stu-id="9cbdf-235">Razor reserved keywords</span></span>

### <a name="razor-keywords"></a><span data-ttu-id="9cbdf-236">Palabras clave de Razor</span><span class="sxs-lookup"><span data-stu-id="9cbdf-236">Razor keywords</span></span>

* <span data-ttu-id="9cbdf-237">página (requiere el núcleo ASP.NET 2.0 y versiones posterior)</span><span class="sxs-lookup"><span data-stu-id="9cbdf-237">page (Requires ASP.NET Core 2.0 and later)</span></span>
* <span data-ttu-id="9cbdf-238">funciones</span><span class="sxs-lookup"><span data-stu-id="9cbdf-238">functions</span></span>
* <span data-ttu-id="9cbdf-239">hereda</span><span class="sxs-lookup"><span data-stu-id="9cbdf-239">inherits</span></span>
* <span data-ttu-id="9cbdf-240">modelo</span><span class="sxs-lookup"><span data-stu-id="9cbdf-240">model</span></span>
* <span data-ttu-id="9cbdf-241">section</span><span class="sxs-lookup"><span data-stu-id="9cbdf-241">section</span></span>
* <span data-ttu-id="9cbdf-242">aplicación auxiliar (no compatible con ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="9cbdf-242">helper   (Not supported by ASP.NET Core.)</span></span>

<span data-ttu-id="9cbdf-243">Palabras clave de Razor se pueden escapar con `@(Razor Keyword)`, por ejemplo `@(functions)`.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-243">Razor keywords can be escaped with `@(Razor Keyword)`, for example `@(functions)`.</span></span> <span data-ttu-id="9cbdf-244">Vea el ejemplo completo siguiente.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-244">See the complete sample below.</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="9cbdf-245">Palabras clave de C# Razor</span><span class="sxs-lookup"><span data-stu-id="9cbdf-245">C# Razor keywords</span></span>

* <span data-ttu-id="9cbdf-246">case</span><span class="sxs-lookup"><span data-stu-id="9cbdf-246">case</span></span>
* <span data-ttu-id="9cbdf-247">do</span><span class="sxs-lookup"><span data-stu-id="9cbdf-247">do</span></span>
* <span data-ttu-id="9cbdf-248">default</span><span class="sxs-lookup"><span data-stu-id="9cbdf-248">default</span></span>
* <span data-ttu-id="9cbdf-249">for</span><span class="sxs-lookup"><span data-stu-id="9cbdf-249">for</span></span>
* <span data-ttu-id="9cbdf-250">foreach</span><span class="sxs-lookup"><span data-stu-id="9cbdf-250">foreach</span></span>
* <span data-ttu-id="9cbdf-251">if</span><span class="sxs-lookup"><span data-stu-id="9cbdf-251">if</span></span>
* <span data-ttu-id="9cbdf-252">else</span><span class="sxs-lookup"><span data-stu-id="9cbdf-252">else</span></span>
* <span data-ttu-id="9cbdf-253">bloquear</span><span class="sxs-lookup"><span data-stu-id="9cbdf-253">lock</span></span>
* <span data-ttu-id="9cbdf-254">switch</span><span class="sxs-lookup"><span data-stu-id="9cbdf-254">switch</span></span>
* <span data-ttu-id="9cbdf-255">try</span><span class="sxs-lookup"><span data-stu-id="9cbdf-255">try</span></span>
* <span data-ttu-id="9cbdf-256">catch</span><span class="sxs-lookup"><span data-stu-id="9cbdf-256">catch</span></span>
* <span data-ttu-id="9cbdf-257">finally</span><span class="sxs-lookup"><span data-stu-id="9cbdf-257">finally</span></span>
* <span data-ttu-id="9cbdf-258">utilizar</span><span class="sxs-lookup"><span data-stu-id="9cbdf-258">using</span></span>
* <span data-ttu-id="9cbdf-259">while</span><span class="sxs-lookup"><span data-stu-id="9cbdf-259">while</span></span>

<span data-ttu-id="9cbdf-260">Palabras clave de C# Razor deba ser dobles de escape `@(@C# Razor Keyword)`, por ejemplo `@(@case)`.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-260">C# Razor keywords need to be double escaped with `@(@C# Razor Keyword)`, for example `@(@case)`.</span></span> <span data-ttu-id="9cbdf-261">La primera `@` antepone el analizador Razor, el segundo `@` antepone el analizador de C#.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-261">The first `@` escapes the Razor parser, the second `@` escapes the C# parser.</span></span> <span data-ttu-id="9cbdf-262">Vea el ejemplo completo siguiente.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-262">See the complete sample below.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="9cbdf-263">Palabras clave reservadas no utilizadas Razor</span><span class="sxs-lookup"><span data-stu-id="9cbdf-263">Reserved keywords not used by Razor</span></span>

* <span data-ttu-id="9cbdf-264">namespace</span><span class="sxs-lookup"><span data-stu-id="9cbdf-264">namespace</span></span>
* <span data-ttu-id="9cbdf-265">clase</span><span class="sxs-lookup"><span data-stu-id="9cbdf-265">class</span></span>

<a name=razor-customcompilationservice-label></a>

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="9cbdf-266">Visualización de la clase de C# de Razor generada por una vista</span><span class="sxs-lookup"><span data-stu-id="9cbdf-266">Viewing the Razor C# class generated for a view</span></span>

<span data-ttu-id="9cbdf-267">Agregue la siguiente clase al proyecto de MVC de ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-267">Add the following class to your ASP.NET Core MVC project:</span></span>

[!code-csharp[Main](razor/sample/Services/CustomCompilationService.cs)]

<span data-ttu-id="9cbdf-268">Invalidar el `ICompilationService` agrega MVC con la clase anterior;</span><span class="sxs-lookup"><span data-stu-id="9cbdf-268">Override the `ICompilationService` added by MVC with the above class;</span></span>

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=29-33)]

<span data-ttu-id="9cbdf-269">Establecer un punto de interrupción en la `Compile` método `CustomCompilationService` y vista `compilationContent`.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-269">Set a break point on the `Compile` method of `CustomCompilationService` and view `compilationContent`.</span></span>

![Vista de compilationContent visualizador de texto](razor/_static/tvr.png)

<a name="case"></a>
## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="9cbdf-271">Búsquedas de vista y entre mayúsculas y minúsculas</span><span class="sxs-lookup"><span data-stu-id="9cbdf-271">View lookups and case sensitivity</span></span>

<span data-ttu-id="9cbdf-272">El motor de vista Razor realiza búsquedas entre mayúsculas y minúsculas para las vistas.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-272">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="9cbdf-273">Sin embargo, la búsqueda real se determina por el origen subyacente:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-273">However, the actual lookup is determined by the underlying source:</span></span>

* <span data-ttu-id="9cbdf-274">En función del origen del archivo:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-274">File based source:</span></span> 

    * <span data-ttu-id="9cbdf-275">En sistemas operativos con sistemas de archivos entre mayúsculas y minúsculas (por ejemplo, Windows), búsquedas de proveedor de archivo físico distinguen entre mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-275">On operating systems with case insensitive file systems (like Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="9cbdf-276">Por ejemplo `return View("Test")` , se crearán `/Views/Home/Test.cshtml`, `/Views/home/test.cshtml` y otras variantes de mayúsculas y minúsculas podrían ser detectados.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-276">For example `return View("Test")` would result in `/Views/Home/Test.cshtml`, `/Views/home/test.cshtml` and all other casing variants would be discovered.</span></span>
    * <span data-ttu-id="9cbdf-277">En sistemas de archivos entre mayúsculas y minúsculas, lo que incluye Linux, OSX y `EmbeddedFileProvider`, las búsquedas distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-277">On case sensitive file systems, which includes Linux, OSX and `EmbeddedFileProvider`, lookups are case sensitive.</span></span> <span data-ttu-id="9cbdf-278">Por ejemplo, `return View("Test")` sería específicamente `/Views/Home/Test.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-278">For example, `return View("Test")` would specifically look for `/Views/Home/Test.cshtml`.</span></span>
        
* <span data-ttu-id="9cbdf-279">Vistas precompiladas:</span><span class="sxs-lookup"><span data-stu-id="9cbdf-279">Precompiled views:</span></span>

   * <span data-ttu-id="9cbdf-280">Con ASP.Net Core 2.0 y versiones posteriores, buscar vistas precompiladas distingue mayúsculas de minúsculas en todos los sistemas operativos.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-280">With ASP.Net Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="9cbdf-281">El comportamiento es idéntico al comportamiento del proveedor del archivo físico en Windows.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-281">The behavior is identical to physical file provider's behavior on Windows.</span></span> 
   <span data-ttu-id="9cbdf-282">Nota: Si dos vistas precompiladas difieren solo en el caso, el resultado de búsqueda es no determinista.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-282">Note: If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="9cbdf-283">Los desarrolladores pueden hacer coincidir las mayúsculas y minúsculas de los nombres de archivo y directorio para las mayúsculas y minúsculas de los nombres de área, acción y controlador.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-283">Developers are encouraged to match the casing of file and directory names to the casing of area, controller and action names.</span></span> <span data-ttu-id="9cbdf-284">Esto garantizará que las implementaciones de permanecen independientes del sistema de archivos subyacente.</span><span class="sxs-lookup"><span data-stu-id="9cbdf-284">This would ensure your deployments remain agnostic of the underlying file system.</span></span>
