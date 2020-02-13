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
# <a name="prevent-cross-site-scripting-xss-in-aspnet-core"></a>Impedir el scripting entre sitios (XSS) en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Cross-Site Scripting (XSS) es una vulnerabilidad que permite a un atacante ejecutar scripts desde el lado del cliente (normalmente JavaScript) en las páginas web. Cuando otros usuarios cargan las páginas afectadas, se ejecutarán los scripts del atacante, lo que permitirá al atacante robar cookies y tokens de sesión, cambiar el contenido de la página web a través de la manipulación del DOM o redirigir el explorador a otra página. Las vulnerabilidades XSS suelen producirse cuando una aplicación toma datos proporcionados por el usuario y los envía a una página sin validarla, codificarla o escapar de ella.

## <a name="protecting-your-application-against-xss"></a>Proteger la aplicación contra XSS

En un nivel básico, XSS funciona engañando a la aplicación para insertar una `<script>` etiqueta en la página representada o insertando un evento de `On*` en un elemento. Los desarrolladores deben seguir los siguientes pasos para evitar introducir una vulnerabilidad XSS en su aplicación.

1. Nunca coloque datos que no sean de confianza en su código html, a menos que siga el resto de los pasos. Los datos que no son de confianza son cualquier dato que puede estar controlado por un atacante, entradas de formulario HTML, textos de consultas, cabeceras HTTP, incluso los datos que proceden de una base de datos donde un atacante puede encontrar una vulnerabilidad sin pasar por la aplicación.

2. Asegúrese de que el contenido HTML de confianza esté codificado antes de ponerlo dentro de un elemento HTML. La codificación HTML toma caracteres como &lt; y los cambia en un formato seguro como &amp;lt;

3. Antes de colocar los datos que no son de confianza en un atributo HTML, asegúrese de que está codificado en HTML. La codificación en atributos es una versión de la codificación html que toma caracteres como "y".

4. Antes de poner los datos que no sean de confianza en JavaScript, coloque los datos en un elemento HTML cuyo contenido se recupera en tiempo de ejecución. Si esto no es posible, asegúrese de que los datos están codificados con JavaScript. La codificación de JavaScript toma caracteres peligrosos para JavaScript y los reemplaza por su hexadecimal, por ejemplo &lt; se codificaría como `\u003C`.

5. Antes de poner los datos que no sean de confianza en el texto de una consulta en una dirección URL asegúrese de que esté codificado como URL.

## <a name="html-encoding-using-razor"></a>Codificación HTML mediante Razor

El motor de Razor usado en MVC codifica automáticamente todas las salidas que proceden de las variables, a menos que te esfuerces para evitarlo. Usa reglas de codificación de atributos HTML siempre que se usa la directiva *@* . Como la codificación HTML de atributos es una mejora de la codificación HTML significa que no tiene que preocuparse de si debe utilizar la codificación HTML o la codificación de atributos. Debe asegurarse de que utiliza *@* solo en un contexto HTML, no cuando intente insertar una entrada que no es de confianza en JavaScript. Algunas etiquetas auxiliares también codificarán la entrada que se usa en los parámetros de otras etiquetas.

Teniendo la siguiente vista de Razor:

```cshtml
@{
       var untrustedInput = "<\"123\">";
   }

   @untrustedInput
   ```

Esta vista genera el contenido de la variable *untrustedInput* . Esta variable incluye algunos caracteres que se usan en ataques XSS, es decir, &lt;, y &gt;. Examinando el origen, muestra la salida codificada y representada como:

```html
&lt;&quot;123&quot;&gt;
   ```

>[!WARNING]
> ASP.NET Core MVC proporciona una clase `HtmlString` que no se codifica automáticamente en la salida. Esto nunca debe utilizarse en combinación con entradas no seguras ya que esto expondrá una vulnerabilidad XSS.

## <a name="javascript-encoding-using-razor"></a>Codificación de JavaScript con Razor

Puede haber ocasiones en las que desee insertar un valor en JavaScript para procesarlo en la vista. Existen dos formas de hacerlo. La forma más segura de insertar valores es colocar el valor en un atributo de datos de una etiqueta y recuperarlo en el código JavaScript. Por ejemplo:

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

Que, cuando se ejecuta, presentará lo siguiente:

```
<"123">
   <"123">
```

También puede llamar al codificador de JavaScript directamente:

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

Esto se representará en el explorador de la siguiente manera:

```html
<script>
    document.write("\u003C\u0022123\u0022\u003E");
</script>
```

>[!WARNING]
> No concatene la entrada de desconfianza en el código JavaScript para crear los elementos DOM. Debe usar `createElement()` y asignar los valores de propiedad de forma adecuada, como `node.TextContent=`, o usar `element.SetAttribute()`/`element[attribute]=` de lo contrario, se expone a sí mismo a XSS basado en DOM.

## <a name="accessing-encoders-in-code"></a>Obtener acceso a codificadores en el código

Los codificadores HTML, JavaScript y URL están disponibles para el código de dos maneras: puede inyectarlos a través de la [inserción de dependencias](xref:fundamentals/dependency-injection) o puede usar los codificadores predeterminados contenidos en el espacio de nombres `System.Text.Encodings.Web`. Si usa los codificadores de forma predeterminada hará que todos los caracteres que se traten como seguros no surjan efecto: los codificadores predeterminados usan las reglas de codificación más seguras posibles.

