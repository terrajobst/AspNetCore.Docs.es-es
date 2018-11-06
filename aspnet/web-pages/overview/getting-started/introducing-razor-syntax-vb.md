---
uid: web-pages/overview/getting-started/introducing-razor-syntax-vb
title: Introducción a la programación Web de ASP.NET mediante la sintaxis Razor (Visual Basic) | Microsoft Docs
author: Rick-Anderson
description: Este apéndice ofrece una visión general de la programación con las páginas Web ASP.NET en Visual Basic, mediante la sintaxis de Razor.
ms.author: riande
ms.date: 02/07/2014
ms.assetid: 5da59646-e973-41cd-88a9-c6b2c0594027
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-vb
msc.type: authoredcontent
ms.openlocfilehash: 17a3a4925766b74446955a8e3a6fddbf9d29a721
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021708"
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-visual-basic"></a><span data-ttu-id="a76af-103">Introducción a la programación Web de ASP.NET mediante la sintaxis Razor (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="a76af-103">Introduction to ASP.NET Web Programming Using the Razor Syntax (Visual Basic)</span></span>
====================
<span data-ttu-id="a76af-104">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="a76af-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="a76af-105">En este artículo ofrece una visión general de la programación con ASP.NET Web Pages con la sintaxis de Razor y Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="a76af-105">This article gives you an overview of programming with ASP.NET Web Pages using the Razor syntax and Visual Basic.</span></span> <span data-ttu-id="a76af-106">ASP.NET es la tecnología de Microsoft para la ejecución de páginas web dinámicas en servidores web.</span><span class="sxs-lookup"><span data-stu-id="a76af-106">ASP.NET is Microsoft's technology for running dynamic web pages on web servers.</span></span>
> 
> <span data-ttu-id="a76af-107">**Aprenderá lo**:</span><span class="sxs-lookup"><span data-stu-id="a76af-107">**What you'll learn**:</span></span>
> 
> - <span data-ttu-id="a76af-108">El 8 de principales sugerencias de introducción a ASP.NET Web Pages con sintaxis Razor de programación de programación.</span><span class="sxs-lookup"><span data-stu-id="a76af-108">The top 8 programming tips for getting started with programming ASP.NET Web Pages using Razor syntax.</span></span>
> - <span data-ttu-id="a76af-109">Conceptos básicos de programación que necesitará.</span><span class="sxs-lookup"><span data-stu-id="a76af-109">Basic programming concepts you'll need.</span></span>
> - <span data-ttu-id="a76af-110">¿Qué código de servidor ASP.NET y la sintaxis Razor es todo acerca de.</span><span class="sxs-lookup"><span data-stu-id="a76af-110">What ASP.NET server code and the Razor syntax is all about.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="a76af-111">Versiones de software</span><span class="sxs-lookup"><span data-stu-id="a76af-111">Software versions</span></span>
> 
> 
> - <span data-ttu-id="a76af-112">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="a76af-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="a76af-113">Este tutorial también funciona con ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="a76af-113">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="a76af-114">La mayoría de los ejemplos del uso de ASP.NET Web Pages con sintaxis Razor usa C#.</span><span class="sxs-lookup"><span data-stu-id="a76af-114">Most examples of using ASP.NET Web Pages with Razor syntax use C#.</span></span> <span data-ttu-id="a76af-115">Pero la sintaxis de Razor también es compatible con Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="a76af-115">But the Razor syntax also supports Visual Basic.</span></span> <span data-ttu-id="a76af-116">Para programar una página web ASP.NET en Visual Basic, crea una página web con un *.vbhtml* extensión de nombre de archivo y, a continuación, agregue el código de Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="a76af-116">To program an ASP.NET web page in Visual Basic, you create a web page with a *.vbhtml* filename extension, and then add Visual Basic code.</span></span> <span data-ttu-id="a76af-117">En este artículo se proporciona información general de cómo trabajar con el lenguaje Visual Basic y la sintaxis para crear las páginas Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a76af-117">This article gives you an overview of working with the Visual Basic language and syntax to create ASP.NET Webpages.</span></span>

> [!NOTE]
> <span data-ttu-id="a76af-118">Las plantillas de sitio Web predeterminado de Microsoft WebMatrix (**pastelería**, **Galería fotográfica**, y **Starter Site**, etc.) están disponibles en versiones de C# y Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="a76af-118">The default website templates for Microsoft WebMatrix (**Bakery**, **Photo Gallery**, and **Starter Site**, etc.) are available in C# and Visual Basic versions.</span></span> <span data-ttu-id="a76af-119">Puede instalar las plantillas de Visual Basic, como paquetes de NuGet.</span><span class="sxs-lookup"><span data-stu-id="a76af-119">You can install the Visual Basic templates by as NuGet packages.</span></span> <span data-ttu-id="a76af-120">Plantillas de sitios Web se instalan en la carpeta raíz del sitio en una carpeta denominada *Templates Microsoft*.</span><span class="sxs-lookup"><span data-stu-id="a76af-120">Website templates are installed in the root folder of your site in a folder named *Microsoft Templates*.</span></span>


## <a name="the-top-8-programming-tips"></a><span data-ttu-id="a76af-121">El 8 principales sugerencias de programación</span><span class="sxs-lookup"><span data-stu-id="a76af-121">The Top 8 Programming Tips</span></span>

<span data-ttu-id="a76af-122">Esta sección enumeran algunas sugerencias que necesita realmente saber a medida que empiece a escribir código de servidor ASP.NET mediante la sintaxis Razor.</span><span class="sxs-lookup"><span data-stu-id="a76af-122">This section lists a few tips that you absolutely need to know as you start writing ASP.NET server code using the Razor syntax.</span></span>

### <a name="1-you-add-code-to-a-page-using-the--character"></a><span data-ttu-id="a76af-123">1. Agregue código a una página con el carácter @</span><span class="sxs-lookup"><span data-stu-id="a76af-123">1. You add code to a page using the @ character</span></span>

<span data-ttu-id="a76af-124">El `@` carácter inicia las expresiones en línea, bloques de instrucción única y bloques de múltiples instrucciones:</span><span class="sxs-lookup"><span data-stu-id="a76af-124">The `@` character starts inline expressions, single-statement blocks, and multi-statement blocks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample1.vbhtml)]

<span data-ttu-id="a76af-125">El resultado se muestra en un explorador:</span><span class="sxs-lookup"><span data-stu-id="a76af-125">The result displayed in a browser:</span></span>

![Razor Img1](introducing-razor-syntax-vb/_static/image1.jpg)

> [!TIP] 
> 
> <span data-ttu-id="a76af-127">**Codificación HTML**</span><span class="sxs-lookup"><span data-stu-id="a76af-127">**HTML Encoding**</span></span>
> 
> <span data-ttu-id="a76af-128">Al mostrar contenido en una página con el `@` de caracteres, como se muestra en los ejemplos anteriores, ASP.NET codifica en HTML el resultado.</span><span class="sxs-lookup"><span data-stu-id="a76af-128">When you display content in a page using the `@` character, as in the preceding examples, ASP.NET HTML-encodes the output.</span></span> <span data-ttu-id="a76af-129">Esto reemplaza los caracteres reservados de HTML (como `<` y `>` y `&`) con los códigos que permiten los caracteres que se va a mostrar como caracteres en una página web no se interprete como etiquetas HTML o entidades.</span><span class="sxs-lookup"><span data-stu-id="a76af-129">This replaces reserved HTML characters (such as `<` and `>` and `&`) with codes that enable the characters to be displayed as characters in a web page instead of being interpreted as HTML tags or entities.</span></span> <span data-ttu-id="a76af-130">Sin codificación HTML, la salida desde el código de servidor podría no mostrarse correctamente y podría exponer una página a riesgos de seguridad.</span><span class="sxs-lookup"><span data-stu-id="a76af-130">Without HTML encoding, the output from your server code might not display correctly, and could expose a page to security risks.</span></span>
> 
> <span data-ttu-id="a76af-131">Si su objetivo es generar marcado HTML que representa las etiquetas como marcado (por ejemplo `<p></p>` para un párrafo o `<em></em>` para resaltar el texto), consulte la sección [combinar texto, marcado y código en bloques de código](#BM_CombiningTextMarkupAndCode) más adelante en este artículo.</span><span class="sxs-lookup"><span data-stu-id="a76af-131">If your goal is to output HTML markup that renders tags as markup (for example `<p></p>` for a paragraph or `<em></em>` to emphasize text), see the section [Combining Text, Markup, and Code in Code Blocks](#BM_CombiningTextMarkupAndCode) later in this article.</span></span>
> 
> <span data-ttu-id="a76af-132">Puede leer más acerca de la codificación HTML en [trabajar con formularios HTML en sitios de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202892).</span><span class="sxs-lookup"><span data-stu-id="a76af-132">You can read more about HTML encoding in [Working with HTML Forms in ASP.NET Web Pages Sites](https://go.microsoft.com/fwlink/?LinkId=202892).</span></span>


### <a name="2-you-enclose-code-blocks-with-codeend-code"></a><span data-ttu-id="a76af-133">2. Incluir bloques de código con el código... Final de código</span><span class="sxs-lookup"><span data-stu-id="a76af-133">2. You enclose code blocks with Code...End Code</span></span>

<span data-ttu-id="a76af-134">Un bloque de código incluye una o varias instrucciones de código y se incluye con las palabras clave `Code` y `End Code`.</span><span class="sxs-lookup"><span data-stu-id="a76af-134">A code block includes one or more code statements and is enclosed with the keywords `Code` and `End Code`.</span></span> <span data-ttu-id="a76af-135">Coloque la apertura `Code` palabra clave inmediatamente después de la `@` carácter &#8212; no puede haber un espacio en blanco entre ellos.</span><span class="sxs-lookup"><span data-stu-id="a76af-135">Place the opening `Code` keyword immediately after the `@` character &#8212; there can't be whitespace between them.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample2.vbhtml)]

<span data-ttu-id="a76af-136">El resultado se muestra en un explorador:</span><span class="sxs-lookup"><span data-stu-id="a76af-136">The result displayed in a browser:</span></span>

![Razor Img2](introducing-razor-syntax-vb/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-line-break"></a><span data-ttu-id="a76af-138">3. Dentro de un bloque, finaliza cada instrucción de código con un salto de línea</span><span class="sxs-lookup"><span data-stu-id="a76af-138">3. Inside a block, you end each code statement with a line break</span></span>

<span data-ttu-id="a76af-139">En un bloque de código de Visual Basic, cada instrucción finaliza con un salto de línea.</span><span class="sxs-lookup"><span data-stu-id="a76af-139">In a Visual Basic code block, each statement ends with a line break.</span></span> <span data-ttu-id="a76af-140">(Más adelante en este artículo verá un método para rodear una instrucción de código larga en varias líneas si es necesario.)</span><span class="sxs-lookup"><span data-stu-id="a76af-140">(Later in the article you'll see a way to wrap a long code statement into multiple lines if needed.)</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample3.vbhtml)]

### <a name="4-you-use-variables-to-store-values"></a><span data-ttu-id="a76af-141">4. Usar variables para almacenar valores</span><span class="sxs-lookup"><span data-stu-id="a76af-141">4. You use variables to store values</span></span>

<span data-ttu-id="a76af-142">Puede almacenar valores en un *variable*, incluidas las cadenas, números y fechas, etcetera. Crear una nueva variable mediante el `Dim` palabra clave.</span><span class="sxs-lookup"><span data-stu-id="a76af-142">You can store values in a *variable*, including strings, numbers, and dates, etc. You create a new variable using the `Dim` keyword.</span></span> <span data-ttu-id="a76af-143">Puede insertar directamente en una página con los valores de variable `@`.</span><span class="sxs-lookup"><span data-stu-id="a76af-143">You can insert variable values directly in a page using `@`.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample4.vbhtml)]

<span data-ttu-id="a76af-144">El resultado se muestra en un explorador:</span><span class="sxs-lookup"><span data-stu-id="a76af-144">The result displayed in a browser:</span></span>

![Razor Img3](introducing-razor-syntax-vb/_static/image3.jpg)

### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a><span data-ttu-id="a76af-146">5. Incluya los valores de cadena literal entre comillas dobles</span><span class="sxs-lookup"><span data-stu-id="a76af-146">5. You enclose literal string values in double quotation marks</span></span>

<span data-ttu-id="a76af-147">Un *cadena* es una secuencia de caracteres que se tratan como texto.</span><span class="sxs-lookup"><span data-stu-id="a76af-147">A *string* is a sequence of characters that are treated as text.</span></span> <span data-ttu-id="a76af-148">Para especificar una cadena, debe encerrarlo entre comillas dobles:</span><span class="sxs-lookup"><span data-stu-id="a76af-148">To specify a string, you enclose it in double quotation marks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample5.vbhtml)]

