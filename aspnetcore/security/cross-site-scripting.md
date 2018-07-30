---
title: Evitar Cross-Site Scripting (XSS) en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre Cross-Site Scripting (XSS) y las técnicas para solucionar esta vulnerabilidad en una aplicación ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/cross-site-scripting
ms.openlocfilehash: 4784b1775d955f0ef00526e50b960fc873ea218d
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342216"
---
# <a name="prevent-cross-site-scripting-xss-in-aspnet-core"></a><span data-ttu-id="08c23-103">Evitar Cross-Site Scripting (XSS) en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="08c23-103">Prevent Cross-Site Scripting (XSS) in ASP.NET Core</span></span>

<span data-ttu-id="08c23-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="08c23-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="08c23-105">Cross-Site Scripting (XSS) es una vulnerabilidad de seguridad que permite que un atacante colocar los scripts de cliente (normalmente JavaScript) en las páginas web.</span><span class="sxs-lookup"><span data-stu-id="08c23-105">Cross-Site Scripting (XSS) is a security vulnerability which enables an attacker to place client side scripts (usually JavaScript) into web pages.</span></span> <span data-ttu-id="08c23-106">Cuando otros usuarios cargar páginas afectadas se ejecutarán las secuencias de comandos de los atacantes, lo que podría robar cookies y tokens de sesión, cambiar el contenido de la página web a través de la manipulación del DOM o redirigir el explorador a otra página.</span><span class="sxs-lookup"><span data-stu-id="08c23-106">When other users load affected pages the attackers scripts will run, enabling the attacker to steal cookies and session tokens, change the contents of the web page through DOM manipulation or redirect the browser to another page.</span></span> <span data-ttu-id="08c23-107">Las vulnerabilidades XSS suelen producen cuando una aplicación toma la entrada del usuario y lo envía en una página sin validación, codificación o secuencias de escape.</span><span class="sxs-lookup"><span data-stu-id="08c23-107">XSS vulnerabilities generally occur when an application takes user input and outputs it in a page without validating, encoding or escaping it.</span></span>

## <a name="protecting-your-application-against-xss"></a><span data-ttu-id="08c23-108">Proteger la aplicación frente a XSS</span><span class="sxs-lookup"><span data-stu-id="08c23-108">Protecting your application against XSS</span></span>

<span data-ttu-id="08c23-109">En un XSS de nivel básico funciona engañar a la aplicación en la inserción de un `<script>` etiqueta en la página representada o insertando un `On*` eventos en un elemento.</span><span class="sxs-lookup"><span data-stu-id="08c23-109">At a basic level XSS works by tricking your application into inserting a `<script>` tag into your rendered page, or by inserting an `On*` event into an element.</span></span> <span data-ttu-id="08c23-110">Los desarrolladores deben usar los siguientes pasos de prevención para evitar introducir XSS en su aplicación.</span><span class="sxs-lookup"><span data-stu-id="08c23-110">Developers should use the following prevention steps to avoid introducing XSS into their application.</span></span>

1. <span data-ttu-id="08c23-111">Nunca Coloque datos que no se confía en la entrada HTML, a menos que siga el resto de los pasos siguientes.</span><span class="sxs-lookup"><span data-stu-id="08c23-111">Never put untrusted data into your HTML input, unless you follow the rest of the steps below.</span></span> <span data-ttu-id="08c23-112">Datos de confianza están cualquier dato que puede estar controlada por un atacante, entradas de formulario HTML, las cadenas de consulta, encabezados HTTP, incluso los datos proceden de una base de datos como un atacante puede infringir la base de datos aunque no pueden infringir la aplicación.</span><span class="sxs-lookup"><span data-stu-id="08c23-112">Untrusted data is any data that may be controlled by an attacker, HTML form inputs, query strings, HTTP headers, even data sourced from a database as an attacker may be able to breach your database even if they cannot breach your application.</span></span>

