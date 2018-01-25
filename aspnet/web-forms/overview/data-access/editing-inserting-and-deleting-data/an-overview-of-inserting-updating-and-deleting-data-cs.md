---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs
title: "Información general de insertar, actualizar y eliminar datos (C#) | Documentos de Microsoft"
author: rick-anderson
description: "En este tutorial veremos cómo asignar Insert() de un ObjectDataSource, Update(), y las clases Delete() métodos a los métodos de BLL, así como cómo configu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: b651dc58-93c7-4f83-a74e-3b99f6d60848
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs
msc.type: authoredcontent
ms.openlocfilehash: e483c37cc773a7255f18c26bc3609d68f71dff7d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="an-overview-of-inserting-updating-and-deleting-data-c"></a>Información general de insertar, actualizar y eliminar datos (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_16_CS.exe) o [descarga de PDF](an-overview-of-inserting-updating-and-deleting-data-cs/_static/datatutorial16cs1.pdf)

> En este tutorial veremos cómo asignar Insert() de un ObjectDataSource, Update(), y las clases Delete() métodos a los métodos de BLL, y cómo configurar los controles GridView, DetailsView y FormView para proporcionar capacidades de modificación de datos.


## <a name="introduction"></a>Introducción

En los últimos tutoriales varias hemos examinado cómo mostrar datos en una página ASP.NET mediante los controles GridView, DetailsView y FormView. Estos controles simplemente trabajan con datos proporcionados a ellos. Normalmente, estos controles obtener acceso a datos mediante el uso de un control de origen de datos, como ObjectDataSource. Hemos visto cómo ObjectDataSource actúa como un proxy entre la página ASP.NET y los datos subyacentes. Cuando se necesita un control GridView para mostrar los datos, invoca su ObjectDataSource `Select()` método, que a su vez, se invoca un método de la capa de lógica empresarial (BLL), que llama a un método en adecuado datos acceso de la capa (DAL) TableAdapter, que a su vez envía un `SELECT` consulta a la base de datos Northwind.

Recuerde que cuando se crean los TableAdapters en la capa DAL en [nuestro primer tutorial](../introduction/creating-a-data-access-layer-cs.md), Visual Studio agrega automáticamente métodos para insertar, actualizar, y eliminar datos de subyacente de la tabla de base de datos. Además, en [creación de una capa de lógica de negocios](../introduction/creating-a-business-logic-layer-cs.md) diseñamos métodos en la capa BLL que invocó a estos métodos de la capa DAL de modificación de datos.

Además su `Select()` método, también tiene la ObjectDataSource `Insert()`, `Update()`, y `Delete()` métodos. Al igual que el `Select()` método, estos tres métodos pueden asignarse a métodos en un objeto subyacente. Cuando se configura para insertar, actualizar o eliminar datos, los controles GridView, DetailsView y FormView proporcionan una interfaz de usuario para modificar los datos subyacentes. Llama a esta interfaz de usuario la `Insert()`, `Update()`, y `Delete()` métodos de ObjectDataSource, que, a continuación, invocación al objeto subyacente asociado métodos (consulte la figura 1).


