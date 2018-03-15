---
title: Evitar que las secuencias de comandos (XSS) en ASP.NET Core
author: rick-anderson
description: "Obtenga información acerca de Scripting entre sitios (XSS) y las técnicas para tener acceso a esta vulnerabilidad en una aplicación de ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/cross-site-scripting
ms.openlocfilehash: 9e54ee0b1169c01629c3cd91a378509a73c53904
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/15/2018
---
# <a name="preventing-cross-site-scripting-xss-in-aspnet-core"></a>Evitar que las secuencias de comandos (XSS) en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Scripting entre sitios (XSS) son una vulnerabilidad de seguridad que permite a un atacante colocar scripts del lado cliente (normalmente JavaScript) en las páginas web. Cuando otros usuarios cargar páginas afectadas se ejecutarán las secuencias de comandos de los atacantes, lo que podría robar cookies y tokens de sesión, cambiar el contenido de la página web mediante la manipulación de DOM o redirigir el explorador a otra página. Vulnerabilidades XSS suelen producen cuando una aplicación toma la entrada del usuario y genera en una página sin validación, codificación o secuencias de escape.

## <a name="protecting-your-application-against-xss"></a>Protección de la aplicación en XSS

AT un XSS de nivel básico funciona engañar a su aplicación en Insertar un `<script>` etiqueta en la página representada, o insertando una `On*` eventos en un elemento. Los desarrolladores deben usar los siguientes pasos de prevención para evitar la introducción XSS en su aplicación.

1. Nunca Coloque datos que no se confía en la entrada HTML, a menos que siga el resto de los pasos siguientes. Datos no es de confianza están cualquier dato que puede controlarse mediante un atacante, entradas de formulario HTML, las cadenas de consulta, los encabezados HTTP, ni siquiera los datos proceden de una base de datos como un atacante puede infringir la base de datos aunque no pueden infringir la aplicación.

2. Antes de poner los datos no es de confianza dentro de un elemento HTML Asegúrese de que está codificado en HTML. Codificación de HTML tiene caracteres como &lt; y se modifican de forma segura como &amp;lt;

3. Antes de poner los datos que no se confía en un atributo HTML Asegúrese sea atributo HTML codificado. Codificación del atributo HTML es un supraconjunto de codificación HTML y codifica los caracteres adicionales como "y".

4. Antes de poner los datos que no se confía en JavaScript colocar los datos en un elemento HTML cuyo contenido recuperar en tiempo de ejecución. Si esto no es posible garantizar que los datos se codifica JavaScript. Codificación de JavaScript tiene caracteres peligrosos para JavaScript y los reemplaza con su hexadecimal, por ejemplo &lt; se codifica como `\u003C`.

5. Antes de poner los datos que no se confía en una cadena de consulta de dirección URL Asegúrese de que es la dirección URL codificada.

## <a name="html-encoding-using-razor"></a>Codificación HTML con Razor

El motor de Razor usado en MVC automáticamente codifica todos los resultados como origen de las variables, a menos que trabaje mucho para evitar que esto. Usa el atributo HTML reglas de codificación, siempre que se use la  *@*  directiva. Como HTML la codificación del atributo es un supraconjunto de codificación HTML, que esto significa que no tiene que preocuparse de si debe utilizar la codificación HTML o codificación del atributo HTML. Debe asegurarse de que utiliza solo en un contexto HTML, no al intentar insertar la entrada que no se confía directamente en JavaScript. Aplicaciones auxiliares de etiquetas codificará también usen en parámetros de la etiqueta de entrada.

Realizar la siguiente vista Razor;

```none
@{
       var untrustedInput = "<\"123\">";
   }

   @untrustedInput
   ```

Esta vista muestra el contenido de la *untrustedInput* variable. Esta variable incluye algunos caracteres que se usan en los ataques XSS, es decir, &lt;, "y &gt;. Examinar el código fuente, muestra el resultado representado codificado como:

```html
&lt;&quot;123&quot;&gt;
   ```

>[!WARNING]
> Núcleo ASP.NET MVC proporciona una `HtmlString` clase que no está codificado automáticamente tras la salida. Esto nunca debe utilizarse en combinación con la entrada no es de confianza como esto expondrá una vulnerabilidad XSS.

## <a name="javascript-encoding-using-razor"></a>Codificación de JavaScript con Razor

Puede haber ocasiones en que desea insertar un valor en JavaScript para procesar en la vista. Existen dos formas de lograr esto. La forma más segura para insertar valores simples es colocar el valor en un atributo de una etiqueta de datos y recuperarlos en el JavaScript. Por ejemplo:

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

Que, cuando se ejecuta, representarán lo siguiente;

```none
<"123">
   <"123">
   ```

También puede llamar directamente, el codificador de JavaScript

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

Esto se representará en el Explorador de manera;

```html
<script>
       document.write("\u003C\u0022123\u0022\u003E");
   </script>
   ```

>[!WARNING]
> No concatene que no se confía en JavaScript para crear elementos DOM. Debe usar `createElement()` y asignar valores de propiedad correctamente como `node.TextContent=`, o use `element.SetAttribute()` / `element[attribute]=` en caso contrario, se expone a DOM-based XSS.

## <a name="accessing-encoders-in-code"></a>Obtener acceso a los codificadores en código

