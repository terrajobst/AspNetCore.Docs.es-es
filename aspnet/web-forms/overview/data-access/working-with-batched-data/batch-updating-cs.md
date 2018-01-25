---
uid: web-forms/overview/data-access/working-with-batched-data/batch-updating-cs
title: "Actualización (C#) por lotes | Documentos de Microsoft"
author: rick-anderson
description: "Obtenga información acerca de cómo actualizar varios registros de base de datos en una sola operación. En la capa de interfaz de usuario, creamos un GridView donde cada fila es editable. En los datos..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: 4e849bcc-c557-4bc3-937e-f7453ee87265
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-updating-cs
msc.type: authoredcontent
ms.openlocfilehash: 1210f9048401ca1b4e29d6dde9bf5dbef987091f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="batch-updating-c"></a>Lote de actualización (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_64_CS.zip) o [descarga de PDF](batch-updating-cs/_static/datatutorial64cs1.pdf)

> Obtenga información acerca de cómo actualizar varios registros de base de datos en una sola operación. En la capa de interfaz de usuario, creamos un GridView donde cada fila es editable. En la capa de acceso a datos se ajustan las múltiples operaciones de actualización dentro de una transacción para asegurarse de que todas las actualizaciones correctamente o se revierten todas las actualizaciones.


## <a name="introduction"></a>Introducción

En el [tutorial anterior](wrapping-database-modifications-within-a-transaction-cs.md) hemos visto cómo ampliar la capa de acceso a datos para agregar compatibilidad con transacciones de base de datos. Las transacciones de base de datos garantizan que una serie de instrucciones de modificación de datos se tratará como una operación atómica, lo cual garantiza que todas las modificaciones se producirá un error o todos se realizará correctamente. Con este nivel bajo funcionalidad DAL fuera de la vista, se re listo nos centraremos en crear interfaces de modificación de datos de lote.

En este tutorial vamos a crear un control GridView donde cada fila es editable (consulte la figura 1). Puesto que cada fila no se representa en la interfaz de edición, allí s necesidad de una columna de edición, actualizar y botones para cancelar. En su lugar, hay dos botones Actualizar productos en la página que, al hacer clic, enumerar las filas de GridView y actualizar la base de datos.


[![Cada fila de GridView es Editable](batch-updating-cs/_static/image1.gif)](batch-updating-cs/_static/image1.png)

**Figura 1**: cada fila de GridView es Editable ([haga clic aquí para ver la imagen a tamaño completo](batch-updating-cs/_static/image2.png))


Permiten s empiece a trabajar.

> [!NOTE]
> En el [realizar actualizaciones por lotes](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs.md) tutorial, creamos una edición por lotes de la interfaz con el control DataList. Este tutorial difiere de la anterior en el que es usa un control GridView y la actualización por lotes se realiza dentro del ámbito de una transacción. Después de completar este tutorial le animo a devolver para el tutorial anterior y actualizar para usar la funcionalidad relacionada con la transacción de base de datos agregada en el tutorial anterior.


## <a name="examining-the-steps-for-making-all-gridview-rows-editable"></a>Examinar los pasos para realizar todas las filas de GridView Editable

Como se describe en el [una visión general de insertar, actualizar y eliminar datos](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) tutorial, GridView ofrece compatibilidad integrada para editar sus datos subyacentes por fila. Internamente, el control GridView notas qué fila es editable a través de su [ `EditIndex` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.editindex(VS.80).aspx). Se está enlazando el control GridView a su origen de datos, comprueba cada fila para ver si el índice de la fila es igual al valor de `EditIndex`. Si es así, esa fila s campos se representan mediante la edición de sus interfaces. Para BoundFields, la interfaz de edición es un cuadro de texto cuyo `Text` propiedad se asigna el valor del campo de datos especificado por la s BoundField `DataField` propiedad. Para TemplateFields, el `EditItemTemplate` se utiliza en lugar de la `ItemTemplate`.