2. <span data-ttu-id="08c23-113">Asegúrese de que está codificado en HTML antes de poner los datos de confianza dentro de un elemento HTML.</span><span class="sxs-lookup"><span data-stu-id="08c23-113">Before putting untrusted data inside an HTML element ensure it's HTML encoded.</span></span> <span data-ttu-id="08c23-114">Codificación HTML toma como caracteres &lt; y se modifican en un formulario seguro como &amp;lt;</span><span class="sxs-lookup"><span data-stu-id="08c23-114">HTML encoding takes characters such as &lt; and changes them into a safe form like &amp;lt;</span></span>

3. <span data-ttu-id="08c23-115">Antes de poner los datos que no se confía en un atributo HTML Asegúrese de que es el atributo HTML codificado.</span><span class="sxs-lookup"><span data-stu-id="08c23-115">Before putting untrusted data into an HTML attribute ensure it's HTML attribute encoded.</span></span> <span data-ttu-id="08c23-116">Codificación del atributo HTML es un superconjunto de codificación HTML y codifica los caracteres adicionales, como "y".</span><span class="sxs-lookup"><span data-stu-id="08c23-116">HTML attribute encoding is a superset of HTML encoding and encodes additional characters such as " and '.</span></span>

4. <span data-ttu-id="08c23-117">Antes de poner los datos que no se confía en JavaScript, coloque los datos en un elemento HTML cuyo contenido se recupera en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="08c23-117">Before putting untrusted data into JavaScript place the data in an HTML element whose contents you retrieve at runtime.</span></span> <span data-ttu-id="08c23-118">Si esto no es posible, asegúrese de los datos se codifica JavaScript.</span><span class="sxs-lookup"><span data-stu-id="08c23-118">If this isn't possible then ensure the data is JavaScript encoded.</span></span> <span data-ttu-id="08c23-119">Codificación de JavaScript toma caracteres peligrosos para JavaScript y se reemplaza con su hexadecimal, por ejemplo &lt; podría codificarse como `\u003C`.</span><span class="sxs-lookup"><span data-stu-id="08c23-119">JavaScript encoding takes dangerous characters for JavaScript and replaces them with their hex, for example &lt; would be encoded as `\u003C`.</span></span>

5. <span data-ttu-id="08c23-120">Antes de poner los datos que no se confía en una cadena de consulta de dirección URL Asegúrese de que está codificado como URL.</span><span class="sxs-lookup"><span data-stu-id="08c23-120">Before putting untrusted data into a URL query string ensure it's URL encoded.</span></span>

## <a name="html-encoding-using-razor"></a><span data-ttu-id="08c23-121">Codificación HTML con Razor</span><span class="sxs-lookup"><span data-stu-id="08c23-121">HTML Encoding using Razor</span></span>

<span data-ttu-id="08c23-122">El motor de Razor uso automáticamente en MVC codifica todos salida proceden de las variables, a menos que se trabaja muy duro para evitar que hacerlo.</span><span class="sxs-lookup"><span data-stu-id="08c23-122">The Razor engine used in MVC automatically encodes all output sourced from variables, unless you work really hard to prevent it doing so.</span></span> <span data-ttu-id="08c23-123">Usa el atributo HTML las reglas de codificación, siempre que se use la *@* directiva.</span><span class="sxs-lookup"><span data-stu-id="08c23-123">It uses HTML Attribute encoding rules whenever you use the *@* directive.</span></span> <span data-ttu-id="08c23-124">Como HTML la codificación del atributo es un superconjunto de codificación HTML, que esto significa que no tiene que preocuparse de si debe utilizar la codificación HTML o la codificación del atributo HTML.</span><span class="sxs-lookup"><span data-stu-id="08c23-124">As HTML attribute encoding is a superset of HTML encoding this means you don't have to concern yourself with whether you should use HTML encoding or HTML attribute encoding.</span></span> <span data-ttu-id="08c23-125">Debe asegurarse de que utiliza solo en un contexto HTML, no cuando se intenta insertar una entrada que no se confía directamente en JavaScript.</span><span class="sxs-lookup"><span data-stu-id="08c23-125">You must ensure that you only use @ in an HTML context, not when attempting to insert untrusted input directly into JavaScript.</span></span> <span data-ttu-id="08c23-126">Aplicaciones auxiliares de etiquetas también codificarán la entrada que se usa en los parámetros tag.</span><span class="sxs-lookup"><span data-stu-id="08c23-126">Tag helpers will also encode input you use in tag parameters.</span></span>

