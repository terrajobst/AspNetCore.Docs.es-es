---
uid: web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-cs
title: Controlar la nomenclatura de Id. en las páginas de contenido (C#) | Microsoft Docs
author: rick-anderson
description: Ilustra cómo los controles ContentPlaceHolder actúan como un contenedor de nomenclatura y, por tanto, asegúrese de trabajar mediante programación con un control difícil (a través de FindConrol)...
ms.author: riande
ms.date: 06/10/2008
ms.assetid: 1c7d0916-0988-4b4f-9a03-935e4b5af6af
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 0c8617bb14c7023cfd926022b66c69bb5762758b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838476"
---
<a name="control-id-naming-in-content-pages-c"></a>Id. de control de nomenclatura en las páginas de contenido (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_05_CS.zip) o [descargar PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_05_CS.pdf)

> Muestra cómo los controles ContentPlaceHolder actúan como un contenedor de nomenclatura y, por tanto, asegúrese de trabajar mediante programación con un control difícil (a través de FindConrol). Examina este problema y soluciones alternativas. También se explica cómo obtener acceso mediante programación el valor de ClientID resultante.


## <a name="introduction"></a>Introducción

Todos los controles de servidor ASP.NET incluyen una `ID` propiedad que identifica el control y es el medio por el que el control mediante programación se accede en la clase de código subyacente. De forma similar, pueden incluir los elementos de un documento HTML una `id` atributo que identifica el elemento; estas `id` valores a menudo se usan en el script de cliente para hacer referencia a un elemento HTML determinado mediante programación. Por tanto, se puede suponer que cuando un control de servidor ASP.NET se representa en HTML, su `ID` valor se utiliza como el `id` valor del elemento HTML representado. Esto no es necesariamente el caso porque en determinadas circunstancias de un único control con una sola `ID` valor puede aparecer varias veces en el marcado representado. Considere la posibilidad de un control GridView que incluye un TemplateField con un control Web Label con una `ID` valor de ProductName. Cuando el control GridView se enlaza a su origen de datos en tiempo de ejecución, esta etiqueta se repite una vez para cada fila de GridView. Representan las necesidades de etiqueta única `id` valor.

Para administrar tales escenarios, ASP.NET permite ciertos controles se designa como contenedores de nomenclatura. Un contenedor de nomenclatura actúa como un nuevo `ID` espacio de nombres. Los controles de servidor que aparecen en el contenedor de nomenclatura tienen sus representado `id` valor prefijado con la `ID` del control de contenedor de nomenclatura. Por ejemplo, el `GridView` y `GridViewRow` clases son ambos contenedores de nomenclatura. Por lo tanto, un control de etiqueta definido en GridView TemplateField con `ID` ProductName tiene un representado `id` valor `GridViewID_GridViewRowID_ProductName`. Dado que *GridViewRowID* es único para cada fila GridView, resultante `id` valores sean únicos.

> [!NOTE]
> El [ `INamingContainer` interfaz](https://msdn.microsoft.com/library/system.web.ui.inamingcontainer.aspx) se utiliza para indicar que un determinado control de servidor ASP.NET debe funcionar como un contenedor de nomenclatura. El `INamingContainer` interfaz deletrear no los métodos que debe implementar el control de servidor; en su lugar, se usa como marcador. Para generar el marcado representado, si un control implementa esta interfaz, a continuación, el motor ASP.NET agrega automáticamente el prefijo su `ID` representa el valor a sus descendientes `id` valores de atributo. Este proceso se explica con más detalle en el paso 2.


Contenedores de nomenclatura no sólo cambian el texto representado `id` valor de atributo, pero también afecta a cómo el control puede hacer referencia mediante programación de la clase de código subyacente de la página ASP.NET. El `FindControl("controlID")` método se utiliza normalmente para hacer referencia a un control Web mediante programación. Sin embargo, `FindControl` no penetrar a través de contenedores de nomenclatura. Por lo tanto, no se puede usar directamente el `Page.FindControl` método para hacer referencia a los controles dentro de un control GridView o en otro contenedor de nomenclatura.

Como es posible que supuso, páginas maestras y ContentPlaceHolders ambos se implementan como contenedores de nomenclatura. En este tutorial se examina el elemento HTML de afectan a las páginas cómo maestro `id` valores y formas de hacer referencia mediante programación a los controles Web dentro de una página de contenido mediante `FindControl`.

## <a name="step-1-adding-a-new-aspnet-page"></a>Paso 1: Agregar una nueva página de ASP.NET

Para demostrar los conceptos tratados en este tutorial, vamos a agregar una nueva página ASP.NET a nuestro sitio Web. Crear una nueva página de contenido denominada `IDIssues.aspx` en la carpeta raíz, enlazarlo a la `Site.master` página maestra.


![Agregar el contenido IDIssues.aspx página a la carpeta raíz](control-id-naming-in-content-pages-cs/_static/image1.png)

**Figura 01**: agregar la página de contenido `IDIssues.aspx` a la carpeta raíz


Visual Studio crea automáticamente un control de contenido para cada uno de cuatro ContentPlaceHolders de la página maestra. Como se indicó en el [ *varios ContentPlaceHolders y contenido predeterminado* ](multiple-contentplaceholders-and-default-content-cs.md) tutorial, si un control de contenido no está presente el contenido de la página maestra predeterminada ContentPlaceHolder se genera en su lugar. Dado que el `QuickLoginUI` y `LeftColumnContent` ContentPlaceHolders contiene el marcado predeterminado adecuado para esta página, siga adelante y quitar sus correspondientes controles de contenido de `IDIssues.aspx`. En este momento, el marcado declarativo de la página de contenido debe ser similar al siguiente:


[!code-aspx[Main](control-id-naming-in-content-pages-cs/samples/sample1.aspx)]

En el [ *especificar el título, etiquetas Meta y otros encabezados HTML en la página maestra* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) tutorial hemos creado una clase de página base personalizada (`BasePage`) que configura automáticamente el título de la página si es no se establece explícitamente. Para el `IDIssues.aspx` paginarán al emplear esta funcionalidad, debe derivar la clase de código subyacente de la página de la `BasePage` clase (en lugar de `System.Web.UI.Page`). Modifique la definición de la clase de código subyacente para que tenga un aspecto similar al siguiente:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample2.cs)]

