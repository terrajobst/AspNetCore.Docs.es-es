---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs
title: Personalizar la interfaz de modificación de datos (C#) | Microsoft Docs
author: rick-anderson
description: En este tutorial, echaremos un vistazo a cómo personalizar la interfaz de un control GridView editable reemplazando el cuadro de texto estándar y controles CheckBox con alterna...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 22e99600-8d18-4a94-a20e-a3a62bb63798
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: 1f8f08fc78ef5cf6d6878daf6b00d6556f585762
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37383722"
---
<a name="customizing-the-data-modification-interface-c"></a>Personalizar la interfaz de modificación de datos (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_20_CS.exe) o [descargar PDF](customizing-the-data-modification-interface-cs/_static/datatutorial20cs1.pdf)

> En este tutorial, echaremos un vistazo a cómo personalizar la interfaz de un control GridView editable reemplazando el cuadro de texto estándar y controles CheckBox con controles Web de entrada alternativos.


## <a name="introduction"></a>Introducción

El BoundFields y CheckBoxFields utilizado por los controles GridView y DetailsView simplifican el proceso de modificación de datos debido a su capacidad para representar las interfaces de solo lectura, puede editables e insertables. Estas interfaces se pueden representar sin la necesidad de agregar cualquier código o marcado declarativo adicional. Sin embargo, las BoundField del CampoCasillaVerificación interfaces y carecen de la capacidad de personalización a menudo es necesario en escenarios del mundo real. Para personalizar la interfaz editable o insertable en GridView o DetailsView necesitamos usar TemplateField.

En el [tutorial anterior](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) vimos cómo personalizar las interfaces de modificación de datos mediante la adición de controles Web de validación. En este tutorial, buscaremos en cómo personalizar los controles de Web de recopilación de datos reales, reemplazando el BoundField y cuadro de texto estándar del CampoCasillaVerificación y controles CheckBox con controles Web de entrada alternativos. En concreto, vamos a crear un control GridView editable que permite que un producto nombre, categoría, proveedor y estado no incluida en actualizarse. Cuando se edita una fila determinada, los campos de categoría y el proveedor se representarán como listas desplegables, que contiene el conjunto de categorías disponibles y proveedores para elegir. Además, vamos a reemplazar predeterminado del CampoCasillaVerificación CheckBox con un control RadioButtonList que ofrece dos opciones: "Activo" y "Suspendido".


[![Interfaz de edición de GridView incluye listas desplegables y botones de opción](customizing-the-data-modification-interface-cs/_static/image2.png)](customizing-the-data-modification-interface-cs/_static/image1.png)

**Figura 1**: objeto GridView edición interfaz incluye DropDownLists y botones de opción ([haga clic aquí para ver imagen en tamaño completo](customizing-the-data-modification-interface-cs/_static/image3.png))


## <a name="step-1-creating-the-appropriateupdateproductoverload"></a>Paso 1: Crear adecuado`UpdateProduct`sobrecargar

En este tutorial vamos a crear un control GridView editable que permite la edición de nombre de un producto, categoría, proveedor y no incluye el estado. Por lo tanto, necesitamos un `UpdateProduct` sobrecarga que acepta cinco parámetros de entrada los valores de estas cuatro productos más el `ProductID`. Al igual que en nuestro sobrecargas anteriores, esto nos una permitirá:

1. Recuperar la información de producto de la base de datos para el elemento especificado `ProductID`,
2. Actualización de la `ProductName`, `CategoryID`, `SupplierID`, y `Discontinued` campos, y
3. Enviar la solicitud de actualización a la capa DAL a través del TableAdapter `Update()` método.

Para mayor brevedad, para esta sobrecarga en particular he omitido la comprobación de la regla de negocios que garantiza un producto que se va a marcar como discontinuo no es el único producto ofrecido por su proveedor. Si quiere, puede agregarlo si lo prefiere, o bien, idealmente, refactorizar la lógica a un método independiente.

El código siguiente muestra la nueva `UpdateProduct` sobrecarga en el `ProductsBLL` clase:


[!code-csharp[Main](customizing-the-data-modification-interface-cs/samples/sample1.cs)]

## <a name="step-2-crafting-the-editable-gridview"></a>Paso 2: Diseñar el control GridView Editable

Con el `UpdateProduct` agrega sobrecarga, ya estamos listos para crear nuestro control GridView editable. Abra el `CustomizedUI.aspx` página en el `EditInsertDelete` carpeta y agregue un control GridView al diseñador. A continuación, cree un nuevo origen ObjectDataSource de etiqueta inteligente de GridView. Configurar el origen ObjectDataSource para recuperar la información de productos a través de la `ProductBLL` la clase `GetProducts()` método y actualizar los datos de producto mediante el `UpdateProduct` sobrecarga que acabamos de crear. En las pestañas INSERT y DELETE, seleccionar (ninguno) en las listas de la lista desplegable.


[![Configurar el origen ObjectDataSource para utilizar la sobrecarga de UpdateProduct acaba de crear](customizing-the-data-modification-interface-cs/_static/image5.png)](customizing-the-data-modification-interface-cs/_static/image4.png)

**Figura 2**: configurar el origen ObjectDataSource que se usarán el `UpdateProduct` sobrecargar acaba de crear ([haga clic aquí para ver imagen en tamaño completo](customizing-the-data-modification-interface-cs/_static/image6.png))


Como hemos visto a lo largo de los tutoriales de modificación de datos, la sintaxis declarativa para el origen ObjectDataSource creado por Visual Studio asigna el `OldValuesParameterFormatString` propiedad `original_{0}`. Esto, por supuesto, no funcionará con la capa de lógica empresarial desde nuestros métodos no espera que el original `ProductID` valor pasarse. Por lo tanto, como hemos hecho en los tutoriales anteriores, dedique un momento para quitar esta asignación de propiedad de la sintaxis declarativa o, en su lugar, establezca el valor de esta propiedad `{0}`.

Después de este cambio, el marcado declarativo de ObjectDataSource debe ser similar al siguiente:


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample2.aspx)]