<span data-ttu-id="08c23-127">Realizar la siguiente vista de Razor;</span><span class="sxs-lookup"><span data-stu-id="08c23-127">Take the following Razor view;</span></span>

```none
@{
       var untrustedInput = "<\"123\">";
   }

   @untrustedInput
   ```

<span data-ttu-id="08c23-128">Esta vista muestra el contenido de la *untrustedInput* variable.</span><span class="sxs-lookup"><span data-stu-id="08c23-128">This view outputs the contents of the *untrustedInput* variable.</span></span> <span data-ttu-id="08c23-129">Esta variable incluye algunos caracteres que se usan en los ataques XSS, es decir, &lt;, "y &gt;.</span><span class="sxs-lookup"><span data-stu-id="08c23-129">This variable includes some characters which are used in XSS attacks, namely &lt;, " and &gt;.</span></span> <span data-ttu-id="08c23-130">Examinando el origen, muestra la salida representada codificada como:</span><span class="sxs-lookup"><span data-stu-id="08c23-130">Examining the source shows the rendered output encoded as:</span></span>

```html
&lt;&quot;123&quot;&gt;
   ```

>[!WARNING]
> <span data-ttu-id="08c23-131">ASP.NET Core MVC proporciona un `HtmlString` clase que no se codifica de forma automática tras la salida.</span><span class="sxs-lookup"><span data-stu-id="08c23-131">ASP.NET Core MVC provides an `HtmlString` class which isn't automatically encoded upon output.</span></span> <span data-ttu-id="08c23-132">Esto nunca debe utilizarse en combinación con entradas no seguras como esto expondrá una vulnerabilidad XSS.</span><span class="sxs-lookup"><span data-stu-id="08c23-132">This should never be used in combination with untrusted input as this will expose an XSS vulnerability.</span></span>

## <a name="javascript-encoding-using-razor"></a><span data-ttu-id="08c23-133">Codificación de JavaScript usa Razor</span><span class="sxs-lookup"><span data-stu-id="08c23-133">Javascript Encoding using Razor</span></span>

<span data-ttu-id="08c23-134">Puede haber ocasiones en que desea insertar un valor en JavaScript para procesar en la vista.</span><span class="sxs-lookup"><span data-stu-id="08c23-134">There may be times you want to insert a value into JavaScript to process in your view.</span></span> <span data-ttu-id="08c23-135">Hay dos formas de hacerlo.</span><span class="sxs-lookup"><span data-stu-id="08c23-135">There are two ways to do this.</span></span> <span data-ttu-id="08c23-136">La forma más segura para insertar valores es colocar el valor en un atributo de datos de una etiqueta y recuperarlo en el código JavaScript.</span><span class="sxs-lookup"><span data-stu-id="08c23-136">The safest way to insert  values is to place the value in a data attribute of a tag and retrieve it in your JavaScript.</span></span> <span data-ttu-id="08c23-137">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="08c23-137">For example:</span></span>

```none
@{
       var untrustedInput = "<\"123\">";
   }

   <div
       id="injectedData"
       data-untrustedinput="@untrustedInput" />

   <script>
     var injectedData = document.getElementById("injectedData");

     // All clients
     var clientSideUntrustedInputOldStyle =
         injectedData.getAttribute("data-untrustedinput");

     // HTML 5 clients only
     var clientSideUntrustedInputHtml5 =
         injectedData.dataset.untrustedinput;

     document.write(clientSideUntrustedInputOldStyle);
     document.write("<br />")
     document.write(clientSideUntrustedInputHtml5);
   </script>
   ```