Por último, actualice el `Web.sitemap` archivo para incluir una entrada en esta lección de nuevo. Agregar un `<siteMapNode>` y establezca su `title` y `url` atributos a los "Problemas de nomenclatura de Id. de Control" y `~/IDIssues.aspx`, respectivamente. Después de realizar esta adición su `Web.sitemap` marcado del archivo debe ser similar al siguiente:


[!code-xml[Main](control-id-naming-in-content-pages-cs/samples/sample3.xml)]

Como se muestra en la figura 2, la nueva entrada de asignación de sitio en `Web.sitemap` se reflejan inmediatamente en la sección de lecciones en la columna izquierda.


![La sección lecciones ahora incluye un vínculo a &quot;problemas de nomenclatura de Id. de Control&quot;](control-id-naming-in-content-pages-cs/_static/image2.png)

**Figura 02**: la sección lecciones ahora incluye un vínculo a "Problemas de nomenclatura de Id. de Control"


## <a name="step-2-examining-the-renderedidchanges"></a>Paso 2: Examinar el representado`ID`cambios

Para comprender mejor las modificaciones ASP.NET motor realiza a la representada `id` controla los valores del servidor, vamos a agregar algunos controles Web a la `IDIssues.aspx` página y, a continuación, ver el marcado representado que se envía al explorador. En concreto, escriba el texto "Escriba su edad:" seguida de un control de cuadro de texto Web. Más abajo en la página Agregar un control Web Button y un control Web Label. Establezca el cuadro de texto `ID` y `Columns` propiedades a `Age` y 3, respectivamente. Establezca el botón `Text` y `ID` propiedades para "Enviar" y `SubmitButton`. Limpiar la etiqueta `Text` propiedad y establezca su `ID` a `Results`.

