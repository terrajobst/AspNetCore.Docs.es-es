---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-vb
title: Personalizar el control DataList de la edición de interfaz (VB) | Microsoft Docs
author: rick-anderson
description: En este tutorial crearemos una interfaz de edición más rica para el control DataList, que incluye una casilla de verificación y listas desplegables.
ms.author: aspnetcontent
ms.date: 10/30/2006
ms.assetid: 718628e2-224c-455f-b33a-a41efd48d5a0
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 78001e977a4696e905317eab35604518d059e66d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828762"
---
<a name="customizing-the-datalists-editing-interface-vb"></a>Personalizar la interfaz de edición de DataList (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_40_VB.exe) o [descargar PDF](customizing-the-datalist-s-editing-interface-vb/_static/datatutorial40vb1.pdf)

> En este tutorial crearemos una interfaz de edición más rica para el control DataList, que incluye una casilla de verificación y listas desplegables.


## <a name="introduction"></a>Introducción

El marcado y los controles Web en el control DataList s `EditItemTemplate` definir su interfaz editable. En todos los ejemplos de DataList editables se ve hasta ahora, examina el editable se ha creado la interfaz de controles de cuadro de texto Web. En el [tutorial anterior](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md) se ha mejorado la experiencia del usuario de la hora de modificación mediante la adición de controles de validación.

El `EditItemTemplate` puede ampliarse para incluir controles Web que no sea el cuadro de texto, como listas desplegables, RadioButtonList, calendarios y así sucesivamente. Al igual que con los cuadros de texto, al personalizar la interfaz de edición para incluir otros controles Web, emplear los siguientes pasos:

1. Agregar el control Web a la `EditItemTemplate`.
2. Use la sintaxis de enlace de datos para asignar el valor del campo de datos correspondiente a la propiedad adecuada.
3. En el `UpdateCommand` controlador de eventos, acceso mediante programación a la Web controlar valor y lo pasa en el método BLL adecuado.

En este tutorial crearemos una interfaz de edición más rica para el control DataList, que incluye una casilla de verificación y listas desplegables. En concreto, vamos a crear un control DataList que muestra información de producto y permite que el nombre de producto s, proveedor, categoría y estado discontinuo actualizarse (consulte la figura 1).


[![La interfaz de edición incluye una casilla de verificación, un cuadro de texto y dos DropDownLists](customizing-the-datalist-s-editing-interface-vb/_static/image2.png)](customizing-the-datalist-s-editing-interface-vb/_static/image1.png)

**Figura 1**: la interfaz de edición incluye una casilla de verificación, un cuadro de texto y dos DropDownLists ([haga clic aquí para ver imagen en tamaño completo](customizing-the-datalist-s-editing-interface-vb/_static/image3.png))


## <a name="step-1-displaying-product-information"></a>Paso 1: Mostrar información de producto

Para poder crear la interfaz editable de DataList s, primero es necesario generar la interfaz de solo lectura. Comience abriendo la `CustomizedUI.aspx` página desde la `EditDeleteDataList` carpeta y, desde el diseñador, agregue un control DataList a la página, estableciendo su `ID` propiedad a `Products`. En la etiqueta inteligente s DataList, cree un nuevo origen ObjectDataSource. Nombre de este nuevo origen ObjectDataSource `ProductsDataSource` y configúrelo para recuperar los datos de la `ProductsBLL` clase s `GetProducts` método. Como con los tutoriales anteriores de DataList editables, actualizaremos la información de producto modificada s, vaya directamente a la capa de lógica empresarial. En consecuencia, establezca las listas desplegables en la actualización, INSERCIÓN y elimine las pestañas en (None).


[![Establecer las listas desplegables de fichas DELETE, INSERT y UPDATE (ninguna)](customizing-the-datalist-s-editing-interface-vb/_static/image5.png)](customizing-the-datalist-s-editing-interface-vb/_static/image4.png)

**Figura 2**: establecer la actualización, INSERCIÓN y listas desplegables de las pestañas eliminar en (None) ([haga clic aquí para ver imagen en tamaño completo](customizing-the-datalist-s-editing-interface-vb/_static/image6.png))


