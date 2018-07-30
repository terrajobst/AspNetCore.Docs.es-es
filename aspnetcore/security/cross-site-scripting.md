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
# <a name="prevent-cross-site-scripting-xss-in-aspnet-core"></a>Evitar Cross-Site Scripting (XSS) en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Cross-Site Scripting (XSS) es una vulnerabilidad de seguridad que permite que un atacante colocar los scripts de cliente (normalmente JavaScript) en las páginas web. Cuando otros usuarios cargar páginas afectadas se ejecutarán las secuencias de comandos de los atacantes, lo que podría robar cookies y tokens de sesión, cambiar el contenido de la página web a través de la manipulación del DOM o redirigir el explorador a otra página. Las vulnerabilidades XSS suelen producen cuando una aplicación toma la entrada del usuario y lo envía en una página sin validación, codificación o secuencias de escape.

## <a name="protecting-your-application-against-xss"></a>Proteger la aplicación frente a XSS

En un XSS de nivel básico funciona engañar a la aplicación en la inserción de un `<script>` etiqueta en la página representada o insertando un `On*` eventos en un elemento. Los desarrolladores deben usar los siguientes pasos de prevención para evitar introducir XSS en su aplicación.

1. Nunca Coloque datos que no se confía en la entrada HTML, a menos que siga el resto de los pasos siguientes. Datos de confianza están cualquier dato que puede estar controlada por un atacante, entradas de formulario HTML, las cadenas de consulta, encabezados HTTP, incluso los datos proceden de una base de datos como un atacante puede infringir la base de datos aunque no pueden infringir la aplicación.

2. Asegúrese de que está codificado en HTML antes de poner los datos de confianza dentro de un elemento HTML. Codificación HTML toma como caracteres &lt; y se modifican en un formulario seguro como &amp;lt;

3. Antes de poner los datos que no se confía en un atributo HTML Asegúrese de que es el atributo HTML codificado. Codificación del atributo HTML es un superconjunto de codificación HTML y codifica los caracteres adicionales, como "y".

4. Antes de poner los datos que no se confía en JavaScript, coloque los datos en un elemento HTML cuyo contenido se recupera en tiempo de ejecución. Si esto no es posible, asegúrese de los datos se codifica JavaScript. Codificación de JavaScript toma caracteres peligrosos para JavaScript y se reemplaza con su hexadecimal, por ejemplo &lt; podría codificarse como `\u003C`.

5. Antes de poner los datos que no se confía en una cadena de consulta de dirección URL Asegúrese de que está codificado como URL.

## <a name="html-encoding-using-razor"></a>Codificación HTML con Razor

El motor de Razor uso automáticamente en MVC codifica todos salida proceden de las variables, a menos que se trabaja muy duro para evitar que hacerlo. Usa el atributo HTML las reglas de codificación, siempre que se use la *@* directiva. Como HTML la codificación del atributo es un superconjunto de codificación HTML, que esto significa que no tiene que preocuparse de si debe utilizar la codificación HTML o la codificación del atributo HTML. Debe asegurarse de que utiliza solo en un contexto HTML, no cuando se intenta insertar una entrada que no se confía directamente en JavaScript. Aplicaciones auxiliares de etiquetas también codificarán la entrada que se usa en los parámetros tag.

Realizar la siguiente vista de Razor;

```none
@{
       var untrustedInput = "<\"123\">";
   }

   @untrustedInput
   ```

Esta vista muestra el contenido de la *untrustedInput* variable. Esta variable incluye algunos caracteres que se usan en los ataques XSS, es decir, &lt;, "y &gt;. Examinando el origen, muestra la salida representada codificada como:

```html
&lt;&quot;123&quot;&gt;
   ```

>[!WARNING]
> ASP.NET Core MVC proporciona un `HtmlString` clase que no se codifica de forma automática tras la salida. Esto nunca debe utilizarse en combinación con entradas no seguras como esto expondrá una vulnerabilidad XSS.

## <a name="javascript-encoding-using-razor"></a>Codificación de JavaScript usa Razor

Puede haber ocasiones en que desea insertar un valor en JavaScript para procesar en la vista. Hay dos formas de hacerlo. La forma más segura para insertar valores es colocar el valor en un atributo de datos de una etiqueta y recuperarlo en el código JavaScript. Por ejemplo:

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

Esto generará el siguiente código HTML

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

Que, cuando se ejecuta, representarán lo siguiente:

```none
<"123">
   <"123">
   ```

También se puede llamar directamente, el codificador de JavaScript

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

Esto se representará en el explorador lo siguiente:

```html
<script>
       document.write("\u003C\u0022123\u0022\u003E");
   </script>
   ```

>[!WARNING]
> No concatene la entrada que no se confía en JavaScript para crear los elementos DOM. Debe usar `createElement()` y asignar valores de propiedad de forma adecuada, como `node.TextContent=`, o use `element.SetAttribute()` / `element[attribute]=` en caso contrario, se expone a basado en DOM XSS.

## <a name="accessing-encoders-in-code"></a>Obtener acceso a los codificadores en código

