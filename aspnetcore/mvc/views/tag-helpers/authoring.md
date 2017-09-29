---
title: "Creación de aplicaciones auxiliares de etiquetas en el núcleo de ASP.NET"
author: rick-anderson
description: "Obtenga información acerca de cómo crear aplicaciones auxiliares de etiquetas en ASP.NET Core."
keywords: "Núcleo de ASP.NET, aplicaciones auxiliares de etiquetas"
ms.author: riande
manager: wpickett
ms.date: 06/14/2017
ms.topic: article
ms.assetid: 4f16d978-5695-4abf-a785-fdaabf3bbcb9
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/tag-helpers/authoring
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fe1dc0949afced0c7c9ca1236c2f1bb4fe78b00e
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2017
---
# <a name="authoring-tag-helpers-in-aspnet-core-a-walkthrough-with-samples"></a>Aplicaciones auxiliares de etiquetas en el núcleo de ASP.NET, un tutorial con ejemplos de creación

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

[Ver o descargar el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample)

## <a name="getting-started-with-tag-helpers"></a>Introducción a las aplicaciones auxiliares de etiquetas

Este tutorial proporciona una introducción a la programación de aplicaciones auxiliares de etiquetas. [Introducción a las aplicaciones auxiliares de etiquetas](intro.md) describe las ventajas que proporciona aplicaciones auxiliares de etiquetas.

Una aplicación auxiliar para etiqueta es cualquier clase que implemente la `ITagHelper` interfaz. Sin embargo, al crear una aplicación auxiliar de la etiqueta, derivan normalmente de `TagHelper`, esto le brinda acceso a la `Process` método.

1. Crear un nuevo proyecto de ASP.NET Core denominado **AuthoringTagHelpers**. No necesita autenticación para este proyecto.

2. Cree una carpeta para almacenar las aplicaciones auxiliares de etiqueta denominado *TagHelpers*. El *TagHelpers* carpeta es *no* necesario, pero es una convención razonable. Ahora vamos a empezar a escribir algunas aplicaciones auxiliares de etiqueta simple.

## <a name="a-minimal-tag-helper"></a>Una aplicación auxiliar de etiquetas mínima

En esta sección, se escribe una aplicación auxiliar de etiquetas que actualiza una etiqueta de correo electrónico. Por ejemplo:

```html
<email>Support</email>
   ```

El servidor usará nuestra herramienta de etiqueta de correo electrónico para convertir ese marcado en lo siguiente:

```html
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
   ```

Es decir, una etiqueta delimitadora lo hace un vínculo de correo electrónico. Puede hacerlo si está escribiendo un motor de blogs y lo necesite para enviar correo electrónico de marketing, soporte técnico y otros contactos, todo ello en el mismo dominio.

1.  Agregue el siguiente `EmailTagHelper` clase a la *TagHelpers* carpeta.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1EmailTagHelperCopy.cs)]
    
    **Notas:**
    
    * Aplicaciones auxiliares de etiquetas usan una convención de nomenclatura que tenga como destino de los elementos de la clase raíz (menos la *TagHelper* parte del nombre de clase). En este ejemplo, el nombre de raíz de **correo electrónico**TagHelper es *correo electrónico*, por lo que el `<email>` se destinará la etiqueta. Esta convención de nomenclatura debería funcionar para la mayoría de aplicaciones auxiliares de etiquetas, más adelante voy a explicar cómo reemplazarlo.
    
    * La clase `EmailTagHelper` se deriva de `TagHelper`. La `TagHelper` clase proporciona propiedades y métodos para escribir aplicaciones auxiliares de etiquetas.
    
    * Reemplazados `Process` método controla lo que hace la aplicación auxiliar de la etiqueta cuando se ejecuta. El `TagHelper` clase también proporciona una versión asincrónica (`ProcessAsync`) con los mismos parámetros.
    
    * El parámetro de contexto a `Process` (y `ProcessAsync`) contiene información relacionada con la ejecución de la etiqueta HTML actual.
    
    * El parámetro de salida para `Process` (y `ProcessAsync`) contiene un elemento HTML con estado relacionado con el origen original que se usa para generar una etiqueta HTML y contenido.
    
    * El nombre de la clase tiene un sufijo de **TagHelper**, que es *no* necesario, pero que considera una convención de práctica recomendada. Podría declarar la clase como:
    
    ```csharp
    public class Email : TagHelper
    ```