<span data-ttu-id="a76af-149">Para insertar las comillas dobles dentro de un valor de cadena, inserte dos caracteres de comillas dobles.</span><span class="sxs-lookup"><span data-stu-id="a76af-149">To embed double quotation marks within a string value, insert two double quotation mark characters.</span></span> <span data-ttu-id="a76af-150">Si desea que el carácter de comillas dobles a aparecer una vez en el resultado de la página, escríbalo como `""` dentro el entrecomillado de cadena y si desea que aparezca dos veces, escríbala como `""""` dentro de la cadena entre comillas.</span><span class="sxs-lookup"><span data-stu-id="a76af-150">If you want the double quotation character to appear once in the page output, enter it as `""` within the quoted string, and if you want it to appear twice, enter it as `""""` within the quoted string.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample6.vbhtml)]

<span data-ttu-id="a76af-151">El resultado se muestra en un explorador:</span><span class="sxs-lookup"><span data-stu-id="a76af-151">The result displayed in a browser:</span></span>

![Razor Img4](introducing-razor-syntax-vb/_static/image4.jpg)

### <a name="6-visual-basic-code-is-not-case-sensitive"></a><span data-ttu-id="a76af-153">6. Código de Visual Basic no distingue mayúsculas de minúsculas</span><span class="sxs-lookup"><span data-stu-id="a76af-153">6. Visual Basic code is not case sensitive</span></span>

<span data-ttu-id="a76af-154">El lenguaje Visual Basic no distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="a76af-154">The Visual Basic language is not case sensitive.</span></span> <span data-ttu-id="a76af-155">Palabras clave de programación (como `Dim`, `If`, y `True`) y los nombres de variable (como `myString`, o `subTotal`) pueden escribirse en cualquier caso.</span><span class="sxs-lookup"><span data-stu-id="a76af-155">Programming keywords (like `Dim`, `If`, and `True`) and variable names (like `myString`, or `subTotal`) can be written in any case.</span></span>

<span data-ttu-id="a76af-156">Las siguientes líneas de código asignación un valor a la variable `lastname` con una minúscula, asigne un nombre y, a continuación, generar el valor de variable a la página mediante un nombre en mayúsculas.</span><span class="sxs-lookup"><span data-stu-id="a76af-156">The following lines of code assign a value to the variable `lastname` using a lowercase name, and then output the variable value to the page using an uppercase name.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample7.vbhtml)]

<span data-ttu-id="a76af-157">El resultado se muestra en un explorador:</span><span class="sxs-lookup"><span data-stu-id="a76af-157">The result displayed in a browser:</span></span>

![sintaxis-VB-5](introducing-razor-syntax-vb/_static/image5.jpg)

### <a name="7-much-of-your-coding-involves-working-with-objects"></a><span data-ttu-id="a76af-159">7. Gran parte de la codificación implica trabajar con objetos</span><span class="sxs-lookup"><span data-stu-id="a76af-159">7. Much of your coding involves working with objects</span></span>

<span data-ttu-id="a76af-160">Un objeto representa algo que se puede programar con &#8212; una página, un cuadro de texto, un archivo, una imagen, una solicitud web, un mensaje de correo electrónico, un registro de cliente (fila de la base de datos), etcetera. Objetos tienen propiedades que describen sus características &#8212; un objeto de cuadro de texto tiene un `Text` propiedad, un objeto de solicitud tiene un `Url` propiedad, un mensaje de correo electrónico tiene un `From` propiedad y un objeto customer tiene una `FirstName` propiedad.</span><span class="sxs-lookup"><span data-stu-id="a76af-160">An object represents a thing that you can program with &#8212; a page, a text box, a file, an image, a web request, an email message, a customer record (database row), etc. Objects have properties that describe their characteristics &#8212; a text box object has a `Text` property, a request object has a `Url` property, an email message has a `From` property, and a customer object has a `FirstName` property.</span></span> <span data-ttu-id="a76af-161">Los objetos también tienen métodos que son el &quot;verbos&quot; pueden realizar.</span><span class="sxs-lookup"><span data-stu-id="a76af-161">Objects also have methods that are the &quot;verbs&quot; they can perform.</span></span> <span data-ttu-id="a76af-162">Algunos ejemplos son un objeto de archivo `Save` método, un objeto de imagen `Rotate` método y un objeto de correo electrónico `Send` método.</span><span class="sxs-lookup"><span data-stu-id="a76af-162">Examples include a file object's `Save` method, an image object's `Rotate` method, and an email object's `Send` method.</span></span>

<span data-ttu-id="a76af-163">A menudo trabajará con la `Request` campos de objeto, lo que le ofrece información como los valores de formulario en la página (cuadros de texto, etc.), el tipo de explorador realizó la solicitud, la dirección URL de la página, la identidad del usuario, etcetera. En este ejemplo se muestra cómo obtener acceso a las propiedades de la `Request` objeto y cómo llamar a la `MapPath` método de la `Request` objeto, que proporciona la ruta de acceso absoluta de la página en el servidor:</span><span class="sxs-lookup"><span data-stu-id="a76af-163">You'll often work with the `Request` object, which gives you information like the values of form fields on the page (text boxes, etc.), what type of browser made the request, the URL of the page, the user identity, etc. This example shows how to access properties of the `Request` object and how to call the `MapPath` method of the `Request` object, which gives you the absolute path of the page on the server:</span></span>

[!code-html[Main](introducing-razor-syntax-vb/samples/sample8.html)]

<span data-ttu-id="a76af-164">El resultado se muestra en un explorador:</span><span class="sxs-lookup"><span data-stu-id="a76af-164">The result displayed in a browser:</span></span>

![Razor Img5](introducing-razor-syntax-vb/_static/image6.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a><span data-ttu-id="a76af-166">8. Puede escribir código que toma decisiones</span><span class="sxs-lookup"><span data-stu-id="a76af-166">8. You can write code that makes decisions</span></span>

<span data-ttu-id="a76af-167">Una característica clave de páginas web dinámicas es que puede determinar qué hacer en función de condiciones.</span><span class="sxs-lookup"><span data-stu-id="a76af-167">A key feature of dynamic web pages is that you can determine what to do based on conditions.</span></span> <span data-ttu-id="a76af-168">La manera más común de hacerlo es con la `If` instrucción (y opcionales `Else` instrucción).</span><span class="sxs-lookup"><span data-stu-id="a76af-168">The most common way to do this is with the `If` statement (and optional `Else` statement).</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample9.vbhtml)]

<span data-ttu-id="a76af-169">La instrucción `If IsPost` es una manera abreviada de la escritura `If IsPost = True`.</span><span class="sxs-lookup"><span data-stu-id="a76af-169">The statement `If IsPost` is a shorthand way of writing `If IsPost = True`.</span></span> <span data-ttu-id="a76af-170">Junto con `If` instrucciones, hay una gran variedad de formas para probar condiciones, repita los bloques de código, y así sucesivamente, que se describen más adelante en este artículo.</span><span class="sxs-lookup"><span data-stu-id="a76af-170">Along with `If` statements, there are a variety of ways to test conditions, repeat blocks of code, and so on, which are described later in this article.</span></span>

<span data-ttu-id="a76af-171">El resultado que se muestra en un explorador (después de hacer clic **enviar**):</span><span class="sxs-lookup"><span data-stu-id="a76af-171">The result displayed in a browser (after clicking **Submit**):</span></span>

![Razor Img6](introducing-razor-syntax-vb/_static/image7.jpg)

> [!TIP] 
> 
> <span data-ttu-id="a76af-173">**HTTP GET y POST métodos y la propiedad IsPost**</span><span class="sxs-lookup"><span data-stu-id="a76af-173">**HTTP GET and POST Methods and the IsPost Property**</span></span>
> 
> <span data-ttu-id="a76af-174">El protocolo utilizado para las páginas web (HTTP) admite un número muy limitado de métodos (&quot;verbos&quot;) que se usan para realizar solicitudes al servidor.</span><span class="sxs-lookup"><span data-stu-id="a76af-174">The protocol used for web pages (HTTP) supports a very limited number of methods (&quot;verbs&quot;) that are used to make requests to the server.</span></span> <span data-ttu-id="a76af-175">Dos las más comunes son GET, que se utiliza para leer una página, y POST, que se usa para enviar una página.</span><span class="sxs-lookup"><span data-stu-id="a76af-175">The two most common ones are GET, which is used to read a page, and POST, which is used to submit a page.</span></span> <span data-ttu-id="a76af-176">En general, la primera vez que un usuario solicita una página, se solicita la página con GET.</span><span class="sxs-lookup"><span data-stu-id="a76af-176">In general, the first time a user requests a page, the page is requested using GET.</span></span> <span data-ttu-id="a76af-177">Si el usuario escribe en un formulario y, a continuación, hace clic en **enviar**, el explorador realiza una solicitud POST al servidor.</span><span class="sxs-lookup"><span data-stu-id="a76af-177">If the user fills in a form and then clicks **Submit**, the browser makes a POST request to the server.</span></span>
> 
> <span data-ttu-id="a76af-178">En la programación web, a menudo resulta útil saber si una página se solicitan como una operación GET o como una entrada de blog para que sepa cómo procesar la página.</span><span class="sxs-lookup"><span data-stu-id="a76af-178">In web programming, it's often useful to know whether a page is being requested as a GET or as a POST so that you know how to process the page.</span></span> <span data-ttu-id="a76af-179">En ASP.NET Web Pages, puede usar el `IsPost` propiedad para ver si una solicitud es una operación GET o POST.</span><span class="sxs-lookup"><span data-stu-id="a76af-179">In ASP.NET Web Pages, you can use the `IsPost` property to see whether a request is a GET or a POST.</span></span> <span data-ttu-id="a76af-180">Si la solicitud es una publicación, el `IsPost` propiedad devolverá true, y puede hacer cosas como la lectura de los valores de los cuadros de texto en un formulario.</span><span class="sxs-lookup"><span data-stu-id="a76af-180">If the request is a POST, the `IsPost` property will return true, and you can do things like read the values of text boxes on a form.</span></span> <span data-ttu-id="a76af-181">Muchos ejemplos verá mostrarle cómo procesar la página de manera diferente dependiendo del valor de `IsPost`.</span><span class="sxs-lookup"><span data-stu-id="a76af-181">Many examples you'll see show you how to process the page differently depending on the value of `IsPost`.</span></span>


## <a name="a-simple-code-example"></a><span data-ttu-id="a76af-182">Un ejemplo de código Simple</span><span class="sxs-lookup"><span data-stu-id="a76af-182">A Simple Code Example</span></span>

<span data-ttu-id="a76af-183">Este procedimiento muestra cómo crear una página que muestra las técnicas de programación básicas.</span><span class="sxs-lookup"><span data-stu-id="a76af-183">This procedure shows you how to create a page that illustrates basic programming techniques.</span></span> <span data-ttu-id="a76af-184">En el ejemplo, cree una página que permite a los usuarios escribir dos números, a continuación, agrega y muestra el resultado.</span><span class="sxs-lookup"><span data-stu-id="a76af-184">In the example, you create a page that lets users enter two numbers, then it adds them and displays the result.</span></span>