Después de configurar el origen ObjectDataSource, Visual Studio creará una predeterminada `ItemTemplate` para el control DataList que muestra el nombre y valor para cada uno de los campos de datos devueltos. Modificar el `ItemTemplate` para que la plantilla muestra el nombre de producto en un `<h4>` elemento junto con el nombre de categoría, nombre de proveedor, precio y estado discontinuo. Además, agregue un botón de edición, asegurándose de que su `CommandName` propiedad está establecida en Editar. El marcado declarativo para mi `ItemTemplate` sigue:


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample1.aspx)]

El marcado anterior presenta la información de producto mediante un &lt;h4&gt; encabezado para el nombre de producto s y cuatro columnas `<table>` para los campos restantes. El `ProductPropertyLabel` y `ProductPropertyValue` clases CSS, definidas en `Styles.css`, se han explicado en los tutoriales anteriores. Figura 3 muestra nuestro progreso cuando se ve mediante un explorador.


[![Se muestra el nombre de proveedor, categoría, estado suspendido y precio de cada producto](customizing-the-datalist-s-editing-interface-vb/_static/image8.png)](customizing-the-datalist-s-editing-interface-vb/_static/image7.png)

**Figura 3**: se muestra el nombre del proveedor, categoría, estado suspendido y precio de cada producto ([haga clic aquí para ver imagen en tamaño completo](customizing-the-datalist-s-editing-interface-vb/_static/image9.png))


## <a name="step-2-adding-the-web-controls-to-the-editing-interface"></a>Paso 2: Agregar los controles Web a la interfaz de edición

El primer paso para crear el control DataList personalizado interfaz de edición consiste en agregar los controles Web necesarios para el `EditItemTemplate`. En concreto, necesitamos un DropDownList para la categoría, otro para el proveedor y una casilla de verificación para el estado no incluido. Puesto que el precio del producto s no es editable en este ejemplo, podemos seguimos mostrarla mediante un control Web Label.

Para personalizar la interfaz de edición, haga clic en el vínculo Editar plantillas de la etiqueta inteligente de DataList s y elija el `EditItemTemplate` opción en la lista desplegable. Agregar un DropDownList para la `EditItemTemplate` y establezca su `ID` a `Categories`.


[![Agregar un DropDownList para las categorías](customizing-the-datalist-s-editing-interface-vb/_static/image11.png)](customizing-the-datalist-s-editing-interface-vb/_static/image10.png)

**Figura 4**: agregar un DropDownList para las categorías ([haga clic aquí para ver imagen en tamaño completo](customizing-the-datalist-s-editing-interface-vb/_static/image12.png))


A continuación, en la etiqueta inteligente de DropDownList s, seleccione la opción de Elegir origen de datos y crear un nuevo origen ObjectDataSource denominado `CategoriesDataSource`. Configurar este origen ObjectDataSource para usar el `CategoriesBLL` clase s `GetCategories()` (método) (consulte la figura 5). A continuación, la s DropDownList solicita de Asistente para configuración de origen de datos para que los campos de datos para cada uno `ListItem` s `Text` y `Value` propiedades. Tiene la presentación de DropDownList el `CategoryName` campo de datos y use el `CategoryID` como el valor, como se muestra en la figura 6.


[![Crear un nuevo origen ObjectDataSource denominado CategoriesDataSource](customizing-the-datalist-s-editing-interface-vb/_static/image14.png)](customizing-the-datalist-s-editing-interface-vb/_static/image13.png)

**Figura 5**: crear un nuevo origen ObjectDataSource denominado `CategoriesDataSource` ([haga clic aquí para ver imagen en tamaño completo](customizing-the-datalist-s-editing-interface-vb/_static/image15.png))


[![Configurar la pantalla de s DropDownList y campos de valor](customizing-the-datalist-s-editing-interface-vb/_static/image17.png)](customizing-the-datalist-s-editing-interface-vb/_static/image16.png)

**Figura 6**: configurar los campos de valor y s DropDownList para mostrar ([haga clic aquí para ver imagen en tamaño completo](customizing-the-datalist-s-editing-interface-vb/_static/image18.png))


