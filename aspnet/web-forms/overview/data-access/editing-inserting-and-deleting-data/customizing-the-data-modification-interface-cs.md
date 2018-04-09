---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs
title: Personalizar la interfaz de modificación de datos (C#) | Documentos de Microsoft
author: rick-anderson
description: En este tutorial se examinará cómo personalizar la interfaz de un control GridView editable, sustituyendo el cuadro de texto estándar y controles CheckBox con alterna...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 22e99600-8d18-4a94-a20e-a3a62bb63798
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: f25b265c50870d59721a94c01d78f589a5d5f1c3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="customizing-the-data-modification-interface-c"></a>Personalizar la interfaz de modificación de datos (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_20_CS.exe) o [descarga de PDF](customizing-the-data-modification-interface-cs/_static/datatutorial20cs1.pdf)

> En este tutorial se examinará cómo personalizar la interfaz de un control GridView editable, sustituyendo el cuadro de texto estándar y controles CheckBox con controles Web de entrada alternativos.


## <a name="introduction"></a>Introducción

El BoundFields y CheckBoxFields utilizado por los controles GridView y DetailsView simplifican el proceso de modificación de los datos debido a su capacidad para representar interfaces de solo lectura, puede editables e insertables. Estas interfaces se pueden representar sin la necesidad de agregar cualquier código o marcado declarativo adicional. Sin embargo, las BoundField del CampoCasillaVerificación interfaces y no tienen la capacidad de personalización a menudo se necesita en situaciones del mundo real. Con el fin de personalizar la interfaz modificable o pueda insertar en una GridView o DetailsView que debemos usar en su lugar TemplateField.

En el [tutorial anterior](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) hemos visto cómo personalizar las interfaces de modificación de datos mediante la adición de controles Web de validación. En este tutorial se examinará cómo personalizar los controles de Web de recopilación de datos reales, reemplazando el BoundField y cuadro de texto estándar del CampoCasillaVerificación y controles de casilla con controles Web de entrada alternativos. En concreto, vamos a crear un control GridView editable que permite a un producto nombre, categoría, proveedor y estado no incluida en actualizarse. Cuando se edita una fila determinada, los campos de categoría y el proveedor se representarán como listas desplegables, que contiene el conjunto de categorías disponibles y proveedores que puede elegir. Además, vamos a reemplazar predeterminado del CampoCasillaVerificación casilla con un control RadioButtonList que ofrece dos opciones: "Activo" y "Suspendido".


[![Interfaz de edición de GridView incluye listas desplegables y botones de opción](customizing-the-data-modification-interface-cs/_static/image2.png)](customizing-the-data-modification-interface-cs/_static/image1.png)

**Figura 1**: edición de listas desplegables de incluye de interfaz y los botones de opción The GridView ([haga clic aquí para ver la imagen a tamaño completo](customizing-the-data-modification-interface-cs/_static/image3.png))


## <a name="step-1-creating-the-appropriateupdateproductoverload"></a>Paso 1: Crear la correspondiente`UpdateProduct`sobrecarga

En este tutorial crearemos un control GridView editable que permite la edición de nombre de un producto, categoría, proveedor y estado de suspendido. Por lo tanto, tenemos un `UpdateProduct` sobrecarga que acepta cinco parámetros de entrada estos valores de cuatro producto además de la interfaz `ProductID`. Al igual que en nuestro sobrecargas anteriores, con ello uno se:

1. Recuperar la información de producto de la base de datos para el elemento especificado `ProductID`,
2. Actualización de la `ProductName`, `CategoryID`, `SupplierID`, y `Discontinued` campos, y
3. Enviar la solicitud de actualización a la capa DAL a través del TableAdapter `Update()` método.

Para mayor brevedad, esta sobrecarga determinado he omitido la verificación de reglas de negocios que garantiza un producto que se va a marcar como discontinuo no es el único producto proporcionado por su proveedor. Llamarme agregarlo si lo prefiere, o bien, idealmente, refactorizar la lógica a un método independiente.

El código siguiente muestra la nueva `UpdateProduct` sobrecarga en el `ProductsBLL` clase:


[!code-csharp[Main](customizing-the-data-modification-interface-cs/samples/sample1.cs)]

## <a name="step-2-crafting-the-editable-gridview"></a>Paso 2: Diseñar el control GridView Editable

Con el `UpdateProduct` agrega sobrecarga, estamos listos crear nuestra GridView editable. Abra la `CustomizedUI.aspx` página en el `EditInsertDelete` carpeta y agregar un control GridView al diseñador. A continuación, cree un nuevo origen ObjectDataSource de etiqueta inteligente de GridView. Configurar el ObjectDataSource para recuperar información del producto a través de la `ProductBLL` la clase `GetProducts()` método y actualizar los datos de producto mediante el `UpdateProduct` sobrecarga que acabamos de crear. En las pestañas INSERT y DELETE, seleccione (ninguno) en las listas de lista desplegable.


[![Configurar el ObjectDataSource para utilizar la sobrecarga de UpdateProduct recién creada](customizing-the-data-modification-interface-cs/_static/image5.png)](customizing-the-data-modification-interface-cs/_static/image4.png)

**Figura 2**: configurar el ObjectDataSource que se utilizan los `UpdateProduct` sobrecarga acaba de crear ([haga clic aquí para ver la imagen a tamaño completo](customizing-the-data-modification-interface-cs/_static/image6.png))


Como hemos visto a lo largo de los tutoriales de modificación de datos, la sintaxis declarativa para ObjectDataSource creada por Visual Studio asigna el `OldValuesParameterFormatString` propiedad `original_{0}`. Esto, por supuesto, no funcionará con la capa de lógica de negocios desde nuestros métodos no espera que el original `ProductID` valor deben pasarse en. Por lo tanto, como hemos hecho en tutoriales anteriores, tómese un momento para quitar esta asignación de propiedad de la sintaxis declarativa o, en su lugar, establezca el valor de esta propiedad `{0}`.

Después de este cambio, el marcado declarativo de ObjectDataSource debe ser similar al siguiente:


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample2.aspx)]