1. <span data-ttu-id="a76af-185">En el editor, cree un nuevo archivo y asígnele el nombre *AddNumbers.vbhtml*.</span><span class="sxs-lookup"><span data-stu-id="a76af-185">In your editor, create a new file and name it *AddNumbers.vbhtml*.</span></span>
2. <span data-ttu-id="a76af-186">Copie el siguiente código y marcado en la página, reemplazando cualquier cosa ya está en la página.</span><span class="sxs-lookup"><span data-stu-id="a76af-186">Copy the following code and markup into the page, replacing anything already in the page.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample10.vbhtml)]

    <span data-ttu-id="a76af-187">Estos son algunos aspectos que tener en cuenta:</span><span class="sxs-lookup"><span data-stu-id="a76af-187">Here are some things for you to note:</span></span>

    - <span data-ttu-id="a76af-188">El `@` carácter inicia el primer bloque de código en la página y precede a la `totalMessage` variable incrustada en la parte inferior.</span><span class="sxs-lookup"><span data-stu-id="a76af-188">The `@` character starts the first block of code in the page, and it precedes the `totalMessage` variable embedded near the bottom.</span></span>
    - <span data-ttu-id="a76af-189">El bloque en la parte superior de la página se incluye en `Code...End Code`.</span><span class="sxs-lookup"><span data-stu-id="a76af-189">The block at the top of the page is enclosed in `Code...End Code`.</span></span>
    - <span data-ttu-id="a76af-190">Las variables `total`, `num1`, `num2`, y `totalMessage` almacenar varios números y una cadena.</span><span class="sxs-lookup"><span data-stu-id="a76af-190">The variables `total`, `num1`, `num2`, and `totalMessage` store several numbers and a string.</span></span>
    - <span data-ttu-id="a76af-191">El valor literal de cadena asignado a la `totalMessage` es variable entre comillas dobles.</span><span class="sxs-lookup"><span data-stu-id="a76af-191">The literal string value assigned to the `totalMessage` variable is in double quotation marks.</span></span>
    - <span data-ttu-id="a76af-192">Dado que el código de Visual Basic no distingue mayúsculas de minúsculas, cuando el `totalMessage` variable se usa la parte inferior de la página, su nombre sólo debe coincidir con la ortografía de la declaración de variable en la parte superior de la página.</span><span class="sxs-lookup"><span data-stu-id="a76af-192">Because Visual Basic code is not case sensitive, when the `totalMessage` variable is used near the bottom of the page, its name only needs to match the spelling of the variable declaration at the top of the page.</span></span> <span data-ttu-id="a76af-193">No importan las mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="a76af-193">The casing doesn't matter.</span></span>
    - <span data-ttu-id="a76af-194">La expresión `num1.AsInt()`  +  `num2.AsInt()` muestra cómo trabajar con objetos y métodos.</span><span class="sxs-lookup"><span data-stu-id="a76af-194">The expression `num1.AsInt()` + `num2.AsInt()` shows how to work with objects and methods.</span></span> <span data-ttu-id="a76af-195">El `AsInt` método en cada variable convierte la cadena especificada por el usuario a un número entero (entero) que se pueden agregar.</span><span class="sxs-lookup"><span data-stu-id="a76af-195">The `AsInt` method on each variable converts the string entered by a user to a whole number (an integer) that can be added.</span></span>
    - <span data-ttu-id="a76af-196">El `<form>` etiqueta incluye un `method="post"` atributo.</span><span class="sxs-lookup"><span data-stu-id="a76af-196">The `<form>` tag includes a `method="post"` attribute.</span></span> <span data-ttu-id="a76af-197">Especifica que, cuando el usuario hace clic en **agregar**, la página se enviará al servidor mediante el método HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="a76af-197">This specifies that when the user clicks **Add**, the page will be sent to the server using the HTTP POST method.</span></span> <span data-ttu-id="a76af-198">Cuando se envía la página, el código `If IsPost` se evalúa como true y operador condicional de código se ejecuta, muestra el resultado de sumar los números.</span><span class="sxs-lookup"><span data-stu-id="a76af-198">When the page is submitted, the code `If IsPost` evaluates to true and the conditional code runs, displaying the result of adding the numbers.</span></span>
3. <span data-ttu-id="a76af-199">Guarde la página y ejecútelo en un explorador.</span><span class="sxs-lookup"><span data-stu-id="a76af-199">Save the page and run it in a browser.</span></span> <span data-ttu-id="a76af-200">(Asegúrese de que la página está seleccionada en el **archivos** área de trabajo antes de ejecutarlo.) Escriba los dos números enteros y, a continuación, haga clic en el **agregar** botón.</span><span class="sxs-lookup"><span data-stu-id="a76af-200">(Make sure the page is selected in the **Files** workspace before you run it.) Enter two whole numbers and then click the **Add** button.</span></span>

    ![Razor Img7](introducing-razor-syntax-vb/_static/image8.jpg)

## <a name="visual-basic-language-and-syntax"></a><span data-ttu-id="a76af-202">Sintaxis y el lenguaje Visual Basic</span><span class="sxs-lookup"><span data-stu-id="a76af-202">Visual Basic Language and Syntax</span></span>

<span data-ttu-id="a76af-203">Ya vimos un ejemplo básico de cómo crear una página web ASP.NET y cómo puede agregar código de servidor en formato HTML.</span><span class="sxs-lookup"><span data-stu-id="a76af-203">Earlier you saw a basic example of how to create an ASP.NET web page, and how you can add server code to HTML markup.</span></span> <span data-ttu-id="a76af-204">Aquí aprenderá los conceptos básicos del uso de Visual Basic para escribir código de servidor ASP.NET mediante la sintaxis Razor &#8212; es decir, las reglas de lenguaje de programación.</span><span class="sxs-lookup"><span data-stu-id="a76af-204">Here you'll learn the basics of using Visual Basic to write ASP.NET server code using the Razor syntax &#8212; that is, the programming language rules.</span></span>

<span data-ttu-id="a76af-205">Si tiene experiencia con la programación (especialmente si ha usado C, C++, C#, Visual Basic o JavaScript), gran parte de lo que lee aquí le resultarán familiar.</span><span class="sxs-lookup"><span data-stu-id="a76af-205">If you're experienced with programming (especially if you've used C, C++, C#, Visual Basic, or JavaScript), much of what you read here will be familiar.</span></span> <span data-ttu-id="a76af-206">Probablemente deberá familiarizarse con sólo de cómo se agrega código de WebMatrix para marcado en *.vbhtml* archivos.</span><span class="sxs-lookup"><span data-stu-id="a76af-206">You'll probably need to familiarize yourself only with how WebMatrix code is added to markup in *.vbhtml* files.</span></span>

### <a id="BM_CombiningTextMarkupAndCode"></a>  <span data-ttu-id="a76af-207">Combinación de texto, marcado y código en bloques de código</span><span class="sxs-lookup"><span data-stu-id="a76af-207">Combining text, markup, and code in code blocks</span></span>

<span data-ttu-id="a76af-208">En los bloques de código de servidor, a menudo conveniente generar texto y marcado en la página.</span><span class="sxs-lookup"><span data-stu-id="a76af-208">In server code blocks, you'll often want to output text and markup to the page.</span></span> <span data-ttu-id="a76af-209">Si un bloque de código de servidor contiene texto que no es código y que en su lugar se debe representar como está, debe ser capaz de distinguir ese texto desde el código ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a76af-209">If a server code block contains text that's not code and that instead should be rendered as is, ASP.NET needs to be able to distinguish that text from code.</span></span> <span data-ttu-id="a76af-210">Existen varias formas de hacerlo.</span><span class="sxs-lookup"><span data-stu-id="a76af-210">There are several ways to do this.</span></span>

- <span data-ttu-id="a76af-211">Incluir el texto en un elemento de bloque HTML como `<p></p>` o `<em></em>`:</span><span class="sxs-lookup"><span data-stu-id="a76af-211">Enclose the text in an HTML block element like `<p></p>` or `<em></em>`:</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample11.vbhtml)]

    <span data-ttu-id="a76af-212">El elemento HTML puede incluir texto, los elementos adicionales de HTML y expresiones de código del servidor.</span><span class="sxs-lookup"><span data-stu-id="a76af-212">The HTML element can include text, additional HTML elements, and server-code expressions.</span></span> <span data-ttu-id="a76af-213">Cuando ASP.NET ve la etiqueta HTML de apertura (por ejemplo, `<p>`), todo lo que representa el elemento y su contenido como es el explorador (y resuelve las expresiones de código del servidor).</span><span class="sxs-lookup"><span data-stu-id="a76af-213">When ASP.NET sees the opening HTML tag (for example, `<p>`), it renders everything the element and its content as is to the browser (and resolves the server-code expressions).</span></span>

- <span data-ttu-id="a76af-214">Use la `@:` operador o la `<text>` elemento.</span><span class="sxs-lookup"><span data-stu-id="a76af-214">Use the `@:` operator or the `<text>` element.</span></span> <span data-ttu-id="a76af-215">El `@:` da como resultado una sola línea de contenido que contiene texto sin formato o etiquetas HTML no coincidentes; el `<text>` elemento abarca varias líneas de salida.</span><span class="sxs-lookup"><span data-stu-id="a76af-215">The `@:` outputs a single line of content containing plain text or unmatched HTML tags; the `<text>` element encloses multiple lines to output.</span></span> <span data-ttu-id="a76af-216">Estas opciones son útiles cuando no quiere representar un elemento HTML como parte de la salida.</span><span class="sxs-lookup"><span data-stu-id="a76af-216">These options are useful when you don't want to render an HTML element as part of the output.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample12.vbhtml)]

    <span data-ttu-id="a76af-217">El ejemplo siguiente se repite el ejemplo anterior, pero usa un único par de `<text>` etiquetas para delimitar el texto para representar.</span><span class="sxs-lookup"><span data-stu-id="a76af-217">The following example repeats the previous example but uses a single pair of `<text>` tags to enclose the text to render.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample13.vbhtml)]

    <span data-ttu-id="a76af-218">En el ejemplo siguiente, la `<text>` y `</text>` etiquetas incluir tres líneas, todos ellos tienen algunas no contenido texto y etiquetas HTML no coincidentes (`<br />`), junto con el código de servidor y las etiquetas HTML coincidentes.</span><span class="sxs-lookup"><span data-stu-id="a76af-218">In the following example, the `<text>` and `</text>` tags enclose three lines, all of which have some uncontained text and unmatched HTML tags (`<br />`), along with server code and matched HTML tags.</span></span> <span data-ttu-id="a76af-219">De nuevo, también se podría preceder cada línea individualmente con el `@:` operador; cualquiera de ellas funciona de forma.</span><span class="sxs-lookup"><span data-stu-id="a76af-219">Again, you could also precede each line individually with the `@:` operator; either way works.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample14.vbhtml)]

    > [!NOTE]
    > <span data-ttu-id="a76af-220">Al generar texto, como se muestra en esta sección &#8212; con el elemento HTML, el `@:` (operador), o la `<text>` elemento &#8212; ASP.NET no codifica como HTML la salida.</span><span class="sxs-lookup"><span data-stu-id="a76af-220">When you output text as shown in this section &#8212; using an HTML element, the `@:` operator, or the `<text>` element &#8212; ASP.NET doesn't HTML-encode the output.</span></span> <span data-ttu-id="a76af-221">(Como se indicó anteriormente, ASP.NET codificar la salida de las expresiones de código de servidor y los bloques de código de servidor que van precedidos por `@`, excepto en los casos especiales que se indican en esta sección.)</span><span class="sxs-lookup"><span data-stu-id="a76af-221">(As noted earlier, ASP.NET does encode the output of server code expressions and server code blocks that are preceded by `@`, except in the special cases noted in this section.)</span></span>

### <a name="whitespace"></a><span data-ttu-id="a76af-222">Whitespace</span><span class="sxs-lookup"><span data-stu-id="a76af-222">Whitespace</span></span>

<span data-ttu-id="a76af-223">Los espacios adicionales en una instrucción (y fuera de un literal de cadena) no afectan a la instrucción:</span><span class="sxs-lookup"><span data-stu-id="a76af-223">Extra spaces in a statement (and outside of a string literal) don't affect the statement:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample15.vbhtml)]

### <a name="breaking-long-statements-into-multiple-lines"></a><span data-ttu-id="a76af-224">Dividir instrucciones largas en varias líneas</span><span class="sxs-lookup"><span data-stu-id="a76af-224">Breaking long statements into multiple lines</span></span>

<span data-ttu-id="a76af-225">Puede dividir una instrucción de código larga en varias líneas mediante el uso del carácter de subrayado `_` (que en Visual Basic se denomina el *carácter de continuación*) después de cada línea de código.</span><span class="sxs-lookup"><span data-stu-id="a76af-225">You can break a long code statement into multiple lines by using the underscore character `_` (which in Visual Basic is called the *continuation character*) after each line of code.</span></span> <span data-ttu-id="a76af-226">Para interrumpir una instrucción en la siguiente línea, al final de la línea, agregue un espacio y, a continuación, el carácter de continuación.</span><span class="sxs-lookup"><span data-stu-id="a76af-226">To break a statement onto the next line, at the end of the line add a space and then the continuation character.</span></span> <span data-ttu-id="a76af-227">Continuar con la instrucción en la línea siguiente.</span><span class="sxs-lookup"><span data-stu-id="a76af-227">Continue the statement on the next line.</span></span> <span data-ttu-id="a76af-228">Puede ajustar las instrucciones en líneas tantos como necesite para mejorar la legibilidad.</span><span class="sxs-lookup"><span data-stu-id="a76af-228">You can wrap statements onto as many lines as you need to improve readability.</span></span> <span data-ttu-id="a76af-229">Las instrucciones siguientes son los mismos:</span><span class="sxs-lookup"><span data-stu-id="a76af-229">The following statements are the same:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample16.vbhtml)]

