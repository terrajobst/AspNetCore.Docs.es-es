---
uid: web-forms/overview/data-access/working-with-batched-data/batch-updating-vb
title: Actualización (VB) por lotes | Microsoft Docs
author: rick-anderson
description: Obtenga información sobre cómo actualizar varios registros de base de datos en una sola operación. En la capa de interfaz de usuario, creamos un GridView donde cada fila es editable. En los datos...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: d191a204-d7ea-458d-b81c-0b9049ecb55f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-updating-vb
msc.type: authoredcontent
ms.openlocfilehash: 7830e010ea8dbf6ce9bd59154c10eb4c30a3dceb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363318"
---
<a name="batch-updating-vb"></a>Lote de actualización (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_64_VB.zip) o [descargar PDF](batch-updating-vb/_static/datatutorial64vb1.pdf)

> Obtenga información sobre cómo actualizar varios registros de base de datos en una sola operación. En la capa de interfaz de usuario, creamos un GridView donde cada fila es editable. En la capa de acceso a datos se encapsulan las varias operaciones de actualización dentro de una transacción para garantizar que todas las actualizaciones se realizan correctamente o se revierten todas las actualizaciones.


## <a name="introduction"></a>Introducción

En el [tutorial anterior](wrapping-database-modifications-within-a-transaction-vb.md) vimos cómo ampliar la capa de acceso a datos para agregar compatibilidad con transacciones de base de datos. Las transacciones de base de datos garantizan que una serie de instrucciones de modificación de datos se tratará como una operación atómica, lo que garantiza que todas las modificaciones se producirá un error o todos se realizará correctamente. Con esta funcionalidad bajo nivel DAL fuera de la vista, estamos re preparados para prestar atención a la creación de interfaces de modificación de datos de batch.

En este tutorial crearemos un GridView donde cada fila es editable (consulte la figura 1). Puesto que cada fila no se representa en la interfaz de edición, allí s necesidad de una columna de edición, actualizar y cancelar los botones. En su lugar, hay dos botones Actualizar productos en la página que, al hacer clic, enumerar las filas con GridView y actualizar la base de datos.


[![Cada fila en el control GridView es modificable](batch-updating-vb/_static/image1.gif)](batch-updating-vb/_static/image1.png)

**Figura 1**: cada fila en el control GridView es Editable ([haga clic aquí para ver imagen en tamaño completo](batch-updating-vb/_static/image2.png))


Introducción s Let!

> [!NOTE]
> En el [realizar actualizaciones por lotes](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md) tutorial, creamos una edición por lotes de la interfaz mediante el control DataList. Este tutorial difiere el anterior en el que está usa un control GridView y la actualización por lotes se realiza dentro del ámbito de una transacción. Después de completar este tutorial le sugiero que vuelva al tutorial anterior y actualícelo para utilizar la funcionalidad relacionada con la transacción de base de datos agregada en el tutorial anterior.


## <a name="examining-the-steps-for-making-all-gridview-rows-editable"></a>Examen de los pasos para realizar todas las filas del control GridView Editable

Como se describe en el [una visión general de insertar, actualizar y eliminar datos](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) tutorial, el control GridView ofrece compatibilidad integrada para editar sus datos subyacentes por fila. Internamente, el control GridView notas qué fila es editable a través de su [ `EditIndex` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.editindex(VS.80).aspx). Como el control GridView se está enlazando a su origen de datos, comprueba cada fila para ver si el índice de la fila es igual al valor de `EditIndex`. Si es así, las interfaces de esa fila s campos se representan mediante su edición. Para BoundFields, la interfaz de edición es un cuadro de texto cuyo `Text` propiedad se asigna el valor del campo de datos especificado por la s BoundField `DataField` propiedad. Para TemplateFields, el `EditItemTemplate` se utiliza en lugar de la `ItemTemplate`.