Tenga en cuenta que la `OldValuesParameterFormatString` propiedad se ha quitado y que hay un `Parameter` en el `UpdateParameters` recopilación para cada uno de los parámetros de entrada esperados por nuestro `UpdateProduct` de sobrecarga.

Mientras el ObjectDataSource está configurado para actualizar solo un subconjunto de valores de producto, el control GridView muestra actualmente *todos los* de los campos de producto. Tómese un momento para editar GridView para que:

- Solo se incluyan los `ProductName`, `SupplierName`, `CategoryName` BoundFields y `Discontinued` CampoCasillaVerificación
- El `CategoryName` y `SupplierName` campos que aparecerán antes (a la izquierda de) la `Discontinued` CampoCasillaVerificación
- El `CategoryName` y `SupplierName` 'BoundFields `HeaderText` propiedad está establecida en "Categoría" y "Proveedor", respectivamente
- Está habilitada la funcionalidad de edición (Active la casilla Habilitar edición de etiquetas inteligentes de GridView)

Después de estos cambios, el diseñador tendrá un aspecto similar a la figura 3, con la sintaxis declarativa de GridView que se muestra a continuación.


[![Quite los campos innecesarios de GridView](customizing-the-data-modification-interface-cs/_static/image8.png)](customizing-the-data-modification-interface-cs/_static/image7.png)

**Figura 3**: quitar los campos innecesarios del GridView ([haga clic aquí para ver la imagen a tamaño completo](customizing-the-data-modification-interface-cs/_static/image9.png))


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample3.aspx)]

En este momento el comportamiento de solo lectura de GridView es completando. Cuando se visualizan los datos, cada producto se representa como una fila en el control GridView, que muestra el nombre del producto, categoría, proveedor y no incluye el estado.


[![Interfaz de solo lectura de GridView es completada](customizing-the-data-modification-interface-cs/_static/image11.png)](customizing-the-data-modification-interface-cs/_static/image10.png)

**Figura 4**: interfaz de solo lectura de The GridView es completar ([haga clic aquí para ver la imagen a tamaño completo](customizing-the-data-modification-interface-cs/_static/image12.png))