<span data-ttu-id="a76af-230">Sin embargo, no se puede ajustar una línea en el medio de un literal de cadena.</span><span class="sxs-lookup"><span data-stu-id="a76af-230">However, you can't wrap a line in the middle of a string literal.</span></span> <span data-ttu-id="a76af-231">No funciona en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="a76af-231">The following example doesn't work:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample17.vbhtml)]

<span data-ttu-id="a76af-232">Para combinar una cadena larga que se ajusta a varias líneas, como el código anterior, deberá usar el *operador de concatenación* (`&`), que verá más adelante en este artículo.</span><span class="sxs-lookup"><span data-stu-id="a76af-232">To combine a long string that wraps to multiple lines like the above code, you would need to use the *concatenation operator* (`&`), which you'll see later in this article.</span></span>

### <a name="code-comments"></a><span data-ttu-id="a76af-233">Comentarios del código</span><span class="sxs-lookup"><span data-stu-id="a76af-233">Code comments</span></span>

<span data-ttu-id="a76af-234">Comentarios le permite dejar notas para uso personal o a otros usuarios.</span><span class="sxs-lookup"><span data-stu-id="a76af-234">Comments let you leave notes for yourself or others.</span></span> <span data-ttu-id="a76af-235">Van precedidos de comentarios de la sintaxis de Razor `@*` y terminar por `*@`.</span><span class="sxs-lookup"><span data-stu-id="a76af-235">Razor syntax comments are prefixed with `@*` and end with `*@`.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-vb/samples/sample18.cshtml)]

<span data-ttu-id="a76af-236">Dentro de bloques de código puede usar los comentarios de la sintaxis de Razor, o puede usar normal carácter de comentario de Visual Basic, que es una comilla simple (`'`) como prefijo a cada línea.</span><span class="sxs-lookup"><span data-stu-id="a76af-236">Within code blocks you can use the Razor syntax comments, or you can use ordinary Visual Basic comment character, which is a single quote (`'`) prefixed to each line.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample19.vbhtml)]

## <a name="variables"></a><span data-ttu-id="a76af-237">Variables</span><span class="sxs-lookup"><span data-stu-id="a76af-237">Variables</span></span>

<span data-ttu-id="a76af-238">Una variable es un objeto con nombre que usa para almacenar los datos.</span><span class="sxs-lookup"><span data-stu-id="a76af-238">A variable is a named object that you use to store data.</span></span> <span data-ttu-id="a76af-239">Puede asignar variables, pero el nombre debe comenzar con un carácter alfabético y no puede contener espacios en blanco o caracteres reservados.</span><span class="sxs-lookup"><span data-stu-id="a76af-239">You can name variables anything, but the name must begin with an alphabetic character and it cannot contain whitespace or reserved characters.</span></span> <span data-ttu-id="a76af-240">En Visual Basic, como se vio anteriormente, no importa el caso de las letras de un nombre de variable.</span><span class="sxs-lookup"><span data-stu-id="a76af-240">In Visual Basic, as you saw earlier, the case of the letters in a variable name doesn't matter.</span></span>

### <a name="variables-and-data-types"></a><span data-ttu-id="a76af-241">Las variables y tipos de datos</span><span class="sxs-lookup"><span data-stu-id="a76af-241">Variables and data types</span></span>

<span data-ttu-id="a76af-242">Una variable puede tener un tipo de datos específico, lo que indica qué tipo de datos se almacena en la variable.</span><span class="sxs-lookup"><span data-stu-id="a76af-242">A variable can have a specific data type, which indicates what kind of data is stored in the variable.</span></span> <span data-ttu-id="a76af-243">Puede tener variables de cadena que almacenan valores de cadena (como &quot;Hola mundo&quot;), las variables de entero que almacenan valores de número entero (por ejemplo, 3 o 79) y las variables de fecha que almacenan los valores de fecha en una variedad de formatos (por ejemplo, 12/4/2012 o de marzo de 2009 ).</span><span class="sxs-lookup"><span data-stu-id="a76af-243">You can have string variables that store string values (like &quot;Hello world&quot;), integer variables that store whole-number values (like 3 or 79), and date variables that store date values in a variety of formats (like 4/12/2012 or March 2009).</span></span> <span data-ttu-id="a76af-244">Y hay muchos otros tipos de datos que se puede usar.</span><span class="sxs-lookup"><span data-stu-id="a76af-244">And there are many other data types you can use.</span></span>

<span data-ttu-id="a76af-245">Sin embargo, no debe especificar un tipo para una variable.</span><span class="sxs-lookup"><span data-stu-id="a76af-245">However, you don't have to specify a type for a variable.</span></span> <span data-ttu-id="a76af-246">En la mayoría de los casos, ASP.NET puede deducir el tipo según cómo se utilizan los datos en la variable.</span><span class="sxs-lookup"><span data-stu-id="a76af-246">In most cases ASP.NET can figure out the type based on how the data in the variable is being used.</span></span> <span data-ttu-id="a76af-247">(En ocasiones, debe especificar un tipo; verá ejemplos donde esto es cierto).</span><span class="sxs-lookup"><span data-stu-id="a76af-247">(Occasionally you must specify a type; you'll see examples where this is true.)</span></span>

<span data-ttu-id="a76af-248">Para declarar una variable sin especificar un tipo, use `Dim` más el nombre de variable (por ejemplo, `Dim myVar`).</span><span class="sxs-lookup"><span data-stu-id="a76af-248">To declare a variable without specifying a type, use `Dim` plus the variable name (for instance, `Dim myVar`).</span></span> <span data-ttu-id="a76af-249">Para declarar una variable con un tipo, use `Dim` más el nombre de variable, seguido de `As` y, a continuación, el nombre de tipo (por ejemplo, `Dim myVar As String`).</span><span class="sxs-lookup"><span data-stu-id="a76af-249">To declare a variable with a type, use `Dim` plus the variable name, followed by `As` and then the type name (for instance, `Dim myVar As String`).</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample20.vbhtml)]

<span data-ttu-id="a76af-250">El ejemplo siguiente muestra algunas expresiones en línea que utilizan las variables en una página web.</span><span class="sxs-lookup"><span data-stu-id="a76af-250">The following example shows some inline expressions that use the variables in a web page.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample21.vbhtml)]

<span data-ttu-id="a76af-251">El resultado se muestra en un explorador:</span><span class="sxs-lookup"><span data-stu-id="a76af-251">The result displayed in a browser:</span></span>

![Razor Img9](introducing-razor-syntax-vb/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a><span data-ttu-id="a76af-253">Convertir y pruebas de tipos de datos</span><span class="sxs-lookup"><span data-stu-id="a76af-253">Converting and testing data types</span></span>

<span data-ttu-id="a76af-254">Aunque ASP.NET normalmente pueden determinar automáticamente un tipo de datos, a veces, no puede.</span><span class="sxs-lookup"><span data-stu-id="a76af-254">Although ASP.NET can usually determine a data type automatically, sometimes it can't.</span></span> <span data-ttu-id="a76af-255">Por lo tanto, es posible que deba ayudarnos ASP.NET mediante la realización de una conversión explícita.</span><span class="sxs-lookup"><span data-stu-id="a76af-255">Therefore, you might need to help ASP.NET out by performing an explicit conversion.</span></span> <span data-ttu-id="a76af-256">Incluso si no tiene que convertir los tipos, a veces resulta útil probar para ver qué tipo de datos podría estar trabajando con.</span><span class="sxs-lookup"><span data-stu-id="a76af-256">Even if you don't have to convert types, sometimes it's helpful to test to see what type of data you might be working with.</span></span>

<span data-ttu-id="a76af-257">El caso más común es que se debe convertir una cadena a otro tipo, como en un entero o una fecha.</span><span class="sxs-lookup"><span data-stu-id="a76af-257">The most common case is that you have to convert a string to another type, such as to an integer or date.</span></span> <span data-ttu-id="a76af-258">El ejemplo siguiente muestra un caso típico que debe convertir una cadena en un número.</span><span class="sxs-lookup"><span data-stu-id="a76af-258">The following example shows a typical case where you must convert a string to a number.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample22.vbhtml)]

<span data-ttu-id="a76af-259">Como norma, proporcionados por el usuario aparecerán como cadenas.</span><span class="sxs-lookup"><span data-stu-id="a76af-259">As a rule, user input comes to you as strings.</span></span> <span data-ttu-id="a76af-260">Incluso si ha le pide al usuario que escriba un número e incluso si ha escrito un dígito, cuando se envía la entrada del usuario y leerlo en el código, los datos están en formato de cadena.</span><span class="sxs-lookup"><span data-stu-id="a76af-260">Even if you've prompted the user to enter a number, and even if they've entered a digit, when user input is submitted and you read it in code, the data is in string format.</span></span> <span data-ttu-id="a76af-261">Por lo tanto, debe convertir la cadena en un número.</span><span class="sxs-lookup"><span data-stu-id="a76af-261">Therefore, you must convert the string to a number.</span></span> <span data-ttu-id="a76af-262">En el ejemplo, si se intenta realizar operaciones aritméticas en los valores sin necesidad de convertirlos, produce el siguiente error, porque ASP.NET no se puede agregar dos cadenas:</span><span class="sxs-lookup"><span data-stu-id="a76af-262">In the example, if you try to perform arithmetic on the values without converting them, the following error results, because ASP.NET cannot add two strings:</span></span>

`Cannot implicitly convert type 'string' to 'int'.`

<span data-ttu-id="a76af-263">Para convertir los valores enteros, se llama a la `AsInt` método.</span><span class="sxs-lookup"><span data-stu-id="a76af-263">To convert the values to integers, you call the `AsInt` method.</span></span> <span data-ttu-id="a76af-264">Si la conversión se realiza correctamente, a continuación, puede agregar los números.</span><span class="sxs-lookup"><span data-stu-id="a76af-264">If the conversion is successful, you can then add the numbers.</span></span>

<span data-ttu-id="a76af-265">En la tabla siguiente se enumera algunos métodos de conversión y prueba comunes para las variables.</span><span class="sxs-lookup"><span data-stu-id="a76af-265">The following table lists some common conversion and test methods for variables.</span></span>


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
        Converts a string that represents a whole number (like &quot;593&quot;) to an integer.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample23.vb)]
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
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample24.vb)]
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
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample25.vb)]
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
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample26.vb)]
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
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample27.vb)]
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
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample28.vb)]
    :::column-end:::
:::row-end:::


## <a name="operators"></a><span data-ttu-id="a76af-266">Operadores</span><span class="sxs-lookup"><span data-stu-id="a76af-266">Operators</span></span>

<span data-ttu-id="a76af-267">Un operador es una palabra clave o el carácter que le indica a ASP.NET qué tipo de comando que se ejecuta en una expresión.</span><span class="sxs-lookup"><span data-stu-id="a76af-267">An operator is a keyword or character that tells ASP.NET what kind of command to perform in an expression.</span></span> <span data-ttu-id="a76af-268">Visual Basic admite muchos operadores, pero deberá reconocer algunas para comenzar a desarrollar páginas web de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a76af-268">Visual Basic supports many operators, but you only need to recognize a few to get started developing ASP.NET web pages.</span></span> <span data-ttu-id="a76af-269">En la tabla siguiente se resume los operadores más comunes.</span><span class="sxs-lookup"><span data-stu-id="a76af-269">The following table summarizes the most common operators.</span></span>


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
        `+ - * /`
    :::column-end:::
    :::column:::
        Math operators used in numerical expressions.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample29.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `=`
    :::column-end:::
    :::column:::
        Assignment and equality. Depending on context, either assigns the value on the right side of a statement to the object on the left side, or checks the values for equality.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample30.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `<>`
    :::column-end:::
    :::column:::
        Inequality. Returns `True` if the values are not equal.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample31.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `< > <= >=`
    :::column-end:::
    :::column:::
        Less than, greater than, less than or equal, and greater than or equal.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample32.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `&`
    :::column-end:::
    :::column:::
        Concatenation, which is used to join strings.
    :::column-end:::
    :::column:::
        [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample33.vbhtml)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+= -=`
    :::column-end:::
    :::column:::
        The increment and decrement operators, which add and subtract 1 (respectively) from a variable.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample34.vb)]
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
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample35.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `()`
    :::column-end:::
    :::column:::
        Parentheses. Used to group expressions, to pass parameters to methods, and to access members of arrays and collections.
    :::column-end:::
    :::column:::
        [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample36.vbhtml)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `Not`
    :::column-end:::
    :::column:::
        Not. Reverses a true value to false and vice versa. Typically used as a shorthand way to test for `False` (that is, for not `True`).
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample37.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AndAlso OrElse`
    :::column-end:::
    :::column:::
        Logical AND and OR, which are used to link conditions together.
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample38.vb)]
    :::column-end:::