Tenga en cuenta que el `OldValuesParameterFormatString` se ha quitado la propiedad y que hay un `Parameter` en el `UpdateParameters` colección para cada uno de los parámetros de entrada esperados por nuestro `UpdateProduct` sobrecargar.

Mientras se configura el origen ObjectDataSource para actualizar solo un subconjunto de los valores de producto, el control GridView se muestra actualmente *todas* de los campos de producto. Dedique un momento para editar el control GridView para que:

- Solo incluye el `ProductName`, `SupplierName`, `CategoryName` BoundFields y `Discontinued` CampoCasillaVerificación
- El `CategoryName` y `SupplierName` campos que aparecen antes (a la izquierda de) la `Discontinued` CampoCasillaVerificación
- El `CategoryName` y `SupplierName` 'BoundFields `HeaderText` propiedad está establecida en "Category" y "Proveedor", respectivamente
- Se habilita la compatibilidad de edición (Active la casilla Habilitar edición de etiquetas inteligentes de GridView)

Después de estos cambios, el diseñador tendrá un aspecto similar a la figura 3, con sintaxis declarativa de GridView que se muestra a continuación.


[![Quitar los campos innecesarios del control GridView](customizing-the-data-modification-interface-cs/_static/image8.png)](customizing-the-data-modification-interface-cs/_static/image7.png)

**Figura 3**: quitar los campos innecesarios del control GridView ([haga clic aquí para ver imagen en tamaño completo](customizing-the-data-modification-interface-cs/_static/image9.png))


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample3.aspx)]

En este momento el comportamiento de solo lectura de GridView es completando. Cuando se visualizan los datos, cada producto se representa como una fila en el control GridView que muestra el nombre del producto, categoría, proveedor y no incluye el estado.


[![Interfaz de solo lectura de GridView completada](customizing-the-data-modification-interface-cs/_static/image11.png)](customizing-the-data-modification-interface-cs/_static/image10.png)

**Figura 4**: interfaz de solo lectura del objeto GridView es Complete ([haga clic aquí para ver imagen en tamaño completo](customizing-the-data-modification-interface-cs/_static/image12.png))