Recuerde que el flujo de trabajo de edición se inicia cuando un usuario hace clic en un botón de edición de fila s. Esto provoca una devolución de datos, Establece la s GridView `EditIndex` propiedad en el índice de fila donde ha hecho clic s y Reenlaces los datos a la cuadrícula. Cuando un botón de cancelación de la fila s se hace clic en, en el postback el `EditIndex` se establece en un valor de `-1` antes de volver a enlazar los datos a la cuadrícula. Puesto que las filas de GridView s inicia la indización en la posición cero, establecer `EditIndex` a `-1` tiene el efecto de presentación de GridView en modo de solo lectura.

El `EditIndex` propiedad funciona bien para cada fila de edición, pero no está diseñada para su edición por lotes. Para que pueda modificar GridView todo, es necesario que cada fila que se usara la interfaz de edición. La manera más fácil de hacerlo consiste en crear donde cada campo editable se implementa como se define un TemplateField a su interfaz de edición en el `ItemTemplate`.

En los próximos varios pasos vamos a crear un control GridView completamente editable. En el paso 1 Comenzaremos creando GridView y su ObjectDataSource y convertir sus BoundFields y CampoCasillaVerificación TemplateFields. En los pasos 2 y 3 se moverá las interfaces de edición desde el TemplateFields `EditItemTemplate` s a sus `ItemTemplate` s.

## <a name="step-1-displaying-product-information"></a>Paso 1: Mostrar información de producto

Antes de que nos preocupamos sobre cómo crear un control GridView donde son filas son editables, permiten s simplemente muestra la información del producto primero. Abra la `BatchUpdate.aspx` página en el `BatchData` carpeta y arrastre un control GridView en el cuadro de herramientas hasta el diseñador. Establecer la s GridView `ID` a `ProductsGrid` y, en su etiqueta inteligente, elija para enlazar a un nuevo ObjectDataSource denominado `ProductsDataSource`. Configurar el ObjectDataSource para recuperar sus datos de la `ProductsBLL` clase s. `GetProducts` método.


[![Configurar el ObjectDataSource para utilizar la clase ProductsBLL](batch-updating-cs/_static/image2.gif)](batch-updating-cs/_static/image3.png)

**Figura 2**: configurar el ObjectDataSource que se utilizan los `ProductsBLL` clase ([haga clic aquí para ver la imagen a tamaño completo](batch-updating-cs/_static/image4.png))


[![Recuperar los datos de producto mediante el método GetProducts](batch-updating-cs/_static/image3.gif)](batch-updating-cs/_static/image5.png)

**Figura 3**: recuperar los datos de producto mediante el `GetProducts` método ([haga clic aquí para ver la imagen a tamaño completo](batch-updating-cs/_static/image6.png))


Al igual que el control GridView, las características de modificación de ObjectDataSource s están diseñadas para trabajar por fila. Para actualizar un conjunto de registros, necesitamos escribir un poco de código en la clase de código subyacente de páginas ASP.NET que procesa por lotes los datos y la pasa a la capa BLL. Por tanto, establecer las listas desplegables en las operaciones de asignación ObjectDataSource pestañas UPDATE, INSERT y DELETE en (ninguno). Haga clic en Finalizar para completar al asistente.


[![Establece las listas de lista desplegable en la actualización, INSERCIÓN y eliminar las pestañas en (ninguno)](batch-updating-cs/_static/image4.gif)](batch-updating-cs/_static/image7.png)

**Figura 4**: establecer las listas de lista desplegable en la actualización, INSERCIÓN y eliminar pestañas en (ninguno) ([haga clic aquí para ver la imagen a tamaño completo](batch-updating-cs/_static/image8.png))


Después de completar al Asistente para configurar orígenes de datos, el marcado declarativo de ObjectDataSource s debería ser similar al siguiente:


[!code-aspx[Main](batch-updating-cs/samples/sample1.aspx)]