Los codificadores HTML, JavaScript y URL están disponibles en el código de dos maneras, insértelos a través de [inserción de dependencias](xref:fundamentals/dependency-injection) o puede usar los codificadores predeterminados incluidos en el `System.Text.Encodings.Web` espacio de nombres. Si usa los codificadores de forma predeterminada, a continuación, aplicado a los intervalos de caracteres se traten como seguros no surtirán efecto: los codificadores predeterminada use las reglas de codificación más seguras posible.

Para usar los codificadores configurables a través de DI sus constructores tardará un *HtmlEncoder*, *JavaScriptEncoder* y *UrlEncoder* parámetro según corresponda. Por ejemplo:

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

## <a name="encoding-url-parameters"></a>Codificación de parámetros de URL

Si desea compilar una cadena de consulta de dirección URL con entradas no seguras como un valor, use la `UrlEncoder` para codificar el valor. Por ejemplo,

```csharp
var example = "\"Quoted Value with spaces and &\"";
   var encodedValue = _urlEncoder.Encode(example);
   ```

Después de la codificación del encodedValue variable contendrá `%22Quoted%20Value%20with%20spaces%20and%20%26%22`. Espacios, las comillas, signos de puntuación y otros caracteres no seguros será porcentaje codificado en su valor hexadecimal, por ejemplo un carácter de espacio se convertirá en % 20.

>[!WARNING]
> No use la confianza de entrada como parte de una ruta de acceso de dirección URL. Pasar siempre la confianza de entrada como un valor de cadena de consulta.

<a name="security-cross-site-scripting-customization"></a>

## <a name="customizing-the-encoders"></a>Personalización de los codificadores

De forma predeterminada, los codificadores utiliza una lista segura limitada al intervalo Unicode Latín básico y codifican todos los caracteres fuera de ese intervalo como sus equivalentes de código de carácter. Este comportamiento también afecta a la representación de TagHelper Razor y HtmlHelper como utilizarán los codificadores para las cadenas de salida.

El razonamiento sirve como protección ante errores desconocidos o futuros explorador (errores de explorador anterior han confundiré análisis basado en el procesamiento de caracteres no válidos). Si su sitio web hace un uso intensivo de los caracteres no latinos, como el chino, cirílico u otros Esto probablemente no es el comportamiento que desee.

Puede personalizar las listas seguras de codificador para incluir Unicode intervalos adecuados a la aplicación durante el inicio, en `ConfigureServices()`.

Por ejemplo, mediante la configuración predeterminada podría usar un HtmlHelper Razor de este modo;

```html
<p>This link text is in Chinese: @Html.ActionLink("汉语/漢語", "Index")</p>
   ```

Al ver el origen de la página web, verá que se ha representado como sigue, con el texto en chino codificado;

```html
<p>This link text is in Chinese: <a href="/">&#x6C49;&#x8BED;/&#x6F22;&#x8A9E;</a></p>
   ```

Para ampliar los caracteres que se trata como seguro el codificador se insertaría la línea siguiente en el `ConfigureServices()` método `startup.cs`;

```csharp
services.AddSingleton<HtmlEncoder>(
     HtmlEncoder.Create(allowedRanges: new[] { UnicodeRanges.BasicLatin,
                                               UnicodeRanges.CjkUnifiedIdeographs }));
   ```

En este ejemplo se amplía la lista segura para que incluya el CjkUnifiedIdeographs intervalo Unicode. Ahora se convertiría en la salida representada

```html
<p>This link text is in Chinese: <a href="/">汉语/漢語</a></p>
   ```

Los intervalos de la lista segura se especifican como gráficos de código Unicode, no los idiomas. El [estándar Unicode](http://unicode.org/) tiene una lista de [gráficos de código](http://www.unicode.org/charts/index.html) puede usar para buscar el gráfico que contiene los caracteres. Cada codificador, Html, JavaScript y dirección Url, debe configurarse por separado.

> [!NOTE]
> Personalización de la lista segura solo afecta a los codificadores de código abiertos a través de DI. Si tiene acceso directamente a un codificador a través de `System.Text.Encodings.Web.*Encoder.Default` , a continuación, el valor predeterminado, Latín básico solo safelist se usará.

## <a name="where-should-encoding-take-place"></a>¿Dónde debe colocar tomar codificación?

General acepta práctica es que lleva a cabo en el punto de salida de codificación y los valores codificados nunca deben almacenarse en una base de datos. Codificación en el punto de salida permite cambiar el uso de datos, por ejemplo, de HTML a un valor de cadena de consulta. También permite buscar fácilmente los datos sin tener que codificar valores antes de buscar y le permite aprovechar las ventajas de los cambios o correcciones de errores realizadas en los codificadores.

## <a name="validation-as-an-xss-prevention-technique"></a>Validación como una técnica de prevención de XSS

Validación puede ser una herramienta útil para limitar los ataques XSS. Por ejemplo, un tipo numérico string que contiene los caracteres 0-9 no desencadenará un ataque XSS. Validación se complica si quiere aceptar HTML en la entrada de usuario - análisis de la entrada HTML es difícil, si no imposible. MarkDown y otros formatos de texto sería una opción más segura para la entrada enriquecido. Nunca debe depender únicamente de validación. Codificar entrada confianza antes de la salida de siempre, independientemente de qué validación ha realizado.