> [!NOTE]
> Como se describe en [una visión general de insertar, actualizar y eliminar datos tutorial](an-overview-of-inserting-updating-and-deleting-data-cs.md), es sumamente importante que GridView posible estado de vista de s habilitado (comportamiento predeterminado). Si establece la s GridView `EnableViewState` propiedad `false`, se corre el riesgo de tener usuarios simultáneos involuntariamente eliminen o modifiquen los registros. Vea [advertencia: problema de simultaneidad con ASP.NET 2.0 GridView/DetailsView/FormViews que admiten la edición o eliminación y cuyo estado de vista está deshabilitado](http://scottonwriting.net/sowblog/posts/10054.aspx) para obtener más información.


## <a name="step-3-using-a-dropdownlist-for-the-category-and-supplier-editing-interfaces"></a>Paso 3: Usando DropDownList para la categoría y la edición de Interfaces de proveedor

Recuerde que el `ProductsRow` objeto contiene `CategoryID`, `CategoryName`, `SupplierID`, y `SupplierName` propiedades, que proporcionan los valores reales de Id. de clave externa en la `Products` tabla y las correspondientes bases de datos `Name` los valores en el `Categories` y `Suppliers` tablas. El `ProductRow`del `CategoryID` y `SupplierID` ambos pueden leer y escritos a, mientras que la `CategoryName` y `SupplierName` propiedad se marca como de solo lectura.

Debido al estado de solo lectura de la `CategoryName` y `SupplierName` propiedades, que han tenido la correspondientes BoundFields sus `ReadOnly` propiedad establecida en `true`, de modo que estos valores que se va a modificar cuando se edita una fila. Mientras que podemos establecer la `ReadOnly` propiedad `false`, representación el `CategoryName` y `SupplierName` BoundFields como cuadros de texto durante la edición, este enfoque se producirá una excepción cuando el usuario intenta actualizar el producto ya que no hay ninguna `UpdateProduct` sobrecarga que toma en `CategoryName` y `SupplierName` entradas. De hecho, no queremos crear este tipo de sobrecarga por dos motivos:

- El `Products` tabla no tiene `SupplierName` o `CategoryName` campos, pero `SupplierID` y `CategoryID`. Por lo tanto, es deseable nuestro método va a pasar estos valores de Id. concretos, no los valores de sus tablas de consulta.
- Requerir al usuario que escriba el nombre del proveedor o la categoría es menor ideal, ya que requiere que el usuario sepa las categorías disponibles y proveedores y su ortografía correcta.

Los campos de categoría y el proveedor deberían mostrar la categoría y nombres de proveedores en el modo de solo lectura (tal y como lo hace ahora) y una lista desplegable de opciones aplicables al que se está editando. Usar una lista desplegable, el usuario final puede ver rápidamente qué categorías y proveedores están disponibles para elegir entre y más fácilmente realizar su selección.

Para proporcionar este comportamiento, necesitamos convertir el `SupplierName` y `CategoryName` BoundFields en TemplateFields cuyo `ItemTemplate` emite el `SupplierName` y `CategoryName` valores y cuyo `EditItemTemplate` utiliza un control DropDownList a lista el categorías disponibles y proveedores.

## <a name="adding-thecategoriesandsuppliersdropdownlists"></a>Agregar el`Categories`y`Suppliers`listas desplegables

Iniciar convirtiendo el `SupplierName` y `CategoryName` BoundFields en TemplateFields por: al hacer clic en el vínculo Editar columnas de etiqueta inteligente de GridView; seleccione el BoundField en la lista en la parte inferior izquierda; y haga clic en el "convertir este campo en un Vínculo de TemplateField". El proceso de conversión creará un TemplateField con ambos un `ItemTemplate` y `EditItemTemplate`, tal y como se muestra en la sintaxis declarativa siguiente:


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample4.aspx)]

Puesto que la BoundField se marcó como de solo lectura, tanto la `ItemTemplate` y `EditItemTemplate` contienen una etiqueta de Web control cuya `Text` propiedad se enlaza al campo de datos aplicable (`CategoryName`, en la sintaxis anterior). Es necesario modificar el `EditItemTemplate`, reemplazando el control Label Web con un control DropDownList.

Como hemos visto en tutoriales anteriores, la plantilla se puede editar a través del diseñador o directamente desde la sintaxis declarativa. Para editar a través del diseñador, haga clic en el vínculo Editar plantillas de etiqueta inteligente de GridView y prefieren trabajar con el campo de categoría `EditItemTemplate`. Quitar el control Web Label y reemplazarlo con un control DropDownList, establecer la propiedad de Id. de DropDownList en `Categories`.