[![El ObjectDataSource Insert(), Update() y métodos de Delete() actúan como un Proxy en la capa BLL](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image2.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image1.png)

**Figura 1**: The ObjectDataSource `Insert()`, `Update()`, y `Delete()` métodos actuar como un Proxy en la capa BLL ([haga clic aquí para ver la imagen a tamaño completo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image3.png))


En este tutorial veremos cómo asignar el ObjectDataSource `Insert()`, `Update()`, y `Delete()` métodos a los métodos de clases en el BLL, así como cómo configurar los controles GridView, DetailsView y FormView para proporcionar la modificación de datos capacidades.

## <a name="step-1-creating-the-insert-update-and-delete-tutorials-web-pages"></a>Paso 1: Crear el Insert, Update y Delete tutoriales Web páginas

Antes de empezar a explorar cómo insertar, actualizar y eliminar datos, primero dedique un momento para crear las páginas ASP.NET en el proyecto de sitio Web que se necesitará para este tutorial y los varios siguientes. Empiece por agregar una nueva carpeta denominada `EditInsertDelete`. A continuación, agregue las siguientes páginas ASP.NET a la carpeta y asegúrese de que asociar cada página con el `Site.master` página maestra:

- `Default.aspx`
- `Basics.aspx`
- `DataModificationEvents.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`


![Agregar las páginas ASP.NET para los tutoriales relacionados con la modificación de datos](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image4.png)

**Figura 2**: agregar las páginas ASP.NET para los tutoriales relacionados con la modificación de datos


Al igual que en las otras carpetas, `Default.aspx` en el `EditInsertDelete` carpeta enumerará los tutoriales en su sección. Recuerde que el `SectionLevelTutorialListing.ascx` Control de usuario proporciona esta funcionalidad. Por lo tanto, agregue este Control de usuario para `Default.aspx` arrastrándolo desde el Explorador de soluciones en la vista de diseño de la página.


[![Agregar el Control de usuario SectionLevelTutorialListing.ascx a Default.aspx](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image6.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image5.png)

**Figura 3**: agregar la `SectionLevelTutorialListing.ascx` Control de usuario a `Default.aspx` ([haga clic aquí para ver la imagen a tamaño completo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image7.png))


Por último, agregue las páginas como entradas para el `Web.sitemap` archivo. En concreto, agregue el siguiente marcado después Customized Formatting `<siteMapNode>`:


[!code-xml[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample1.xml)]

Después de actualizar `Web.sitemap`, tómese un momento para ver el sitio Web de tutoriales a través de un explorador. El menú de la izquierda ahora incluye elementos para modificar, insertar y eliminar los tutoriales.


![El mapa del sitio ahora incluye las entradas para modificar, insertar y eliminar los tutoriales](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image8.png)

**Figura 4**: la asignación de sitio ahora incluye las entradas para modificar, insertar y eliminar los tutoriales


## <a name="step-2-adding-and-configuring-the-objectdatasource-control"></a>Paso 2: Agregar y configurar el Control ObjectDataSource

Desde la GridView, DetailsView y FormView cada uno de ellos difieren en sus capacidades de modificación de datos y el diseño, vamos a examinar cada uno de ellos individualmente. En lugar de cada control mediante su propio ObjectDataSource, sin embargo, vamos a basta con crear una sola ObjectDataSource que pueden compartir todos los ejemplos de control tres.

Abra la `Basics.aspx` página, arrastre un ObjectDataSource del cuadro de herramientas hasta el diseñador y haga clic en el vínculo Configurar origen de datos de la etiqueta inteligente. Puesto que el `ProductsBLL` es la única clase BLL que proporciona la edición, inserción y eliminación métodos, configuran el ObjectDataSource para utilizar esta clase.


[![Configurar el ObjectDataSource para utilizar la clase ProductsBLL](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image10.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image9.png)

**Figura 5**: configurar el ObjectDataSource que se utilizan los `ProductsBLL` clase ([haga clic aquí para ver la imagen a tamaño completo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image11.png))


En la siguiente pantalla podemos especificar qué métodos de la `ProductsBLL` clase están asignadas a la ObjectDataSource `Select()`, `Insert()`, `Update()`, y `Delete()` , seleccionando la ficha adecuada y el método de la lista desplegable. Figura 6, que deberían estar familiarizados hasta ahora, se asigna el ObjectDataSource `Select()` método a la `ProductsBLL` la clase `GetProducts()` método. El `Insert()`, `Update()`, y `Delete()` métodos se pueden configurar seleccionando la ficha correspondiente en la lista en la parte superior.


[![Hay ObjectDataSource devolver todos los productos](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image13.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image12.png)

**Figura 6**: tiene la ObjectDataSource devolver todos los de los productos ([haga clic aquí para ver la imagen a tamaño completo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image14.png))


Pestañas de las figuras 7, 8 y 9 muestran el ObjectDataSource UPDATE, INSERT y DELETE. Configurar estas pestañas para que la `Insert()`, `Update()`, y `Delete()` métodos invocan la `ProductsBLL` la clase `UpdateProduct`, `AddProduct`, y `DeleteProduct` métodos, respectivamente.


[![Map (método) de ObjectDataSource Update() al método de la clase ProductBLL UpdateProduct](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image16.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image15.png)

**Figura 7**: asignar el ObjectDataSource `Update()` método a la `ProductBLL` la clase `UpdateProduct` método ([haga clic aquí para ver la imagen a tamaño completo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image17.png))


[![Map (método) de ObjectDataSource Insert() al método de la clase ProductBLL AddProduct](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image19.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image18.png)

**Figura 8**: asignar el ObjectDataSource `Insert()` método a la `ProductBLL` agregar la clase `Product` método ([haga clic aquí para ver la imagen a tamaño completo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image20.png))


[![Map (método) de ObjectDataSource Delete() al método de la clase ProductBLL DeleteProduct](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image22.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image21.png)

**Figura 9**: asignar el ObjectDataSource `Delete()` método a la `ProductBLL` la clase `DeleteProduct` método ([haga clic aquí para ver la imagen a tamaño completo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image23.png))


Es podrán que haya observado que las listas desplegables en las pestañas UPDATE, INSERT y DELETE ya tenían estos métodos seleccionados. Gracias a nuestro uso de la `DataObjectMethodAttribute` que decora los métodos de `ProducstBLL`. Por ejemplo, el método DeleteProduct tiene la siguiente firma:


[!code-csharp[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample2.cs)]

El `DataObjectMethodAttribute` atributo indica el propósito de cada método, ya sea para seleccionar, insertar, actualizar, o eliminar y si no es el valor predeterminado. Si omite estos atributos al crear las clases BLL, necesita seleccionar manualmente los métodos de la actualización, INSERTARÁ y eliminar las fichas.

Después de asegurarse de que la correspondiente `ProductsBLL` métodos se asignan a la ObjectDataSource `Insert()`, `Update()`, y `Delete()` métodos, haga clic en Finalizar para completar el asistente.

## <a name="examining-the-objectdatasources-markup"></a>Examen de marcado de ObjectDataSource

Después de configurar el ObjectDataSource a través de su asistente, vaya a la vista de código fuente para examinar el marcado declarativo generado. La `<asp:ObjectDataSource>` etiqueta Especifica el objeto subyacente y los métodos de invocación. Además, existen `DeleteParameters`, `UpdateParameters`, y `InsertParameters` que se asignan a los parámetros de entrada para el `ProductsBLL` la clase `AddProduct`, `UpdateProduct`, y `DeleteProduct` métodos:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample3.aspx)]

ObjectDataSource incluye un parámetro para cada uno de los parámetros de entrada para sus métodos asociados, como una lista de `SelectParameter` s está presente cuando se configura el ObjectDataSource para llamar a un método de selección que espera un parámetro de entrada (por ejemplo, `GetProductsByCategoryID(categoryID)`). Como veremos en breve, valores de estas `DeleteParameters`, `UpdateParameters`, y `InsertParameters` se establecen automáticamente la GridView, DetailsView y FormView antes de invocar el ObjectDataSource `Insert()`, `Update()`, o `Delete()` método. Estos valores también pueden establecerse mediante programación según sea necesario, tal y como se abordará más adelante en un tutorial posterior.

Un efecto secundario de utilizar el Asistente para configurar a ObjectDataSource es que Visual Studio establece la [propiedad OldValuesParameterFormatString](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.oldvaluesparameterformatstring(VS.80).aspx) a `original_{0}`. Valor de esta propiedad se usa para incluir los valores originales de los datos que se está editando y es útil en dos escenarios:

- Si, al editar un registro, los usuarios pueden cambiar el valor de clave principal. En este caso, se deben proporcionar el nuevo valor de clave principal y el valor de clave principal original para que el registro con el valor de clave principal original pueda encontrarse y tiene el valor que se actualiza en consecuencia.
- Cuando se utiliza simultaneidad optimista. Optimista de simultaneidad es una técnica para asegurarse de que dos usuarios simultáneos no sobrescriben los cambios del otro y son el tema para obtener un tutorial futuras.

El `OldValuesParameterFormatString` propiedad indica el nombre de los parámetros de entrada en la actualización del objeto subyacente y los métodos de eliminación para los valores originales. Trataremos esta propiedad y su finalidad con mayor detalle cuando se explora la simultaneidad optimista. Abrirá, ahora, sin embargo, dado que los métodos de nuestro BLL no espera que los valores originales y, por tanto, es importante que se quite esta propiedad. Dejar la `OldValuesParameterFormatString` propiedad establecida en algo distinto del predeterminado (`{0}`) se producirá un error cuando trate de un control Web de datos invocar el ObjectDataSource `Update()` o `Delete()` métodos ya que el ObjectDataSource intentar pasar tanto en el `UpdateParameters` o `DeleteParameters` especificado, así como parámetros de valor original.

Si esto no está muy claro en ese momento, no se preocupe, examinaremos esta propiedad y su utilidad en un tutorial posterior. Por ahora, solo Asegúrese de quitar esta declaración de propiedad del todo en la sintaxis declarativa o establezca el valor en el valor predeterminado ({0}).

> [!NOTE]
> Si simplemente limpiar el `OldValuesParameterFormatString` valor de la propiedad de la ventana de propiedades en la vista de diseño, la propiedad seguirá existiendo en la sintaxis declarativa, pero se establece en una cadena vacía. Esto, Lamentablemente, todavía producirá el mismo problema descrito anteriormente. Por lo tanto, quite la propiedad por completo de la sintaxis declarativa, o bien, en la ventana Propiedades, establezca el valor en el valor predeterminado, `{0}`.


## <a name="step-3-adding-a-data-web-control-and-configuring-it-for-data-modification"></a>Paso 3: Agregar un Control de datos Web y configurarlo para la modificación de datos

Una vez que se ha agregado a la página y configurado el ObjectDataSource, estamos listos agregar controles Web de datos a la página para mostrar los datos y proporcionan un medio para el usuario final modificarlo. Trataremos la GridView, DetailsView y FormView por separado, ya que estos datos en controles Web variar en sus capacidades de modificación de datos y la configuración.

Tal y como se verá en el resto de este artículo, agregar la edición muy básica, insertar y eliminar el soporte técnico a través del control GridView, DetailsView, y controla FormView es tan fácil como la comprobación de un par de casillas de verificación. Hay muchos matices y casos avanzados del mundo real que proporciona esta funcionalidad más compleja que simplemente y haga clic en. Este tutorial, sin embargo, se centra exclusivamente en lo que demuestra las capacidades de modificación de datos simplista. Tutoriales futuros examinará preocupaciones que surjan indudablemente en una configuración del mundo real.

## <a name="deleting-data-from-the-gridview"></a>Eliminar datos de GridView

Iniciar, arrastre un control GridView del cuadro de herramientas hasta el diseñador. A continuación, enlazar el ObjectDataSource en GridView, selecciónelo en la lista desplegable de etiqueta inteligente de GridView. En este momento marcado declarativo del GridView será:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample4.aspx)]

Enlazar el control GridView a ObjectDataSource mediante su etiqueta inteligente tiene dos ventajas:

- BoundFields y CheckBoxFields se crean automáticamente para cada uno de los campos devueltos por la ObjectDataSource. Además, el BoundField y del CampoCasillaVerificación propiedades se establecen según los metadatos del campo subyacente. Por ejemplo, el `ProductID`, `CategoryName`, y `SupplierName` campos están marcados como de solo lectura en el `ProductsDataTable` y, por tanto, no debe ser actualizable cuando se edita. Para dar cabida a este, estas BoundFields [propiedades ReadOnly](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.readonly(VS.80).aspx) se establecen en `true`.
- El [propiedad DataKeyNames](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames(VS.80).aspx) se asigna a los campos de clave principales del objeto subyacente. Esto es muy importante cuando ese único con GridView para modificar o eliminar datos, como esta propiedad indica el campo (o conjunto de campos) identifica cada registro. Para obtener más información sobre la `DataKeyNames` propiedad, hacen referencia a la [principal/detalle mediante un GridView maestro seleccionable con un DetailView de detalles](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) tutorial.

Mientras que GridView puede estar limitado por el ObjectDataSource a través de la ventana Propiedades o la sintaxis declarativa, si lo hace, necesita agregar manualmente el BoundField adecuado y `DataKeyNames` marcado.

El control GridView proporciona compatibilidad integrada para la edición de nivel de fila y eliminar. Configurar un control GridView para admitir la eliminación, agrega una columna de botones de eliminación. Cuando el usuario final hace clic en el botón Eliminar para una fila determinada, seguidamente, tiene lugar una devolución de datos y GridView realiza los pasos siguientes:

1. El ObjectDataSource `DeleteParameters` se asignan valores
2. El ObjectDataSource `Delete()` se invoca el método, eliminando el registro especificado
3. GridView propio Reenlaces a ObjectDataSource invocando su `Select()` (método)

Los valores asignados a la `DeleteParameters` son los valores de la `DataKeyNames` campos de la fila cuyo botón Eliminar se hizo clic. Por lo tanto, es esencial que una GridView `DataKeyNames` propiedad establecerse correctamente. Si está presente, el `DeleteParameters` se va a asignar un `null` valor en el paso 1, lo que a su vez no dará lugar en cualquier elimina registros en el paso 2.

> [!NOTE]
> El `DataKeys` colección se almacena en el estado del control GridView s, lo que significa que el `DataKeys` valores se recordará a través de la devolución de datos incluso si se ha deshabilitado el estado de vista de s de GridView. Sin embargo, es muy importante que el estado de vista sigue estando habilitado para GridView que admiten editen o eliminen (comportamiento predeterminado). Si establece la s GridView `EnableViewState` propiedad `false`, la edición y eliminación de comportamiento funcionará sin problemas para un solo usuario, pero si hay usuarios con acceso simultáneos al eliminar datos, existe la posibilidad de que estos usuarios simultáneos pueden accidentalmente eliminar o editar que registra Professionals t previsto. Consulte mi entrada de blog, [advertencia: problema de simultaneidad con ASP.NET 2.0 GridView/DetailsView/FormViews que admiten la edición o eliminación y cuyo estado de vista está deshabilitado](http://scottonwriting.net/sowblog/archive/2006/10/03/163215.aspx), para obtener más información.


Esta advertencia mismo también se aplica a DetailsViews y FormViews.

Para agregar capacidades de eliminación a un control GridView, vaya a su etiqueta inteligente y Active la casilla Habilitar eliminación.


![Compruebe los permiten eliminar casilla de verificación](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image24.png)

**Figura 10**: Compruebe los permiten eliminar casilla de verificación


Comprobación de la casilla de verificación Habilitar eliminación de la etiqueta inteligente, agrega un CommandField a GridView. El CommandField representa una columna de GridView con botones para llevar a cabo una o varias de las siguientes tareas: al seleccionar un registro, editar un registro y eliminar un registro. Hemos visto anteriormente el CommandField en la acción con la selección de registros en la [principal/detalle mediante un GridView maestro seleccionable con un DetailView de detalles](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) tutorial.

El CommandField contiene un número de `ShowXButton` propiedades que indican qué conjunto de botones que se muestra en la CommandField. Activando la casilla de verificación Habilitar eliminación un CommandField cuyo `ShowDeleteButton` propiedad es `true` se ha agregado a la colección de columnas de GridView.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample5.aspx)]

En este momento, aunque no lo crea, acabamos con agregar compatibilidad eliminando a GridView! Como se muestra en la figura 11, cuando visite esta página a través de un explorador, una columna de botones de Delete está presente.


[![El CommandField agrega una columna de botones de eliminación](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image26.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image25.png)

**Figura 11**: el CommandField agrega una columna de botones Eliminar ([haga clic aquí para ver la imagen a tamaño completo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image27.png))


Si ha sido generando este tutorial desde el principio por su cuenta, al probar esta página al hacer clic en el botón Eliminar, producirá una excepción. Siga leyendo para obtener información sobre por qué se produjeron las siguientes excepciones y cómo corregirlos.

> [!NOTE]
> Si va a lo largo de mediante la descarga que acompaña a este tutorial, estos problemas se ya se tiene en cuenta. Sin embargo, recomendamos que lea los detalles enumerados a continuación para ayudar a identificar problemas que pueden surgir y soluciones alternativas adecuadas.


Si, al intentar eliminar un producto, obtendrá una excepción cuyo mensaje es similar a "*ObjectDataSource 'ObjectDataSource1' no pudo encontrar un método no genérico 'DeleteProduct' que tiene parámetros: productID, original\_ ProductID*, "es probable que haya olvidado de quitar el `OldValuesParameterFormatString` propiedad de ObjectDataSource. Con el `OldValuesParameterFormatString` especifica la propiedad, el ObjectDataSource intenta pasar tanto en `productID` y `original_ProductID` parámetros de entrada que el `DeleteProduct` método. `DeleteProduct`, sin embargo, solo acepta un único parámetro de entrada, por lo tanto, la excepción. Quitar el `OldValuesParameterFormatString` propiedad (o si se establece en `{0}`) indica el ObjectDataSource para no intenta pasar en el parámetro de entrada original.


[![Asegúrese de que se ha borrado la propiedad OldValuesParameterFormatString Out](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image29.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image28.png)

**Figura 12**: asegúrese de que el `OldValuesParameterFormatString` propiedad ha sido desactivada Out ([haga clic aquí para ver la imagen a tamaño completo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image30.png))


Incluso si se hubiera quitado la `OldValuesParameterFormatString` propiedad, todavía obtendrá una excepción al intentar eliminar un producto con el mensaje: "*la instrucción DELETE entran en conflicto con la restricción de referencia ' FK\_orden\_detalles \_Productos*. " La base de datos de Northwind contiene una restricción de clave externa entre la `Order Details` y `Products` tabla, lo que significa que no se puede eliminar un producto del sistema si hay uno o más registros para él en el `Order Details` tabla. Puesto que todos los productos en la base de datos Northwind tiene al menos un registro `Order Details`, no podemos eliminar todos los productos hasta que se elimine primero los registros de detalles de pedido asociado del producto.


[![Una restricción Foreign Key prohíbe la eliminación de productos](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image32.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image31.png)

**Figura 13**: una restricción Foreign Key prohíbe la eliminación de productos ([haga clic aquí para ver la imagen a tamaño completo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image33.png))


En nuestro tutorial, vamos a elimine todos los registros desde el `Order Details` tabla. En una aplicación del mundo real se necesitarían ya sea:

- Tiene otra pantalla para administrar la información de detalles de pedido
- Aumentar la `DeleteProduct` método para incluir una lógica para eliminar los detalles del pedido del producto especificado
- Modificar la consulta SQL utilizada por el objeto TableAdapter que incluyen la eliminación de los detalles del pedido del producto especificado

Vamos a elimine todos los registros desde el `Order Details` tabla para evitar la restricción foreign key. Vaya al explorador de servidores en Visual Studio, haga doble clic en el `NORTHWND.MDF` nodo y elija la nueva consulta. A continuación, en la ventana de consulta, ejecute la siguiente instrucción SQL:`DELETE FROM [Order Details]`


[![Eliminar todos los registros de la tabla Order Details](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image35.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image34.png)

**Figura 14**: eliminar todos los registros de la `Order Details` tabla ([haga clic aquí para ver la imagen a tamaño completo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image36.png))


Después de borrando la `Order Details` tabla haciendo clic en el botón Eliminar, eliminará el producto sin errores. Si al hacer clic en el botón Eliminar no elimina el producto, compruebe para asegurarse de que la GridView `DataKeyNames` propiedad está establecida en el campo de clave principal (`ProductID`).

> [!NOTE]
> Al hacer clic en el botón Eliminar tiene lugar una devolución de datos y se elimina el registro. Esto puede ser peligroso porque es fácil accidentalmente, haga clic en botón de eliminar la fila incorrecta. En un futuro tutorial veremos cómo agregar una confirmación de cliente cuando se elimina un registro.


## <a name="editing-data-with-the-gridview"></a>Edición de datos con el control GridView

Además de eliminar, el control GridView también proporciona compatibilidad integrada de edición de nivel de fila. Configurar un control GridView para admitir la edición, agrega una columna de botones Editar. Desde la perspectiva del usuario final, haga clic en las causas de botón de edición de una fila que la fila se convierta en modificable, convirtiendo las celdas en cuadros de texto que contiene los valores existentes y reemplazar los botones de cancelación y el botón Editar con la actualización. Después de realizar los cambios deseados, el usuario final puede haga clic en el botón de actualización para confirmar los cambios o Cancelar para que se descarten. En cualquier caso, tras hacer clic en la actualización o en Cancelar GridView devuelve a su estado anterior de edición.

Desde la perspectiva como programador de la página, cuando el usuario final hace clic en el botón Editar para una fila determinada, seguidamente, tiene lugar una devolución de datos y GridView realiza los pasos siguientes:

1. La GridView `EditItemIndex` propiedad se asigna al índice de la fila cuyo botón Edición se hizo clic
2. GridView propio Reenlaces a ObjectDataSource invocando su `Select()` (método)
3. El índice de fila que coincide con el `EditItemIndex` se representa en "modo de edición". En este modo, el botón de edición se reemplaza con botones Actualizar y Cancelar y BoundFields cuyo `ReadOnly` propiedades son False (valor predeterminado) se representan como controles de cuadro de texto Web cuyo `Text` propiedades se asignan a los valores de los campos de datos.

En este momento se devuelve el marcado en el explorador, que permite al usuario final realizar cambios en los datos de la fila. Cuando el usuario hace clic en el botón de actualización, se produce un postback y GridView realiza los pasos siguientes:

1. El ObjectDataSource `UpdateParameters` valores se asignan los valores especificados por el usuario final en la interfaz de edición de GridView
2. El ObjectDataSource `Update()` se invoca el método, actualizar el registro especificado
3. GridView propio Reenlaces a ObjectDataSource invocando su `Select()` (método)

Los valores de clave principal que se asigna a la `UpdateParameters` en el paso 1 proceden de los valores especificados en la `DataKeyNames` propiedad, mientras que los valores de clave no principal proceden del texto de los controles Web de cuadro de texto de la fila editada. Como con la eliminación, es fundamental que una GridView `DataKeyNames` propiedad establecerse correctamente. Si está presente, el `UpdateParameters` se asignará el valor de clave principal una `null` valor en el paso 1, lo que a su vez no dará lugar en cualquier actualiza registros en el paso 2.

Funcionalidad de edición puede activarse activando la casilla de verificación Habilitar edición de etiquetas inteligentes de GridView simplemente.


![Comprobar la activación de la casilla de verificación de edición](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image37.png)

**Figura 15**: comprobar la activación de la casilla de verificación de edición


Comprobación de la casilla de verificación Habilitar edición agregará un CommandField (si es necesario) y establezca su `ShowEditButton` propiedad `true`. Como se explicó anteriormente, el CommandField contiene un número de `ShowXButton` propiedades que indican qué conjunto de botones que se muestra en la CommandField. Activando la casilla de verificación Habilitar edición agrega la `ShowEditButton` propiedad a la CommandField existente:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample6.aspx)]

Eso es todo lo hay para agregar compatibilidad con edición rudimentario. Como se muestra en Figure16, la interfaz de edición es bastante tosco cada BoundField cuyo `ReadOnly` propiedad está establecida en `false` (valor predeterminado) se representa como un cuadro de texto. Esto incluye los campos como `CategoryID` y `SupplierID`, que son claves con otras tablas.


[![Haga clic en botón de edición de Chai s muestra la fila en modo de edición](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image39.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image38.png)

**Figura 16**: s Chai al hacer clic en el botón de edición muestra la fila en modo de edición ([haga clic aquí para ver la imagen a tamaño completo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image40.png))


Además de solicitar a los usuarios editar directamente los valores de clave externa, se carece de interfaz de la interfaz de edición de las maneras siguientes:

- Si el usuario escribe un `CategoryID` o `SupplierID` que no existe en la base de datos, la `UPDATE` infringen una restricción foreign key, provocando una excepción que se genere.
- La interfaz de edición no incluye ninguna validación. Si no proporciona un valor necesario (como `ProductName`), o escriba un valor de cadena donde se espera un valor numérico (por ejemplo, escriba "Demasiado!" en la `UnitPrice` cuadro de texto), se producirá una excepción. Un tutorial futuras examinará cómo agregar controles de validación a la interfaz de usuario de edición.
- Actualmente, *todos los* campos de producto que no son de solo lectura deben incluirse en el control GridView. Si deseáramos quitar un campo de GridView, diga `UnitPrice`, al actualizar los datos GridView no establecería el `UnitPrice` `UpdateParameters` valor, que cambiaría el registro de base de datos `UnitPrice` a una `NULL` valor. De forma similar, si un campo obligatorio, como `ProductName`, se quita de GridView, se producirá un error de la actualización con el mismo "*columna 'ProductName' no permite valores NULL*" excepción mencionadas anteriormente.
- El formato de interfaz edición deja mucho que se desee. El `UnitPrice` se muestra con cuatro decimales. Lo ideal es que la `CategoryID` y `SupplierID` valores contendría listas desplegables que se enumeran las categorías y los proveedores en el sistema.

Estas son todas las limitaciones que tenemos en vivo con para ahora, pero que se tratarán en tutoriales futuros.

## <a name="inserting-editing-and-deleting-data-with-the-detailsview"></a>Insertar, modificar y eliminar datos con DetailsView

Como hemos visto en tutoriales anteriores, el control DetailsView muestra un registro a la vez y, como GridView, permite editar y eliminar del registro mostrado actualmente. Tanto la experiencia del usuario final con la edición y eliminación de elementos de DetailsView y el flujo de trabajo desde el lado ASP.NET es idéntica de GridView. Donde DetailsView difiere de GridView es que también proporciona compatibilidad integrada con la inserción.

Para demostrar las capacidades de modificación de datos de GridView, empiece agregando un DetailsView a la `Basics.aspx` página por encima de GridView existente y enlazarla a ObjectDataSource existente a través de la etiqueta inteligente de DetailsView. Después, desactive out de DetailsView `Height` y `Width` propiedades y Active la opción Habilitar paginación de la etiqueta inteligente. Para habilitar la edición, inserción y eliminación de soporte técnico, solo tiene que activar las casillas de verificación Habilitar edición, Habilitar inserción y habilitar la eliminación de la etiqueta inteligente.


![Configurar el DetailsView al soporte técnico de edición, inserción y eliminación](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image41.png)

**Figura 17**: configurar DetailsView al soporte técnico de edición, inserción y eliminación


Como con el control GridView, adición de edición, inserción o eliminación de soporte técnico agrega un CommandField a DetailsView, tal como se muestra en la siguiente sintaxis declarativa:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample7.aspx)]

Tenga en cuenta que para el CommandField DetailsView aparece al final de la colección de columnas de forma predeterminada. Puesto que los campos de DetailsView se representan como filas, el CommandField aparece como una fila con Insert, editar y eliminar botones en la parte inferior de DetailsView.


[![Configurar el DetailsView al soporte técnico de edición, inserción y eliminación](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image43.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image42.png)

**Figura 18**: configurar DetailsView que admiten la edición, inserción y eliminación ([haga clic aquí para ver la imagen a tamaño completo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image44.png))


Al hacer clic en el botón Eliminar inicia la misma secuencia de eventos al igual que con GridView: un postback; seguido de DetailsView rellenar su ObjectDataSource `DeleteParameters` tomando como base la `DataKeyNames` valores; y finalizó con una llamada a su ObjectDataSource `Delete()` método, lo que elimina realmente el producto de la base de datos. Edición en DetailsView también funciona de forma idéntica a la de GridView.

Para insertar, se presenta al usuario final con un nuevo botón que, al hacer clic, representa el DetailsView en "modo de inserción". Con el modo"Insertar" el nuevo botón se reemplaza con botones Insertar y Cancelar y sólo esos BoundFields cuyo `InsertVisible` propiedad está establecida en `true` (valor predeterminado) se muestran. Los campos de datos identificados como campos de incremento automático, como `ProductID`, tienen sus [propiedad InsertVisible](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.insertvisible(VS.80).aspx) establecido en `false` al enlazar DetailsView con el origen de datos a través de la etiqueta inteligente.

Al enlazar un origen de datos con un DetailsView a través de la etiqueta inteligente, Visual Studio establece la `InsertVisible` propiedad `false` solamente para campos de incremento automático. Campos de solo lectura, como `CategoryName` y `SupplierName`, se mostrará en la interfaz de usuario de "modo de inserción", a menos que sus `InsertVisible` propiedad se establece explícitamente en `false`. Tómese un momento para establecer estos dos campos `InsertVisible` propiedades para `false`, ya sea a través de la sintaxis declarativa de DetailsView o editar campos de vínculo en la etiqueta inteligente. Figura 19 muestra cómo establecer el `InsertVisible` propiedades `false` haciendo clic en Editar campos de vínculo.


[![Northwind Traders ahora ofrece té Acme](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image46.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image45.png)

**Figura 19**: Té Northwind Traders ahora ofrece Acme ([haga clic aquí para ver la imagen a tamaño completo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image47.png))


Después de establecer el `InsertVisible` propiedades, vista la `Basics.aspx` página en un explorador y haga clic en el botón nuevo. Figura 20 muestra DetailsView al agregar una nuevo bebida, té Acme, en la línea de productos.


[![Northwind Traders ahora ofrece té Acme](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image49.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image48.png)

**Figura 20**: Té Northwind Traders ahora ofrece Acme ([haga clic aquí para ver la imagen a tamaño completo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image50.png))


Después de escribir los detalles de té Acme y haga clic en el botón Insertar, seguidamente, tiene lugar una devolución de datos y el nuevo registro se agrega a la `Products` tabla de base de datos. Puesto que este DetailsView muestra los productos en orden con el que se encuentran en la tabla de base de datos, debemos página hasta el último producto para ver el nuevo producto.


[![Detalles de té Acme](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image52.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image51.png)

**Figura 21**: detalles de té Acme ([haga clic aquí para ver la imagen a tamaño completo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image53.png))


> [!NOTE]
> El DetailsView [propiedad CurrentMode](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.currentmode(VS.80).aspx) indica la interfaz que se muestren y puede tener uno de los siguientes valores: `Edit`, `Insert`, o `ReadOnly`. El [propiedad DefaultMode](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode(VS.80).aspx) indica el modo de DetailsView vuelve a tras una modificación o insertar se ha completado y es útil para mostrar un DetailsView que está permanentemente en la edición o el modo de inserción.


El punto y haga clic en Insertar y modificar las capacidades de DetailsView sufren las mismas limitaciones que GridView: el usuario debe introducir existente `CategoryID` y `SupplierID` valores a través de un cuadro de texto; la falta de la interfaz ninguna lógica de validación; todos los campos de producto que no permiten `NULL` valores o no tiene valor predeterminado especificado en el nivel de base de datos de valor debe estar incluido en la interfaz de inserción y así sucesivamente.

Las técnicas examinaremos para extender y mejorar interfaz de edición de GridView en el futuro artículos pueden aplicarse a DetailsView del control de edición e insertar interfaces también.

## <a name="using-the-formview-for-a-more-flexible-data-modification-user-interface"></a>Uso de FormView para una interfaz de usuario de modificación de datos más Flexible

FormView ofrece compatibilidad integrada para insertar, modificar y eliminar datos, pero porque utiliza plantillas en lugar de campos no hay ningún lugar para agregar el BoundFields o el CommandField utilizado por los controles GridView y DetailsView para proporcionar los datos interfaz de modificación. En su lugar, esta interfaz, los controles Web para la recopilación de usuario cuando se agrega un nuevo elemento de entrada o editar una existente junto con el icono nuevo, editar, eliminar, insertar, actualizar y cancelar botones debe agregarse manualmente a las plantillas adecuadas. Afortunadamente, Visual Studio creará automáticamente la interfaz necesaria cuando se enlazan FormView a un origen de datos a través de la lista desplegable en la etiqueta inteligente.

Para ilustrar estas técnicas, empiece agregando un FormView a la `Basics.aspx` página y, de las etiquetas inteligentes de FormView, enlazarla a ObjectDataSource ya creado. Esto generará un `EditItemTemplate`, `InsertItemTemplate`, y `ItemTemplate` para FormView con controles de cuadro de texto Web para la recopilación de controles Web de botón y la entrada del usuario para el nuevo, editar, eliminar, insertar, actualizar y botones para cancelar. Del Además, el FormView `DataKeyNames` propiedad está establecida en el campo de clave principal (`ProductID`) del objeto devuelto por la ObjectDataSource. Por último, active la opción Habilitar paginación en la etiqueta inteligente de FormView.

La siguiente muestra el marcado declarativo para la FormView `ItemTemplate` después de que se ha enlazado la FormView a ObjectDataSource. De forma predeterminada, cada campo de producto de valor no booleano se enlaza a la `Text` propiedad de un control de etiqueta Web durante cada campo de valor booleano (`Discontinued`) se enlaza a la `Checked` propiedad de un control de casilla de verificación Web deshabilitado. En el orden de los botones de nuevo, editar y eliminar desencadenar cierto comportamiento FormView cuando hace clic en, es imprescindible que su `CommandName` valores se establecen en `New`, `Edit`, y `Delete`, respectivamente.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample8.aspx)]

La figura 22 muestra el FormView `ItemTemplate` cuando se ven a través de un explorador. Cada campo de producto se muestra con los botones nuevo, editar y eliminar en la parte inferior.


[![Cada campo de producto junto con nuevos enumera ItemTemplate FormView de forma predeterminada, editar y eliminar botones](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image55.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image54.png)

**Figura 22**: FormView de forma predeterminada el `ItemTemplate` enumera cada producto campo junto con nuevo, editar y eliminar botones ([haga clic aquí para ver la imagen a tamaño completo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image56.png))


Al igual que con el control GridView y DetailsView, haga clic en el botón Eliminar o cualquier Button, LinkButton o ImageButton cuyo `CommandName` propiedad está establecida en Delete hace una devolución de datos, rellena el ObjectDataSource `DeleteParameters` tomando como base el FormView `DataKeyNames`valor e invoca el ObjectDataSource `Delete()` método.

Cuando se hace clic en el botón Editar tiene lugar una devolución de datos y se vuelve a enlazar los datos a la `EditItemTemplate`, que es responsable de procesar la interfaz de edición. Esta interfaz incluye los controles Web para modificar los datos junto con los botones Actualizar y Cancelar. El valor predeterminado `EditItemTemplate` generados por Visual Studio contiene una etiqueta para los campos de incremento automático (`ProductID`), un cuadro de texto para cada campo de valor no booleano y una casilla para cada campo de valor booleano. Este comportamiento es muy similar a la BoundFields generado automáticamente en los controles GridView y DetailsView.

> [!NOTE]
> Uno de los problemas pequeño con la generación automática de FormView de la `EditItemTemplate` es que presenta TextBox Web controles para los campos que son de solo lectura, como `CategoryName` y `SupplierName`. Veremos cómo tenerlo en cuenta en breve.


El cuadro de texto se controla en el `EditItemTemplate` tienen sus `Text` propiedad enlazada al valor de su campo de datos correspondiente mediante *enlace de datos bidireccional*. Enlace de datos bidireccional, se indica mediante `<%# Bind("dataField") %>`, lleva a cabo tanto el enlace de datos cuando se enlazan datos a la plantilla y al rellenar los parámetros de ObjectDataSource para insertar o modificar registros. Es decir, cuando el usuario hace clic en el botón de edición de la `ItemTemplate`, el `Bind()` método devuelve el valor del campo de datos especificado. Cuando el usuario realiza los cambios y haga clic en la actualización, los valores envíe de vuelta que corresponden a los campos de datos especificados mediante `Bind()` se aplican a la ObjectDataSource `UpdateParameters`. Como alternativa, el enlace de datos unidireccional, denotado por `<%# Eval("dataField") %>`, solo recupera los valores de campo de datos cuando se enlazan datos a la plantilla y *no* devolver los valores introducidos por el usuario a los parámetros del origen de datos en el postback.

El siguiente marcado declarativo muestra la FormView `EditItemTemplate`. Tenga en cuenta que la `Bind()` método se utiliza en la sintaxis del enlace de datos y que los controles Web de Update y cancelar botón tienen sus `CommandName` propiedades establecidas en consecuencia.

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample9.aspx)]

Nuestro `EditItemTemplate`, en este punto, se producirá una excepción que se produce si se intenta usarlo. El problema es que la `CategoryName` y `SupplierName` campos se representan como controles de cuadro de texto Web en la `EditItemTemplate`. O bien, es necesario cambiar estos cuadros de texto a las etiquetas o quitarlas por completo. Vamos a simplemente eliminarlos completamente desde el `EditItemTemplate`.

Figura 23 FormView se muestra en un explorador después de que se ha hecho clic el botón Editar para Chai. Tenga en cuenta que la `SupplierName` y `CategoryName` campos que aparecen en la `ItemTemplate` ya no están presentes, tal y como se quitan de la `EditItemTemplate`. Cuando se hace clic en el botón de actualización FormView continúa a través de la misma secuencia de pasos que los controles GridView y DetailsView.


[![De forma predeterminada, la plantilla EditItemTemplate muestra cada campo Editable producto como un cuadro de texto o una casilla de verificación](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image58.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image57.png)

**Figura 23**: de forma predeterminada el `EditItemTemplate` muestra cada producto campo Editable como un cuadro de texto o una casilla de verificación ([haga clic aquí para ver la imagen a tamaño completo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image59.png))


Cuando se hace clic el botón Insertar en el FormView `ItemTemplate` tiene lugar una devolución de datos. Sin embargo, no hay datos se enlazan a FormView porque se está agregando un nuevo registro. El `InsertItemTemplate` interfaz incluye los controles Web para agregar un nuevo registro junto con los botones Insertar y Cancelar. El valor predeterminado `InsertItemTemplate` generados por Visual Studio contiene un cuadro de texto para cada campo de valor no booleano y una casilla para cada campo de valor booleano, similar a la autogenerada `EditItemTemplate`de la interfaz. Los controles de cuadro de texto tienen sus `Text` propiedad enlazada con el valor de su campo de datos correspondiente con el enlace de datos bidireccional.

El siguiente marcado declarativo muestra la FormView `InsertItemTemplate`. Tenga en cuenta que la `Bind()` método se utiliza en la sintaxis del enlace de datos y que los controles Web de botón Cancelar y de inserción tienen sus `CommandName` propiedades establecidas en consecuencia.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample10.aspx)]

Hay un problema con la generación automática de FormView de la `InsertItemTemplate`. En concreto, se crean los controles de cuadro de texto Web incluso para los campos que son de solo lectura, como `CategoryName` y `SupplierName`. Al igual que con la `EditItemTemplate`, tenemos que quitar estos cuadros de texto de la `InsertItemTemplate`.

Figura 24 muestra FormView en un explorador cuando se agrega un nuevo producto, Acme Coffee. Tenga en cuenta que la `SupplierName` y `CategoryName` campos que aparecen en la `ItemTemplate` ya no están presentes, tal y como se retiró. Cuando se presiona el botón de insertar la lleva a cabo a través de la misma secuencia de pasos que el control DetailsView FormView, agregar un nuevo registro a la `Products` tabla. Figura 25 muestra detalles del producto de café Acme en FormView después de que se ha insertado.


[![El InsertItemTemplate dicta insertar interfaz de FormView](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image61.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image60.png)

**Figura 24**: el `InsertItemTemplate` dicta la interfaz de inserción de FormView ([haga clic aquí para ver la imagen a tamaño completo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image62.png))


[![Los detalles para el nuevo producto, Coffee Acme, se muestran en FormView](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image64.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image63.png)

**Figura 25**: los detalles para el nuevo producto, Coffee Acme, se muestran en FormView ([haga clic aquí para ver la imagen a tamaño completo](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image65.png))


Al separar los solo lectura, editar e insertar interfaces en tres plantillas independientes, FormView permite un mayor grado de control sobre estas interfaces que los DetailsView y GridView.

> [!NOTE]
> Al igual que el DetailsView, FormView `CurrentMode` propiedad indica la interfaz que se muestren y su `DefaultMode` propiedad indica el modo de la devuelve FormView a después de una operación de edición o inserción se ha completado.


## <a name="summary"></a>Resumen

En este tutorial se examinan los conceptos básicos de insertar, modificar y eliminar datos mediante el control GridView, DetailsView y FormView. Las tres de estos controles proporcionan un cierto nivel de capacidades de modificación de datos integrados que puede utilizarse sin escribir una sola línea de código en la página ASP.NET gracias a los controles Web de datos y el ObjectDataSource. Sin embargo, los más sencillos de punto y haga clic en representación técnicas un bastante frail y la interfaz de usuario de modificación de datos de naïve. Para proporcionar la validación, insertar valores mediante programación, administrar correctamente las excepciones, personalizar la interfaz de usuario y así sucesivamente, necesitaremos confiar en lugar de una serie de técnicas que se describen en la siguientes varios tutoriales.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Siguiente](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)