Finalización del Asistente para configurar origen de datos también hace que Visual Studio crear BoundFields y un CampoCasillaVerificación para los campos de datos de producto en GridView. Para este tutorial, permiten s sólo permite al usuario ver y editar el nombre del producto s, la categoría, el precio y el estado no incluida. Quitar todos menos el `ProductName`, `CategoryName`, `UnitPrice`, y `Discontinued` campos y el nombre del `HeaderText` propiedades de los tres primeros campos al producto, la categoría y el precio, respectivamente. Por último, active las casillas de verificación Habilitar paginación y habilitar la ordenación en la etiqueta inteligente de GridView s.

En este momento GridView tiene tres BoundFields (`ProductName`, `CategoryName`, y `UnitPrice`) y un CampoCasillaVerificación (`Discontinued`). Es necesario convertir estos cuatro campos en TemplateFields y, a continuación, mover la interfaz de edición de las operaciones de asignación TemplateField `EditItemTemplate` a su `ItemTemplate`.

> [!NOTE]
> Se han explorado crear y personalizar TemplateFields en el [personalizar la interfaz de modificación de datos](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) tutorial. Le guiaremos por el proceso de conversión de la BoundFields y CampoCasillaVerificación en TemplateFields y definir su edición interfaces en sus `ItemTemplate` s, pero si bloquea o necesita un actualizador, don t dudan a la hora hacen referencia a este tutorial anterior.


En la etiqueta inteligente de GridView s, haga clic en el vínculo Editar columnas para abrir el cuadro de diálogo de campos. A continuación, seleccione cada campo y haga clic en la función Convert este campo en un vínculo de TemplateField.


![Convertir el BoundFields existente y CampoCasillaVerificación en TemplateField](batch-updating-cs/_static/image5.gif)

**Figura 5**: convertir la BoundFields existente y CampoCasillaVerificación en TemplateField


Ahora que cada campo está TemplateField, nos re listo para mover la edición de la interfaz de la `EditItemTemplate` s para el `ItemTemplate` s.

## <a name="step-2-creating-theproductnameunitprice-anddiscontinuedediting-interfaces"></a>Paso 2: Crear el`ProductName`,`UnitPrice`, y`Discontinued`editar Interfaces

Crear el `ProductName`, `UnitPrice`, y `Discontinued` editar interfaces son el tema de este paso y son bastante sencillo, como cada interfaz ya está definida en las operaciones de asignación TemplateField `EditItemTemplate`. Crear el `CategoryName` la edición de interfaz es un poco más compleja debido a que necesitamos crear un DropDownList de las categorías aplicables. Esto `CategoryName` interfaz de edición se abordó en el paso 3.

Permiten s comenzar con la `ProductName` TemplateField. Haga clic en el vínculo Editar plantillas de la etiqueta inteligente de GridView s y profundizar en los `ProductName` TemplateField s `EditItemTemplate`. Seleccione el cuadro de texto, cópielo en el Portapapeles y, a continuación, péguelo en el `ProductName` TemplateField s `ItemTemplate`. Cambiar la s de cuadro de texto `ID` propiedad `ProductName`.

A continuación, agregue un control RequiredFieldValidator a la `ItemTemplate` para asegurarse de que el usuario proporciona un valor para cada nombre de producto s. Establecer el `ControlToValidate` propiedad ProductName, el `ErrorMessage` la propiedad a la debe proporcionar el nombre del producto. y el `Text` propiedad \*. Después de realizar estas adiciones a la `ItemTemplate`, la pantalla debe ser similar a la figura 6.


[![ProductName TemplateField ahora incluye un cuadro de texto y un control RequiredFieldValidator](batch-updating-cs/_static/image6.gif)](batch-updating-cs/_static/image9.png)

**Figura 6**: el `ProductName` TemplateField ahora incluye un cuadro de texto y un RequiredFieldValidator ([haga clic aquí para ver la imagen a tamaño completo](batch-updating-cs/_static/image10.png))


Para el `UnitPrice` edición interfaz, iniciar copiando el cuadro de texto de la `EditItemTemplate` a la `ItemTemplate`. A continuación, coloque un signo $ delante del cuadro de texto y el conjunto de sus `ID` propiedad UnitPrice y su `Columns` propiedad a 8.