[![Quite el TextBox puede y agregue un DropDownList a EditItemTemplate](customizing-the-data-modification-interface-cs/_static/image14.png)](customizing-the-data-modification-interface-cs/_static/image13.png)

**Figura 5**: quitar el TextBox puede y agregar un DropDownList para la `EditItemTemplate` ([haga clic aquí para ver la imagen a tamaño completo](customizing-the-data-modification-interface-cs/_static/image15.png))


A continuación, necesitamos rellenar la DropDownList con las categorías disponibles. Haga clic en el vínculo Elegir origen de datos de etiqueta inteligente de DropDownList y optar por crear un nuevo origen ObjectDataSource denominado `CategoriesDataSource`.


[![Crear un nuevo Control ObjectDataSource denominado CategoriesDataSource](customizing-the-data-modification-interface-cs/_static/image17.png)](customizing-the-data-modification-interface-cs/_static/image16.png)

**Figura 6**: crear un nuevo Control ObjectDataSource denominado `CategoriesDataSource` ([haga clic aquí para ver la imagen a tamaño completo](customizing-the-data-modification-interface-cs/_static/image18.png))


Para que este ObjectDataSource devolver todas las categorías, enlazarlo a la `CategoriesBLL` la clase `GetCategories()` método.


[![Enlazar el ObjectDataSource al método de GetCategories() del CategoriesBLL](customizing-the-data-modification-interface-cs/_static/image20.png)](customizing-the-data-modification-interface-cs/_static/image19.png)

**Figura 7**: enlazar el ObjectDataSource a la `CategoriesBLL`del `GetCategories()` método ([haga clic aquí para ver la imagen a tamaño completo](customizing-the-data-modification-interface-cs/_static/image21.png))


Por último, configurar opciones de DropDownList tal que la `CategoryName` campo se muestra en cada DropDownList `ListItem` con el `CategoryID` campo que se usa como el valor.


[![Tiene el campo CategoryName muestra y CategoryID usar como valor](customizing-the-data-modification-interface-cs/_static/image23.png)](customizing-the-data-modification-interface-cs/_static/image22.png)

**Figura 8**: tiene la `CategoryName` muestra campo y la `CategoryID` utiliza como el valor ([haga clic aquí para ver la imagen a tamaño completo](customizing-the-data-modification-interface-cs/_static/image24.png))


Después de realizar estos cambios en el marcado declarativo para la `EditItemTemplate` en el `CategoryName` TemplateField incluirá un DropDownList y un ObjectDataSource:


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample5.aspx)]

> [!NOTE]
> DropDownList en el `EditItemTemplate` debe tener habilitado el estado de su vista. Pronto agregaremos sintaxis de enlace de datos a la sintaxis declarativa de DropDownList y comandos de enlace de datos como `Eval()` y `Bind()` sólo puede aparecer en los controles cuyo estado de vista está habilitado.


Repita estos pasos para agregar un DropDownList denominado `Suppliers` a la `SupplierName` del TemplateField `EditItemTemplate`. Esto implica agregar DropDownList para el `EditItemTemplate` y crear otro ObjectDataSource. El `Suppliers` ObjectDataSource de DropDownList, sin embargo, se debe configurar para invocar la `SuppliersBLL` la clase `GetSuppliers()` método. Además, configurar la `Suppliers` DropDownList para mostrar el `CompanyName` campo y use la `SupplierID` campo como el valor de su `ListItem` s.

Después de agregar las listas desplegables para los dos `EditItemTemplate` , cargar la página en un explorador y haga clic en el botón Editar para el producto de Cajun Seasoning del Chef Anton. Como se muestra en la figura 9, las columnas de categoría y el proveedor del producto se representan como listas desplegables que contiene las categorías disponibles y los proveedores que puede elegir. Sin embargo, tenga en cuenta que la *primer* elementos en las listas desplegables están seleccionados de forma predeterminada (bebidas para la categoría) y Exotic Liquids como el proveedor, aunque Chef Anton Cajun Seasoning sea un sazonadoras proporcionado por Nueva Orleans Cajun Delights.