Para usar los codificadores configurables a través de DI, los constructores deben tomar un parámetro *HtmlEncoder*, *JavaScriptEncoder* y *UrlEncoder* , según corresponda. Por ejemplo:

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

## <a name="encoding-url-parameters"></a>Parámetros de URL de codificación

Si desea crear una cadena de consulta de dirección URL con una entrada que no sea de confianza como valor, use el `UrlEncoder` para codificar el valor. Por ejemplo,

```csharp
var example = "\"Quoted Value with spaces and &\"";
   var encodedValue = _urlEncoder.Encode(example);
   ```

Después de codificar, la variable encodedValue contendrá `%22Quoted%20Value%20with%20spaces%20and%20%26%22`. Los espacios, las comillas, signos de puntuación y otros caracteres no seguros serán codificados en su valor hexadecimal, por ejemplo un carácter de espacio se convertirá en % 20.

>[!WARNING]
> No use datos que no son de confianza como parte de una consulta en una URL. Pase siempre los datos que no son de confianza como una cadena de texto.

<a name="security-cross-site-scripting-customization"></a>

## <a name="customizing-the-encoders"></a>Personalización de los codificadores

De forma predeterminada, los codificadores utilizan una lista segura limitada en el rango Unicode del Latín básico y codifican todos los caracteres fuera de ese intervalo como sus equivalentes. Este comportamiento también afecta a la representación de TagHelper Razor y HtmlHelper que utilizarán los codificadores para las cadenas de salida.

El razonamiento sirve como protección ante posibles errores desconocidos o que puedan suceder en un futuro (se han observado anteriores errores de exploradores al confundir el análisis basado en el procesamiento de caracteres no válidos en caracteres no ingleses). Si su sitio web hace un uso intensivo de los caracteres no latinos, como el chino, cirílico u otros probablemente no es el comportamiento que desee.

Puede personalizar las listas seguras del codificador para incluir los intervalos Unicode adecuados para la aplicación durante el inicio, en `ConfigureServices()`.

Por ejemplo, si usa la configuración predeterminada, puede usar una de Razor, como la siguiente.

```html
<p>This link text is in Chinese: @Html.ActionLink("汉语/漢語", "Index")</p>
   ```

Al ver el origen de la página web, verá que se ha representado de la siguiente forma, con el texto en chino codificado;

```html
<p>This link text is in Chinese: <a href="/">&#x6C49;&#x8BED;/&#x6F22;&#x8A9E;</a></p>
   ```

Para ampliar los caracteres que el codificador trata como seguros, debe insertar la siguiente línea en el método `ConfigureServices()` de `startup.cs`;

```csharp
services.AddSingleton<HtmlEncoder>(
     HtmlEncoder.Create(allowedRanges: new[] { UnicodeRanges.BasicLatin,
                                               UnicodeRanges.CjkUnifiedIdeographs }));
   ```

En este ejemplo se amplía la lista segura para que incluya el intervalo Unicode CjkUnifiedIdeographs. Su salida se convertirá a:

```html
<p>This link text is in Chinese: <a href="/">汉语/漢語</a></p>
   ```

Los intervalos de la lista segura se especifican como gráficos de códigos Unicode, no como lenguajes. El [estándar Unicode](https://unicode.org/) tiene una lista de [gráficos de código](https://www.unicode.org/charts/index.html) que puede usar para buscar el gráfico que contiene los caracteres. Cada codificador, HTML, JavaScript y URL, debe configurarse por separado.

> [!NOTE]
> La personalización de la lista segura solo afecta a los codificadores de código abiertos a través de DI. Si obtiene acceso directamente a un codificador a través de `System.Text.Encodings.Web.*Encoder.Default`, se usará la configuración de la base de datos de solo latín básica básica.

## <a name="where-should-encoding-take-place"></a>¿Dónde se debe realizar la codificación?

Una buena practica aceptada es la que se lleva a cabo en la salida de la codificación, los valores codificados nunca deben almacenarse en una base de datos. La codificación en el punto de salida permite cambiar el uso de datos, por ejemplo, de HTML a un valor de una consulta. También permite realizar búsquedas fácilmente en los datos sin tener que codificar valores antes de realizar búsquedas y permite aprovechar las ventajas de los cambios o las correcciones de errores que se realicen en los codificadores.

## <a name="validation-as-an-xss-prevention-technique"></a>Validación como técnica de prevención de XSS

La validación puede ser una herramienta útil para limitar los ataques XSS. Por ejemplo, una cadena numérica que solo contenga los caracteres 0-9 no desencadenará un ataque XSS. La validación pasa a ser más complicada al aceptar código HTML en la entrada del usuario. El análisis de la entrada HTML es difícil, si no imposible. Markdown permite junto con un analizador eliminar los HTML incrustados, es una opción más segura para aceptar la entrada de texto enriquecido. Nunca hay que basarse únicamente en la validación. Es necesario codificar siempre la entrada no confiable antes de la salida, independientemente de qué validación o comprobación de estado se ha realizado.