Recuerde que el flujo de trabajo de edición comienza cuando un usuario hace clic en un botón de edición de fila s. Esto produce un postback, Establece la s GridView `EditIndex` propiedad en el índice de fila donde ha hecho clic s y Reenlaces los datos a la cuadrícula. Cuando se hace clic un botón de cancelación de la fila s, en el postback el `EditIndex` está establecido en un valor de `-1` antes de volver a enlazar los datos a la cuadrícula. Puesto que las filas de GridView s inicia la indexación a cero, establecer `EditIndex` a `-1` tiene el efecto de mostrar el control GridView en modo de solo lectura.

El `EditIndex` propiedad funciona bien para cada fila de edición, pero no está diseñado para su edición por lotes. Para hacer todo GridView editable, necesitamos que cada fila representar mediante su interfaz de edición. La manera más fácil de hacerlo es crear donde cada campo editable se implementa como se define un TemplateField con su interfaz de edición en el `ItemTemplate`.

En los próximos pasos varias vamos a crear un control GridView editable por completo. En el paso 1 Comenzaremos creando GridView y su origen ObjectDataSource y convertir sus BoundFields y CampoCasillaVerificación TemplateFields. En los pasos 2 y 3 se moverá las interfaces de edición desde la TemplateFields `EditItemTemplate` s para sus `ItemTemplate` s.

## <a name="step-1-displaying-product-information"></a>Paso 1: Mostrar información de producto

Antes de que preocuparse por crear un control GridView donde están las filas es editables, permiten s comience por mostrar simplemente la información del producto. Abra el `BatchUpdate.aspx` página en el `BatchData` carpetas y arrastre un control GridView del cuadro de herramientas hasta el diseñador. Establezca la s GridView `ID` a `ProductsGrid` y, en su etiqueta inteligente, elija para enlazarla a un nuevo origen ObjectDataSource denominado `ProductsDataSource`. Configurar el origen ObjectDataSource para recuperar sus datos desde el `ProductsBLL` clase s `GetProducts` método.


[![Configurar el origen ObjectDataSource para usar la clase ProductsBLL](batch-updating-vb/_static/image2.gif)](batch-updating-vb/_static/image3.png)

**Figura 2**: configurar el origen ObjectDataSource que se usarán el `ProductsBLL` clase ([haga clic aquí para ver imagen en tamaño completo](batch-updating-vb/_static/image4.png))


[![Recuperar los datos de producto mediante el método GetProducts](batch-updating-vb/_static/image3.gif)](batch-updating-vb/_static/image5.png)

**Figura 3**: recuperar los datos de producto mediante la `GetProducts` método ([haga clic aquí para ver imagen en tamaño completo](batch-updating-vb/_static/image6.png))


Como GridView, las características de modificación de ObjectDataSource s están diseñadas para trabajar por fila. Para actualizar un conjunto de registros, debemos escribir algo de código en la clase de código subyacente de s de la página ASP.NET que procesa por lotes los datos y lo pasa a la capa BLL. Por consiguiente, establecer las listas desplegables en las operaciones de asignación ObjectDataSource pestañas UPDATE, INSERT y DELETE en (None). Haga clic en Finalizar para completar al asistente.


[![Establecer las listas desplegables en la actualización, INSERCIÓN y eliminar las fichas en (None)](batch-updating-vb/_static/image4.gif)](batch-updating-vb/_static/image7.png)

**Figura 4**: establecer la lista desplegable se enumeran en la actualización, INSERCIÓN y eliminar pestañas en (None) ([haga clic aquí para ver imagen en tamaño completo](batch-updating-vb/_static/image8.png))


Después de completar al Asistente para configurar orígenes de datos, el marcado declarativo de ObjectDataSource s debería ser similar al siguiente:


[!code-aspx[Main](batch-updating-vb/samples/sample1.aspx)]