Repita esta serie de pasos para crear un DropDownList para los proveedores. Establecer el `ID` para este DropDownList para `Suppliers` y el nombre de su origen ObjectDataSource `SuppliersDataSource`.

Después de agregar los dos DropDownLists, agregar una casilla de verificación para el estado discontinuo y un cuadro de texto para el nombre del producto. Establecer el `ID` s para la casilla de verificación y cuadro de texto para `Discontinued` y `ProductName`, respectivamente. Agregue un control RequiredFieldValidator para asegurarse de que el usuario proporcione un valor para el nombre del producto s.

Por último, agregue los botones Actualizar y Cancelar. Recuerde que para estos dos botones es imperativo que sus `CommandName` propiedades se establecen para actualizar y Cancelar, respectivamente.

No dude en diseñar la interfaz de edición que desee. Se ve optado por usar la misma columna de cuatro `<table>` ilustra el diseño de la interfaz de solo lectura, como la siguiente sintaxis declarativa y la captura de pantalla:


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample2.aspx)]


[![La interfaz de edición está definido fuera como la interfaz de solo lectura](customizing-the-datalist-s-editing-interface-vb/_static/image20.png)](customizing-the-datalist-s-editing-interface-vb/_static/image19.png)

**Figura 7**: la interfaz de edición es disponer horizontal como la interfaz de solo lectura ([haga clic aquí para ver imagen en tamaño completo](customizing-the-datalist-s-editing-interface-vb/_static/image21.png))


## <a name="step-3-creating-the-editcommand-and-cancelcommand-event-handlers"></a>Paso 3: Crear el EditCommand y controladores de eventos de CancelCommand

Actualmente, no hay ninguna sintaxis de enlace de datos en el `EditItemTemplate` (excepto para el `UnitPriceLabel`, que se copian literalmente del `ItemTemplate`). Vamos a agregar momentáneamente la sintaxis de enlace de datos, pero permiten primera s crear los controladores de eventos para el control DataList s `EditCommand` y `CancelCommand` eventos. Recuerde que la responsabilidad de la `EditCommand` es el controlador de eventos representar la interfaz de edición para el elemento DataList cuyo botón Editar se hizo clic, mientras que el `CancelCommand` s labor consiste en devolver el control DataList a su estado de edición previamente.

Crear estos dos controladores de eventos y hacer que use el código siguiente:


[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample3.vb)]

Con estos dos controladores de eventos en su lugar, al hacer clic en el botón Editar muestra la interfaz de edición y haga clic en el botón de cancelación devuelve el elemento editado a su modo de solo lectura. Figura 8 muestra al control DataList después de que se ha presionado el botón Editar para Chef Antón s tártara. Desde que se ve aún para agregar cualquier sintaxis de enlace de datos a la interfaz de edición, el `ProductName` cuadro de texto está en blanco, el `Discontinued` casilla desactivada y los primeros elementos seleccionan de la `Categories` y `Suppliers` DropDownLists.


[![Al hacer clic en el botón de edición se muestra la interfaz de edición](customizing-the-datalist-s-editing-interface-vb/_static/image23.png)](customizing-the-datalist-s-editing-interface-vb/_static/image22.png)

**Figura 8**: al hacer clic en el botón de edición muestra la interfaz de edición ([haga clic aquí para ver imagen en tamaño completo](customizing-the-datalist-s-editing-interface-vb/_static/image24.png))


## <a name="step-4-adding-the-databinding-syntax-to-the-editing-interface"></a>Paso 4: Agregar la sintaxis de enlace de datos a la interfaz de edición

Para que la interfaz de edición de mostrar los valores actuales de s de producto, es necesario utilizar la sintaxis de enlace de datos para asignar los valores de campo de datos a los valores adecuados de control Web. La sintaxis de enlace de datos se pueden aplicar a través del diseñador para ello, vaya a la pantalla Editar plantillas y seleccionar el vínculo Editar DataBindings de la Web controla las etiquetas inteligentes. Como alternativa, la sintaxis de enlace de datos se pueden agregar directamente en el marcado declarativo.

