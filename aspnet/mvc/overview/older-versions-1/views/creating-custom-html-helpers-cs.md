---
uid: aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
title: Creación de aplicaciones auxiliares HTML personalizadas (C#) | Microsoft Docs
author: microsoft
description: El objetivo de este tutorial es mostrar cómo puede crear aplicaciones auxiliares de HTML personalizado que puede usar en las vistas MVC. Aprovechando las ventajas de la aplicación auxiliar HTML...
ms.author: aspnetcontent
ms.date: 10/07/2008
ms.assetid: e454c67d-a86e-4119-a858-eb04bbec2dff
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: 0606ec3b5595fbe73918b82e32b393871e8533a1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839414"
---
<a name="creating-custom-html-helpers-c"></a>Creación de aplicaciones auxiliares HTML personalizadas (C#)
====================
por [Microsoft](https://github.com/microsoft)

[Descargar PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_CS.pdf)

> El objetivo de este tutorial es mostrar cómo puede crear aplicaciones auxiliares de HTML personalizado que puede usar en las vistas MVC. Aprovechando las ventajas de las aplicaciones auxiliares HTML, puede reducir la cantidad de una mecanografía tediosa de etiquetas HTML que debe realizar para crear una página HTML estándar.


El objetivo de este tutorial es mostrar cómo puede crear aplicaciones auxiliares de HTML personalizado que puede usar en las vistas MVC. Aprovechando las ventajas de las aplicaciones auxiliares HTML, puede reducir la cantidad de una mecanografía tediosa de etiquetas HTML que debe realizar para crear una página HTML estándar.

En la primera parte de este tutorial, describen algunas de las aplicaciones auxiliares de HTML existentes incluidos con el marco de MVC de ASP.NET. A continuación, describen dos métodos para crear aplicaciones auxiliares de HTML personalizado: explica cómo crear aplicaciones auxiliares de HTML personalizado mediante la creación de un método estático y mediante la creación de un método de extensión.

## <a name="understanding-html-helpers"></a>Descripción de las aplicaciones auxiliares HTML

Una aplicación auxiliar HTML es simplemente un método que devuelve una cadena. La cadena puede representar cualquier tipo de contenido que desee. Por ejemplo, puede usar aplicaciones auxiliares HTML para representar las etiquetas HTML estándar como HTML `<input>` y `<img>` etiquetas. También puede usar aplicaciones auxiliares HTML para representar el contenido más complejo, como una tabla HTML de la base de datos o de una franja de pestañas.

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

Por ejemplo, considere el formulario en el listado 1. Este formulario se representa con la Ayuda de dos de las aplicaciones auxiliares de HTML estándar (consulte la figura 1). Este formulario utiliza la `Html.BeginForm()` y `Html.TextBox()` métodos auxiliares para representar un formulario HTML sencillo.


[![Representa la página con las aplicaciones auxiliares HTML](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)

**Figura 01**: representa la página con las aplicaciones auxiliares HTML ([haga clic aquí para ver imagen en tamaño completo](creating-custom-html-helpers-cs/_static/image3.png))


**Listado 1: `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample1.aspx)]

El método auxiliar Html.BeginForm() se usa para crear el código HTML de apertura y cierre `<form>` etiquetas. Tenga en cuenta que el `Html.BeginForm()` se llama al método dentro de un uso de la instrucción. La instrucción using garantiza que el `<form>` etiqueta a la que se cierra al final del uso de los bloques.

Si lo prefiere, en lugar de crear el uso de un bloque, se puede llamar al método auxiliar Html.EndForm() para cerrar el `<form>` etiqueta. Utilice el método de creación de apertura y cierre `<form>` etiqueta que se parece más intuitiva.

El `Html.TextBox()` métodos auxiliares se utilizan en el listado 1 para representar HTML `<input>` etiquetas. Si selecciona Ver código fuente en el explorador, a continuación, verá el código fuente HTML en el listado 2. Tenga en cuenta que el origen contiene etiquetas HTML estándar.

> [!IMPORTANT]
> Tenga en cuenta que el `Html.TextBox()`-HTML auxiliar se presenta con `<%= %>` etiquetas en lugar de `<% %>` etiquetas. Si no incluye el signo igual, nada obtiene representa en el explorador.

El marco ASP.NET MVC contiene un pequeño conjunto de aplicaciones auxiliares. Probablemente, necesitará ampliar el marco de MVC con las aplicaciones auxiliares de HTML personalizado. En el resto de este tutorial, aprenderá dos métodos para crear aplicaciones auxiliares de HTML personalizado.

**Listado 2: `Index.aspx Source`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-static-methods"></a>Crear aplicaciones auxiliares HTML con métodos estáticos

Es la manera más fácil de crear una nueva aplicación auxiliar de HTML crear un método estático que devuelve una cadena. Por ejemplo, imagine que se decide crear una nueva aplicación auxiliar HTML que representa un elemento HTML `<label>` etiqueta. Puede usar la clase en el listado 2 para representar un `<label>` .

**Listado 2: `Helpers\LabelHelper.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample3.cs)]

No hay nada especial acerca de la clase en el listado 2. El `Label()` método simplemente devuelve una cadena.

La vista de índice modificada en el listado 3 utiliza el `LabelHelper` para representar HTML `<label>` etiquetas. Tenga en cuenta que la vista incluye un `<%@ imports %>` directiva que importa el `Application1.Helpers` espacio de nombres.

**Listado 2: `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a>Crear aplicaciones auxiliares HTML con los métodos de extensión

Si desea crear aplicaciones auxiliares de HTML que simplemente funcionan como las aplicaciones auxiliares de HTML estándar incluidos en el marco de ASP.NET MVC, deberá crear métodos de extensión. Métodos de extensión permiten agregar nuevos métodos a una clase existente. Al crear un método de aplicación auxiliar HTML, agregar nuevos métodos a la clase HtmlHelper representada por la propiedad de una vista Html.

La clase en el listado 3 agrega un método de extensión para el `HtmlHelper` clase denominada `Label()`. Hay un par de cosas que debe tener en cuenta acerca de esta clase. En primer lugar, tenga en cuenta que la clase es una clase estática. Debe definir un método de extensión con una clase estática.

En segundo lugar, tenga en cuenta que el primer parámetro de la `Label()` método va precedido por la palabra clave `this`. El primer parámetro de un método de extensión indica la clase que extiende el método de extensión.

**Listado 3: `Helpers\LabelExtensions.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample5.cs)]

