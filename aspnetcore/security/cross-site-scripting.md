---
title: Evitar Cross-Site Scripting (XSS) en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre Cross-Site Scripting (XSS) y las técnicas para solucionar esta vulnerabilidad en una aplicación ASP.NET Core.
ms.author: riande
ms.date: 10/02/2018
uid: security/cross-site-scripting
ms.openlocfilehash: 50f0211a2c64708d9b788dd10ce9064e66014d55
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910530"
---
# <a name="prevent-cross-site-scripting-xss-in-aspnet-core"></a>Evitar Cross-Site Scripting (XSS) en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Cross-Site Scripting (XSS) es una vulnerabilidad de seguridad que permite que un atacante colocar los scripts de cliente (normalmente JavaScript) en las páginas web. Cuando otros usuarios cargar páginas afectadas se ejecutarán las secuencias de comandos del atacante, lo que podría robar cookies y tokens de sesión, cambiar el contenido de la página web a través de la manipulación del DOM o redirigir el explorador a otra página. Las vulnerabilidades XSS suelen producen cuando una aplicación toma la entrada del usuario y lo envía a una página sin validación, codificación o secuencias de escape.

## <a name="protecting-your-application-against-xss"></a>Proteger la aplicación frente a XSS

Un XSS de nivel básico consiste en engañar a la aplicación con la inserción de una etiqueta `<script>` en la página renderizada o insertando eventos `On*` en un elemento. Los desarrolladores deben seguir los siguientes pasos para evitar introducir una vulnerabilidad XSS en su aplicación.

1. Nunca coloque datos que no sean de confianza en su código html, a menos que siga el resto de los pasos. Los datos que no son de confianza son cualquier dato que puede estar controlado por un atacante, entradas de formulario HTML, textos de consultas, cabeceras HTTP, incluso los datos que proceden de una base de datos donde un atacante puede encontrar una vulnerabilidad sin pasar por la aplicación.

2. Asegúrese de que el contenido HTML de confianza esté codificado antes de ponerlo dentro de un elemento HTML. La codificación HTML toma caracteres como &lt; y lo modifican a algo seguro como &amp;lt;

3. Asegúrese de que está codificado en HTML antes de poner los datos que no se confía en un atributo HTML. Codificación del atributo HTML es un superconjunto de codificación HTML y codifica los caracteres adicionales, como "y".

4. Antes de poner los datos que no se confía en JavaScript, coloque los datos en un elemento HTML cuyo contenido se recupera en tiempo de ejecución. Si esto no es posible, asegúrese de que los datos están codificado de JavaScript. Codificación de JavaScript toma caracteres peligrosos para JavaScript y se reemplaza con su hexadecimal, por ejemplo &lt; podría codificarse como `\u003C`.

5. Antes de poner los datos que no sean de confianza en el texto de una consulta en una dirección URL asegúrese de que esté codificado como URL.

## <a name="html-encoding-using-razor"></a>Codificación HTML con Razor

El motor de Razor uso automáticamente en MVC codifica todos salida proceden de las variables, a menos que se trabaja muy duro para evitar que hacerlo. Usa reglas de codificación de atributo HTML siempre que se use la *@* directiva. Como HTML la codificación del atributo es un superconjunto de codificación HTML, que esto significa que no tiene que preocuparse de si debe utilizar la codificación HTML o la codificación del atributo HTML. Debe asegurarse de que utiliza solo en un contexto HTML, no cuando se intenta insertar una entrada que no se confía directamente en JavaScript. Aplicaciones auxiliares de etiquetas también codificarán la entrada que se usa en los parámetros tag.

Teniendo la siguiente vista de Razor:

```cshtml
@{
       var untrustedInput = "<\"123\">";
   }

   @untrustedInput
   ```

Esta vista muestra el contenido de la variable *untrustedInput* . Esta variable incluye algunos caracteres que se usan en los ataques XSS, es decir, &lt;, "y &gt;. Examinando el origen, muestra la salida codificada y representada como:

```html
&lt;&quot;123&quot;&gt;
   ```

>[!WARNING]
> ASP.NET Core MVC proporciona una clase `HtmlString` que no se codifica de forma automática tras la salida. Esto nunca debe utilizarse en combinación con entradas no seguras ya que esto expondrá una vulnerabilidad XSS.

## <a name="javascript-encoding-using-razor"></a>Codificación de JavaScript usa Razor

Puede haber ocasiones en que desea insertar un valor en JavaScript para procesar en la vista. Hay dos formas de hacerlo. La forma más segura para insertar valores es colocar el valor en un atributo de datos de una etiqueta y recuperarlo en el código JavaScript. Por ejemplo:

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

Que, cuando se ejecuta, representará el texto siguiente:

```none
<"123">
   <"123">
   ```

También puede llamar directamente el codificador de JavaScript:

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

Esto se representará en el explorador como sigue:

```html
<script>
       document.write("\u003C\u0022123\u0022\u003E");
   </script>
   ```

>[!WARNING]
> No concatene la entrada que no se confía en JavaScript para crear los elementos DOM. Debe usar `createElement()` y asignar valores de propiedad de forma adecuada, como `node.TextContent=`, o use `element.SetAttribute()` / `element[attribute]=` en caso contrario, se expone a basado en DOM XSS.

## <a name="accessing-encoders-in-code"></a>Obtener acceso a los codificadores en código