En este punto marcado declarativo del control de su contenido debe ser similar al siguiente:


[!code-aspx[Main](control-id-naming-in-content-pages-cs/samples/sample4.aspx)]

Figura 3 muestra la página cuando se ven a través del Diseñador de Visual Studio.


[![La página incluye tres controles Web: un cuadro de texto, botón y etiqueta](control-id-naming-in-content-pages-cs/_static/image4.png)](control-id-naming-in-content-pages-cs/_static/image3.png)

**Figura 03**: la página incluye tres controles Web: un cuadro de texto, un botón y una etiqueta ([haga clic aquí para ver imagen en tamaño completo](control-id-naming-in-content-pages-cs/_static/image5.png))


Visite la página a través de un explorador y, a continuación, ver el código fuente HTML. Como el marcado siguiente se muestra, el `id` valores de los elementos HTML para los controles de cuadro de texto, botón y etiqueta Web son una combinación de la `ID` los valores de los controles Web y la `ID` los valores de los contenedores de nomenclatura en la página.


[!code-html[Main](control-id-naming-in-content-pages-cs/samples/sample5.html)]

Como se indicó anteriormente en este tutorial, la página principal y su ContentPlaceHolders sirven como contenedores de nomenclatura. Por lo tanto, ambos contribuyen el representado `ID` valores de sus controles anidados. Tome el cuadro de texto `id` atributo, por ejemplo: `ctl00_MainContent_Age`. Recuerde que el control de cuadro de texto `ID` valor era `Age`. Esto va precedido de su control ContentPlaceHolder `ID` valor, `MainContent`. Además, este valor está prefijado con la página maestra `ID` valor, `ctl00`. El efecto neto es un `id` valor de atributo que consta de los `ID` valores de la página maestra, el control ContentPlaceHolder y el cuadro de texto.

Figura 4 ilustra este comportamiento. Para determinar el representado `id` de la `Age` TextBox, empiece con la `ID` valor del control de cuadro de texto, `Age`. A continuación, avance hacia la jerarquía de controles. En cada contenedor de nomenclatura (dichos nodos con un color color melocotón), el prefijo actual representan `id` con el contenedor de nomenclatura `id`.


![Los atributos de identificador representado están basado en el Id. de los valores de los contenedores de nomenclatura](control-id-naming-in-content-pages-cs/_static/image6.png)

**Figura 04**: representa el `id` atributos son basada en la `ID` los valores de los contenedores de nomenclatura


> [!NOTE]
> Como se explicó, el `ctl00` parte de la representado `id` atributo constituye el `ID` valor de la página maestra, pero quizás se pregunte cómo esta `ID` surgió el valor. No se ha especificado en cualquier lugar en nuestra página maestra o contenido. La mayoría de los controles de servidor en una página ASP.NET se agregan explícitamente a través de marcado declarativo de la página. El `MainContent` control ContentPlaceHolder especificó explícitamente en el marcado de `Site.master`; el `Age` se definió el cuadro de texto `IDIssues.aspx`del marcado. Podemos especificar la `ID` valores para estos tipos de controles a través de la ventana Propiedades o desde la sintaxis declarativa. Otros controles, como la página maestra, no se definen en el marcado declarativo. Por consiguiente, sus `ID` se deben generar automáticamente valores para nosotros. Los conjuntos de motor ASP.NET el `ID` valores en tiempo de ejecución para esos controles cuyos identificadores no se han establecido explícitamente. Usa el patrón de nomenclatura `ctlXX`, donde *XX* es un valor entero aumenta de forma secuencial.


Dado que la página maestra propio actúa como un contenedor de nomenclatura, los controles Web definidos en la página principal también han alterado representado `id` valores de atributo. Por ejemplo, el `DisplayDate` etiqueta se ha agregado a la página maestra en el [ *crear un diseño de todo el sitio con páginas maestras* ](creating-a-site-wide-layout-using-master-pages-cs.md) tutorial tiene las siguientes representan el marcado:


[!code-html[Main](control-id-naming-in-content-pages-cs/samples/sample6.html)]

Tenga en cuenta que el `id` atributo incluye tanto la página principal `ID` valor (`ctl00`) y el `ID` valor del control Web Label (`DateDisplay`).

## <a name="step-3-programmatically-referencing-web-controls-viafindcontrol"></a>Paso 3: Hacer referencia a los controles Web a través mediante programación`FindControl`

Cada control de servidor ASP.NET incluye un `FindControl("controlID")` método que busca en los descendientes del control para un control denominado *controlID*. Si se encuentra este tipo de control, se devuelve; Si no se encuentra ningún control coincidente, `FindControl` devuelve `null`.

`FindControl` es útil en escenarios donde necesita tener acceso a un control, pero no tiene una referencia directa a él. Cuando se trabaja con datos de los controles Web, como GridView, por ejemplo, los controles en los campos de GridView se definen una vez en la sintaxis declarativa, pero en tiempo de ejecución se crea una instancia del control para cada fila de GridView. Por lo tanto, los controles que se generan en tiempo de ejecución existen, pero no tenemos una referencia directa disponible desde la clase de código subyacente. Como resultado, es necesario usar `FindControl` para trabajar mediante programación con un control específico en los campos de GridView. (Para obtener más información sobre el uso de `FindControl` para tener acceso a los controles de plantillas del control de Web de datos, vea [formato basado en datos personalizados](../../data-access/custom-formatting/custom-formatting-based-upon-data-cs.md).) Este mismo escenario se produce cuando se agrega dinámicamente controles Web a un formulario Web Forms, un tema se describe en [crear Interfaces de usuario de entrada de Dynamic Data](https://msdn.microsoft.com/library/aa479330.aspx).

Para ilustrar el uso de la `FindControl` método para buscar los controles dentro de una página de contenido, cree un controlador de eventos para el `SubmitButton`del `Click` eventos. En el controlador de eventos, agregue el código siguiente, que hace referencia a mediante programación el `Age` TextBox y `Results` etiqueta mediante el `FindControl` método y, a continuación, muestra un mensaje en `Results` basándose en la entrada del usuario.

> [!NOTE]
> Por supuesto, no es necesario usar `FindControl` para hacer referencia a los controles Label y TextBox para este ejemplo. Nos podríamos hacer referencia a ellos directamente a través de sus `ID` los valores de propiedad. Usar `FindControl` aquí para ilustrar lo que ocurre cuando se usa `FindControl` desde una página de contenido.


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample7.cs)]

Aunque la sintaxis usada para llamar a la `FindControl` método difiere ligeramente de las dos primeras líneas de `SubmitButton_Click`, son semánticamente equivalentes. Recuerde que todos los controles de servidor ASP.NET incluyen un `FindControl` método. Esto incluye la `Page` (clase), desde qué ASP.NET todas las clases de código subyacente deben derivar de. Por lo tanto, una llamada a `FindControl("controlID")` equivale a llamar a `Page.FindControl("controlID")`, suponiendo que aún no se reemplaza el `FindControl` método en la clase de código subyacente o en una clase base personalizada.

Después de escribir este código, visite la `IDIssues.aspx` página a través de un explorador, escriba su edad y haga clic en el botón "Enviar". Al hacer clic en el botón "Enviar" un `NullReferenceException` se genera (consulte la figura 5).


[![Se produce una excepción NullReferenceException](control-id-naming-in-content-pages-cs/_static/image8.png)](control-id-naming-in-content-pages-cs/_static/image7.png)

**Figura 05**: un `NullReferenceException` se genera ([haga clic aquí para ver imagen en tamaño completo](control-id-naming-in-content-pages-cs/_static/image9.png))


Si establece un punto de interrupción el `SubmitButton_Click` controlador de eventos, verá que tanto las llamadas a `FindControl` devolver un `null` valor. El `NullReferenceException` se genera cuando se intenta tener acceso a la `Age` del cuadro de texto `Text` propiedad.