:::row-end:::

## <a name="working-with-file-and-folder-paths-in-code"></a><span data-ttu-id="a76af-270">Trabajar con archivos y rutas de acceso de carpeta en el código</span><span class="sxs-lookup"><span data-stu-id="a76af-270">Working with File and Folder Paths in Code</span></span>

<span data-ttu-id="a76af-271">A menudo a trabajar con rutas de acceso de archivos y carpetas en el código.</span><span class="sxs-lookup"><span data-stu-id="a76af-271">You'll often work with file and folder paths in your code.</span></span> <span data-ttu-id="a76af-272">Este es un ejemplo de la estructura de la carpeta física de un sitio Web como podría aparecer en el equipo de desarrollo:</span><span class="sxs-lookup"><span data-stu-id="a76af-272">Here is an example of physical folder structure for a website as it might appear on your development computer:</span></span>

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

<span data-ttu-id="a76af-273">Estos son algunos detalles esenciales sobre las direcciones URL y rutas de acceso:</span><span class="sxs-lookup"><span data-stu-id="a76af-273">Here are some essential details about URLs and paths:</span></span>

- <span data-ttu-id="a76af-274">Una dirección URL comienza con cualquiera de un nombre de dominio (`http://www.example.com`) o un nombre de servidor (`http://localhost`, `http://mycomputer`).</span><span class="sxs-lookup"><span data-stu-id="a76af-274">A URL begins with either a domain name (`http://www.example.com`) or a server name (`http://localhost`, `http://mycomputer`).</span></span>
- <span data-ttu-id="a76af-275">Una dirección URL que se corresponde con una ruta de acceso física en un equipo host.</span><span class="sxs-lookup"><span data-stu-id="a76af-275">A URL corresponds to a physical path on a host computer.</span></span> <span data-ttu-id="a76af-276">Por ejemplo, `http://myserver` pueden corresponder a la carpeta *C:\websites\mywebsite* en el servidor.</span><span class="sxs-lookup"><span data-stu-id="a76af-276">For example, `http://myserver` might correspond to the folder *C:\websites\mywebsite* on the server.</span></span>
- <span data-ttu-id="a76af-277">Una ruta de acceso virtual es una abreviatura para representar las rutas de acceso en el código sin tener que especificar la ruta de acceso completa.</span><span class="sxs-lookup"><span data-stu-id="a76af-277">A virtual path is shorthand to represent paths in code without having to specify the full path.</span></span> <span data-ttu-id="a76af-278">Incluye la parte de una dirección URL que sigue al nombre de dominio o servidor.</span><span class="sxs-lookup"><span data-stu-id="a76af-278">It includes the portion of a URL that follows the domain or server name.</span></span> <span data-ttu-id="a76af-279">Al usar rutas de acceso virtuales, puede mover el código a un dominio diferente o un servidor sin tener que actualizar las rutas de acceso.</span><span class="sxs-lookup"><span data-stu-id="a76af-279">When you use virtual paths, you can move your code to a different domain or server without having to update the paths.</span></span>

<span data-ttu-id="a76af-280">Este es un ejemplo que le ayudarán a comprender las diferencias:</span><span class="sxs-lookup"><span data-stu-id="a76af-280">Here's an example to help you understand the differences:</span></span>

| <span data-ttu-id="a76af-281">Dirección URL completa</span><span class="sxs-lookup"><span data-stu-id="a76af-281">Complete URL</span></span> | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| <span data-ttu-id="a76af-282">Nombre del servidor</span><span class="sxs-lookup"><span data-stu-id="a76af-282">Server name</span></span> | <span data-ttu-id="a76af-283">*mycompanyserver*</span><span class="sxs-lookup"><span data-stu-id="a76af-283">*mycompanyserver*</span></span> |
| <span data-ttu-id="a76af-284">Ruta de acceso virtual</span><span class="sxs-lookup"><span data-stu-id="a76af-284">Virtual path</span></span> | <span data-ttu-id="a76af-285">*/HumanResources/CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="a76af-285">*/humanresources/CompanyPolicy.htm*</span></span> |
| <span data-ttu-id="a76af-286">Ruta de acceso física</span><span class="sxs-lookup"><span data-stu-id="a76af-286">Physical path</span></span> | <span data-ttu-id="a76af-287">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="a76af-287">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span></span> |

<span data-ttu-id="a76af-288">Es la raíz virtual /, al igual que la raíz de la unidad C: es la unidad \.</span><span class="sxs-lookup"><span data-stu-id="a76af-288">The virtual root is /, just like the root of your C: drive is \.</span></span> <span data-ttu-id="a76af-289">(Las rutas de acceso de la carpeta virtual siempre usar barras diagonales). La ruta de acceso virtual de una carpeta no debe tener el mismo nombre que la carpeta física; puede ser un alias.</span><span class="sxs-lookup"><span data-stu-id="a76af-289">(Virtual folder paths always use forward slashes.) The virtual path of a folder doesn't have to have the same name as the physical folder; it can be an alias.</span></span> <span data-ttu-id="a76af-290">(En los servidores de producción, la ruta de acceso virtual rara vez coincide con una ruta de acceso física exacta.)</span><span class="sxs-lookup"><span data-stu-id="a76af-290">(On production servers, the virtual path rarely matches an exact physical path.)</span></span>

<span data-ttu-id="a76af-291">A veces, cuando se trabaja con archivos y carpetas en el código, debe hacer referencia a la ruta de acceso física y a veces, una ruta de acceso virtual, dependiendo de los objetos que se trabaja con.</span><span class="sxs-lookup"><span data-stu-id="a76af-291">When you work with files and folders in code, sometimes you need to reference the physical path and sometimes a virtual path, depending on what objects you're working with.</span></span> <span data-ttu-id="a76af-292">ASP.NET proporciona estas herramientas para trabajar con rutas de acceso de archivos y carpetas en el código: el `Server.MapPath` método y el `~` operador y `Href` método.</span><span class="sxs-lookup"><span data-stu-id="a76af-292">ASP.NET gives you these tools for working with file and folder paths in code: the `Server.MapPath` method, and the `~` operator and `Href` method.</span></span>

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a><span data-ttu-id="a76af-293">Convertir rutas de acceso virtuales a físico: el método Server.MapPath</span><span class="sxs-lookup"><span data-stu-id="a76af-293">Converting virtual to physical paths: the Server.MapPath method</span></span>

<span data-ttu-id="a76af-294">El `Server.MapPath` método convierte una ruta de acceso virtual (como */default.cshtml*) a una ruta de acceso física absoluta (como *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="a76af-294">The `Server.MapPath` method converts a virtual path (like */default.cshtml*) to an absolute physical path (like *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span></span> <span data-ttu-id="a76af-295">Utilice este método cada vez que necesite una ruta de acceso física completa.</span><span class="sxs-lookup"><span data-stu-id="a76af-295">You use this method any time you need a complete physical path.</span></span> <span data-ttu-id="a76af-296">Un ejemplo típico es cuando se va a leer o escribir un archivo de texto o imagen en el servidor web.</span><span class="sxs-lookup"><span data-stu-id="a76af-296">A typical example is when you're reading or writing a text file or image file on the web server.</span></span>

<span data-ttu-id="a76af-297">Normalmente no conoce la ruta de acceso física absoluta de su sitio en el servidor de hospedaje de un sitio, por lo que este método puede convertir la ruta de acceso es saber, la ruta de acceso virtual, la ruta de acceso correspondiente en el servidor para usted.</span><span class="sxs-lookup"><span data-stu-id="a76af-297">You typically don't know the absolute physical path of your site on a hosting site's server, so this method can convert the path you do know — the virtual path — to the corresponding path on the server for you.</span></span> <span data-ttu-id="a76af-298">Pase la ruta de acceso virtual a un archivo o carpeta al método y devuelve la ruta de acceso física:</span><span class="sxs-lookup"><span data-stu-id="a76af-298">You pass the virtual path to a file or folder to the method, and it returns the physical path:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample39.vbhtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a><span data-ttu-id="a76af-299">Hacer referencia a la raíz virtual: el ~ operador y Href (método)</span><span class="sxs-lookup"><span data-stu-id="a76af-299">Referencing the virtual root: the ~ operator and Href method</span></span>

<span data-ttu-id="a76af-300">En un *.cshtml* o *.vbhtml* archivo, puede hacer referencia a la ruta de acceso virtual raíz mediante el `~` operador.</span><span class="sxs-lookup"><span data-stu-id="a76af-300">In a *.cshtml* or *.vbhtml* file, you can reference the virtual root path using the `~` operator.</span></span> <span data-ttu-id="a76af-301">Esto es muy útil porque puede mover las páginas en un sitio y los vínculos que contienen a otras páginas no se interrumpe.</span><span class="sxs-lookup"><span data-stu-id="a76af-301">This is very handy because you can move pages around in a site, and any links they contain to other pages won't be broken.</span></span> <span data-ttu-id="a76af-302">También es útil en caso de alguna vez mover su sitio Web a una ubicación diferente.</span><span class="sxs-lookup"><span data-stu-id="a76af-302">It's also handy in case you ever move your website to a different location.</span></span> <span data-ttu-id="a76af-303">A continuación se muestran algunos ejemplos:</span><span class="sxs-lookup"><span data-stu-id="a76af-303">Here are some examples:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample40.vbhtml)]

<span data-ttu-id="a76af-304">Si el sitio Web está `http://myserver/myapp`, le mostramos cómo ASP.NET tratan estas rutas de acceso cuando se ejecuta la página:</span><span class="sxs-lookup"><span data-stu-id="a76af-304">If the website is `http://myserver/myapp`, here's how ASP.NET will treat these paths when the page runs:</span></span>

- <span data-ttu-id="a76af-305">`myImagesFolder`: `http://myserver/myapp/images`</span><span class="sxs-lookup"><span data-stu-id="a76af-305">`myImagesFolder`: `http://myserver/myapp/images`</span></span>
- <span data-ttu-id="a76af-306">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span><span class="sxs-lookup"><span data-stu-id="a76af-306">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span></span>

<span data-ttu-id="a76af-307">(En realidad, no verá estas rutas de acceso que los valores de la variable, pero ASP.NET tratará las rutas de acceso como si eso es lo que fueran).</span><span class="sxs-lookup"><span data-stu-id="a76af-307">(You won't actually see these paths as the values of the variable, but ASP.NET will treat the paths as if that's what they were.)</span></span>

<span data-ttu-id="a76af-308">Puede usar el `~` operador tanto en código del servidor (como anteriormente) en el marcado, similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="a76af-308">You can use the `~` operator both in server code (as above) and in markup, like this:</span></span>

[!code-html[Main](introducing-razor-syntax-vb/samples/sample41.html)]

<span data-ttu-id="a76af-309">En el marcado, usa el `~` operador para crear rutas de acceso a recursos como archivos de imagen, otras páginas web y archivos CSS.</span><span class="sxs-lookup"><span data-stu-id="a76af-309">In markup, you use the `~` operator to create paths to resources like image files, other web pages, and CSS files.</span></span> <span data-ttu-id="a76af-310">Cuando se ejecuta la página, ASP.NET busca en la página (marcado y código) y resuelve todos los `~` referencias a la ruta de acceso adecuada.</span><span class="sxs-lookup"><span data-stu-id="a76af-310">When the page runs, ASP.NET looks through the page (both code and markup) and resolves all the `~` references to the appropriate path.</span></span>

## <a name="conditional-logic-and-loops"></a><span data-ttu-id="a76af-311">Bucles y la lógica condicional</span><span class="sxs-lookup"><span data-stu-id="a76af-311">Conditional Logic and Loops</span></span>

<span data-ttu-id="a76af-312">Código de servidor ASP.NET le permite realizar tareas basadas en condiciones y escribir código que se repite las instrucciones de código, es decir, un número específico de veces y que se ejecuta un bucle).</span><span class="sxs-lookup"><span data-stu-id="a76af-312">ASP.NET server code lets you perform tasks based on conditions and write code that repeats statements a specific number of times that is, code that runs a loop).</span></span>

### <a name="testing-conditions"></a><span data-ttu-id="a76af-313">Condiciones de pruebas</span><span class="sxs-lookup"><span data-stu-id="a76af-313">Testing conditions</span></span>

