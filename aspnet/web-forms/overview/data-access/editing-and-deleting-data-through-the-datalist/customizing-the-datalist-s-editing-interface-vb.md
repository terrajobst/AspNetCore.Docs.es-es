---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-vb
title: "Personalizar el control DataList de la edición de interfaz (VB) | Documentos de Microsoft"
author: rick-anderson
description: "En este tutorial se creará una interfaz de edición más enriquecida para el control DataList, que incluye una casilla de verificación y listas desplegables."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 718628e2-224c-455f-b33a-a41efd48d5a0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: bd31c18b9fced12feeda8ca8dea7dace0ef63573
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="customizing-the-datalists-editing-interface-vb"></a>Personalizar la interfaz de edición del control DataList (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_40_VB.exe) o [descarga de PDF](customizing-the-datalist-s-editing-interface-vb/_static/datatutorial40vb1.pdf)

> En este tutorial se creará una interfaz de edición más enriquecida para el control DataList, que incluye una casilla de verificación y listas desplegables.


## <a name="introduction"></a>Introducción

El marcado y los controles Web de DataList s `EditItemTemplate` definir su interfaz editable. En todos los ejemplos de DataList editables se ve hasta el momento, examina el editable se ha creado la interfaz de controles Web de cuadro de texto. En el [tutorial anterior](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md) se ha mejorado la experiencia del usuario de tiempo de edición mediante la adición de controles de validación.

La `EditItemTemplate` puede ampliarse para incluir controles Web que no sea el cuadro de texto, como listas desplegables, RadioButtonList, calendarios y así sucesivamente. Al igual que con los cuadros de texto, al personalizar la interfaz de edición para incluir otros controles Web, emplear los siguientes pasos:

1. Agregar el control de Web a la `EditItemTemplate`.
2. Use la sintaxis de enlace de datos para asignar el valor del campo de datos correspondiente a la propiedad adecuada.
3. En el `UpdateCommand` controlador de eventos, acceso mediante programación a la Web controlar valor y lo pasa en el método BLL adecuado.

En este tutorial se creará una interfaz de edición más enriquecida para el control DataList, que incluye una casilla de verificación y listas desplegables. En concreto, vamos a crear un control DataList que muestra información de producto y permite que el nombre de producto s, proveedor, categoría y estado discontinuo actualizarse (consulte la figura 1).


[![La interfaz de edición incluye un cuadro de texto y dos listas desplegables, una casilla de verificación](customizing-the-datalist-s-editing-interface-vb/_static/image2.png)](customizing-the-datalist-s-editing-interface-vb/_static/image1.png)

**Figura 1**: la interfaz de edición incluye un cuadro de texto y dos listas desplegables, una casilla de verificación ([haga clic aquí para ver la imagen a tamaño completo](customizing-the-datalist-s-editing-interface-vb/_static/image3.png))


## <a name="step-1-displaying-product-information"></a>Paso 1: Mostrar información de producto

Antes de que podemos crear la interfaz editable de DataList s, primero es necesario generar la interfaz de solo lectura. Comience abriendo la `CustomizedUI.aspx` página desde el `EditDeleteDataList` carpeta y, en el diseñador, agregue un control DataList a la página, estableciendo su `ID` propiedad a `Products`. Desde la etiqueta inteligente de DataList s, cree un nuevo origen ObjectDataSource. Nombre de esta nueva ObjectDataSource `ProductsDataSource` y configúrelo para recuperar los datos de la `ProductsBLL` clase s. `GetProducts` método. Como con los tutoriales anteriores de DataList editables, actualizaremos la información de producto editado s, vaya directamente a la capa de lógica de negocios. En consecuencia, establecer las listas desplegables en la actualización, INSERCIÓN y eliminar las fichas (ninguno).


[![Establece las listas de lista desplegable UPDATE, INSERT y DELETE pestañas en (ninguno)](customizing-the-datalist-s-editing-interface-vb/_static/image5.png)](customizing-the-datalist-s-editing-interface-vb/_static/image4.png)

**Figura 2**: establézcalo UPDATE, INSERT y eliminar listas de lista desplegable de pestañas (ninguno) ([haga clic aquí para ver la imagen a tamaño completo](customizing-the-datalist-s-editing-interface-vb/_static/image6.png))