Agregar un control CompareValidator a la `UnitPrice` s `ItemTemplate` para asegurarse de que el valor especificado por el usuario es un valor de moneda válidos mayor o igual a 0,00 USD. Establece el validador s `ControlToValidate` propiedad UnitPrice, su `ErrorMessage` propiedad debe especificar un valor de moneda válidos. Por favor, omita cualquier moneda símbolos., su `Text` propiedad a \*, sus `Type` propiedad a `Currency`, sus `Operator` propiedad a `GreaterThanEqual`y su `ValueToCompare` propiedad en 0.


[![Agregar un control CompareValidator para asegurarse de que el precio especificado es un valor de moneda no negativo](batch-updating-cs/_static/image7.gif)](batch-updating-cs/_static/image11.png)

**Figura 7**: agregar un control CompareValidator para asegurarse de que el precio especificado es un valor de moneda no negativo ([haga clic aquí para ver la imagen a tamaño completo](batch-updating-cs/_static/image12.png))


Para el `Discontinued` TemplateField puede utilizar la casilla de verificación ya definido en el `ItemTemplate`. Basta con establecer su `ID` a discontinuo y su `Enabled` propiedad `true`.

## <a name="step-3-creating-thecategorynameediting-interface"></a>Paso 3: Crear el`CategoryName`edición (interfaz)

La interfaz de edición en el `CategoryName` TemplateField s `EditItemTemplate` contiene un cuadro de texto que muestra el valor de la `CategoryName` campo de datos. Es necesario reemplazar esto con un DropDownList que enumera las categorías posibles.

> [!NOTE]
> El [personalizar la interfaz de modificación de datos](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) tutorial contiene una explicación más exhaustiva y completa acerca de cómo personalizar una plantilla para incluir un DropDownList en lugar de un cuadro de texto. Aunque los pasos aquí son completados, se presentan lacónicamente. Para obtener información más detallada en la creación y configuración de las categorías de DropDownList, hacen referencia a la [personalizar la interfaz de modificación de datos](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) tutorial.


Arrastre un DropDownList desde el cuadro de herramientas en el `CategoryName` TemplateField s `ItemTemplate`, y establece su `ID` a `Categories`. En este momento se normalmente definiría el origen de datos de listas desplegables s mediante su etiqueta inteligente, crear un nuevo origen ObjectDataSource. Sin embargo, se agregará el ObjectDataSource dentro de la `ItemTemplate`, lo que producirá una instancia de ObjectDataSource creada para cada fila de GridView. En su lugar, permita s crear ObjectDataSource fuera de las operaciones de asignación GridView TemplateFields. Finalizar la edición de plantillas y arrastre un ObjectDataSource desde el cuadro de herramientas hasta el diseñador debajo de la `ProductsDataSource` ObjectDataSource. Nombre de la nueva ObjectDataSource `CategoriesDataSource` y configúrelo para utilizar el `CategoriesBLL` clase s. `GetCategories` (método).


[![Configurar el ObjectDataSource para usar el CategoriesBLL Clas](batch-updating-cs/_static/image8.gif)](batch-updating-cs/_static/image13.png)

**Figura 8**: configurar el ObjectDataSource que se utilizan los `CategoriesBLL` Clas ([haga clic aquí para ver la imagen a tamaño completo](batch-updating-cs/_static/image14.png))


[![Recuperar los datos de categoría mediante el método GetCategories](batch-updating-cs/_static/image9.gif)](batch-updating-cs/_static/image15.png)

**Figura 9**: recuperar los datos de categoría mediante el `GetCategories` método ([haga clic aquí para ver la imagen a tamaño completo](batch-updating-cs/_static/image16.png))


Puesto que este ObjectDataSource se utiliza simplemente para recuperar los datos, establezca las listas desplegables en las pestañas UPDATE y DELETE para (ninguno). Haga clic en Finalizar para completar al asistente.