2.  Para realizar la `EmailTagHelper` clase disponible para todas las vistas de nuestro Razor, agregue el `addTagHelper` la directiva a la *Views/_ViewImports.cshtml* archivo:[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]
    
    El código anterior utiliza la sintaxis de comodín para especificar que todas las aplicaciones auxiliares de etiqueta en el ensamblado estará disponibles. La primera cadena después `@addTagHelper` especifica la aplicación auxiliar de etiquetas para cargar (utilice "*" para todas las aplicaciones auxiliares de etiquetas), y la segunda cadena "AuthoringTagHelpers" especifica el ensamblado de la aplicación auxiliar de etiqueta. Además, tenga en cuenta que la segunda línea se pondrá en las aplicaciones auxiliares de la etiqueta principal de ASP.NET MVC usando la sintaxis de comodines (esas aplicaciones auxiliares se tratan en [Introducción a las aplicaciones auxiliares de etiquetas](intro.md).) Es el `@addTagHelper` directiva que hace la aplicación auxiliar de etiquetas esté disponible para la vista de Razor. Como alternativa, puede proporcionar el nombre completo (FQN) de una aplicación auxiliar de etiquetas tal y como se muestra a continuación:
    
    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImports.cshtml?highlight=3&range=1-3)]
    
    Para agregar una aplicación auxiliar de etiqueta a una vista con un FQN, agregue primero la FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) y, a continuación, el nombre del ensamblado (*AuthoringTagHelpers*). La mayoría de los programadores preferirán utilizar la sintaxis de comodines. [Introducción a las aplicaciones auxiliares de etiquetas](intro.md) entra en detalles acerca de la sintaxis de agregar, quitar, jerarquía y caracteres comodín de la aplicación auxiliar de etiqueta.
    
3.  Actualizar el marcado en el *Views/Home/Contact.cshtml* archivo con estos cambios:

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

4.  Ejecutar la aplicación y usar su explorador favorito para ver el código fuente HTML para que pueda comprobar que las etiquetas de correo electrónico se reemplazan con marcado delimitador (por ejemplo, `<a>Support</a>`). *Compatibilidad con* y *Marketing* se representan como vínculos, pero no tienen un `href` atributo para que sean funcionales. Se corregirá en la siguiente sección.

Nota: Al igual que las etiquetas HTML, etiquetas y atributos, nombres de clases y atributos en Razor y C# no son distingue mayúsculas de minúsculas.

## <a name="setattribute-and-setcontent"></a>SetAttribute y SetContent

En esta sección, actualizaremos el `EmailTagHelper` para que se va a crear una etiqueta de delimitador válido para correo electrónico. Le mantendremos para disponer de información de una vista Razor (en forma de un `mail-to` atributo) y que utilizar al generar el delimitador.

Actualización de la `EmailTagHelper` clase por lo siguiente:

[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?range=6-22)]

**Notas:**

* Nombres de propiedad y clase la grafía Pascal para aplicaciones auxiliares de etiquetas se convierten en sus [reducir caso kebab](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101). Por lo tanto, para usar el `MailTo` atributo, deberá usar `<email mail-to="value"/>` equivalente.

* La última línea establece el contenido completado para nuestra herramienta etiqueta mínimamente funcional.

* La línea resaltada muestra la sintaxis para agregar atributos:

[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?highlight=6&range=14-21)]

Este enfoque funciona para el atributo "href" siempre y cuando no existe actualmente en la colección de atributos. También puede usar el `output.Attributes.Add` método para agregar un atributo de aplicación auxiliar de etiquetas al final de la colección de atributos de etiqueta.

1.  Actualizar el marcado en el *Views/Home/Contact.cshtml* archivo con estos cambios:[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]

2.  Ejecute la aplicación y compruebe que éste genera los enlaces correctos.
    
    > [!NOTE]
    >Si fuera a escribir el correo electrónico etiqueta de autocierre (`<email mail-to="Rick" />`), el resultado final también sería autocierre. Para habilitar la capacidad para escribir la etiqueta con una etiqueta de apertura (`<email mail-to="Rick">`) debe decorar la clase con la siguiente:
    >
    > [!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailVoid.cs?highlight=1&range=6-10)]
    
    Con una aplicación auxiliar autocierre etiqueta de correo electrónico, el resultado sería `<a href="mailto:Rick@contoso.com" />`. Las etiquetas de cierre automático no son HTML válido, así, no deseará crear uno, pero podría ser conveniente crear una aplicación auxiliar de etiquetas que es de cierre automático. Aplicaciones auxiliares de etiquetas establecer el tipo de la `TagMode` propiedad después de leer una etiqueta.
    