Después de configurar el ObjectDataSource, Visual Studio creará el valor predeterminado es `ItemTemplate` para el control DataList que enumera el nombre y valor para cada uno de los campos de datos devueltos. Modificar el `ItemTemplate` para que la plantilla muestra el nombre de producto en un `<h4>` elemento junto con el nombre de categoría, el nombre de proveedor, el precio y el estado no incluida. Además, agregue un botón Editar, asegurándose de que su `CommandName` propiedad se establece en Editar. El marcado declarativo para mi `ItemTemplate` sigue:


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample1.aspx)]

El marcado anterior distribuye la información de producto mediante un &lt;h4&gt; encabezado para el nombre de producto s y cuatro columnas `<table>` para los campos restantes. El `ProductPropertyLabel` y `ProductPropertyValue` clases CSS, definidas en `Styles.css`, se han explicado en tutoriales anteriores. La figura 3 muestra nuestro progreso cuando se ven a través de un explorador.


[![Se muestra el nombre de proveedor, categoría, estado suspendido y precio de cada producto](customizing-the-datalist-s-editing-interface-vb/_static/image8.png)](customizing-the-datalist-s-editing-interface-vb/_static/image7.png)

**Figura 3**: muestra el nombre, proveedor, categoría, estado suspendido y el precio de cada producto ([haga clic aquí para ver la imagen a tamaño completo](customizing-the-datalist-s-editing-interface-vb/_static/image9.png))


## <a name="step-2-adding-the-web-controls-to-the-editing-interface"></a>Paso 2: Agregar los controles Web a la interfaz de edición

El primer paso en la creación de DataList personalizado interfaz edición consiste en agregar los controles Web necesarios para el `EditItemTemplate`. En concreto, necesitamos un DropDownList para la categoría, otro para el proveedor y una casilla de verificación para el estado no incluido. Puesto que el precio del producto s no es editable en este ejemplo, podemos seguir mostrar mediante un control Web Label.

Para personalizar la interfaz de edición, haga clic en el vínculo Editar plantillas de la etiqueta inteligente de DataList s y elija la `EditItemTemplate` opción en la lista desplegable. Agregar un DropDownList para la `EditItemTemplate` y establecer su `ID` a `Categories`.


[![Agregar un DropDownList para las categorías](customizing-the-datalist-s-editing-interface-vb/_static/image11.png)](customizing-the-datalist-s-editing-interface-vb/_static/image10.png)

**Figura 4**: agregar un DropDownList para las categorías ([haga clic aquí para ver la imagen a tamaño completo](customizing-the-datalist-s-editing-interface-vb/_static/image12.png))


A continuación, desde la etiqueta inteligente de DropDownList s, seleccione la opción de Elegir origen de datos y crear un nuevo origen ObjectDataSource denominado `CategoriesDataSource`. Configurar este ObjectDataSource para usar el `CategoriesBLL` clase s. `GetCategories()` método (Véase la figura 5). A continuación, las operaciones de asignación de DropDownList solicita de Asistente para configuración de orígenes de datos para que los campos de datos para cada una `ListItem` s `Text` y `Value` propiedades. Tiene la presentación de DropDownList el `CategoryName` campo de datos y use el `CategoryID` como el valor, como se muestra en la figura 6.


[![Crear un nuevo origen ObjectDataSource denominado CategoriesDataSource](customizing-the-datalist-s-editing-interface-vb/_static/image14.png)](customizing-the-datalist-s-editing-interface-vb/_static/image13.png)

**Figura 5**: crear un nuevo ObjectDataSource denominado `CategoriesDataSource` ([haga clic aquí para ver la imagen a tamaño completo](customizing-the-datalist-s-editing-interface-vb/_static/image15.png))


[![Configurar la pantalla de s DropDownList y campos de valor](customizing-the-datalist-s-editing-interface-vb/_static/image17.png)](customizing-the-datalist-s-editing-interface-vb/_static/image16.png)

**Figura 6**: configurar los campos de valor y DropDownList s Mostrar ([haga clic aquí para ver la imagen a tamaño completo](customizing-the-datalist-s-editing-interface-vb/_static/image18.png))