[![Conjunto de las listas desplegables de la actualización y eliminación fichas (ninguno)](batch-updating-cs/_static/image10.gif)](batch-updating-cs/_static/image17.png)

**Figura 10**: establecer las listas de lista desplegable en la actualización y eliminar pestañas en (ninguno) ([haga clic aquí para ver la imagen a tamaño completo](batch-updating-cs/_static/image18.png))


Después de completar el asistente, la `CategoriesDataSource` marcado declarativo s debería ser similar al siguiente:


[!code-aspx[Main](batch-updating-cs/samples/sample2.aspx)]

Con el `CategoriesDataSource` creado y configurado, volver a la `CategoryName` TemplateField s `ItemTemplate` y, en la etiqueta inteligente de DropDownList s, haga clic en el vínculo Elegir origen de datos. En el Asistente de configuración del origen de datos, seleccione la `CategoriesDataSource` opción en la primera lista desplegable y elija tener `CategoryName` utilizan para mostrar y `CategoryID` como el valor.


[![Enlazar el CategoriesDataSource DropDownList](batch-updating-cs/_static/image11.gif)](batch-updating-cs/_static/image19.png)

**Figura 11**: enlazar DropDownList para la `CategoriesDataSource` ([haga clic aquí para ver la imagen a tamaño completo](batch-updating-cs/_static/image20.png))


En este momento la `Categories` DropDownList enumera todas las categorías, pero no aún automáticamente seleccione la categoría apropiada para el producto enlazado a la fila de GridView. Para ello es necesario establecer la `Categories` DropDownList s `SelectedValue` al producto s `CategoryID` valor. Haga clic en el vínculo Editar DataBindings de la etiqueta inteligente de DropDownList s y asociar el `SelectedValue` propiedad con el `CategoryID` del campo de datos tal como se muestra en la figura 12.


![Enlazar el valor de Id. de categoría de producto s a la propiedad de SelectedValue s DropDownList](batch-updating-cs/_static/image12.gif)

**Figura 12**: enlazar el producto s `CategoryID` valor para las operaciones de asignación de DropDownList `SelectedValue` propiedad


Una última sigue siendo de problema: si el producto tiene un `CategoryID` especificó un valor, a continuación, la instrucción de enlace de datos en `SelectedValue` se producirá una excepción. Esto es porque la DropDownList contiene solo los elementos de las categorías y no ofrece una opción para aquellos productos que tienen un `NULL` valor para la base de datos `CategoryID`. Para solucionarlo, establezca la s DropDownList `AppendDataBoundItems` propiedad `true` y agregar un nuevo elemento a los controles de DropDownList, omitiendo el `Value` propiedad de la sintaxis declarativa. Es decir, asegúrese de que el `Categories` DropDownList la sintaxis declarativa de s es similar al siguiente:


[!code-aspx[Main](batch-updating-cs/samples/sample3.aspx)]

Tenga en cuenta cómo el `<asp:ListItem Value="">` : seleccione uno--tiene su `Value` atributo se establece explícitamente en una cadena vacía. Hacen referencia a la [personalizar la interfaz de modificación de datos](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) tutorial para obtener una explicación más exhaustiva sobre por qué se necesita este elemento DropDownList adicional para controlar la `NULL` caso y por qué la asignación de la `Value` propiedad en una cadena vacía es esencial.