<span data-ttu-id="a76af-314">Para probar una condición sencilla que use el `If...Then` instrucción, que devuelve `True` o `False` basándose en una prueba que especifique:</span><span class="sxs-lookup"><span data-stu-id="a76af-314">To test a simple condition you use the `If...Then` statement, which returns `True` or `False` based on a test you specify:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample42.vbhtml)]

<span data-ttu-id="a76af-315">El `If` palabra clave inicia un bloque.</span><span class="sxs-lookup"><span data-stu-id="a76af-315">The `If` keyword starts a block.</span></span> <span data-ttu-id="a76af-316">La prueba real (condición) sigue el `If` palabra clave y devuelve true o false.</span><span class="sxs-lookup"><span data-stu-id="a76af-316">The actual test (condition) follows the `If` keyword and returns true or false.</span></span> <span data-ttu-id="a76af-317">El `If` instrucción termina con `Then`.</span><span class="sxs-lookup"><span data-stu-id="a76af-317">The `If` statement ends with `Then`.</span></span> <span data-ttu-id="a76af-318">Las instrucciones que se ejecutarán si la prueba es verdadera se incluyan entre `If` y `End If`.</span><span class="sxs-lookup"><span data-stu-id="a76af-318">The statements that will run if the test is true are enclosed by `If` and `End If`.</span></span> <span data-ttu-id="a76af-319">Un `If` instrucción puede incluir un `Else` bloque que especifica las instrucciones que se ejecutarán si la condición es falsa:</span><span class="sxs-lookup"><span data-stu-id="a76af-319">An `If` statement can include an `Else` block that specifies statements to run if the condition is false:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample43.vbhtml)]

<span data-ttu-id="a76af-320">Si un `If` instrucción inicia un bloque de código, no tiene que usar el valor normal `Code...End Code` instrucciones para incluir los bloques.</span><span class="sxs-lookup"><span data-stu-id="a76af-320">If an `If` statement starts a code block, you don't have to use the normal `Code...End Code` statements to include the blocks.</span></span> <span data-ttu-id="a76af-321">Puede agregar `@` al bloque, y funcionará.</span><span class="sxs-lookup"><span data-stu-id="a76af-321">You can just add `@` to the block, and it will work.</span></span> <span data-ttu-id="a76af-322">Este enfoque funciona con `If` , así como otra palabras clave que se van seguidas de bloques de código, incluidos de programación de Visual Basic `For`, `For Each`, `Do While`, etcetera.</span><span class="sxs-lookup"><span data-stu-id="a76af-322">This approach works with `If` as well as other Visual Basic programming keywords that are followed by code blocks, including `For`, `For Each`, `Do While`, etc.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample44.vbhtml)]

<span data-ttu-id="a76af-323">Puede agregar varias condiciones de uso de uno o varios `ElseIf` bloques:</span><span class="sxs-lookup"><span data-stu-id="a76af-323">You can add multiple conditions using one or more `ElseIf` blocks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample45.vbhtml)]

<span data-ttu-id="a76af-324">En este ejemplo, si la primera condición en la `If` bloque no es true, el `ElseIf` se comprueba la condición.</span><span class="sxs-lookup"><span data-stu-id="a76af-324">In this example, if the first condition in the `If` block is not true, the `ElseIf` condition is checked.</span></span> <span data-ttu-id="a76af-325">Si se cumple esa condición, las instrucciones en el `ElseIf` bloque se ejecutan.</span><span class="sxs-lookup"><span data-stu-id="a76af-325">If that condition is met, the statements in the `ElseIf` block are executed.</span></span> <span data-ttu-id="a76af-326">Si se cumple ninguna de las condiciones, las instrucciones en el `Else` bloque se ejecutan.</span><span class="sxs-lookup"><span data-stu-id="a76af-326">If none of the conditions are met, the statements in the `Else` block are executed.</span></span> <span data-ttu-id="a76af-327">Puede agregar cualquier número de `ElseIf` bloquea y, a continuación, cierre con un `Else` bloquear como el &quot;todo lo demás&quot; condición.</span><span class="sxs-lookup"><span data-stu-id="a76af-327">You can add any number of `ElseIf` blocks, and then close with an `Else` block as the &quot;everything else&quot; condition.</span></span>

<span data-ttu-id="a76af-328">Para probar un gran número de condiciones, use un `Select Case` bloque:</span><span class="sxs-lookup"><span data-stu-id="a76af-328">To test a large number of conditions, use a `Select Case` block:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample46.vbhtml)]

<span data-ttu-id="a76af-329">El valor para probar es entre paréntesis (en el ejemplo, la variable de día de la semana).</span><span class="sxs-lookup"><span data-stu-id="a76af-329">The value to test is in parentheses (in the example, the weekday variable).</span></span> <span data-ttu-id="a76af-330">Cada prueba individual usa un `Case` instrucción que se muestra un valor.</span><span class="sxs-lookup"><span data-stu-id="a76af-330">Each individual test uses a `Case` statement that lists a value.</span></span> <span data-ttu-id="a76af-331">Si el valor de un `Case` instrucción coincide con el valor de prueba, el código que `Case` bloque se ejecuta.</span><span class="sxs-lookup"><span data-stu-id="a76af-331">If the value of a `Case` statement matches the test value, the code in that `Case` block is executed.</span></span>

<span data-ttu-id="a76af-332">El resultado de los dos últimos bloques condicionales mostrada en un explorador:</span><span class="sxs-lookup"><span data-stu-id="a76af-332">The result of the last two conditional blocks displayed in a browser:</span></span>

![Razor-Img10](introducing-razor-syntax-vb/_static/image10.jpg)

### <a name="looping-code"></a><span data-ttu-id="a76af-334">Código de bucles</span><span class="sxs-lookup"><span data-stu-id="a76af-334">Looping code</span></span>

<span data-ttu-id="a76af-335">A menudo necesitará ejecutar repetidamente las mismas instrucciones.</span><span class="sxs-lookup"><span data-stu-id="a76af-335">You often need to run the same statements repeatedly.</span></span> <span data-ttu-id="a76af-336">La forma de hacerlo es mediante un bucle.</span><span class="sxs-lookup"><span data-stu-id="a76af-336">You do this by looping.</span></span> <span data-ttu-id="a76af-337">Por ejemplo, se ejecuta con frecuencia las mismas instrucciones para cada elemento en una colección de datos.</span><span class="sxs-lookup"><span data-stu-id="a76af-337">For example, you often run the same statements for each item in a collection of data.</span></span> <span data-ttu-id="a76af-338">Si sabe exactamente cuántas veces desea crear un bucle, puede usar un `For` bucle.</span><span class="sxs-lookup"><span data-stu-id="a76af-338">If you know exactly how many times you want to loop, you can use a `For` loop.</span></span> <span data-ttu-id="a76af-339">Este tipo de bucle es especialmente útil para contar o cuenta atrás:</span><span class="sxs-lookup"><span data-stu-id="a76af-339">This kind of loop is especially useful for counting up or counting down:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample47.vbhtml)]

<span data-ttu-id="a76af-340">El bucle comienza con la `For` palabra clave, seguida de tres elementos:</span><span class="sxs-lookup"><span data-stu-id="a76af-340">The loop begins with the `For` keyword, followed by three elements:</span></span>

- <span data-ttu-id="a76af-341">Inmediatamente después de la `For` la instrucción, se declara una variable de contador (no tiene que usar `Dim`) y, a continuación, indicar el intervalo, como en `i = 10 to 20`.</span><span class="sxs-lookup"><span data-stu-id="a76af-341">Immediately after the `For` statement, you declare a counter variable (you don't have to use `Dim`) and then indicate the range, as in `i = 10 to 20`.</span></span> <span data-ttu-id="a76af-342">Esto significa que la variable `i` se empezará a contar en 10 y continúa hasta que llega a 20 (ambos inclusive).</span><span class="sxs-lookup"><span data-stu-id="a76af-342">This means the variable `i` will start counting at 10 and continue until it reaches 20 (inclusive).</span></span>
- <span data-ttu-id="a76af-343">Entre los `For` y `Next` instrucciones es el contenido del bloque.</span><span class="sxs-lookup"><span data-stu-id="a76af-343">Between the `For` and `Next` statements is the content of the block.</span></span> <span data-ttu-id="a76af-344">Esto puede contener una o varias instrucciones de código que se ejecutan con cada bucle.</span><span class="sxs-lookup"><span data-stu-id="a76af-344">This can contain one or more code statements that execute with each loop.</span></span>
- <span data-ttu-id="a76af-345">El `Next i` instrucción finaliza el bucle.</span><span class="sxs-lookup"><span data-stu-id="a76af-345">The `Next i` statement ends the loop.</span></span> <span data-ttu-id="a76af-346">Incrementa el contador y se inicia la siguiente iteración del bucle.</span><span class="sxs-lookup"><span data-stu-id="a76af-346">It increments the counter and starts the next iteration of the loop.</span></span>

<span data-ttu-id="a76af-347">La línea de código entre la `For` y `Next` líneas contiene el código que se ejecuta para cada iteración del bucle.</span><span class="sxs-lookup"><span data-stu-id="a76af-347">The line of code between the `For` and `Next` lines contains the code that runs for each iteration of the loop.</span></span> <span data-ttu-id="a76af-348">El marcado crea un nuevo párrafo (`<p>` elemento) cada vez y se agrega una línea a la salida, mostrar el valor de i (el contador).</span><span class="sxs-lookup"><span data-stu-id="a76af-348">The markup creates a new paragraph (`<p>` element) each time and adds a line to the output, displaying the value of i (the counter).</span></span> <span data-ttu-id="a76af-349">Al ejecutar esta página, el ejemplo crea 11 líneas que muestra la salida, con el texto de cada línea que indica el número de elemento.</span><span class="sxs-lookup"><span data-stu-id="a76af-349">When you run this page, the example creates 11 lines displaying the output, with the text in each line indicating the item number.</span></span>

![Razor Img11](introducing-razor-syntax-vb/_static/image11.jpg)

<span data-ttu-id="a76af-351">Si está trabajando con una colección o matriz, a menudo se usa un `For Each` bucle.</span><span class="sxs-lookup"><span data-stu-id="a76af-351">If you're working with a collection or array, you often use a `For Each` loop.</span></span> <span data-ttu-id="a76af-352">Una colección es un grupo de objetos similares y el `For Each` bucle permite llevar a cabo una tarea en cada elemento de la colección.</span><span class="sxs-lookup"><span data-stu-id="a76af-352">A collection is a group of similar objects, and the `For Each` loop lets you carry out a task on each item in the collection.</span></span> <span data-ttu-id="a76af-353">Este tipo de bucle es conveniente para las colecciones, ya que a diferencia de un `For` bucle, no tendrá que incrementar el contador o establecer un límite.</span><span class="sxs-lookup"><span data-stu-id="a76af-353">This type of loop is convenient for collections, because unlike a `For` loop, you don't have to increment the counter or set a limit.</span></span> <span data-ttu-id="a76af-354">En su lugar, el `For Each` código del bucle for simplemente continúa a través de la colección hasta que haya terminado.</span><span class="sxs-lookup"><span data-stu-id="a76af-354">Instead, the `For Each` loop code simply proceeds through the collection until it's finished.</span></span>

<span data-ttu-id="a76af-355">En este ejemplo devuelve los elementos en el `Request.ServerVariables` colección (que contiene información sobre el servidor web).</span><span class="sxs-lookup"><span data-stu-id="a76af-355">This example returns the items in the `Request.ServerVariables` collection (which contains information about your web server).</span></span> <span data-ttu-id="a76af-356">Usa un `For Each` bucle para mostrar el nombre de cada elemento mediante la creación de un nuevo `<li>` elemento en una lista con viñetas en HTML.</span><span class="sxs-lookup"><span data-stu-id="a76af-356">It uses a `For Each` loop to display the name of each item by creating a new `<li>` element in an HTML bulleted list.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample48.vbhtml)]

<span data-ttu-id="a76af-357">El `For Each` palabra clave va seguida de una variable que representa un elemento único en la colección (en el ejemplo, `myItem`), seguido por el `In` palabra clave, seguida de la colección que desea para recorrer en iteración.</span><span class="sxs-lookup"><span data-stu-id="a76af-357">The `For Each` keyword is followed by a variable that represents a single item in the collection (in the example, `myItem`), followed by the `In` keyword, followed by the collection you want to loop through.</span></span> <span data-ttu-id="a76af-358">En el cuerpo de la `For Each` bucle, puede obtener acceso al elemento actual utilizando la variable que declaró anteriormente.</span><span class="sxs-lookup"><span data-stu-id="a76af-358">In the body of the `For Each` loop, you can access the current item using the variable that you declared earlier.</span></span>