Repita esta serie de pasos para crear un DropDownList para los proveedores. Establecer el `ID` para este DropDownList a `Suppliers` y el nombre de su ObjectDataSource `SuppliersDataSource`.

Después de agregar las listas dos desplegables, agregue una casilla de verificación para el estado no incluido y un cuadro de texto para el nombre del producto s. Establecer el `ID` para ver si la casilla de verificación y cuadro de texto a `Discontinued` y `ProductName`, respectivamente. Agregue un control RequiredFieldValidator para asegurarse de que el usuario proporciona un valor para el nombre del producto s.

Por último, agregue los botones Actualizar y Cancelar. Recuerde que para estos dos botones es imperativo que su `CommandName` se establecen propiedades para actualizar y Cancelar, respectivamente.

No dude en diseñar la interfaz de edición sin embargo igual. Se ha elegido para utilizar las mismas cuatro columnas `<table>` muestra el diseño de la interfaz de solo lectura, como la siguiente sintaxis declarativa y la captura de pantalla:


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample2.aspx)]


[![La interfaz de edición es disponen de salida, al igual que la interfaz de solo lectura](customizing-the-datalist-s-editing-interface-vb/_static/image20.png)](customizing-the-datalist-s-editing-interface-vb/_static/image19.png)

**Figura 7**: es la interfaz de edición debe disponer de salida, al igual que la interfaz de solo lectura ([haga clic aquí para ver la imagen a tamaño completo](customizing-the-datalist-s-editing-interface-vb/_static/image21.png))


## <a name="step-3-creating-the-editcommand-and-cancelcommand-event-handlers"></a>Paso 3: Crear las EditCommand y los controladores de evento CancelCommand

Actualmente, no hay ninguna sintaxis de enlace de datos en el `EditItemTemplate` (excepto para la `UnitPriceLabel`, que se copian literalmente de la `ItemTemplate`). Vamos a agregar la sintaxis de enlace de datos en breve, pero permiten primera s crear los controladores de eventos para el control DataList s `EditCommand` y `CancelCommand` eventos. Recuerde que la responsabilidad de la `EditCommand` es el controlador de eventos representar la interfaz de edición para el elemento DataList cuyo botón Editar se hizo clic, mientras que la `CancelCommand` trabajo s que se va a devolver el control DataList a su estado anterior edición.

Cree estos dos controladores de eventos y hacer que usen el código siguiente:


[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample3.vb)]

Con estos dos controladores de eventos en su lugar, haga clic en el botón Editar muestra la interfaz de edición y haga clic en el botón Cancelar devuelve el elemento editado en su modo de solo lectura. La figura 8 muestra al control DataList después de que se ha hecho clic el botón Editar para Chef Anton s Gumbo Mix. Desde que se ve todavía para agregar cualquier sintaxis de enlace de datos a la interfaz de edición, el `ProductName` cuadro de texto está en blanco, el `Discontinued` casilla desactivada y los primeros elementos seleccionan desde la `Categories` y `Suppliers` listas desplegables.


[![Haga clic en el botón modificar, aparece la interfaz de edición](customizing-the-datalist-s-editing-interface-vb/_static/image23.png)](customizing-the-datalist-s-editing-interface-vb/_static/image22.png)

**Figura 8**: haga clic en el botón Editar muestra la interfaz de edición ([haga clic aquí para ver la imagen a tamaño completo](customizing-the-datalist-s-editing-interface-vb/_static/image24.png))


## <a name="step-4-adding-the-databinding-syntax-to-the-editing-interface"></a>Paso 4: Agregar la sintaxis de enlace de datos a la interfaz de edición

Para hacer la interfaz de edición que muestre los valores actuales de s de producto, es necesario utilizar la sintaxis de enlace de datos para asignar los valores de campo de datos a los valores adecuados de control Web. La sintaxis de enlace de datos se puede aplicar mediante el diseñador, vaya a la pantalla Editar plantillas y seleccionar el vínculo Editar DataBindings de la Web controla las etiquetas inteligentes. Como alternativa, la sintaxis de enlace de datos pueden agregarse directamente en el marcado declarativo.