<span data-ttu-id="08c23-138">Esto generará el siguiente código HTML</span><span class="sxs-lookup"><span data-stu-id="08c23-138">This will produce the following HTML</span></span>

```html
<div
     id="injectedData"
     data-untrustedinput="&lt;&quot;123&quot;&gt;" />

   <script>
     var injectedData = document.getElementById("injectedData");

     var clientSideUntrustedInputOldStyle =
         injectedData.getAttribute("data-untrustedinput");

     var clientSideUntrustedInputHtml5 =
         injectedData.dataset.untrustedinput;

     document.write(clientSideUntrustedInputOldStyle);
     document.write("<br />")
     document.write(clientSideUntrustedInputHtml5);
   </script>
   ```

<span data-ttu-id="08c23-139">Que, cuando se ejecuta, representarán lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="08c23-139">Which, when it runs, will render the following;</span></span>

```none
<"123">
   <"123">
   ```

<span data-ttu-id="08c23-140">También se puede llamar directamente, el codificador de JavaScript</span><span class="sxs-lookup"><span data-stu-id="08c23-140">You can also call the JavaScript encoder directly,</span></span>

```none
@using System.Text.Encodings.Web;
   @inject JavaScriptEncoder encoder;

   @{
       var untrustedInput = "<\"123\">";
   }

   <script>
       document.write("@encoder.Encode(untrustedInput)");
   </script>
   ```

<span data-ttu-id="08c23-141">Esto se representará en el explorador lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="08c23-141">This will render in the browser as follows;</span></span>

```html
<script>
       document.write("\u003C\u0022123\u0022\u003E");
   </script>
   ```

>[!WARNING]
> <span data-ttu-id="08c23-142">No concatene la entrada que no se confía en JavaScript para crear los elementos DOM.</span><span class="sxs-lookup"><span data-stu-id="08c23-142">Don't concatenate untrusted input in JavaScript to create DOM elements.</span></span> <span data-ttu-id="08c23-143">Debe usar `createElement()` y asignar valores de propiedad de forma adecuada, como `node.TextContent=`, o use `element.SetAttribute()` / `element[attribute]=` en caso contrario, se expone a basado en DOM XSS.</span><span class="sxs-lookup"><span data-stu-id="08c23-143">You should use `createElement()` and assign property values appropriately such as `node.TextContent=`, or use `element.SetAttribute()`/`element[attribute]=` otherwise you expose yourself to DOM-based XSS.</span></span>

## <a name="accessing-encoders-in-code"></a><span data-ttu-id="08c23-144">Obtener acceso a los codificadores en código</span><span class="sxs-lookup"><span data-stu-id="08c23-144">Accessing encoders in code</span></span>

<span data-ttu-id="08c23-145">Los codificadores HTML, JavaScript y URL están disponibles en el código de dos maneras, insértelos a través de [inserción de dependencias](xref:fundamentals/dependency-injection) o puede usar los codificadores predeterminados incluidos en el `System.Text.Encodings.Web` espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="08c23-145">The HTML, JavaScript and URL encoders are available to your code in two ways, you can inject them via [dependency injection](xref:fundamentals/dependency-injection) or you can use the default encoders contained in the `System.Text.Encodings.Web` namespace.</span></span> <span data-ttu-id="08c23-146">Si usa los codificadores de forma predeterminada, a continuación, aplicado a los intervalos de caracteres se traten como seguros no surtirán efecto: los codificadores predeterminada use las reglas de codificación más seguras posible.</span><span class="sxs-lookup"><span data-stu-id="08c23-146">If you use the default encoders then any  you applied to character ranges to be treated as safe won't take effect - the default encoders use the safest encoding rules possible.</span></span>

<span data-ttu-id="08c23-147">Para usar los codificadores configurables a través de DI sus constructores tardará un *HtmlEncoder*, *JavaScriptEncoder* y *UrlEncoder* parámetro según corresponda.</span><span class="sxs-lookup"><span data-stu-id="08c23-147">To use the configurable encoders via DI your constructors should take an *HtmlEncoder*, *JavaScriptEncoder* and *UrlEncoder* parameter as appropriate.</span></span> <span data-ttu-id="08c23-148">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="08c23-148">For example;</span></span>

