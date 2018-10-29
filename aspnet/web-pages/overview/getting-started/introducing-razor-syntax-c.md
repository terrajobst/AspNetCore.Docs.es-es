---
uid: web-pages/overview/getting-started/introducing-razor-syntax-c
title: Introducción a la programación Web de ASP.NET mediante la sintaxis Razor (C#) | Microsoft Docs
author: tfitzmac
description: Este capítulo ofrece una visión general de la programación con ASP.NET Web Pages usando la sintaxis Razor. ASP.NET es la tecnología de Microsoft para la ejecución de pa de web dinámico...
ms.author: riande
ms.date: 02/07/2014
ms.assetid: aa67d304-583b-4bf8-a231-195656cfb587
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-c
msc.type: authoredcontent
ms.openlocfilehash: 347e5ddbc02866887d3f422ecc291e5e3dfacaaf
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207919"
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-c"></a><span data-ttu-id="38e99-104">Introducción a la programación Web de ASP.NET mediante la sintaxis Razor (C#)</span><span class="sxs-lookup"><span data-stu-id="38e99-104">Introduction to ASP.NET Web Programming Using the Razor Syntax (C#)</span></span>
====================
<span data-ttu-id="38e99-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="38e99-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="38e99-106">En este artículo ofrece una visión general de la programación con ASP.NET Web Pages usando la sintaxis Razor.</span><span class="sxs-lookup"><span data-stu-id="38e99-106">This article gives you an overview of programming with ASP.NET Web Pages using the Razor syntax.</span></span> <span data-ttu-id="38e99-107">ASP.NET es la tecnología de Microsoft para la ejecución de páginas web dinámicas en servidores web.</span><span class="sxs-lookup"><span data-stu-id="38e99-107">ASP.NET is Microsoft's technology for running dynamic web pages on web servers.</span></span> <span data-ttu-id="38e99-108">Este focos de artículos sobre el uso del lenguaje de programación de C#.</span><span class="sxs-lookup"><span data-stu-id="38e99-108">This articles focuses on using the C# programming language.</span></span>
> 
> <span data-ttu-id="38e99-109">**Aprenderá lo**:</span><span class="sxs-lookup"><span data-stu-id="38e99-109">**What you'll learn**:</span></span>
> 
> - <span data-ttu-id="38e99-110">El 8 de principales sugerencias de introducción a ASP.NET Web Pages con sintaxis Razor de programación de programación.</span><span class="sxs-lookup"><span data-stu-id="38e99-110">The top 8 programming tips for getting started with programming ASP.NET Web Pages using Razor syntax.</span></span>
> - <span data-ttu-id="38e99-111">Conceptos básicos de programación que necesitará.</span><span class="sxs-lookup"><span data-stu-id="38e99-111">Basic programming concepts you'll need.</span></span>
> - <span data-ttu-id="38e99-112">¿Qué código de servidor ASP.NET y la sintaxis Razor es todo acerca de.</span><span class="sxs-lookup"><span data-stu-id="38e99-112">What ASP.NET server code and the Razor syntax is all about.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="38e99-113">Versiones de software</span><span class="sxs-lookup"><span data-stu-id="38e99-113">Software versions</span></span>
> 
> 
> - <span data-ttu-id="38e99-114">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="38e99-114">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="38e99-115">Este tutorial también funciona con ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="38e99-115">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="the-top-8-programming-tips"></a><span data-ttu-id="38e99-116">El 8 principales sugerencias de programación</span><span class="sxs-lookup"><span data-stu-id="38e99-116">The Top 8 Programming Tips</span></span>

<span data-ttu-id="38e99-117">Esta sección enumeran algunas sugerencias que necesita realmente saber a medida que empiece a escribir código de servidor ASP.NET mediante la sintaxis Razor.</span><span class="sxs-lookup"><span data-stu-id="38e99-117">This section lists a few tips that you absolutely need to know as you start writing ASP.NET server code using the Razor syntax.</span></span>

> [!NOTE]
> <span data-ttu-id="38e99-118">La sintaxis de Razor se basa en el lenguaje de programación de C#, y que es el idioma que se suele usar con ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="38e99-118">The Razor syntax is based on the C# programming language, and that's the language that's used most often with ASP.NET Web Pages.</span></span> <span data-ttu-id="38e99-119">Sin embargo, la sintaxis Razor también admite el lenguaje Visual Basic y todo lo que ve que también puede hacer en Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="38e99-119">However, the Razor syntax also supports the Visual Basic language, and everything you see you can also do in Visual Basic.</span></span> <span data-ttu-id="38e99-120">Para obtener más información, consulte el apéndice [lenguaje Visual Basic y la sintaxis](https://go.microsoft.com/fwlink/?LinkId=202908).</span><span class="sxs-lookup"><span data-stu-id="38e99-120">For details, see the appendix [Visual Basic Language and Syntax](https://go.microsoft.com/fwlink/?LinkId=202908).</span></span>


<span data-ttu-id="38e99-121">Puede encontrar más detalles sobre la mayoría de estas técnicas de programación más adelante en el artículo.</span><span class="sxs-lookup"><span data-stu-id="38e99-121">You can find more details about most of these programming techniques later in the article.</span></span>

### <a name="1-you-add-code-to-a-page-using-the--character"></a><span data-ttu-id="38e99-122">1. Agregue código a una página con el carácter @</span><span class="sxs-lookup"><span data-stu-id="38e99-122">1. You add code to a page using the @ character</span></span>

<span data-ttu-id="38e99-123">El `@` carácter inicia las expresiones en línea, bloques de instrucciones único y bloques de múltiples instrucciones:</span><span class="sxs-lookup"><span data-stu-id="38e99-123">The `@` character starts inline expressions, single statement blocks, and multi-statement blocks:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample1.html)]

<span data-ttu-id="38e99-124">Este es el aspecto de estas instrucciones cuando la página se ejecuta en un explorador:</span><span class="sxs-lookup"><span data-stu-id="38e99-124">This is what these statements look like when the page runs in a browser:</span></span>

![Razor Img1](introducing-razor-syntax-c/_static/image1.jpg)

> [!TIP] 
> 
> <span data-ttu-id="38e99-126">**Codificación HTML**</span><span class="sxs-lookup"><span data-stu-id="38e99-126">**HTML Encoding**</span></span>
> 
> <span data-ttu-id="38e99-127">Al mostrar contenido en una página con el `@` de caracteres, como se muestra en los ejemplos anteriores, ASP.NET codifica en HTML el resultado.</span><span class="sxs-lookup"><span data-stu-id="38e99-127">When you display content in a page using the `@` character, as in the preceding examples, ASP.NET HTML-encodes the output.</span></span> <span data-ttu-id="38e99-128">Esto reemplaza los caracteres reservados de HTML (como `<` y `>` y `&`) con los códigos que permiten los caracteres que se va a mostrar como caracteres en una página web no se interprete como etiquetas HTML o entidades.</span><span class="sxs-lookup"><span data-stu-id="38e99-128">This replaces reserved HTML characters (such as `<` and `>` and `&`) with codes that enable the characters to be displayed as characters in a web page instead of being interpreted as HTML tags or entities.</span></span> <span data-ttu-id="38e99-129">Sin codificación HTML, la salida desde el código de servidor podría no mostrarse correctamente y podría exponer una página a riesgos de seguridad.</span><span class="sxs-lookup"><span data-stu-id="38e99-129">Without HTML encoding, the output from your server code might not display correctly, and could expose a page to security risks.</span></span>
> 
> <span data-ttu-id="38e99-130">Si su objetivo es generar marcado HTML que representa las etiquetas como marcado (por ejemplo `<p></p>` para un párrafo o `<em></em>` para resaltar el texto), consulte la sección [combinar texto, marcado y código en bloques de código](#BM_CombiningTextMarkupAndCode) más adelante en este artículo.</span><span class="sxs-lookup"><span data-stu-id="38e99-130">If your goal is to output HTML markup that renders tags as markup (for example `<p></p>` for a paragraph or `<em></em>` to emphasize text), see the section [Combining Text, Markup, and Code in Code Blocks](#BM_CombiningTextMarkupAndCode) later in this article.</span></span>
> 
> <span data-ttu-id="38e99-131">Puede leer más acerca de la codificación HTML en [trabajar con formularios](https://go.microsoft.com/fwlink/?LinkId=202892).</span><span class="sxs-lookup"><span data-stu-id="38e99-131">You can read more about HTML encoding in [Working with Forms](https://go.microsoft.com/fwlink/?LinkId=202892).</span></span>


### <a name="2-you-enclose-code-blocks-in-braces"></a><span data-ttu-id="38e99-132">2. Incluir bloques de código entre llaves</span><span class="sxs-lookup"><span data-stu-id="38e99-132">2. You enclose code blocks in braces</span></span>

<span data-ttu-id="38e99-133">Un *bloque de código* incluye una o varias instrucciones de código y aparece encerrado entre llaves.</span><span class="sxs-lookup"><span data-stu-id="38e99-133">A *code block* includes one or more code statements and is enclosed in braces.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample2.html)]

<span data-ttu-id="38e99-134">El resultado se muestra en un explorador:</span><span class="sxs-lookup"><span data-stu-id="38e99-134">The result displayed in a browser:</span></span>

![Razor Img2](introducing-razor-syntax-c/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-semicolon"></a><span data-ttu-id="38e99-136">3. Dentro de un bloque, finaliza cada instrucción de código con un punto y coma</span><span class="sxs-lookup"><span data-stu-id="38e99-136">3. Inside a block, you end each code statement with a semicolon</span></span>

<span data-ttu-id="38e99-137">Dentro de un bloque de código, cada instrucción de código completo debe terminar con un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="38e99-137">Inside a code block, each complete code statement must end with a semicolon.</span></span> <span data-ttu-id="38e99-138">Las expresiones en línea no terminen con un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="38e99-138">Inline expressions don't end with a semicolon.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample3.html)]

### <a name="4-you-use-variables-to-store-values"></a><span data-ttu-id="38e99-139">4. Usar variables para almacenar valores</span><span class="sxs-lookup"><span data-stu-id="38e99-139">4. You use variables to store values</span></span>

<span data-ttu-id="38e99-140">Puede almacenar valores en un *variable*, incluidas las cadenas, números y fechas, etcetera. Crear una nueva variable mediante el `var` palabra clave.</span><span class="sxs-lookup"><span data-stu-id="38e99-140">You can store values in a *variable*, including strings, numbers, and dates, etc. You create a new variable using the `var` keyword.</span></span> <span data-ttu-id="38e99-141">Puede insertar directamente en una página con los valores de variable `@`.</span><span class="sxs-lookup"><span data-stu-id="38e99-141">You can insert variable values directly in a page using `@`.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample4.html)]

<span data-ttu-id="38e99-142">El resultado se muestra en un explorador:</span><span class="sxs-lookup"><span data-stu-id="38e99-142">The result displayed in a browser:</span></span>

![Razor Img3](introducing-razor-syntax-c/_static/image3.jpg)

<a id="ID_StringLiterals"></a>
### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a><span data-ttu-id="38e99-144">5. Incluya los valores de cadena literal entre comillas dobles</span><span class="sxs-lookup"><span data-stu-id="38e99-144">5. You enclose literal string values in double quotation marks</span></span>

<span data-ttu-id="38e99-145">Un *cadena* es una secuencia de caracteres que se tratan como texto.</span><span class="sxs-lookup"><span data-stu-id="38e99-145">A *string* is a sequence of characters that are treated as text.</span></span> <span data-ttu-id="38e99-146">Para especificar una cadena, debe encerrarlo entre comillas dobles:</span><span class="sxs-lookup"><span data-stu-id="38e99-146">To specify a string, you enclose it in double quotation marks:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample5.cshtml)]

