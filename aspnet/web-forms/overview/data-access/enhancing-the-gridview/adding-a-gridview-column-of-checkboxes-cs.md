---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs
title: Agregar una columna GridView de casillas de verificación (C#) | Microsoft Docs
author: rick-anderson
description: Este tutorial explica cómo agregar una columna de casillas de verificación a un control GridView para proporcionar al usuario una manera intuitiva de la selección de varias filas de la G...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2007
ms.topic: article
ms.assetid: f63a9443-2db0-4f80-8246-840d3e86c2a3
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs
msc.type: authoredcontent
ms.openlocfilehash: 2d79a95bd1e2b0f83d15709cf3ede601a707cb77
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362977"
---
<a name="adding-a-gridview-column-of-checkboxes-c"></a>Agregar una columna GridView de casillas de verificación (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_52_CS.exe) o [descargar PDF](adding-a-gridview-column-of-checkboxes-cs/_static/datatutorial52cs1.pdf)

> Este tutorial examina cómo agregar una columna de casillas de verificación a un control GridView para proporcionar al usuario una manera intuitiva de la selección de varias filas del control GridView.


## <a name="introduction"></a>Introducción

En el tutorial anterior, hemos visto cómo agregar una columna de botones de radio en el control GridView con el fin de seleccionar un registro concreto. Una columna de botones de opción es una interfaz de usuario adecuado cuando el usuario está limitado a elegir a lo sumo un elemento de la cuadrícula. En ocasiones, sin embargo, nos conviene permite al usuario seleccionar un número arbitrario de elementos de la cuadrícula. Por ejemplo, los clientes de correo electrónico basado en Web, mostrar la lista de mensajes con una columna de casillas de verificación. El usuario puede seleccionar un número arbitrario de mensajes y, a continuación, realizar alguna acción, como mover los mensajes de correo electrónico a otra carpeta o eliminarlos.

En este tutorial, veremos cómo agregar una columna de casillas de verificación y cómo determinar qué casillas de verificación se protegieron en el postback. En concreto, vamos a crear un ejemplo que imita la interfaz de usuario del cliente de correo electrónico basado en web. En nuestro ejemplo incluirá un GridView paginado lista de los productos en el `Products` (consulte la figura 1) de tabla de base de datos con una casilla en cada fila. Un botón Eliminar productos seleccionados, al hacer clic, eliminará los productos seleccionados.


[![Cada fila del producto incluye una casilla de verificación](adding-a-gridview-column-of-checkboxes-cs/_static/image1.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image1.png)

**Figura 1**: cada fila del producto incluye una casilla de verificación ([haga clic aquí para ver imagen en tamaño completo](adding-a-gridview-column-of-checkboxes-cs/_static/image2.png))


## <a name="step-1-adding-a-paged-gridview-that-lists-product-information"></a>Paso 1: Agregar un control GridView paginado que muestra información de producto

Antes de que preocuparse acerca de cómo agregar una columna de casillas de verificación, permiten centrarnos primero de s en la lista de los productos en un control GridView que admita la paginación. Comience abriendo la `CheckBoxField.aspx` página en el `EnhancedGridView` carpeta y arrastre un control GridView del cuadro de herramientas hasta el diseñador, establecer su `ID` a `Products`. A continuación, elija enlazar el control GridView a un nuevo origen ObjectDataSource denominado `ProductsDataSource`. Configurar el origen ObjectDataSource para usar el `ProductsBLL` clase, una llamada a la `GetProducts()` método para devolver los datos. Puesto que este GridView será de solo lectura, establezca las listas desplegables en la actualización, INSERCIÓN y eliminar pestañas en (None).


[![Crear un nuevo origen ObjectDataSource denominado ProductsDataSource](adding-a-gridview-column-of-checkboxes-cs/_static/image2.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image3.png)

**Figura 2**: crear un nuevo origen ObjectDataSource denominado `ProductsDataSource` ([haga clic aquí para ver imagen en tamaño completo](adding-a-gridview-column-of-checkboxes-cs/_static/image4.png))


[![Configurar el origen ObjectDataSource para recuperar datos mediante el método GetProducts()](adding-a-gridview-column-of-checkboxes-cs/_static/image3.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image5.png)

**Figura 3**: configurar el origen ObjectDataSource para recuperar datos mediante el `GetProducts()` método ([haga clic aquí para ver imagen en tamaño completo](adding-a-gridview-column-of-checkboxes-cs/_static/image6.png))


[![Establecer las listas desplegables en la actualización, INSERCIÓN y eliminar las fichas en (None)](adding-a-gridview-column-of-checkboxes-cs/_static/image4.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image7.png)

**Figura 4**: establecer la lista desplegable se enumeran en la actualización, INSERCIÓN y eliminar pestañas en (None) ([haga clic aquí para ver imagen en tamaño completo](adding-a-gridview-column-of-checkboxes-cs/_static/image8.png))


Después de completar al Asistente para configurar orígenes de datos, Visual Studio creará automáticamente BoundColumns y un CheckBoxColumn para los campos de datos relacionados con el producto. Igual que hizo en el tutorial anterior, quite todos menos a los `ProductName`, `CategoryName`, y `UnitPrice` BoundFields y cambie el `HeaderText` propiedades para el producto, Category y Price. Configurar la `UnitPrice` BoundField para que su valor tiene el formato como una moneda. Configure también la GridView para admitir la paginación activando la casilla Habilitar la paginación de la etiqueta inteligente.

Permitir s también agregar la interfaz de usuario para eliminar los productos seleccionados. Agregar un control de botón Web bajo el control GridView, establecer su `ID` a `DeleteSelectedProducts` y su `Text` propiedad para eliminar los productos seleccionados. En lugar de eliminar realmente los productos de la base de datos, en este ejemplo se sólo mostrará un mensaje que indica los productos que se habrían eliminados. Para dar cabida a esto, agregue un control Web de la etiqueta debajo del botón. Establezca su identificador en `DeleteResults`, desactive horizontalmente su `Text` propiedad y establezca su `Visible` y `EnableViewState` propiedades para `false`.

Después de realizar estos cambios, el marcado declarativo de s GridView, ObjectDataSource, botón y etiqueta debería similar al siguiente:


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample1.aspx)]

Dedique un momento para ver la página en un explorador (consulte la figura 5). En este momento debería ver el nombre, la categoría y el precio de los diez primeros productos.


[![Se muestran el nombre, la categoría y el precio de los primeros diez productos](adding-a-gridview-column-of-checkboxes-cs/_static/image5.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image9.png)

**Figura 5**: se enumeran el nombre, categoría y el precio de los primeros diez productos ([haga clic aquí para ver imagen en tamaño completo](adding-a-gridview-column-of-checkboxes-cs/_static/image10.png))


## <a name="step-2-adding-a-column-of-checkboxes"></a>Paso 2: Agregar una columna de casillas de verificación

Dado que ASP.NET 2.0 incluye una CampoCasillaVerificación, uno puede pensar que podría usarse para agregar una columna de casillas de verificación en un control GridView. Por desgracia, que no es el caso, como el CampoCasillaVerificación está diseñado para trabajar con un campo de datos booleano. Es decir, para poder usar CheckBoxField debemos especificar el campo de datos subyacente cuyo valor se consulta para determinar si se activa la casilla representada. No podemos utilizar CheckBoxField para incluir solo una columna de casillas de verificación desactivada.

En su lugar, debemos agregar TemplateField y agregar un control de casilla de verificación Web a su `ItemTemplate`. Agregue un TemplateField para el `Products` GridView y conviértalo en el primer campo (izquierdo). En la etiqueta inteligente de GridView s, haga clic en el vínculo Editar plantillas y, a continuación, arrastre un control CheckBox Web desde el cuadro de herramientas en el `ItemTemplate`. Establecer esta casilla de verificación s `ID` propiedad `ProductSelector`.


[![Agregue un Control de casilla de verificación Web denominado ProductSelector a TemplateField s ItemTemplate](adding-a-gridview-column-of-checkboxes-cs/_static/image6.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image11.png)

**Figura 6**: agregar un Control de casilla de verificación Web denominado `ProductSelector` al s TemplateField `ItemTemplate` ([haga clic aquí para ver imagen en tamaño completo](adding-a-gridview-column-of-checkboxes-cs/_static/image12.png))


Con el control Web de la casilla de verificación y TemplateField agregado, cada fila ahora incluye una casilla de verificación. Figura 7 muestra esta página, cuando se ve mediante un explorador, después de han agregado el TemplateField y la casilla de verificación.


[![Cada fila del producto ahora incluye una casilla de verificación](adding-a-gridview-column-of-checkboxes-cs/_static/image7.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image13.png)

**Figura 7**: cada fila del producto ahora incluye una casilla de verificación ([haga clic aquí para ver imagen en tamaño completo](adding-a-gridview-column-of-checkboxes-cs/_static/image14.png))


## <a name="step-3-determining-what-checkboxes-were-checked-on-postback"></a>Paso 3: Determinar qué casillas de verificación se comprobaron en devolución de datos

En este momento tenemos una columna de casillas, pero no se puede determinar qué casillas de verificación se protegieron en el postback. Sin embargo, cuando se hace clic en el botón Eliminar productos seleccionados, necesitamos saber qué casillas de verificación se comprobaron con el fin de eliminar esos productos.

Las operaciones de asignación GridView [ `Rows` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows.aspx) proporciona acceso a las filas de datos en GridView. Nos podemos iterar a través de estas filas, obtener acceso mediante programación el control de casilla de verificación y, a continuación, consulte su `Checked` propiedad para determinar si se ha seleccionado la casilla de verificación.

Crear un controlador de eventos para el `DeleteSelectedProducts` control de botón Web s `Click` eventos y agregue el código siguiente:


[!code-csharp[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample2.cs)]

El `Rows` propiedad devuelve una colección de `GridViewRow` instancias que utilizan las filas de datos s GridView. El `foreach` bucle aquí enumera esta colección. Para cada `GridViewRow` objeto, la fila s CheckBox se accede mediante programación utilizando `row.FindControl("controlID")`. Si está activada la casilla de verificación, la correspondiente fila s `ProductID` se recupera el valor de la `DataKeys` colección. En este ejercicio, simplemente mostrar un mensaje informativo en el `DeleteResults` etiquetar, aunque en una aplicación de trabajo d en su lugar, realizamos una llamada a la `ProductsBLL` clase s `DeleteProduct(productID)` método.

Con la adición de este controlador de eventos, haga clic en el botón Eliminar productos seleccionados ahora muestra el `ProductID` s de los productos seleccionados.


[![Cuando se hace clic en el botón Eliminar productos de seleccionado se muestran el ProductID de productos seleccionada](adding-a-gridview-column-of-checkboxes-cs/_static/image8.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image15.png)

**Figura 8**: al eliminar seleccionado productos Button se hace clic en los productos seleccionados `ProductID` se enumeran ([haga clic aquí para ver imagen en tamaño completo](adding-a-gridview-column-of-checkboxes-cs/_static/image16.png))


## <a name="step-4-adding-check-all-and-uncheck-all-buttons"></a>Paso 4: Agregar Active todos y desactive todos los botones

Si un usuario desea eliminar todos los productos en la página actual, debe comprobar cada una de las casillas de diez verificación. Podemos ayudarle a acelerar este proceso mediante la adición de comprobar todas botón que, al hacer clic, selecciona todas las casillas de verificación en la cuadrícula. Un botón Desactivar todo sería igualmente útil.

Agregue dos controles de botón Web a la página, y los coloca por encima del control GridView. Establecer la s primero un `ID` a `CheckAll` y su `Text` la propiedad comprobar todo; la un segundo s conjunto `ID` a `UncheckAll` y su `Text` propiedad para desactivar todo.


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample3.aspx)]

A continuación, cree un método en la clase de código subyacente denominada `ToggleCheckState(checkState)` que, cuando se invoca, enumera el `Products` GridView s `Rows` colección y establece cada casilla de verificación s `Checked` propiedad en el valor pasado en *checkState*  parámetro.


[!code-csharp[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample4.cs)]

A continuación, cree `Click` controladores de eventos para el `CheckAll` y `UncheckAll` botones. En `CheckAll` controlador de eventos de s, simplemente llamada `ToggleCheckState(true)`; en `UncheckAll`, llame a `ToggleCheckState(false)`.


[!code-csharp[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample5.cs)]

Con este código, al hacer clic en el botón Comprobar todo produce un postback y comprueba todas las casillas de verificación en el control GridView. Del mismo modo, al hacer clic en desactivar todo anula la selección de todas las casillas. Figura 9 muestra la pantalla después el botón Comprobar todo se ha protegido.


[![Al hacer clic en la comprobación de que todos los botones selecciona todas las casillas de verificación](adding-a-gridview-column-of-checkboxes-cs/_static/image9.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image17.png)

**Figura 9**: al hacer clic en la comprobación de todos los botón selecciona todas las casillas ([haga clic aquí para ver imagen en tamaño completo](adding-a-gridview-column-of-checkboxes-cs/_static/image18.png))


> [!NOTE]
> Al mostrar una columna de casillas de verificación, un enfoque para seleccionar o anular la selección todas las casillas de verificación es a través de una casilla en la fila de encabezado. Además, actual marque todas las / desactive toda implementación requiere un postback. Las casillas de verificación puede activar o desactivar, sin embargo, completamente a través de script de cliente, lo que proporciona una experiencia de usuario más ágil. Para explorar el uso de una casilla de la fila de encabezado para comprobar todo y desactive todo en detalle, junto con una discusión sobre el uso de técnicas de lado cliente, consulte [comprobación todas las casillas de verificación en un GridView Using Script del lado cliente y una todas las casilla](http://aspnet.4guysfromrolla.com/articles/053106-1.aspx).


## <a name="summary"></a>Resumen

En casos en que necesite para permitir que los usuarios elijan un número arbitrario de filas de un control GridView antes de continuar, agregar una columna de casillas de verificación es una opción. Como hemos visto en este tutorial, incluida una columna de casillas de verificación en el control GridView implica agregar TemplateField con un control de casilla de verificación Web. Mediante el uso de un control Web (frente a la inyección de marcado directamente en la plantilla, como hicimos en el tutorial anterior) ASP.NET automáticamente recuerda lo que eran las casillas de verificación y a través de la devolución de datos no comprobados. Podemos acceder mediante programación a las casillas de verificación en el código para determinar si se activa una casilla determinada, o para cambiar el estado activado.

En este tutorial y la última de ellas examinado agregando una columna de selector de fila en el control GridView. En nuestro tutorial siguiente examinaremos cómo, con un poco de trabajo, podemos agregar funciones de inserción en el control GridView.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](adding-a-gridview-column-of-radio-buttons-cs.md)
> [Siguiente](inserting-a-new-record-from-the-gridview-s-footer-cs.md)