Completar al Asistente para configurar orígenes de datos también hace que Visual Studio crear BoundFields y un CampoCasillaVerificación para los campos de datos de producto en el control GridView. Para este tutorial, deje que s solo permite al usuario ver y editar el nombre del producto, categoría, precios y estado no incluida. Quite todos menos a los `ProductName`, `CategoryName`, `UnitPrice`, y `Discontinued` campos y cambie el nombre el `HeaderText` las propiedades de los tres primeros campos de producto, categoría y el precio, respectivamente. Por último, active las casillas de habilitar la paginación y habilitar la ordenación en la etiqueta inteligente de s GridView.

En este momento la GridView tiene tres BoundFields (`ProductName`, `CategoryName`, y `UnitPrice`) y un CampoCasillaVerificación (`Discontinued`). Necesitamos convertir estos cuatro campos en TemplateFields y, a continuación, mueva la interfaz de edición de s TemplateField `EditItemTemplate` a su `ItemTemplate`.

> [!NOTE]
> Exploramos la creación y personalización de TemplateFields en el [personalizar la interfaz de modificación de datos](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) tutorial. Le guiaremos a través de los pasos necesarios para convertir el BoundFields y CampoCasillaVerificación en TemplateFields y definir su edición de las interfaces en sus `ItemTemplate` s, pero si se bloquea o necesita un actualizador, don t dude para hacer referencia a este tutorial anterior.


En la etiqueta inteligente de GridView s, haga clic en el vínculo Edit Columns para abrir el cuadro de diálogo campos. A continuación, seleccione cada campo y haga clic en la función Convert este campo en un vínculo TemplateField.


![Convertir el BoundFields existente y CampoCasillaVerificación TemplateFields](batch-updating-vb/_static/image5.gif)

**Figura 5**: convertir el BoundFields existente y CampoCasillaVerificación TemplateFields


Ahora que cada campo está TemplateField, que está listo para mover la edición de la interfaz desde el `EditItemTemplate` s para el `ItemTemplate` s.

## <a name="step-2-creating-theproductnameunitprice-anddiscontinuedediting-interfaces"></a>Paso 2: Crear el`ProductName`,`UnitPrice`, y`Discontinued`Interfaces de edición

Crear el `ProductName`, `UnitPrice`, y `Discontinued` interfaces de edición son el tema de este paso y son bastante sencillo, como cada interfaz ya está definida en la s TemplateField `EditItemTemplate`. Creación de la `CategoryName` interfaz de edición es un poco más complicado, ya que es necesario crear un DropDownList de las categorías aplicables. Esto `CategoryName` se aborda la interfaz de edición en el paso 3.

Permiten s comenzar con la `ProductName` TemplateField. Haga clic en el vínculo Editar plantillas de la etiqueta inteligente de GridView s y profundice para el `ProductName` TemplateField s `EditItemTemplate`. Seleccione el cuadro de texto, cópielo en el Portapapeles y, a continuación, péguelo en el `ProductName` TemplateField s `ItemTemplate`. Cambiar la s de cuadro de texto `ID` propiedad `ProductName`.

A continuación, agregue un control RequiredFieldValidator a la `ItemTemplate` para asegurarse de que el usuario proporcione un valor para cada nombre de producto s. Establecer el `ControlToValidate` propiedad ProductName, el `ErrorMessage` la propiedad a la debe proporcionar el nombre del producto. y el `Text` propiedad \*. Después de realizar estas adiciones a la `ItemTemplate`, la pantalla debe ser similar a la figura 6.


[![ProductName TemplateField ahora incluye un cuadro de texto y un control RequiredFieldValidator](batch-updating-vb/_static/image6.gif)](batch-updating-vb/_static/image9.png)

**Figura 6**: el `ProductName` TemplateField ahora incluye un cuadro de texto y un control RequiredFieldValidator ([haga clic aquí para ver imagen en tamaño completo](batch-updating-vb/_static/image10.png))


Para el `UnitPrice` interfaz de edición, empiece por copiar en el cuadro de texto de la `EditItemTemplate` a la `ItemTemplate`. A continuación, coloque un carácter $ delante el cuadro de texto y establezca su `ID` propiedad UnitPrice y su `Columns` propiedad a 8.