<span data-ttu-id="38e99-147">Si la cadena que se va a mostrar contiene un carácter de barra diagonal inversa ( `\` ) o comillas dobles ( `"` ), utilice un *literal de cadena textual* que tiene como prefijo el `@` operador.</span><span class="sxs-lookup"><span data-stu-id="38e99-147">If the string that you want to display contains a backslash character ( `\` ) or double quotation marks ( `"` ), use a *verbatim string literal* that's prefixed with the `@` operator.</span></span> <span data-ttu-id="38e99-148">(En C#, la \ carácter tiene un significado especial a menos que use un literal de cadena textual.)</span><span class="sxs-lookup"><span data-stu-id="38e99-148">(In C#, the \ character has special meaning unless you use a verbatim string literal.)</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample6.html)]

<span data-ttu-id="38e99-149">Para incrustar en comillas dobles, use un literal de cadena textual y repetir las comillas:</span><span class="sxs-lookup"><span data-stu-id="38e99-149">To embed double quotation marks, use a verbatim string literal and repeat the quotation marks:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample7.html)]

<span data-ttu-id="38e99-150">Este es el resultado del uso de estos ejemplos de ambos en una página:</span><span class="sxs-lookup"><span data-stu-id="38e99-150">Here's the result of using both of these examples in a page:</span></span>

![Razor Img4](introducing-razor-syntax-c/_static/image4.jpg)

> [!NOTE]
> <span data-ttu-id="38e99-152">Tenga en cuenta que el `@` carácter se utiliza para marcar los literales de cadena textual en C# y que marca el código en las páginas ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="38e99-152">Notice that the `@` character is used both to mark verbatim string literals in C# and to mark code in ASP.NET pages.</span></span>


### <a name="6-code-is-case-sensitive"></a><span data-ttu-id="38e99-153">6. Código distingue mayúsculas de minúsculas</span><span class="sxs-lookup"><span data-stu-id="38e99-153">6. Code is case sensitive</span></span>

<span data-ttu-id="38e99-154">En C#, palabras clave (como `var`, `true`, y `if`) y los nombres de variables distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="38e99-154">In C#, keywords (like `var`, `true`, and `if`) and variable names are case sensitive.</span></span> <span data-ttu-id="38e99-155">Las siguientes líneas de código crean dos variables distintas, `lastName` y `LastName.`</span><span class="sxs-lookup"><span data-stu-id="38e99-155">The following lines of code create two different variables, `lastName` and `LastName.`</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample8.cshtml)]

<span data-ttu-id="38e99-156">Si declara una variable como `var lastName = "Smith";` y si se intenta hacer referencia a esa variable en la página como `@LastName`, se producirá un error porque `LastName` no se reconoce.</span><span class="sxs-lookup"><span data-stu-id="38e99-156">If you declare a variable as `var lastName = "Smith";` and if you try to reference that variable in your page as `@LastName`, an error results because `LastName` won't be recognized.</span></span>

> [!NOTE]
> <span data-ttu-id="38e99-157">En Visual Basic, palabras clave y las variables son *no* distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="38e99-157">In Visual Basic, keywords and variables are *not* case sensitive.</span></span>


### <a name="7-much-of-your-coding-involves-objects"></a><span data-ttu-id="38e99-158">7. Gran parte de la codificación implica objetos</span><span class="sxs-lookup"><span data-stu-id="38e99-158">7. Much of your coding involves objects</span></span>

<span data-ttu-id="38e99-159">Un *objeto* representa algo que se puede programar con &#8212; una página, un cuadro de texto, un archivo, una imagen, una solicitud web, un mensaje de correo electrónico, un registro de cliente (fila de la base de datos), etcetera. Los objetos tienen propiedades que describen sus características y que puede leer o cambiar &#8212; un objeto de cuadro de texto tiene un `Text` propiedad (entre otros), un objeto de solicitud tiene un `Url` propiedad, un mensaje de correo electrónico tiene un `From` propiedad, y un objeto customer tiene una `FirstName` propiedad.</span><span class="sxs-lookup"><span data-stu-id="38e99-159">An *object* represents a thing that you can program with &#8212; a page, a text box, a file, an image, a web request, an email message, a customer record (database row), etc. Objects have properties that describe their characteristics and that you can read or change &#8212; a text box object has a `Text` property (among others), a request object has a `Url` property, an email message has a `From` property, and a customer object has a `FirstName` property.</span></span> <span data-ttu-id="38e99-160">Los objetos también tienen métodos que son el &quot;verbos&quot; pueden realizar.</span><span class="sxs-lookup"><span data-stu-id="38e99-160">Objects also have methods that are the &quot;verbs&quot; they can perform.</span></span> <span data-ttu-id="38e99-161">Algunos ejemplos son un objeto de archivo `Save` método, un objeto de imagen `Rotate` método y un objeto de correo electrónico `Send` método.</span><span class="sxs-lookup"><span data-stu-id="38e99-161">Examples include a file object's `Save` method, an image object's `Rotate` method, and an email object's `Send` method.</span></span>

<span data-ttu-id="38e99-162">A menudo trabajará con la `Request` objeto, que proporciona información como los valores de los cuadros de texto (campos de formulario) en la página, el tipo de explorador realizó la solicitud, la dirección URL de la página, la identidad del usuario, etcetera. El ejemplo siguiente muestra cómo obtener acceso a las propiedades de la `Request` objeto y cómo llamar a la `MapPath` método de la `Request` objeto, que proporciona la ruta de acceso absoluta de la página en el servidor:</span><span class="sxs-lookup"><span data-stu-id="38e99-162">You'll often work with the `Request` object, which gives you information like the values of text boxes (form fields) on the page, what type of browser made the request, the URL of the page, the user identity, etc. The following example shows how to access properties of the `Request` object and how to call the `MapPath` method of the `Request` object, which gives you the absolute path of the page on the server:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample9.html)]

<span data-ttu-id="38e99-163">El resultado se muestra en un explorador:</span><span class="sxs-lookup"><span data-stu-id="38e99-163">The result displayed in a browser:</span></span>

![Razor Img5](introducing-razor-syntax-c/_static/image5.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a><span data-ttu-id="38e99-165">8. Puede escribir código que toma decisiones</span><span class="sxs-lookup"><span data-stu-id="38e99-165">8. You can write code that makes decisions</span></span>

<span data-ttu-id="38e99-166">Una característica clave de páginas web dinámicas es que puede determinar qué hacer en función de condiciones.</span><span class="sxs-lookup"><span data-stu-id="38e99-166">A key feature of dynamic web pages is that you can determine what to do based on conditions.</span></span> <span data-ttu-id="38e99-167">La manera más común de hacerlo es con la `if` instrucción (y opcionales `else` instrucción).</span><span class="sxs-lookup"><span data-stu-id="38e99-167">The most common way to do this is with the `if` statement (and optional `else` statement).</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample10.cshtml)]

<span data-ttu-id="38e99-168">La instrucción `if(IsPost)` es una manera abreviada de la escritura `if(IsPost == true)`.</span><span class="sxs-lookup"><span data-stu-id="38e99-168">The statement `if(IsPost)` is a shorthand way of writing `if(IsPost == true)`.</span></span> <span data-ttu-id="38e99-169">Junto con `if` instrucciones, hay una gran variedad de formas para probar condiciones, repita los bloques de código, y así sucesivamente, que se describen más adelante en este artículo.</span><span class="sxs-lookup"><span data-stu-id="38e99-169">Along with `if` statements, there are a variety of ways to test conditions, repeat blocks of code, and so on, which are described later in this article.</span></span>

<span data-ttu-id="38e99-170">El resultado que se muestra en un explorador (después de hacer clic **enviar**):</span><span class="sxs-lookup"><span data-stu-id="38e99-170">The result displayed in a browser (after clicking **Submit**):</span></span>

![Razor Img6](introducing-razor-syntax-c/_static/image6.jpg)

> [!TIP] 
> 
> <a id="SB_HttpGetPost"></a>
> ### <a name="http-get-and-post-methods-and-the-ispost-property"></a><span data-ttu-id="38e99-172">HTTP GET y POST métodos y la propiedad IsPost</span><span class="sxs-lookup"><span data-stu-id="38e99-172">HTTP GET and POST Methods and the IsPost Property</span></span>
> 
> <span data-ttu-id="38e99-173">El protocolo utilizado para las páginas web (HTTP) admite un número muy limitado de métodos (verbos) que se usan para realizar solicitudes al servidor.</span><span class="sxs-lookup"><span data-stu-id="38e99-173">The protocol used for web pages (HTTP) supports a very limited number of methods (verbs) that are used to make requests to the server.</span></span> <span data-ttu-id="38e99-174">Dos las más comunes son GET, que se utiliza para leer una página, y POST, que se usa para enviar una página.</span><span class="sxs-lookup"><span data-stu-id="38e99-174">The two most common ones are GET, which is used to read a page, and POST, which is used to submit a page.</span></span> <span data-ttu-id="38e99-175">En general, la primera vez que un usuario solicita una página, se solicita la página con GET.</span><span class="sxs-lookup"><span data-stu-id="38e99-175">In general, the first time a user requests a page, the page is requested using GET.</span></span> <span data-ttu-id="38e99-176">Si el usuario escribe en un formulario y, a continuación, hace clic en un botón de envío, el explorador realiza una solicitud POST al servidor.</span><span class="sxs-lookup"><span data-stu-id="38e99-176">If the user fills in a form and then clicks a submit button, the browser makes a POST request to the server.</span></span>
> 
> <span data-ttu-id="38e99-177">En la programación web, a menudo resulta útil saber si una página se solicitan como una operación GET o como una entrada de blog para que sepa cómo procesar la página.</span><span class="sxs-lookup"><span data-stu-id="38e99-177">In web programming, it's often useful to know whether a page is being requested as a GET or as a POST so that you know how to process the page.</span></span> <span data-ttu-id="38e99-178">En ASP.NET Web Pages, puede usar el `IsPost` propiedad para ver si una solicitud es una operación GET o POST.</span><span class="sxs-lookup"><span data-stu-id="38e99-178">In ASP.NET Web Pages, you can use the `IsPost` property to see whether a request is a GET or a POST.</span></span> <span data-ttu-id="38e99-179">Si la solicitud es una publicación, el `IsPost` propiedad devolverá true, y puede hacer cosas como la lectura de los valores de los cuadros de texto en un formulario.</span><span class="sxs-lookup"><span data-stu-id="38e99-179">If the request is a POST, the `IsPost` property will return true, and you can do things like read the values of text boxes on a form.</span></span> <span data-ttu-id="38e99-180">Muchos ejemplos verá mostrarle cómo procesar la página de manera diferente dependiendo del valor de `IsPost`.</span><span class="sxs-lookup"><span data-stu-id="38e99-180">Many examples you'll see show you how to process the page differently depending on the value of `IsPost`.</span></span>


## <a name="a-simple-code-example"></a><span data-ttu-id="38e99-181">Un ejemplo de código Simple</span><span class="sxs-lookup"><span data-stu-id="38e99-181">A Simple Code Example</span></span>

<span data-ttu-id="38e99-182">Este procedimiento muestra cómo crear una página que muestra las técnicas de programación básicas.</span><span class="sxs-lookup"><span data-stu-id="38e99-182">This procedure shows you how to create a page that illustrates basic programming techniques.</span></span> <span data-ttu-id="38e99-183">En el ejemplo, cree una página que permite a los usuarios escribir dos números, a continuación, agrega y muestra el resultado.</span><span class="sxs-lookup"><span data-stu-id="38e99-183">In the example, you create a page that lets users enter two numbers, then it adds them and displays the result.</span></span>

1. <span data-ttu-id="38e99-184">En el editor, cree un nuevo archivo y asígnele el nombre *AddNumbers.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="38e99-184">In your editor, create a new file and name it *AddNumbers.cshtml*.</span></span>
2. <span data-ttu-id="38e99-185">Copie el siguiente código y marcado en la página, reemplazando cualquier cosa ya está en la página.</span><span class="sxs-lookup"><span data-stu-id="38e99-185">Copy the following code and markup into the page, replacing anything already in the page.</span></span>  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample11.cshtml)]

    <span data-ttu-id="38e99-186">Estos son algunos aspectos que tener en cuenta:</span><span class="sxs-lookup"><span data-stu-id="38e99-186">Here are some things for you to note:</span></span>

    - <span data-ttu-id="38e99-187">El `@` carácter inicia el primer bloque de código en la página y precede a la `totalMessage` variable que se incrusta en la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="38e99-187">The `@` character starts the first block of code in the page, and it precedes the `totalMessage` variable that's embedded near the bottom of the page.</span></span>
    - <span data-ttu-id="38e99-188">El bloque en la parte superior de la página aparece encerrado entre llaves.</span><span class="sxs-lookup"><span data-stu-id="38e99-188">The block at the top of the page is enclosed in braces.</span></span>
    - <span data-ttu-id="38e99-189">En el bloque, en la parte superior de todas las líneas terminan con un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="38e99-189">In the block at the top, all lines end with a semicolon.</span></span>
    - <span data-ttu-id="38e99-190">Las variables `total`, `num1`, `num2`, y `totalMessage` almacenar varios números y una cadena.</span><span class="sxs-lookup"><span data-stu-id="38e99-190">The variables `total`, `num1`, `num2`, and `totalMessage` store several numbers and a string.</span></span>
    - <span data-ttu-id="38e99-191">El valor literal de cadena asignado a la `totalMessage` es variable entre comillas dobles.</span><span class="sxs-lookup"><span data-stu-id="38e99-191">The literal string value assigned to the `totalMessage` variable is in double quotation marks.</span></span>
    - <span data-ttu-id="38e99-192">Dado que el código distingue mayúsculas de minúsculas cuando la `totalMessage` variable se usa la parte inferior de la página, su nombre debe coincidir exactamente con la variable en la parte superior.</span><span class="sxs-lookup"><span data-stu-id="38e99-192">Because the code is case-sensitive, when the `totalMessage` variable is used near the bottom of the page, its name must match the variable at the top exactly.</span></span>
    - <span data-ttu-id="38e99-193">La expresión `num1.AsInt() + num2.AsInt()` se muestra cómo trabajar con objetos y métodos.</span><span class="sxs-lookup"><span data-stu-id="38e99-193">The expression `num1.AsInt() + num2.AsInt()` shows how to work with objects and methods.</span></span> <span data-ttu-id="38e99-194">El `AsInt` método en cada variable convierte la cadena especificada por el usuario a un número (entero) para que pueda realizar operaciones aritméticas en él.</span><span class="sxs-lookup"><span data-stu-id="38e99-194">The `AsInt` method on each variable converts the string entered by a user to a number (an integer) so that you can perform arithmetic on it.</span></span>
    - <span data-ttu-id="38e99-195">El `<form>` etiqueta incluye un `method="post"` atributo.</span><span class="sxs-lookup"><span data-stu-id="38e99-195">The `<form>` tag includes a `method="post"` attribute.</span></span> <span data-ttu-id="38e99-196">Especifica que, cuando el usuario hace clic en **agregar**, la página se enviará al servidor mediante el método HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="38e99-196">This specifies that when the user clicks **Add**, the page will be sent to the server using the HTTP POST method.</span></span> <span data-ttu-id="38e99-197">Cuando se envía la página, el `if(IsPost)` test se evalúa como true y operador condicional de código se ejecuta, muestra el resultado de sumar los números.</span><span class="sxs-lookup"><span data-stu-id="38e99-197">When the page is submitted, the `if(IsPost)` test evaluates to true and the conditional code runs, displaying the result of adding the numbers.</span></span>
3. <span data-ttu-id="38e99-198">Guarde la página y ejecútelo en un explorador.</span><span class="sxs-lookup"><span data-stu-id="38e99-198">Save the page and run it in a browser.</span></span> <span data-ttu-id="38e99-199">(Asegúrese de que la página está seleccionada en el **archivos** área de trabajo antes de ejecutarlo.) Escriba los dos números enteros y, a continuación, haga clic en el **agregar** botón.</span><span class="sxs-lookup"><span data-stu-id="38e99-199">(Make sure the page is selected in the **Files** workspace before you run it.) Enter two whole numbers and then click the **Add** button.</span></span> 

    ![Razor Img7](introducing-razor-syntax-c/_static/image7.jpg)

## <a name="basic-programming-concepts"></a><span data-ttu-id="38e99-201">Conceptos básicos de programación</span><span class="sxs-lookup"><span data-stu-id="38e99-201">Basic Programming Concepts</span></span>

<span data-ttu-id="38e99-202">Este artículo proporciona una visión general de programación web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="38e99-202">This article provides you with an overview of ASP.NET web programming.</span></span> <span data-ttu-id="38e99-203">No es un examen exhaustivo, simplemente un paseo rápido a través de los conceptos de programación que se usará con más frecuencia.</span><span class="sxs-lookup"><span data-stu-id="38e99-203">It isn't an exhaustive examination, just a quick tour through the programming concepts you'll use most often.</span></span> <span data-ttu-id="38e99-204">Aun así, cubre casi todo que lo necesario para empezar a trabajar con ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="38e99-204">Even so, it covers almost everything you'll need to get started with ASP.NET Web Pages.</span></span>

<span data-ttu-id="38e99-205">Pero primero, una pequeña Introducción técnica.</span><span class="sxs-lookup"><span data-stu-id="38e99-205">But first, a little technical background.</span></span>

### <a name="the-razor-syntax-server-code-and-aspnet"></a><span data-ttu-id="38e99-206">La sintaxis de Razor y código de servidor ASP.NET</span><span class="sxs-lookup"><span data-stu-id="38e99-206">The Razor Syntax, Server Code, and ASP.NET</span></span>

<span data-ttu-id="38e99-207">Sintaxis de Razor es una simple programación para insertar código basado en servidor en una página web.</span><span class="sxs-lookup"><span data-stu-id="38e99-207">Razor syntax is a simple programming syntax for embedding server-based code in a web page.</span></span> <span data-ttu-id="38e99-208">En una página web que usa la sintaxis de Razor, hay dos tipos de contenido: código de servidor y de contenido de cliente.</span><span class="sxs-lookup"><span data-stu-id="38e99-208">In a web page that uses the Razor syntax, there are two kinds of content: client content and server code.</span></span> <span data-ttu-id="38e99-209">Contenido de cliente es lo que solía usar en las páginas web: el formato HTML (elementos), la información de estilo como CSS, quizá algún script de cliente, como JavaScript y texto sin formato.</span><span class="sxs-lookup"><span data-stu-id="38e99-209">Client content is the stuff you're used to in web pages: HTML markup (elements), style information such as CSS, maybe some client script such as JavaScript, and plain text.</span></span>

<span data-ttu-id="38e99-210">Sintaxis de Razor le permite agregar código de servidor a este contenido de cliente.</span><span class="sxs-lookup"><span data-stu-id="38e99-210">Razor syntax lets you add server code to this client content.</span></span> <span data-ttu-id="38e99-211">Si no hay código de servidor en la página, el servidor ejecuta ese código en primer lugar, antes de enviar la página al explorador.</span><span class="sxs-lookup"><span data-stu-id="38e99-211">If there's server code in the page, the server runs that code first, before it sends the page to the browser.</span></span> <span data-ttu-id="38e99-212">Al ejecutarse en el servidor, el código puede realizar las tareas que pueden ser mucho más complejas con contenido de cliente por sí solo, como acceder a bases de datos basadas en servidor.</span><span class="sxs-lookup"><span data-stu-id="38e99-212">By running on the server, the code can perform tasks that can be a lot more complex to do using client content alone, like accessing server-based databases.</span></span> <span data-ttu-id="38e99-213">Lo más importante, el código de servidor puede crear dinámicamente el contenido de cliente &#8212; puede generar marcado HTML u otro contenido sobre la marcha y, a continuación, vuelva a enviarlo al explorador junto con código HTML estático que la página puede contener.</span><span class="sxs-lookup"><span data-stu-id="38e99-213">Most importantly, server code can dynamically create client content &#8212; it can generate HTML markup or other content on the fly and then send it to the browser along with any static HTML that the page might contain.</span></span> <span data-ttu-id="38e99-214">Desde la perspectiva del explorador, el contenido de cliente generado por el código del servidor es similar a cualquier otro contenido de cliente.</span><span class="sxs-lookup"><span data-stu-id="38e99-214">From the browser's perspective, client content that's generated by your server code is no different than any other client content.</span></span> <span data-ttu-id="38e99-215">Como ya hemos visto, el código del servidor que se necesita es bastante sencillo.</span><span class="sxs-lookup"><span data-stu-id="38e99-215">As you've already seen, the server code that's required is quite simple.</span></span>

<span data-ttu-id="38e99-216">ASP.NET web pages que incluya la sintaxis de Razor tienen una extensión de archivo especial (*.cshtml* o *.vbhtml*).</span><span class="sxs-lookup"><span data-stu-id="38e99-216">ASP.NET web pages that include the Razor syntax have a special file extension (*.cshtml* or *.vbhtml*).</span></span> <span data-ttu-id="38e99-217">El servidor reconoce estas extensiones, se ejecuta el código que está marcado con sintaxis Razor y, a continuación, envía la página al explorador.</span><span class="sxs-lookup"><span data-stu-id="38e99-217">The server recognizes these extensions, runs the code that's marked with Razor syntax, and then sends the page to the browser.</span></span>

### <a name="where-does-aspnet-fit-in"></a><span data-ttu-id="38e99-218">¿Dónde encaja ASP.NET?</span><span class="sxs-lookup"><span data-stu-id="38e99-218">Where does ASP.NET fit in?</span></span>

<span data-ttu-id="38e99-219">Sintaxis de Razor se basa en una tecnología de Microsoft llamada ASP.NET, que a su vez se basa en Microsoft .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="38e99-219">Razor syntax is based on a technology from Microsoft called ASP.NET, which in turn is based on the Microsoft .NET Framework.</span></span> <span data-ttu-id="38e99-220">.NET Framework es un marco de programación grande y completa de Microsoft para el desarrollo de prácticamente cualquier tipo de aplicación del equipo.</span><span class="sxs-lookup"><span data-stu-id="38e99-220">The.NET Framework is a big, comprehensive programming framework from Microsoft for developing virtually any type of computer application.</span></span> <span data-ttu-id="38e99-221">ASP.NET es la parte de .NET Framework que está diseñado específicamente para crear aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="38e99-221">ASP.NET is the part of the .NET Framework that's specifically designed for creating web applications.</span></span> <span data-ttu-id="38e99-222">Los desarrolladores han usado ASP.NET para crear muchos de los sitios Web más grandes y más alto de tráfico en el mundo.</span><span class="sxs-lookup"><span data-stu-id="38e99-222">Developers have used ASP.NET to create many of the largest and highest-traffic websites in the world.</span></span> <span data-ttu-id="38e99-223">(Cada vez que vea la extensión de nombre de archivo *.aspx* como parte de la dirección URL en un sitio, sabrá que el sitio se ha escrito con ASP.NET.)</span><span class="sxs-lookup"><span data-stu-id="38e99-223">(Any time you see the file-name extension *.aspx* as part of the URL in a site, you'll know that the site was written using ASP.NET.)</span></span>

<span data-ttu-id="38e99-224">La sintaxis de Razor le proporciona toda la potencia de ASP.NET, pero con una sintaxis simplificada que es más fácil obtener información sobre si es principiante y que mejora su productividad si es un experto.</span><span class="sxs-lookup"><span data-stu-id="38e99-224">The Razor syntax gives you all the power of ASP.NET, but using a simplified syntax that's easier to learn if you're a beginner and that makes you more productive if you're an expert.</span></span> <span data-ttu-id="38e99-225">Aunque esta sintaxis es fácil de usar, su relación familia con ASP.NET y .NET Framework significa que como los sitios Web son más sofisticados, tiene la potencia de los marcos más grandes a su disposición.</span><span class="sxs-lookup"><span data-stu-id="38e99-225">Even though this syntax is simple to use, its family relationship to ASP.NET and the .NET Framework means that as your websites become more sophisticated, you have the power of the larger frameworks available to you.</span></span>

![Razor Img8](introducing-razor-syntax-c/_static/image8.jpg)

> [!TIP] 
> 
> <span data-ttu-id="38e99-227">**Las clases e instancias**</span><span class="sxs-lookup"><span data-stu-id="38e99-227">**Classes and Instances**</span></span>
> 
> <span data-ttu-id="38e99-228">Código de servidor ASP.NET usa los objetos, que a su vez se basan en la idea de las clases.</span><span class="sxs-lookup"><span data-stu-id="38e99-228">ASP.NET server code uses objects, which are in turn built on the idea of classes.</span></span> <span data-ttu-id="38e99-229">La clase es la definición o la plantilla para un objeto.</span><span class="sxs-lookup"><span data-stu-id="38e99-229">The class is the definition or template for an object.</span></span> <span data-ttu-id="38e99-230">Por ejemplo, una aplicación podría contener un `Customer` clase que define las propiedades y métodos que necesita de cualquier objeto de cliente.</span><span class="sxs-lookup"><span data-stu-id="38e99-230">For example, an application might contain a `Customer` class that defines the properties and methods that any customer object needs.</span></span>
> 
> <span data-ttu-id="38e99-231">Cuando la aplicación necesita trabajar con información de clientes reales, crea una instancia de (o *crea una instancia*) un objeto de cliente.</span><span class="sxs-lookup"><span data-stu-id="38e99-231">When the application needs to work with actual customer information, it creates an instance of (or *instantiates*) a customer object.</span></span> <span data-ttu-id="38e99-232">Cada cliente individual es una instancia independiente de la `Customer` clase.</span><span class="sxs-lookup"><span data-stu-id="38e99-232">Each individual customer is a separate instance of the `Customer` class.</span></span> <span data-ttu-id="38e99-233">Cada instancia es compatible con las mismas propiedades y métodos, pero los valores de propiedad para cada instancia son normalmente diferentes porque cada objeto customer es único.</span><span class="sxs-lookup"><span data-stu-id="38e99-233">Every instance supports the same properties and methods, but the property values for each instance are typically different, because each customer object is unique.</span></span> <span data-ttu-id="38e99-234">En el objeto de un cliente, el `LastName` propiedad podría ser "Smith"; en otro objeto de cliente, el `LastName` propiedad podría ser "Jones".</span><span class="sxs-lookup"><span data-stu-id="38e99-234">In one customer object, the `LastName` property might be "Smith"; in another customer object, the `LastName` property might be "Jones."</span></span>
> 
> <span data-ttu-id="38e99-235">De forma similar, cualquier página web individual en su sitio es un `Page` objeto que es una instancia de la `Page` clase.</span><span class="sxs-lookup"><span data-stu-id="38e99-235">Similarly, any individual web page in your site is a `Page` object that's an instance of the `Page` class.</span></span> <span data-ttu-id="38e99-236">Un botón en la página es una `Button` objeto que es una instancia de la `Button` clase y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="38e99-236">A button on the page is a `Button` object that is an instance of the `Button` class, and so on.</span></span> <span data-ttu-id="38e99-237">Cada instancia tiene sus propias características, pero todos ellos se basan en lo que se especifica en la definición de clase del objeto.</span><span class="sxs-lookup"><span data-stu-id="38e99-237">Each instance has its own characteristics, but they all are based on what's specified in the object's class definition.</span></span>


## <a name="basic-syntax"></a><span data-ttu-id="38e99-238">Sintaxis básica</span><span class="sxs-lookup"><span data-stu-id="38e99-238">Basic Syntax</span></span>

<span data-ttu-id="38e99-239">Ya vimos un ejemplo básico de cómo crear una página de ASP.NET Web Pages y cómo puede agregar código de servidor en formato HTML.</span><span class="sxs-lookup"><span data-stu-id="38e99-239">Earlier you saw a basic example of how to create an ASP.NET Web Pages page, and how you can add server code to HTML markup.</span></span> <span data-ttu-id="38e99-240">Aquí aprenderá los fundamentos de escribir código de servidor ASP.NET mediante la sintaxis Razor &#8212; es decir, las reglas de lenguaje de programación.</span><span class="sxs-lookup"><span data-stu-id="38e99-240">Here you'll learn the basics of writing ASP.NET server code using the Razor syntax &#8212; that is, the programming language rules.</span></span>

<span data-ttu-id="38e99-241">Si tiene experiencia con la programación (especialmente si ha usado C, C++, C#, Visual Basic o JavaScript), gran parte de lo que lee aquí le resultarán familiar.</span><span class="sxs-lookup"><span data-stu-id="38e99-241">If you're experienced with programming (especially if you've used C, C++, C#, Visual Basic, or JavaScript), much of what you read here will be familiar.</span></span> <span data-ttu-id="38e99-242">Probablemente deberá familiarizarse con sólo de cómo se agrega el código de servidor al marcado en *.cshtml* archivos.</span><span class="sxs-lookup"><span data-stu-id="38e99-242">You'll probably need to familiarize yourself only with how server code is added to markup in *.cshtml* files.</span></span>

<a id="BM_CombiningTextMarkupAndCode"></a>
### <a name="combining-text-markup-and-code-in-code-blocks"></a><span data-ttu-id="38e99-243">Combinación de texto, marcado y código en bloques de código</span><span class="sxs-lookup"><span data-stu-id="38e99-243">Combining Text, Markup, and Code in Code Blocks</span></span>

<span data-ttu-id="38e99-244">En los bloques de código de servidor, a menudo desea salida texto o marcado (o ambos) a la página.</span><span class="sxs-lookup"><span data-stu-id="38e99-244">In server code blocks, you often want to output text or markup (or both) to the page.</span></span> <span data-ttu-id="38e99-245">Si un bloque de código de servidor contiene texto que no es código y que en su lugar se debe representar como está, debe ser capaz de distinguir ese texto desde el código ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="38e99-245">If a server code block contains text that's not code and that instead should be rendered as is, ASP.NET needs to be able to distinguish that text from code.</span></span> <span data-ttu-id="38e99-246">Existen varias formas de hacerlo.</span><span class="sxs-lookup"><span data-stu-id="38e99-246">There are several ways to do this.</span></span>

- <span data-ttu-id="38e99-247">Incluir el texto en un elemento HTML como `<p></p>` o `<em></em>`:</span><span class="sxs-lookup"><span data-stu-id="38e99-247">Enclose the text in an HTML element like `<p></p>` or `<em></em>`:</span></span>   

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample12.cshtml)]

    <span data-ttu-id="38e99-248">El elemento HTML puede incluir texto, los elementos adicionales de HTML y expresiones de código del servidor.</span><span class="sxs-lookup"><span data-stu-id="38e99-248">The HTML element can include text, additional HTML elements, and server-code expressions.</span></span> <span data-ttu-id="38e99-249">Cuando ASP.NET ve la etiqueta HTML de apertura (por ejemplo, `<p>`), representa todo lo que incluye el elemento y su contenido como está en el explorador, resolver las expresiones de código de servidor conforme avanza.</span><span class="sxs-lookup"><span data-stu-id="38e99-249">When ASP.NET sees the opening HTML tag (for example, `<p>`), it renders everything including the element and its content as is to the browser, resolving server-code expressions as it goes.</span></span>
- <span data-ttu-id="38e99-250">Use la `@:` operador o la `<text>` elemento.</span><span class="sxs-lookup"><span data-stu-id="38e99-250">Use the `@:` operator or the `<text>` element.</span></span> <span data-ttu-id="38e99-251">El `@:` da como resultado una sola línea de contenido que contiene texto sin formato o etiquetas HTML no coincidentes; el `<text>` elemento abarca varias líneas de salida.</span><span class="sxs-lookup"><span data-stu-id="38e99-251">The `@:` outputs a single line of content containing plain text or unmatched HTML tags; the `<text>` element encloses multiple lines to output.</span></span> <span data-ttu-id="38e99-252">Estas opciones son útiles cuando no quiere representar un elemento HTML como parte de la salida.</span><span class="sxs-lookup"><span data-stu-id="38e99-252">These options are useful when you don't want to render an HTML element as part of the output.</span></span>  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample13.cshtml)]

    <span data-ttu-id="38e99-253">Si desea que varias líneas de texto o no coincidentes de las etiquetas HTML de salida, puede preceder a cada línea con `@:`, ni puede incluir la línea en un `<text>` elemento.</span><span class="sxs-lookup"><span data-stu-id="38e99-253">If you want to output multiple lines of text or unmatched HTML tags, you can precede each line with `@:`, or you can enclose the line in a `<text>` element.</span></span> <span data-ttu-id="38e99-254">Al igual que el `@:` operador,`<text>` las etiquetas se usan por ASP.NET para identificar el contenido de texto y nunca se representan en el resultado de la página.</span><span class="sxs-lookup"><span data-stu-id="38e99-254">Like the `@:` operator,`<text>` tags are used by ASP.NET to identify text content and are never rendered in the page output.</span></span>

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample14.cshtml)]

    <span data-ttu-id="38e99-255">El primer ejemplo se repite el ejemplo anterior, pero usa un único par de `<text>` etiquetas para delimitar el texto para representar.</span><span class="sxs-lookup"><span data-stu-id="38e99-255">The first example repeats the previous example but uses a single pair of `<text>` tags to enclose the text to render.</span></span> <span data-ttu-id="38e99-256">En el segundo ejemplo, el `<text>` y `</text>` etiquetas incluir tres líneas, todos ellos tienen algunas no contenido texto y etiquetas HTML no coincidentes (`<br />`), junto con el código de servidor y las etiquetas HTML coincidentes.</span><span class="sxs-lookup"><span data-stu-id="38e99-256">In the second example, the `<text>` and `</text>` tags enclose three lines, all of which have some uncontained text and unmatched HTML tags (`<br />`), along with server code and matched HTML tags.</span></span> <span data-ttu-id="38e99-257">De nuevo, también se podría preceder cada línea individualmente con el `@:` operador; cualquiera de ellas funciona de forma.</span><span class="sxs-lookup"><span data-stu-id="38e99-257">Again, you could also precede each line individually with the `@:` operator; either way works.</span></span>

    > [!NOTE]
    > <span data-ttu-id="38e99-258">Al generar texto, como se muestra en esta sección &#8212; con el elemento HTML, el `@:` (operador), o la `<text>` elemento &#8212; ASP.NET no codifica como HTML la salida.</span><span class="sxs-lookup"><span data-stu-id="38e99-258">When you output text as shown in this section &#8212; using an HTML element, the `@:` operator, or the `<text>` element &#8212; ASP.NET doesn't HTML-encode the output.</span></span> <span data-ttu-id="38e99-259">(Como se indicó anteriormente, ASP.NET codificar la salida de las expresiones de código de servidor y los bloques de código de servidor que van precedidos por `@`, excepto en los casos especiales que se indican en esta sección.)</span><span class="sxs-lookup"><span data-stu-id="38e99-259">(As noted earlier, ASP.NET does encode the output of server code expressions and server code blocks that are preceded by `@`, except in the special cases noted in this section.)</span></span>

### <a name="whitespace"></a><span data-ttu-id="38e99-260">Whitespace</span><span class="sxs-lookup"><span data-stu-id="38e99-260">Whitespace</span></span>

<span data-ttu-id="38e99-261">Los espacios adicionales en una instrucción (y fuera de un literal de cadena) no afectan a la instrucción:</span><span class="sxs-lookup"><span data-stu-id="38e99-261">Extra spaces in a statement (and outside of a string literal) don't affect the statement:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample15.cshtml)]

<span data-ttu-id="38e99-262">Un salto de línea en una instrucción no tiene ningún efecto en la instrucción, y puede incluir instrucciones para mejorar la legibilidad.</span><span class="sxs-lookup"><span data-stu-id="38e99-262">A line break in a statement has no effect on the statement, and you can wrap statements for readability.</span></span> <span data-ttu-id="38e99-263">Las instrucciones siguientes son los mismos:</span><span class="sxs-lookup"><span data-stu-id="38e99-263">The following statements are the same:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample16.cshtml)]

<span data-ttu-id="38e99-264">Sin embargo, no se puede ajustar una línea en el medio de un literal de cadena.</span><span class="sxs-lookup"><span data-stu-id="38e99-264">However, you can't wrap a line in the middle of a string literal.</span></span> <span data-ttu-id="38e99-265">No funciona en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="38e99-265">The following example doesn't work:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample17.cshtml)]

<span data-ttu-id="38e99-266">Para combinar una cadena larga que se ajusta a varias líneas, como el código anterior, hay dos opciones.</span><span class="sxs-lookup"><span data-stu-id="38e99-266">To combine a long string that wraps to multiple lines like the above code, there are two options.</span></span> <span data-ttu-id="38e99-267">Puede usar el operador de concatenación (`+`), que verá más adelante en este artículo.</span><span class="sxs-lookup"><span data-stu-id="38e99-267">You can use the concatenation operator (`+`), which you'll see later in this article.</span></span> <span data-ttu-id="38e99-268">También puede usar el `@` carácter que se va a crear una cadena textual literal, como se vio anteriormente en este artículo.</span><span class="sxs-lookup"><span data-stu-id="38e99-268">You can also use the `@` character to create a verbatim string literal, as you saw earlier in this article.</span></span> <span data-ttu-id="38e99-269">Puede dividir literales de cadena textual en líneas:</span><span class="sxs-lookup"><span data-stu-id="38e99-269">You can break verbatim string literals across lines:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample18.cshtml)]

### <a name="code-and-markup-comments"></a><span data-ttu-id="38e99-270">Código (y marcado) comentarios</span><span class="sxs-lookup"><span data-stu-id="38e99-270">Code (and Markup) Comments</span></span>

<span data-ttu-id="38e99-271">Comentarios le permite dejar notas para uso personal o a otros usuarios.</span><span class="sxs-lookup"><span data-stu-id="38e99-271">Comments let you leave notes for yourself or others.</span></span> <span data-ttu-id="38e99-272">También permiten deshabilitar (*comentario*) una sección de código o marcado que no desea ejecutar, pero desea conservar en la página por el momento.</span><span class="sxs-lookup"><span data-stu-id="38e99-272">They also allow you to disable (*comment out*) a section of code or markup that you don't want to run but want to keep in your page for the time being.</span></span>

<span data-ttu-id="38e99-273">Hay diferentes comentar la sintaxis de código de Razor y de marcado HTML.</span><span class="sxs-lookup"><span data-stu-id="38e99-273">There's different commenting syntax for Razor code and for HTML markup.</span></span> <span data-ttu-id="38e99-274">Al igual que con todo el código Razor, Razor comentarios son procesados (y, a continuación, se quitan) en el servidor antes de la página se envía al explorador.</span><span class="sxs-lookup"><span data-stu-id="38e99-274">As with all Razor code, Razor comments are processed (and then removed) on the server before the page is sent to the browser.</span></span> <span data-ttu-id="38e99-275">Por lo tanto, la sintaxis de comentario de Razor le permite colocar comentarios en el código (o incluso en el código de marcado) que se pueden ver cuando edite el archivo, pero los usuarios no ven, incluso en el origen de la página.</span><span class="sxs-lookup"><span data-stu-id="38e99-275">Therefore, the Razor commenting syntax lets you put comments into the code (or even into the markup) that you can see when you edit the file, but that users don't see, even in the page source.</span></span>

<span data-ttu-id="38e99-276">Para los comentarios de Razor de ASP.NET, debe comenzar con el comentario `@*` y finalizan con `*@`.</span><span class="sxs-lookup"><span data-stu-id="38e99-276">For ASP.NET Razor comments, you start the comment with `@*` and end it with `*@`.</span></span> <span data-ttu-id="38e99-277">El comentario puede estar en una o varias líneas:</span><span class="sxs-lookup"><span data-stu-id="38e99-277">The comment can be on one line or multiple lines:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample19.cshtml)]

<span data-ttu-id="38e99-278">Este es un comentario dentro de un bloque de código:</span><span class="sxs-lookup"><span data-stu-id="38e99-278">Here is a comment within a code block:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample20.cshtml)]

<span data-ttu-id="38e99-279">Aquí está el mismo bloque de código, con la línea de código marcada como comentario para que no se ejecutará:</span><span class="sxs-lookup"><span data-stu-id="38e99-279">Here is the same block of code, with the line of code commented out so that it won't run:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample21.cshtml)]

<span data-ttu-id="38e99-280">Dentro de un bloque de código, como una alternativa al uso de la sintaxis de comentario de Razor, puede usar la sintaxis de comentario del lenguaje de programación que está usando, como C#:</span><span class="sxs-lookup"><span data-stu-id="38e99-280">Inside a code block, as an alternative to using Razor comment syntax, you can use the commenting syntax of the programming language you're using, such as C#:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample22.cshtml)]

<span data-ttu-id="38e99-281">En C#, los comentarios de una línea están precedidos por la `//` caracteres y los comentarios de varias líneas que comienzan por `/*` y terminar por `*/`.</span><span class="sxs-lookup"><span data-stu-id="38e99-281">In C#, single-line comments are preceded by the `//` characters, and multi-line comments begin with `/*` and end with `*/`.</span></span> <span data-ttu-id="38e99-282">(Al igual que con los comentarios de Razor, C#, los comentarios no se representan en el explorador).</span><span class="sxs-lookup"><span data-stu-id="38e99-282">(As with Razor comments, C# comments are not rendered to the browser.)</span></span>

<span data-ttu-id="38e99-283">Para el marcado, como probablemente sabe, puede crear un comentario HTML:</span><span class="sxs-lookup"><span data-stu-id="38e99-283">For markup, as you probably know, you can create an HTML comment:</span></span>

[!code-xml[Main](introducing-razor-syntax-c/samples/sample23.xml)]

<span data-ttu-id="38e99-284">Iniciar comentarios HTML con `<!--` caracteres y terminan con `-->`.</span><span class="sxs-lookup"><span data-stu-id="38e99-284">HTML comments start with `<!--` characters and end with `-->`.</span></span> <span data-ttu-id="38e99-285">Puede utilizar comentarios HTML para rodear el texto no solo, sino también cualquier marcado HTML que quizás desee conservar en la página pero no desea representar.</span><span class="sxs-lookup"><span data-stu-id="38e99-285">You can use HTML comments to surround not only text, but also any HTML markup that you may want to keep in the page but don't want to render.</span></span> <span data-ttu-id="38e99-286">Este comentario HTML ocultará todo el contenido de las etiquetas y el texto que contienen:</span><span class="sxs-lookup"><span data-stu-id="38e99-286">This HTML comment will hide the entire content of the tags and the text they contain:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample24.html)]

<span data-ttu-id="38e99-287">A diferencia de los comentarios de Razor, comentarios HTML *son* representado en la página y el usuario puede ver consultando el origen de la página.</span><span class="sxs-lookup"><span data-stu-id="38e99-287">Unlike Razor comments, HTML comments *are* rendered to the page and the user can see them by viewing the page source.</span></span>

<span data-ttu-id="38e99-288">Razor presenta limitaciones en los bloques anidados de C#.</span><span class="sxs-lookup"><span data-stu-id="38e99-288">Razor has limitations on nested blocks of C#.</span></span> <span data-ttu-id="38e99-289">Para obtener más información, consulte [denominado las Variables de C# y anidadas bloquea generar divide el código](http://aspnetwebstack.codeplex.com/workitem/1914)</span><span class="sxs-lookup"><span data-stu-id="38e99-289">For more information see [Named C# Variables and Nested Blocks Generate Broken Code](http://aspnetwebstack.codeplex.com/workitem/1914)</span></span>

## <a name="variables"></a><span data-ttu-id="38e99-290">Variables</span><span class="sxs-lookup"><span data-stu-id="38e99-290">Variables</span></span>

<span data-ttu-id="38e99-291">Una variable es un objeto con nombre que usa para almacenar los datos.</span><span class="sxs-lookup"><span data-stu-id="38e99-291">A variable is a named object that you use to store data.</span></span> <span data-ttu-id="38e99-292">Puede asignar variables, pero el nombre debe comenzar con un carácter alfabético y no puede contener espacios en blanco o caracteres reservados.</span><span class="sxs-lookup"><span data-stu-id="38e99-292">You can name variables anything, but the name must begin with an alphabetic character and it cannot contain whitespace or reserved characters.</span></span>

### <a name="variables-and-data-types"></a><span data-ttu-id="38e99-293">Las variables y tipos de datos</span><span class="sxs-lookup"><span data-stu-id="38e99-293">Variables and Data Types</span></span>

<span data-ttu-id="38e99-294">Una variable puede tener un tipo de datos específico, lo que indica qué tipo de datos se almacena en la variable.</span><span class="sxs-lookup"><span data-stu-id="38e99-294">A variable can have a specific data type, which indicates what kind of data is stored in the variable.</span></span> <span data-ttu-id="38e99-295">Puede tener variables de cadena que almacenan valores de cadena (como &quot;Hola mundo&quot;), las variables de entero que almacenan valores de número entero (por ejemplo, 3 o 79) y las variables de fecha que almacenan los valores de fecha en una variedad de formatos (por ejemplo, 12/4/2012 o de marzo de 2009 ).</span><span class="sxs-lookup"><span data-stu-id="38e99-295">You can have string variables that store string values (like &quot;Hello world&quot;), integer variables that store whole-number values (like 3 or 79), and date variables that store date values in a variety of formats (like 4/12/2012 or March 2009).</span></span> <span data-ttu-id="38e99-296">Y hay muchos otros tipos de datos que se puede usar.</span><span class="sxs-lookup"><span data-stu-id="38e99-296">And there are many other data types you can use.</span></span>

<span data-ttu-id="38e99-297">Sin embargo, generalmente no debe especificar un tipo para una variable.</span><span class="sxs-lookup"><span data-stu-id="38e99-297">However, you generally don't have to specify a type for a variable.</span></span> <span data-ttu-id="38e99-298">La mayoría de las veces, ASP.NET puede deducir el tipo según cómo se utilizan los datos en la variable.</span><span class="sxs-lookup"><span data-stu-id="38e99-298">Most of the time, ASP.NET can figure out the type based on how the data in the variable is being used.</span></span> <span data-ttu-id="38e99-299">(En ocasiones, debe especificar un tipo; verá ejemplos donde esto es cierto).</span><span class="sxs-lookup"><span data-stu-id="38e99-299">(Occasionally you must specify a type; you'll see examples where this is true.)</span></span>