![Razor Img12](introducing-razor-syntax-vb/_static/image12.jpg)

<span data-ttu-id="a76af-360">Para crear un bucle de uso más general, utilice el `Do While` instrucción:</span><span class="sxs-lookup"><span data-stu-id="a76af-360">To create a more general-purpose loop, use the `Do While` statement:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample49.vbhtml)]

<span data-ttu-id="a76af-361">Este bucle comienza con la `Do While` palabra clave, seguida de una condición, seguido del bloque que se repita.</span><span class="sxs-lookup"><span data-stu-id="a76af-361">This loop begins with the `Do While` keyword, followed by a condition, followed by the block to repeat.</span></span> <span data-ttu-id="a76af-362">Bucles normalmente incrementar (agregar a) o el decremento (restar) una variable o un objeto que se usa para el recuento.</span><span class="sxs-lookup"><span data-stu-id="a76af-362">Loops typically increment (add to) or decrement (subtract from) a variable or object used for counting.</span></span> <span data-ttu-id="a76af-363">En el ejemplo, el `+=` operador de suma 1 al valor de una variable cada vez que se ejecuta el bucle.</span><span class="sxs-lookup"><span data-stu-id="a76af-363">In the example, the `+=` operator adds 1 to the value of a variable each time the loop runs.</span></span> <span data-ttu-id="a76af-364">(Para disminuyen una variable en un bucle que cuenta hacia atrás, usaría el operador de decremento `-=`.)</span><span class="sxs-lookup"><span data-stu-id="a76af-364">(To decrement a variable in a loop that counts down, you would use the decrement operator `-=`.)</span></span>

## <a name="objects-and-collections"></a><span data-ttu-id="a76af-365">Objetos y colecciones</span><span class="sxs-lookup"><span data-stu-id="a76af-365">Objects and Collections</span></span>

<span data-ttu-id="a76af-366">Casi todo el contenido de un sitio Web ASP.NET es un objeto, incluida la propia página web.</span><span class="sxs-lookup"><span data-stu-id="a76af-366">Nearly everything in an ASP.NET website is an object, including the web page itself.</span></span> <span data-ttu-id="a76af-367">En esta sección se describe algunos objetos importantes que se va a trabajar con frecuencia en el código.</span><span class="sxs-lookup"><span data-stu-id="a76af-367">This section discusses some important objects you'll work with frequently in your code.</span></span>

### <a name="page-objects"></a><span data-ttu-id="a76af-368">Objetos de página</span><span class="sxs-lookup"><span data-stu-id="a76af-368">Page objects</span></span>

<span data-ttu-id="a76af-369">El objeto más básico de ASP.NET es la página.</span><span class="sxs-lookup"><span data-stu-id="a76af-369">The most basic object in ASP.NET is the page.</span></span> <span data-ttu-id="a76af-370">Puede tener acceso a las propiedades del objeto page directamente sin ningún objeto de calificación.</span><span class="sxs-lookup"><span data-stu-id="a76af-370">You can access properties of the page object directly without any qualifying object.</span></span> <span data-ttu-id="a76af-371">El código siguiente obtiene la ruta de acceso de archivo de la página, utilizando el `Request` objeto de la página:</span><span class="sxs-lookup"><span data-stu-id="a76af-371">The following code gets the page's file path, using the `Request` object of the page:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample50.vbhtml)]

<span data-ttu-id="a76af-372">Puede usar las propiedades de la `Page` objeto para obtener una gran cantidad de información, como:</span><span class="sxs-lookup"><span data-stu-id="a76af-372">You can use properties of the `Page` object to get a lot of information, such as:</span></span>

- <span data-ttu-id="a76af-373">`Request`.</span><span class="sxs-lookup"><span data-stu-id="a76af-373">`Request`.</span></span> <span data-ttu-id="a76af-374">Como ya hemos visto, esto es una colección de información acerca de la solicitud actual, incluidos el tipo de explorador realizó la solicitud, la dirección URL de la página, la identidad del usuario, etcetera.</span><span class="sxs-lookup"><span data-stu-id="a76af-374">As you've already seen, this is a collection of information about the current request, including what type of browser made the request, the URL of the page, the user identity, etc.</span></span>
- <span data-ttu-id="a76af-375">`Response`.</span><span class="sxs-lookup"><span data-stu-id="a76af-375">`Response`.</span></span> <span data-ttu-id="a76af-376">Se trata de una colección de información sobre la respuesta (página) que se enviará al explorador cuando haya terminado de ejecutarse el código del servidor.</span><span class="sxs-lookup"><span data-stu-id="a76af-376">This is a collection of information about the response (page) that will be sent to the browser when the server code has finished running.</span></span> <span data-ttu-id="a76af-377">Por ejemplo, puede utilizar esta propiedad para escribir información en la respuesta.</span><span class="sxs-lookup"><span data-stu-id="a76af-377">For example, you can use this property to write information into the response.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample51.vbhtml)]

### <a name="collection-objects-arrays-and-dictionaries"></a><span data-ttu-id="a76af-378">Objetos de colección (matrices y diccionarios)</span><span class="sxs-lookup"><span data-stu-id="a76af-378">Collection objects (arrays and dictionaries)</span></span>

<span data-ttu-id="a76af-379">Una colección es un grupo de objetos del mismo tipo, como una colección de `Customer` objetos desde una base de datos.</span><span class="sxs-lookup"><span data-stu-id="a76af-379">A collection is a group of objects of the same type, such as a collection of `Customer` objects from a database.</span></span> <span data-ttu-id="a76af-380">ASP.NET contiene muchas colecciones integradas, como el `Request.Files` colección.</span><span class="sxs-lookup"><span data-stu-id="a76af-380">ASP.NET contains many built-in collections, like the `Request.Files` collection.</span></span>

<span data-ttu-id="a76af-381">A menudo a trabajar con datos en colecciones.</span><span class="sxs-lookup"><span data-stu-id="a76af-381">You'll often work with data in collections.</span></span> <span data-ttu-id="a76af-382">Dos tipos de colección comunes son el *matriz* y *diccionario*.</span><span class="sxs-lookup"><span data-stu-id="a76af-382">Two common collection types are the *array* and the *dictionary*.</span></span> <span data-ttu-id="a76af-383">Una matriz es útil cuando desea almacenar una colección de elementos similares, pero no desea crear una variable independiente para contener cada elemento:</span><span class="sxs-lookup"><span data-stu-id="a76af-383">An array is useful when you want to store a collection of similar items but don't want to create a separate variable to hold each item:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample52.vbhtml)]