El problema es que `Control.FindControl` sólo busca *Control*de descendientes que son *en el mismo contenedor de nomenclatura*. Dado que la página maestra constituye un nuevo contenedor de nomenclatura, una llamada a `Page.FindControl("controlID")` no impregna nunca el objeto de página maestra `ctl00`. (Hacer referencia a la figura 4 para ver la jerarquía de controles, que muestra la `Page` objeto como elemento primario del objeto de página maestra `ctl00`.) Por lo tanto, el `Results` etiqueta y `Age` no se encuentra el cuadro de texto y `ResultsLabel` y `AgeTextBox` se asignan los valores `null`.

Hay dos soluciones alternativas a este desafío: se puede explorar en profundidad, un contenedor de nomenclatura a la vez, el control adecuado; o podemos crear nuestros propio `FindControl` método que hace posible la nomenclatura de contenedores. Examinemos cada una de estas opciones.

### <a name="drilling-into-the-appropriate-naming-container"></a>Obtención de detalles en el contenedor de nomenclatura adecuada

Para usar `FindControl` para hacer referencia a la `Results` etiqueta o `Age` cuadro de texto, es necesario llamar a `FindControl` desde un control de antecesor en el mismo contenedor de nomenclatura. Como se muestra en la figura 4, el `MainContent` control ContentPlaceHolder es el antecesor solo de `Results` o `Age` que está dentro del mismo contenedor de nomenclatura. En otras palabras, una llamada a la `FindControl` método desde el `MainContent` control, como se muestra en el fragmento de código siguiente, devuelve correctamente una referencia a la `Results` o `Age` controles.


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample8.cs)]

Sin embargo, no podemos trabajamos con los `MainContent` ContentPlaceHolder de la clase de código subyacente de nuestra página de contenido mediante la sintaxis anterior porque el control ContentPlaceHolder está definido en la página maestra. En su lugar, debemos usar `FindControl` para obtener una referencia a `MainContent`. Reemplace el código en el `SubmitButton_Click` controlador de eventos con las siguientes modificaciones:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample9.cs)]

Si visita la página a través de un explorador, escriba su edad y haga clic en el botón "Enviar", un `NullReferenceException` se genera. Si establece un punto de interrupción el `SubmitButton_Click` controlador de eventos, verá que esta excepción se produce cuando se intenta llamar a la `MainContent` del objeto `FindControl` método. El `MainContent` objeto es `null` porque el `FindControl` método no puede encontrar un objeto denominado "MainContent". El motivo subyacente es el mismo que con la `Results` etiqueta y `Age` controles TextBox: `FindControl` su búsqueda se inicia desde la parte superior de la jerarquía de controles y no penetrar en contenedores de nomenclatura, pero la `MainContent` ContentPlaceHolder es en la página maestra, que es un contenedor de nomenclatura.

Para poder usar `FindControl` para obtener una referencia a `MainContent`, primero necesitamos una referencia al control de la página maestra. Una vez que tenemos una referencia a la página maestra podemos obtener una referencia a la `MainContent` ContentPlaceHolder a través de `FindControl` y, a partir de ahí, las referencias a la `Results` etiqueta y `Age` TextBox (nuevamente, a través del uso de `FindControl`). ¿Pero cómo obtenemos una referencia a la página maestra? Inspeccionando el `id` atributos en el marcado representado es evidente que la página maestra `ID` valor es `ctl00`. Por lo tanto, podríamos usar `Page.FindControl("ctl00")` para obtener una referencia a la página maestra, a continuación, usar ese objeto para obtener una referencia a `MainContent`, y así sucesivamente. El fragmento de código siguiente ilustra esta lógica:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample10.cs)]

Aunque este código funcionará sin duda, se presupone que generado automáticamente de la página maestra `ID` siempre será `ctl00`. Nunca es una buena idea realizar suposiciones sobre los valores generados automáticamente.