<span data-ttu-id="38e99-300">Declarar una variable utilizando el `var` palabra clave (si no desea especificar un tipo) o con el nombre del tipo:</span><span class="sxs-lookup"><span data-stu-id="38e99-300">You declare a variable using the `var` keyword (if you don't want to specify a type) or by using the name of the type:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample25.cshtml)]

<span data-ttu-id="38e99-301">El ejemplo siguiente muestra los usos típicos de las variables en una página web:</span><span class="sxs-lookup"><span data-stu-id="38e99-301">The following example shows some typical uses of variables in a web page:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample26.cshtml)]

<span data-ttu-id="38e99-302">Si combina los ejemplos anteriores en una página, verá esto aparece en un explorador:</span><span class="sxs-lookup"><span data-stu-id="38e99-302">If you combine the previous examples in a page, you see this displayed in a browser:</span></span>

![Razor Img9](introducing-razor-syntax-c/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a><span data-ttu-id="38e99-304">Convertir y pruebas de tipos de datos</span><span class="sxs-lookup"><span data-stu-id="38e99-304">Converting and Testing Data Types</span></span>

<span data-ttu-id="38e99-305">Aunque ASP.NET normalmente pueden determinar automáticamente un tipo de datos, a veces, no puede.</span><span class="sxs-lookup"><span data-stu-id="38e99-305">Although ASP.NET can usually determine a data type automatically, sometimes it can't.</span></span> <span data-ttu-id="38e99-306">Por lo tanto, es posible que deba ayudarnos ASP.NET mediante la realización de una conversión explícita.</span><span class="sxs-lookup"><span data-stu-id="38e99-306">Therefore, you might need to help ASP.NET out by performing an explicit conversion.</span></span> <span data-ttu-id="38e99-307">Incluso si no tiene que convertir los tipos, a veces resulta útil probar para ver qué tipo de datos podría estar trabajando con.</span><span class="sxs-lookup"><span data-stu-id="38e99-307">Even if you don't have to convert types, sometimes it's helpful to test to see what type of data you might be working with.</span></span>

<span data-ttu-id="38e99-308">El caso más común es que se debe convertir una cadena a otro tipo, como en un entero o una fecha.</span><span class="sxs-lookup"><span data-stu-id="38e99-308">The most common case is that you have to convert a string to another type, such as to an integer or date.</span></span> <span data-ttu-id="38e99-309">El ejemplo siguiente muestra un caso típico que debe convertir una cadena en un número.</span><span class="sxs-lookup"><span data-stu-id="38e99-309">The following example shows a typical case where you must convert a string to a number.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample27.cshtml)]

