---
title: Impedir el scripting entre sitios (XSS) en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre las técnicas de scripting entre sitios (XSS) y para solucionar esta vulnerabilidad en una aplicación ASP.NET Core.
ms.author: riande
ms.date: 10/02/2018
uid: security/cross-site-scripting
ms.openlocfilehash: 1d6f605dc336d8768b8a47e4995f119d198a61af
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172634"
---
# <a name="prevent-cross-site-scripting-xss-in-aspnet-core"></a><span data-ttu-id="197a1-103">Impedir el scripting entre sitios (XSS) en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="197a1-103">Prevent Cross-Site Scripting (XSS) in ASP.NET Core</span></span>

<span data-ttu-id="197a1-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="197a1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="197a1-105">Cross-Site Scripting (XSS) es una vulnerabilidad que permite a un atacante ejecutar scripts desde el lado del cliente (normalmente JavaScript) en las páginas web.</span><span class="sxs-lookup"><span data-stu-id="197a1-105">Cross-Site Scripting (XSS) is a security vulnerability which enables an attacker to place client side scripts (usually JavaScript) into web pages.</span></span> <span data-ttu-id="197a1-106">Cuando otros usuarios cargan las páginas afectadas, se ejecutarán los scripts del atacante, lo que permitirá al atacante robar cookies y tokens de sesión, cambiar el contenido de la página web a través de la manipulación del DOM o redirigir el explorador a otra página.</span><span class="sxs-lookup"><span data-stu-id="197a1-106">When other users load affected pages the attacker's scripts will run, enabling the attacker to steal cookies and session tokens, change the contents of the web page through DOM manipulation or redirect the browser to another page.</span></span> <span data-ttu-id="197a1-107">Las vulnerabilidades XSS suelen producirse cuando una aplicación toma datos proporcionados por el usuario y los envía a una página sin validarla, codificarla o escapar de ella.</span><span class="sxs-lookup"><span data-stu-id="197a1-107">XSS vulnerabilities generally occur when an application takes user input and outputs it to a page without validating, encoding or escaping it.</span></span>

## <a name="protecting-your-application-against-xss"></a><span data-ttu-id="197a1-108">Proteger la aplicación contra XSS</span><span class="sxs-lookup"><span data-stu-id="197a1-108">Protecting your application against XSS</span></span>

<span data-ttu-id="197a1-109">En un nivel básico, XSS funciona engañando a la aplicación para insertar una `<script>` etiqueta en la página representada o insertando un evento de `On*` en un elemento.</span><span class="sxs-lookup"><span data-stu-id="197a1-109">At a basic level XSS works by tricking your application into inserting a `<script>` tag into your rendered page, or by inserting an `On*` event into an element.</span></span> <span data-ttu-id="197a1-110">Los desarrolladores deben seguir los siguientes pasos para evitar introducir una vulnerabilidad XSS en su aplicación.</span><span class="sxs-lookup"><span data-stu-id="197a1-110">Developers should use the following prevention steps to avoid introducing XSS into their application.</span></span>

1. <span data-ttu-id="197a1-111">Nunca coloque datos que no sean de confianza en su código html, a menos que siga el resto de los pasos.</span><span class="sxs-lookup"><span data-stu-id="197a1-111">Never put untrusted data into your HTML input, unless you follow the rest of the steps below.</span></span> <span data-ttu-id="197a1-112">Los datos que no son de confianza son cualquier dato que puede estar controlado por un atacante, entradas de formulario HTML, textos de consultas, cabeceras HTTP, incluso los datos que proceden de una base de datos donde un atacante puede encontrar una vulnerabilidad sin pasar por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="197a1-112">Untrusted data is any data that may be controlled by an attacker, HTML form inputs, query strings, HTTP headers, even data sourced from a database as an attacker may be able to breach your database even if they cannot breach your application.</span></span>

2. <span data-ttu-id="197a1-113">Asegúrese de que el contenido HTML de confianza esté codificado antes de ponerlo dentro de un elemento HTML.</span><span class="sxs-lookup"><span data-stu-id="197a1-113">Before putting untrusted data inside an HTML element ensure it's HTML encoded.</span></span> <span data-ttu-id="197a1-114">La codificación HTML toma caracteres como &lt; y los cambia en un formato seguro como &amp;lt;</span><span class="sxs-lookup"><span data-stu-id="197a1-114">HTML encoding takes characters such as &lt; and changes them into a safe form like &amp;lt;</span></span>