Agregue también un CompareValidator para los `UnitPrice` s `ItemTemplate` para asegurarse de que el valor especificado por el usuario es un valor de moneda válido mayor que o igual a 0,00 USD. Establece el validador s `ControlToValidate` propiedad UnitPrice, su `ErrorMessage` propiedad a la debe especificar un valor de moneda válidos. Por favor, omita cualquier moneda símbolos., su `Text` propiedad a \*, sus `Type` propiedad a `Currency`, sus `Operator` propiedad a `GreaterThanEqual`y su `ValueToCompare` propiedad en 0.


[![Agregar un control CompareValidator para asegurarse de que el precio especificado es un valor de moneda no negativo](batch-updating-vb/_static/image7.gif)](batch-updating-vb/_static/image11.png)

**Figura 7**: agregar un control CompareValidator para asegurarse de que el precio especificado es un valor de moneda no negativo ([haga clic aquí para ver imagen en tamaño completo](batch-updating-vb/_static/image12.png))


Para el `Discontinued` TemplateField puede utilizar la casilla de verificación ya definido en el `ItemTemplate`. Basta con establecer su `ID` a suspendido y su `Enabled` propiedad `True`.

## <a name="step-3-creating-thecategorynameediting-interface"></a>Paso 3: Crear el`CategoryName`interfaz de edición

La interfaz de edición en el `CategoryName` TemplateField s `EditItemTemplate` contiene un cuadro de texto que muestra el valor de la `CategoryName` campo de datos. Es necesario reemplazar esto con un DropDownList que enumera las categorías posibles.

> [!NOTE]
> El [personalizar la interfaz de modificación de datos](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) tutorial contiene una explicación más exhaustiva y completa acerca de cómo personalizar una plantilla para incluir un DropDownList en lugar de un cuadro de texto. Mientras que finalizan los pasos siguientes, se presentan lacónicamente. Para una información más detallada sobre creación y configuración de las categorías de DropDownList, vuelva a consultar el [personalizar la interfaz de modificación de datos](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) tutorial.


Arrastre DropDownList desde el cuadro de herramientas hasta la `CategoryName` TemplateField s `ItemTemplate`, estableciendo su `ID` a `Categories`. En este momento se suele definiría el origen de datos de listas desplegables s a través de su etiqueta inteligente, crear un nuevo origen ObjectDataSource. Sin embargo, esta acción agregará ObjectDataSource dentro de la `ItemTemplate`, lo que producirá una instancia de origen ObjectDataSource creada para cada fila de GridView. En su lugar, permiten s crear fuera de la s GridView TemplateFields ObjectDataSource. Finalizar la edición de plantillas y arrastre un origen ObjectDataSource desde el cuadro de herramientas hasta el diseñador debajo el `ProductsDataSource` ObjectDataSource. Nombre del nuevo origen ObjectDataSource `CategoriesDataSource` y configúrelo para utilizar el `CategoriesBLL` clase s `GetCategories` método.


[![Configurar el origen ObjectDataSource para usar la clase CategoriesBLL](batch-updating-vb/_static/image8.gif)](batch-updating-vb/_static/image13.png)

**Figura 8**: configurar el origen ObjectDataSource que se usarán el `CategoriesBLL` clase ([haga clic aquí para ver imagen en tamaño completo](batch-updating-vb/_static/image14.png))


[![Recuperar los datos de categoría mediante el método GetCategories](batch-updating-vb/_static/image9.gif)](batch-updating-vb/_static/image15.png)

**Figura 9**: recuperar los datos de categoría mediante la `GetCategories` método ([haga clic aquí para ver imagen en tamaño completo](batch-updating-vb/_static/image16.png))


Puesto que este origen ObjectDataSource se usa simplemente para recuperar los datos, establezca las listas desplegables en las fichas de UPDATE y DELETE en (None). Haga clic en Finalizar para completar al asistente.


[![Conjunto de las listas desplegables en la actualización y eliminación pestañas en (None)](batch-updating-vb/_static/image10.gif)](batch-updating-vb/_static/image17.png)