Afortunadamente, una referencia a la página maestra es accesible a través de la `Page` la clase `Master` propiedad. Por lo tanto, en lugar de tener que usar `FindControl("ctl00")` para obtener una referencia de la página maestra con el fin de obtener acceso a la `MainContent` ContentPlaceHolder, podemos usar en su lugar `Page.Master.FindControl("MainContent")`. Actualización de la `SubmitButton_Click` controlador de eventos con el código siguiente:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample11.cs)]

Esta vez, visite la página a través de un explorador, escribir tu edad y haga clic en el botón "Enviar" muestra el mensaje en el `Results` etiquetar, según lo previsto.


[![La edad del usuario se muestra en la etiqueta](control-id-naming-in-content-pages-cs/_static/image11.png)](control-id-naming-in-content-pages-cs/_static/image10.png)

**Figura 06**: edad del usuario se muestra en la etiqueta ([haga clic aquí para ver imagen en tamaño completo](control-id-naming-in-content-pages-cs/_static/image12.png))


### <a name="recursively-searching-through-naming-containers"></a>Buscar a través de contenedores de nomenclatura de forma recursiva

El motivo por el ejemplo de código anterior hace referencia a la `MainContent` control ContentPlaceHolder desde la página maestra y, a continuación, el `Results` etiqueta y `Age` controles TextBox `MainContent`, es porque el `Control.FindControl` solo busca el método dentro de *Control*del contenedor de nomenclatura. Tener `FindControl` permanezca dentro del contenedor de nomenclatura que tiene sentido en la mayoría de los escenarios, porque los dos controles en dos contenedores de nomenclatura diferentes pueden tener el mismo `ID` valores. Considere el caso de un control GridView que define un control etiqueta Web denominado `ProductName` dentro de una de sus TemplateFields. Cuando se enlazan los datos en el control GridView en tiempo de ejecución, un `ProductName` etiqueta se crea para cada fila de GridView. ¿Si `FindControl` buscando nombres de todos los contenedores y se denomina `Page.FindControl("ProductName")`, qué instancia de la etiqueta debe el `FindControl` devolver? ¿El `ProductName` etiqueta en la primera fila GridView? ¿El que aparece en la última fila?

Por lo que tener `Control.FindControl` simplemente buscar *Control*de nomenclatura de contenedor tiene sentido en la mayoría de los casos. Sin embargo, hay otros casos, como el que nos encontramos, si se dispone de un único `ID` en todos los contenedores de nomenclatura y desea evitar la necesidad de hacer referencia meticulosamente cada contenedor de nomenclatura en la jerarquía de controles a un control de acceso. Tener un `FindControl` variante que busca de forma recursiva todos los contenedores de nomenclatura hace que sea coherente, demasiado. Desafortunadamente, .NET Framework no incluye este tipo de método.

La buena noticia es que podemos crear nuestros propio `FindControl` método busca en ese recursivamente todos los contenedores de nomenclatura. De hecho, usar *métodos de extensión* nos podemos agregamos un `FindControlRecursive` método a la `Control` clase para acompañar su existente `FindControl` método.

> [!NOTE]
> Métodos de extensión son una característica nueva en C# 3.0 y Visual Basic 9, que son los idiomas que se suministran con .NET Framework versión 3.5 y Visual Studio 2008. En resumen, los métodos de extensión permiten para que un desarrollador crear un nuevo método para un tipo de clase existente a través de una sintaxis especial. Para obtener más información sobre esta característica útil, consulte mi artículo [extender la funcionalidad de tipo de Base con métodos de extensión](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx).


Para crear el método de extensión, agregue un nuevo archivo a la `App_Code` carpeta denominada `PageExtensionMethods.cs`. Agregar un método de extensión denominado `FindControlRecursive` que toma como entrada un `string` parámetro denominado `controlID`. Para que los métodos de extensión para que funcione correctamente, es fundamental que la propia clase y sus métodos de extensión se marquen `static`. Además, deben aceptar todos los métodos de extensión como su primer parámetro un objeto del tipo al que se aplica el método de extensión y este parámetro de entrada deben ir precedidas por la palabra clave `this`.