> [!NOTE]
> Hay un rendimiento y escalabilidad problema potencial aquí que vale la pena mencionar. Puesto que cada fila tiene un DropDownList que usa el `CategoriesDataSource` como origen de datos, el `CategoriesBLL` clase s `GetCategories` se llama al método  *n*  visitan veces por página, donde  *n*  es el número de filas en GridView. Estos  *n*  llama a `GetCategories` dar lugar a  *n*  consultas a la base de datos. Este impacto en la base de datos podría reducirse al almacenar en caché las categorías devueltas en una caché de cada solicitud o a través de la capa de almacenamiento en caché con una dependencia o una expiración muy basada en poco tiempo el almacenamiento en caché de SQL. Para obtener más información sobre la solicitud por opción, el almacenamiento en caché vea [ `HttpContext.Items` un almacén de caché por solicitud](http://aspnet.4guysfromrolla.com/articles/060904-1.aspx).


## <a name="step-4-completing-the-editing-interface"></a>Paso 4: Completar la interfaz de edición

Se ha realizado una serie de cambios en las plantillas de s GridView sin poner en pausa para ver el progreso. Tómese un momento para ver el progreso a través de un explorador. Como se muestra en la figura 13, cada fila se representa mediante su `ItemTemplate`, que contiene la celda edición de interfaz.


[![Cada fila de GridView es Editable](batch-updating-cs/_static/image13.gif)](batch-updating-cs/_static/image21.png)

**Figura 13**: cada fila de GridView es Editable ([haga clic aquí para ver la imagen a tamaño completo](batch-updating-cs/_static/image22.png))


Hay algunos problemas de formato poco importantes que se deben tener en cuenta de en este momento. En primer lugar, tenga en cuenta que la `UnitPrice` valor contiene cuatro espacios decimales. Para solucionar este problema, vuelva a la `UnitPrice` TemplateField s `ItemTemplate` y, en la etiqueta inteligente s de cuadro de texto, haga clic en el vínculo Editar DataBindings. A continuación, especificar que la `Text` propiedad debe tener el formato como un número.


![Dar formato a la propiedad de texto como un número](batch-updating-cs/_static/image14.gif)

**Figura 14**: formato el `Text` propiedad como un número


En segundo lugar, permita que s del centro de la casilla de verificación en la `Discontinued` columna (en lugar de tener que alinea a la izquierda). Haga clic en Editar columnas de la etiqueta inteligente de GridView s y seleccione la `Discontinued` TemplateField en la lista de campos en la esquina inferior izquierda. Explorar en profundidad `ItemStyle` y establezca el `HorizontalAlign` propiedad al centro tal como se muestra en la figura 15.


![Centro de la casilla de verificación no incluida](batch-updating-cs/_static/image15.gif)

**Figura 15**: Center el `Discontinued` casilla de verificación


A continuación, agregue un control a la página y establezca su `ShowMessageBox` propiedad `true` y su `ShowSummary` propiedad `false`. Agregar que controles de la Web de botón que, al hacer clic en, se actualizarán los cambios de s de usuario. En concreto, agregue dos controles de botón Web, uno encima de GridView y otro después de ella, establecer ambos controles `Text` propiedades para productos de actualización.

Desde el s GridView edición interfaz se define en su TemplateFields `ItemTemplate` s, el `EditItemTemplate` se superfluo y pueden eliminarse.

Después de realizar los pasos anteriores se ha mencionado cambios de formato, los controles de botón de agregar y quitar el innecesarios `EditItemTemplate` s, la sintaxis declarativa de página s debería ser similar al siguiente:


[!code-aspx[Main](batch-updating-cs/samples/sample4.aspx)]

La figura 16 muestra esta página cuando se ve mediante un explorador después de han agregado los controles de botón Web y los cambios de formato realizados.


[![Página ahora incluye dos botones de productos de actualización](batch-updating-cs/_static/image16.gif)](batch-updating-cs/_static/image23.png)

**Figura 16**: la página ahora incluye dos actualización productos botones ([haga clic aquí para ver la imagen a tamaño completo](batch-updating-cs/_static/image24.png))


## <a name="step-5-updating-the-products"></a>Paso 5: Actualización de los productos

Cuando un usuario visita esta página que se realice sus modificaciones y, a continuación, haga clic en uno de los dos botones de productos de actualización. En ese momento se necesita algún modo guardar los valores introducidos por el usuario para cada fila en un `ProductsDataTable` de instancia y, a continuación, páselo a un método BLL que, a continuación, pasará que `ProductsDataTable` instancia a la capa DAL s `UpdateWithTransaction` método. El `UpdateWithTransaction` método, que hemos creado en el [tutorial anterior](wrapping-database-modifications-within-a-transaction-cs.md), garantiza que el lote de cambios se actualizarán como una operación atómica.

Crear un método denominado `BatchUpdate` en `BatchUpdate.aspx.cs` y agregue el código siguiente:


[!code-csharp[Main](batch-updating-cs/samples/sample5.cs)]

Este método comienza a obteniendo todos los productos en un `ProductsDataTable` mediante una llamada a las operaciones de asignación BLL `GetProducts` método. A continuación, enumera el `ProductGrid` GridView s [ `Rows` colección](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows(VS.80).aspx). El `Rows` colección contiene un [ `GridViewRow` instancia](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewrow.aspx) para cada fila que se muestra en el control GridView. Puesto que vamos a presentar como máximo diez filas por página, las operaciones de asignación GridView `Rows` colección contará con no más de diez elementos.

Para cada fila el `ProductID` es obtener un elemento de la `DataKeys` recopilación y la correspondiente `ProductsRow` está seleccionado en el `ProductsDataTable`. Los cuatro controles de entrada de TemplateField mediante programación se hace referencia y sus valores se asignan a las `ProductsRow` s propiedades de la instancia. Después de cada GridView se usaron los valores de fila s para actualizar la `ProductsDataTable`, s pasado a las operaciones de asignación BLL `UpdateWithTransaction` método que, como hemos visto en el tutorial anterior, simplemente llama a hacia abajo en la capa DAL s `UpdateWithTransaction` método.

El algoritmo de actualización de lote utilizado para este tutorial actualiza cada fila de la `ProductsDataTable` que corresponde a una fila en el control GridView, independientemente de si ha cambiado la información de producto s. Mientras estos blind actualiza t normalmente un problema de rendimiento, puede generar registros superfluos si se van a auditar cambios en la tabla de base de datos. En el [realizar actualizaciones por lotes](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs.md) tutorial se explora una actualización por lotes interfaz con el control DataList y agrega código que solo se actualiza los registros que realmente se han modificado por el usuario. No dude en usar las técnicas de [realizar actualizaciones por lotes](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs.md) para actualizar el código en este tutorial, si lo desea.

> [!NOTE]
> Cuando se enlaza el origen de datos en GridView mediante su etiqueta inteligente, Visual Studio asigna automáticamente los valores de clave principal de la s de origen de datos a las operaciones de asignación GridView `DataKeyNames` propiedad. Si no se enlazó ObjectDataSource en GridView a través de la etiqueta inteligente de GridView s tal como se describe en el paso 1, entonces debe establecer manualmente la s GridView `DataKeyNames` propiedad ProductID para tener acceso a la `ProductID` valor para cada fila a través de la `DataKeys` colección.


El código usado en `BatchUpdate` es similar al utilizado en las operaciones de asignación BLL `UpdateProduct` métodos, la principal diferencia es que en el `UpdateProduct` métodos solo una `ProductRow` se recupera la instancia de la arquitectura. El código que asigna las propiedades de la `ProductRow` es el mismo entre la `UpdateProducts` métodos y el código dentro de la `foreach` un bucle en `BatchUpdate`, tal y como se muestra el patrón general.

Para completar este tutorial, es necesario tener la `BatchUpdate` método invoca cuando se hace clic en cualquiera de los botones de productos de actualización. Crear controladores de eventos para el `Click` eventos de estos dos controles de botón y agregue el código siguiente en los controladores de eventos:


[!code-csharp[Main](batch-updating-cs/samples/sample6.cs)]

Primero se realiza una llamada a `BatchUpdate`. Después, el `ClientScript property` se utiliza para insertar código JavaScript que se mostrará un cuadro de mensajes que lee los productos se han actualizado.

Tómese un minuto para probar este código. Visite `BatchUpdate.aspx` a través de un explorador, editar un número de filas y haga clic en uno de los botones de productos de actualización. Suponiendo que no hay ningún error de validación de entrada, debería ver un cuadro de mensajes que lee que los productos se han actualizado. Para comprobar la atomicidad de la actualización, considere la posibilidad de agregar un aleatorio `CHECK` restricción, al igual que uno que no permite `UnitPrice` valores de 1234,56. A continuación, desde `BatchUpdate.aspx`, editar un número de registros, asegurarse de establecer uno de los productos s `UnitPrice` valor con el valor prohibido (1234,56). Esto debería producir un error cuando haga clic en los productos de actualización con los demás cambios durante esa operación por lotes revierte a sus valores originales.

## <a name="an-alternativebatchupdatemethod"></a>Una alternativa`BatchUpdate`(método)

El `BatchUpdate` método simplemente examina recupera *todos los* de los productos de las operaciones de asignación BLL `GetProducts` método y, a continuación, actualiza únicamente los registros que aparecen en el control GridView. Este enfoque es ideal si GridView no use la paginación, pero si lo hace, puede haber cientos, miles o decenas de miles de productos, pero solo diez filas en GridView. En tal caso, obtener todos los productos de la base de datos solo para modificar el 10 de ellos es menor que ideal.

Para los tipos de situaciones, considere el uso de los siguientes `BatchUpdateAlternate` método en su lugar:


[!code-csharp[Main](batch-updating-cs/samples/sample7.cs)]

`BatchMethodAlternate`se inicia mediante la creación de una nueva y vacía `ProductsDataTable` denominado `products`. A continuación, se recorre la s GridView `Rows` colección y para cada fila Obtiene la información de producto en particular con las operaciones de asignación BLL `GetProductByProductID(productID)` método. El objeto recuperado `ProductsRow` instancia tiene sus propiedades actualizados en la misma forma que `BatchUpdate`, pero después de actualizar la fila se importa en el `products``ProductsDataTable` a través de las operaciones de asignación DataTable [ `ImportRow(DataRow)` método](https://msdn.microsoft.com/library/system.data.datatable.importrow(VS.80).aspx).

Después de la `foreach` bucle se completa, `products` contiene un `ProductsRow` instancia para cada fila de GridView. Desde cada uno de la `ProductsRow` instancias se han agregado a la `products` (en lugar de actualizar), si pasamos a ciegas que el `UpdateWithTransaction` método el `ProductsTableAdatper` intentará insertar cada uno de los registros en la base de datos. En su lugar, es preciso especificar que cada una de estas filas se han modificado (no agregado).

Esto puede realizarse mediante la adición de un nuevo método a la capa BLL denominada `UpdateProductsWithTransaction`. `UpdateProductsWithTransaction`, conjuntos se muestra a continuación, el `RowState` de cada uno de la `ProductsRow` instancias en el `ProductsDataTable` a `Modified` y, a continuación, pasa el `ProductsDataTable` a la capa DAL s `UpdateWithTransaction` método.


[!code-csharp[Main](batch-updating-cs/samples/sample8.cs)]

## <a name="summary"></a>Resumen

GridView proporciona capacidades de edición integrada por fila, pero no es compatible con la creación de interfaces puede modificables por completo. Como hemos visto en este tutorial, dichas interfaces son posibles, pero requieren una cantidad de trabajo. Para crear un control GridView donde cada fila es editable, es necesario convertir los campos de s GridView TemplateFields y definir la interfaz de edición dentro de la `ItemTemplate` s. Además, actualice todos los - tipo de controles de botón Web debe agregarse a la página, independiente de GridView. Estos botones `Click` necesario que los controladores de eventos enumerar las operaciones de asignación GridView `Rows` colección, almacenar los cambios en un `ProductsDataTable`y pasar la información actualizada en el método BLL adecuado.

En el siguiente tutorial veremos cómo crear una interfaz para la eliminación de un lote. En concreto, cada fila de GridView incluirá una casilla de verificación y en lugar de actualizar todo-escriba botones, tendremos botones eliminar filas seleccionadas.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Los revisores iniciales para este tutorial fueron Teresa Murphy y David Suru. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](wrapping-database-modifications-within-a-transaction-cs.md)
[Siguiente](batch-deleting-cs.md)