<span data-ttu-id="38e99-310">Como norma, proporcionados por el usuario aparecerán como cadenas.</span><span class="sxs-lookup"><span data-stu-id="38e99-310">As a rule, user input comes to you as strings.</span></span> <span data-ttu-id="38e99-311">Incluso si le los usuarios escriban un número y, incluso si ha especificado un dígito, cuando se envía la entrada del usuario y leerlo en el código, los datos están en formato de cadena.</span><span class="sxs-lookup"><span data-stu-id="38e99-311">Even if you've prompted users to enter a number, and even if they've entered a digit, when user input is submitted and you read it in code, the data is in string format.</span></span> <span data-ttu-id="38e99-312">Por lo tanto, debe convertir la cadena en un número.</span><span class="sxs-lookup"><span data-stu-id="38e99-312">Therefore, you must convert the string to a number.</span></span> <span data-ttu-id="38e99-313">En el ejemplo, si se intenta realizar operaciones aritméticas en los valores sin necesidad de convertirlos, produce el siguiente error, porque ASP.NET no se puede agregar dos cadenas:</span><span class="sxs-lookup"><span data-stu-id="38e99-313">In the example, if you try to perform arithmetic on the values without converting them, the following error results, because ASP.NET cannot add two strings:</span></span>

<span data-ttu-id="38e99-314">*No se puede convertir implícitamente el tipo 'string' a 'int'.*</span><span class="sxs-lookup"><span data-stu-id="38e99-314">*Cannot implicitly convert type 'string' to 'int'.*</span></span>