### <a name="processasync"></a>ProcessAsync

En esta sección, se escribirá una aplicación auxiliar de correo electrónico asincrónica.

1.  Reemplace el `EmailTagHelper` clase con el código siguiente:

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelper.cs?range=6-17)]

    **Notas:**

    * Esta versión usa asincrónico `ProcessAsync` método. Asincrónico `GetChildContentAsync` devuelve un `Task` que contiene el `TagHelperContent`.

    * Use la `output` para obtener el contenido del elemento HTML.

2.  Realice el siguiente cambio en el *Views/Home/Contact.cshtml* de archivos para la aplicación auxiliar de la etiqueta puede obtener el correo electrónico de destino.

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

3.  Ejecute la aplicación y compruebe que éste genera vínculos de correo electrónico válida.

### <a name="removeall-precontentsethtmlcontent-and-postcontentsethtmlcontent"></a>RemoveAll, PreContent.SetHtmlContent y PostContent.SetHtmlContent

1.  Agregue el siguiente `BoldTagHelper` clase a la *TagHelpers* carpeta.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/BoldTagHelper.cs)]

    **Notas:**
    
    * El `[HtmlTargetElement]` atributo pasadas coincidirá con un parámetro de atributo que especifica que cualquier elemento HTML que contiene un atributo HTML denominado "bold", y el `Process` se ejecutará el método de invalidación de la clase. En nuestro ejemplo, el `Process` método quita el atributo "negrita" y rodea el contenedor marcado con `<strong></strong>`.
    
    * Dado que no desea reemplazar el contenido de la etiqueta existente, debe escribir la apertura `<strong>` etiquetar con el `PreContent.SetHtmlContent` método y el cierre `</strong>` etiquetar con los `PostContent.SetHtmlContent` método.
    
2.  Modificar el *About.cshtml* vista para que contenga un `bold` valor de atributo. El código completo se muestra a continuación.

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutBoldOnly.cshtml?highlight=7)]

3.  Ejecute la aplicación. Puede utilizar el explorador que prefiera para inspeccionar el origen y compruebe el marcado.

    El `[HtmlTargetElement]` atributo anterior solo tiene como destino la marca HTML que proporciona un nombre de atributo de "bold". El `<bold>` elemento no fue modificado por la aplicación auxiliar de etiqueta.

4. Convierta en comentario la `[HtmlTargetElement]` la línea de atributo y serán destinos `<bold>` etiquetas, es decir, el formato HTML del formulario `<bold>`. Recuerde que la convención de nomenclatura predeterminado coincidirá con el nombre de clase **negrita**TagHelper a `<bold>` etiquetas.

5. Ejecute la aplicación y compruebe que la `<bold>` etiqueta es procesada por la aplicación auxiliar de etiqueta.

Al decorar una clase con varios `[HtmlTargetElement]` da como resultado una operación lógica OR de los destinos de atributos. Por ejemplo, con el código siguiente, una etiqueta de negrita o un atributo negrita coincidirá.

[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zBoldTagHelperCopy.cs?highlight=1,2&range=5-15)]

Cuando se agregan varios atributos a la misma instrucción, el tiempo de ejecución los trata como un AND lógico. Por ejemplo, en el código siguiente, un elemento HTML debe denominarse "bold" con un atributo denominado "bold" (`<bold bold />`) para hacer coincidir.

```csharp
[HtmlTargetElement("bold", Attributes = "bold")]
   ```

También puede usar el `[HtmlTargetElement]` para cambiar el nombre del elemento de destino. Por ejemplo, si deseara la `BoldTagHelper` destino `<MyBold>` etiquetas, usaría el siguiente atributo:

```csharp
[HtmlTargetElement("MyBold")]
   ```

## <a name="passing-a-model-to-a-tag-helper"></a>Pasar un modelo a una aplicación auxiliar de etiqueta

1.  Agregar un *modelos* carpeta.

2.  Agregue la clase `WebsiteContext` siguiente a la carpeta *Models*:

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Models/WebsiteContext.cs)]