Después de crear un método de extensión y compilar la aplicación correctamente, el método de extensión aparece en Intellisense en Visual Studio, como todos los otros métodos de una clase (consulte la figura 2). La única diferencia es que dicha extensión se muestran métodos con un símbolo especial junto a ellos (un icono de una flecha hacia abajo).


[![Mediante el método de extensión Html.Label()](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)

**Figura 02**: mediante el método de extensión Html.Label() ([haga clic aquí para ver imagen en tamaño completo](creating-custom-html-helpers-cs/_static/image6.png))


La vista de índice modificada en el listado 4 usa el método de extensión Html.Label() para representar todos sus `<label>` etiquetas.

**Listado 4: `Views\Home\Index3.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample6.aspx)]

## <a name="summary"></a>Resumen

En este tutorial, ha aprendido a dos métodos para crear aplicaciones auxiliares de HTML personalizado. En primer lugar, ha aprendido a crear un personalizado `Label()` aplicación auxiliar HTML mediante la creación de un método estático que devuelve una cadena. A continuación, ha aprendido a crear un personalizado `Label()` método de aplicación auxiliar HTML mediante la creación de un método de extensión en el `HtmlHelper` clase.

En este tutorial, me centré en la creación de un método muy sencillo de aplicación auxiliar HTML. Tenga en cuenta que una aplicación auxiliar HTML puede ser tan complicada como desee. Puede compilar aplicaciones auxiliares de HTML que presentan contenido enriquecido, como vistas de árbol, los menús o tablas de la base de datos.

> [!div class="step-by-step"]
> [Anterior](asp-net-mvc-views-overview-cs.md)
> [Siguiente](using-the-tagbuilder-class-to-build-html-helpers-cs.md)