Agregue el código siguiente a la `PageExtensionMethods.cs` archivo de clase para definir esta clase y el `FindControlRecursive` método de extensión:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample12.cs)]

Con este código en su lugar, volver a la `IDIssues.aspx` comentario actual y clase de código subyacente de la página `FindControl` llamadas al método. Sustitúyalas por llamadas a `Page.FindControlRecursive("controlID")`. ¿Qué es clara sobre los métodos de extensión es que aparecen directamente en las listas de la lista desplegable de IntelliSense. Como se muestra en la figura 7, cuando escriba página y, a continuación, presione período, el `FindControlRecursive` método está incluido en la lista desplegable, junto con el otro IntelliSense `Control` métodos de la clase.


[![Métodos de extensión se incluyen en las desplegables de IntelliSense](control-id-naming-in-content-pages-cs/_static/image14.png)](control-id-naming-in-content-pages-cs/_static/image13.png)

**Figura 07**: métodos de extensión se incluyen en IntelliSense listas desplegables ([haga clic aquí para ver imagen en tamaño completo](control-id-naming-in-content-pages-cs/_static/image15.png))


Escriba el código siguiente en el `SubmitButton_Click` controlador de eventos y, a continuación, pruébela al visitar la página, introduzca la edad del usuario y al hacer clic en el botón "Enviar". Como se muestra en la figura 6, el resultado será el mensaje, "Son años de edad!"


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample13.cs)]

> [!NOTE]
> Dado que los métodos de extensión están familiarizados con C# 3.0 y Visual Basic 9, si usa Visual Studio 2005 no se puede utilizar métodos de extensión. En su lugar, deberá implementar la `FindControlRecursive` método en una clase auxiliar. [Rick Strahl](http://www.west-wind.com/WebLog/default.aspx) tiene un ejemplo en su blog [maestra las páginas ASP.NET y `FindControl` ](http://www.west-wind.com/WebLog/posts/5127.aspx).


## <a name="step-4-using-the-correctidattribute-value-in-client-side-script"></a>Paso 4: El uso correcto`id`atributo valor en el Script de cliente

Como se indica en Introducción de este tutorial, del representa un control Web `id` atributo se utiliza a menudo en el script de cliente para hacer referencia a un elemento HTML determinado mediante programación. Por ejemplo, el siguiente código de JavaScript hace referencia a un elemento HTML por su `id` y, a continuación, muestra su valor en un cuadro de mensaje modal:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample14.cs)]

Recuerde que en ASP.NET las páginas que no incluya un contenedor de nomenclatura, el elemento HTML presentado `id` atributo es idéntico del control Web `ID` valor de propiedad. Por este motivo, resulta tentador codificar de forma rígida en `id` valores de atributo en el código de JavaScript. Es decir, si sabe que desea tener acceso el `Age` control de cuadro de texto Web a través del script del lado cliente, hacerlo a través de una llamada a `document.getElementById("Age")`.

El problema con este enfoque es que al utilizar las páginas maestras (o en otros controles del contenedor de nomenclatura), el HTML representado `id` no es sinónimo con el control Web `ID` propiedad. Puede ser la primera tendencia visite la página a través de un explorador y ver el código fuente para determinar el estado real `id` atributo. Una vez que sepa el representado `id` valor, puede pegarlo en la llamada a `getElementById` para acceder al elemento HTML que necesita para trabajar con a través del script del lado cliente. Este enfoque es ideal, ya que ciertos cambios en la página de la jerarquía de controles o cambios en el `ID` alteran las propiedades de los controles de nomenclatura resultante `id` atributo, lo que interrumpe su código de JavaScript.