3. <span data-ttu-id="197a1-115">Antes de colocar los datos que no son de confianza en un atributo HTML, asegúrese de que está codificado en HTML.</span><span class="sxs-lookup"><span data-stu-id="197a1-115">Before putting untrusted data into an HTML attribute ensure it's HTML encoded.</span></span> <span data-ttu-id="197a1-116">La codificación en atributos es una versión de la codificación html que toma caracteres como "y".</span><span class="sxs-lookup"><span data-stu-id="197a1-116">HTML attribute encoding is a superset of HTML encoding and encodes additional characters such as " and '.</span></span>

4. <span data-ttu-id="197a1-117">Antes de poner los datos que no sean de confianza en JavaScript, coloque los datos en un elemento HTML cuyo contenido se recupera en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="197a1-117">Before putting untrusted data into JavaScript place the data in an HTML element whose contents you retrieve at runtime.</span></span> <span data-ttu-id="197a1-118">Si esto no es posible, asegúrese de que los datos están codificados con JavaScript.</span><span class="sxs-lookup"><span data-stu-id="197a1-118">If this isn't possible, then ensure the data is JavaScript encoded.</span></span> <span data-ttu-id="197a1-119">La codificación de JavaScript toma caracteres peligrosos para JavaScript y los reemplaza por su hexadecimal, por ejemplo &lt; se codificaría como `\u003C`.</span><span class="sxs-lookup"><span data-stu-id="197a1-119">JavaScript encoding takes dangerous characters for JavaScript and replaces them with their hex, for example &lt; would be encoded as `\u003C`.</span></span>

5. <span data-ttu-id="197a1-120">Antes de poner los datos que no sean de confianza en el texto de una consulta en una dirección URL asegúrese de que esté codificado como URL.</span><span class="sxs-lookup"><span data-stu-id="197a1-120">Before putting untrusted data into a URL query string ensure it's URL encoded.</span></span>

## <a name="html-encoding-using-razor"></a><span data-ttu-id="197a1-121">Codificación HTML mediante Razor</span><span class="sxs-lookup"><span data-stu-id="197a1-121">HTML Encoding using Razor</span></span>

<span data-ttu-id="197a1-122">El motor de Razor usado en MVC codifica automáticamente todas las salidas que proceden de las variables, a menos que te esfuerces para evitarlo.</span><span class="sxs-lookup"><span data-stu-id="197a1-122">The Razor engine used in MVC automatically encodes all output sourced from variables, unless you work really hard to prevent it doing so.</span></span> <span data-ttu-id="197a1-123">Usa reglas de codificación de atributos HTML siempre que se usa la directiva *@* .</span><span class="sxs-lookup"><span data-stu-id="197a1-123">It uses HTML attribute encoding rules whenever you use the *@* directive.</span></span> <span data-ttu-id="197a1-124">Como la codificación HTML de atributos es una mejora de la codificación HTML significa que no tiene que preocuparse de si debe utilizar la codificación HTML o la codificación de atributos.</span><span class="sxs-lookup"><span data-stu-id="197a1-124">As HTML attribute encoding is a superset of HTML encoding this means you don't have to concern yourself with whether you should use HTML encoding or HTML attribute encoding.</span></span> <span data-ttu-id="197a1-125">Debe asegurarse de que utiliza *@* solo en un contexto HTML, no cuando intente insertar una entrada que no es de confianza en JavaScript.</span><span class="sxs-lookup"><span data-stu-id="197a1-125">You must ensure that you only use @ in an HTML context, not when attempting to insert untrusted input directly into JavaScript.</span></span> <span data-ttu-id="197a1-126">Algunas etiquetas auxiliares también codificarán la entrada que se usa en los parámetros de otras etiquetas.</span><span class="sxs-lookup"><span data-stu-id="197a1-126">Tag helpers will also encode input you use in tag parameters.</span></span>

<span data-ttu-id="197a1-127">Teniendo la siguiente vista de Razor:</span><span class="sxs-lookup"><span data-stu-id="197a1-127">Take the following Razor view:</span></span>

```cshtml
@{
       var untrustedInput = "<\"123\">";
   }

   @untrustedInput
   ```