```csharp
public class HomeController : Controller
   {
       HtmlEncoder _htmlEncoder;
       JavaScriptEncoder _javaScriptEncoder;
       UrlEncoder _urlEncoder;

       public HomeController(HtmlEncoder htmlEncoder,
                             JavaScriptEncoder javascriptEncoder,
                             UrlEncoder urlEncoder)
       {
           _htmlEncoder = htmlEncoder;
           _javaScriptEncoder = javascriptEncoder;
           _urlEncoder = urlEncoder;
       }
   }
   ```

## <a name="encoding-url-parameters"></a><span data-ttu-id="08c23-149">Codificación de parámetros de URL</span><span class="sxs-lookup"><span data-stu-id="08c23-149">Encoding URL Parameters</span></span>

<span data-ttu-id="08c23-150">Si desea compilar una cadena de consulta de dirección URL con entradas no seguras como un valor, use la `UrlEncoder` para codificar el valor.</span><span class="sxs-lookup"><span data-stu-id="08c23-150">If you want to build a URL query string with untrusted input as a value use the `UrlEncoder` to encode the value.</span></span> <span data-ttu-id="08c23-151">Por ejemplo,</span><span class="sxs-lookup"><span data-stu-id="08c23-151">For example,</span></span>

```csharp
var example = "\"Quoted Value with spaces and &\"";
   var encodedValue = _urlEncoder.Encode(example);
   ```

<span data-ttu-id="08c23-152">Después de la codificación del encodedValue variable contendrá `%22Quoted%20Value%20with%20spaces%20and%20%26%22`.</span><span class="sxs-lookup"><span data-stu-id="08c23-152">After encoding the encodedValue variable will contain `%22Quoted%20Value%20with%20spaces%20and%20%26%22`.</span></span> <span data-ttu-id="08c23-153">Espacios, las comillas, signos de puntuación y otros caracteres no seguros será porcentaje codificado en su valor hexadecimal, por ejemplo un carácter de espacio se convertirá en % 20.</span><span class="sxs-lookup"><span data-stu-id="08c23-153">Spaces, quotes, punctuation and other unsafe characters will be percent encoded to their hexadecimal value, for example a space character will become %20.</span></span>

>[!WARNING]
> <span data-ttu-id="08c23-154">No use la confianza de entrada como parte de una ruta de acceso de dirección URL.</span><span class="sxs-lookup"><span data-stu-id="08c23-154">Don't use untrusted input as part of a URL path.</span></span> <span data-ttu-id="08c23-155">Pasar siempre la confianza de entrada como un valor de cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="08c23-155">Always pass untrusted input as a query string value.</span></span>

<a name="security-cross-site-scripting-customization"></a>

## <a name="customizing-the-encoders"></a><span data-ttu-id="08c23-156">Personalización de los codificadores</span><span class="sxs-lookup"><span data-stu-id="08c23-156">Customizing the Encoders</span></span>

<span data-ttu-id="08c23-157">De forma predeterminada, los codificadores utiliza una lista segura limitada al intervalo Unicode Latín básico y codifican todos los caracteres fuera de ese intervalo como sus equivalentes de código de carácter.</span><span class="sxs-lookup"><span data-stu-id="08c23-157">By default encoders use a safe list limited to the Basic Latin Unicode range and encode all characters outside of that range as their character code equivalents.</span></span> <span data-ttu-id="08c23-158">Este comportamiento también afecta a la representación de TagHelper Razor y HtmlHelper como utilizarán los codificadores para las cadenas de salida.</span><span class="sxs-lookup"><span data-stu-id="08c23-158">This behavior also affects Razor TagHelper and HtmlHelper rendering as it will use the encoders to output your strings.</span></span>