<span data-ttu-id="38e99-315">Para convertir los valores enteros, se llama a la `AsInt` método.</span><span class="sxs-lookup"><span data-stu-id="38e99-315">To convert the values to integers, you call the `AsInt` method.</span></span> <span data-ttu-id="38e99-316">Si la conversión se realiza correctamente, a continuación, puede agregar los números.</span><span class="sxs-lookup"><span data-stu-id="38e99-316">If the conversion is successful, you can then add the numbers.</span></span>

<span data-ttu-id="38e99-317">En la tabla siguiente se enumera algunos métodos de conversión y prueba comunes para las variables.</span><span class="sxs-lookup"><span data-stu-id="38e99-317">The following table lists some common conversion and test methods for variables.</span></span>

:::row:::
    :::column:::
        <strong>Method</strong>
    :::column-end:::
    :::column:::
        <strong>Description</strong>
    :::column-end:::
    :::column:::
        <strong>Example</strong>
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsInt(), IsInt()`
    :::column-end:::
    :::column:::
        Converts a string that represents a whole number (like "593") to an integer.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample28.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsBool(), IsBool()`
    :::column-end:::
    :::column:::
        Converts a string like &quot;true&quot; or &quot;false&quot; to a Boolean type.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample29.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsFloat(), IsFloat()`
    :::column-end:::
    :::column:::
        Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a floating-point number.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample30.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsDecimal(), IsDecimal()`
    :::column-end:::
    :::column:::
        Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a decimal number. (In ASP.NET, a decimal number is more precise than a floating-point number.)
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample31.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsDateTime(), IsDateTime()`
    :::column-end:::
    :::column:::
        Converts a string that represents a date and time value to the ASP.NET `DateTime` type.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample32.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `ToString()`
    :::column-end:::
    :::column:::
        Converts any other data type to a string.
    :::column-end:::
    :::column:::
        [!code-javascript[Main](introducing-razor-syntax-c/samples/sample33.js)]
    :::column-end:::
:::row-end:::

## <a name="operators"></a><span data-ttu-id="38e99-318">Operadores</span><span class="sxs-lookup"><span data-stu-id="38e99-318">Operators</span></span>

<span data-ttu-id="38e99-319">Un operador es una palabra clave o el carácter que le indica a ASP.NET qué tipo de comando que se ejecuta en una expresión.</span><span class="sxs-lookup"><span data-stu-id="38e99-319">An operator is a keyword or character that tells ASP.NET what kind of command to perform in an expression.</span></span> <span data-ttu-id="38e99-320">El lenguaje C# (y la sintaxis de Razor que se basa en él) admiten muchos operadores, pero deberá reconocer algunas para comenzar.</span><span class="sxs-lookup"><span data-stu-id="38e99-320">The C# language (and the Razor syntax that's based on it) supports many operators, but you only need to recognize a few to get started.</span></span> <span data-ttu-id="38e99-321">En la tabla siguiente se resume los operadores más comunes.</span><span class="sxs-lookup"><span data-stu-id="38e99-321">The following table summarizes the most common operators.</span></span>


:::row:::
    :::column:::
        <strong>Operator</strong>
    :::column-end:::
    :::column:::
        <strong>Description</strong>
    :::column-end:::
    :::column:::
        <strong>Examples</strong>
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+` `-` `*` `/`
    :::column-end:::
    :::column:::
        Math operators used in numerical expressions.
    :::column-end:::
    :::column:::
        [!code-css[Main](introducing-razor-syntax-c/samples/sample34.css)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `=`
    :::column-end:::
    :::column:::
        Assignment. Assigns the value on the right side of a statement to the object on the left side.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample35.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `==`
    :::column-end:::
    :::column:::
        Equality. Returns `true` if the values are equal. (Notice the distinction between the `=` operator and the `==` operator.)
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample36.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `!=`
    :::column-end:::
    :::column:::
        Inequality. Returns `true` if the values are not equal.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample37.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `< > <= >=`
    :::column-end:::
    :::column:::
        Less-than, greater-than, less-than-or-equal, and greater-than-or-equal.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample38.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+`
    :::column-end:::
    :::column:::
        Concatenation, which is used to join strings. ASP.NET knows the difference between this operator and the addition operator based on the data type of the expression.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample39.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+=` `-=`
    :::column-end:::
    :::column:::
        The increment and decrement operators, which add and subtract 1 (respectively) from a variable.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample40.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `.`
    :::column-end:::
    :::column:::
        Dot. Used to distinguish objects and their properties and methods.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample41.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `()`
    :::column-end:::
    :::column:::
        Parentheses. Used to group expressions and to pass parameters to methods.
    :::column-end:::
    :::column:::
        [!code-javascript[Main](introducing-razor-syntax-c/samples/sample42.js)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `[]`
    :::column-end:::
    :::column:::
        Brackets. Used for accessing values in arrays or collections.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample43.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `!`
    :::column-end:::
    :::column:::
        Not. Reverses a `true` value to `false` and vice versa. Typically used as a shorthand way to test for `false` (that is, for not `true`).
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample44.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `&&` <code>&#124;&#124;</code>
    :::column-end:::
    :::column:::
        Logical AND and OR, which are used to link conditions together.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample45.cs)]
    :::column-end:::
:::row-end:::

<a id="ID_WorkingWithFileAndFolderPaths"></a>
## <a name="working-with-file-and-folder-paths-in-code"></a><span data-ttu-id="38e99-322">Trabajar con archivos y rutas de acceso de carpeta en el código</span><span class="sxs-lookup"><span data-stu-id="38e99-322">Working with File and Folder Paths in Code</span></span>

<span data-ttu-id="38e99-323">A menudo a trabajar con rutas de acceso de archivos y carpetas en el código.</span><span class="sxs-lookup"><span data-stu-id="38e99-323">You'll often work with file and folder paths in your code.</span></span> <span data-ttu-id="38e99-324">Este es un ejemplo de la estructura de la carpeta física de un sitio Web como podría aparecer en el equipo de desarrollo:</span><span class="sxs-lookup"><span data-stu-id="38e99-324">Here is an example of physical folder structure for a website as it might appear on your development computer:</span></span>

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

<span data-ttu-id="38e99-325">Estos son algunos detalles esenciales sobre las direcciones URL y rutas de acceso:</span><span class="sxs-lookup"><span data-stu-id="38e99-325">Here are some essential details about URLs and paths:</span></span>

- <span data-ttu-id="38e99-326">Una dirección URL comienza con cualquiera de un nombre de dominio (`http://www.example.com`) o un nombre de servidor (`http://localhost`, `http://mycomputer`).</span><span class="sxs-lookup"><span data-stu-id="38e99-326">A URL begins with either a domain name (`http://www.example.com`) or a server name (`http://localhost`, `http://mycomputer`).</span></span>
- <span data-ttu-id="38e99-327">Una dirección URL que se corresponde con una ruta de acceso física en un equipo host.</span><span class="sxs-lookup"><span data-stu-id="38e99-327">A URL corresponds to a physical path on a host computer.</span></span> <span data-ttu-id="38e99-328">Por ejemplo, `http://myserver` pueden corresponder a la carpeta *C:\websites\mywebsite* en el servidor.</span><span class="sxs-lookup"><span data-stu-id="38e99-328">For example, `http://myserver` might correspond to the folder *C:\websites\mywebsite* on the server.</span></span>
- <span data-ttu-id="38e99-329">Una ruta de acceso virtual es una abreviatura para representar las rutas de acceso en el código sin tener que especificar la ruta de acceso completa.</span><span class="sxs-lookup"><span data-stu-id="38e99-329">A virtual path is shorthand to represent paths in code without having to specify the full path.</span></span> <span data-ttu-id="38e99-330">Incluye la parte de una dirección URL que sigue al nombre de dominio o servidor.</span><span class="sxs-lookup"><span data-stu-id="38e99-330">It includes the portion of a URL that follows the domain or server name.</span></span> <span data-ttu-id="38e99-331">Al usar rutas de acceso virtuales, puede mover el código a un dominio diferente o un servidor sin tener que actualizar las rutas de acceso.</span><span class="sxs-lookup"><span data-stu-id="38e99-331">When you use virtual paths, you can move your code to a different domain or server without having to update the paths.</span></span>

<span data-ttu-id="38e99-332">Este es un ejemplo que le ayudarán a comprender las diferencias:</span><span class="sxs-lookup"><span data-stu-id="38e99-332">Here's an example to help you understand the differences:</span></span>

| <span data-ttu-id="38e99-333">Dirección URL completa</span><span class="sxs-lookup"><span data-stu-id="38e99-333">Complete URL</span></span> | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| <span data-ttu-id="38e99-334">Nombre del servidor</span><span class="sxs-lookup"><span data-stu-id="38e99-334">Server name</span></span> | <span data-ttu-id="38e99-335">*mycompanyserver*</span><span class="sxs-lookup"><span data-stu-id="38e99-335">*mycompanyserver*</span></span> |
| <span data-ttu-id="38e99-336">Ruta de acceso virtual</span><span class="sxs-lookup"><span data-stu-id="38e99-336">Virtual path</span></span> | <span data-ttu-id="38e99-337">*/HumanResources/CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="38e99-337">*/humanresources/CompanyPolicy.htm*</span></span> |
| <span data-ttu-id="38e99-338">Ruta de acceso física</span><span class="sxs-lookup"><span data-stu-id="38e99-338">Physical path</span></span> | <span data-ttu-id="38e99-339">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="38e99-339">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span></span> |

<span data-ttu-id="38e99-340">Es la raíz virtual /, al igual que la raíz de la unidad C: es la unidad \.</span><span class="sxs-lookup"><span data-stu-id="38e99-340">The virtual root is /, just like the root of your C: drive is \.</span></span> <span data-ttu-id="38e99-341">(Las rutas de acceso de la carpeta virtual siempre usar barras diagonales). La ruta de acceso virtual de una carpeta no debe tener el mismo nombre que la carpeta física; puede ser un alias.</span><span class="sxs-lookup"><span data-stu-id="38e99-341">(Virtual folder paths always use forward slashes.) The virtual path of a folder doesn't have to have the same name as the physical folder; it can be an alias.</span></span> <span data-ttu-id="38e99-342">(En los servidores de producción, la ruta de acceso virtual rara vez coincide con una ruta de acceso física exacta.)</span><span class="sxs-lookup"><span data-stu-id="38e99-342">(On production servers, the virtual path rarely matches an exact physical path.)</span></span>

<span data-ttu-id="38e99-343">A veces, cuando se trabaja con archivos y carpetas en el código, debe hacer referencia a la ruta de acceso física y a veces, una ruta de acceso virtual, dependiendo de los objetos que se trabaja con.</span><span class="sxs-lookup"><span data-stu-id="38e99-343">When you work with files and folders in code, sometimes you need to reference the physical path and sometimes a virtual path, depending on what objects you're working with.</span></span> <span data-ttu-id="38e99-344">ASP.NET proporciona estas herramientas para trabajar con rutas de acceso de archivos y carpetas en el código: el `Server.MapPath` método y el `~` operador y `Href` método.</span><span class="sxs-lookup"><span data-stu-id="38e99-344">ASP.NET gives you these tools for working with file and folder paths in code: the `Server.MapPath` method, and the `~` operator and `Href` method.</span></span>

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a><span data-ttu-id="38e99-345">Convertir rutas de acceso virtuales a físico: el método Server.MapPath</span><span class="sxs-lookup"><span data-stu-id="38e99-345">Converting virtual to physical paths: the Server.MapPath method</span></span>