3.  Agregue el siguiente `WebsiteInformationTagHelper` clase a la *TagHelpers* carpeta.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/WebsiteInformationTagHelper.cs)]
    
    **Notas:**
    
    * Como se mencionó anteriormente, aplicaciones auxiliares de etiquetas traduce los nombres de clase de C# la grafía Pascal y propiedades para aplicaciones auxiliares de etiquetas en [reducir caso kebab](http://wiki.c2.com/?KebabCase). Por lo tanto, para usar el `WebsiteInformationTagHelper` en Razor, se escribirá `<website-information />`.
    
    * No se está identificando explícitamente el elemento de destino con el `[HtmlTargetElement]` atributo, por lo que el valor predeterminado de `website-information` se destinará. Si ha aplicado el atributo siguiente (tenga en cuenta no es el caso de kebab pero coincide con el nombre de clase):
    
    ```csharp
    [HtmlTargetElement("WebsiteInformation")]
    ```
    
    La etiqueta de caso kebab inferior `<website-information />` no coincidirían. Si desea usar el `[HtmlTargetElement]` atributo, usaría el caso de kebab tal y como se muestra a continuación:
    
    ```csharp
    [HtmlTargetElement("Website-Information")]
    ```
    
    * Elementos que se cierre automático no tienen ningún contenido. En este ejemplo, el marcado de Razor usará una etiqueta de cierre automático, pero se creará la aplicación auxiliar de etiqueta un [sección](http://www.w3.org/TR/html5/sections.html#the-section-element) elemento (que no se cierre automático y se escribe el contenido dentro de la `section` elemento). Por lo tanto, debe establecer `TagMode` a `StartTagAndEndTag` para escribir la salida. Como alternativa, puede convertir en comentario la línea donde se establece `TagMode` y escribir marcado con una etiqueta de cierre. (Marcado de ejemplo se proporciona más adelante en este tutorial.)
    
    * El `$` (signo de dólar) en la siguiente línea usa un [interpolan cadena](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings):
    
    ```cshtml
    $@"<ul><li><strong>Version:</strong> {Info.Version}</li>
    ```

4.  Agregue el siguiente marcado para la *About.cshtml* vista. El marcado resaltado muestra la información del sitio web.
    
    [!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?highlight=1,12-)]
    
    >[!NOTE]
    > En el marcado de Razor que se muestra a continuación:
    >
    >[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?range=13-17)]
    > 
    >Razor sepa el `info` atributo es una clase, no una cadena, y desea escribir código C#. Cualquier atributo de aplicación auxiliar de etiqueta no es una cadena debe escribirse sin el `@` caracteres.
    
5.  Ejecutar la aplicación y navegue hasta la vista acerca de para ver la información del sitio web.

    >[!NOTE]
    >Puede usar el siguiente marcado con una etiqueta de cierre y quite la línea con `TagMode.StartTagAndEndTag` en la aplicación auxiliar de etiqueta:
    >
    >[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutNotSelfClosing.cshtml?range=13-18)]

## <a name="condition-tag-helper"></a>Aplicación auxiliar de etiquetas de condición

La aplicación auxiliar de etiquetas de condición presenta la salida cuando se pasa un valor true.

1.  Agregue el siguiente `ConditionTagHelper` clase a la *TagHelpers* carpeta.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/ConditionTagHelper.cs)]

2.  Reemplace el contenido de la *Views/Home/Index.cshtml* archivo con el siguiente marcado:

    ```cshtml
    @using AuthoringTagHelpers.Models
    @model WebsiteContext
    
    @{
        ViewData["Title"] = "Home Page";
    }
    
    <div>
        <h3>Information about our website (outdated):</h3>
        <website-information info=Model />
        <div condition="Model.Approved">
            <p>
                This website has <strong surround="em"> @Model.Approved </strong> been approved yet.
                Visit www.contoso.com for more information.
            </p>
        </div>
    </div>
    ```
    
3.  Reemplace el `Index` método en el `Home` controlador con el código siguiente:

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Controllers/HomeController.cs?range=9-18)]

4.  Ejecutar la aplicación y vaya a la página principal. El marcado en el atributo conditional `div` no se representará. Anexar la cadena de consulta `?approved=true` a la dirección URL (por ejemplo, `http://localhost:1235/Home/Index?approved=true`). `approved`se establece en true y el atributo conditional aparecerá marcado.

>[!NOTE]
>Use la [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operador para especificar el atributo de destino en lugar de especificar una cadena como lo hizo con la aplicación auxiliar de etiqueta negrita:
>
>[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zConditionTagHelperCopy.cs?highlight=1,2,5&range=5-18)]
>
>El [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operador protegerá el código nunca se debería refactorizar (que queramos cambie el nombre a `RedCondition`).

### <a name="avoiding-tag-helper-conflicts"></a>Evitar conflictos de aplicación auxiliar de etiqueta

En esta sección, se escribe un par de aplicaciones auxiliares de etiquetas de la vinculación automática. La primera reemplazará el marcado que contiene una dirección URL a partir de HTTP a un HTML delimitador etiqueta que contiene la misma dirección URL (y, por tanto, lo que produce un vínculo a la dirección URL). El segundo se haga lo mismo para una dirección URL a partir de World Wide Web.

Dado que estas aplicaciones auxiliares de dos están estrechamente relacionados y puede refactorizar en el futuro, se podrá mantener en el mismo archivo.

1.  Agregue el siguiente `AutoLinkerHttpTagHelper` clase a la *TagHelpers* carpeta.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=7-19)]

    >[!NOTE]
    >El `AutoLinkerHttpTagHelper` clase destinos `p` elementos y se utiliza [Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) para crear el delimitador.