Los codificadores HTML, JavaScript y URL están disponibles en el código de dos maneras, insértelos a través de [inserción de dependencias](xref:fundamentals/dependency-injection) o puede usar los codificadores predeterminados incluidos en el espacio de nombres `System.Text.Encodings.Web`. Si usa los codificadores de forma predeterminada hará que todos los caracteres que se traten como seguros no surjan efecto: los codificadores predeterminados usan las reglas de codificación más seguras posibles.

Para usar los configuradores de codificadores a través del DI sus constructores cogerán un *HtmlEncoder*, *JavaScriptEncoder* y *UrlEncoder* como parámetro según corresponda. Por ejemplo:

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

Si desea construir una consulta de una dirección URL con entradas no seguras como un valor, use `UrlEncoder` para codificar el valor. Por ejemplo,

```csharp
var example = "\"Quoted Value with spaces and &\"";
   var encodedValue = _urlEncoder.Encode(example);
   ```

Después de la codificación la variable encodedValue contendrá `%22Quoted%20Value%20with%20spaces%20and%20%26%22`. Los espacios, las comillas, signos de puntuación y otros caracteres no seguros serán codificados en su valor hexadecimal, por ejemplo un carácter de espacio se convertirá en % 20.

>[!WARNING]
> No use datos que no son de confianza como parte de una consulta en una URL. Pase siempre los datos que no son de confianza como una cadena de texto.

<a name="security-cross-site-scripting-customization"></a>

## <a name="customizing-the-encoders"></a>Personalización de los codificadores

De forma predeterminada, los codificadores utilizan una lista segura limitada en el rango Unicode del Latín básico y codifican todos los caracteres fuera de ese intervalo como sus equivalentes. Este comportamiento también afecta a la representación de TagHelper Razor y HtmlHelper que utilizarán los codificadores para las cadenas de salida.

El razonamiento sirve como protección ante posibles errores desconocidos o que puedan suceder en un futuro (se han observado anteriores errores de exploradores al confundir el análisis basado en el procesamiento de caracteres no válidos en caracteres no ingleses). Si su sitio web hace un uso intensivo de los caracteres no latinos, como el chino, cirílico u otros probablemente no es el comportamiento que desee.

Puede personalizar las listas seguras del codificador para incluir intervalos Unicode adecuados a la aplicación durante el inicio, en `ConfigureServices()`.

Por ejemplo, mediante la configuración predeterminada podría usar un HtmlHelper Razor de este modo;

```html
<p>This link text is in Chinese: @Html.ActionLink("汉语/漢語", "Index")</p>
   ```

Al ver el origen de la página web, verá que se ha representado de la siguiente forma, con el texto en chino codificado;

```html
<p>This link text is in Chinese: <a href="/">&#x6C49;&#x8BED;/&#x6F22;&#x8A9E;</a></p>
   ```

Para ampliar los caracteres que se interpretan como seguro el codificador se insertaría la línea siguiente en el `ConfigureServices()` método `startup.cs`;

```csharp
services.AddSingleton<HtmlEncoder>(
     HtmlEncoder.Create(allowedRanges: new[] { UnicodeRanges.BasicLatin,
                                               UnicodeRanges.CjkUnifiedIdeographs }));
   ```

En este ejemplo se amplía la lista segura para que incluya el intervalo Unicode CjkUnifiedIdeographs. Su salida se convertirá a:

```html
<p>This link text is in Chinese: <a href="/">汉语/漢語</a></p>
   ```

Los intervalos de la lista segura se especifican como gráficos de código Unicode, no los idiomas. El [estándar Unicode](http://unicode.org/) tiene una lista de [gráficos de código](http://www.unicode.org/charts/index.html) puede usar para buscar el gráfico que contiene los caracteres. Cada codificador, Html, JavaScript y dirección Url, debe configurarse por separado.

> [!NOTE]
> La personalización de la lista segura solo afecta a los codificadores de código abiertos a través de DI. Si tiene acceso directamente a un codificador a través de `System.Text.Encodings.Web.*Encoder.Default` , a continuación, el valor predeterminado, Latín básico solo safelist se usará.

## <a name="where-should-encoding-take-place"></a>¿Dónde debe colocar tomar codificación?

Una buena practica aceptada es la que se lleva a cabo en la salida de la codificación, los valores codificados nunca deben almacenarse en una base de datos. La codificación en el punto de salida permite cambiar el uso de datos, por ejemplo, de HTML a un valor de una consulta. También permite buscar fácilmente los datos sin tener que codificar valores antes de buscar y le permite aprovechar las ventajas de los cambios o correcciones de errores realizadas en los codificadores.

## <a name="validation-as-an-xss-prevention-technique"></a>Validación como una técnica de prevención de XSS

La validación puede ser una herramienta útil para limitar los ataques XSS. Por ejemplo, un tipo numérico string que contiene los caracteres 0-9 no desencadenará un ataque XSS. La validación pasa a ser más complicada al aceptar código HTML en la entrada del usuario. El análisis de la entrada HTML es difícil, si no imposible. Markdown permite junto con un analizador eliminar los HTML incrustados, es una opción más segura para aceptar la entrada de texto enriquecido. Nunca hay que basarse únicamente en la validación. Es necesario codificar siempre la entrada no confiable antes de la salida, independientemente de qué validación o comprobación de estado se ha realizado.