Los codificadores HTML, JavaScript y URL están disponibles en el código de dos maneras, también puede insertar ellos a través de [inyección de dependencia](../fundamentals/dependency-injection.md#fundamentals-dependency-injection) o puede usar los codificadores predeterminados incluidos en el `System.Text.Encodings.Web` espacio de nombres. Si usas los codificadores de manera predeterminada, a continuación, aplicado a los intervalos de caracteres se traten como seguros no surtirán efecto: los codificadores predeterminado usen las reglas de codificación más seguras posible.

Para usar los codificadores configurables a través de DI deben tomar los constructores de una *HtmlEncoder*, *JavaScriptEncoder* y *UrlEncoder* parámetro según corresponda. Por ejemplo,

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

## <a name="encoding-url-parameters"></a>Parámetros de codificación de dirección URL

Si desea compilar una cadena de consulta de dirección URL con una entrada no es de confianza como un valor, use la `UrlEncoder` para codificar el valor. Por ejemplo,

```csharp
var example = "\"Quoted Value with spaces and &\"";
   var encodedValue = _urlEncoder.Encode(example);
   ```

Después de la codificación del encodedValue variable contendrá `%22Quoted%20Value%20with%20spaces%20and%20%26%22`. Espacios, comillas, signos de puntuación y otros caracteres no seguros se porcentaje codificados en su valor hexadecimal, por ejemplo un carácter de espacio se convertirá en % 20.

>[!WARNING]
> No use la entrada no es de confianza como parte de una ruta de acceso de dirección URL. Siempre pase datos proporcionados no es de confianza como un valor de cadena de consulta.

<a name="security-cross-site-scripting-customization"></a>

## <a name="customizing-the-encoders"></a>Personalizar los codificadores

De forma predeterminada codificadores utiliza una lista segura limitada en el intervalo de Unicode Latín básico y codifican todos los caracteres fuera de ese intervalo como sus equivalentes de código de carácter. Este comportamiento también afecta a la representación TagHelper Razor y HtmlHelper como utilizarán los codificadores a sus cadenas de salida.

El razonamiento sirve como protección ante errores desconocidos o futuros explorador (errores de explorador anterior se desencadena una análisis basado en el procesamiento de caracteres no válidos). Si el sitio web hace un uso intensivo de los caracteres no latinos, como el chino, cirílico u otros Esto probablemente no es el comportamiento que desee.

Puede personalizar las listas seguras de codificador para incluir Unicode intervalos apropiados para la aplicación durante el inicio, en `ConfigureServices()`.

Por ejemplo, mediante la configuración predeterminada que se puede usar un HtmlHelper Razor así;

```html
<p>This link text is in Chinese: @Html.ActionLink("汉语/漢語", "Index")</p>
   ```

Al ver el origen de la página web verá que se ha representado como sigue, con el texto en chino codificado;

```html
<p>This link text is in Chinese: <a href="/">&#x6C49;&#x8BED;/&#x6F22;&#x8A9E;</a></p>
   ```

Para ampliar los caracteres que se tratan como seguro para el codificador se insertaría la línea siguiente en el `ConfigureServices()` método `startup.cs`;

```csharp
services.AddSingleton<HtmlEncoder>(
     HtmlEncoder.Create(allowedRanges: new[] { UnicodeRanges.BasicLatin,
                                               UnicodeRanges.CjkUnifiedIdeographs }));
   ```

En este ejemplo se amplía la lista segura para que incluya la CjkUnifiedIdeographs de intervalo de Unicode. Ahora se convertiría en la salida representada

```html
<p>This link text is in Chinese: <a href="/">汉语/漢語</a></p>
   ```

Intervalos de lista segura se especifican como gráficos de código Unicode, no los idiomas. El [estándar Unicode](http://unicode.org/) tiene una lista de [gráficos de código](http://www.unicode.org/charts/index.html) puede usar para buscar el gráfico que contiene los caracteres. Cada codificador, Html, JavaScript y dirección Url, debe configurarse por separado.

> [!NOTE]
> Personalización de la lista segura sólo afecta a los codificadores de origen a través de DI. Si tiene acceso directamente a un codificador a través de `System.Text.Encodings.Web.*Encoder.Default` , a continuación, el valor predeterminado, Latín básico se usará sólo lista segura.

## <a name="where-should-encoding-take-place"></a>¿Dónde debe colocar tienen codificación?

La ficha general acepta práctica es que la codificación realiza en el punto de salida y valores codificados nunca deben almacenarse en una base de datos. Codificación en el punto de salida permite cambiar el uso de datos, por ejemplo, de HTML a un valor de cadena de consulta. También le permite buscar fácilmente los datos sin tener que codificar valores antes de buscar y le permite aprovechar las ventajas de las modificaciones o intentados codificadores de correcciones de errores.

## <a name="validation-as-an-xss-prevention-technique"></a>Validación como una técnica de prevención de XSS

Validación puede ser una herramienta útil para limitar los ataques XSS. Por ejemplo, una cadena numérica simple que contiene los caracteres 0-9 no desencadenará un ataque XSS. Validación se complica si desea aceptar HTML en proporcionados por el usuario - analiza la entrada de HTML es difícil, si no imposible. Otros formatos de texto y marcado sería una opción más segura para la entrada enriquecido. Nunca debe confiar en la validación por sí sola. Codifique siempre la entrada no es de confianza antes de la salida, con independencia de qué validación ha llevado a cabo.