2.  Agregue el siguiente marcado al final de la *Views/Home/Contact.cshtml* archivo:

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=19)]

3.  Ejecutar la aplicación y compruebe que la aplicación auxiliar de la etiqueta representa el delimitador correctamente.

4.  Actualización de la `AutoLinker` clase para que incluya la `AutoLinkerWwwTagHelper` que convertirá el texto de World Wide Web en una etiqueta delimitadora que también contiene el texto original de World Wide Web. El código actualizado se resalta a continuación:

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?highlight=15-34&range=7-34)]

5.  Ejecute la aplicación. Observe el texto de World Wide Web se representa como un vínculo, pero el texto HTTP no está. Si coloca un punto de interrupción en ambas clases, puede ver que la clase de aplicación auxiliar de etiqueta HTTP se ejecuta primera. El problema es que se almacena en caché el resultado de la aplicación auxiliar de etiqueta y, cuando se ejecuta la aplicación auxiliar de etiqueta de World Wide Web, sobrescribe la salida almacenada en caché de la aplicación auxiliar de etiqueta HTTP. Más adelante en el tutorial veremos cómo controlar el orden en que se ejecutan aplicaciones auxiliares de etiquetas en. Se corregirá el código con lo siguiente:

    [!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10,21,22,26&range=8-37)]

    >[!NOTE]
    >En la primera edición de las aplicaciones auxiliares la vinculación automática de etiqueta, que obtuvo el contenido del destino con el código siguiente:
    >
    >[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=12)]
    >
    >Es decir, se llama a `GetChildContentAsync` mediante la `TagHelperOutput` pasará el `ProcessAsync` método. Como se mencionó anteriormente, porque el resultado se almacena en caché, la última etiqueta auxiliares para ejecutar wins. Había corregido el error con el código siguiente:
    >
    >[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?range=34-35)]
    >
    >El código anterior comprueba si se ha modificado el contenido y, si existe, obtiene el contenido del búfer de salida.

6.  Ejecute la aplicación y compruebe que los dos vínculos funcionan según lo esperado. Aunque podría parecer que nuestra herramienta de etiqueta del vinculador automática es correcta y completa, tiene un pequeño problema. Si la aplicación auxiliar de etiqueta de World Wide Web se ejecuta primera, los vínculos de World Wide Web no será correctos. Actualice el código y agregue el `Order` sobrecarga para controlar el orden en que la etiqueta se ejecuta en. El `Order` propiedad determina el orden de ejecución en relación con otras aplicaciones auxiliares de etiquetas como destino el mismo elemento. El valor de orden predeterminado es cero y se ejecutan en primer lugar instancias con valores más bajos.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?highlight=5,6,7,8&range=8-15)]
    
    El código anterior se garantizará que la aplicación auxiliar de etiqueta HTTP se ejecuta antes de la aplicación auxiliar de etiqueta de World Wide Web. Cambio `Order` a `MaxValue` y compruebe que el marcado generado para la etiqueta de World Wide Web es incorrecto.

## <a name="inspecting-and-retrieving-child-content"></a>Inspeccionar y recuperar el contenido secundario

Las aplicaciones auxiliares de etiquetas proporcionan varias propiedades para recuperar el contenido.

-  El resultado de `GetChildContentAsync` se pueden anexar a `output.Content`.
-  Puede inspeccionar el resultado de `GetChildContentAsync` con `GetContent`.
-  Si modifica `output.Content`, el cuerpo de TagHelper no se puede ejecutar o se representa a menos que llame a `GetChildContentAsync` igual que en nuestro ejemplo de vinculador automática:

[!code-csharp[Main](../../views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10&range=8-21)]

-  Varias llamadas a `GetChildContentAsync` devolverá el mismo valor y no se ejecute de nuevo el `TagHelper` cuerpo a menos que se sitúe en un parámetro false que indica no usar el resultado almacenado en caché.