Asignar el `ProductName` valor al campo de datos el `ProductName` s del cuadro de texto `Text` propiedad, el `CategoryID` y `SupplierID` a los valores del campo de datos el `Categories` y `Suppliers` DropDownLists `SelectedValue` propiedades y la `Discontinued` valor al campo de datos el `Discontinued` casilla s `Checked` propiedad. Después de realizar estos cambios, ya sea a través del diseñador o directamente mediante el marcado declarativo, volver a visitar la página a través de un explorador y haga clic en el botón Editar para Chef Antón s tártara. Como se muestra en la figura 9, la sintaxis de enlace de datos ha agregado los valores actuales en el cuadro de texto, listas desplegables y casilla de verificación.


[![Al hacer clic en el botón de edición se muestra la interfaz de edición](customizing-the-datalist-s-editing-interface-vb/_static/image26.png)](customizing-the-datalist-s-editing-interface-vb/_static/image25.png)

**Figura 9**: al hacer clic en el botón de edición muestra la interfaz de edición ([haga clic aquí para ver imagen en tamaño completo](customizing-the-datalist-s-editing-interface-vb/_static/image27.png))


## <a name="step-5-saving-the-user-s-changes-in-the-updatecommand-event-handler"></a>Paso 5: Guardar los cambios de usuario s en el controlador de eventos UpdateCommand

Cuando el usuario edita un producto y hace clic en el botón Actualizar, se produce un postback y el control DataList s `UpdateCommand` desencadena el evento. En el evento controlador, es necesario leer los valores de los controles Web de la `EditItemTemplate` e interactúe con el nivel de lógica empresarial para actualizar el producto en la base de datos. Como se ve visto en los tutoriales anteriores, el `ProductID` del producto actualizado es accesible a través de la `DataKeys` colección. Los campos introducidos por el usuario tiene acceso mediante programación que hacen referencia a los controles Web utilizando `FindControl("controlID")`, como se muestra en el código siguiente:


[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample4.vb)]

El código empieza consultando el `Page.IsValid` propiedad para asegurarse de que todos los controles de validación en la página son válidos. Si `Page.IsValid` es `True`, el producto editado s `ProductID` se lee el valor de la `DataKeys` recopilación y la entrada de datos Web controla en el `EditItemTemplate` mediante programación se hace referencia. A continuación, se leen los valores de estos controles Web en variables que, a continuación, se pasan a la correspondiente `UpdateProduct` sobrecargar. Después de actualizar los datos, se devuelve el control DataList a su estado de edición previamente.

> [!NOTE]
> Se ve omite la excepción que controla la lógica agregada en el [BLL - control y las excepciones de nivel DAL](handling-bll-and-dal-level-exceptions-vb.md) tutorial con el fin de mantener el código y en este ejemplo se centra. Como ejercicio, agregar esta funcionalidad después de completar este tutorial.


## <a name="step-6-handling-null-categoryid-and-supplierid-values"></a>Paso 6: Procesar CategoryID NULL y los valores de SupplierID

Permite que la base de datos Northwind para `NULL` valores para el `Products` tabla s `CategoryID` y `SupplierID` columnas. Sin embargo, actualmente acomodar nuestra edición t de interfaz `NULL` valores. Si se intenta editar un producto que tiene un `NULL` valor para cualquiera su `CategoryID` o `SupplierID` columnas, obtendremos un `ArgumentOutOfRangeException` con un mensaje de error similar a: *"Categories" tiene un SelectedValue que no es válida porque hace no existe en la lista de elementos.* Además, s hay actualmente ninguna forma de cambiar una categoría de producto s o proveedor valor desde el que no es`NULL` valor a un `NULL` uno.

Para admitir `NULL` valores para la categoría y proveedor DropDownLists, necesitamos agregar otro `ListItem`. Se ve elegido usar (ninguno) como el `Text` valor para este `ListItem`, pero puede cambiar a otra cosa (por ejemplo, una cadena vacía) si d le gusta. Por último, recuerde establecer la DropDownLists `AppendDataBoundItems` a `True`; si olvida hacerlo, las categorías y proveedores enlazados al control DropDownList sobrescribirá estáticamente agregado `ListItem`.