<span data-ttu-id="08c23-159">El razonamiento sirve como protección ante errores desconocidos o futuros explorador (errores de explorador anterior han confundiré análisis basado en el procesamiento de caracteres no válidos).</span><span class="sxs-lookup"><span data-stu-id="08c23-159">The reasoning behind this is to protect against unknown or future browser bugs (previous browser bugs have tripped up parsing based on the processing of non-English characters).</span></span> <span data-ttu-id="08c23-160">Si su sitio web hace un uso intensivo de los caracteres no latinos, como el chino, cirílico u otros Esto probablemente no es el comportamiento que desee.</span><span class="sxs-lookup"><span data-stu-id="08c23-160">If your web site makes heavy use of non-Latin characters, such as Chinese, Cyrillic or others this is probably not the behavior you want.</span></span>

<span data-ttu-id="08c23-161">Puede personalizar las listas seguras de codificador para incluir Unicode intervalos adecuados a la aplicación durante el inicio, en `ConfigureServices()`.</span><span class="sxs-lookup"><span data-stu-id="08c23-161">You can customize the encoder safe lists to include Unicode ranges appropriate to your application during startup, in `ConfigureServices()`.</span></span>

<span data-ttu-id="08c23-162">Por ejemplo, mediante la configuración predeterminada podría usar un HtmlHelper Razor de este modo;</span><span class="sxs-lookup"><span data-stu-id="08c23-162">For example, using the default configuration you might use a Razor HtmlHelper like so;</span></span>

```html
<p>This link text is in Chinese: @Html.ActionLink("汉语/漢語", "Index")</p>
   ```

<span data-ttu-id="08c23-163">Al ver el origen de la página web, verá que se ha representado como sigue, con el texto en chino codificado;</span><span class="sxs-lookup"><span data-stu-id="08c23-163">When you view the source of the web page you will see it has been rendered as follows, with the Chinese text encoded;</span></span>

```html
<p>This link text is in Chinese: <a href="/">&#x6C49;&#x8BED;/&#x6F22;&#x8A9E;</a></p>
   ```

<span data-ttu-id="08c23-164">Para ampliar los caracteres que se trata como seguro el codificador se insertaría la línea siguiente en el `ConfigureServices()` método `startup.cs`;</span><span class="sxs-lookup"><span data-stu-id="08c23-164">To widen the characters treated as safe by the encoder you would insert the following line into the `ConfigureServices()` method in `startup.cs`;</span></span>

```csharp
services.AddSingleton<HtmlEncoder>(
     HtmlEncoder.Create(allowedRanges: new[] { UnicodeRanges.BasicLatin,
                                               UnicodeRanges.CjkUnifiedIdeographs }));
   ```

<span data-ttu-id="08c23-165">En este ejemplo se amplía la lista segura para que incluya el CjkUnifiedIdeographs intervalo Unicode.</span><span class="sxs-lookup"><span data-stu-id="08c23-165">This example widens the safe list to include the Unicode Range CjkUnifiedIdeographs.</span></span> <span data-ttu-id="08c23-166">Ahora se convertiría en la salida representada</span><span class="sxs-lookup"><span data-stu-id="08c23-166">The rendered output would now become</span></span>

```html
<p>This link text is in Chinese: <a href="/">汉语/漢語</a></p>
   ```