<span data-ttu-id="38e99-346">El `Server.MapPath` método convierte una ruta de acceso virtual (como */default.cshtml*) a una ruta de acceso física absoluta (como *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="38e99-346">The `Server.MapPath` method converts a virtual path (like */default.cshtml*) to an absolute physical path (like *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span></span> <span data-ttu-id="38e99-347">Utilice este método cada vez que necesite una ruta de acceso física completa.</span><span class="sxs-lookup"><span data-stu-id="38e99-347">You use this method any time you need a complete physical path.</span></span> <span data-ttu-id="38e99-348">Un ejemplo típico es cuando se va a leer o escribir un archivo de texto o imagen en el servidor web.</span><span class="sxs-lookup"><span data-stu-id="38e99-348">A typical example is when you're reading or writing a text file or image file on the web server.</span></span>

<span data-ttu-id="38e99-349">Normalmente no conoce la ruta de acceso física absoluta de su sitio en el servidor de hospedaje de un sitio, por lo que este método puede convertir la ruta de acceso es saber, la ruta de acceso virtual, la ruta de acceso correspondiente en el servidor para usted.</span><span class="sxs-lookup"><span data-stu-id="38e99-349">You typically don't know the absolute physical path of your site on a hosting site's server, so this method can convert the path you do know — the virtual path — to the corresponding path on the server for you.</span></span> <span data-ttu-id="38e99-350">Pase la ruta de acceso virtual a un archivo o carpeta al método y devuelve la ruta de acceso física:</span><span class="sxs-lookup"><span data-stu-id="38e99-350">You pass the virtual path to a file or folder to the method, and it returns the physical path:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample46.cshtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a><span data-ttu-id="38e99-351">Hacer referencia a la raíz virtual: el ~ operador y Href (método)</span><span class="sxs-lookup"><span data-stu-id="38e99-351">Referencing the virtual root: the ~ operator and Href method</span></span>

<span data-ttu-id="38e99-352">En un *.cshtml* o *.vbhtml* archivo, puede hacer referencia a la ruta de acceso virtual raíz mediante el `~` operador.</span><span class="sxs-lookup"><span data-stu-id="38e99-352">In a *.cshtml* or *.vbhtml* file, you can reference the virtual root path using the `~` operator.</span></span> <span data-ttu-id="38e99-353">Esto es muy útil porque puede mover las páginas en un sitio y los vínculos que contienen a otras páginas no se interrumpe.</span><span class="sxs-lookup"><span data-stu-id="38e99-353">This is very handy because you can move pages around in a site, and any links they contain to other pages won't be broken.</span></span> <span data-ttu-id="38e99-354">También es útil en caso de alguna vez mover su sitio Web a una ubicación diferente.</span><span class="sxs-lookup"><span data-stu-id="38e99-354">It's also handy in case you ever move your website to a different location.</span></span> <span data-ttu-id="38e99-355">A continuación se muestran algunos ejemplos:</span><span class="sxs-lookup"><span data-stu-id="38e99-355">Here are some examples:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample47.cshtml)]

<span data-ttu-id="38e99-356">Si el sitio Web está `http://myserver/myapp`, le mostramos cómo ASP.NET tratan estas rutas de acceso cuando se ejecuta la página:</span><span class="sxs-lookup"><span data-stu-id="38e99-356">If the website is `http://myserver/myapp`, here's how ASP.NET will treat these paths when the page runs:</span></span>

- <span data-ttu-id="38e99-357">`myImagesFolder`: `http://myserver/myapp/images`</span><span class="sxs-lookup"><span data-stu-id="38e99-357">`myImagesFolder`: `http://myserver/myapp/images`</span></span>
- <span data-ttu-id="38e99-358">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span><span class="sxs-lookup"><span data-stu-id="38e99-358">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span></span>

<span data-ttu-id="38e99-359">(En realidad, no verá estas rutas de acceso que los valores de la variable, pero ASP.NET tratará las rutas de acceso como si eso es lo que fueran).</span><span class="sxs-lookup"><span data-stu-id="38e99-359">(You won't actually see these paths as the values of the variable, but ASP.NET will treat the paths as if that's what they were.)</span></span>

<span data-ttu-id="38e99-360">Puede usar el `~` operador tanto en código del servidor (como anteriormente) en el marcado, similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="38e99-360">You can use the `~` operator both in server code (as above) and in markup, like this:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample48.html)]

<span data-ttu-id="38e99-361">En el marcado, usa el `~` operador para crear rutas de acceso a recursos como archivos de imagen, otras páginas web y archivos CSS.</span><span class="sxs-lookup"><span data-stu-id="38e99-361">In markup, you use the `~` operator to create paths to resources like image files, other web pages, and CSS files.</span></span> <span data-ttu-id="38e99-362">Cuando se ejecuta la página, ASP.NET busca en la página (marcado y código) y resuelve todos los `~` referencias a la ruta de acceso adecuada.</span><span class="sxs-lookup"><span data-stu-id="38e99-362">When the page runs, ASP.NET looks through the page (both code and markup) and resolves all the `~` references to the appropriate path.</span></span>

## <a name="conditional-logic-and-loops"></a><span data-ttu-id="38e99-363">Bucles y la lógica condicional</span><span class="sxs-lookup"><span data-stu-id="38e99-363">Conditional Logic and Loops</span></span>

<span data-ttu-id="38e99-364">Código de servidor ASP.NET le permite realizar tareas en función de condiciones y escribir código que se repite las instrucciones de un número específico de veces (es decir, código que se ejecuta un bucle).</span><span class="sxs-lookup"><span data-stu-id="38e99-364">ASP.NET server code lets you perform tasks based on conditions and write code that repeats statements a specific number of times (that is, code that runs a loop).</span></span>

### <a name="testing-conditions"></a><span data-ttu-id="38e99-365">Condiciones de pruebas</span><span class="sxs-lookup"><span data-stu-id="38e99-365">Testing Conditions</span></span>

<span data-ttu-id="38e99-366">Para probar una condición sencilla que use el `if` instrucción, que devuelve true o false en función de una prueba que especifique:</span><span class="sxs-lookup"><span data-stu-id="38e99-366">To test a simple condition you use the `if` statement, which returns true or false based on a test you specify:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample49.cshtml)]

<span data-ttu-id="38e99-367">El `if` palabra clave inicia un bloque.</span><span class="sxs-lookup"><span data-stu-id="38e99-367">The `if` keyword starts a block.</span></span> <span data-ttu-id="38e99-368">La prueba real (condición) entre paréntesis y devuelve true o false.</span><span class="sxs-lookup"><span data-stu-id="38e99-368">The actual test (condition) is in parentheses and returns true or false.</span></span> <span data-ttu-id="38e99-369">Las instrucciones que se ejecutan si se cumple la prueba están entre llaves.</span><span class="sxs-lookup"><span data-stu-id="38e99-369">The statements that run if the test is true are enclosed in braces.</span></span> <span data-ttu-id="38e99-370">Un `if` instrucción puede incluir un `else` bloque que especifica las instrucciones que se ejecutarán si la condición es falsa:</span><span class="sxs-lookup"><span data-stu-id="38e99-370">An `if` statement can include an `else` block that specifies statements to run if the condition is false:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample50.cshtml)]

<span data-ttu-id="38e99-371">Puede agregar varias condiciones mediante un `else if` bloque:</span><span class="sxs-lookup"><span data-stu-id="38e99-371">You can add multiple conditions using an `else if` block:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample51.cshtml)]

<span data-ttu-id="38e99-372">En este ejemplo, si la primera condición de if bloque no es true, el `else if` se comprueba la condición.</span><span class="sxs-lookup"><span data-stu-id="38e99-372">In this example, if the first condition in the if block is not true, the `else if` condition is checked.</span></span> <span data-ttu-id="38e99-373">Si se cumple esa condición, las instrucciones en el `else if` bloque se ejecutan.</span><span class="sxs-lookup"><span data-stu-id="38e99-373">If that condition is met, the statements in the `else if` block are executed.</span></span> <span data-ttu-id="38e99-374">Si se cumple ninguna de las condiciones, las instrucciones en el `else` bloque se ejecutan.</span><span class="sxs-lookup"><span data-stu-id="38e99-374">If none of the conditions are met, the statements in the `else` block are executed.</span></span> <span data-ttu-id="38e99-375">Puede agregar cualquier número de o si bloquea y, a continuación, cierre con un `else` bloquear como el &quot;todo lo demás&quot; condición.</span><span class="sxs-lookup"><span data-stu-id="38e99-375">You can add any number of else if blocks, and then close with an `else` block as the &quot;everything else&quot; condition.</span></span>

<span data-ttu-id="38e99-376">Para probar un gran número de condiciones, use un `switch` bloque:</span><span class="sxs-lookup"><span data-stu-id="38e99-376">To test a large number of conditions, use a `switch` block:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample52.cshtml)]

<span data-ttu-id="38e99-377">El valor para comprobar que esté entre paréntesis (en el ejemplo, el `weekday` variable).</span><span class="sxs-lookup"><span data-stu-id="38e99-377">The value to test is in parentheses (in the example, the `weekday` variable).</span></span> <span data-ttu-id="38e99-378">Cada prueba individual usa un `case` instrucción que finaliza con un signo de dos puntos (:).</span><span class="sxs-lookup"><span data-stu-id="38e99-378">Each individual test uses a `case` statement that ends with a colon (:).</span></span> <span data-ttu-id="38e99-379">Si el valor de un `case` instrucción coincide con el valor de prueba, se ejecuta el código en ese bloque de casos.</span><span class="sxs-lookup"><span data-stu-id="38e99-379">If the value of a `case` statement matches the test value, the code in that case block is executed.</span></span> <span data-ttu-id="38e99-380">Cerrar cada instrucción case con una `break` instrucción.</span><span class="sxs-lookup"><span data-stu-id="38e99-380">You close each case statement with a `break` statement.</span></span> <span data-ttu-id="38e99-381">(Si se olvida de incluir la interrupción en cada `case` bloquea, el código del siguiente `case` instrucción se ejecutará también.) Un `switch` bloque tiene a menudo un `default` instrucción como el último caso para un &quot;todo lo demás&quot; opción que se ejecuta si se cumple ninguno de los demás casos.</span><span class="sxs-lookup"><span data-stu-id="38e99-381">(If you forget to include break in each `case` block, the code from the next `case` statement will run also.) A `switch` block often has a `default` statement as the last case for an &quot;everything else&quot; option that runs if none of the other cases are true.</span></span>

<span data-ttu-id="38e99-382">El resultado de los dos últimos bloques condicionales mostrada en un explorador:</span><span class="sxs-lookup"><span data-stu-id="38e99-382">The result of the last two conditional blocks displayed in a browser:</span></span>

![Razor-Img10](introducing-razor-syntax-c/_static/image10.jpg)

### <a name="looping-code"></a><span data-ttu-id="38e99-384">Código de bucles</span><span class="sxs-lookup"><span data-stu-id="38e99-384">Looping Code</span></span>

<span data-ttu-id="38e99-385">A menudo necesitará ejecutar repetidamente las mismas instrucciones.</span><span class="sxs-lookup"><span data-stu-id="38e99-385">You often need to run the same statements repeatedly.</span></span> <span data-ttu-id="38e99-386">La forma de hacerlo es mediante un bucle.</span><span class="sxs-lookup"><span data-stu-id="38e99-386">You do this by looping.</span></span> <span data-ttu-id="38e99-387">Por ejemplo, se ejecuta con frecuencia las mismas instrucciones para cada elemento en una colección de datos.</span><span class="sxs-lookup"><span data-stu-id="38e99-387">For example, you often run the same statements for each item in a collection of data.</span></span> <span data-ttu-id="38e99-388">Si sabe exactamente cuántas veces desea crear un bucle, puede usar un `for` bucle.</span><span class="sxs-lookup"><span data-stu-id="38e99-388">If you know exactly how many times you want to loop, you can use a `for` loop.</span></span> <span data-ttu-id="38e99-389">Este tipo de bucle es especialmente útil para contar o cuenta atrás:</span><span class="sxs-lookup"><span data-stu-id="38e99-389">This kind of loop is especially useful for counting up or counting down:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample53.html)]

<span data-ttu-id="38e99-390">El bucle comienza con la `for` palabra clave, seguida de tres instrucciones entre paréntesis, cada una termina con un punto y coma.</span><span class="sxs-lookup"><span data-stu-id="38e99-390">The loop begins with the `for` keyword, followed by three statements in parentheses, each terminated with a semicolon.</span></span>

- <span data-ttu-id="38e99-391">Dentro de los paréntesis, la primera instrucción (`var i=10;`) crea un contador y la inicializa a 10.</span><span class="sxs-lookup"><span data-stu-id="38e99-391">Inside the parentheses, the first statement (`var i=10;`) creates a counter and initializes it to 10.</span></span> <span data-ttu-id="38e99-392">No tiene un nombre de contador `i` &#8212; puede usar cualquier variable.</span><span class="sxs-lookup"><span data-stu-id="38e99-392">You don't have to name the counter `i` &#8212; you can use any variable.</span></span> <span data-ttu-id="38e99-393">Cuando el `for` bucle se ejecuta, el contador se incrementa automáticamente.</span><span class="sxs-lookup"><span data-stu-id="38e99-393">When the `for` loop runs, the counter is automatically incremented.</span></span>
- <span data-ttu-id="38e99-394">La segunda instrucción (`i < 21;`) establece la condición de hasta qué punto desea contar.</span><span class="sxs-lookup"><span data-stu-id="38e99-394">The second statement (`i < 21;`) sets the condition for how far you want to count.</span></span> <span data-ttu-id="38e99-395">En este caso, tiene que ir a un máximo de 20 (es decir, continúe mientras el contador es inferior a 21).</span><span class="sxs-lookup"><span data-stu-id="38e99-395">In this case, you want it to go to a maximum of 20 (that is, keep going while the counter is less than 21).</span></span>
- <span data-ttu-id="38e99-396">La tercera instrucción (`i++` ) usa un operador de incremento, que simplemente especifica que el contador debe tener 1 le agrega cada vez que se ejecuta el bucle.</span><span class="sxs-lookup"><span data-stu-id="38e99-396">The third statement (`i++` ) uses an increment operator, which simply specifies that the counter should have 1 added to it each time the loop runs.</span></span>

<span data-ttu-id="38e99-397">Dentro de las llaves es el código que se ejecutará en cada iteración del bucle.</span><span class="sxs-lookup"><span data-stu-id="38e99-397">Inside the braces is the code that will run for each iteration of the loop.</span></span> <span data-ttu-id="38e99-398">El marcado crea un nuevo párrafo (`<p>` elemento) cada vez y se agrega una línea a la salida, mostrar el valor de `i` (el contador).</span><span class="sxs-lookup"><span data-stu-id="38e99-398">The markup creates a new paragraph (`<p>` element) each time and adds a line to the output, displaying the value of `i` (the counter).</span></span> <span data-ttu-id="38e99-399">Al ejecutar esta página, el ejemplo crea 11 líneas que muestra la salida, con el texto de cada línea que indica el número de elemento.</span><span class="sxs-lookup"><span data-stu-id="38e99-399">When you run this page, the example creates 11 lines displaying the output, with the text in each line indicating the item number.</span></span>