Después de realizar estos cambios, el marcado de listas desplegables en el control DataList s `EditItemTemplate` debe ser similar al siguiente:


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample5.aspx)]

> [!NOTE]
> Estática `ListItem` s pueden agregarse a un DropDownList a través del diseñador o directamente a través de la sintaxis declarativa. Al agregar un elemento de DropDownList para representar una base de datos `NULL` de valor, no olvide agregar el `ListItem` a través de la sintaxis declarativa. Si usas el `ListItem` Editor de colecciones en el diseñador, la sintaxis declarativa generada omitirá el `Value` valor por completo cuando asigna una cadena en blanco, creando el marcado declarativo, como: `<asp:ListItem>(None)</asp:ListItem>`. Aunque esto puede parecer inofensivo, la falta `Value` hace que la DropDownList usar el `Text` valor de propiedad en su lugar. Que significa que si esto `NULL` `ListItem` está seleccionada, el valor (ninguno) se volverá a intentar que se asignará al campo de datos de producto (`CategoryID` o `SupplierID`, en este tutorial), lo que producirá una excepción. Si se establece explícitamente `Value=""`, un `NULL` se asignará el valor para el producto del campo de datos cuando el `NULL` `ListItem` está seleccionada.


Dedique un momento para ver nuestro progreso a través de un explorador. Al editar un producto, tenga en cuenta que el `Categories` y `Suppliers` DropDownLists ambos tienen un (ninguno) opción al principio de la DropDownList.


[![Las categorías y proveedores DropDownLists (None) opción incluir](customizing-the-datalist-s-editing-interface-vb/_static/image29.png)](customizing-the-datalist-s-editing-interface-vb/_static/image28.png)

**Figura 10**: el `Categories` y `Suppliers` DropDownLists (None) opción Incluir ([haga clic aquí para ver imagen en tamaño completo](customizing-the-datalist-s-editing-interface-vb/_static/image30.png))


Para guardar el (ninguno) opción como una base de datos `NULL` valor, es necesario volver a la `UpdateCommand` controlador de eventos. Cambiar el `categoryIDValue` y `supplierIDValue` variables de enteros que acepta valores NULL y asignarles un valor distinto de `Nothing` solo si la s DropDownList `SelectedValue` no es una cadena vacía:


[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample6.vb)]

Con este cambio, un valor de `Nothing` se pasarán a la `UpdateProduct` opción método BLL, si el usuario seleccionó el (ninguno) en cualquiera de las listas desplegables, que corresponde a un `NULL` valor de la base de datos.

## <a name="summary"></a>Resumen

En este tutorial hemos visto cómo crear una interfaz de edición de DataList más compleja que incluye tres controles Web de entrada diferentes de un control TextBox, dos DropDownLists y una casilla de verificación junto con los controles de validación. Al compilar la interfaz de edición, los pasos son los mismos independientemente de los controles Web que se va a usar: empiece por agregar los controles Web para el control DataList s `EditItemTemplate`; use la sintaxis de enlace de datos para asignar los valores de campo de datos correspondiente con el Web apropiado propiedades del control; y, en el `UpdateCommand` controlador de eventos, acceso mediante programación a los controles Web y sus propiedades adecuados, pasando los valores en la capa BLL.

Al crear una interfaz de edición, si lo s consta sólo de los cuadros de texto o una colección de controles Web diferentes, debe asegurarse de controlar correctamente la base de datos `NULL` valores. Cuando se trata de `NULL` s, es imperativo que no sólo correctamente mostrar existente `NULL` valor en la interfaz de edición, pero también que proporcionan un medio para marcar un valor como `NULL`. Para listas desplegables en DataList, que normalmente significa agregar estático `ListItem` cuyo `Value` propiedad se establece explícitamente en una cadena vacía (`Value=""`) y agrega un poco de código para el `UpdateCommand` controlador de eventos para determinar si el `NULL``ListItem` se ha seleccionado.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Los revisores para este tutorial fueron Randy Schmidt, Dennis Patterson y David Suru. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md)
