---
uid: mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
title: Crear aplicaciones auxiliares HTML personalizado (VB) | Documentos de Microsoft
author: microsoft
description: El objetivo de este tutorial es mostrar cómo puede crear aplicaciones auxiliares de HTML personalizado que puede usar en las vistas MVC. Aprovechando las ventajas de la aplicación auxiliar HTML...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: f96f4800-19ef-44c0-b457-55e777eb5de8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
msc.type: authoredcontent
ms.openlocfilehash: 6980026e2653eacb71697f9b34def9bc38638726
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="creating-custom-html-helpers-vb"></a>Crear aplicaciones auxiliares HTML personalizado (VB)
====================
por [Microsoft](https://github.com/microsoft)

[Descarga de PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_VB.pdf)

> El objetivo de este tutorial es mostrar cómo puede crear aplicaciones auxiliares de HTML personalizado que puede usar en las vistas MVC. Aprovechando las ventajas de las aplicaciones auxiliares HTML, puede reducir la cantidad de escritura de una tarea tediosa de etiquetas HTML que debe realizar para crear una página HTML estándar.


El objetivo de este tutorial es mostrar cómo puede crear aplicaciones auxiliares de HTML personalizado que puede usar en las vistas MVC. Aprovechando las ventajas de las aplicaciones auxiliares HTML, puede reducir la cantidad de escritura de una tarea tediosa de etiquetas HTML que debe realizar para crear una página HTML estándar.

En la primera parte de este tutorial, describen algunas de las aplicaciones auxiliares de HTML existente incluido con el marco de MVC de ASP.NET. A continuación, describen dos métodos de creación de aplicaciones auxiliares de HTML personalizado: explican cómo crear aplicaciones auxiliares HTML personalizadas mediante la creación de un método compartido y mediante la creación de un método de extensión.

## <a name="understanding-html-helpers"></a>Descripción de las aplicaciones auxiliares HTML

Una aplicación auxiliar de HTML es simplemente un método que devuelve una cadena. La cadena puede representar cualquier tipo de contenido que desee. Por ejemplo, puede usar aplicaciones auxiliares HTML para representar las etiquetas HTML estándar como HTML `<input>` y `<img>` etiquetas. También puede usar aplicaciones auxiliares HTML para representar el contenido más complejo, como una franja de pestañas o una tabla HTML de la base de datos.

El marco de ASP.NET MVC incluye el siguiente conjunto de aplicaciones auxiliares de HTML estándar (no es una lista completa):

- Html.ActionLink()
- Html.BeginForm()
- Html.CheckBox()
- Html.DropDownList()
- Html.EndForm()
- Html.Hidden()
- Html.ListBox()
- Html.Password()
- Html.RadioButton()
- Html.TextArea()
- Html.TextBox()

Por ejemplo, tenga en cuenta la forma en la lista 1. Este formulario se representa con la Ayuda de dos de las aplicaciones auxiliares de HTML estándar (consulte la figura 1). Este formulario usa el `Html.BeginForm()` y `Html.TextBox()` métodos auxiliares.


[![Representa la página con las aplicaciones auxiliares HTML](creating-custom-html-helpers-vb/_static/image2.png)](creating-custom-html-helpers-vb/_static/image1.png)

**Figura 01**: página se representa con aplicaciones auxiliares HTML ([haga clic aquí para ver la imagen a tamaño completo](creating-custom-html-helpers-vb/_static/image3.png))


**Lista 1: `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample1.aspx)]

El `Html.BeginForm()` método auxiliar se utiliza para crear el código HTML de apertura y cierre `<form>` etiquetas. Tenga en cuenta que el `Html.BeginForm()` se denomina método dentro de un uso de la instrucción. La instrucción using garantiza que la `<form>` etiqueta se cierra al final del uso de bloque.

Si lo prefiere, en lugar de crear un uso de bloque, puede llamar al método de aplicación auxiliar Html.EndForm() para cerrar la `<form>` etiqueta. Usar cualquier enfoque para la creación de apertura y cierre de `<form>` etiqueta que parezca más intuitivo.

El `Html.TextBox()` métodos auxiliares que se usan en el listado 1 para representar HTML `<input>` etiquetas. Si selecciona Ver código fuente en el explorador, a continuación, vea el código fuente HTML en el listado 2. Observe que el origen contiene etiquetas HTML estándar.

> [!IMPORTANT]
> Tenga en cuenta que la `Html.TextBox()`-HTML auxiliar se representa con `<%= %>` etiquetas en lugar de `<% %>` etiquetas. Si no incluye el signo igual, a continuación, nada se representa en el explorador.

El marco de MVC de ASP.NET contiene un pequeño conjunto de aplicaciones auxiliares. Probablemente, debe extender el marco MVC con las aplicaciones auxiliares HTML personalizado. En el resto de este tutorial, aprenderá dos métodos de creación de aplicaciones auxiliares de HTML personalizado.

**La lista 2: `Index.aspx Source`**

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-shared-methods"></a>Crear aplicaciones auxiliares HTML con métodos compartidos

Es la manera más fácil de crear una nueva aplicación auxiliar de HTML crear un método compartido que devuelve una cadena. Por ejemplo, imagine que decide crear una nueva aplicación auxiliar de HTML que representa un elemento HTML `<label>` etiqueta. Puede utilizar la clase en el listado 2 para representar un `<label>`.

**La lista 2: `Helpers\LabelHelper.vb`**

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample3.vb)]

No hay nada especial acerca de la clase en el listado 2. El `Label()` método simplemente devuelve una cadena.

La vista de índice modificada en el listado 3 utiliza el `LabelHelper` para representar HTML `<label>` etiquetas. Tenga en cuenta que la vista incluye una `<%@ imports %>` directiva que importa el espacio de nombres Application1.Helpers.

**La lista 2: `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a>Crear aplicaciones auxiliares HTML con los métodos de extensión

Si desea crear aplicaciones auxiliares de HTML que solo funcionan como las aplicaciones auxiliares de HTML estándar incluidos en el marco de MVC de ASP.NET, a continuación, debe crear métodos de extensión. Métodos de extensión permiten agregar nuevos métodos a una clase existente. Cuando se crea un método de aplicación auxiliar HTML, agregar nuevos métodos para la `HtmlHelper` clase representada por la propiedad de la vista Html.

El módulo de Visual Basic en el listado 3 agrega un método de extensión denominado `Label()` a la `HtmlHelper` clase. Hay un par de cosas que debe tener en cuenta acerca de este módulo. En primer lugar, tenga en cuenta que el módulo se decora con el `<Extension()>` atributo. Para poder usar este atributo, debe importar el `System.Runtime.CompilerServices` espacio de nombres

En segundo lugar, observe que el primer parámetro de la `Label()` método representa la `HtmlHelper` clase. El primer parámetro de un método de extensión indica la clase que extiende el método de extensión.

**Enumerar 3: `Helpers\LabelExtensions.vb`**

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample5.vb)]