> [!NOTE]
> Como se describe en [una visión general de insertar, actualizar y eliminar datos tutorial](an-overview-of-inserting-updating-and-deleting-data-cs.md), es sumamente importante que el control GridView s estado de vista habilitado (comportamiento predeterminado). Si establece la s GridView `EnableViewState` propiedad `false`, corre el riesgo de tener usuarios simultáneos que se eliminen o modifiquen de forma no intencionada de registros. Consulte [advertencia: problema de simultaneidad con ASP.NET 2.0 GridView y DetailsView/FormViews esa compatibilidad de edición o eliminación y cuyo estado de vista está deshabilitado](http://scottonwriting.net/sowblog/posts/10054.aspx) para obtener más información.


## <a name="step-3-using-a-dropdownlist-for-the-category-and-supplier-editing-interfaces"></a>Paso 3: Usando DropDownList para la categoría y la edición de las Interfaces de proveedor

Recuerde que el `ProductsRow` contiene el objeto `CategoryID`, `CategoryName`, `SupplierID`, y `SupplierName` propiedades, que proporcionan los valores reales de identificador de clave externa en el `Products` tabla y la correspondiente base de datos `Name` los valores en el `Categories` y `Suppliers` tablas. El `ProductRow`del `CategoryID` y `SupplierID` ambos se leen y escritos en, mientras que el `CategoryName` y `SupplierName` propiedades son de sólo lectura.

Debido al estado de solo lectura de la `CategoryName` y `SupplierName` propiedades, que haya tenido la correspondientes BoundFields sus `ReadOnly` propiedad establecida en `true`, de modo que estos valores que se va a modificar cuando se edita una fila. Aunque podemos establecer el `ReadOnly` propiedad `false`, representación el `CategoryName` y `SupplierName` BoundFields como cuadros de texto durante la edición, este método producirá una excepción cuando el usuario intenta actualizar el producto, ya que no hay ninguna `UpdateProduct` sobrecarga que toma en `CategoryName` y `SupplierName` entradas. De hecho, no queremos crear una sobrecarga de este tipo por dos motivos:

- El `Products` tabla no tiene `SupplierName` o `CategoryName` campos, pero `SupplierID` y `CategoryID`. Por lo tanto, queremos que nuestro método pasar estos valores de identificador determinados, no los valores de sus tablas de consulta.
- Solicitar al usuario que escriba el nombre del proveedor o categoría no es ideal, ya que requiere que el usuario sepa las categorías disponibles y los proveedores y la ortografía correcta.

Los campos de categoría y el proveedor deberían mostrar la categoría y los nombres de proveedores cuando está en modo de solo lectura (como lo hace ahora) y una lista desplegable de opciones aplicables al que se está editando. Usar una lista desplegable, el usuario final puede ver rápidamente qué categorías y proveedores están disponibles para elegir entre y mucho más fácil hacer su selección.

Para proporcionar este comportamiento, necesitamos convertir el `SupplierName` y `CategoryName` BoundFields en TemplateFields cuyo `ItemTemplate` emite el `SupplierName` y `CategoryName` valores y cuyo `EditItemTemplate` utiliza un control DropDownList para la lista el las categorías disponibles y proveedores.

## <a name="adding-thecategoriesandsuppliersdropdownlists"></a>Agregar el`Categories`y`Suppliers`DropDownLists

Convierta el `SupplierName` y `CategoryName` BoundFields en TemplateFields por: al hacer clic en el vínculo Editar columnas de etiqueta inteligente de GridView; seleccione el BoundField en la lista en la parte inferior izquierda; y haga clic en el "convertir este campo en un Vínculo TemplateField". El proceso de conversión creará TemplateField con ambos un `ItemTemplate` y un `EditItemTemplate`, tal y como se muestra en la sintaxis declarativa siguiente:


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample4.aspx)]

Puesto que el BoundField se marcó como de solo lectura, tanto el `ItemTemplate` y `EditItemTemplate` contienen una etiqueta de Web control cuya `Text` propiedad está enlazada al campo de datos aplicable (`CategoryName`, en la sintaxis anterior). Tenemos que modificar el `EditItemTemplate`, reemplazando el control Web de etiqueta con un control DropDownList.

Como hemos visto en los tutoriales anteriores, se puede editar la plantilla a través del diseñador o directamente desde la sintaxis declarativa. Para editar a través del diseñador, haga clic en el vínculo Editar plantillas de etiqueta inteligente de GridView y elegir trabajar con el campo de categoría `EditItemTemplate`. Quitar el control Web Label y reemplazarlo con un control DropDownList, establecer la propiedad de Id. de DropDownList en `Categories`.