<span data-ttu-id="08c23-167">Los intervalos de la lista segura se especifican como gráficos de código Unicode, no los idiomas.</span><span class="sxs-lookup"><span data-stu-id="08c23-167">Safe list ranges are specified as Unicode code charts, not languages.</span></span> <span data-ttu-id="08c23-168">El [estándar Unicode](http://unicode.org/) tiene una lista de [gráficos de código](http://www.unicode.org/charts/index.html) puede usar para buscar el gráfico que contiene los caracteres.</span><span class="sxs-lookup"><span data-stu-id="08c23-168">The [Unicode standard](http://unicode.org/) has a list of [code charts](http://www.unicode.org/charts/index.html) you can use to find the chart containing your characters.</span></span> <span data-ttu-id="08c23-169">Cada codificador, Html, JavaScript y dirección Url, debe configurarse por separado.</span><span class="sxs-lookup"><span data-stu-id="08c23-169">Each encoder, Html, JavaScript and Url, must be configured separately.</span></span>

> [!NOTE]
> <span data-ttu-id="08c23-170">Personalización de la lista segura solo afecta a los codificadores de código abiertos a través de DI.</span><span class="sxs-lookup"><span data-stu-id="08c23-170">Customization of the safe list only affects encoders sourced via DI.</span></span> <span data-ttu-id="08c23-171">Si tiene acceso directamente a un codificador a través de `System.Text.Encodings.Web.*Encoder.Default` , a continuación, el valor predeterminado, Latín básico solo safelist se usará.</span><span class="sxs-lookup"><span data-stu-id="08c23-171">If you directly access an encoder via `System.Text.Encodings.Web.*Encoder.Default` then the default, Basic Latin only safelist will be used.</span></span>

## <a name="where-should-encoding-take-place"></a><span data-ttu-id="08c23-172">¿Dónde debe colocar tomar codificación?</span><span class="sxs-lookup"><span data-stu-id="08c23-172">Where should encoding take place?</span></span>

<span data-ttu-id="08c23-173">General acepta práctica es que lleva a cabo en el punto de salida de codificación y los valores codificados nunca deben almacenarse en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="08c23-173">The general accepted practice is that encoding takes place at the point of output and encoded values should never be stored in a database.</span></span> <span data-ttu-id="08c23-174">Codificación en el punto de salida permite cambiar el uso de datos, por ejemplo, de HTML a un valor de cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="08c23-174">Encoding at the point of output allows you to change the use of data, for example, from HTML to a query string value.</span></span> <span data-ttu-id="08c23-175">También permite buscar fácilmente los datos sin tener que codificar valores antes de buscar y le permite aprovechar las ventajas de los cambios o correcciones de errores realizadas en los codificadores.</span><span class="sxs-lookup"><span data-stu-id="08c23-175">It also enables you to easily search your data without having to encode values before searching and allows you to take advantage of any changes or bug fixes made to encoders.</span></span>

## <a name="validation-as-an-xss-prevention-technique"></a><span data-ttu-id="08c23-176">Validación como una técnica de prevención de XSS</span><span class="sxs-lookup"><span data-stu-id="08c23-176">Validation as an XSS prevention technique</span></span>

<span data-ttu-id="08c23-177">Validación puede ser una herramienta útil para limitar los ataques XSS.</span><span class="sxs-lookup"><span data-stu-id="08c23-177">Validation can be a useful tool in limiting XSS attacks.</span></span> <span data-ttu-id="08c23-178">Por ejemplo, un tipo numérico string que contiene los caracteres 0-9 no desencadenará un ataque XSS.</span><span class="sxs-lookup"><span data-stu-id="08c23-178">For example, a numeric string containing only the characters 0-9 won't trigger an XSS attack.</span></span> <span data-ttu-id="08c23-179">Validación se complica si quiere aceptar HTML en la entrada de usuario - análisis de la entrada HTML es difícil, si no imposible.</span><span class="sxs-lookup"><span data-stu-id="08c23-179">Validation becomes more complicated should you wish to accept HTML in user input - parsing HTML input is difficult, if not impossible.</span></span> <span data-ttu-id="08c23-180">MarkDown y otros formatos de texto sería una opción más segura para la entrada enriquecido.</span><span class="sxs-lookup"><span data-stu-id="08c23-180">MarkDown and other text formats would be a safer option for rich input.</span></span> <span data-ttu-id="08c23-181">Nunca debe depender únicamente de validación.</span><span class="sxs-lookup"><span data-stu-id="08c23-181">You should never rely on validation alone.</span></span> <span data-ttu-id="08c23-182">Codificar entrada confianza antes de la salida de siempre, independientemente de qué validación ha realizado.</span><span class="sxs-lookup"><span data-stu-id="08c23-182">Always encode untrusted input before output, no matter what validation you have performed.</span></span>
