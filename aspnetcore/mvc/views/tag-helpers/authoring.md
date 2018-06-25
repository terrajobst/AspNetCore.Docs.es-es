---
title: Crear aplicaciones auxiliares de etiquetas en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo crear aplicaciones auxiliares de etiquetas en ASP.NET Core.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 01/19/2018
uid: mvc/views/tag-helpers/authoring
ms.openlocfilehash: 5873c6dbdeba1b5f2bf7ac85d8992480228b7125
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275322"
---
# <a name="author-tag-helpers-in-aspnet-core"></a>Crear aplicaciones auxiliares de etiquetas en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="get-started-with-tag-helpers"></a>Introducción a las aplicaciones auxiliares de etiquetas

En este tutorial se proporciona una introducción a la programación de aplicaciones auxiliares de etiquetas. En [Introducción a las aplicaciones auxiliares de etiquetas](intro.md) se describen las ventajas que proporcionan las aplicaciones auxiliares de etiquetas.

Una aplicación auxiliar de etiquetas es una clase que implementa la interfaz `ITagHelper`. A pesar de ello, cuando se crea una aplicación auxiliar de etiquetas, normalmente se deriva de `TagHelper`, lo que da acceso al método `Process`.

1. Cree un proyecto de ASP.NET Core denominado **AuthoringTagHelpers**. No necesita autenticación para este proyecto.

2. Cree una carpeta para almacenar las aplicaciones auxiliares de etiquetas denominada *TagHelpers*. La carpeta *TagHelpers* *no* es necesaria, pero es una convención razonable. Ahora vamos a empezar a escribir algunas aplicaciones auxiliares de etiquetas simples.

## <a name="a-minimal-tag-helper"></a>Aplicación auxiliar de etiquetas mínima

En esta sección, escribirá una aplicación auxiliar de etiquetas que actualice una etiqueta de correo electrónico. Por ejemplo:

```html
<email>Support</email>
   ```

El servidor usará nuestra aplicación auxiliar de etiquetas de correo electrónico para convertir ese marcado en lo siguiente:

```html
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

Es decir, una etiqueta delimitadora lo convierte en un vínculo de correo electrónico. Tal vez le interese si está escribiendo un motor de blogs y necesita que envíe correos electrónicos a contactos de marketing, soporte técnico y de otro tipo, todos ellos en el mismo dominio.

1. Agregue la siguiente clase `EmailTagHelper` a la carpeta *TagHelpers*.

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1EmailTagHelperCopy.cs)]
    
   **Notas:**
    
   * Las aplicaciones auxiliares de etiquetas usan una convención de nomenclatura que tiene como destino los elementos de la clase raíz (menos la parte *TagHelper* del nombre de clase). En este ejemplo, el nombre raíz de **Email**TagHelper es *email*, por lo que el destino será la etiqueta `<email>`. Esta convención de nomenclatura debería funcionar para la mayoría de las aplicaciones auxiliares de etiquetas. Más adelante veremos cómo invalidarla.
    
   * La clase `EmailTagHelper` se deriva de `TagHelper`. La clase `TagHelper` proporciona métodos y propiedades para escribir aplicaciones auxiliares de etiquetas.
    
   * El método `Process` invalidado controla lo que hace la aplicación auxiliar de etiquetas cuando se ejecuta. La clase `TagHelper` también proporciona una versión asincrónica (`ProcessAsync`) con los mismos parámetros.
    
   * El parámetro de contexto para `Process` (y `ProcessAsync`) contiene información relacionada con la ejecución de la etiqueta HTML actual.
    
   * El parámetro de salida para `Process` (y `ProcessAsync`) contiene un elemento HTML con estado que representa el origen original usado para generar una etiqueta y contenido HTML.
    
   * El nombre de nuestra clase tiene un sufijo **TagHelper**, que *no* es necesario, pero es una convención recomendada. Podría declarar la clase de la manera siguiente:
    
   ```csharp
   public class Email : TagHelper
   ```

2. Para hacer que la clase `EmailTagHelper` esté disponible para todas nuestras vistas de Razor, agregue la directiva `addTagHelper` al archivo *Views/_ViewImports.cshtml*: [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]
    
   El código anterior usa la sintaxis de comodines para especificar que todas las aplicaciones auxiliares de etiquetas del ensamblado estarán disponibles. La primera cadena después de `@addTagHelper` especifica la aplicación auxiliar de etiquetas que se va a cargar (use "*" para todas las aplicaciones auxiliares de etiquetas), mientras que la segunda cadena "AuthoringTagHelpers" especifica el ensamblado en el que se encuentra la aplicación auxiliar de etiquetas. Además, tenga en cuenta que la segunda línea incorpora las aplicaciones auxiliares de etiquetas de ASP.NET Core MVC mediante la sintaxis de comodines (esas aplicaciones auxiliares se tratan en el tema [Introducción a las aplicaciones auxiliares de etiquetas](intro.md)). Es la directiva `@addTagHelper` la que hace que la aplicación auxiliar de etiquetas esté disponible para la vista de Razor. Como alternativa, puede proporcionar el nombre completo (FQN) de una aplicación auxiliar de etiquetas como se muestra a continuación:
    
```csharp
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```
    
<!--
the following snippet uses TagHelpers3 and should use TagHelpers (not the 3)
    [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImports.cshtml?highlight=3&range=1-3)]
-->
    
Para agregar una aplicación auxiliar de etiquetas a una vista con un FQN, agregue primero el FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) y, después, el nombre del ensamblado (*AuthoringTagHelpers*). La mayoría de los desarrolladores prefiere usar la sintaxis de comodines. En [Introducción a las aplicaciones auxiliares de etiquetas](intro.md) se describe en detalle la adición y eliminación de aplicaciones auxiliares de etiquetas, la jerarquía y la sintaxis de comodines.
    
3. Actualice el marcado del archivo *Views/Home/Contact.cshtml* con estos cambios:

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

4. Ejecute la aplicación y use su explorador favorito para ver el código fuente HTML, a fin de comprobar que las etiquetas de correo electrónico se han reemplazado por un marcado delimitador (por ejemplo, `<a>Support</a>`). *Support* y *Marketing* se representan como vínculos, pero no tienen un atributo `href` que los haga funcionales. Esto lo corregiremos en la sección siguiente.

## <a name="setattribute-and-setcontent"></a>SetAttribute y SetContent

En esta sección, actualizaremos `EmailTagHelper` para que cree una etiqueta delimitadora válida para correo electrónico. Lo actualizaremos para que tome información de una vista de Razor (en forma de atributo `mail-to`) y la use al generar el delimitador.

Actualice la clase `EmailTagHelper` con lo siguiente:

[!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?range=6-22)]

**Notas:**

* Los nombres de clase y propiedad con grafía Pascal para las aplicaciones auxiliares de etiquetas se convierten a su [grafía kebab en minúsculas](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101). Por tanto, para usar el atributo `MailTo`, usará su equivalente `<email mail-to="value"/>`.

* La última línea establece el contenido completado para nuestra aplicación auxiliar de etiquetas mínimamente funcional.

* La línea resaltada muestra la sintaxis para agregar atributos:

[!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?highlight=6&range=14-21)]

Este enfoque funciona para el atributo "href" siempre y cuando no exista actualmente en la colección de atributos. También puede usar el método `output.Attributes.Add` para agregar un atributo de aplicación auxiliar de etiquetas al final de la colección de atributos de etiqueta.

1. Actualice el marcado del archivo *Views/Home/Contact.cshtml* con estos cambios: [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]

2. Ejecute la aplicación y compruebe que genera los vínculos correctos.
    
   > [!NOTE]
   > Si escribe la etiqueta de correo electrónico como de autocierre (`<email mail-to="Rick" />`), la salida final también será de autocierre. Para habilitar la capacidad de escribir la etiqueta únicamente con una etiqueta de apertura (`<email mail-to="Rick">`) debe decorar la clase con lo siguiente:
   > 
   > [!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailVoid.cs?highlight=1&range=6-10)]
    
   Con una aplicación auxiliar de etiquetas de correo electrónico de autocierre, el resultado sería `<a href="mailto:Rick@contoso.com" />`. Las etiquetas delimitadoras de autocierre no son HTML válido, por lo que no le interesa crear una, pero tal vez le convenga crear una aplicación auxiliar de etiquetas de autocierre. Las aplicaciones auxiliares de etiquetas establecen el tipo de la propiedad `TagMode` después de leer una etiqueta.
    
### <a name="processasync"></a>ProcessAsync

En esta sección, escribiremos una aplicación auxiliar de correo electrónico asincrónica.

1. Reemplace la clase `EmailTagHelper` por el siguiente código:

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelper.cs?range=6-17)]

   **Notas:**

   * Esta versión usa el método `ProcessAsync` asincrónico. El método `GetChildContentAsync` asincrónico devuelve un valor `Task` que contiene `TagHelperContent`.

   * Use el parámetro `output` para obtener el contenido del elemento HTML.

2. Realice el cambio siguiente en el archivo *Views/Home/Contact.cshtml* para que la aplicación auxiliar de etiquetas pueda obtener el correo electrónico de destino.

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

3. Ejecute la aplicación y compruebe que genera vínculos de correo electrónico válidos.

### <a name="removeall-precontentsethtmlcontent-and-postcontentsethtmlcontent"></a>RemoveAll, PreContent.SetHtmlContent y PostContent.SetHtmlContent

1. Agregue la siguiente clase `BoldTagHelper` a la carpeta *TagHelpers*.

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/BoldTagHelper.cs)]

   **Notas:**
    
   * El atributo `[HtmlTargetElement]` pasa un parámetro de atributo que especifica que todos los elementos HTML que contengan un atributo HTML denominado "bold" coincidirán, y se ejecutará el método de invalidación `Process` de la clase. En nuestro ejemplo, el método `Process` quita el atributo "bold" y rodea el marcado contenedor con `<strong></strong>`.
    
   * Dado que no le interesa reemplazar el contenido existente de la etiqueta, debe escribir la etiqueta de apertura `<strong>` con el método `PreContent.SetHtmlContent` y la etiqueta de cierre `</strong>` con el método `PostContent.SetHtmlContent`.
    
2. Modifique la vista *About.cshtml* para que contenga un valor de atributo `bold`. A continuación se muestra el código completado.

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutBoldOnly.cshtml?highlight=7)]

3. Ejecute la aplicación. Puede usar el explorador que prefiera para inspeccionar el origen y comprobar el marcado.

   El atributo `[HtmlTargetElement]` anterior solo tiene como destino el marcado HTML que proporciona el nombre de atributo "bold". La aplicación auxiliar de etiquetas no ha modificado el elemento `<bold>`.

4. Convierta en comentario la línea de atributo `[HtmlTargetElement]` y de forma predeterminada tendrá como destino las etiquetas `<bold>`, es decir, el marcado HTML con formato `<bold>`. Recuerde que la convención de nomenclatura predeterminada hará coincidir el nombre de clase **Bold**TagHelper con las etiquetas `<bold>`.

5. Ejecute la aplicación y compruebe que la aplicación auxiliar de etiquetas procesa la etiqueta `<bold>`.

El proceso de decorar una clase con varios atributos `[HtmlTargetElement]` tiene como resultado una operación OR lógica de los destinos. Por ejemplo, si se usa el código siguiente, una etiqueta bold o un atributo bold coincidirán.

[!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zBoldTagHelperCopy.cs?highlight=1,2&range=5-15)]

Cuando se agregan varios atributos a la misma instrucción, el tiempo de ejecución los trata como una operación AND lógica. Por ejemplo, en el código siguiente, un elemento HTML debe denominarse "bold" con un atributo denominado "bold" (`<bold bold />`) para que coincida.

```csharp
[HtmlTargetElement("bold", Attributes = "bold")]
   ```

También puede usar `[HtmlTargetElement]` para cambiar el nombre del elemento de destino. Por ejemplo, si quiere que `BoldTagHelper` tenga como destino etiquetas `<MyBold>`, use el atributo siguiente:

```csharp
[HtmlTargetElement("MyBold")]
   ```

## <a name="pass-a-model-to-a-tag-helper"></a>Pasar un modelo a una aplicación auxiliar de etiquetas

1. Agregue una carpeta *Models*.

2. Agregue la clase `WebsiteContext` siguiente a la carpeta *Models*:

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Models/WebsiteContext.cs)]

3. Agregue la siguiente clase `WebsiteInformationTagHelper` a la carpeta *TagHelpers*.

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/WebsiteInformationTagHelper.cs)]
    
   **Notas:**
    
   * Como se ha indicado anteriormente, las aplicaciones auxiliares de etiquetas convierten las propiedades y nombres de clase de C# con grafía Pascal para aplicaciones auxiliares de etiquetas en [grafía kebab en minúsculas](http://wiki.c2.com/?KebabCase). Por tanto, para usar `WebsiteInformationTagHelper` en Razor, deberá escribir `<website-information />`.
    
   * No está identificando de manera explícita el elemento de destino con el atributo `[HtmlTargetElement]`, por lo que el destino será el valor predeterminado de `website-information`. Si ha aplicado el atributo siguiente (tenga en cuenta que no tiene grafía kebab, pero coincide con el nombre de clase):
    
   ```csharp
   [HtmlTargetElement("WebsiteInformation")]
   ```
    
   La etiqueta con grafía kebab en minúsculas `<website-information />` no coincidiría. Si quiere usar el atributo `[HtmlTargetElement]`, debe usar la grafía kebab como se muestra a continuación:
    
   ```csharp
   [HtmlTargetElement("Website-Information")]
   ```
    
   * Los elementos que son de autocierre no tienen contenido. En este ejemplo, el marcado de Razor usará una etiqueta de autocierre, pero la aplicación auxiliar de etiquetas creará un elemento [section](http://www.w3.org/TR/html5/sections.html#the-section-element) (que no es de autocierre, y el contenido se escribirá dentro del elemento `section`). Por tanto, debe establecer `TagMode` en `StartTagAndEndTag` para escribir la salida. Como alternativa, puede convertir en comentario la línea donde se establece `TagMode` y escribir marcado con una etiqueta de cierre. (Más adelante en este tutorial se proporciona marcado de ejemplo).
    
   * El signo de dólar `$` de la línea siguiente usa una [cadena interpolada](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings):
    
   ```cshtml
   $@"<ul><li><strong>Version:</strong> {Info.Version}</li>
   ```

4. Agregue el marcado siguiente a la vista *About.cshtml*. En el marcado resaltado se muestra la información del sitio web.
    
   [!code-html[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?highlight=1,12-999)]
    
   > [!NOTE]
   > En el marcado de Razor que se muestra a continuación:
   > 
   > [!code-html[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?range=13-17)]
   > 
   > Razor sabe que el atributo `info` es una clase, no una cadena, y usted quiere escribir código de C#. Todos los atributos de aplicaciones auxiliares de etiquetas que no sean una cadena deben escribirse sin el carácter `@`.
    
5. Ejecute la aplicación y vaya a la vista About para ver la información del sitio web.

   > [!NOTE]
   > Puede usar el marcado siguiente con una etiqueta de cierre y quitar la línea con `TagMode.StartTagAndEndTag` de la aplicación auxiliar de etiquetas:
   > 
   > [!code-html[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutNotSelfClosing.cshtml?range=13-18)]

## <a name="condition-tag-helper"></a>Aplicación auxiliar de etiquetas de condición

La aplicación auxiliar de etiquetas de condición representa la salida cuando se pasa un valor true.

1. Agregue la siguiente clase `ConditionTagHelper` a la carpeta *TagHelpers*.

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/ConditionTagHelper.cs)]

2. Reemplace el contenido del archivo *Views/Home/Index.cshtml* por el marcado siguiente:

   ```cshtml
   @using AuthoringTagHelpers.Models
   @model WebsiteContext
    
   @{
       ViewData["Title"] = "Home Page";
   }
    
   <div>
       <h3>Information about our website (outdated):</h3>
       <website-information info=@Model />
       <div condition="@Model.Approved">
           <p>
               This website has <strong surround="em"> @Model.Approved </strong> been approved yet.
               Visit www.contoso.com for more information.
           </p>
       </div>
   </div>
   ```
    
3. Reemplace el método `Index` del controlador `Home` por el código siguiente:

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Controllers/HomeController.cs?range=9-18)]

4. Ejecute la aplicación y vaya a la página principal. El marcado del elemento condicional `div` no se representará. Anexe la cadena de consulta `?approved=true` a la dirección URL (por ejemplo, `http://localhost:1235/Home/Index?approved=true`). `approved` se establece en true y se muestra el marcado condicional.