[![Quite el cuadro de texto y agregue un DropDownList a EditItemTemplate](customizing-the-data-modification-interface-cs/_static/image14.png)](customizing-the-data-modification-interface-cs/_static/image13.png)

**Figura 5**: quite el cuadro de texto y agregue un DropDownList para la `EditItemTemplate` ([haga clic aquí para ver imagen en tamaño completo](customizing-the-data-modification-interface-cs/_static/image15.png))


A continuación, se debe rellenar la DropDownList con las categorías disponibles. Haga clic en el vínculo Elegir origen de datos de etiqueta inteligente de DropDownList y optar por crear un nuevo origen ObjectDataSource denominado `CategoriesDataSource`.


[![Crear un nuevo Control ObjectDataSource denominado CategoriesDataSource](customizing-the-data-modification-interface-cs/_static/image17.png)](customizing-the-data-modification-interface-cs/_static/image16.png)

**Figura 6**: crear un nuevo Control ObjectDataSource denominado `CategoriesDataSource` ([haga clic aquí para ver imagen en tamaño completo](customizing-the-data-modification-interface-cs/_static/image18.png))


Para que este origen ObjectDataSource devuelven todas las categorías, enlazarlo a la `CategoriesBLL` la clase `GetCategories()` método.


[![Enlazar el origen ObjectDataSource GetCategories() método del CategoriesBLL](customizing-the-data-modification-interface-cs/_static/image20.png)](customizing-the-data-modification-interface-cs/_static/image19.png)

**Figura 7**: enlazar el origen ObjectDataSource a los `CategoriesBLL`del `GetCategories()` método ([haga clic aquí para ver imagen en tamaño completo](customizing-the-data-modification-interface-cs/_static/image21.png))


Por último, configure la configuración de la DropDownList que la `CategoryName` campo se muestra en cada DropDownList `ListItem` con el `CategoryID` utilizado como el valor del campo.


[![Tiene muestra el campo de nombre de categoría y el Id. de categoría que se utiliza como el valor](customizing-the-data-modification-interface-cs/_static/image23.png)](customizing-the-data-modification-interface-cs/_static/image22.png)

**Figura 8**: tiene la `CategoryName` muestra el campo y el `CategoryID` utiliza como el valor ([haga clic aquí para ver imagen en tamaño completo](customizing-the-data-modification-interface-cs/_static/image24.png))


Después de realizar estos cambios el marcado declarativo para la `EditItemTemplate` en el `CategoryName` TemplateField incluirá DropDownList y un origen ObjectDataSource:


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample5.aspx)]

> [!NOTE]
> DropDownList en el `EditItemTemplate` debe tener el estado de vista habilitado. Pronto agregaremos sintaxis de enlace de datos a la sintaxis declarativa de DropDownList y comandos de enlace de datos como `Eval()` y `Bind()` solo puede aparecer en los controles cuyo estado de vista está habilitado.


Repita estos pasos para agregar un DropDownList denominado `Suppliers` a la `SupplierName` del TemplateField `EditItemTemplate`. Esto implica agregar DropDownList para la `EditItemTemplate` y la creación de ObjectDataSource otro. El `Suppliers` ObjectDataSource de DropDownList, sin embargo, debe configurarse para invocar el `SuppliersBLL` la clase `GetSuppliers()` método. Además, configurar el `Suppliers` DropDownList para mostrar el `CompanyName` campo y use la `SupplierID` campo como el valor de su `ListItem` s.

Después de agregar las listas desplegables para los dos `EditItemTemplate` s, cargue la página en un explorador y haga clic en el botón Editar para el producto de Cajun Seasoning del Chef Antón. Como se muestra en la figura 9, las columnas de categoría y el proveedor del producto se representan como listas desplegables que contiene las categorías disponibles y los proveedores para elegir. Sin embargo, tenga en cuenta que el *primera* elementos en ambas listas desplegables seleccionados de manera predeterminada (bebidas para la categoría) y líquidos exóticos como el proveedor, aunque Cajun Seasoning de Chef Antón es un sazonadoras proporcionado por la nueva Orleans Cajun Terrenales.