[![El primer elemento en el cuadro desplegable enumera está activado de forma predeterminada](customizing-the-data-modification-interface-cs/_static/image26.png)](customizing-the-data-modification-interface-cs/_static/image25.png)

**Figura 9**: el primer elemento en el cuadro desplegable enumera está activado de forma predeterminada ([haga clic aquí para ver la imagen a tamaño completo](customizing-the-data-modification-interface-cs/_static/image27.png))


Además, si hace clic en actualizar, encontrará que el producto `CategoryID` y `SupplierID` valores se establecen en `NULL`. Ambos no deseadas de comportamientos se producen porque las listas desplegables en el `EditItemTemplate` s no están enlazados a los campos de datos de los datos subyacentes del producto.

## <a name="binding-the-dropdownlists-to-thecategoryidandsupplieriddata-fields"></a>Enlace las listas desplegables para los`CategoryID`y`SupplierID`campos de datos

Para disponer de categoría y el proveedor del producto editado establecen listas desplegables para los valores adecuados y que estos valores que se envían hacia el BLL `UpdateProduct` método al hacer clic en actualizar, es necesario enlazar las listas desplegables `SelectedValue` propiedades para el `CategoryID` y `SupplierID` campos de datos utilizando enlace de datos bidireccional. Para lograr esto con la `Categories` DropDownList, puede agregar `SelectedValue='<%# Bind("CategoryID") %>'` directamente a la sintaxis declarativa.

Como alternativa, puede establecer databindings de DropDownList modificando la plantilla a través del diseñador y haga clic en el vínculo Editar DataBindings de etiqueta inteligente de DropDownList. A continuación, indicar que la `SelectedValue` propiedad debe estar enlazada a la `CategoryID` campo utilizando el enlace de datos bidireccional (consulte la figura 10). Repita el proceso declarativo o de diseñador para enlazar la `SupplierID` campo de datos a la `Suppliers` DropDownList.


[![Enlazar el CategoryID SelectedValue propiedad del DropDownList con enlace de datos bidireccional](customizing-the-data-modification-interface-cs/_static/image29.png)](customizing-the-data-modification-interface-cs/_static/image28.png)

**Figura 10**: enlazar el `CategoryID` a los controles de DropDownList `SelectedValue` enlace de datos bidireccional utilizando de propiedad ([haga clic aquí para ver la imagen a tamaño completo](customizing-the-data-modification-interface-cs/_static/image30.png))


Una vez que se han aplicado los enlaces a la `SelectedValue` propiedades de las listas dos desplegables columnas de categoría y el proveedor del producto editado serán los valores actuales del producto. Al hacer clic en actualizar, la `CategoryID` y `SupplierID` valores de elemento de lista desplegable seleccionado se pasará a la `UpdateProduct` método. La figura 11 muestra el tutorial una vez agregadas las instrucciones de enlace de datos; Tenga en cuenta cómo los elementos de la lista desplegable seleccionada para Chef Anton Cajun Seasoning están correctamente sazonadoras y New Orleans Cajun Delights.


[![El producto de Editar categoría actual y los valores de proveedor están seleccionados de forma predeterminada](customizing-the-data-modification-interface-cs/_static/image32.png)](customizing-the-data-modification-interface-cs/_static/image31.png)

**Figura 11**: The Editar del producto categoría actual y los valores de proveedor están seleccionados de forma predeterminada ([haga clic aquí para ver la imagen a tamaño completo](customizing-the-data-modification-interface-cs/_static/image33.png))


## <a name="handlingnullvalues"></a>Control`NULL`valores

El `CategoryID` y `SupplierID` columnas en la `Products` tabla puede ser `NULL`, pero las listas desplegables en el `EditItemTemplate` s no incluya un elemento de lista para representar un `NULL` valor. Esto tiene dos consecuencias:

- Usuario no puede utilizar la interfaz para cambiar la categoría o el proveedor de un producto de no`NULL` valor a un `NULL` uno
- Si un producto tiene un `NULL` `CategoryID` o `SupplierID`, haga clic en el botón Editar se producirá una excepción. Esto es porque el `NULL` valor devuelto por `CategoryID` (o `SupplierID`) en el `Bind()` instrucción no se asigna a un valor en DropDownList (DropDownList produce una excepción cuando su `SelectedValue` propiedad está establecida en un valor *no* en su colección de elementos de lista).