La buena noticia es que la `id` valor del atributo que se presenta es accesible en el código del lado servidor a través del control Web [ `ClientID` propiedad](https://msdn.microsoft.com/library/system.web.ui.control.clientid.aspx). Debe utilizar esta propiedad para determinar el `id` atributo valor usado en el script de cliente. Por ejemplo, para agregar una función JavaScript a la página que, cuando se llama, muestra el valor de la `Age` cuadro de texto en un cuadro de mensaje modal, agregue el código siguiente a la `Page_Load` controlador de eventos:


[!code-javascript[Main](control-id-naming-in-content-pages-cs/samples/sample15.js)]

El código anterior inserta el valor de la `Age` propiedad ClientID del cuadro de texto a la llamada de JavaScript para `getElementById`. Si visita esta página a través de un explorador y ver el código fuente HTML, encontrará el código JavaScript siguiente:


[!code-html[Main](control-id-naming-in-content-pages-cs/samples/sample16.html)]

Observe cómo el valor correcto `id` valor de atributo, `ctl00_MainContent_Age`, aparece dentro de la llamada a `getElementById`. Dado que este valor se calcula en tiempo de ejecución, funciona independientemente de los cambios posteriores en la jerarquía de controles de página.

> [!NOTE]
> En este ejemplo de JavaScript simplemente se muestra cómo agregar una función JavaScript que hace referencia correctamente el elemento HTML representado por un control de servidor. Para usar esta función necesitaría crear código JavaScript para llamar a la función cuando se carga el documento o cuando ocurre alguna acción específica del usuario. Para obtener más información sobre estos y otros temas relacionados, leer [trabajar con el Script de cliente](https://msdn.microsoft.com/library/aa479302.aspx).


## <a name="summary"></a>Resumen

Determinados controles de servidor ASP.NET actúan como contenedores de nomenclatura, lo que afecta a la representada `id` atributo valores de sus controles descendientes, así como el ámbito de los controles canvassed por el `FindControl` método. Con respecto a las páginas maestras, la propia página maestra y sus controles ContentPlaceHolder son contenedores de nomenclatura. Por lo tanto, es preciso poner incluir detalla un poco más trabajo para hacer referencia mediante programación a los controles dentro de la página de contenido mediante `FindControl`. En este tutorial se examinan dos técnicas: obtención de detalles en el control ContentPlaceHolder y que realiza la llamada su `FindControl` método; y el lanzamiento de nuestro propio `FindControl` implementación de forma recursiva que busca en todos los contenedores de nomenclatura.

Además de los problemas de lado servidor introducen contenedores de nomenclatura con respecto a la que hacen referencia a los controles Web, también hay problemas de lado cliente. En ausencia de contenedores de nomenclatura, el control Web de `ID` valor de propiedad y representan `id` valor de atributo son iguales. Pero con la adición de contenedor de nomenclatura, el texto representado `id` atributo incluye tanto el `ID` valores del control Web y los contenedores de nomenclatura en ascendencia de su jerarquía de controles. Estas cuestiones de nomenclatura son un problema siempre y cuando utilice el control Web `ClientID` propiedad para determinar el representado `id` atributo valor en el script del lado cliente.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Páginas maestras en ASP.NET y `FindControl`](http://www.west-wind.com/WebLog/posts/5127.aspx)
- [Crear Interfaces de usuario de entrada de datos dinámicos](https://msdn.microsoft.com/library/aa479330.aspx)
- [Extender la funcionalidad de tipo Base con métodos de extensión](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx)
- [Cómo: Hacer referencia a contenido de la página maestra ASP.NET](https://msdn.microsoft.com/library/xxwa0ff0.aspx)
- [Páginas maestro: Sugerencias, trucos y capturas](http://www.odetocode.com/articles/450.aspx)
- [Trabajar con el Script de cliente](https://msdn.microsoft.com/library/aa479302.aspx)

### <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de varios libros sobre ASP/ASP.NET y fundador de 4GuysFromRolla.com, ha estado trabajando con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [ *Sams Teach usted mismo ASP.NET 3.5 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) o a través de su blog en [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Los revisores para este tutorial fueron Zack Jones y Suchi Barnerjee. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Anterior](urls-in-master-pages-cs.md)
> [Siguiente](interacting-with-the-master-page-from-the-content-page-cs.md)
