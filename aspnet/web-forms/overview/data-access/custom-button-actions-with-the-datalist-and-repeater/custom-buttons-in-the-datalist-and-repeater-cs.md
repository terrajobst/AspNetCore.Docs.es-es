---
uid: web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-cs
title: Botones personalizados en los controles DataList y Repeater (C#) | Microsoft Docs
author: rick-anderson
description: En este tutorial vamos a crear una interfaz que usa un control Repeater para mostrar las categorías en el sistema, y cada categoría de proporcionar un botón para mostrar su associ...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2006
ms.topic: article
ms.assetid: 1f42e332-78dc-438b-9e35-0c97aa0ad929
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-cs
msc.type: authoredcontent
ms.openlocfilehash: 802f52e8e4e1ca1addec3321503ac92474ffd6b8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369491"
---
<a name="custom-buttons-in-the-datalist-and-repeater-c"></a>Botones personalizados en los controles DataList y Repeater (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_46_CS.exe) o [descargar PDF](custom-buttons-in-the-datalist-and-repeater-cs/_static/datatutorial46cs1.pdf)

> En este tutorial vamos a crear una interfaz que usa un control Repeater para mostrar las categorías en el sistema, y cada categoría de proporcionar un botón para mostrar sus productos asociados con un control BulletedList.


## <a name="introduction"></a>Introducción

A lo largo de los últimos tutoriales diecisiete DataList y Repeater, se ve crea los ejemplos de solo lectura y edición y eliminación de ejemplos. Para facilitar la edición y eliminación de capacidades dentro de un control DataList, agregamos botones al control DataList s `ItemTemplate` que, al hacer clic, produjo una devolución de datos y se genera un evento de DataList correspondiente al botón s `CommandName` propiedad. Por ejemplo, al agregar un botón para el `ItemTemplate` con un `CommandName` DataList s hace que el valor de propiedad de edición `EditCommand` para desencadenar en postback; una con el `CommandName` eliminar genera el `DeleteCommand`.

Además para editar y eliminar botones, los controles DataList y Repeater pueden incluir también los botones, LinkButtons o ImageButtons que, al hacer clic, realizar alguna lógica personalizada de servidor. En este tutorial vamos a crear una interfaz que usa un control Repeater para mostrar las categorías en el sistema. Para cada categoría, el control Repeater incluirá un botón para mostrar la categoría de productos asociada con un control BulletedList (consulte la figura 1).


[![Al hacer clic en el vínculo Mostrar productos se muestra los productos de s de categoría en una lista con viñetas](custom-buttons-in-the-datalist-and-repeater-cs/_static/image2.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image1.png)

**Figura 1**: al hacer clic en la muestra de vínculo Mostrar productos de la categoría de productos de s en una lista con viñetas ([haga clic aquí para ver imagen en tamaño completo](custom-buttons-in-the-datalist-and-repeater-cs/_static/image3.png))


## <a name="step-1-adding-the-custom-button-tutorial-web-pages"></a>Paso 1: Agregar las páginas Web del Tutorial botón personalizado

Antes de adentrarnos en cómo agregar un botón personalizado, permiten s primero dedique un momento para crear las páginas ASP.NET en nuestro proyecto de sitio Web que necesitaremos para este tutorial. Empiece por agregar una nueva carpeta denominada `CustomButtonsDataListRepeater`. A continuación, agregue las siguientes dos páginas ASP.NET a la carpeta y asegúrese de asociar cada página con el `Site.master` página maestra:

- `Default.aspx`
- `CustomButtons.aspx`


![Agregar las páginas ASP.NET para los tutoriales relacionados con los botones personalizados](custom-buttons-in-the-datalist-and-repeater-cs/_static/image4.png)

**Figura 2**: agregar las páginas ASP.NET para los tutoriales relacionados con los botones personalizados


Al igual que en las demás carpetas `Default.aspx` en el `CustomButtonsDataListRepeater` carpeta mostrará una lista de los tutoriales en su sección. Recuerde que el `SectionLevelTutorialListing.ascx` Control de usuario proporciona esta funcionalidad. Agregar este Control de usuario a `Default.aspx` arrastrándolo desde el Explorador de soluciones en la página de vista de diseño de s.


[![Agregar el Control de usuario SectionLevelTutorialListing.ascx a Default.aspx](custom-buttons-in-the-datalist-and-repeater-cs/_static/image6.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image5.png)

**Figura 3**: agregar el `SectionLevelTutorialListing.ascx` Control de usuario `Default.aspx` ([haga clic aquí para ver imagen en tamaño completo](custom-buttons-in-the-datalist-and-repeater-cs/_static/image7.png))


Por último, agregue las páginas como entradas para el `Web.sitemap` archivo. En concreto, agregue el siguiente marcado después de la paginación y ordenación con los controles DataList y Repeater `<siteMapNode>`:


[!code-xml[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample1.xml)]

Después de actualizar `Web.sitemap`, dedique un momento para ver el sitio Web de tutoriales a través de un explorador. El menú de la izquierda ahora incluye elementos para la edición, inserción y eliminación de tutoriales.


![El mapa del sitio incluye ahora la entrada para el Tutorial de botones personalizados](custom-buttons-in-the-datalist-and-repeater-cs/_static/image8.png)

**Figura 4**: el mapa del sitio incluye ahora la entrada para el Tutorial de botones personalizados


## <a name="step-2-adding-the-list-of-categories"></a>Paso 2: Adición de la lista de categorías

Para este tutorial se necesita crear un control Repeater que enumera todas las categorías, junto con un control LinkButton mostrar productos que, al hacer clic, muestra los productos de la categoría asociada s en una lista con viñetas. Permiten s en primer lugar cree un control Repeater simple que muestra las categorías en el sistema. Comience abriendo la `CustomButtons.aspx` página en el `CustomButtonsDataListRepeater` carpeta. Arrastre un control Repeater desde el cuadro de herramientas hasta el diseñador y el conjunto de sus `ID` propiedad `Categories`. A continuación, cree un nuevo control de origen de datos de la etiqueta inteligente de s Repeater. En concreto, cree un nuevo control ObjectDataSource denominado `CategoriesDataSource` que selecciona los datos desde el `CategoriesBLL` clase s `GetCategories()` método.


[![Configurar el origen ObjectDataSource para usar el método de clase CategoriesBLL s GetCategories()](custom-buttons-in-the-datalist-and-repeater-cs/_static/image10.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image9.png)

**Figura 5**: configurar el origen ObjectDataSource que se usarán el `CategoriesBLL` clase s `GetCategories()` método ([haga clic aquí para ver imagen en tamaño completo](custom-buttons-in-the-datalist-and-repeater-cs/_static/image11.png))


A diferencia del control DataList, para que Visual Studio crea un valor predeterminado `ItemTemplate` según el origen de datos, las plantillas de Repeater s deben definirse manualmente. Además, las plantillas de Repeater s deben crearse y editarse mediante declaración (es decir, hay s editar plantillas no opción en la etiqueta inteligente del control Repeater s).

Haga clic en la ficha de origen en la esquina inferior izquierda y agregue un `ItemTemplate` que muestra el nombre de categoría s en un `<h3>` elemento y su descripción en un párrafo de etiquetas; incluye una `SeparatorTemplate` que muestra una regla horizontal (`<hr />`) entre cada uno categoría. Agregue también un control LinkButton con su `Text` propiedad establecida en Mostrar productos. Después de completar estos pasos, el marcado declarativo s de página debe ser similar al siguiente:


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample2.aspx)]

Figura 6 muestra la página cuando se ve mediante un explorador. Cada nombre de categoría y descripción se muestran. El botón Mostrar productos, al hacer clic, produce un postback pero aún no realiza ninguna acción.


[![Cada nombre de categoría y la descripción se muestran, junto con un control LinkButton mostrar productos](custom-buttons-in-the-datalist-and-repeater-cs/_static/image13.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image12.png)

**Figura 6**: s nombre de cada categoría y la descripción se muestran, junto con un control LinkButton mostrar productos ([haga clic aquí para ver imagen en tamaño completo](custom-buttons-in-the-datalist-and-repeater-cs/_static/image14.png))


## <a name="step-3-executing-server-side-logic-when-the-show-products-linkbutton-is-clicked"></a>Paso 3: Ejecutar servidor lógica cuando el mostrar productos LinkButton se hace clic en

Cada vez que se hace clic en un botón, LinkButton o ImageButton dentro de una plantilla en un control DataList o Repeater, se produce un postback y la s DataList o Repeater `ItemCommand` desencadena el evento. Además el `ItemCommand` eventos, el control DataList control también puede generar más específico, otro evento si el botón s `CommandName` propiedad está establecida en una de las cadenas reservadas (eliminar, editar, Cancelar, Update o Select), pero la `ItemCommand` evento es *siempre* desencadena.

Cuando se hace clic en un botón dentro de un control DataList o Repeater, a menudo, necesitamos pasar (como qué botón se hizo clic (en el caso de que puede haber varios botones dentro del control, por ejemplo, tanto una edición y botón Eliminar) y quizás algo de información adicional el valor de clave principal del elemento cuyo botón se hizo clic). El botón, LinkButton y ImageButton proporcionan dos propiedades cuyos valores se pasan a la `ItemCommand` controlador de eventos:

- `CommandName` una cadena que se utiliza normalmente para identificar cada botón en la plantilla
- `CommandArgument` usan normalmente para almacenar el valor de algún campo de datos, como el valor de clave principal

Este ejemplo, establezca la s LinkButton `CommandName` propiedad ShowProducts y enlace el valor de clave principal actual de registro s `CategoryID` a la `CommandArgument` propiedad utilizando la sintaxis de enlace de datos `CategoryArgument='<%# Eval("CategoryID") %>'`. Después de especificar estas dos propiedades, la sintaxis declarativa del control LinkButton s debería ser similar al siguiente:


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample3.aspx)]

Cuando se presiona el botón, se produce un postback y la s DataList o Repeater `ItemCommand` desencadena el evento. El controlador de eventos se pasa en el botón s `CommandName` y `CommandArgument` valores.

Crear un controlador de eventos para el control Repeater s `ItemCommand` eventos y anote el segundo parámetro pasan al controlador de eventos (denominado `e`). Este segundo parámetro es de tipo [ `RepeaterCommandEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeatercommandeventargs.aspx) y tiene las siguientes cuatro propiedades:

- `CommandArgument` el valor del botón ha hecho clic s `CommandArgument` propiedad
- `CommandName` el valor del botón s `CommandName` propiedad
- `CommandSource` una referencia al control de botón que se hizo clic
- `Item` una referencia a la [ `RepeaterItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem.aspx) que contiene el botón que se hizo clic; cada registro enlazado a Repeater se manifiesta como un `RepeaterItem`

Desde la categoría seleccionada s `CategoryID` se pasa a través de la `CommandArgument` propiedad, podemos obtener el conjunto de productos asociados con la categoría seleccionada en el `ItemCommand` controlador de eventos. Estos productos, a continuación, se pueden enlazar a un control BulletedList en el `ItemTemplate` (que se ve aún para agregar). Todo lo que permanece, a continuación, consiste en Agregar BulletedList, haga referencia a él en el `ItemCommand` controlador de eventos y enlazar a él en el conjunto de productos para la categoría seleccionada, que abordaremos en el paso 4.

> [!NOTE]
> El control DataList s `ItemCommand` controlador de eventos se pasa un objeto de tipo [ `DataListCommandEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistcommandeventargs.aspx), que ofrece las mismas cuatro propiedades que el `RepeaterCommandEventArgs` clase.


## <a name="step-4-displaying-the-selected-category-s-products-in-a-bulleted-list"></a>Paso 4: Mostrar los productos de s de la categoría seleccionada en una lista con viñetas

Los productos de la categoría seleccionada s pueden mostrarse en el control Repeater s `ItemTemplate` con cualquier número de controles. Podríamos agregar que otro anidados Repeater, DataList, DropDownList, un GridView y así sucesivamente. Sin embargo, puesto que deseamos mostrar los productos como una lista con viñetas, vamos a usar el control BulletedList. Volver a la `CustomButtons.aspx` marcado declarativo s, agregue un control BulletedList a la `ItemTemplate` después de mostrar productos LinkButton. Establezca la s BulletedLists `ID` a `ProductsInCategory`. BulletedList muestra el valor del campo de datos especificado a través de la `DataTextField` propiedad; ya que este control tendrá la información del producto enlazado a él, establezca el `DataTextField` propiedad `ProductName`.


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample4.aspx)]

En el `ItemCommand` controlador de eventos, hacen referencia a este control mediante `e.Item.FindControl("ProductsInCategory")` y enlácelo al conjunto de productos asociados con la categoría seleccionada.


[!code-csharp[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample5.cs)]

Antes de realizar cualquier acción en el `ItemCommand` controlador de eventos, lo s prudente compruebe primero el valor de entrada `CommandName`. Puesto que la `ItemCommand` controlador de eventos desencadena cuando *cualquier* botón se hizo clic, si hay varios botones en la plantilla, use el `CommandName` valor discernir qué acción realizar. Comprobando el `CommandName` aquí es discutible, puesto que sólo tenemos un solo botón, pero es un buen hábito al formulario. A continuación, el `CategoryID` de la categoría seleccionada se recuperan desde el `CommandArgument` propiedad. El control de BulletedList en la plantilla, a continuación, se hace referencia y enlazado a los resultados de la `ProductsBLL` clase s `GetProductsByCategoryID(categoryID)` método.

En los tutoriales anteriores que usan los botones dentro de un control DataList, tales como [una visión general de edición y eliminación de datos en el control DataList](../editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md), determinamos que el valor de clave principal de un elemento determinado a través de la `DataKeys` colección. Aunque este enfoque funciona bien con el control DataList, el control Repeater no tiene un `DataKeys` propiedad. En su lugar, debemos usar un enfoque alternativo para proporcionar el valor de clave principal, como a través del botón s `CommandArgument` propiedad o asignando el valor de clave principal a un control Web Label oculto dentro de la plantilla y leer su valor en el `ItemCommand`controlador de eventos mediante `e.Item.FindControl("LabelID")`.

Después de completar la `ItemCommand` controlador de eventos, dedique un momento para probar esta página en un explorador. Como se muestra en la figura 7, al hacer clic en los productos Mostrar vínculo provoca una devolución de datos y muestra los productos para la categoría seleccionada en un BulletedList. Además, tenga en cuenta que esta información de producto permanece, incluso si se hace clic en otras categorías de vínculos de mostrar productos.

> [!NOTE]
> Si desea modificar el comportamiento de este informe, tal que se enumeran los productos sólo una categoría s a la vez, basta con establecer el control BulletedList s `EnableViewState` propiedad `False`.


[![Un BulletedList se usa para mostrar los productos de la categoría seleccionada](custom-buttons-in-the-datalist-and-repeater-cs/_static/image16.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image15.png)

**Figura 7**: BulletedList A se usa para mostrar los productos de la categoría seleccionada ([haga clic aquí para ver imagen en tamaño completo](custom-buttons-in-the-datalist-and-repeater-cs/_static/image17.png))


## <a name="summary"></a>Resumen

Los controles DataList y Repeater pueden incluir cualquier número de botones, LinkButtons o ImageButtons dentro de sus plantillas. Estos botones, al hacer clic, hacer que una devolución de datos y generar el `ItemCommand` eventos. Para asociar la acción personalizada de servidor con un botón que se hace clic, cree un controlador de eventos para el `ItemCommand` eventos. En este controlador de eventos, primero compruebe entrante `CommandName` valor para determinar qué botón se hizo clic. Opcionalmente, se puede suministrar información adicional mediante el botón s `CommandArgument` propiedad.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Revisor de clientes potencial para este tutorial era Dennis Patterson. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Siguiente](custom-buttons-in-the-datalist-and-repeater-vb.md)
