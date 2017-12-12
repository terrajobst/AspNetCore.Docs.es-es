---
uid: aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
title: Crear aplicaciones auxiliares HTML personalizado (C#) | Documentos de Microsoft
author: microsoft
description: "El objetivo de este tutorial es mostrar cómo puede crear aplicaciones auxiliares de HTML personalizado que puede usar en las vistas MVC. Aprovechando las ventajas de la aplicación auxiliar HTML..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: e454c67d-a86e-4119-a858-eb04bbec2dff
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: a0b6d67eb7aab51ba2b422fab0788e34255f2c8c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="creating-custom-html-helpers-c"></a>Crear aplicaciones auxiliares HTML personalizado (C#)
====================
por [Microsoft](https://github.com/microsoft)

[Descarga de PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_CS.pdf)

> El objetivo de este tutorial es mostrar cómo puede crear aplicaciones auxiliares de HTML personalizado que puede usar en las vistas MVC. Aprovechando las ventajas de las aplicaciones auxiliares HTML, puede reducir la cantidad de escritura de una tarea tediosa de etiquetas HTML que debe realizar para crear una página HTML estándar.


El objetivo de este tutorial es mostrar cómo puede crear aplicaciones auxiliares de HTML personalizado que puede usar en las vistas MVC. Aprovechando las ventajas de las aplicaciones auxiliares HTML, puede reducir la cantidad de escritura de una tarea tediosa de etiquetas HTML que debe realizar para crear una página HTML estándar.

En la primera parte de este tutorial, describen algunas de las aplicaciones auxiliares de HTML existente incluido con el marco de MVC de ASP.NET. A continuación, describen dos métodos de creación de aplicaciones auxiliares de HTML personalizado: explican cómo crear aplicaciones auxiliares HTML personalizadas mediante la creación de un método estático y mediante la creación de un método de extensión.

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

Por ejemplo, tenga en cuenta la forma en la lista 1. Este formulario se representa con la Ayuda de dos de las aplicaciones auxiliares de HTML estándar (consulte la figura 1). Este formulario usa el `Html.BeginForm()` y `Html.TextBox()` métodos auxiliares para representar un formulario HTML sencillo.


[![Representa la página con las aplicaciones auxiliares HTML](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)

**Figura 01**: página se representa con aplicaciones auxiliares HTML ([haga clic aquí para ver la imagen a tamaño completo](creating-custom-html-helpers-cs/_static/image3.png))


**Lista 1:`Views\Home\Index.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample1.aspx)]

El método auxiliar Html.BeginForm() se usa para crear el código HTML de apertura y cierre `<form>` etiquetas. Tenga en cuenta que el `Html.BeginForm()` se denomina método dentro de un uso de la instrucción. La instrucción using garantiza que la `<form>` etiqueta se cierra al final del uso de bloque.

Si lo prefiere, en lugar de crear un uso de bloque, puede llamar al método de aplicación auxiliar Html.EndForm() para cerrar la `<form>` etiqueta. Usar cualquier enfoque para la creación de apertura y cierre de `<form>` etiqueta que parezca más intuitivo.

El `Html.TextBox()` métodos auxiliares que se usan en el listado 1 para representar HTML `<input>` etiquetas. Si selecciona Ver código fuente en el explorador, a continuación, vea el código fuente HTML en el listado 2. Observe que el origen contiene etiquetas HTML estándar.

> [!IMPORTANT]
> Tenga en cuenta que la `Html.TextBox()`-HTML auxiliar se representa con `<%= %>` etiquetas en lugar de `<% %>` etiquetas. Si no incluye el signo igual, a continuación, nada se representa en el explorador.

El marco de MVC de ASP.NET contiene un pequeño conjunto de aplicaciones auxiliares. Probablemente, debe extender el marco MVC con las aplicaciones auxiliares HTML personalizado. En el resto de este tutorial, aprenderá dos métodos de creación de aplicaciones auxiliares de HTML personalizado.

**La lista 2:`Index.aspx Source`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-static-methods"></a>Crear aplicaciones auxiliares HTML con métodos estáticos

Es la manera más fácil de crear una nueva aplicación auxiliar de HTML crear un método estático que devuelve una cadena. Por ejemplo, imagine que decide crear una nueva aplicación auxiliar de HTML que representa un elemento HTML `<label>` etiqueta. Puede utilizar la clase en el listado 2 para representar un `<label>` .

**La lista 2:`Helpers\LabelHelper.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample3.cs)]

No hay nada especial acerca de la clase en el listado 2. El `Label()` método simplemente devuelve una cadena.

La vista de índice modificada en el listado 3 utiliza el `LabelHelper` para representar HTML `<label>` etiquetas. Tenga en cuenta que la vista incluye una `<%@ imports %>` directiva que importa el `Application1.Helpers` espacio de nombres.

**La lista 2:`Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a>Crear aplicaciones auxiliares HTML con los métodos de extensión

Si desea crear aplicaciones auxiliares de HTML que solo funcionan como las aplicaciones auxiliares de HTML estándar incluidos en el marco de MVC de ASP.NET, a continuación, debe crear métodos de extensión. Métodos de extensión permiten agregar nuevos métodos a una clase existente. Cuando se crea un método de aplicación auxiliar HTML, agregar nuevos métodos a la clase HtmlHelper representada por la propiedad de la vista Html.

La clase en la lista 3 agrega un método de extensión para la `HtmlHelper` clase denominada `Label()`. Hay un par de cosas que debe tener en cuenta acerca de esta clase. En primer lugar, tenga en cuenta que la clase es una clase estática. Debe definir un método de extensión con una clase estática.

En segundo lugar, observe que el primer parámetro de la `Label()` método va precedido por la palabra clave `this`. El primer parámetro de un método de extensión indica la clase que extiende el método de extensión.

**Enumerar 3:`Helpers\LabelExtensions.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample5.cs)]

Después de crear un método de extensión y compilar la aplicación correctamente, el método de extensión aparece en Visual Studio Intellisense al igual que todos los otros métodos de una clase (consulte la figura 2). La única diferencia es que dicha extensión se muestran métodos con un símbolo especial junto a ellos (un icono de flecha hacia abajo).


[![Mediante el método de extensión Html.Label()](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)

**Figura 02**: mediante el método de extensión Html.Label() ([haga clic aquí para ver la imagen a tamaño completo](creating-custom-html-helpers-cs/_static/image6.png))


La vista de índice modificada en el listado 4 utiliza el método de extensión Html.Label() para representar todos sus `<label>` etiquetas.

**Enumerar 4:`Views\Home\Index3.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample6.aspx)]

## <a name="summary"></a>Resumen

En este tutorial, ha aprendido dos métodos de creación de aplicaciones auxiliares de HTML personalizado. En primer lugar, aprendió a crear una personalizada `Label()` aplicación auxiliar de HTML mediante la creación de un método estático que devuelve una cadena. A continuación, ha aprendido cómo crear una personalizada `Label()` método de aplicación auxiliar HTML mediante la creación de un método de extensión en la `HtmlHelper` clase.

En este tutorial, centra en la creación de un método de aplicación auxiliar HTML muy simple. Tenga en cuenta que una aplicación auxiliar de HTML puede ser tan complicada como desee. Puede compilar aplicaciones auxiliares de HTML que representa el contenido enriquecido, como vistas de árbol, los menús o tablas de la base de datos.

>[!div class="step-by-step"]
[Anterior](asp-net-mvc-views-overview-cs.md)
[Siguiente](using-the-tagbuilder-class-to-build-html-helpers-cs.md)