Para admitir `NULL` `CategoryID` y `SupplierID` valores, tenemos que agregar otro `ListItem` a cada DropDownList para representar la `NULL` valor. En el [filtrado de maestro y detalles con un DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) tutorial, hemos visto cómo agregar otro `ListItem` para un enlace de datos DropDownList, que implicaba la configuración de DropDownList `AppendDataBoundItems` propiedad `true` y cómo agregar manualmente el adicionales `ListItem`. En ese tutorial anterior, sin embargo, hemos agregado un `ListItem` con un `Value` de `-1`. Sin embargo, la lógica de enlace de datos en ASP.NET, convertirá automáticamente en una cadena vacía para un `NULL` valor y un viceversa. Por lo tanto, para este tutorial desea la `ListItem`del `Value` para ser una cadena vacía.

Inicio estableciendo ambas listas desplegables `AppendDataBoundItems` propiedad `true`. A continuación, agregue el `NULL` `ListItem` agregando las siguientes `<asp:ListItem>` elemento para cada DropDownList para que el marcado declarativo asemeje como:


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample6.aspx)]

He decidido usar "(ninguno)" como el valor de texto para este `ListItem`, pero puede cambiarlo para ser también una cadena en blanco si lo desea.

> [!NOTE]
> Como vimos en el *filtrado de maestro y detalles con un DropDownList* tutorial, `ListItem` s pueden agregarse a DropDownList a través del diseñador haciendo clic en el DropDownList `Items` propiedad en la ventana Propiedades (que se mostrará el `ListItem` Editor de la colección). Sin embargo, no olvide agregar la `NULL` `ListItem` para este tutorial a través de la sintaxis declarativa. Si usas el `ListItem` Editor de colecciones, la sintaxis declarativa generada omitirá la `Value` valor por completo cuando asigna una cadena en blanco, crear marcado declarativo como: `<asp:ListItem>(None)</asp:ListItem>`. Aunque esto puede parecer inofensivo, el valor que falta hace DropDownList usar la `Text` valor de propiedad en su lugar. Que significa que si esto `NULL` `ListItem` está seleccionado, se intentará el valor "(ninguno)" que se asignará a la `CategoryID`, lo que producirá una excepción. Si se establece explícitamente `Value=""`, `NULL` valor se asignará a `CategoryID` cuando el `NULL` `ListItem` está seleccionada.


Repita estos pasos para los proveedores de DropDownList.

Con esto adicionales `ListItem`, ahora puede asignar la interfaz de edición `NULL` valores para un producto `CategoryID` y `SupplierID` campos, tal como se muestra en la figura 12.


[![Elija (ninguno) para asignar un valor NULL para la categoría o el proveedor de un producto](customizing-the-data-modification-interface-cs/_static/image35.png)](customizing-the-data-modification-interface-cs/_static/image34.png)

**Figura 12**: elija (ninguno) para asignar un `NULL` valor de categoría o el proveedor de un producto ([haga clic aquí para ver la imagen a tamaño completo](customizing-the-data-modification-interface-cs/_static/image36.png))


## <a name="step-4-using-radiobuttons-for-the-discontinued-status"></a>Paso 4: Uso de botones de opción para que el estado no incluido

Actualmente los productos `Discontinued` campo de datos se expresa mediante un CampoCasillaVerificación, que representa una casilla de verificación deshabilitada para las filas de solo lectura y una casilla de verificación habilitado para la fila que se está edita. Mientras esta interfaz de usuario es a menudo conveniente, se puede personalizar si es necesario usar TemplateField. Para este tutorial, vamos a cambiar la CampoCasillaVerificación en TemplateField que utiliza un control RadioButtonList con dos opciones "Activos" y "Suspendido" desde el que el usuario puede especificar el producto `Discontinued` valor.

Iniciar convirtiendo el `Discontinued` CampoCasillaVerificación en TemplateField, lo cual creará un TemplateField con un `ItemTemplate` y `EditItemTemplate`. Ambas plantillas incluyen una casilla de verificación con su `Checked` propiedad enlazada a la `Discontinued` del campo de datos, la única diferencia entre los dos es que el `ItemTemplate`de la casilla de verificación `Enabled` propiedad está establecida en `false`.