**Figura 10**: establecer la lista desplegable se enumeran en las fichas de DELETE y UPDATE (ninguna) ([haga clic aquí para ver imagen en tamaño completo](batch-updating-vb/_static/image18.png))


Después de completar el asistente, la `CategoriesDataSource` marcado declarativo s debería ser similar al siguiente:


[!code-aspx[Main](batch-updating-vb/samples/sample2.aspx)]

Con el `CategoriesDataSource` creada y configurada, volver a la `CategoryName` TemplateField s `ItemTemplate` y, en la etiqueta inteligente de DropDownList s, haga clic en el vínculo Elegir origen de datos. En el Asistente para configuración de orígenes de datos, seleccione el `CategoriesDataSource` opción en la primera lista desplegable y elija `CategoryName` utilizan para mostrar y `CategoryID` como valor.


[![Enlazar el CategoriesDataSource DropDownList](batch-updating-vb/_static/image11.gif)](batch-updating-vb/_static/image19.png)

**Figura 11**: enlazar la DropDownList para la `CategoriesDataSource` ([haga clic aquí para ver imagen en tamaño completo](batch-updating-vb/_static/image20.png))


En este momento el `Categories` DropDownList enumera todas las categorías, pero no aún automáticamente y seleccione la categoría apropiada para el producto enlazado a la fila GridView. Para lograr esto es necesario establecer la `Categories` DropDownList s `SelectedValue` al producto s `CategoryID` valor. Haga clic en el vínculo Editar DataBindings de la etiqueta inteligente de DropDownList s y asociar el `SelectedValue` propiedad con el `CategoryID` del campo de datos tal como se muestra en la figura 12.


![Enlazar el valor de Id. de categoría de producto s a la propiedad SelectedValue de s de DropDownList](batch-updating-vb/_static/image12.gif)

**Figura 12**: enlazar el producto s `CategoryID` valor para el s DropDownList `SelectedValue` propiedad


Una última sigue siendo de problema: si el producto tiene un `CategoryID` valor especificado, a continuación, la instrucción de enlace de datos en `SelectedValue` dará como resultado una excepción. Esto es porque la DropDownList contiene solo los elementos de las categorías y no ofrece una opción para aquellos productos que tienen un `NULL` valor para la base de datos `CategoryID`. Para solucionar este problema, establezca la s DropDownList `AppendDataBoundItems` propiedad `True` y agregue un nuevo elemento al control DropDownList, omitiendo la `Value` propiedad de la sintaxis declarativa. Es decir, asegúrese de que el `Categories` sintaxis declarativa de DropDownList s es similar al siguiente:


[!code-aspx[Main](batch-updating-vb/samples/sample3.aspx)]

Tenga en cuenta cómo el `<asp:ListItem Value="">` : seleccione uno--tiene su `Value` atributo se establece explícitamente en una cadena vacía. Vuelva a consultar el [personalizar la interfaz de modificación de datos](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) tutorial para obtener una explicación más exhaustiva sobre por qué se necesita este elemento DropDownList adicional para controlar la `NULL` caso y por qué la asignación de la `Value` propiedad en una cadena vacía es esencial.