> [!NOTE]
> Use el operador [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) para especificar el atributo de destino en lugar de especificar una cadena, como hizo con la aplicación auxiliar de etiquetas bold:
> 
> [!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zConditionTagHelperCopy.cs?highlight=1,2,5&range=5-18)]
> 
> El operador [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) protegerá el código si en algún momento debe refactorizarse (tal vez interese cambiar el nombre a `RedCondition`).

### <a name="avoid-tag-helper-conflicts"></a>Evitar conflictos de aplicaciones auxiliares de etiquetas

En esta sección, escribirá un par de aplicaciones auxiliares de etiquetas de vinculación automática. La primera reemplazará el marcado que contiene una dirección URL que empieza con HTTP por una etiqueta delimitadora HTML que contiene la misma dirección URL (y, por tanto, produce un vínculo a la dirección URL). La segunda hará lo mismo para una dirección URL que empieza con WWW.

Dado que estas dos aplicaciones auxiliares están estrechamente relacionadas y tal vez las refactorice en el futuro, las guardaremos en el mismo archivo.

1. Agregue la siguiente clase `AutoLinkerHttpTagHelper` a la carpeta *TagHelpers*.

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=7-19)]

   >[!NOTE]
   >La clase `AutoLinkerHttpTagHelper` tiene como destino elementos `p` y usa [Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) para crear el delimitador.

