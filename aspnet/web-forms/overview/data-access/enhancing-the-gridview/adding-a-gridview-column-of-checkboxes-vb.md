---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb
title: "Agregar una columna de GridView de casillas de verificación (VB) | Documentos de Microsoft"
author: rick-anderson
description: "Este tutorial examina cómo agregar una columna de casillas de verificación a un control GridView para proporcionar al usuario de una forma intuitiva de la selección de varias filas de la G...."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2007
ms.topic: article
ms.assetid: 39253d05-75c0-41c7-b9d4-a6c58ecf69ce
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: 326201f9fe9ba5f482308dc8bfd7d2decb9fbd8f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-gridview-column-of-checkboxes-vb"></a>Agregar una columna de GridView de casillas de verificación (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_52_VB.exe) o [descarga de PDF](adding-a-gridview-column-of-checkboxes-vb/_static/datatutorial52vb1.pdf)

> Este tutorial examina cómo agregar una columna de casillas de verificación a un control GridView para proporcionar al usuario de una forma intuitiva de la selección de varias filas de GridView.


## <a name="introduction"></a>Introducción

En el tutorial anterior, se examina cómo agregar una columna de botones de radio a la GridView con el fin de seleccionar un registro concreto. Una columna de botones de radio es una interfaz de usuario adecuado cuando el usuario está limitado a elegir al menos un elemento de la cuadrícula. En ocasiones, sin embargo, podemos queremos permitir al usuario elegir un número arbitrario de elementos de la cuadrícula. Por ejemplo, los clientes de correo electrónico basado en Web, mostrar la lista de mensajes con una columna de casillas de verificación. El usuario puede seleccionar un número arbitrario de mensajes y, a continuación, realizar alguna acción, como mover los mensajes de correo electrónico a otra carpeta o eliminarlos.

En este tutorial veremos cómo agregar una columna de casillas de verificación y cómo determinar qué casillas de verificación se seleccionaron en el postback. En concreto, vamos a crear un ejemplo que se asemeje estrechamente a la interfaz de usuario del cliente de correo electrónico basado en web. Nuestro ejemplo incluirá un GridView paginada enumerar los productos en la `Products` (consulte la figura 1) de la fila de tabla de base de datos con un control checkbox en cada uno. Un botón Eliminar los productos seleccionados, al hacer clic, eliminará los productos seleccionados.