[![Se selecciona el primer elemento en la lista desplegable se muestran de forma predeterminada](customizing-the-data-modification-interface-cs/_static/image26.png)](customizing-the-data-modification-interface-cs/_static/image25.png)

**Figura 9**: se selecciona el primer elemento de la lista desplegable se muestran de forma predeterminada ([haga clic aquí para ver imagen en tamaño completo](customizing-the-data-modification-interface-cs/_static/image27.png))


Además, si hace clic en la actualización, se dará cuenta de que el producto `CategoryID` y `SupplierID` valores se establecen en `NULL`. Ambas opciones no deseadas de los comportamientos se deben a que las listas desplegables en el `EditItemTemplate` s no están enlazados a los campos de datos de los datos subyacentes del producto.

## <a name="binding-the-dropdownlists-to-thecategoryidandsupplieriddata-fields"></a>Enlace las listas desplegables a la`CategoryID`y`SupplierID`campos de datos

Para disponer de categoría del producto editado y el proveedor establece listas desplegables para los valores adecuados y que estos valores enviados de vuelta a las BLL `UpdateProduct` método tras hacer clic en actualizar, debemos enlazar 'DropDownLists `SelectedValue` propiedades para el `CategoryID` y `SupplierID` campos de datos mediante el enlace de datos bidireccional. Para lograr esto con la `Categories` DropDownList, puede agregar `SelectedValue='<%# Bind("CategoryID") %>'` directamente a la sintaxis declarativa.

Como alternativa, puede establecer databindings de DropDownList editar la plantilla a través del diseñador y haciendo clic en el vínculo Editar DataBindings de etiqueta inteligente de DropDownList. A continuación, indicar que la `SelectedValue` propiedad debe enlazarse a la `CategoryID` campo utilizando el enlace de datos bidireccional (consulte la figura 10). Repita el proceso declarativo o diseñador para enlazar el `SupplierID` campo de datos a la `Suppliers` DropDownList.


[![Enlazar el CategoryID para la propiedad SelectedValue de DropDownList mediante enlace de datos bidireccional](customizing-the-data-modification-interface-cs/_static/image29.png)](customizing-the-data-modification-interface-cs/_static/image28.png)

**Figura 10**: enlazar el `CategoryID` a los controles de DropDownList `SelectedValue` enlace de datos bidireccional utilizando de propiedad ([haga clic aquí para ver imagen en tamaño completo](customizing-the-data-modification-interface-cs/_static/image30.png))


Una vez que se han aplicado los enlaces para el `SelectedValue` propiedades de los dos DropDownLists, las columnas de categoría y el proveedor del producto editado será valores actuales del producto. Al hacer clic en la actualización, el `CategoryID` y `SupplierID` valores de elemento de lista desplegable seleccionado se pasará a la `UpdateProduct` método. Figura 11 muestra el tutorial después de han agregado las instrucciones de enlace de datos; Tenga en cuenta cómo los elementos de lista desplegable seleccionado para Cajun Seasoning de Chef Antón están correctamente sazonadoras y Nueva Orleans Cajun terrenales.


[![El producto Editar categoría actual y los valores del proveedor se seleccionan de forma predeterminada](customizing-the-data-modification-interface-cs/_static/image32.png)](customizing-the-data-modification-interface-cs/_static/image31.png)

**Figura 11**: The editado del producto categoría actual y los valores del proveedor se seleccionan de forma predeterminada ([haga clic aquí para ver imagen en tamaño completo](customizing-the-data-modification-interface-cs/_static/image33.png))


## <a name="handlingnullvalues"></a>Control`NULL`valores

El `CategoryID` y `SupplierID` columnas en el `Products` tabla puede ser `NULL`, pero las listas desplegables en el `EditItemTemplate` s no incluya un elemento de lista para representar un `NULL` valor. Esto tiene dos consecuencias:

- Usuario no puede usar la interfaz para cambiar la categoría o el proveedor de un producto que no es de`NULL` valor a un `NULL` uno
- Si un producto tiene un `NULL` `CategoryID` o `SupplierID`, al hacer clic en el botón Editar se producirá una excepción. Esto es porque el `NULL` valor devuelto por `CategoryID` (o `SupplierID`) en el `Bind()` instrucción no se asigna a un valor de DropDownList (DropDownList produce una excepción cuando su `SelectedValue` propiedad está establecida en un valor *no* en su colección de elementos de lista).