2. Agregue el marcado siguiente al final del archivo *Views/Home/Contact.cshtml*:

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=19)]

3. Ejecute la aplicación y compruebe que la aplicación auxiliar de etiquetas representa el delimitador correctamente.

4. Actualice la clase `AutoLinker` para que incluya la aplicación auxiliar de etiquetas `AutoLinkerWwwTagHelper` que convertirá el texto www en una etiqueta delimitadora que también contenga el texto www original. El código actualizado aparece resaltado a continuación:

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?highlight=15-34&range=7-34)]

5. Ejecute la aplicación. Observe que el texto www se representa como un vínculo, a diferencia del texto HTTP. Si coloca un punto de interrupción en ambas clases, verá que la clase de la aplicación auxiliar de etiquetas HTTP se ejecuta primero. El problema es que la salida de la aplicación auxiliar de etiquetas se almacena en caché y, cuando se ejecuta la aplicación auxiliar de etiquetas WWW, sobrescribe la salida almacenada en caché desde la aplicación auxiliar de etiquetas HTTP. Más adelante en el tutorial veremos cómo se controla el orden en el que se ejecutan las aplicaciones auxiliares de etiquetas. Corregiremos el código con lo siguiente:

   [!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10,21,22,26&range=8-37)]

   > [!NOTE]
   > La primera vez que editó las aplicaciones auxiliares de etiquetas de vinculación automática, obtuvo el contenido del destino con el código siguiente:
   > 
   > [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=12)]
   > 
   > Es decir, ha llamado a `GetChildContentAsync` mediante la salida `TagHelperOutput` pasada al método `ProcessAsync`. Como ya se ha indicado, como la salida se almacena en caché, prevalece la última aplicación auxiliar de etiquetas que se ejecuta. Para corregir el error, ha usado el código siguiente:
   > 
   > [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?range=34-35)]
   > 
   > El código anterior comprueba si se ha modificado el contenido y, en caso afirmativo, obtiene el contenido del búfer de salida.