> [!NOTE]
> Hay un rendimiento y escalabilidad problema potencial aquí que vale la pena mencionar. Puesto que cada fila tiene un DropDownList que usa el `CategoriesDataSource` como origen de datos, el `CategoriesBLL` clase s `GetCategories` se llamará al método *n* visitan veces por página, donde *n* es el número de filas en GridView. Estos *n* las llamadas a `GetCategories` dar lugar a *n* consultas a la base de datos. Este impacto en la base de datos podría reducirse al almacenar en caché las categorías devueltas en una caché de cada solicitud o a través de la capa de almacenamiento en caché con una dependencia o una expiración de muy corta duración definida de almacenamiento en caché de SQL. Para obtener más información sobre la solicitud por opción, consulte [ `HttpContext.Items` un Store de la memoria caché por solicitud](http://aspnet.4guysfromrolla.com/articles/060904-1.aspx).


## <a name="step-4-completing-the-editing-interface"></a>Paso 4: Completar la interfaz de edición

Hemos llegado una serie de cambios en las plantillas de GridView s sin poner en pausa para ver nuestro progreso. Dedique un momento para ver nuestro progreso a través de un explorador. Como se muestra en la figura 13, cada fila se representa mediante su `ItemTemplate`, que contiene la celda interfaz de edición.


[![Cada fila GridView es modificable](batch-updating-vb/_static/image13.gif)](batch-updating-vb/_static/image21.png)

**Figura 13**: cada fila GridView es Editable ([haga clic aquí para ver imagen en tamaño completo](batch-updating-vb/_static/image22.png))


Hay algunos problemas menores de formato que nosotros nos debemos encargamos de en este momento. En primer lugar, tenga en cuenta que el `UnitPrice` valor contiene cuatro espacios decimales. Para solucionar este problema, vuelva a la `UnitPrice` TemplateField s `ItemTemplate` y, en la etiqueta inteligente s de cuadro de texto, haga clic en el vínculo Editar DataBindings. A continuación, especifique que la `Text` propiedad se debe dar formato como un número.


![Dar formato a la propiedad de texto como un número](batch-updating-vb/_static/image14.gif)

**Figura 14**: formato el `Text` propiedad como un número


En segundo lugar, s permiten centrar la casilla de verificación en la `Discontinued` columna (en lugar de tener que alinea a la izquierda). Haga clic en Editar columnas de la etiqueta inteligente de GridView s y seleccione el `Discontinued` TemplateField en la lista de campos en la esquina inferior izquierda. Explorar en profundidad `ItemStyle` y establezca el `HorizontalAlign` propiedad al centro, como se muestra en la figura 15.


![Centro de la casilla de verificación no incluida](batch-updating-vb/_static/image15.gif)

**Figura 15**: Centro de la `Discontinued` casilla de verificación


A continuación, agregue un control ValidationSummary a la página y establezca su `ShowMessageBox` propiedad `True` y su `ShowSummary` propiedad `False`. Agregar también que controles de botón Web que, al hacer clic en, se actualizará el usuario cambia de s. En concreto, agregue dos controles de botón Web, uno por encima del control GridView y otro después de ella, establecer ambos controles `Text` propiedades a los productos de actualización.

Desde la s GridView la interfaz de edición se define en su TemplateFields `ItemTemplate` s, la `EditItemTemplate` s no son necesarios y pueden eliminarse.

Después de realizar los pasos anteriores, los cambios de formato mencionó, los controles de botón de agregar y quitar el innecesarios `EditItemTemplate` s, su sintaxis declarativa de la página s debería ser similar al siguiente:


[!code-aspx[Main](batch-updating-vb/samples/sample4.aspx)]

Figura 16 se muestra esta página cuando se ve mediante un explorador después de han agregado los controles Web de botón y realizados los cambios de formato.


[![Página ahora incluye dos botones de productos de actualización](batch-updating-vb/_static/image16.gif)](batch-updating-vb/_static/image23.png)

**Figura 16**: la página ahora incluye dos actualización productos botones ([haga clic aquí para ver imagen en tamaño completo](batch-updating-vb/_static/image24.png))


## <a name="step-5-updating-the-products"></a>Paso 5: Actualización de los productos

Cuando un usuario visita esta página que se realicen modificaciones y, a continuación, haga clic en uno de los dos botones de productos de actualización. En ese momento se necesita alguna manera guardar los valores introducidos por el usuario para cada fila en un `ProductsDataTable` de instancia y luego se pasa a un método BLL que, a continuación, pasará que `ProductsDataTable` instancia para el s DAL `UpdateWithTransaction` método. El `UpdateWithTransaction` método, que hemos creado en el [tutorial anterior](wrapping-database-modifications-within-a-transaction-vb.md), garantiza que el lote de cambios se actualizarán como una operación atómica.

Cree un método denominado `BatchUpdate` en `BatchUpdate.aspx.vb` y agregue el código siguiente:


[!code-vb[Main](batch-updating-vb/samples/sample5.vb)]

Este método comienza mediante la obtención de todos los productos en un `ProductsDataTable` mediante una llamada a la s BLL `GetProducts` método. A continuación, enumera el `ProductGrid` GridView s [ `Rows` colección](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows(VS.80).aspx). El `Rows` colección contiene un [ `GridViewRow` instancia](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewrow.aspx) para cada fila que se muestra en el control GridView. Puesto que vamos a presentar como máximo diez filas por página, la s GridView `Rows` colección contará con no más de diez elementos.

Para cada fila del `ProductID` se obtiene de la `DataKeys` adecuado y colección `ProductsRow` está seleccionado en el `ProductsDataTable`. Los cuatro controles de entrada TemplateField mediante programación se hace referencia y sus valores se asignan a la `ProductsRow` s propiedades de la instancia. Después de cada control GridView se usaron los valores de fila s para actualizar la `ProductsDataTable`, se han pasado a la s BLL `UpdateWithTransaction` método que, como hemos visto en el tutorial anterior, simplemente llama a hacia abajo en la s DAL `UpdateWithTransaction` método.

El algoritmo de actualización por lotes utilizado para este tutorial actualiza cada fila de la `ProductsDataTable` que corresponde a una fila en el control GridView, independientemente de si ha cambiado la información del producto s. Mientras estas blind actualiza no son normalmente un problema de rendimiento, puede provocar registros superfluo si se van a auditar los cambios en la tabla de base de datos. En el [realizar actualizaciones por lotes](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md) tutorial hemos explorado una actualización por lotes interfaz con el control DataList y agregado código que va a actualizar solo aquellos registros que realmente se han modificado por el usuario. No dude en usar las técnicas de [realizar actualizaciones por lotes](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md) para actualizar el código en este tutorial, si lo desea.

> [!NOTE]
> Al enlazar el origen de datos en el control GridView a través de su etiqueta inteligente, Visual Studio asigna automáticamente los valores de clave principal de s de origen de datos para el s GridView `DataKeyNames` propiedad. Si no se enlazó ObjectDataSource en GridView a través de la etiqueta inteligente de GridView s tal como se describe en el paso 1, entonces deberá establecer manualmente la s GridView `DataKeyNames` propiedad ProductID para tener acceso a la `ProductID` valor para cada fila a través de la `DataKeys` colección.


El código usado en `BatchUpdate` es similar al usado en las operaciones de asignación BLL `UpdateProduct` métodos, la principal diferencia es que en el `UpdateProduct` métodos una sola `ProductRow` se recupera la instancia de la arquitectura. El código que asigna las propiedades de la `ProductRow` es el mismo entre el `UpdateProducts` métodos y el código dentro de la `For Each` bucle en `BatchUpdate`, tal y como es el patrón general.

Para completar este tutorial, necesitamos tener la `BatchUpdate` método invoca cuando se hace clic en cualquiera de los botones de productos de actualización. Crear controladores de eventos para el `Click` eventos de estos dos controles de botón y agregue el código siguiente en los controladores de eventos:


[!code-vb[Main](batch-updating-vb/samples/sample6.vb)]

En primer lugar se realiza una llamada a `BatchUpdate`. A continuación, el [ `ClientScript` propiedad](https://msdn.microsoft.com/library/system.web.ui.page.clientscript(VS.80).aspx) se usa para inyectar código JavaScript que se mostrará un cuadro de mensajes que lee los productos se han actualizado.

Tómese un minuto para probar este código. Visite `BatchUpdate.aspx` a través de un explorador, editar un número de filas y haga clic en uno de los botones de productos de actualización. Suponiendo que no haya ningún error de validación de entrada, debería ver un cuadro de mensajes que lee que los productos se han actualizado. Para comprobar la atomicidad de la actualización, considere la posibilidad de agregar uno aleatorio `CHECK` restricción, al igual que uno que no admita `UnitPrice` valores de 1234,56. A continuación, en `BatchUpdate.aspx`, editar un número de registros, procurando establecer uno de los productos s `UnitPrice` valor para el valor prohibido (1234,56). Esto debería producir un error cuando haga clic en productos de actualización con los otros cambios durante esa operación por lotes revierte a sus valores originales.

## <a name="an-alternativebatchupdatemethod"></a>Una alternativa`BatchUpdate`(método)

El `BatchUpdate` método simplemente examina recupera *todas* de los productos de BLL s `GetProducts` método y, a continuación, actualiza los registros que aparecen en el control GridView. Este enfoque es ideal si el control GridView no utiliza la paginación, pero si lo hace, puede haber cientos, miles o decenas de miles de productos, pero sólo diez filas en GridView. En tal caso, la obtención de todos los productos de la base de datos solo para modificar el 10 de ellas es ideal.

Para esos tipos de situaciones, considere el uso de los siguientes `BatchUpdateAlternate` método en su lugar:


[!code-vb[Main](batch-updating-vb/samples/sample7.vb)]

`BatchMethodAlternate` se inicia mediante la creación de una nueva y vacía `ProductsDataTable` denominado `products`. A continuación, se recorre la s GridView `Rows` colección y para cada fila Obtiene la información de producto en particular utilizando la s BLL `GetProductByProductID(productID)` método. El objeto recuperado `ProductsRow` instancia tiene sus propiedades actualizados en la misma manera que `BatchUpdate`, pero después de actualizar la fila que se importan los `products` `ProductsDataTable` a través de las operaciones de asignación DataTable [ `ImportRow(DataRow)` método](https://msdn.microsoft.com/library/system.data.datatable.importrow(VS.80).aspx).

Después de la `For Each` bucle se completa, `products` contiene uno `ProductsRow` instancia para cada fila en el control GridView. Desde cada uno de los `ProductsRow` instancias se han agregado a la `products` (en lugar de actualización), si se pasa a ciegas la `UpdateWithTransaction` método la `ProductsTableAdatper` intentará insertar cada uno de los registros en la base de datos. En su lugar, necesitamos especificar que cada una de estas filas se se ha modificado (no agregado).

Esto puede realizarse mediante la adición de un nuevo método a la capa BLL denominada `UpdateProductsWithTransaction`. `UpdateProductsWithTransaction`, conjuntos se muestra a continuación, el `RowState` de cada uno de los `ProductsRow` instancias en el `ProductsDataTable` a `Modified` y, a continuación, pasa el `ProductsDataTable` al s DAL `UpdateWithTransaction` método.


[!code-vb[Main](batch-updating-vb/samples/sample8.vb)]

## <a name="summary"></a>Resumen

El control GridView proporciona capacidades de edición integrada por fila, pero no es compatible con la creación de interfaces puede modificables por completo. Como hemos visto en este tutorial, estas interfaces son posibles, pero requieren un poco de trabajo. Para crear un control GridView donde cada fila es editable, es necesario convertir los campos de s GridView TemplateFields y definir la interfaz de edición dentro de la `ItemTemplate` s. Además, actualice todos los - tipo de controles Web de botón debe agregarse a la página, independiente de GridView. Estos botones `Click` necesitan controladores de eventos enumerar las operaciones de asignación GridView `Rows` colección, almacenar los cambios en un `ProductsDataTable`y pasar la información actualizada en el método BLL adecuado.

En el siguiente tutorial veremos cómo crear una interfaz para la eliminación de un lote. En concreto, cada fila GridView incluirá una casilla de verificación y, en lugar de actualizar todos-tipo de botones, tendremos los botones eliminar las filas seleccionadas.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Los revisores para este tutorial fueron Teresa Murphy y David Suru. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](wrapping-database-modifications-within-a-transaction-vb.md)
> [Siguiente](batch-deleting-vb.md)