Con el fin de admitir `NULL` `CategoryID` y `SupplierID` valores, tenemos que agregar otro `ListItem` a cada DropDownList para representar el `NULL` valor. En el [filtrado de maestro y detalles con un DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) tutorial, vimos cómo agregar otro `ListItem` para un enlace de datos DropDownList, que implicaba la configuración de la DropDownList `AppendDataBoundItems` propiedad `true` y cómo agregar manualmente el adicionales `ListItem`. En ese tutorial anterior, sin embargo, hemos agregado un `ListItem` con un `Value` de `-1`. Sin embargo, la lógica de enlace de datos en ASP.NET, convertirá automáticamente en una cadena en blanco a un `NULL` valor y un viceversa. Por lo tanto, para este tutorial desea el `ListItem`del `Value` a ser una cadena vacía.

Empiece por establecer 'dos DropDownLists `AppendDataBoundItems` propiedad `true`. A continuación, agregue el `NULL` `ListItem` agregando el siguiente `<asp:ListItem>` elemento para cada DropDownList para que busca el marcado declarativo, como:


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample6.aspx)]

He elegido usar "(None)" como el valor de texto para este `ListItem`, pero puede cambiarlo para que también sea una cadena en blanco si lo desea.

> [!NOTE]
> Como hemos visto en el *filtrado de maestro y detalles con un DropDownList* tutorial, `ListItem` s se pueden agregar a un DropDownList a través del diseñador, haga clic en la DropDownList `Items` propiedad en la ventana Propiedades (que se mostrará el `ListItem` Editor de la colección). Sin embargo, no olvide agregar el `NULL` `ListItem` para este tutorial a través de la sintaxis declarativa. Si usas el `ListItem` Editor de colecciones, la sintaxis declarativa generada omitirá el `Value` valor por completo cuando asigna una cadena en blanco, creando el marcado declarativo, como: `<asp:ListItem>(None)</asp:ListItem>`. Aunque esto puede parecer inofensivo, el valor que falta hace que la DropDownList usar el `Text` valor de propiedad en su lugar. Que significa que si esto `NULL` `ListItem` está seleccionada, el valor "(None)" se volverá a intentar que se asignará a la `CategoryID`, lo que producirá una excepción. Si se establece explícitamente `Value=""`, un `NULL` valor se asignará a `CategoryID` cuando el `NULL` `ListItem` está seleccionada.


Repita estos pasos para proveedores DropDownList.

Con esto adicionales `ListItem`, ahora puede asignar la interfaz de edición `NULL` valores a un producto `CategoryID` y `SupplierID` campos, tal como se muestra en la figura 12.


[![Elija (ninguno) para asignar un valor NULL para la categoría o el proveedor de un producto](customizing-the-data-modification-interface-cs/_static/image35.png)](customizing-the-data-modification-interface-cs/_static/image34.png)

**Figura 12**: elija (ninguno) para asignar un `NULL` valor de categoría o el proveedor de un producto ([haga clic aquí para ver imagen en tamaño completo](customizing-the-data-modification-interface-cs/_static/image36.png))


## <a name="step-4-using-radiobuttons-for-the-discontinued-status"></a>Paso 4: Uso de botones de opción para el estado no incluido

Actualmente los productos `Discontinued` campo de datos se expresa mediante un CampoCasillaVerificación, que representa una casilla deshabilitada para las filas de solo lectura y una casilla habilitada para la fila que se está edita. Aunque esta interfaz de usuario suele ser adecuada, nos podemos personalizar si es necesario usar TemplateField. Para este tutorial, vamos a cambiar el CampoCasillaVerificación en TemplateField que usa un control RadioButtonList con dos opciones: "Activas" y "Suspendido" desde el que el usuario puede especificar el producto `Discontinued` valor.

Convierta el `Discontinued` CampoCasillaVerificación en TemplateField, lo cual creará un TemplateField con un `ItemTemplate` y `EditItemTemplate`. Ambas plantillas incluyen una casilla de verificación con su `Checked` propiedad enlazada a la `Discontinued` del campo de datos, la única diferencia entre los dos es que el `ItemTemplate`del casilla de verificación `Enabled` propiedad está establecida en `false`.