6. Ejecute la aplicación y compruebe que los dos vínculos funcionan según lo previsto. Aunque podría parecer que nuestra aplicación auxiliar de etiquetas de vinculación automática es correcta y está completa, tiene un pequeño problema. Si la aplicación auxiliar de etiquetas WWW se ejecuta en primer lugar, los vínculos www no serán correctos. Actualice el código mediante la adición de la sobrecarga `Order` para controlar el orden en el que se ejecuta la etiqueta. La propiedad `Order` determina el orden de ejecución en relación con las demás aplicaciones auxiliares de etiquetas que tienen como destino el mismo elemento. El valor de orden predeterminado es cero, y se ejecutan en primer lugar las instancias con los valores más bajos.

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?highlight=5,6,7,8&range=8-15)]
    
   El código anterior garantizará que la aplicación auxiliar de etiquetas HTTP se ejecute antes que la aplicación auxiliar de etiquetas WWW. Cambie `Order` a `MaxValue` y compruebe que el marcado generado para la etiqueta WWW es incorrecto.

## <a name="inspect-and-retrieve-child-content"></a>Inspeccionar y recuperar contenido secundario

Las aplicaciones auxiliares de etiquetas proporcionan varias propiedades para recuperar contenido.

-  El resultado de `GetChildContentAsync` se pueden anexar a `output.Content`.
-  Puede inspeccionar el resultado de `GetChildContentAsync` con `GetContent`.
-  Si modifica `output.Content`, el cuerpo de TagHelper no se ejecutará ni representará a menos que llame a `GetChildContentAsync`, como en nuestro ejemplo de vinculación automática:

[!code-csharp[](../../views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10&range=8-21)]

-  Varias llamadas a `GetChildContentAsync` devuelven el mismo valor y no vuelven a ejecutar el cuerpo de `TagHelper` a menos que se pase un parámetro false que indique que no se use el resultado almacenado en caché.