Asigne el `ProductName` valor al campo de datos el `ProductName` s de cuadro de texto `Text` propiedad, el `CategoryID` y `SupplierID` valores del campo de datos el `Categories` y `Suppliers` listas desplegables `SelectedValue` propiedades y la `Discontinued` valor al campo de datos el `Discontinued` casilla s `Checked` propiedad. Después de realizar estos cambios, ya sea a través del diseñador o directamente mediante el marcado declarativo, volver a visitar la página a través de un explorador y haga clic en el botón Editar para Chef Anton s Gumbo Mix. Como se muestra en la figura 9, la sintaxis de enlace de datos ha agregado los valores actuales en el cuadro de texto, listas desplegables y casilla de verificación.


[![Haga clic en el botón modificar, aparece la interfaz de edición](customizing-the-datalist-s-editing-interface-vb/_static/image26.png)](customizing-the-datalist-s-editing-interface-vb/_static/image25.png)

**Figura 9**: haga clic en el botón Editar muestra la interfaz de edición ([haga clic aquí para ver la imagen a tamaño completo](customizing-the-datalist-s-editing-interface-vb/_static/image27.png))


## <a name="step-5-saving-the-user-s-changes-in-the-updatecommand-event-handler"></a>Paso 5: Guardar los cambios de s de usuario en el controlador de eventos de UpdateCommand

Cuando el usuario edita un producto y hace clic en el botón de actualización, se produce un postback y DataList s `UpdateCommand` desencadena el evento. En el evento controlador, se tiene que leer los valores de los controles Web de la `EditItemTemplate` y comunicarse con la capa BLL para actualizar el producto en la base de datos. Como se ha visto en tutoriales anteriores, el `ProductID` del producto actualizado es accesible a través del `DataKeys` colección. Los campos introducidos por el usuario tiene acceso mediante programación que hacen referencia a los controles Web mediante `FindControl("controlID")`, como se muestra en el código siguiente:


[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample4.vb)]

El código se inicia consultando la `Page.IsValid` propiedad para asegurarse de que todos los controles de validación en la página son válidos. Si `Page.IsValid` es `True`, el producto editado s `ProductID` se lee el valor de la `DataKeys` recopilación y la entrada de datos Web controla en el `EditItemTemplate` mediante programación se hace referencia. A continuación, se leen los valores de estos controles Web en variables que, a continuación, se pasan a la correspondiente `UpdateProduct` sobrecarga. Después de actualizar los datos, se devuelve el control DataList a su estado anterior de edición.

> [!NOTE]
> Se ha omitido la lógica agregada en de control de excepciones la [BLL - control y excepciones de nivel de la capa DAL](handling-bll-and-dal-level-exceptions-vb.md) tutorial con el fin de mantener el código y este ejemplo centrado. Como ejercicio, agregar esta funcionalidad después de completar este tutorial.


## <a name="step-6-handling-null-categoryid-and-supplierid-values"></a>Paso 6: Procesar CategoryID NULL y los valores de SupplierID

Permite que la base de datos Northwind para `NULL` los valores para la `Products` tabla s `CategoryID` y `SupplierID` columnas. Sin embargo, actualmente alojar nuestro edición t de interfaz `NULL` valores. Si se intenta editar un producto que tiene un `NULL` valor para cada uno su `CategoryID` o `SupplierID` columnas, obtendremos un `ArgumentOutOfRangeException` con un mensaje de error similar al: *'Categorías' tiene un SelectedValue que no es válido porque lleva a cabo no existe en la lista de elementos.* Además, s hay actualmente ninguna manera de cambiar una categoría de producto s o proveedor valor desde no`NULL` valor a un `NULL` uno.

Para admitir `NULL` valores para la categoría y el proveedor listas desplegables, tenemos que agregar más `ListItem`. ¿Ha decidido utilizar (ninguno) como el `Text` valor para este `ListItem`, pero puede cambiarlo por otro (por ejemplo, una cadena vacía) si d le gusta. Por último, recuerde establecer las listas desplegables `AppendDataBoundItems` a `True`; si se olvida de hacerlo, las categorías y proveedores enlazados a los controles DropDownList sobrescribirá estáticamente agregado `ListItem`.