<span data-ttu-id="a76af-384">Con las matrices, declarar un tipo de datos específico, como `String`, `Integer`, o `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="a76af-384">With arrays, you declare a specific data type, such as `String`, `Integer`, or `DateTime`.</span></span> <span data-ttu-id="a76af-385">Para indicar que la variable puede contener una matriz, agregue un paréntesis al nombre de variable en la declaración (como `Dim myVar() As String`).</span><span class="sxs-lookup"><span data-stu-id="a76af-385">To indicate that the variable can contain an array, you add parentheses to the variable name in the declaration (such as `Dim myVar() As String`).</span></span> <span data-ttu-id="a76af-386">Puede obtener acceso a los elementos en una matriz mediante su posición (index) o mediante el `For Each` instrucción.</span><span class="sxs-lookup"><span data-stu-id="a76af-386">You can access items in an array using their position (index) or by using the `For Each` statement.</span></span> <span data-ttu-id="a76af-387">Los índices de matriz son de base cero &#8212; es decir, el primer elemento está en posición 0, el segundo elemento es en la posición 1 y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="a76af-387">Array indexes are zero-based &#8212; that is, the first item is at position 0, the second item is at position 1, and so on.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample53.vbhtml)]

<span data-ttu-id="a76af-388">Puede determinar el número de elementos en una matriz mediante la obtención de su `Length` propiedad.</span><span class="sxs-lookup"><span data-stu-id="a76af-388">You can determine the number of items in an array by getting its `Length` property.</span></span> <span data-ttu-id="a76af-389">Para obtener la posición de un elemento específico de la matriz (es decir, para buscar en la matriz), use el `Array.IndexOf` método.</span><span class="sxs-lookup"><span data-stu-id="a76af-389">To get the position of a specific item in the array (that is, to search the array), use the `Array.IndexOf` method.</span></span> <span data-ttu-id="a76af-390">También puede hacer cosas como inversa el contenido de una matriz (el `Array.Reverse` método) u ordenar el contenido (el `Array.Sort` método).</span><span class="sxs-lookup"><span data-stu-id="a76af-390">You can also do things like reverse the contents of an array (the `Array.Reverse` method) or sort the contents (the `Array.Sort` method).</span></span>

<span data-ttu-id="a76af-391">La salida del código de matriz de cadena mostrado en un explorador:</span><span class="sxs-lookup"><span data-stu-id="a76af-391">The output of the string array code displayed in a browser:</span></span>

![Razor-Img13](introducing-razor-syntax-vb/_static/image13.jpg)

<span data-ttu-id="a76af-393">Un diccionario es una colección de pares clave/valor, donde proporcionar la clave (o nombre) para establecer o recuperar el valor correspondiente:</span><span class="sxs-lookup"><span data-stu-id="a76af-393">A dictionary is a collection of key/value pairs, where you provide the key (or name) to set or retrieve the corresponding value:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample54.vbhtml)]

<span data-ttu-id="a76af-394">Para crear un diccionario, utilice el `New` palabra clave para indicar que está creando un nuevo `Dictionary` objeto.</span><span class="sxs-lookup"><span data-stu-id="a76af-394">To create a dictionary, you use the `New` keyword to indicate that you're creating a new `Dictionary` object.</span></span> <span data-ttu-id="a76af-395">Un diccionario puede asignar a una variable mediante el `Dim` palabra clave.</span><span class="sxs-lookup"><span data-stu-id="a76af-395">You can assign a dictionary to a variable using the `Dim` keyword.</span></span> <span data-ttu-id="a76af-396">Indicar los tipos de datos de los elementos del diccionario utilizando paréntesis ( `( )` ).</span><span class="sxs-lookup"><span data-stu-id="a76af-396">You indicate the data types of the items in the dictionary using parentheses ( `( )` ).</span></span> <span data-ttu-id="a76af-397">Al final de la declaración, debe agregar otro par de paréntesis, ya que esto es realmente un método que crea un nuevo diccionario.</span><span class="sxs-lookup"><span data-stu-id="a76af-397">At the end of the declaration, you must add another pair of parentheses, because this is actually a method that creates a new dictionary.</span></span>

<span data-ttu-id="a76af-398">Para agregar elementos al diccionario, puede llamar a la `Add` método de la variable del diccionario (`myScores` en este caso) y, a continuación, especifique una clave y un valor.</span><span class="sxs-lookup"><span data-stu-id="a76af-398">To add items to the dictionary, you can call the `Add` method of the dictionary variable (`myScores` in this case), and then specify a key and a value.</span></span> <span data-ttu-id="a76af-399">Como alternativa, puede usar paréntesis para indicar la clave y realizar una asignación sencilla, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="a76af-399">Alternatively, you can use parentheses to indicate the key and do a simple assignment, as in the following example:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample55.vbhtml)]

<span data-ttu-id="a76af-400">Para obtener un valor del diccionario, especifique la clave entre paréntesis:</span><span class="sxs-lookup"><span data-stu-id="a76af-400">To get a value from the dictionary, you specify the key in parentheses:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample56.vbhtml)]

## <a name="calling-methods-with-parameters"></a><span data-ttu-id="a76af-401">Llamar a métodos con parámetros</span><span class="sxs-lookup"><span data-stu-id="a76af-401">Calling Methods with Parameters</span></span>

<span data-ttu-id="a76af-402">Como vimos anteriormente en este artículo, los objetos que programación con tienen métodos.</span><span class="sxs-lookup"><span data-stu-id="a76af-402">As you saw earlier in this article, the objects that you program with have methods.</span></span> <span data-ttu-id="a76af-403">Por ejemplo, un `Database` objeto podría tener un `Database.Connect` método.</span><span class="sxs-lookup"><span data-stu-id="a76af-403">For example, a `Database` object might have a `Database.Connect` method.</span></span> <span data-ttu-id="a76af-404">Muchos métodos también tienen uno o más parámetros.</span><span class="sxs-lookup"><span data-stu-id="a76af-404">Many methods also have one or more parameters.</span></span> <span data-ttu-id="a76af-405">Un *parámetro* es un valor que se pasa a un método para habilitar el método completar su tarea.</span><span class="sxs-lookup"><span data-stu-id="a76af-405">A *parameter* is a value that you pass to a method to enable the method to complete its task.</span></span> <span data-ttu-id="a76af-406">Por ejemplo, examine una declaración para el `Request.MapPath` método, que toma tres parámetros:</span><span class="sxs-lookup"><span data-stu-id="a76af-406">For example, look at a declaration for the `Request.MapPath` method, which takes three parameters:</span></span>

[!code-vb[Main](introducing-razor-syntax-vb/samples/sample57.vb)]

<span data-ttu-id="a76af-407">Este método devuelve la ruta de acceso física en el servidor que corresponde a una ruta de acceso virtual especificada.</span><span class="sxs-lookup"><span data-stu-id="a76af-407">This method returns the physical path on the server that corresponds to a specified virtual path.</span></span> <span data-ttu-id="a76af-408">Los tres parámetros del método son `virtualPath`, `baseVirtualDir`, y `allowCrossAppMapping`.</span><span class="sxs-lookup"><span data-stu-id="a76af-408">The three parameters for the method are `virtualPath`, `baseVirtualDir`, and `allowCrossAppMapping`.</span></span> <span data-ttu-id="a76af-409">(Tenga en cuenta que en la declaración, se enumeran los parámetros con los tipos de datos de los datos que aceptan). Cuando se llama a este método, debe proporcionar valores para los tres parámetros.</span><span class="sxs-lookup"><span data-stu-id="a76af-409">(Notice that in the declaration, the parameters are listed with the data types of the data that they'll accept.) When you call this method, you must supply values for all three parameters.</span></span>

<span data-ttu-id="a76af-410">Cuando se está utilizando Visual Basic con la sintaxis de Razor, tiene dos opciones para pasar parámetros a un método: *parámetros posicionales* o *parámetros con nombre*.</span><span class="sxs-lookup"><span data-stu-id="a76af-410">When you're using Visual Basic with the Razor syntax, you have two options for passing parameters to a method: *positional parameters* or *named parameters*.</span></span> <span data-ttu-id="a76af-411">Para llamar a un método con parámetros posicionales, pasar los parámetros en un orden estricto que se especifica en la declaración del método.</span><span class="sxs-lookup"><span data-stu-id="a76af-411">To call a method using positional parameters, you pass the parameters in a strict order that's specified in the method declaration.</span></span> <span data-ttu-id="a76af-412">(Normalmente, sabría este orden, lea la documentación del método.) Debe seguir el orden y no se puede omitir cualquiera de los parámetros &#8212; si es necesario, pasa una cadena vacía (`""`) o null para un parámetro de posición que no tienen un valor para.</span><span class="sxs-lookup"><span data-stu-id="a76af-412">(You would typically know this order by reading documentation for the method.) You must follow the order, and you can't skip any of the parameters &#8212; if necessary, you pass an empty string (`""`) or null for a positional parameter that you don't have a value for.</span></span>

<span data-ttu-id="a76af-413">En el siguiente ejemplo se supone que tiene una carpeta denominada *scripts* en su sitio Web.</span><span class="sxs-lookup"><span data-stu-id="a76af-413">The following example assumes you have a folder named *scripts* on your website.</span></span> <span data-ttu-id="a76af-414">El código llama a la `Request.MapPath` y pasa valores para los tres parámetros en el orden correcto.</span><span class="sxs-lookup"><span data-stu-id="a76af-414">The code calls the `Request.MapPath` method and passes values for the three parameters in the correct order.</span></span> <span data-ttu-id="a76af-415">A continuación, muestra la ruta de acceso asignado resultante.</span><span class="sxs-lookup"><span data-stu-id="a76af-415">It then displays the resulting mapped path.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample58.vbhtml)]

<span data-ttu-id="a76af-416">Cuando hay demasiados parámetros para un método, puede mantener el código más limpio y sea más legible mediante el uso de parámetros con nombre.</span><span class="sxs-lookup"><span data-stu-id="a76af-416">When there are many parameters for a method, you can keep your code cleaner and more readable by using named parameters.</span></span> <span data-ttu-id="a76af-417">Para llamar a un método con parámetros con nombre, especifique el nombre del parámetro seguido `:=` y, a continuación, proporcione el valor.</span><span class="sxs-lookup"><span data-stu-id="a76af-417">To call a method using named parameters, specify the parameter name followed by `:=` and then provide the value.</span></span> <span data-ttu-id="a76af-418">Una ventaja de parámetros con nombre es que puede agregarlos en cualquier orden que desee.</span><span class="sxs-lookup"><span data-stu-id="a76af-418">An advantage of named parameters is that you can add them in any order you want.</span></span> <span data-ttu-id="a76af-419">(Una desventaja es que la llamada al método no es lo más compacto).</span><span class="sxs-lookup"><span data-stu-id="a76af-419">(A disadvantage is that the method call is not as compact.)</span></span>

<span data-ttu-id="a76af-420">El ejemplo siguiente llama al mismo método que el anterior, pero usa parámetros para proporcionar los valores con nombre:</span><span class="sxs-lookup"><span data-stu-id="a76af-420">The following example calls the same method as above, but uses named parameters to supply the values:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample59.vbhtml)]

<span data-ttu-id="a76af-421">Como puede ver, los parámetros se pasan en un orden diferente.</span><span class="sxs-lookup"><span data-stu-id="a76af-421">As you can see, the parameters are passed in a different order.</span></span> <span data-ttu-id="a76af-422">Sin embargo, si ejecuta el ejemplo anterior y este ejemplo, devolverá el mismo valor.</span><span class="sxs-lookup"><span data-stu-id="a76af-422">However, if you run the previous example and this example, they'll return the same value.</span></span>

## <a name="handling-errors"></a><span data-ttu-id="a76af-423">Control de errores</span><span class="sxs-lookup"><span data-stu-id="a76af-423">Handling Errors</span></span>

### <a name="try-catch-statements"></a><span data-ttu-id="a76af-424">Instrucciones Try-Catch</span><span class="sxs-lookup"><span data-stu-id="a76af-424">Try-Catch statements</span></span>

<span data-ttu-id="a76af-425">A menudo tendrá las instrucciones en el código que podría producir un error por razones de fuera de su control.</span><span class="sxs-lookup"><span data-stu-id="a76af-425">You'll often have statements in your code that might fail for reasons outside your control.</span></span> <span data-ttu-id="a76af-426">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="a76af-426">For example:</span></span>

- <span data-ttu-id="a76af-427">Si el código intenta abrir, crear, leer o escribir un archivo, puede producirse todo tipo de errores.</span><span class="sxs-lookup"><span data-stu-id="a76af-427">If your code tries to open, create, read, or write a file, all sorts of errors might occur.</span></span> <span data-ttu-id="a76af-428">No es posible que existe el archivo que desea, podría estar bloqueado, el código podría no tener los permisos y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="a76af-428">The file you want might not exist, it might be locked, the code might not have permissions, and so on.</span></span>
- <span data-ttu-id="a76af-429">De forma similar, si el código intenta actualizar registros en una base de datos, puede haber problemas con los permisos, es posible que se pierda la conexión a la base de datos, los datos que se va a guardar podrían ser no válido y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="a76af-429">Similarly, if your code tries to update records in a database, there can be permissions issues, the connection to the database might be dropped, the data to save might be invalid, and so on.</span></span>

<span data-ttu-id="a76af-430">En términos de programación, estas situaciones se conocen como *excepciones*.</span><span class="sxs-lookup"><span data-stu-id="a76af-430">In programming terms, these situations are called *exceptions*.</span></span> <span data-ttu-id="a76af-431">Si el código encuentra una excepción, genera (produce) un mensaje de error: es decir, en el mejor, molesta a los usuarios.</span><span class="sxs-lookup"><span data-stu-id="a76af-431">If your code encounters an exception, it generates (throws) an error message that is, at best, annoying to users.</span></span>

![Razor Img14](introducing-razor-syntax-vb/_static/image14.jpg)

<span data-ttu-id="a76af-433">En situaciones donde el código puede encontrar excepciones y con el fin de evitar los mensajes de error de este tipo, puede usar `Try/Catch` instrucciones.</span><span class="sxs-lookup"><span data-stu-id="a76af-433">In situations where your code might encounter exceptions, and in order to avoid error messages of this type, you can use `Try/Catch` statements.</span></span> <span data-ttu-id="a76af-434">En el `Try` (instrucción), ejecute el código que está protegiendo.</span><span class="sxs-lookup"><span data-stu-id="a76af-434">In the `Try` statement, you run the code that you're checking.</span></span> <span data-ttu-id="a76af-435">En uno o varios `Catch` instrucciones, puede buscar de determinados errores (tipos específicos de excepciones) que pudieran haberse producido.</span><span class="sxs-lookup"><span data-stu-id="a76af-435">In one or more `Catch` statements, you can look for specific errors (specific types of exceptions) that might have occurred.</span></span> <span data-ttu-id="a76af-436">Puede incluir tantos `Catch` las instrucciones que necesitan buscar errores que están anticipación.</span><span class="sxs-lookup"><span data-stu-id="a76af-436">You can include as many `Catch` statements as you need to look for errors that you're anticipating.</span></span>

> [!NOTE]
> <span data-ttu-id="a76af-437">Se recomienda evitar el uso de la `Response.Redirect` método `Try/Catch` instrucciones, ya que puede provocar una excepción en la página.</span><span class="sxs-lookup"><span data-stu-id="a76af-437">We recommend that you avoid using the `Response.Redirect` method in `Try/Catch` statements, because it can cause an exception in your page.</span></span>


<span data-ttu-id="a76af-438">El ejemplo siguiente muestra una página que se crea un archivo de texto en la primera solicitud y, a continuación, muestra un botón que permite al usuario abrir el archivo.</span><span class="sxs-lookup"><span data-stu-id="a76af-438">The following example shows a page that creates a text file on the first request and then displays a button that lets the user open the file.</span></span> <span data-ttu-id="a76af-439">En el ejemplo se usa deliberadamente un nombre de archivo incorrecto por lo que provocará una excepción.</span><span class="sxs-lookup"><span data-stu-id="a76af-439">The example deliberately uses a bad file name so that it will cause an exception.</span></span> <span data-ttu-id="a76af-440">El código incluye `Catch` instrucciones para dos posibles excepciones: `FileNotFoundException`, que se produce si el nombre de archivo es incorrecto, y `DirectoryNotFoundException`, lo que sucederá si ASP.NET aún no se encuentra la carpeta.</span><span class="sxs-lookup"><span data-stu-id="a76af-440">The code includes `Catch` statements for two possible exceptions: `FileNotFoundException`, which occurs if the file name is bad, and `DirectoryNotFoundException`, which occurs if ASP.NET can't even find the folder.</span></span> <span data-ttu-id="a76af-441">(Se puede quitar el comentario una instrucción en el ejemplo con el fin de ver cómo se ejecuta cuando todo funciona correctamente.)</span><span class="sxs-lookup"><span data-stu-id="a76af-441">(You can uncomment a statement in the example in order to see how it runs when everything works properly.)</span></span>

<span data-ttu-id="a76af-442">Si el código no controla la excepción, verá una página de error similar a la captura de pantalla anterior.</span><span class="sxs-lookup"><span data-stu-id="a76af-442">If your code didn't handle the exception, you would see an error page like the previous screen shot.</span></span> <span data-ttu-id="a76af-443">Sin embargo, la `Try/Catch` sección le ayuda a impedir que el usuario se vean estos tipos de errores.</span><span class="sxs-lookup"><span data-stu-id="a76af-443">However, the `Try/Catch` section helps prevent the user from seeing these types of errors.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample60.vbhtml)]

## <a name="additional-resources"></a><span data-ttu-id="a76af-444">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="a76af-444">Additional Resources</span></span>

### <a name="reference-documentation"></a><span data-ttu-id="a76af-445">Documentación de referencia</span><span class="sxs-lookup"><span data-stu-id="a76af-445">Reference Documentation</span></span>

- [<span data-ttu-id="a76af-446">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a76af-446">ASP.NET</span></span>](https://msdn.microsoft.com/library/ee532866.aspx)
- [<span data-ttu-id="a76af-447">Visual Basic (lenguaje)</span><span class="sxs-lookup"><span data-stu-id="a76af-447">Visual Basic Language</span></span>](https://msdn.microsoft.com/library/2x7h1hfk.aspx)