![Razor Img11](introducing-razor-syntax-c/_static/image11.jpg)

<span data-ttu-id="38e99-401">Si está trabajando con una colección o matriz, a menudo se usa un `foreach` bucle.</span><span class="sxs-lookup"><span data-stu-id="38e99-401">If you're working with a collection or array, you often use a `foreach` loop.</span></span> <span data-ttu-id="38e99-402">Una colección es un grupo de objetos similares y el `foreach` bucle permite llevar a cabo una tarea en cada elemento de la colección.</span><span class="sxs-lookup"><span data-stu-id="38e99-402">A collection is a group of similar objects, and the `foreach` loop lets you carry out a task on each item in the collection.</span></span> <span data-ttu-id="38e99-403">Este tipo de bucle es conveniente para las colecciones, ya que a diferencia de un `for` bucle, no tendrá que incrementar el contador o establecer un límite.</span><span class="sxs-lookup"><span data-stu-id="38e99-403">This type of loop is convenient for collections, because unlike a `for` loop, you don't have to increment the counter or set a limit.</span></span> <span data-ttu-id="38e99-404">En su lugar, el `foreach` código del bucle for simplemente continúa a través de la colección hasta que haya terminado.</span><span class="sxs-lookup"><span data-stu-id="38e99-404">Instead, the `foreach` loop code simply proceeds through the collection until it's finished.</span></span>

<span data-ttu-id="38e99-405">Por ejemplo, el código siguiente devuelve los elementos de la `Request.ServerVariables` colección, que es un objeto que contiene información sobre el servidor web.</span><span class="sxs-lookup"><span data-stu-id="38e99-405">For example, the following code returns the items in the `Request.ServerVariables` collection, which is an object that contains information about your web server.</span></span> <span data-ttu-id="38e99-406">Usa un `foreac` bucle h para mostrar el nombre de cada elemento mediante la creación de un nuevo `<li>` elemento en una lista con viñetas en HTML.</span><span class="sxs-lookup"><span data-stu-id="38e99-406">It uses a `foreac` h loop to display the name of each item by creating a new `<li>` element in an HTML bulleted list.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample54.html)]

<span data-ttu-id="38e99-407">El `foreach` palabra clave seguido de paréntesis donde se declara una variable que representa un elemento único en la colección (en el ejemplo, `var item`), seguido por el `in` palabra clave, seguida de la colección que desea para recorrer en iteración.</span><span class="sxs-lookup"><span data-stu-id="38e99-407">The `foreach` keyword is followed by parentheses where you declare a variable that represents a single item in the collection (in the example, `var item`), followed by the `in` keyword, followed by the collection you want to loop through.</span></span> <span data-ttu-id="38e99-408">En el cuerpo de la `foreach` bucle, puede obtener acceso al elemento actual utilizando la variable que declaró anteriormente.</span><span class="sxs-lookup"><span data-stu-id="38e99-408">In the body of the `foreach` loop, you can access the current item using the variable that you declared earlier.</span></span>

![Razor Img12](introducing-razor-syntax-c/_static/image12.jpg)

<span data-ttu-id="38e99-410">Para crear un bucle de uso más general, utilice el `while` instrucción:</span><span class="sxs-lookup"><span data-stu-id="38e99-410">To create a more general-purpose loop, use the `while` statement:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample55.cshtml)]

<span data-ttu-id="38e99-411">Un `while` bucle comienza con la `while` palabra clave, seguido de paréntesis en el que especificar cuánto tiempo el bucle continúa (aquí para siempre y cuando `countNum` es inferior a 50), a continuación, el bloque que se repita.</span><span class="sxs-lookup"><span data-stu-id="38e99-411">A `while` loop begins with the `while` keyword, followed by parentheses where you specify how long the loop continues (here, for as long as `countNum` is less than 50), then the block to repeat.</span></span> <span data-ttu-id="38e99-412">Bucles normalmente incrementar (agregar a) o el decremento (restar) una variable o un objeto que se usa para el recuento.</span><span class="sxs-lookup"><span data-stu-id="38e99-412">Loops typically increment (add to) or decrement (subtract from) a variable or object used for counting.</span></span> <span data-ttu-id="38e99-413">En el ejemplo, el `+=` operador de suma 1 a `countNum` cada vez que se ejecuta el bucle.</span><span class="sxs-lookup"><span data-stu-id="38e99-413">In the example, the `+=` operator adds 1 to `countNum` each time the loop runs.</span></span> <span data-ttu-id="38e99-414">(Para disminuyen una variable en un bucle que cuenta hacia atrás, usaría el operador de decremento `-=`).</span><span class="sxs-lookup"><span data-stu-id="38e99-414">(To decrement a variable in a loop that counts down, you would use the decrement operator `-=`).</span></span>

## <a name="objects-and-collections"></a><span data-ttu-id="38e99-415">Objetos y colecciones</span><span class="sxs-lookup"><span data-stu-id="38e99-415">Objects and Collections</span></span>

<span data-ttu-id="38e99-416">Casi todo el contenido de un sitio Web ASP.NET es un objeto, incluida la propia página web.</span><span class="sxs-lookup"><span data-stu-id="38e99-416">Nearly everything in an ASP.NET website is an object, including the web page itself.</span></span> <span data-ttu-id="38e99-417">En esta sección se describe algunos objetos importantes que se va a trabajar con frecuencia en el código.</span><span class="sxs-lookup"><span data-stu-id="38e99-417">This section discusses some important objects you'll work with frequently in your code.</span></span>

### <a name="page-objects"></a><span data-ttu-id="38e99-418">Objetos de página</span><span class="sxs-lookup"><span data-stu-id="38e99-418">Page Objects</span></span>

<span data-ttu-id="38e99-419">El objeto más básico de ASP.NET es la página.</span><span class="sxs-lookup"><span data-stu-id="38e99-419">The most basic object in ASP.NET is the page.</span></span> <span data-ttu-id="38e99-420">Puede tener acceso a las propiedades del objeto page directamente sin ningún objeto de calificación.</span><span class="sxs-lookup"><span data-stu-id="38e99-420">You can access properties of the page object directly without any qualifying object.</span></span> <span data-ttu-id="38e99-421">El código siguiente obtiene la ruta de acceso de archivo de la página, utilizando el `Request` objeto de la página:</span><span class="sxs-lookup"><span data-stu-id="38e99-421">The following code gets the page's file path, using the `Request` object of the page:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample56.cshtml)]

<span data-ttu-id="38e99-422">Para que sea claro que hacer referencia a propiedades y métodos en el objeto de la página actual, también puede usar la palabra clave `this` para representar el objeto de página en el código.</span><span class="sxs-lookup"><span data-stu-id="38e99-422">To make it clear that you're referencing properties and methods on the current page object, you can optionally use the keyword `this` to represent the page object in your code.</span></span> <span data-ttu-id="38e99-423">Este es el ejemplo de código anterior, con `this` agrega para representar la página:</span><span class="sxs-lookup"><span data-stu-id="38e99-423">Here is the previous code example, with `this` added to represent the page:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample57.cshtml)]

<span data-ttu-id="38e99-424">Puede usar las propiedades de la `Page` objeto para obtener una gran cantidad de información, como:</span><span class="sxs-lookup"><span data-stu-id="38e99-424">You can use properties of the `Page` object to get a lot of information, such as:</span></span>

- <span data-ttu-id="38e99-425">`Request`.</span><span class="sxs-lookup"><span data-stu-id="38e99-425">`Request`.</span></span> <span data-ttu-id="38e99-426">Como ya hemos visto, esto es una colección de información acerca de la solicitud actual, incluidos el tipo de explorador realizó la solicitud, la dirección URL de la página, la identidad del usuario, etcetera.</span><span class="sxs-lookup"><span data-stu-id="38e99-426">As you've already seen, this is a collection of information about the current request, including what type of browser made the request, the URL of the page, the user identity, etc.</span></span>
- <span data-ttu-id="38e99-427">`Response`.</span><span class="sxs-lookup"><span data-stu-id="38e99-427">`Response`.</span></span> <span data-ttu-id="38e99-428">Se trata de una colección de información sobre la respuesta (página) que se enviará al explorador cuando haya terminado de ejecutarse el código del servidor.</span><span class="sxs-lookup"><span data-stu-id="38e99-428">This is a collection of information about the response (page) that will be sent to the browser when the server code has finished running.</span></span> <span data-ttu-id="38e99-429">Por ejemplo, puede utilizar esta propiedad para escribir información en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="38e99-429">For example, you can use this property to write information into the response.</span></span> 

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample58.cshtml)]

<a id="ID_CollectionsAndObjects"></a>
### <a name="collection-objects-arrays-and-dictionaries"></a><span data-ttu-id="38e99-430">Objetos de colección (matrices y diccionarios)</span><span class="sxs-lookup"><span data-stu-id="38e99-430">Collection Objects (Arrays and Dictionaries)</span></span>

<span data-ttu-id="38e99-431">Un *colección* es un grupo de objetos del mismo tipo, como una colección de `Customer` objetos desde una base de datos.</span><span class="sxs-lookup"><span data-stu-id="38e99-431">A *collection* is a group of objects of the same type, such as a collection of `Customer` objects from a database.</span></span> <span data-ttu-id="38e99-432">ASP.NET contiene muchas colecciones integradas, como el `Request.Files` colección.</span><span class="sxs-lookup"><span data-stu-id="38e99-432">ASP.NET contains many built-in collections, like the `Request.Files` collection.</span></span>

<span data-ttu-id="38e99-433">A menudo a trabajar con datos en colecciones.</span><span class="sxs-lookup"><span data-stu-id="38e99-433">You'll often work with data in collections.</span></span> <span data-ttu-id="38e99-434">Dos tipos de colección comunes son el *matriz* y *diccionario*.</span><span class="sxs-lookup"><span data-stu-id="38e99-434">Two common collection types are the *array* and the *dictionary*.</span></span> <span data-ttu-id="38e99-435">Una matriz es útil cuando desea almacenar una colección de elementos similares, pero no desea crear una variable independiente para contener cada elemento:</span><span class="sxs-lookup"><span data-stu-id="38e99-435">An array is useful when you want to store a collection of similar items but don't want to create a separate variable to hold each item:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample59.cshtml)]

