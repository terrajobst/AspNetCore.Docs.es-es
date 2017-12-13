---
title: Referencia de la sintaxis de Razor para ASP.NET Core
author: rick-anderson
description: "Obtenga información acerca de la sintaxis de marcado de Razor para incrustar código basado en servidor en las páginas Web."
keywords: Directivas de ASP.NET Core, Razor, Razor
ms.author: riande
manager: wpickett
ms.date: 10/18/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/razor
ms.openlocfilehash: e3c3149254d602db1fcc6d42360690be026189a5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
# <a name="razor-syntax-for-aspnet-core"></a><span data-ttu-id="27f95-104">Sintaxis de Razor para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="27f95-104">Razor syntax for ASP.NET Core</span></span>

<span data-ttu-id="27f95-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex), [Taylor Mullen](https://twitter.com/ntaylormullen), y [Dan Vicarel](https://github.com/Rabadash8820)</span><span class="sxs-lookup"><span data-stu-id="27f95-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Luke Latham](https://github.com/guardrex),  [Taylor Mullen](https://twitter.com/ntaylormullen), and [Dan Vicarel](https://github.com/Rabadash8820)</span></span>

<span data-ttu-id="27f95-106">Razor es una sintaxis de marcado para incrustar código basado en servidor en las páginas Web.</span><span class="sxs-lookup"><span data-stu-id="27f95-106">Razor is a markup syntax for embedding server-based code into webpages.</span></span> <span data-ttu-id="27f95-107">La sintaxis de Razor consta de Razor marcado, C# y HTML.</span><span class="sxs-lookup"><span data-stu-id="27f95-107">The Razor syntax consists of Razor markup, C#, and HTML.</span></span> <span data-ttu-id="27f95-108">Archivos que contienen Razor generalmente tienen un *.cshtml* la extensión de archivo.</span><span class="sxs-lookup"><span data-stu-id="27f95-108">Files containing Razor generally have a *.cshtml* file extension.</span></span>

## <a name="rendering-html"></a><span data-ttu-id="27f95-109">Representación HTML</span><span class="sxs-lookup"><span data-stu-id="27f95-109">Rendering HTML</span></span>

<span data-ttu-id="27f95-110">El idioma de Razor predeterminado es HTML.</span><span class="sxs-lookup"><span data-stu-id="27f95-110">The default Razor language is HTML.</span></span> <span data-ttu-id="27f95-111">Representación HTML desde el marcado de Razor es similar a representar HTML desde un archivo HTML.</span><span class="sxs-lookup"><span data-stu-id="27f95-111">Rendering HTML from Razor markup is no different than rendering HTML from an HTML file.</span></span>  <span data-ttu-id="27f95-112">El marcado HTML en *.cshtml* archivos Razor se representa en el servidor sin cambios.</span><span class="sxs-lookup"><span data-stu-id="27f95-112">HTML markup in *.cshtml* Razor files is rendered by the server unchanged.</span></span>

## <a name="razor-syntax"></a><span data-ttu-id="27f95-113">Sintaxis de Razor</span><span class="sxs-lookup"><span data-stu-id="27f95-113">Razor syntax</span></span>

<span data-ttu-id="27f95-114">Razor es compatible con C# y usa el `@` símbolo para realizar la transición de HTML para C#.</span><span class="sxs-lookup"><span data-stu-id="27f95-114">Razor supports C# and uses the `@` symbol to transition from HTML to C#.</span></span> <span data-ttu-id="27f95-115">Razor evalúa las expresiones de C# y los representa en la salida HTML.</span><span class="sxs-lookup"><span data-stu-id="27f95-115">Razor evaluates C# expressions and renders them in the HTML output.</span></span>

<span data-ttu-id="27f95-116">Cuando un `@` va seguido de símbolo de un [palabra clave reservada de Razor](#razor-reserved-keywords), realiza una transición marcado específico de Razor.</span><span class="sxs-lookup"><span data-stu-id="27f95-116">When an `@` symbol is followed by a [Razor reserved keyword](#razor-reserved-keywords), it transitions into Razor-specific markup.</span></span> <span data-ttu-id="27f95-117">En caso contrario, realiza una transición sin formato C#.</span><span class="sxs-lookup"><span data-stu-id="27f95-117">Otherwise, it transitions into plain C#.</span></span>

<span data-ttu-id="27f95-118">Escape un `@` de símbolos en el marcado de Razor, use un segundo `@` símbolo:</span><span class="sxs-lookup"><span data-stu-id="27f95-118">To escape an `@` symbol in Razor markup, use a second `@` symbol:</span></span>

```cshtml
<p>@@Username</p>
```

<span data-ttu-id="27f95-119">El código se representa en HTML con una sola `@` símbolo:</span><span class="sxs-lookup"><span data-stu-id="27f95-119">The code is rendered in HTML with a single `@` symbol:</span></span>

```html
<p>@Username</p>
```

<span data-ttu-id="27f95-120">Atributos HTML y el contenido que contiene direcciones de correo electrónico no tratan la `@` símbolos como un carácter de transición.</span><span class="sxs-lookup"><span data-stu-id="27f95-120">HTML attributes and content containing email addresses don't treat the `@` symbol as a transition character.</span></span> <span data-ttu-id="27f95-121">Razor análisis no se modifica las direcciones de correo electrónico en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="27f95-121">The email addresses in the following example are untouched by Razor parsing:</span></span>

```cshtml
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

## <a name="implicit-razor-expressions"></a><span data-ttu-id="27f95-122">Expresiones implícitas de Razor</span><span class="sxs-lookup"><span data-stu-id="27f95-122">Implicit Razor expressions</span></span>

<span data-ttu-id="27f95-123">Las expresiones de Razor implícita empiezan con `@` seguido por el código de C#:</span><span class="sxs-lookup"><span data-stu-id="27f95-123">Implicit Razor expressions start with `@` followed by C# code:</span></span>

```cshtml
<p>@DateTime.Now</p>
<p>@DateTime.IsLeapYear(2016)</p>
```

<span data-ttu-id="27f95-124">Con la excepción de C# `await` palabra clave, las expresiones implícitas no deben contener espacios.</span><span class="sxs-lookup"><span data-stu-id="27f95-124">With the exception of the C# `await` keyword, implicit expressions must not contain spaces.</span></span> <span data-ttu-id="27f95-125">Si la instrucción de C# tiene un final claro, pueden ser mezclados espacios:</span><span class="sxs-lookup"><span data-stu-id="27f95-125">If the C# statement has a clear ending, spaces can be intermingled:</span></span>

```cshtml
<p>@await DoSomething("hello", "world")</p>
```

<span data-ttu-id="27f95-126">Las expresiones implícitas **no** contienen tipos genéricos de C#, como los caracteres dentro de los corchetes (`<>`) se interpretan como una etiqueta HTML.</span><span class="sxs-lookup"><span data-stu-id="27f95-126">Implicit expressions **cannot** contain C# generics, as the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="27f95-127">El código siguiente es **no** válido:</span><span class="sxs-lookup"><span data-stu-id="27f95-127">The following code is **not** valid:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="27f95-128">El código anterior genera un error del compilador similar a uno de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="27f95-128">The preceding code generates a compiler error similar to one of the following:</span></span>

 * <span data-ttu-id="27f95-129">No se cerró el elemento "int".</span><span class="sxs-lookup"><span data-stu-id="27f95-129">The "int" element was not closed.</span></span>  <span data-ttu-id="27f95-130">Todos los elementos deben ser de autocierre o tiene la correspondiente etiqueta de cierre.</span><span class="sxs-lookup"><span data-stu-id="27f95-130">All elements must be either self-closing or have a matching end tag.</span></span>
 *  <span data-ttu-id="27f95-131">No se puede convertir el grupo de métodos 'GenericMethod' a 'object' de tipo no delegado.</span><span class="sxs-lookup"><span data-stu-id="27f95-131">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="27f95-132">Pretendía invocar el método?'</span><span class="sxs-lookup"><span data-stu-id="27f95-132">Did you intend to invoke the method?\`</span></span> 
 
<span data-ttu-id="27f95-133">Llamadas de método genérico deben incluirse en un [expresión explícita de Razor](#explicit-razor-expressions) o un [bloque de código Razor](#razor-code-blocks).</span><span class="sxs-lookup"><span data-stu-id="27f95-133">Generic method calls must be wrapped in an [explicit Razor expression](#explicit-razor-expressions) or a [Razor code block](#razor-code-blocks).</span></span> <span data-ttu-id="27f95-134">Esta restricción no se aplica a *.vbhtml* Razor archivos porque la sintaxis de Visual Basic coloca los parámetros de tipo genérico en lugar de corchetes entre paréntesis.</span><span class="sxs-lookup"><span data-stu-id="27f95-134">This restriction doesn't apply to *.vbhtml* Razor files because Visual Basic syntax places parentheses around generic type parameters instead of brackets.</span></span>

## <a name="explicit-razor-expressions"></a><span data-ttu-id="27f95-135">Expresiones explícitas de Razor</span><span class="sxs-lookup"><span data-stu-id="27f95-135">Explicit Razor expressions</span></span>

<span data-ttu-id="27f95-136">Expresiones de Razor explícitas constan de un `@` símbolo con paréntesis equilibrados.</span><span class="sxs-lookup"><span data-stu-id="27f95-136">Explicit Razor expressions consist of an `@` symbol with balanced parenthesis.</span></span> <span data-ttu-id="27f95-137">Para representar la hora de la semana pasada, se utiliza el siguiente marcado de Razor:</span><span class="sxs-lookup"><span data-stu-id="27f95-137">To render last week's time, the following Razor markup is used:</span></span>

```cshtml
<p>Last week this time: @(DateTime.Now - TimeSpan.FromDays(7))</p>
```

<span data-ttu-id="27f95-138">Cualquier contenido dentro de la `@()` paréntesis se evalúa y se representa en la salida.</span><span class="sxs-lookup"><span data-stu-id="27f95-138">Any content within the `@()` parenthesis is evaluated and rendered to the output.</span></span>

<span data-ttu-id="27f95-139">Por lo general, las expresiones implícitas, que se describe en la sección anterior, no pueden contener espacios.</span><span class="sxs-lookup"><span data-stu-id="27f95-139">Implicit expressions, described in the previous section, generally can't contain spaces.</span></span> <span data-ttu-id="27f95-140">En el código siguiente, una semana no resta de la hora actual:</span><span class="sxs-lookup"><span data-stu-id="27f95-140">In the following code, one week isn't subtracted from the current time:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact.cshtml?range=17)]

<span data-ttu-id="27f95-141">El código representa el código HTML siguiente:</span><span class="sxs-lookup"><span data-stu-id="27f95-141">The code renders the following HTML:</span></span>

```html
<p>Last week: 7/7/2016 4:39:52 PM - TimeSpan.FromDays(7)</p>
```

<span data-ttu-id="27f95-142">Pueden utilizarse expresiones explícitas para concatenar texto con un resultado de la expresión:</span><span class="sxs-lookup"><span data-stu-id="27f95-142">Explicit expressions can be used to concatenate text with an expression result:</span></span>

```cshtml
@{
    var joe = new Person("Joe", 33);
}

<p>Age@(joe.Age)</p>
```

<span data-ttu-id="27f95-143">Sin la expresión explícita, `<p>Age@joe.Age</p>` se trata como una dirección de correo electrónico, y `<p>Age@joe.Age</p>` se representa.</span><span class="sxs-lookup"><span data-stu-id="27f95-143">Without the explicit expression, `<p>Age@joe.Age</p>` is treated as an email address, and `<p>Age@joe.Age</p>` is rendered.</span></span> <span data-ttu-id="27f95-144">Cuando se escriben como una expresión explícita, `<p>Age33</p>` se representa.</span><span class="sxs-lookup"><span data-stu-id="27f95-144">When written as an explicit expression, `<p>Age33</p>` is rendered.</span></span>


<span data-ttu-id="27f95-145">Pueden utilizarse expresiones explícitas para representar el resultado de los métodos genéricos en *.cshtml* archivos.</span><span class="sxs-lookup"><span data-stu-id="27f95-145">Explicit expressions can be used to render output from generic methods in *.cshtml* files.</span></span> <span data-ttu-id="27f95-146">En una expresión implícita, los caracteres dentro de los corchetes (`<>`) se interpretan como una etiqueta HTML.</span><span class="sxs-lookup"><span data-stu-id="27f95-146">In an implicit expression, the characters inside the brackets (`<>`) are interpreted as an HTML tag.</span></span> <span data-ttu-id="27f95-147">Es el marcado siguiente **no** Razor válido:</span><span class="sxs-lookup"><span data-stu-id="27f95-147">The following markup is **not** valid Razor:</span></span>

```cshtml
<p>@GenericMethod<int>()</p>
```

<span data-ttu-id="27f95-148">El código anterior genera un error del compilador similar a uno de los siguientes:</span><span class="sxs-lookup"><span data-stu-id="27f95-148">The preceding code generates a compiler error similar to one of the following:</span></span>

 * <span data-ttu-id="27f95-149">No se cerró el elemento "int".</span><span class="sxs-lookup"><span data-stu-id="27f95-149">The "int" element was not closed.</span></span>  <span data-ttu-id="27f95-150">Todos los elementos deben ser de autocierre o tiene la correspondiente etiqueta de cierre.</span><span class="sxs-lookup"><span data-stu-id="27f95-150">All elements must be either self-closing or have a matching end tag.</span></span>
 *  <span data-ttu-id="27f95-151">No se puede convertir el grupo de métodos 'GenericMethod' a 'object' de tipo no delegado.</span><span class="sxs-lookup"><span data-stu-id="27f95-151">Cannot convert method group 'GenericMethod' to non-delegate type 'object'.</span></span> <span data-ttu-id="27f95-152">Pretendía invocar el método?'</span><span class="sxs-lookup"><span data-stu-id="27f95-152">Did you intend to invoke the method?\`</span></span> 
 
 <span data-ttu-id="27f95-153">El marcado siguiente muestra este código de la escritura de forma correcta.</span><span class="sxs-lookup"><span data-stu-id="27f95-153">The following markup shows the correct way write this code.</span></span>  <span data-ttu-id="27f95-154">El código se escribe como una expresión explícita:</span><span class="sxs-lookup"><span data-stu-id="27f95-154">The code is written as an explicit expression:</span></span>

```cshtml
<p>@(GenericMethod<int>())</p>
```

<span data-ttu-id="27f95-155">Nota: esta restricción no se aplica a *.vbhtml* archivos Razor.</span><span class="sxs-lookup"><span data-stu-id="27f95-155">Note: this restriction doesn't apply to *.vbhtml* Razor files.</span></span>  <span data-ttu-id="27f95-156">Con *.vbhtml* archivos Razor, sintaxis de Visual Basic coloca los parámetros de tipo genérico en lugar de corchetes entre paréntesis.</span><span class="sxs-lookup"><span data-stu-id="27f95-156">With *.vbhtml* Razor files, Visual Basic syntax places parentheses around generic type parameters instead of brackets.</span></span>

## <a name="expression-encoding"></a><span data-ttu-id="27f95-157">Codificación de expresión</span><span class="sxs-lookup"><span data-stu-id="27f95-157">Expression encoding</span></span>

<span data-ttu-id="27f95-158">Expresiones de C# que se evalúan como una cadena están codificado en HTML.</span><span class="sxs-lookup"><span data-stu-id="27f95-158">C# expressions that evaluate to a string are HTML encoded.</span></span> <span data-ttu-id="27f95-159">Expresiones de C# que se evalúan como `IHtmlContent` se representan directamente desde `IHtmlContent.WriteTo`.</span><span class="sxs-lookup"><span data-stu-id="27f95-159">C# expressions that evaluate to `IHtmlContent` are rendered directly through `IHtmlContent.WriteTo`.</span></span> <span data-ttu-id="27f95-160">Expresiones de C# que no se evalúan como `IHtmlContent` se convierte en una cadena por `ToString` y codificar antes de que se va a procesar.</span><span class="sxs-lookup"><span data-stu-id="27f95-160">C# expressions that don't evaluate to `IHtmlContent` are converted to a string by `ToString` and encoded before they're rendered.</span></span>

```cshtml
@("<span>Hello World</span>")
```

<span data-ttu-id="27f95-161">El código representa el código HTML siguiente:</span><span class="sxs-lookup"><span data-stu-id="27f95-161">The code renders the following HTML:</span></span>

```html
&lt;span&gt;Hello World&lt;/span&gt;
```

<span data-ttu-id="27f95-162">El código HTML se muestra en el explorador como:</span><span class="sxs-lookup"><span data-stu-id="27f95-162">The HTML is shown in the browser as:</span></span>

```
<span>Hello World</span>
```

<span data-ttu-id="27f95-163">`HtmlHelper.Raw`salida no está codificada pero representar como marca HTML.</span><span class="sxs-lookup"><span data-stu-id="27f95-163">`HtmlHelper.Raw` output isn't encoded but rendered as HTML markup.</span></span>

> [!WARNING]
> <span data-ttu-id="27f95-164">Usar `HtmlHelper.Raw` unsanitized usuario de la entrada es un riesgo de seguridad.</span><span class="sxs-lookup"><span data-stu-id="27f95-164">Using `HtmlHelper.Raw` on unsanitized user input is a security risk.</span></span> <span data-ttu-id="27f95-165">Proporcionados por el usuario podrían contener código JavaScript malintencionado o seguridad de otras maneras.</span><span class="sxs-lookup"><span data-stu-id="27f95-165">User input might contain malicious JavaScript or other exploits.</span></span> <span data-ttu-id="27f95-166">Es difícil inmunizar proporcionados por el usuario.</span><span class="sxs-lookup"><span data-stu-id="27f95-166">Sanitizing user input is difficult.</span></span> <span data-ttu-id="27f95-167">Evite el uso de `HtmlHelper.Raw` con proporcionados por el usuario.</span><span class="sxs-lookup"><span data-stu-id="27f95-167">Avoid using `HtmlHelper.Raw` with user input.</span></span>

```cshtml
@Html.Raw("<span>Hello World</span>")
```

<span data-ttu-id="27f95-168">El código representa el código HTML siguiente:</span><span class="sxs-lookup"><span data-stu-id="27f95-168">The code renders the following HTML:</span></span>

```html
<span>Hello World</span>
```

## <a name="razor-code-blocks"></a><span data-ttu-id="27f95-169">Bloques de código Razor</span><span class="sxs-lookup"><span data-stu-id="27f95-169">Razor code blocks</span></span>

<span data-ttu-id="27f95-170">Bloques de código Razor iniciar con `@` y encerradas entre `{}`.</span><span class="sxs-lookup"><span data-stu-id="27f95-170">Razor code blocks start with `@` and are enclosed by `{}`.</span></span> <span data-ttu-id="27f95-171">A diferencia de las expresiones, no representa el código de C# dentro de bloques de código.</span><span class="sxs-lookup"><span data-stu-id="27f95-171">Unlike expressions, C# code inside code blocks isn't rendered.</span></span> <span data-ttu-id="27f95-172">Bloques de código y las expresiones de una vista comparten el mismo ámbito y están definidas en orden:</span><span class="sxs-lookup"><span data-stu-id="27f95-172">Code blocks and expressions in a view share the same scope and are defined in order:</span></span>

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

<span data-ttu-id="27f95-173">El código representa el código HTML siguiente:</span><span class="sxs-lookup"><span data-stu-id="27f95-173">The code renders the following HTML:</span></span>

```html
<p>The future depends on what you do today. - Mahatma Gandhi</p>
<p>Hate cannot drive out hate, only love can do that. - Martin Luther King, Jr.</p>
```

### <a name="implicit-transitions"></a><span data-ttu-id="27f95-174">Transiciones implícita</span><span class="sxs-lookup"><span data-stu-id="27f95-174">Implicit transitions</span></span>

<span data-ttu-id="27f95-175">Es el idioma predeterminado en un bloque de código C#, pero la página de Razor puede realizar la transición a HTML:</span><span class="sxs-lookup"><span data-stu-id="27f95-175">The default language in a code block is C#, but the Razor Page can transition back to HTML:</span></span>

```cshtml
@{
    var inCSharp = true;
    <p>Now in HTML, was in C# @inCSharp</p>
}
```

### <a name="explicit-delimited-transition"></a><span data-ttu-id="27f95-176">Transición delimitado explícita</span><span class="sxs-lookup"><span data-stu-id="27f95-176">Explicit delimited transition</span></span>

<span data-ttu-id="27f95-177">Para definir una subsección de un bloque de código que debería presentar HTML, rodear los caracteres para la representación con el código Razor  **\<texto >** etiqueta:</span><span class="sxs-lookup"><span data-stu-id="27f95-177">To define a subsection of a code block that should render HTML, surround the characters for rendering with the Razor **\<text>** tag:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    <text>Name: @person.Name</text>
}
```

<span data-ttu-id="27f95-178">Utilice este enfoque para representar HTML que no está rodeado por una etiqueta HTML.</span><span class="sxs-lookup"><span data-stu-id="27f95-178">Use this approach to render HTML that isn't surrounded by an HTML tag.</span></span> <span data-ttu-id="27f95-179">Sin una etiqueta HTML o Razor, se produce un error de tiempo de ejecución de Razor.</span><span class="sxs-lookup"><span data-stu-id="27f95-179">Without an HTML or Razor tag, a Razor runtime error occurs.</span></span>

<span data-ttu-id="27f95-180">El  **\<texto >** etiqueta es útil para controlar el espacio en blanco al representar el contenido:</span><span class="sxs-lookup"><span data-stu-id="27f95-180">The **\<text>** tag is useful to control whitespace when rendering content:</span></span>

* <span data-ttu-id="27f95-181">Solo el contenido entre el  **\<texto >** se representa la etiqueta.</span><span class="sxs-lookup"><span data-stu-id="27f95-181">Only the content between the **\<text>** tag is rendered.</span></span> 
* <span data-ttu-id="27f95-182">No hay espacio en blanco antes o después de la  **\<texto >** la etiqueta aparece en la salida HTML.</span><span class="sxs-lookup"><span data-stu-id="27f95-182">No whitespace before or after the **\<text>** tag appears in the HTML output.</span></span>

### <a name="explicit-line-transition-with-"></a><span data-ttu-id="27f95-183">Transición de línea explícita con @:</span><span class="sxs-lookup"><span data-stu-id="27f95-183">Explicit Line Transition with @:</span></span>

<span data-ttu-id="27f95-184">Para representar el resto de una línea completa como HTML dentro de un bloque de código, use la `@:` sintaxis:</span><span class="sxs-lookup"><span data-stu-id="27f95-184">To render the rest of an entire line as HTML inside a code block, use the `@:` syntax:</span></span>

```cshtml
@for (var i = 0; i < people.Length; i++)
{
    var person = people[i];
    @:Name: @person.Name
}
```

<span data-ttu-id="27f95-185">Sin el `@:` en el código, se genera un error de tiempo de ejecución de Razor.</span><span class="sxs-lookup"><span data-stu-id="27f95-185">Without the `@:` in the code,  a Razor runtime error is generated.</span></span>

<span data-ttu-id="27f95-186">Advertencia: Adicional `@` caracteres en un archivo Razor pueden producir errores del compilador causa en instrucciones más adelante en el bloque.</span><span class="sxs-lookup"><span data-stu-id="27f95-186">Warning: Extra `@` characters in a Razor file can cause  cause compiler errors at statements later in the block.</span></span> <span data-ttu-id="27f95-187">Estos errores del compilador pueden ser difíciles de entender porque el error real se produce antes del error notificado.</span><span class="sxs-lookup"><span data-stu-id="27f95-187">These compiler errors can be difficult to understand because the actual error occurs before the reported error.</span></span>  <span data-ttu-id="27f95-188">Este error es habitual después de combinar varias expresiones implícito o explícito en un único bloque de código.</span><span class="sxs-lookup"><span data-stu-id="27f95-188">This error is common after combining multiple implicit/explicit expressions into a single code block.</span></span>

## <a name="control-structures"></a><span data-ttu-id="27f95-189">Estructuras de control</span><span class="sxs-lookup"><span data-stu-id="27f95-189">Control Structures</span></span>

<span data-ttu-id="27f95-190">Estructuras de control son una extensión de bloques de código.</span><span class="sxs-lookup"><span data-stu-id="27f95-190">Control structures are an extension of code blocks.</span></span> <span data-ttu-id="27f95-191">Todos los aspectos de bloques de código (transición a marcado, C# en línea) también se aplican a las estructuras siguientes:</span><span class="sxs-lookup"><span data-stu-id="27f95-191">All aspects of code blocks (transitioning to markup, inline C#) also apply to the following structures:</span></span>

### <a name="conditionals-if-else-if-else-and-switch"></a><span data-ttu-id="27f95-192">Instrucciones condicionales @if, else if, else, y@switch</span><span class="sxs-lookup"><span data-stu-id="27f95-192">Conditionals @if, else if, else, and @switch</span></span>

<span data-ttu-id="27f95-193">`@if`controles cuando se ejecuta el código:</span><span class="sxs-lookup"><span data-stu-id="27f95-193">`@if` controls when code runs:</span></span>

```cshtml
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

<span data-ttu-id="27f95-194">`else`y `else if` no requieren la `@` símbolo:</span><span class="sxs-lookup"><span data-stu-id="27f95-194">`else` and `else if` don't require the `@` symbol:</span></span>

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

<span data-ttu-id="27f95-195">El marcado siguiente muestra cómo utilizar una instrucción switch:</span><span class="sxs-lookup"><span data-stu-id="27f95-195">The following markup shows how to use a switch statement:</span></span>

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

### <a name="looping-for-foreach-while-and-do-while"></a><span data-ttu-id="27f95-196">Bucle @for, @foreach, @while, y @do mientras</span><span class="sxs-lookup"><span data-stu-id="27f95-196">Looping @for, @foreach, @while, and @do while</span></span>

<span data-ttu-id="27f95-197">HTML con plantilla se puede representar con las instrucciones de control de bucle.</span><span class="sxs-lookup"><span data-stu-id="27f95-197">Templated HTML can be rendered with looping control statements.</span></span>  <span data-ttu-id="27f95-198">Para presentar una lista de personas:</span><span class="sxs-lookup"><span data-stu-id="27f95-198">To render a list of people:</span></span>

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

<span data-ttu-id="27f95-199">Se admiten las siguientes instrucciones bucles:</span><span class="sxs-lookup"><span data-stu-id="27f95-199">The following looping statements are supported:</span></span>

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

### <a name="compound-using"></a><span data-ttu-id="27f95-200">Compuesta@using</span><span class="sxs-lookup"><span data-stu-id="27f95-200">Compound @using</span></span>

<span data-ttu-id="27f95-201">En C#, un `using` instrucción se utiliza para asegurarse de que se elimina un objeto.</span><span class="sxs-lookup"><span data-stu-id="27f95-201">In C#, a `using` statement is used to ensure an object is disposed.</span></span> <span data-ttu-id="27f95-202">En Razor, el mismo mecanismo se utiliza para crear aplicaciones auxiliares de HTML que contienen contenido adicional.</span><span class="sxs-lookup"><span data-stu-id="27f95-202">In Razor, the same mechanism is used to create HTML Helpers that contain additional content.</span></span> <span data-ttu-id="27f95-203">En el código siguiente, las aplicaciones auxiliares HTML presentar una etiqueta de formulario con el `@using` instrucción:</span><span class="sxs-lookup"><span data-stu-id="27f95-203">In the following code, HTML Helpers render a form tag with the `@using` statement:</span></span>


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

<span data-ttu-id="27f95-204">Se pueden realizar acciones de nivel de ámbito con [aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="27f95-204">Scope-level actions can be performed with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

### <a name="try-catch-finally"></a><span data-ttu-id="27f95-205">@try, catch y finally</span><span class="sxs-lookup"><span data-stu-id="27f95-205">@try, catch, finally</span></span>

<span data-ttu-id="27f95-206">Control de excepciones es similar a C#:</span><span class="sxs-lookup"><span data-stu-id="27f95-206">Exception handling is similar to C#:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact7.cshtml)]

### <a name="lock"></a>@lock

<span data-ttu-id="27f95-207">Razor tiene la capacidad de proteger las secciones críticas con instrucciones de bloqueo:</span><span class="sxs-lookup"><span data-stu-id="27f95-207">Razor has the capability to protect critical sections with lock statements:</span></span>

```cshtml
@lock (SomeLock)
{
    // Do critical section work
}
```

### <a name="comments"></a><span data-ttu-id="27f95-208">Comentarios</span><span class="sxs-lookup"><span data-stu-id="27f95-208">Comments</span></span>

<span data-ttu-id="27f95-209">Razor admite comentarios de C# y el código HTML:</span><span class="sxs-lookup"><span data-stu-id="27f95-209">Razor supports C# and HTML comments:</span></span>

```cshtml
@{
    /* C# comment */
    // Another C# comment
}
<!-- HTML comment -->
```

<span data-ttu-id="27f95-210">El código representa el código HTML siguiente:</span><span class="sxs-lookup"><span data-stu-id="27f95-210">The code renders the following HTML:</span></span>

```html
<!-- HTML comment -->
```

<span data-ttu-id="27f95-211">Comentarios de Razor se quitan mediante el servidor antes de presenta la página Web.</span><span class="sxs-lookup"><span data-stu-id="27f95-211">Razor comments are removed by the server before the webpage is rendered.</span></span> <span data-ttu-id="27f95-212">Razor usa `@*  *@` para delimitar los comentarios.</span><span class="sxs-lookup"><span data-stu-id="27f95-212">Razor uses `@*  *@` to delimit comments.</span></span> <span data-ttu-id="27f95-213">El código siguiente se hace referencia a, por lo que el servidor no representa ningún otro marcado:</span><span class="sxs-lookup"><span data-stu-id="27f95-213">The following code is commented out, so the server doesn't render any markup:</span></span>

```cshtml
@*
    @{
        /* C# comment */
        // Another C# comment
    }
    <!-- HTML comment -->
*@
```

## <a name="directives"></a><span data-ttu-id="27f95-214">Directivas</span><span class="sxs-lookup"><span data-stu-id="27f95-214">Directives</span></span>

<span data-ttu-id="27f95-215">Directivas de Razor se representan mediante expresiones implícitas con las siguientes palabras clave reservadas el `@` símbolos.</span><span class="sxs-lookup"><span data-stu-id="27f95-215">Razor directives are represented by implicit expressions with reserved keywords following the `@` symbol.</span></span> <span data-ttu-id="27f95-216">Normalmente, una directiva cambia la forma en que una vista se analiza o habilita una funcionalidad diferente.</span><span class="sxs-lookup"><span data-stu-id="27f95-216">A directive typically changes the way a view is parsed or enables different functionality.</span></span>

<span data-ttu-id="27f95-217">Descripción de cómo Razor genera código para una vista resulta más fácil comprender cómo funcionan las directivas.</span><span class="sxs-lookup"><span data-stu-id="27f95-217">Understanding how Razor generates code for a view makes it easier to understand how directives work.</span></span>

[!code-html[Main](razor/sample/Views/Home/Contact8.cshtml)]

<span data-ttu-id="27f95-218">El código genera una clase similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="27f95-218">The code generates a class similar to the following:</span></span>

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

<span data-ttu-id="27f95-219">Más adelante en este artículo, la sección [ver la clase de C# de Razor generada por una vista](#viewing-the-razor-c-class-generated-for-a-view) explica cómo ver esta clase generada.</span><span class="sxs-lookup"><span data-stu-id="27f95-219">Later in this article, the section [Viewing the Razor C# class generated for a view](#viewing-the-razor-c-class-generated-for-a-view) explains how to view this generated class.</span></span>

### <a name="using"></a>@using

<span data-ttu-id="27f95-220">El `@using` directiva agrega C# `using` la directiva a la vista generada:</span><span class="sxs-lookup"><span data-stu-id="27f95-220">The `@using` directive adds the C# `using` directive to the generated view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact9.cshtml)]

### <a name="model"></a>@model

<span data-ttu-id="27f95-221">El `@model` directiva especifica el tipo del modelo que se pasan a una vista:</span><span class="sxs-lookup"><span data-stu-id="27f95-221">The `@model` directive specifies the type of the model passed to a view:</span></span>

```cshtml
@model TypeNameOfModel
```

<span data-ttu-id="27f95-222">En una aplicación de MVC de ASP.NET Core creada con cuentas de usuario individuales, el *Views/Account/Login.cshtml* vista contiene la siguiente declaración de modelo:</span><span class="sxs-lookup"><span data-stu-id="27f95-222">In an ASP.NET Core MVC app created with individual user accounts, the *Views/Account/Login.cshtml* view contains the following model declaration:</span></span>

```cshtml
@model LoginViewModel
```

<span data-ttu-id="27f95-223">La clase generada se hereda de `RazorPage<dynamic>`:</span><span class="sxs-lookup"><span data-stu-id="27f95-223">The class generated inherits from `RazorPage<dynamic>`:</span></span>

```csharp
public class _Views_Account_Login_cshtml : RazorPage<LoginViewModel>
```

<span data-ttu-id="27f95-224">Razor expone un `Model` propiedad para tener acceso al modelo que se pasa a la vista:</span><span class="sxs-lookup"><span data-stu-id="27f95-224">Razor exposes a `Model` property for accessing the model passed to the view:</span></span>

```cshtml
<div>The Login Email: @Model.Email</div>
```

<span data-ttu-id="27f95-225">El `@model` directiva especifica el tipo de esta propiedad.</span><span class="sxs-lookup"><span data-stu-id="27f95-225">The `@model` directive specifies the type of this property.</span></span> <span data-ttu-id="27f95-226">La directiva especifica la `T` en `RazorPage<T>` que generado clase que deriva de la vista de.</span><span class="sxs-lookup"><span data-stu-id="27f95-226">The directive specifies the `T` in `RazorPage<T>` that the generated class that the view derives from.</span></span> <span data-ttu-id="27f95-227">Si el `@model` iisn't directiva se especifica, el `Model` propiedad es de tipo `dynamic`.</span><span class="sxs-lookup"><span data-stu-id="27f95-227">If  the `@model` directive iisn't specified, the `Model` property is of type `dynamic`.</span></span> <span data-ttu-id="27f95-228">El valor del modelo se pasa desde el controlador a la vista.</span><span class="sxs-lookup"><span data-stu-id="27f95-228">The value of the model is passed from the controller to the view.</span></span> <span data-ttu-id="27f95-229">Para obtener más información, consulte [fuertemente tipadas modelos y la @model (palabra clave).</span><span class="sxs-lookup"><span data-stu-id="27f95-229">For more information, see [Strongly typed models and the @model keyword.</span></span>

### <a name="inherits"></a>@inherits

<span data-ttu-id="27f95-230">El `@inherits` directiva proporciona el control completo de la clase que hereda de la vista:</span><span class="sxs-lookup"><span data-stu-id="27f95-230">The `@inherits` directive provides  full control of the class the view inherits:</span></span>

```cshtml
@inherits TypeNameOfClassToInheritFrom
```

<span data-ttu-id="27f95-231">El código siguiente es un tipo de página de Razor personalizado:</span><span class="sxs-lookup"><span data-stu-id="27f95-231">The following code is a custom Razor page type:</span></span>

[!code-csharp[Main](razor/sample/Classes/CustomRazorPage.cs)]

<span data-ttu-id="27f95-232">El `CustomText` se muestran en una vista:</span><span class="sxs-lookup"><span data-stu-id="27f95-232">The `CustomText` is displayed in a view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact10.cshtml)]

<span data-ttu-id="27f95-233">El código representa el código HTML siguiente:</span><span class="sxs-lookup"><span data-stu-id="27f95-233">The code renders the following HTML:</span></span>

```html
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

 <span data-ttu-id="27f95-234">`@model`y `@inherits` puede utilizarse en la misma vista.</span><span class="sxs-lookup"><span data-stu-id="27f95-234">`@model` and `@inherits` can be used in the same view.</span></span>  <span data-ttu-id="27f95-235">`@inherits`puede estar en un *_ViewImports.cshtml* archivo que se importa de la vista:</span><span class="sxs-lookup"><span data-stu-id="27f95-235">`@inherits` can be in a *_ViewImports.cshtml* file that the view imports:</span></span>

[!code-cshtml[Main](razor/sample/Views/_ViewImportsModel.cshtml)]

<span data-ttu-id="27f95-236">El código siguiente es un ejemplo de una vista fuertemente tipada:</span><span class="sxs-lookup"><span data-stu-id="27f95-236">The following code is an example of a strongly-typed view:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Login1.cshtml)]

<span data-ttu-id="27f95-237">Si "rick@contoso.com" se pasa en el modelo, la vista genera el siguiente marcado HTML:</span><span class="sxs-lookup"><span data-stu-id="27f95-237">If "rick@contoso.com" is passed in the model, the view generates the following HTML markup:</span></span>

```html
<div>The Login Email: rick@contoso.com</div>
<div>Custom text: Gardyloo! - A Scottish warning yelled from a window before dumping a slop bucket on the street below.</div>
```

### <a name="inject"></a>@inject


<span data-ttu-id="27f95-238">El `@inject` directiva permite a la página de Razor insertar un servicio desde el [contenedor de servicios](xref:fundamentals/dependency-injection) en una vista.</span><span class="sxs-lookup"><span data-stu-id="27f95-238">The `@inject` directive enables the Razor Page to inject a service from the [service container](xref:fundamentals/dependency-injection) into a view.</span></span> <span data-ttu-id="27f95-239">Para obtener más información, consulte [inyección de dependencia en las vistas](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="27f95-239">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

### <a name="functions"></a>@functions

<span data-ttu-id="27f95-240">El `@functions` directiva permite que una página de Razor agregar contenido de nivel de función a una vista:</span><span class="sxs-lookup"><span data-stu-id="27f95-240">The `@functions` directive enables a Razor Page to add function-level content to a view:</span></span>

```cshtml
@functions { // C# Code }
```

<span data-ttu-id="27f95-241">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="27f95-241">For example:</span></span>

[!code-cshtml[Main](razor/sample/Views/Home/Contact6.cshtml)]

<span data-ttu-id="27f95-242">El código genera el siguiente marcado HTML:</span><span class="sxs-lookup"><span data-stu-id="27f95-242">The code generates the following HTML markup:</span></span>

```html
<div>From method: Hello</div>
```

<span data-ttu-id="27f95-243">El código siguiente es la clase generada de Razor C#:</span><span class="sxs-lookup"><span data-stu-id="27f95-243">The following code is the generated Razor C# class:</span></span>

[!code-csharp[Main](razor/sample/Classes/Views_Home_Test_cshtml.cs?range=1-19)]

### <a name="section"></a>@section

<span data-ttu-id="27f95-244">El `@section` directiva se usa junto con el [diseño](xref:mvc/views/layout) para habilitar las vistas representar el contenido en diferentes partes de la página HTML.</span><span class="sxs-lookup"><span data-stu-id="27f95-244">The `@section` directive is used in conjunction with the [layout](xref:mvc/views/layout) to enable views to render content in different parts of the HTML page.</span></span> <span data-ttu-id="27f95-245">Para obtener más información, consulte [secciones](xref:mvc/views/layout#layout-sections-label).</span><span class="sxs-lookup"><span data-stu-id="27f95-245">For more information, see [Sections](xref:mvc/views/layout#layout-sections-label).</span></span>

## <a name="tag-helpers"></a><span data-ttu-id="27f95-246">Aplicaciones auxiliares de etiquetas</span><span class="sxs-lookup"><span data-stu-id="27f95-246">Tag Helpers</span></span>

<span data-ttu-id="27f95-247">Hay tres directivas que pertenecen a [aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="27f95-247">There are three directives that pertain to [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

| <span data-ttu-id="27f95-248">Directiva</span><span class="sxs-lookup"><span data-stu-id="27f95-248">Directive</span></span> | <span data-ttu-id="27f95-249">Función</span><span class="sxs-lookup"><span data-stu-id="27f95-249">Function</span></span> |
| --------- | -------- |
| [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) | <span data-ttu-id="27f95-250">Pone a disposición a una vista de aplicaciones auxiliares de etiquetas.</span><span class="sxs-lookup"><span data-stu-id="27f95-250">Makes Tag Helpers available to a view.</span></span> |
| [@removeTagHelper](xref:mvc/views/tag-helpers/intro#remove-razor-directives-label) | <span data-ttu-id="27f95-251">Quita las aplicaciones auxiliares de etiquetas que agregó anteriormente desde una vista.</span><span class="sxs-lookup"><span data-stu-id="27f95-251">Removes Tag Helpers previously added from a view.</span></span> |
| [@tagHelperPrefix](xref:mvc/views/tag-helpers/intro#prefix-razor-directives-label) | <span data-ttu-id="27f95-252">Especifica un prefijo de etiqueta para habilitar la compatibilidad de la aplicación auxiliar de etiqueta y hacer uso de la aplicación auxiliar de etiqueta explícita.</span><span class="sxs-lookup"><span data-stu-id="27f95-252">Specifies a tag prefix to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> |

## <a name="razor-reserved-keywords"></a><span data-ttu-id="27f95-253">Palabras clave reservada de Razor</span><span class="sxs-lookup"><span data-stu-id="27f95-253">Razor reserved keywords</span></span>

### <a name="razor-keywords"></a><span data-ttu-id="27f95-254">Palabras clave de Razor</span><span class="sxs-lookup"><span data-stu-id="27f95-254">Razor keywords</span></span>

* <span data-ttu-id="27f95-255">página (requiere el núcleo ASP.NET 2.0 y versiones posterior)</span><span class="sxs-lookup"><span data-stu-id="27f95-255">page (Requires ASP.NET Core 2.0 and later)</span></span>
* <span data-ttu-id="27f95-256">funciones</span><span class="sxs-lookup"><span data-stu-id="27f95-256">functions</span></span>
* <span data-ttu-id="27f95-257">hereda</span><span class="sxs-lookup"><span data-stu-id="27f95-257">inherits</span></span>
* <span data-ttu-id="27f95-258">modelo</span><span class="sxs-lookup"><span data-stu-id="27f95-258">model</span></span>
* <span data-ttu-id="27f95-259">section</span><span class="sxs-lookup"><span data-stu-id="27f95-259">section</span></span>
* <span data-ttu-id="27f95-260">aplicación auxiliar (actualmente no admitida ASP.NET Core)</span><span class="sxs-lookup"><span data-stu-id="27f95-260">helper (Not currently supported by ASP.NET Core)</span></span>

<span data-ttu-id="27f95-261">Palabras clave de Razor se escapan con `@(Razor Keyword)` (por ejemplo, `@(functions)`).</span><span class="sxs-lookup"><span data-stu-id="27f95-261">Razor keywords are escaped with `@(Razor Keyword)` (for example, `@(functions)`).</span></span>

### <a name="c-razor-keywords"></a><span data-ttu-id="27f95-262">Palabras clave de C# Razor</span><span class="sxs-lookup"><span data-stu-id="27f95-262">C# Razor keywords</span></span>

* <span data-ttu-id="27f95-263">case</span><span class="sxs-lookup"><span data-stu-id="27f95-263">case</span></span>
* <span data-ttu-id="27f95-264">do</span><span class="sxs-lookup"><span data-stu-id="27f95-264">do</span></span>
* <span data-ttu-id="27f95-265">default</span><span class="sxs-lookup"><span data-stu-id="27f95-265">default</span></span>
* <span data-ttu-id="27f95-266">for</span><span class="sxs-lookup"><span data-stu-id="27f95-266">for</span></span>
* <span data-ttu-id="27f95-267">foreach</span><span class="sxs-lookup"><span data-stu-id="27f95-267">foreach</span></span>
* <span data-ttu-id="27f95-268">if</span><span class="sxs-lookup"><span data-stu-id="27f95-268">if</span></span>
* <span data-ttu-id="27f95-269">else</span><span class="sxs-lookup"><span data-stu-id="27f95-269">else</span></span>
* <span data-ttu-id="27f95-270">bloquear</span><span class="sxs-lookup"><span data-stu-id="27f95-270">lock</span></span>
* <span data-ttu-id="27f95-271">switch</span><span class="sxs-lookup"><span data-stu-id="27f95-271">switch</span></span>
* <span data-ttu-id="27f95-272">try</span><span class="sxs-lookup"><span data-stu-id="27f95-272">try</span></span>
* <span data-ttu-id="27f95-273">catch</span><span class="sxs-lookup"><span data-stu-id="27f95-273">catch</span></span>
* <span data-ttu-id="27f95-274">finally</span><span class="sxs-lookup"><span data-stu-id="27f95-274">finally</span></span>
* <span data-ttu-id="27f95-275">utilizar</span><span class="sxs-lookup"><span data-stu-id="27f95-275">using</span></span>
* <span data-ttu-id="27f95-276">while</span><span class="sxs-lookup"><span data-stu-id="27f95-276">while</span></span>

<span data-ttu-id="27f95-277">Palabras clave de C# Razor debe ser un carácter de escape doble, con `@(@C# Razor Keyword)` (por ejemplo, `@(@case)`).</span><span class="sxs-lookup"><span data-stu-id="27f95-277">C# Razor keywords must be double-escaped with `@(@C# Razor Keyword)` (for example, `@(@case)`).</span></span> <span data-ttu-id="27f95-278">La primera `@` antepone el analizador Razor.</span><span class="sxs-lookup"><span data-stu-id="27f95-278">The first `@` escapes the Razor parser.</span></span> <span data-ttu-id="27f95-279">El segundo `@` antepone el analizador de C#.</span><span class="sxs-lookup"><span data-stu-id="27f95-279">The second `@` escapes the C# parser.</span></span>

### <a name="reserved-keywords-not-used-by-razor"></a><span data-ttu-id="27f95-280">Palabras clave reservadas no utilizadas Razor</span><span class="sxs-lookup"><span data-stu-id="27f95-280">Reserved keywords not used by Razor</span></span>

* <span data-ttu-id="27f95-281">namespace</span><span class="sxs-lookup"><span data-stu-id="27f95-281">namespace</span></span>
* <span data-ttu-id="27f95-282">clase</span><span class="sxs-lookup"><span data-stu-id="27f95-282">class</span></span>

## <a name="viewing-the-razor-c-class-generated-for-a-view"></a><span data-ttu-id="27f95-283">Visualización de la clase de C# de Razor generada por una vista</span><span class="sxs-lookup"><span data-stu-id="27f95-283">Viewing the Razor C# class generated for a view</span></span>

<span data-ttu-id="27f95-284">Agregue la siguiente clase al proyecto de MVC de ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="27f95-284">Add the following class to the ASP.NET Core MVC project:</span></span>

[!code-csharp[Main](razor/sample/Utilities/CustomTemplateEngine.cs)]

<span data-ttu-id="27f95-285">Invalidar el `RazorTemplateEngine` agregado MVC con la `CustomTemplateEngine` clase:</span><span class="sxs-lookup"><span data-stu-id="27f95-285">Override the `RazorTemplateEngine` added by MVC with the `CustomTemplateEngine` class:</span></span>

[!code-csharp[Main](razor/sample/Startup.cs?highlight=4&range=10-14)]

<span data-ttu-id="27f95-286">Establecer un punto de interrupción en la `return csharpDocument` instrucción de `CustomTemplateEngine`.</span><span class="sxs-lookup"><span data-stu-id="27f95-286">Set a break point on the `return csharpDocument` statement of `CustomTemplateEngine`.</span></span> <span data-ttu-id="27f95-287">Cuando la ejecución del programa se detiene en el punto de interrupción, ver el valor de `generatedCode`.</span><span class="sxs-lookup"><span data-stu-id="27f95-287">When program execution stops at the break point, view the value of `generatedCode`.</span></span>

![Vista de generatedCode visualizador de texto](razor/_static/tvr.png)

## <a name="view-lookups-and-case-sensitivity"></a><span data-ttu-id="27f95-289">Búsquedas de vista y entre mayúsculas y minúsculas</span><span class="sxs-lookup"><span data-stu-id="27f95-289">View lookups and case sensitivity</span></span>

<span data-ttu-id="27f95-290">El motor de vista Razor realiza búsquedas entre mayúsculas y minúsculas para las vistas.</span><span class="sxs-lookup"><span data-stu-id="27f95-290">The Razor view engine performs case-sensitive lookups for views.</span></span> <span data-ttu-id="27f95-291">Sin embargo, la búsqueda real se determina por el sistema de archivos subyacente:</span><span class="sxs-lookup"><span data-stu-id="27f95-291">However, the actual lookup is determined by the underlying file system:</span></span>

* <span data-ttu-id="27f95-292">En función del origen del archivo:</span><span class="sxs-lookup"><span data-stu-id="27f95-292">File based source:</span></span> 
  * <span data-ttu-id="27f95-293">En sistemas operativos con sistemas de archivos entre mayúsculas y minúsculas (por ejemplo, Windows), búsquedas de proveedor de archivo físico distinguen entre mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="27f95-293">On operating systems with case insensitive file systems (for example, Windows), physical file provider lookups are case insensitive.</span></span> <span data-ttu-id="27f95-294">Por ejemplo, `return View("Test")` da como resultado que coincidan con la */Views/Home/Test.cshtml*, */Views/home/test.cshtml*y cualquier otra variante de mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="27f95-294">For example, `return View("Test")` results in matches for */Views/Home/Test.cshtml*, */Views/home/test.cshtml*, and any other casing variant.</span></span>
  * <span data-ttu-id="27f95-295">En sistemas de archivos entre mayúsculas y minúsculas (por ejemplo, Linux y OSX y con `EmbeddedFileProvider`), las búsquedas distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="27f95-295">On case-sensitive file systems (for example, Linux, OSX, and with `EmbeddedFileProvider`), lookups are case-sensitive.</span></span> <span data-ttu-id="27f95-296">Por ejemplo, `return View("Test")` específicamente coincidencias */Views/Home/Test.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="27f95-296">For example, `return View("Test")` specifically matches */Views/Home/Test.cshtml*.</span></span>
* <span data-ttu-id="27f95-297">Precompilado vistas: con núcleo ASP.NET 2.0 y versiones posterior, buscar vistas precompiladas distingue mayúsculas de minúsculas en todos los sistemas operativos.</span><span class="sxs-lookup"><span data-stu-id="27f95-297">Precompiled views: With ASP.NET Core 2.0 and later, looking up precompiled views is case insensitive on all operating systems.</span></span> <span data-ttu-id="27f95-298">El comportamiento es idéntico al comportamiento del proveedor del archivo físico en Windows.</span><span class="sxs-lookup"><span data-stu-id="27f95-298">The behavior is identical to physical file provider's behavior on Windows.</span></span> <span data-ttu-id="27f95-299">Si dos vistas precompiladas difieren solo en caso de que el resultado de búsqueda es no determinista.</span><span class="sxs-lookup"><span data-stu-id="27f95-299">If two precompiled views differ only in case, the result of lookup is non-deterministic.</span></span>

<span data-ttu-id="27f95-300">Los desarrolladores pueden hacer coincidir las mayúsculas y minúsculas de los nombres de archivo y directorio para las mayúsculas y minúsculas de:</span><span class="sxs-lookup"><span data-stu-id="27f95-300">Developers are encouraged to match the casing of file and directory names to the casing of:</span></span>

    * <span data-ttu-id="27f95-301">Nombres de área, acción y controlador.</span><span class="sxs-lookup"><span data-stu-id="27f95-301">Area, controller, and action names.</span></span> 
    * <span data-ttu-id="27f95-302">Páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="27f95-302">Razor Pages.</span></span>
    
<span data-ttu-id="27f95-303">Coincidencia de mayúsculas garantiza que las implementaciones encontrar sus vistas sin tener en cuenta el sistema de archivos subyacente.</span><span class="sxs-lookup"><span data-stu-id="27f95-303">Matching case ensures the deployments find their views regardless of the underlying file system.</span></span>