Reemplace la casilla de verificación en dos la `ItemTemplate` y `EditItemTemplate` con un control RadioButtonList, establecer ambas RadioButtonList `ID` propiedades a `DiscontinuedChoice`. A continuación, indicar que la RadioButtonList debe contener dos botones de radio, uno con la etiqueta "activo" en cada uno con un valor de "False" y otro con la etiqueta "Suspendido" con un valor "True". Para lograr esto puede escribir el `<asp:ListItem>` elementos directamente a través de la sintaxis declarativa o use el `ListItem` Editor de la colección desde el diseñador. Figura 13 se muestra el `ListItem` Editor de la colección después de las dos opciones de botón de radio se han especificado.


[![Agregar](customizing-the-data-modification-interface-cs/_static/image38.png)](customizing-the-data-modification-interface-cs/_static/image37.png)

**Figura 13**: agregar opciones de "Suspendido" y "Activo" a RadioButtonList ([haga clic aquí para ver imagen en tamaño completo](customizing-the-data-modification-interface-cs/_static/image39.png))


Desde RadioButtonList en el `ItemTemplate` no debe ser editable, establecer su `Enabled` propiedad `false`, dejando el `Enabled` propiedad a `true` (valor predeterminado) para RadioButtonList en el `EditItemTemplate`. Esto hará que los botones de radio en la fila no editado como de solo lectura, pero permitirá al usuario cambiar los valores de botón de opción para la fila editada.

Necesitamos asignar los controles RadioButtonList `SelectedValue` propiedades para que se selecciona el botón de radio correspondiente según el producto `Discontinued` campo de datos. Al igual que con las listas desplegables examinado anteriormente en este tutorial, esta sintaxis de enlace de datos puede agregarse directamente en el marcado declarativo o a través del vínculo Editar DataBindings de etiquetas inteligentes de RadioButtonList.

Después de agregar los dos RadioButtonList y configurarlos, los `Discontinued` marcado declarativo del TemplateField debería ser similar a:


[!code-aspx[Main](customizing-the-data-modification-interface-cs/samples/sample7.aspx)]

Con estos cambios, la `Discontinued` columna se ha transformado en una lista de casillas de verificación para obtener una lista de pares de botón de radio (vea la figura 14). Al editar un producto, se selecciona el botón de radio correspondiente y se puede actualizar el estado del producto no incluida seleccionando el botón y hacer clic en actualizar.


[![Las casillas de verificación no incluidas han sido reemplazados por pares de botón de Radio](customizing-the-data-modification-interface-cs/_static/image41.png)](customizing-the-data-modification-interface-cs/_static/image40.png)

**Figura 14**: The no incluye las casillas de verificación se reemplazaron por pares de botón de Radio ([haga clic aquí para ver imagen en tamaño completo](customizing-the-data-modification-interface-cs/_static/image42.png))


> [!NOTE]
> Puesto que la `Discontinued` columna en el `Products` base de datos no puede tener `NULL` valores, no necesitamos que preocuparse acerca de cómo capturar `NULL` información en la interfaz. If, sin embargo, `Discontinued` podría contener la columna `NULL` radio de los valores que queremos agregar un tercer botón a la lista con sus `Value` establecido en una cadena vacía (`Value=""`), al igual que con la categoría y proveedor listas desplegables.


## <a name="summary"></a>Resumen

Mientras el BoundField y CampoCasillaVerificación representan automáticamente interfaces de solo lectura, edición e inserción, que carecen de la capacidad de personalización. A menudo, sin embargo, se deberá personalizar la edición o insertar la interfaz, quizás agregando controles de validación (como hemos visto en el tutorial anterior) o mediante la personalización de la interfaz de usuario de la colección de datos (como hemos visto en este tutorial). Personalizar la interfaz con un TemplateField se puede resumir en los pasos siguientes:

1. Agregar un TemplateField o convertir un BoundField existente o CampoCasillaVerificación en TemplateField
2. Aumentar la interfaz según sea necesario
3. Enlazar los campos de datos correspondientes a los controles Web recién agregados mediante el enlace de datos bidireccional

Además de utilizar los controles Web de ASP.NET integrados, también puede personalizar las plantillas de TemplateField con controles de servidor personalizado, compilado y controles de usuario.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md)
> [Siguiente](implementing-optimistic-concurrency-cs.md)