<span data-ttu-id="197a1-128">Esta vista genera el contenido de la variable *untrustedInput* .</span><span class="sxs-lookup"><span data-stu-id="197a1-128">This view outputs the contents of the *untrustedInput* variable.</span></span> <span data-ttu-id="197a1-129">Esta variable incluye algunos caracteres que se usan en ataques XSS, es decir, &lt;, y &gt;.</span><span class="sxs-lookup"><span data-stu-id="197a1-129">This variable includes some characters which are used in XSS attacks, namely &lt;, " and &gt;.</span></span> <span data-ttu-id="197a1-130">Examinando el origen, muestra la salida codificada y representada como:</span><span class="sxs-lookup"><span data-stu-id="197a1-130">Examining the source shows the rendered output encoded as:</span></span>

```html
&lt;&quot;123&quot;&gt;
   ```

>[!WARNING]
> <span data-ttu-id="197a1-131">ASP.NET Core MVC proporciona una clase `HtmlString` que no se codifica automáticamente en la salida.</span><span class="sxs-lookup"><span data-stu-id="197a1-131">ASP.NET Core MVC provides an `HtmlString` class which isn't automatically encoded upon output.</span></span> <span data-ttu-id="197a1-132">Esto nunca debe utilizarse en combinación con entradas no seguras ya que esto expondrá una vulnerabilidad XSS.</span><span class="sxs-lookup"><span data-stu-id="197a1-132">This should never be used in combination with untrusted input as this will expose an XSS vulnerability.</span></span>

## <a name="javascript-encoding-using-razor"></a><span data-ttu-id="197a1-133">Codificación de JavaScript con Razor</span><span class="sxs-lookup"><span data-stu-id="197a1-133">JavaScript Encoding using Razor</span></span>

<span data-ttu-id="197a1-134">Puede haber ocasiones en las que desee insertar un valor en JavaScript para procesarlo en la vista.</span><span class="sxs-lookup"><span data-stu-id="197a1-134">There may be times you want to insert a value into JavaScript to process in your view.</span></span> <span data-ttu-id="197a1-135">Existen dos formas de hacerlo.</span><span class="sxs-lookup"><span data-stu-id="197a1-135">There are two ways to do this.</span></span> <span data-ttu-id="197a1-136">La forma más segura de insertar valores es colocar el valor en un atributo de datos de una etiqueta y recuperarlo en el código JavaScript.</span><span class="sxs-lookup"><span data-stu-id="197a1-136">The safest way to insert values is to place the value in a data attribute of a tag and retrieve it in your JavaScript.</span></span> <span data-ttu-id="197a1-137">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="197a1-137">For example:</span></span>

```cshtml
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

<span data-ttu-id="197a1-138">Esto generará el siguiente código HTML</span><span class="sxs-lookup"><span data-stu-id="197a1-138">This will produce the following HTML</span></span>

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

<span data-ttu-id="197a1-139">Que, cuando se ejecuta, presentará lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="197a1-139">Which, when it runs, will render the following:</span></span>

```
<"123">
   <"123">
```

<span data-ttu-id="197a1-140">También puede llamar al codificador de JavaScript directamente:</span><span class="sxs-lookup"><span data-stu-id="197a1-140">You can also call the JavaScript encoder directly:</span></span>

```cshtml
@using System.Text.Encodings.Web;
   @inject JavaScriptEncoder encoder;

   @{
       var untrustedInput = "<\"123\">";
   }

   <script>
       document.write("@encoder.Encode(untrustedInput)");
   </script>
```

<span data-ttu-id="197a1-141">Esto se representará en el explorador de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="197a1-141">This will render in the browser as follows:</span></span>

```html
<script>
    document.write("\u003C\u0022123\u0022\u003E");