Después de realizar estos cambios, el marcado de listas desplegables en el control DataList s `EditItemTemplate` debe ser similar al siguiente:


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample5.aspx)]

> [!NOTE]
> Estática `ListItem` s pueden agregarse a DropDownList a través del diseñador o directamente a través de la sintaxis declarativa. Al agregar un elemento de DropDownList para representar una base de datos `NULL` valor, no olvide agregar la `ListItem` a través de la sintaxis declarativa. Si usas el `ListItem` Editor de colecciones en el diseñador, la sintaxis declarativa generada omitirá la `Value` valor por completo cuando asigna una cadena en blanco, crear marcado declarativo como: `<asp:ListItem>(None)</asp:ListItem>`. Aunque esto puede parecer inofensivo, el carácter que falta `Value` hace DropDownList usar la `Text` valor de propiedad en su lugar. Que significa que si esto `NULL` `ListItem` está seleccionado, se intentará el valor (ninguno) que se asignará al campo de datos de producto (`CategoryID` o `SupplierID`, en este tutorial), lo que producirá una excepción. Si se establece explícitamente `Value=""`, `NULL` se le asignará el valor para el producto del campo de datos cuando el `NULL` `ListItem` está seleccionada.


Tómese un momento para ver el progreso a través de un explorador. Al editar un producto, tenga en cuenta que la `Categories` y `Suppliers` listas desplegables ambos tienen un (ninguno) opción al principio de DropDownList.


[![Las categorías y las listas desplegables de proveedores incluyen un (ninguno) opción](customizing-the-datalist-s-editing-interface-vb/_static/image29.png)](customizing-the-datalist-s-editing-interface-vb/_static/image28.png)

**Figura 10**: el `Categories` y `Suppliers` listas desplegables incluyen un (ninguno) opción ([haga clic aquí para ver la imagen a tamaño completo](customizing-the-datalist-s-editing-interface-vb/_static/image30.png))


Para guardar el (ninguno) opción como una base de datos `NULL` valor, es necesario volver a la `UpdateCommand` controlador de eventos. Cambiar el `categoryIDValue` y `supplierIDValue` variables de enteros que aceptan valores NULL y asignarles un valor distinto de `Nothing` solo si las operaciones de asignación de DropDownList `SelectedValue` no es una cadena vacía:


[!code-vb[Main](customizing-the-datalist-s-editing-interface-vb/samples/sample6.vb)]

Con este cambio, un valor de `Nothing` pasarán a la `UpdateProduct` opción método BLL si el usuario selecciona el (ninguno) de cualquiera de las listas desplegables, que se corresponde con un `NULL` valor de la base de datos.

## <a name="summary"></a>Resumen

En este tutorial, hemos visto cómo crear una interfaz de edición de DataList más compleja que incluye tres controles Web de entrada diferentes de un cuadro de texto y dos listas desplegables, una casilla de verificación junto con los controles de validación. Cuando se crea la interfaz de edición, los pasos son los mismos, independientemente de los controles Web que se va a usar: iniciar mediante la adición de los controles Web al control DataList s `EditItemTemplate`; use sintaxis de enlace de datos para asignar los valores de campo de datos correspondiente a la Web adecuada propiedades del control; y, en la `UpdateCommand` controlador de eventos, acceso mediante programación a los controles Web y sus propiedades adecuados, pasando los valores en la capa BLL.

Al crear una interfaz de edición, si lo s formada simplemente cuadros de texto o una colección de controles Web diferentes, asegúrese de tratar correctamente la base de datos `NULL` valores. Cuando se trata de `NULL` s, es imperativo que no solo correctamente mostrar existente `NULL` valor en la interfaz de edición, pero también que proporcionan un medio para marcar un valor como `NULL`. Para listas desplegables en DataList, que normalmente significa agregar una variable static `ListItem` cuyo `Value` propiedad se establece explícitamente en una cadena vacía (`Value=""`) y agregando un poco de código para el `UpdateCommand` controlador de eventos para determinar si el `NULL``ListItem` se ha seleccionado.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [ *SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Los revisores iniciales para este tutorial eran Dennis Patterson, David Suru y Randy Schmidt. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md)