<span data-ttu-id="38e99-436">Con las matrices, declarar un tipo de datos específico, como `string`, `int`, o `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="38e99-436">With arrays, you declare a specific data type, such as `string`, `int`, or `DateTime`.</span></span> <span data-ttu-id="38e99-437">Para indicar que la variable puede contener una matriz, agregar paréntesis a la declaración (como `string[]` o `int[]`).</span><span class="sxs-lookup"><span data-stu-id="38e99-437">To indicate that the variable can contain an array, you add brackets to the declaration (such as `string[]` or `int[]`).</span></span> <span data-ttu-id="38e99-438">Puede obtener acceso a los elementos en una matriz mediante su posición (index) o mediante el `foreach` instrucción.</span><span class="sxs-lookup"><span data-stu-id="38e99-438">You can access items in an array using their position (index) or by using the `foreach` statement.</span></span> <span data-ttu-id="38e99-439">Los índices de matriz son de base cero &#8212; es decir, el primer elemento está en posición 0, el segundo elemento es en la posición 1 y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="38e99-439">Array indexes are zero-based &#8212; that is, the first item is at position 0, the second item is at position 1, and so on.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample60.cshtml)]

<span data-ttu-id="38e99-440">Puede determinar el número de elementos en una matriz mediante la obtención de su `Length` propiedad.</span><span class="sxs-lookup"><span data-stu-id="38e99-440">You can determine the number of items in an array by getting its `Length` property.</span></span> <span data-ttu-id="38e99-441">Para obtener la posición de un elemento específico de la matriz (para buscar en la matriz), use el `Array.IndexOf` método.</span><span class="sxs-lookup"><span data-stu-id="38e99-441">To get the position of a specific item in the array (to search the array), use the `Array.IndexOf` method.</span></span> <span data-ttu-id="38e99-442">También puede hacer cosas como inversa el contenido de una matriz (el `Array.Reverse` método) u ordenar el contenido (el `Array.Sort` método).</span><span class="sxs-lookup"><span data-stu-id="38e99-442">You can also do things like reverse the contents of an array (the `Array.Reverse` method) or sort the contents (the `Array.Sort` method).</span></span>

<span data-ttu-id="38e99-443">La salida del código de matriz de cadena mostrado en un explorador:</span><span class="sxs-lookup"><span data-stu-id="38e99-443">The output of the string array code displayed in a browser:</span></span>

![Razor-Img13](introducing-razor-syntax-c/_static/image13.jpg)

<span data-ttu-id="38e99-445">Un diccionario es una colección de pares clave/valor, donde proporcionar la clave (o nombre) para establecer o recuperar el valor correspondiente:</span><span class="sxs-lookup"><span data-stu-id="38e99-445">A dictionary is a collection of key/value pairs, where you provide the key (or name) to set or retrieve the corresponding value:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample61.cshtml)]

<span data-ttu-id="38e99-446">Para crear un diccionario, utilice el `new` palabra clave para indicar que está creando un nuevo objeto de diccionario.</span><span class="sxs-lookup"><span data-stu-id="38e99-446">To create a dictionary, you use the `new` keyword to indicate that you're creating a new dictionary object.</span></span> <span data-ttu-id="38e99-447">Un diccionario puede asignar a una variable mediante el `var` palabra clave.</span><span class="sxs-lookup"><span data-stu-id="38e99-447">You can assign a dictionary to a variable using the `var` keyword.</span></span> <span data-ttu-id="38e99-448">Indicar los tipos de datos de los elementos del diccionario utilizando corchetes angulares ( `< >` ).</span><span class="sxs-lookup"><span data-stu-id="38e99-448">You indicate the data types of the items in the dictionary using angle brackets ( `< >` ).</span></span> <span data-ttu-id="38e99-449">Al final de la declaración, debe agregar un par de paréntesis, ya que esto es realmente un método que crea un nuevo diccionario.</span><span class="sxs-lookup"><span data-stu-id="38e99-449">At the end of the declaration, you must add a pair of parentheses, because this is actually a method that creates a new dictionary.</span></span>

<span data-ttu-id="38e99-450">Para agregar elementos al diccionario, puede llamar a la `Add` método de la variable del diccionario (`myScores` en este caso) y, a continuación, especifique una clave y un valor.</span><span class="sxs-lookup"><span data-stu-id="38e99-450">To add items to the dictionary, you can call the `Add` method of the dictionary variable (`myScores` in this case), and then specify a key and a value.</span></span> <span data-ttu-id="38e99-451">Como alternativa, puede usar corchetes para indicar la clave y realizar una asignación sencilla, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="38e99-451">Alternatively, you can use square brackets to indicate the key and do a simple assignment, as in the following example:</span></span>

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample62.cs)]

<span data-ttu-id="38e99-452">Para obtener un valor del diccionario, especifique la clave con corchetes:</span><span class="sxs-lookup"><span data-stu-id="38e99-452">To get a value from the dictionary, you specify the key in brackets:</span></span>

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample63.cs)]

## <a name="calling-methods-with-parameters"></a><span data-ttu-id="38e99-453">Llamar a métodos con parámetros</span><span class="sxs-lookup"><span data-stu-id="38e99-453">Calling Methods with Parameters</span></span>

<span data-ttu-id="38e99-454">Cuando lea anteriormente en este artículo, los objetos que programación con pueden tener métodos.</span><span class="sxs-lookup"><span data-stu-id="38e99-454">As you read earlier in this article, the objects that you program with can have methods.</span></span> <span data-ttu-id="38e99-455">Por ejemplo, un `Database` objeto podría tener un `Database.Connect` método.</span><span class="sxs-lookup"><span data-stu-id="38e99-455">For example, a `Database` object might have a `Database.Connect` method.</span></span> <span data-ttu-id="38e99-456">Muchos métodos también tienen uno o más parámetros.</span><span class="sxs-lookup"><span data-stu-id="38e99-456">Many methods also have one or more parameters.</span></span> <span data-ttu-id="38e99-457">Un *parámetro* es un valor que se pasa a un método para habilitar el método completar su tarea.</span><span class="sxs-lookup"><span data-stu-id="38e99-457">A *parameter* is a value that you pass to a method to enable the method to complete its task.</span></span> <span data-ttu-id="38e99-458">Por ejemplo, examine una declaración para el `Request.MapPath` método, que toma tres parámetros:</span><span class="sxs-lookup"><span data-stu-id="38e99-458">For example, look at a declaration for the `Request.MapPath` method, which takes three parameters:</span></span>

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample64.cs)]

<span data-ttu-id="38e99-459">(La línea se ha ajustado para que sea más legible.</span><span class="sxs-lookup"><span data-stu-id="38e99-459">(The line has been wrapped to make it more readable.</span></span> <span data-ttu-id="38e99-460">Recuerde que puede colocar los saltos de línea casi cualquier lugar excepto dentro de cadenas que están entre comillas.)</span><span class="sxs-lookup"><span data-stu-id="38e99-460">Remember that you can put line breaks almost any place except inside strings that are enclosed in quotation marks.)</span></span>

<span data-ttu-id="38e99-461">Este método devuelve la ruta de acceso física en el servidor que corresponde a una ruta de acceso virtual especificada.</span><span class="sxs-lookup"><span data-stu-id="38e99-461">This method returns the physical path on the server that corresponds to a specified virtual path.</span></span> <span data-ttu-id="38e99-462">Los tres parámetros del método son `virtualPath`, `baseVirtualDir`, y `allowCrossAppMapping`.</span><span class="sxs-lookup"><span data-stu-id="38e99-462">The three parameters for the method are `virtualPath`, `baseVirtualDir`, and `allowCrossAppMapping`.</span></span> <span data-ttu-id="38e99-463">(Tenga en cuenta que en la declaración, se enumeran los parámetros con los tipos de datos de los datos que aceptan). Cuando se llama a este método, debe proporcionar valores para los tres parámetros.</span><span class="sxs-lookup"><span data-stu-id="38e99-463">(Notice that in the declaration, the parameters are listed with the data types of the data that they'll accept.) When you call this method, you must supply values for all three parameters.</span></span>

<span data-ttu-id="38e99-464">La sintaxis de Razor le ofrece dos opciones para pasar parámetros a un método: *parámetros posicionales* y *parámetros con nombre*.</span><span class="sxs-lookup"><span data-stu-id="38e99-464">The Razor syntax gives you two options for passing parameters to a method: *positional parameters* and *named parameters*.</span></span> <span data-ttu-id="38e99-465">Para llamar a un método con parámetros posicionales, pasar los parámetros en un orden estricto que se especifica en la declaración del método.</span><span class="sxs-lookup"><span data-stu-id="38e99-465">To call a method using positional parameters, you pass the parameters in a strict order that's specified in the method declaration.</span></span> <span data-ttu-id="38e99-466">(Normalmente, sabría este orden, lea la documentación del método.) Debe seguir el orden y no se puede omitir cualquiera de los parámetros &#8212; si es necesario, pasa una cadena vacía (`""`) o `null` para un parámetro de posición que no tienen un valor para.</span><span class="sxs-lookup"><span data-stu-id="38e99-466">(You would typically know this order by reading documentation for the method.) You must follow the order, and you can't skip any of the parameters &#8212; if necessary, you pass an empty string (`""`) or `null` for a positional parameter that you don't have a value for.</span></span>

<span data-ttu-id="38e99-467">En el siguiente ejemplo se supone que tiene una carpeta denominada *scripts* en su sitio Web.</span><span class="sxs-lookup"><span data-stu-id="38e99-467">The following example assumes you have a folder named *scripts* on your website.</span></span> <span data-ttu-id="38e99-468">El código llama a la `Request.MapPath` y pasa valores para los tres parámetros en el orden correcto.</span><span class="sxs-lookup"><span data-stu-id="38e99-468">The code calls the `Request.MapPath` method and passes values for the three parameters in the correct order.</span></span> <span data-ttu-id="38e99-469">A continuación, muestra la ruta de acceso asignado resultante.</span><span class="sxs-lookup"><span data-stu-id="38e99-469">It then displays the resulting mapped path.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample65.cshtml)]

<span data-ttu-id="38e99-470">Cuando un método que tiene muchos parámetros, puede mantener el código más legible mediante el uso de parámetros con nombre.</span><span class="sxs-lookup"><span data-stu-id="38e99-470">When a method has many parameters, you can keep your code more readable by using named parameters.</span></span> <span data-ttu-id="38e99-471">Para llamar a un método con parámetros con nombre, especifique el nombre del parámetro seguido de dos puntos (:) y, a continuación, el valor.</span><span class="sxs-lookup"><span data-stu-id="38e99-471">To call a method using named parameters, you specify the parameter name followed by a colon (:), and then the value.</span></span> <span data-ttu-id="38e99-472">La ventaja de los parámetros con nombre es el que se pueden pasar en cualquier orden que desee.</span><span class="sxs-lookup"><span data-stu-id="38e99-472">The advantage of named parameters is that you can pass them in any order you want.</span></span> <span data-ttu-id="38e99-473">(Una desventaja es que la llamada al método no es lo más compacto).</span><span class="sxs-lookup"><span data-stu-id="38e99-473">(A disadvantage is that the method call is not as compact.)</span></span>

<span data-ttu-id="38e99-474">El ejemplo siguiente llama al mismo método que el anterior, pero usa parámetros para proporcionar los valores con nombre:</span><span class="sxs-lookup"><span data-stu-id="38e99-474">The following example calls the same method as above, but uses named parameters to supply the values:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample66.cshtml)]

<span data-ttu-id="38e99-475">Como puede ver, los parámetros se pasan en un orden diferente.</span><span class="sxs-lookup"><span data-stu-id="38e99-475">As you can see, the parameters are passed in a different order.</span></span> <span data-ttu-id="38e99-476">Sin embargo, si ejecuta el ejemplo anterior y este ejemplo, devolverá el mismo valor.</span><span class="sxs-lookup"><span data-stu-id="38e99-476">However, if you run the previous example and this example, they'll return the same value.</span></span>

<a id="ID_HandlingErrors"></a>
## <a name="handling-errors"></a><span data-ttu-id="38e99-477">Control de errores</span><span class="sxs-lookup"><span data-stu-id="38e99-477">Handling Errors</span></span>

### <a name="try-catch-statements"></a><span data-ttu-id="38e99-478">Instrucciones Try-Catch</span><span class="sxs-lookup"><span data-stu-id="38e99-478">Try-Catch Statements</span></span>

<span data-ttu-id="38e99-479">A menudo tendrá las instrucciones en el código que podría producir un error por razones de fuera de su control.</span><span class="sxs-lookup"><span data-stu-id="38e99-479">You'll often have statements in your code that might fail for reasons outside your control.</span></span> <span data-ttu-id="38e99-480">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="38e99-480">For example:</span></span>

- <span data-ttu-id="38e99-481">Si el código intenta crear o acceder a un archivo, puede producirse todo tipo de errores.</span><span class="sxs-lookup"><span data-stu-id="38e99-481">If your code tries to create or access a file, all sorts of errors might occur.</span></span> <span data-ttu-id="38e99-482">No es posible que existe el archivo que desea, podría estar bloqueado, el código podría no tener los permisos y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="38e99-482">The file you want might not exist, it might be locked, the code might not have permissions, and so on.</span></span>
- <span data-ttu-id="38e99-483">De forma similar, si el código intenta actualizar registros en una base de datos, puede haber problemas con los permisos, es posible que se pierda la conexión a la base de datos, los datos que se va a guardar podrían ser no válido y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="38e99-483">Similarly, if your code tries to update records in a database, there can be permissions issues, the connection to the database might be dropped, the data to save might be invalid, and so on.</span></span>

<span data-ttu-id="38e99-484">En términos de programación, estas situaciones se conocen como *excepciones*.</span><span class="sxs-lookup"><span data-stu-id="38e99-484">In programming terms, these situations are called *exceptions*.</span></span> <span data-ttu-id="38e99-485">Si el código encuentra una excepción, genera (produce) un mensaje de error de, en el mejor, molesta a los usuarios:</span><span class="sxs-lookup"><span data-stu-id="38e99-485">If your code encounters an exception, it generates (throws) an error message that's, at best, annoying to users:</span></span>

![Razor Img14](introducing-razor-syntax-c/_static/image14.jpg)

<span data-ttu-id="38e99-487">En situaciones donde el código puede encontrar excepciones y con el fin de evitar los mensajes de error de este tipo, puede usar `try/catch` instrucciones.</span><span class="sxs-lookup"><span data-stu-id="38e99-487">In situations where your code might encounter exceptions, and in order to avoid error messages of this type, you can use `try/catch` statements.</span></span> <span data-ttu-id="38e99-488">En el `try` (instrucción), ejecute el código que está protegiendo.</span><span class="sxs-lookup"><span data-stu-id="38e99-488">In the `try` statement, you run the code that you're checking.</span></span> <span data-ttu-id="38e99-489">En uno o varios `catch` instrucciones, puede buscar de determinados errores (tipos específicos de excepciones) que pudieran haberse producido.</span><span class="sxs-lookup"><span data-stu-id="38e99-489">In one or more `catch` statements, you can look for specific errors (specific types of exceptions) that might have occurred.</span></span> <span data-ttu-id="38e99-490">Puede incluir tantos `catch` instrucciones que usted necesitan buscar errores que se prevean.</span><span class="sxs-lookup"><span data-stu-id="38e99-490">You can include as many `catch` statements as you need to look for errors that you are anticipating.</span></span>

> [!NOTE]
> <span data-ttu-id="38e99-491">Se recomienda evitar el uso de la `Response.Redirect` método `try/catch` instrucciones, ya que puede provocar una excepción en la página.</span><span class="sxs-lookup"><span data-stu-id="38e99-491">We recommend that you avoid using the `Response.Redirect` method in `try/catch` statements, because it can cause an exception in your page.</span></span>


<span data-ttu-id="38e99-492">El ejemplo siguiente muestra una página que se crea un archivo de texto en la primera solicitud y, a continuación, muestra un botón que permite al usuario abrir el archivo.</span><span class="sxs-lookup"><span data-stu-id="38e99-492">The following example shows a page that creates a text file on the first request and then displays a button that lets the user open the file.</span></span> <span data-ttu-id="38e99-493">En el ejemplo se usa deliberadamente un nombre de archivo incorrecto por lo que provocará una excepción.</span><span class="sxs-lookup"><span data-stu-id="38e99-493">The example deliberately uses a bad file name so that it will cause an exception.</span></span> <span data-ttu-id="38e99-494">El código incluye `catch` instrucciones para dos posibles excepciones: `FileNotFoundException`, que se produce si el nombre de archivo es incorrecto, y `DirectoryNotFoundException`, lo que sucederá si ASP.NET aún no se encuentra la carpeta.</span><span class="sxs-lookup"><span data-stu-id="38e99-494">The code includes `catch` statements for two possible exceptions: `FileNotFoundException`, which occurs if the file name is bad, and `DirectoryNotFoundException`, which occurs if ASP.NET can't even find the folder.</span></span> <span data-ttu-id="38e99-495">(Se puede quitar el comentario una instrucción en el ejemplo con el fin de ver cómo se ejecuta cuando todo funciona correctamente.)</span><span class="sxs-lookup"><span data-stu-id="38e99-495">(You can uncomment a statement in the example in order to see how it runs when everything works properly.)</span></span>

<span data-ttu-id="38e99-496">Si el código no controla la excepción, verá una página de error similar a la captura de pantalla anterior.</span><span class="sxs-lookup"><span data-stu-id="38e99-496">If your code didn't handle the exception, you would see an error page like the previous screen shot.</span></span> <span data-ttu-id="38e99-497">Sin embargo, la `try/catch` sección le ayuda a impedir que el usuario se vean estos tipos de errores.</span><span class="sxs-lookup"><span data-stu-id="38e99-497">However, the `try/catch` section helps prevent the user from seeing these types of errors.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample67.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="38e99-498">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="38e99-498">Additional Resources</span></span>

<span data-ttu-id="38e99-499">**Programación con Visual Basic**</span><span class="sxs-lookup"><span data-stu-id="38e99-499">**Programming with Visual Basic**</span></span>


[<span data-ttu-id="38e99-500">Apéndice: lenguaje de Visual Basic y sintaxis</span><span class="sxs-lookup"><span data-stu-id="38e99-500">Appendix: Visual Basic Language and Syntax</span></span>](https://go.microsoft.com/fwlink/?LinkId=202908)


<span data-ttu-id="38e99-501">**Documentación de referencia**</span><span class="sxs-lookup"><span data-stu-id="38e99-501">**Reference Documentation**</span></span>


[<span data-ttu-id="38e99-502">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="38e99-502">ASP.NET</span></span>](https://msdn.microsoft.com/library/ee532866.aspx)

[<span data-ttu-id="38e99-503">Lenguaje C#</span><span class="sxs-lookup"><span data-stu-id="38e99-503">C# Language</span></span>](https://msdn.microsoft.com/library/kx37x362.aspx)