Después de crear un método de extensión y compilar la aplicación correctamente, el método de extensión aparece en Visual Studio Intellisense al igual que todos los otros métodos de una clase (consulte la figura 2). La única diferencia es que dicha extensión se muestran métodos con un símbolo especial junto a ellos (un icono de flecha hacia abajo).


[![Mediante el método de extensión Html.Label()](creating-custom-html-helpers-vb/_static/image5.png)](creating-custom-html-helpers-vb/_static/image4.png)

**Figura 02**: mediante el método de extensión Html.Label() ([haga clic aquí para ver la imagen a tamaño completo](creating-custom-html-helpers-vb/_static/image6.png))


La vista de índice modificada en el listado 4 utiliza el método de extensión Html.Label() para representar todos sus &lt;etiqueta&gt; etiquetas.

**Enumerar 4: `Views\Home\Index3.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample6.aspx)]

## <a name="summary"></a>Resumen

En este tutorial, ha aprendido dos métodos de creación de aplicaciones auxiliares de HTML personalizado. En primer lugar, aprendió a crear una personalizada `Label()` aplicación auxiliar de HTML mediante la creación de un método compartido que devuelve una cadena. A continuación, ha aprendido cómo crear una personalizada `Label()` método de aplicación auxiliar HTML mediante la creación de un método de extensión en la `HtmlHelper` clase.

En este tutorial, centra en la creación de un método de aplicación auxiliar HTML muy simple. Tenga en cuenta que una aplicación auxiliar de HTML puede ser tan complicada como desee. Puede compilar aplicaciones auxiliares de HTML que representa el contenido enriquecido, como vistas de árbol, los menús o tablas de la base de datos.

> [!div class="step-by-step"]
> [Anterior](asp-net-mvc-views-overview-vb.md)
> [Siguiente](using-the-tagbuilder-class-to-build-html-helpers-vb.md)