[![Cada fila del producto incluye una casilla de verificación](adding-a-gridview-column-of-checkboxes-vb/_static/image1.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image1.png)

**Figura 1**: cada fila del producto incluye una casilla de verificación ([haga clic aquí para ver la imagen a tamaño completo](adding-a-gridview-column-of-checkboxes-vb/_static/image2.png))


## <a name="step-1-adding-a-paged-gridview-that-lists-product-information"></a>Paso 1: Agregar un paginada GridView que muestra información de producto

Antes de que nos preocupamos sobre cómo agregar una columna de casillas de verificación, permiten foco primera s acerca de cómo enumerar los productos en un control GridView que admite la paginación. Comience abriendo la `CheckBoxField.aspx` página en el `EnhancedGridView` carpeta y arrastre un control GridView del cuadro de herramientas hasta el diseñador, establecer su `ID` a `Products`. A continuación, elija enlazar GridView a un nuevo ObjectDataSource denominado `ProductsDataSource`. Configurar el ObjectDataSource para usar el `ProductsBLL` de la clase, una llamada a la `GetProducts()` método para devolver los datos. Puesto que este GridView serán de sólo lectura, establezca las listas desplegables de la actualización, INSERCIÓN y eliminar tabulaciones en (ninguno).


[![Crear un nuevo origen ObjectDataSource denominado ProductsDataSource](adding-a-gridview-column-of-checkboxes-vb/_static/image2.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image3.png)

**Figura 2**: crear un nuevo ObjectDataSource denominado `ProductsDataSource` ([haga clic aquí para ver la imagen a tamaño completo](adding-a-gridview-column-of-checkboxes-vb/_static/image4.png))


[![Configurar el ObjectDataSource para recuperar datos mediante el método GetProducts()](adding-a-gridview-column-of-checkboxes-vb/_static/image3.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image5.png)

**Figura 3**: configurar el ObjectDataSource para recuperar datos mediante el `GetProducts()` método ([haga clic aquí para ver la imagen a tamaño completo](adding-a-gridview-column-of-checkboxes-vb/_static/image6.png))


[![Establece las listas de lista desplegable en la actualización, INSERCIÓN y eliminar las pestañas en (ninguno)](adding-a-gridview-column-of-checkboxes-vb/_static/image4.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image7.png)

**Figura 4**: establecer las listas de lista desplegable en la actualización, INSERCIÓN y eliminar pestañas en (ninguno) ([haga clic aquí para ver la imagen a tamaño completo](adding-a-gridview-column-of-checkboxes-vb/_static/image8.png))


Después de completar al Asistente para configurar orígenes de datos, Visual Studio creará automáticamente BoundColumns y un CheckBoxColumn para los campos de datos relacionados con el producto. Al igual que hicimos en el tutorial anterior, quite todos menos el `ProductName`, `CategoryName`, y `UnitPrice` BoundFields y cambie el `HeaderText` propiedades para el producto, la categoría y el precio. Configurar la `UnitPrice` BoundField para que su valor se le aplica formato como una moneda. Configurar el control GridView para admitir paginación activando la casilla de verificación Habilitar paginación de la etiqueta inteligente.

Permiten s también agregar la interfaz de usuario para eliminar los productos seleccionados. Agregar un control de botón Web bajo el control GridView, establecer su `ID` a `DeleteSelectedProducts` y su `Text` propiedad para eliminar los productos seleccionados. En lugar de eliminar los productos de la base de datos, en este ejemplo solo se podrá mostrar un mensaje que indica los productos que se habrían eliminados. Para ello, agregue un control Web Label debajo del botón. Establecer su Id. `DeleteResults`, desactive fuera su `Text` propiedad y establezca su `Visible` y `EnableViewState` propiedades para `False`.

Después de realizar estos cambios, el marcado declarativo de s GridView, ObjectDataSource, botón y etiqueta debe ser similar al siguiente:


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample1.aspx)]

Tómese un momento para ver la página en un explorador (Véase la figura 5). En este momento debería ver el nombre, la categoría y el precio de los diez primeros productos.


[![Se muestran el nombre, la categoría y el precio de los diez primeros productos](adding-a-gridview-column-of-checkboxes-vb/_static/image5.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image9.png)

**Figura 5**: se enumeran el nombre, categoría y precio de los diez primeros productos ([haga clic aquí para ver la imagen a tamaño completo](adding-a-gridview-column-of-checkboxes-vb/_static/image10.png))


## <a name="step-2-adding-a-column-of-checkboxes"></a>Paso 2: Agregar una columna de casillas de verificación

Dado que ASP.NET 2.0 incluye una CampoCasillaVerificación, uno que piense que se puede usar para agregar una columna de casillas de verificación a un control GridView. Por desgracia, que no es el caso, como la CampoCasillaVerificación está diseñado para trabajar con un campo de datos booleano. Es decir, para poder usar el CampoCasillaVerificación debemos especificar el campo de datos subyacente cuyo valor se consulta para determinar si se activa la casilla representada. No podemos usar la CampoCasillaVerificación para incluir solo una columna de casillas de verificación no está activada.

En su lugar, debemos agregar TemplateField y agregar un control de casilla de verificación Web a su `ItemTemplate`. Agregue un TemplateField para el `Products` GridView y hacer que el primer campo (lejano a la izquierda). Desde la etiqueta inteligente de GridView s, haga clic en el vínculo Editar plantillas y, a continuación, arrastre un control CheckBox Web en el cuadro de herramientas en el `ItemTemplate`. Establecer esta casilla de verificación s `ID` propiedad `ProductSelector`.


[![Agregue un Control de casilla de verificación Web denominado ProductSelector a TemplateField s ItemTemplate](adding-a-gridview-column-of-checkboxes-vb/_static/image6.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image11.png)

**Figura 6**: agregar un Control de casilla de verificación Web denominado `ProductSelector` para las operaciones de asignación TemplateField `ItemTemplate` ([haga clic aquí para ver la imagen a tamaño completo](adding-a-gridview-column-of-checkboxes-vb/_static/image12.png))


Con el control Web de casilla de verificación y TemplateField agregado, cada fila ahora incluye una casilla de verificación. La figura 7 muestra esta página, cuando se ven a través de un explorador, después de han agregado la casilla de verificación y TemplateField.


[![Cada fila del producto ahora incluye una casilla de verificación](adding-a-gridview-column-of-checkboxes-vb/_static/image7.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image13.png)

**Figura 7**: cada fila del producto ahora incluye una casilla de verificación ([haga clic aquí para ver la imagen a tamaño completo](adding-a-gridview-column-of-checkboxes-vb/_static/image14.png))


## <a name="step-3-determining-what-checkboxes-were-checked-on-postback"></a>Paso 3: Determinar qué casillas de verificación se seleccionaron en la devolución de datos

En este momento tenemos una columna de casillas de verificación pero ninguna manera de determinar qué casillas de verificación se seleccionaron en el postback. Sin embargo, cuando se hace clic en el botón Eliminar productos seleccionado, es necesario saber qué casillas de verificación se comprobaron con el fin de eliminar los productos.

Las operaciones de asignación GridView [ `Rows` propiedad](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.rows.aspx) proporciona acceso a las filas de datos en GridView. Se puede recorrer en iteración estas filas, obtener acceso mediante programación el control de casilla de verificación y, a continuación, consulte su `Checked` propiedad para determinar si se ha seleccionado la casilla de verificación.

Crear un controlador de eventos para el `DeleteSelectedProducts` control Button Web s `Click` eventos y agregue el código siguiente:


[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample2.vb)]

El `Rows` propiedad devuelve una colección de `GridViewRow` instancias que utilizan las filas de datos de GridView s. El `For Each` bucle aquí enumera esta colección. Para cada `GridViewRow` objeto, la fila s casilla mediante programación se tiene acceso mediante `row.FindControl("controlID")`. Si se activa la casilla, la correspondiente fila s `ProductID` valor se recupera de la `DataKeys` colección. En este ejercicio, simplemente se muestra un mensaje informativo en el `DeleteResults` etiquetar, aunque en una aplicación operativa d en su lugar, realizamos una llamada a la `ProductsBLL` clase s. `DeleteProduct(productID)` método.

Con la adición de este controlador de eventos, haga clic en el botón Eliminar los productos seleccionados ahora muestra el `ProductID` s de los productos seleccionados.


[![Cuando se hace clic en el botón Eliminar productos de seleccionado se enumeran el ProductID de productos seleccionado](adding-a-gridview-column-of-checkboxes-vb/_static/image8.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image15.png)

**Figura 8**: cuando la selecciona productos botón Eliminar se hace clic en los productos seleccionados `ProductID` se enumeran ([haga clic aquí para ver la imagen a tamaño completo](adding-a-gridview-column-of-checkboxes-vb/_static/image16.png))


## <a name="step-4-adding-check-all-and-uncheck-all-buttons"></a>Paso 4: Agregar Active todos y desactive todos los botones

Si un usuario desea eliminar todos los productos en la página actual, debe comprobar cada una de las casillas de diez verificación. Te podemos ayudar a acelerar este proceso agregando marcar todas botón que, al hacer clic, selecciona todas las casillas de verificación en la cuadrícula. Un botón Desactive todo sería útil igualmente.

Agregue dos controles de botón Web a la página, colocarlas encima de GridView. Establecer la primero s `ID` a `CheckAll` y su `Text` propiedad para comprobar todos los; se establece el segundo s `ID` a `UncheckAll` y su `Text` propiedad desactive todos los.


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample3.aspx)]

A continuación, cree un método en la clase de código subyacente denominada `ToggleCheckState(checkState)` que, cuando se invoca, enumera el `Products` GridView s `Rows` colección y establece cada casilla de verificación s `Checked` en el valor del pasado en *checkState*  parámetro.


[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample4.vb)]

A continuación, cree `Click` controladores de eventos para el `CheckAll` y `UncheckAll` botones. En `CheckAll` controlador de eventos de s, simplemente llamada `ToggleCheckState(True)`; en `UncheckAll`, llame a `ToggleCheckState(False)`.


[!code-vb[Main](adding-a-gridview-column-of-checkboxes-vb/samples/sample5.vb)]

Con este código, haga clic en el botón Comprobar todo provoca una devolución de datos y comprueba todas las casillas de verificación en el control GridView. Del mismo modo, haga clic en desactivar todo anula la selección de todas las casillas. Ilustración 9 se muestra la pantalla después de que se ha comprobado el botón Comprobar todo.


[![Haga clic en la comprobación de que todos los botones selecciona todas las casillas](adding-a-gridview-column-of-checkboxes-vb/_static/image9.gif)](adding-a-gridview-column-of-checkboxes-vb/_static/image17.png)

**Figura 9**: haga clic en la comprobación de todos los botón selecciona todas las casillas ([haga clic aquí para ver la imagen a tamaño completo](adding-a-gridview-column-of-checkboxes-vb/_static/image18.png))


> [!NOTE]
> Al mostrar una columna de casillas de verificación, un enfoque para seleccionar o anulando la selección de todas las casillas de verificación es a través de una casilla en la fila de encabezado. Además, actual marque todo / desactive toda implementación requiere un postback. Las casillas de verificación pueden ser activado o desactivado, sin embargo, completamente a través de script del lado cliente, lo que proporciona una experiencia de usuario más ágil. Para explorar mediante una casilla de la fila de encabezado para comprobar todo y desactive todo en detalle, junto con una explicación sobre el uso de técnicas de lado cliente, visite [comprobar todas las casillas en un Script del lado cliente utilizando GridView y compruebe todas las casilla](http://aspnet.4guysfromrolla.com/articles/053106-1.aspx).


## <a name="summary"></a>Resumen

En casos donde es necesario para permitir que los usuarios elegir un número arbitrario de filas de un control GridView antes de continuar, agregar una columna de casillas de verificación es una opción. Como hemos visto en este tutorial, incluyendo una columna de casillas de verificación en el control GridView implica agregar TemplateField con un control de casilla de verificación Web. Mediante el uso de un control Web (en lugar de insertar marcado directamente en la plantilla, como hicimos en el tutorial anterior) ASP.NET recuerda automáticamente lo que eran las casillas de verificación y no se comprueban a través de la devolución de datos. Podemos acceder mediante programación a las casillas de verificación en el código para determinar si se activa una casilla determinada, o para cambiar el estado de activación.

Este tutorial y la última de ellas examinando agregando una columna de selector de fila en GridView. En nuestro tutorial siguiente examinaremos cómo, con un poco de trabajo, podemos agregar funciones de inserción a la GridView.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [ *SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Anterior](adding-a-gridview-column-of-radio-buttons-vb.md)
[Siguiente](inserting-a-new-record-from-the-gridview-s-footer-vb.md)