Reemplace la casilla de verificación tanto en el `ItemTemplate` y `EditItemTemplate` con un control RadioButtonList, configuración de ambos RadioButtonList `ID` propiedades para `DiscontinuedChoice`. A continuación, indican que el RadioButtonList debe contener dos botones de radio, una con la etiqueta "activa" en cada uno con un valor de "False" y otra con la etiqueta "Suspendido" con un valor "True". Para lograr esto puede escribir el `<asp:ListItem>` elementos directamente a través de la sintaxis declarativa o use el `ListItem` Editor de la colección desde el diseñador. La figura 13 muestra el `ListItem` se han especificado el Editor de la colección después de las dos opciones de botón de radio.


[![Add](customizing-the-data-modification-interface-cs/_static/image38.png)](customizing-the-data-modification-interface-cs/_static/image37.png)

**Figura 13**: agregar opciones de "Suspendido" y "Activo" a RadioButtonList ([haga clic aquí para ver la imagen a tamaño completo](customizing-the-data-modification-interface-cs/_static/image39.png))


Desde RadioButtonList en el `ItemTemplate` no debe ser editable, establecer su `Enabled` propiedad `false`, dejando el `Enabled` propiedad a `true` (valor predeterminado) para RadioButtonList en el `EditItemTemplate`. Esto hará que los botones de radio en la fila no editar como de solo lectura, pero permitirá al usuario cambiar los valores de RadioButton para la fila editada.

Necesitamos asignar los controles de RadioButtonList `SelectedValue` propiedades de modo que se seleccione el botón de radio correspondiente según el producto `Discontinued` campo de datos. Al igual que con las listas desplegables examinado anteriormente en este tutorial, esta sintaxis de enlace de datos puede agregarse directamente en el marcado declarativo o a través del vínculo Editar DataBindings de etiquetas inteligentes de la RadioButtonList.

Después de agregar los dos RadioButtonList y configurarlos, el `Discontinued` marcado declarativo del TemplateField debería ser similar:


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample7.aspx)]

Con estos cambios, la `Discontinued` columna se ha transformado en una lista de casillas de verificación para una lista de pares de botón de radio (Véase la figura 14). Al editar un producto, se selecciona el botón de radio correspondiente y el estado del producto no incluida puede actualizarse seleccionando el botón y haga clic en actualizar.


[![Las casillas de verificación no incluidas se han reemplazado por pares de botón de Radio](customizing-the-data-modification-interface-cs/_static/image41.png)](customizing-the-data-modification-interface-cs/_static/image40.png)

**Figura 14**: The no incluye las casillas de verificación se han reemplazado por pares de botón de Radio ([haga clic aquí para ver la imagen a tamaño completo](customizing-the-data-modification-interface-cs/_static/image42.png))


> [!NOTE]
> Puesto que la `Discontinued` columna en el `Products` base de datos no puede tener `NULL` valores, no necesitamos que preocuparse acerca de cómo capturar `NULL` información en la interfaz. Si es, sin embargo, `Discontinued` podría contener la columna `NULL` valores queremos agregar una tercera opción botón a la lista con su `Value` establecido en una cadena vacía (`Value=""`), igual que con la categoría y el proveedor listas desplegables.


## <a name="summary"></a>Resumen

Aunque las BoundField y CampoCasillaVerificación automáticamente se representan como de solo lectura, editar e insertar interfaces, no tienen la capacidad de personalización. A menudo, sin embargo, será necesario personalizar la edición o la inserción de interfaz, es posible agregar controles de validación (tal y como hemos visto en el tutorial anterior) o mediante la personalización de la interfaz de usuario de la colección de datos (como hemos visto en este tutorial). Personalizar la interfaz con TemplateField se puede resumir en los pasos siguientes:

1. Agregar un TemplateField o convertir un BoundField existente o CampoCasillaVerificación en TemplateField
2. Aumentar la interfaz según sea necesario
3. Enlazar los campos de datos adecuado para los controles Web recién agregados con enlace de datos bidireccional

Además de utilizar los controles Web de ASP.NET integrados, también puede personalizar las plantillas de TemplateField con controles de servidor personalizado, compilado y controles de usuario.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md)
> [Siguiente](implementing-optimistic-concurrency-cs.md)