</script>
```

>[!WARNING]
> <span data-ttu-id="197a1-142">No concatene la entrada de desconfianza en el código JavaScript para crear los elementos DOM.</span><span class="sxs-lookup"><span data-stu-id="197a1-142">Don't concatenate untrusted input in JavaScript to create DOM elements.</span></span> <span data-ttu-id="197a1-143">Debe usar `createElement()` y asignar los valores de propiedad de forma adecuada, como `node.TextContent=`, o usar `element.SetAttribute()`/`element[attribute]=` de lo contrario, se expone a sí mismo a XSS basado en DOM.</span><span class="sxs-lookup"><span data-stu-id="197a1-143">You should use `createElement()` and assign property values appropriately such as `node.TextContent=`, or use `element.SetAttribute()`/`element[attribute]=` otherwise you expose yourself to DOM-based XSS.</span></span>

## <a name="accessing-encoders-in-code"></a><span data-ttu-id="197a1-144">Obtener acceso a codificadores en el código</span><span class="sxs-lookup"><span data-stu-id="197a1-144">Accessing encoders in code</span></span>

<span data-ttu-id="197a1-145">Los codificadores HTML, JavaScript y URL están disponibles para el código de dos maneras: puede inyectarlos a través de la [inserción de dependencias](xref:fundamentals/dependency-injection) o puede usar los codificadores predeterminados contenidos en el espacio de nombres `System.Text.Encodings.Web`.</span><span class="sxs-lookup"><span data-stu-id="197a1-145">The HTML, JavaScript and URL encoders are available to your code in two ways, you can inject them via [dependency injection](xref:fundamentals/dependency-injection) or you can use the default encoders contained in the `System.Text.Encodings.Web` namespace.</span></span> <span data-ttu-id="197a1-146">Si usa los codificadores de forma predeterminada hará que todos los caracteres que se traten como seguros no surjan efecto: los codificadores predeterminados usan las reglas de codificación más seguras posibles.</span><span class="sxs-lookup"><span data-stu-id="197a1-146">If you use the default encoders then any  you applied to character ranges to be treated as safe won't take effect - the default encoders use the safest encoding rules possible.</span></span>

<span data-ttu-id="197a1-147">Para usar los codificadores configurables a través de DI, los constructores deben tomar un parámetro *HtmlEncoder*, *JavaScriptEncoder* y *UrlEncoder* , según corresponda.</span><span class="sxs-lookup"><span data-stu-id="197a1-147">To use the configurable encoders via DI your constructors should take an *HtmlEncoder*, *JavaScriptEncoder* and *UrlEncoder* parameter as appropriate.</span></span> <span data-ttu-id="197a1-148">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="197a1-148">For example;</span></span>

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

## <a name="encoding-url-parameters"></a><span data-ttu-id="197a1-149">Parámetros de URL de codificación</span><span class="sxs-lookup"><span data-stu-id="197a1-149">Encoding URL Parameters</span></span>

<span data-ttu-id="197a1-150">Si desea crear una cadena de consulta de dirección URL con una entrada que no sea de confianza como valor, use el `UrlEncoder` para codificar el valor.</span><span class="sxs-lookup"><span data-stu-id="197a1-150">If you want to build a URL query string with untrusted input as a value use the `UrlEncoder` to encode the value.</span></span> <span data-ttu-id="197a1-151">Por ejemplo,</span><span class="sxs-lookup"><span data-stu-id="197a1-151">For example,</span></span>

```csharp
var example = "\"Quoted Value with spaces and &\"";
   var encodedValue = _urlEncoder.Encode(example);
   ```

<span data-ttu-id="197a1-152">Después de codificar, la variable encodedValue contendrá `%22Quoted%20Value%20with%20spaces%20and%20%26%22`.</span><span class="sxs-lookup"><span data-stu-id="197a1-152">After encoding the encodedValue variable will contain `%22Quoted%20Value%20with%20spaces%20and%20%26%22`.</span></span> <span data-ttu-id="197a1-153">Los espacios, las comillas, signos de puntuación y otros caracteres no seguros serán codificados en su valor hexadecimal, por ejemplo un carácter de espacio se convertirá en % 20.</span><span class="sxs-lookup"><span data-stu-id="197a1-153">Spaces, quotes, punctuation and other unsafe characters will be percent encoded to their hexadecimal value, for example a space character will become %20.</span></span>

>[!WARNING]
> <span data-ttu-id="197a1-154">No use datos que no son de confianza como parte de una consulta en una URL.</span><span class="sxs-lookup"><span data-stu-id="197a1-154">Don't use untrusted input as part of a URL path.</span></span> <span data-ttu-id="197a1-155">Pase siempre los datos que no son de confianza como una cadena de texto.</span><span class="sxs-lookup"><span data-stu-id="197a1-155">Always pass untrusted input as a query string value.</span></span>

<a name="security-cross-site-scripting-customization"></a>

## <a name="customizing-the-encoders"></a><span data-ttu-id="197a1-156">Personalización de los codificadores</span><span class="sxs-lookup"><span data-stu-id="197a1-156">Customizing the Encoders</span></span>

<span data-ttu-id="197a1-157">De forma predeterminada, los codificadores utilizan una lista segura limitada en el rango Unicode del Latín básico y codifican todos los caracteres fuera de ese intervalo como sus equivalentes.</span><span class="sxs-lookup"><span data-stu-id="197a1-157">By default encoders use a safe list limited to the Basic Latin Unicode range and encode all characters outside of that range as their character code equivalents.</span></span> <span data-ttu-id="197a1-158">Este comportamiento también afecta a la representación de TagHelper Razor y HtmlHelper que utilizarán los codificadores para las cadenas de salida.</span><span class="sxs-lookup"><span data-stu-id="197a1-158">This behavior also affects Razor TagHelper and HtmlHelper rendering as it will use the encoders to output your strings.</span></span>

<span data-ttu-id="197a1-159">El razonamiento sirve como protección ante posibles errores desconocidos o que puedan suceder en un futuro (se han observado anteriores errores de exploradores al confundir el análisis basado en el procesamiento de caracteres no válidos en caracteres no ingleses).</span><span class="sxs-lookup"><span data-stu-id="197a1-159">The reasoning behind this is to protect against unknown or future browser bugs (previous browser bugs have tripped up parsing based on the processing of non-English characters).</span></span> <span data-ttu-id="197a1-160">Si su sitio web hace un uso intensivo de los caracteres no latinos, como el chino, cirílico u otros probablemente no es el comportamiento que desee.</span><span class="sxs-lookup"><span data-stu-id="197a1-160">If your web site makes heavy use of non-Latin characters, such as Chinese, Cyrillic or others this is probably not the behavior you want.</span></span>

<span data-ttu-id="197a1-161">Puede personalizar las listas seguras del codificador para incluir los intervalos Unicode adecuados para la aplicación durante el inicio, en `ConfigureServices()`.</span><span class="sxs-lookup"><span data-stu-id="197a1-161">You can customize the encoder safe lists to include Unicode ranges appropriate to your application during startup, in `ConfigureServices()`.</span></span>

<span data-ttu-id="197a1-162">Por ejemplo, si usa la configuración predeterminada, puede usar una de Razor, como la siguiente.</span><span class="sxs-lookup"><span data-stu-id="197a1-162">For example, using the default configuration you might use a Razor HtmlHelper like so;</span></span>

```html
<p>This link text is in Chinese: @Html.ActionLink("汉语/漢語", "Index")</p>
   ```

<span data-ttu-id="197a1-163">Al ver el origen de la página web, verá que se ha representado de la siguiente forma, con el texto en chino codificado;</span><span class="sxs-lookup"><span data-stu-id="197a1-163">When you view the source of the web page you will see it has been rendered as follows, with the Chinese text encoded;</span></span>

```html
<p>This link text is in Chinese: <a href="/">&#x6C49;&#x8BED;/&#x6F22;&#x8A9E;</a></p>
   ```

<span data-ttu-id="197a1-164">Para ampliar los caracteres que el codificador trata como seguros, debe insertar la siguiente línea en el método `ConfigureServices()` de `startup.cs`;</span><span class="sxs-lookup"><span data-stu-id="197a1-164">To widen the characters treated as safe by the encoder you would insert the following line into the `ConfigureServices()` method in `startup.cs`;</span></span>

```csharp
services.AddSingleton<HtmlEncoder>(
     HtmlEncoder.Create(allowedRanges: new[] { UnicodeRanges.BasicLatin,
                                               UnicodeRanges.CjkUnifiedIdeographs }));
   ```

<span data-ttu-id="197a1-165">En este ejemplo se amplía la lista segura para que incluya el intervalo Unicode CjkUnifiedIdeographs.</span><span class="sxs-lookup"><span data-stu-id="197a1-165">This example widens the safe list to include the Unicode Range CjkUnifiedIdeographs.</span></span> <span data-ttu-id="197a1-166">Su salida se convertirá a:</span><span class="sxs-lookup"><span data-stu-id="197a1-166">The rendered output would now become</span></span>

```html
<p>This link text is in Chinese: <a href="/">汉语/漢語</a></p>
   ```

<span data-ttu-id="197a1-167">Los intervalos de la lista segura se especifican como gráficos de códigos Unicode, no como lenguajes.</span><span class="sxs-lookup"><span data-stu-id="197a1-167">Safe list ranges are specified as Unicode code charts, not languages.</span></span> <span data-ttu-id="197a1-168">El [estándar Unicode](https://unicode.org/) tiene una lista de [gráficos de código](https://www.unicode.org/charts/index.html) que puede usar para buscar el gráfico que contiene los caracteres.</span><span class="sxs-lookup"><span data-stu-id="197a1-168">The [Unicode standard](https://unicode.org/) has a list of [code charts](https://www.unicode.org/charts/index.html) you can use to find the chart containing your characters.</span></span> <span data-ttu-id="197a1-169">Cada codificador, HTML, JavaScript y URL, debe configurarse por separado.</span><span class="sxs-lookup"><span data-stu-id="197a1-169">Each encoder, Html, JavaScript and Url, must be configured separately.</span></span>

> [!NOTE]
> <span data-ttu-id="197a1-170">La personalización de la lista segura solo afecta a los codificadores de código abiertos a través de DI.</span><span class="sxs-lookup"><span data-stu-id="197a1-170">Customization of the safe list only affects encoders sourced via DI.</span></span> <span data-ttu-id="197a1-171">Si obtiene acceso directamente a un codificador a través de `System.Text.Encodings.Web.*Encoder.Default`, se usará la configuración de la base de datos de solo latín básica básica.</span><span class="sxs-lookup"><span data-stu-id="197a1-171">If you directly access an encoder via `System.Text.Encodings.Web.*Encoder.Default` then the default, Basic Latin only safelist will be used.</span></span>

## <a name="where-should-encoding-take-place"></a><span data-ttu-id="197a1-172">¿Dónde se debe realizar la codificación?</span><span class="sxs-lookup"><span data-stu-id="197a1-172">Where should encoding take place?</span></span>

<span data-ttu-id="197a1-173">Una buena practica aceptada es la que se lleva a cabo en la salida de la codificación, los valores codificados nunca deben almacenarse en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="197a1-173">The general accepted practice is that encoding takes place at the point of output and encoded values should never be stored in a database.</span></span> <span data-ttu-id="197a1-174">La codificación en el punto de salida permite cambiar el uso de datos, por ejemplo, de HTML a un valor de una consulta.</span><span class="sxs-lookup"><span data-stu-id="197a1-174">Encoding at the point of output allows you to change the use of data, for example, from HTML to a query string value.</span></span> <span data-ttu-id="197a1-175">También permite realizar búsquedas fácilmente en los datos sin tener que codificar valores antes de realizar búsquedas y permite aprovechar las ventajas de los cambios o las correcciones de errores que se realicen en los codificadores.</span><span class="sxs-lookup"><span data-stu-id="197a1-175">It also enables you to easily search your data without having to encode values before searching and allows you to take advantage of any changes or bug fixes made to encoders.</span></span>

## <a name="validation-as-an-xss-prevention-technique"></a><span data-ttu-id="197a1-176">Validación como técnica de prevención de XSS</span><span class="sxs-lookup"><span data-stu-id="197a1-176">Validation as an XSS prevention technique</span></span>

<span data-ttu-id="197a1-177">La validación puede ser una herramienta útil para limitar los ataques XSS.</span><span class="sxs-lookup"><span data-stu-id="197a1-177">Validation can be a useful tool in limiting XSS attacks.</span></span> <span data-ttu-id="197a1-178">Por ejemplo, una cadena numérica que solo contenga los caracteres 0-9 no desencadenará un ataque XSS.</span><span class="sxs-lookup"><span data-stu-id="197a1-178">For example, a numeric string containing only the characters 0-9 won't trigger an XSS attack.</span></span> <span data-ttu-id="197a1-179">La validación pasa a ser más complicada al aceptar código HTML en la entrada del usuario.</span><span class="sxs-lookup"><span data-stu-id="197a1-179">Validation becomes more complicated when accepting HTML in user input.</span></span> <span data-ttu-id="197a1-180">El análisis de la entrada HTML es difícil, si no imposible.</span><span class="sxs-lookup"><span data-stu-id="197a1-180">Parsing HTML input is difficult, if not impossible.</span></span> <span data-ttu-id="197a1-181">Markdown permite junto con un analizador eliminar los HTML incrustados, es una opción más segura para aceptar la entrada de texto enriquecido.</span><span class="sxs-lookup"><span data-stu-id="197a1-181">Markdown, coupled with a parser that strips embedded HTML, is a safer option for accepting rich input.</span></span> <span data-ttu-id="197a1-182">Nunca hay que basarse únicamente en la validación.</span><span class="sxs-lookup"><span data-stu-id="197a1-182">Never rely on validation alone.</span></span> <span data-ttu-id="197a1-183">Es necesario codificar siempre la entrada no confiable antes de la salida, independientemente de qué validación o comprobación de estado se ha realizado.</span><span class="sxs-lookup"><span data-stu-id="197a1-183">Always encode untrusted input before output, no matter what validation or sanitization has been performed.</span></span>
