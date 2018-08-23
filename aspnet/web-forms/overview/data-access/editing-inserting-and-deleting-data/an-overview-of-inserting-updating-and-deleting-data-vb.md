---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb
title: Información general de insertar, actualizar y eliminar los datos (VB) | Microsoft Docs
author: rick-anderson
description: En este tutorial veremos cómo asignar Insert() de un origen ObjectDataSource, Update(), y las clases Delete() métodos a los métodos de BLL, así como cómo configu...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 35b40b8f-2ca8-4ab3-9c19-f361a91a3647
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 55fab6bb7a1041a14f8734a0d2ae1238b3801149
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836909"
---
<a name="an-overview-of-inserting-updating-and-deleting-data-vb"></a>Información general de insertar, actualizar y eliminar los datos (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_16_VB.exe) o [descargar PDF](an-overview-of-inserting-updating-and-deleting-data-vb/_static/datatutorial16vb1.pdf)

> En este tutorial, veremos cómo asignar Insert() de un origen ObjectDataSource, Update(), y las clases Delete() métodos a los métodos de BLL, además de cómo configurar los controles GridView, DetailsView y FormView para proporcionar capacidades de modificación de datos.


## <a name="introduction"></a>Introducción

A través de los diversos tutoriales anteriores hemos examinado cómo mostrar datos en una página ASP.NET mediante los controles GridView, DetailsView y FormView. Estos controles simplemente funcionan con datos proporcionados a ellos. Normalmente, estos controles tener acceso a datos mediante el uso de un control de origen de datos, como ObjectDataSource. Hemos visto cómo ObjectDataSource actúa como un proxy entre la página ASP.NET y los datos subyacentes. Cuando se necesita un control GridView para mostrar los datos, invoca su ObjectDataSource `Select()` método, que a su vez invoca un método de la capa de lógica empresarial (BLL), que llama a un método en adecuado datos acceso de la capa (DAL) TableAdapter, que a su vez envía un `SELECT` consulta a la base de datos Northwind.

Recuerde que cuando creamos los TableAdapters en la capa DAL en [nuestro primer tutorial](../introduction/creating-a-data-access-layer-cs.md), Visual Studio agrega automáticamente los métodos para insertar, actualizar, y tabla de base de datos de eliminación de datos subyacente. Además, en [crear una capa de lógica empresarial](../introduction/creating-a-business-logic-layer-vb.md) diseñamos métodos en el nivel de lógica empresarial que invocó a estos métodos DAL de modificación de datos.

Además su `Select()` método, también tiene el origen ObjectDataSource `Insert()`, `Update()`, y `Delete()` métodos. Al igual que el `Select()` método, estos tres métodos pueden asignarse a métodos en un objeto subyacente. Cuando se configura para insertar, actualizar o eliminar datos, los controles GridView, DetailsView y FormView proporcionan una interfaz de usuario para modificar los datos subyacentes. Esta interfaz de usuario llama a la `Insert()`, `Update()`, y `Delete()` métodos (consulte la figura 1) asociados a los métodos del origen ObjectDataSource, que, a continuación, invocan el objeto subyacente.


[![De ObjectDataSource Insert(), actualizar() y métodos Delete() actúan como un Proxy en el nivel de lógica empresarial](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image2.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image1.png)

**Figura 1**: el origen ObjectDataSource `Insert()`, `Update()`, y `Delete()` métodos actuar como un Proxy en la capa BLL ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image3.png))


En este tutorial veremos cómo asignar el ObjectDataSource `Insert()`, `Update()`, y `Delete()` métodos a los métodos de clases en el BLL, así como cómo configurar los controles GridView, DetailsView y FormView para proporcionar la modificación de datos capacidades.

## <a name="step-1-creating-the-insert-update-and-delete-tutorials-web-pages"></a>Paso 1: Crear el Insert, Update y las páginas Web de tutoriales de Delete

Antes de empezar a explorar cómo insertar, actualizar y eliminar datos, en primer lugar dedique un momento para crear las páginas ASP.NET en nuestro proyecto de sitio Web que necesitaremos para este tutorial y las diversas siguientes. Empiece por agregar una nueva carpeta denominada `EditInsertDelete`. A continuación, agregue las siguientes páginas ASP.NET a la carpeta y asegúrese de asociar cada página con el `Site.master` página maestra:

- `Default.aspx`
- `Basics.aspx`
- `DataModificationEvents.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`


![Agregar las páginas ASP.NET para los tutoriales relacionados con la modificación de datos](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image4.png)

**Figura 2**: agregar las páginas ASP.NET para los tutoriales relacionados con la modificación de datos


Al igual que en las demás carpetas `Default.aspx` en el `EditInsertDelete` carpeta mostrará una lista de los tutoriales en su sección. Recuerde que el `SectionLevelTutorialListing.ascx` Control de usuario proporciona esta funcionalidad. Por lo tanto, agrega este Control de usuario a `Default.aspx` arrastrándolo desde el Explorador de soluciones en la vista de diseño de la página.


[![Agregar el Control de usuario SectionLevelTutorialListing.ascx a Default.aspx](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image6.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image5.png)

**Figura 3**: agregar el `SectionLevelTutorialListing.ascx` Control de usuario `Default.aspx` ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image7.png))


Por último, agregue las páginas como entradas para el `Web.sitemap` archivo. En concreto, agregue el siguiente marcado después Customized Formatting `<siteMapNode>`:


[!code-xml[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample1.xml)]

Después de actualizar `Web.sitemap`, dedique un momento para ver el sitio Web de tutoriales a través de un explorador. El menú de la izquierda ahora incluye elementos para la edición, inserción y eliminación de tutoriales.


![El mapa del sitio incluye ahora entradas para la edición, inserción y eliminación de tutoriales](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image8.png)

**Figura 4**: el mapa del sitio incluye ahora entradas para la edición, inserción y eliminación de tutoriales


## <a name="step-2-adding-and-configuring-the-objectdatasource-control"></a>Paso 2: Agregar y configurar el Control ObjectDataSource

Desde la GridView, DetailsView y FormView cada uno de ellos difieren en sus capacidades de modificación de datos y el diseño, vamos a examinar cada uno de ellos individualmente. En lugar de tener cada control mediante su propio origen ObjectDataSource, sin embargo, creemos un único origen ObjectDataSource que pueden compartir todos los ejemplos de control de tres.

Abra el `Basics.aspx` página, arrastre un origen ObjectDataSource desde el cuadro de herramientas hasta el diseñador y haga clic en el vínculo Configurar origen de datos de su etiqueta inteligente. Puesto que el `ProductsBLL` es la única clase de nivel de lógica empresarial que proporciona la edición, inserción y eliminación de los métodos, configuran el origen ObjectDataSource para usar esta clase.


[![Configurar el origen ObjectDataSource para usar la clase ProductsBLL](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image10.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image9.png)

**Figura 5**: configurar el origen ObjectDataSource que se usarán el `ProductsBLL` clase ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image11.png))


En la siguiente pantalla podemos especificar qué métodos de la `ProductsBLL` clase se asignan a de ObjectDataSource `Select()`, `Insert()`, `Update()`, y `Delete()` , seleccione la ficha apropiada y elija el método en la lista desplegable. Figura 6, que debería resultar familiar en este momento, se asigna el ObjectDataSource `Select()` método a la `ProductsBLL` la clase `GetProducts()` método. El `Insert()`, `Update()`, y `Delete()` métodos se pueden configurar seleccionando la ficha correspondiente en la lista en la parte superior.


[![Dispone de ObjectDataSource devuelven todos los productos](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image13.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image12.png)

**Figura 6**: tiene el origen ObjectDataSource devuelven todos los de los productos ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image14.png))


Pestañas de las figuras 7, 8 y 9 muestran el ObjectDataSource UPDATE, INSERT y DELETE. Configurar estas pestañas para que la `Insert()`, `Update()`, y `Delete()` métodos invocan la `ProductsBLL` la clase `UpdateProduct`, `AddProduct`, y `DeleteProduct` métodos, respectivamente.


[![Map (método) de ObjectDataSource Update() al método UpdateProduct por separado de la clase ProductBLL](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image16.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image15.png)

**Figura 7**: asignar el ObjectDataSource `Update()` método a la `ProductBLL` la clase `UpdateProduct` método ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image17.png))


[![Map (método) de ObjectDataSource Insert() al método de la clase ProductBLL AddProduct](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image19.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image18.png)

**Figura 8**: asignar de ObjectDataSource `Insert()` método a la `ProductBLL` Add de la clase `Product` método ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image20.png))


[![Map (método) de ObjectDataSource Delete() al método de la clase ProductBLL DeleteProduct](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image22.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image21.png)

**Figura 9**: asignar el ObjectDataSource `Delete()` método a la `ProductBLL` la clase `DeleteProduct` método ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image23.png))


Es posible que haya observado que las listas desplegables en las pestañas UPDATE, INSERT y DELETE ya tenían estos métodos seleccionados. Gracias a nuestro uso de la `DataObjectMethodAttribute` que decora los métodos de `ProducstBLL`. Por ejemplo, el método DeleteProduct tiene la firma siguiente:


[!code-vb[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample2.vb)]

El `DataObjectMethodAttribute` atributo indica la finalidad de cada método, ya sea para seleccionar, insertar, actualizar, o eliminar y si es el valor predeterminado. Si omite estos atributos al crear las clases BLL, debe seleccionar manualmente los métodos de la actualización, INSERTARÁ y eliminar las fichas.

Después de asegurarse de que el correspondiente `ProductsBLL` métodos se asignan a de ObjectDataSource `Insert()`, `Update()`, y `Delete()` métodos, haga clic en Finalizar para completar el asistente.

## <a name="examining-the-objectdatasources-markup"></a>Examinar el marcado de ObjectDataSource

Después de configurar el origen ObjectDataSource mediante el asistente, vaya a la vista del origen para examinar el marcado declarativo generado. El `<asp:ObjectDataSource>` etiqueta Especifica el objeto subyacente y los métodos para invocar. Además, existen `DeleteParameters`, `UpdateParameters`, y `InsertParameters` que se asignan a los parámetros de entrada para el `ProductsBLL` la clase `AddProduct`, `UpdateProduct`, y `DeleteProduct` métodos:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample3.aspx)]

ObjectDataSource incluye un parámetro para cada uno de los parámetros de entrada para sus métodos asociados, como una lista de `SelectParameter` s está presente cuando se configura el origen ObjectDataSource para llamar a un método de selección que espera un parámetro de entrada (por ejemplo, `GetProductsByCategoryID(categoryID)`). Como veremos en breve, los valores para estos `DeleteParameters`, `UpdateParameters`, y `InsertParameters` se establecen automáticamente por el control GridView, DetailsView y FormView antes de invocar el ObjectDataSource `Insert()`, `Update()`, o `Delete()` método. Estos valores también pueden establecerse mediante programación según sea necesario, como se explicará en un futuro tutorial.

Un efecto de utilizar el Asistente para configurar al origen ObjectDataSource es que Visual Studio establece la [propiedad OldValuesParameterFormatString](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.oldvaluesparameterformatstring(VS.80).aspx) a `original_{0}`. Este valor de propiedad se usa para incluir los valores originales de los datos que se está editando y es útil en dos escenarios:

- Si, al editar un registro, los usuarios pueden cambiar el valor de clave principal. En este caso, se deben proporcionar el nuevo valor de clave principal y el valor de clave principal original para que pueda encontrarse el registro con el valor de clave principal original y se dispone de su valor actualiza en consecuencia.
- Cuando se utiliza simultaneidad optimista. Optimista de simultaneidad es una técnica para asegurarse de que dos usuarios simultáneos no sobrescriben los cambios del otro y es el tema para ver un tutorial futuras.

El `OldValuesParameterFormatString` propiedad indica el nombre de los parámetros de entrada en la actualización del objeto subyacente y los métodos de eliminación para los valores originales. Trataremos esta propiedad y su finalidad en mayor detalle cuando se explora la simultaneidad optimista. Activarla ahora, sin embargo, dado que los métodos de nuestro BLL no espera que los valores originales y, por tanto, es importante que se quite esta propiedad. Dejar la `OldValuesParameterFormatString` propiedad establecida en algo distinto del predeterminado (`{0}`) se producirá un error cuando un control Web de datos intenta invocar el ObjectDataSource `Update()` o `Delete()` métodos ya que el origen ObjectDataSource intenta pasar tanto en el `UpdateParameters` o `DeleteParameters` especificado, así como los parámetros de valor original.

Si esto no está muy claro en ese momento, no se preocupe, examinaremos esta propiedad y su utilidad en un futuro tutorial. Por ahora, solo Asegúrese de quitar esta declaración de propiedad completamente de la sintaxis declarativa o establezca el valor en el valor predeterminado ({0}).

> [!NOTE]
> Si simplemente borre el `OldValuesParameterFormatString` seguirá existiendo en la sintaxis declarativa del valor de propiedad de la ventana de propiedades en la vista Diseño, la propiedad, pero se establece en una cadena vacía. Esto, Lamentablemente, todavía producirá el mismo problema que se ha explicado anteriormente. Por lo tanto, quite la propiedad completamente de la sintaxis declarativa o, en la ventana Propiedades, establezca el valor en el valor predeterminado, `{0}`.


## <a name="step-3-adding-a-data-web-control-and-configuring-it-for-data-modification"></a>Paso 3: Agregar un Control de datos Web y configurarlo para la modificación de datos

Una vez que se ha agregado a la página y configurado el origen ObjectDataSource, estamos listos para agregar controles Web de datos a la página para mostrar los datos y proporcionan un medio para el usuario final modificarlo. Veremos la GridView, DetailsView y FormView por separado, como estos controles Web de datos difieren en sus capacidades de modificación de datos y la configuración.

Como veremos en el resto de este artículo, agregar muy básico de edición, inserción y eliminación de soporte técnico a través del control GridView, DetailsView, controles y FormView es realmente tan sencillo como comprobar un par de casillas de verificación. Hay muchos matices y casos extremos en el mundo real que hacen que proporciona dicha funcionalidad más compleja que simplemente elija y haga clic en. En este tutorial, sin embargo, se centra exclusivamente en demostrar las capacidades de modificación de datos simple. Tutoriales futuros examinará los problemas que sin duda surgirán en una configuración del mundo real.

## <a name="deleting-data-from-the-gridview"></a>Eliminar datos de GridView

Iniciar, arrastre un control GridView desde el cuadro de herramientas hasta el diseñador. A continuación, enlazar el origen ObjectDataSource en GridView, selecciónelo en la lista desplegable en la etiqueta inteligente de GridView. En este punto marcado declarativo de GridView será:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample4.aspx)]

Enlazar el control GridView a ObjectDataSource mediante su etiqueta inteligente tiene dos ventajas:

- BoundFields y CheckBoxFields se crean automáticamente para cada uno de los campos devueltos por el origen ObjectDataSource. Además, las BoundField del CampoCasillaVerificación propiedades y se establecen según los metadatos del campo subyacente. Por ejemplo, el `ProductID`, `CategoryName`, y `SupplierName` campos están marcados como de solo lectura en el `ProductsDataTable` y, por tanto, no debería ser actualizable cuando se edita. Para dar cabida a este, estas BoundFields [propiedades ReadOnly](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.readonly(VS.80).aspx) se establecen en `True`.
- El [propiedad DataKeyNames](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames(VS.80).aspx) se asigna a los campos de clave principales del objeto subyacente. Esto es esencial cuando ese único mediante el control GridView para modificar o eliminar datos, puesto que esta propiedad indica el campo (o conjunto de campos) identifica cada registro. Para obtener más información sobre la `DataKeyNames` propiedad, vuelva a consultar el [Master/Detail mediante un GridView maestro seleccionable con un DetailView de detalles](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) tutorial.

Mientras el control GridView se puede enlazar a ObjectDataSource a través de la ventana Propiedades o la sintaxis declarativa, si lo hace, requiere que se agreguen manualmente el BoundField adecuado y `DataKeyNames` marcado.

El control GridView proporciona compatibilidad integrada para la edición de nivel de fila y la eliminación. Configurar un control GridView para admitir la eliminación, agrega una columna de botones de eliminación. Cuando el usuario final hace clic en el botón Eliminar para una fila determinada, una devolución de datos que habrá trastornos y GridView realiza los pasos siguientes:

1. El ObjectDataSource `DeleteParameters` se asignan valores
2. El ObjectDataSource `Delete()` se invoca el método, al eliminar el registro especificado
3. GridView propio Reenlaces a ObjectDataSource invocando su `Select()` (método)

Los valores asignados a la `DeleteParameters` son los valores de la `DataKeyNames` campos para la fila cuyo botón Delete se hizo clic. Por lo tanto, es fundamental que un GridView `DataKeyNames` propiedad se establezca correctamente. Si está presente, el `DeleteParameters` se asignará un valor de `Nothing` en el paso 1, lo que a su vez no producirá los registros eliminados en el paso 2.

> [!NOTE]
> El `DataKeys` colección se almacena en el estado del control GridView s, lo que significa que el `DataKeys` valores se conservarán en la devolución de datos incluso si se ha deshabilitado el estado de vista GridView s. Sin embargo, es muy importante que el estado de vista sigue estando habilitado para GridView que admiten editen o eliminen (comportamiento predeterminado). Si establece la s GridView `EnableViewState` propiedad `false`, la edición y eliminación de comportamiento funcionará correctamente para un solo usuario, pero si hay una eliminación de datos de usuarios simultáneos, existe la posibilidad de que estos usuarios simultáneos, es posible que accidentalmente eliminar o editar los registros que t no ha previsto. Vea Mi entrada de blog, [advertencia: problema de simultaneidad con ASP.NET 2.0 GridView y DetailsView/FormViews esa compatibilidad de edición o eliminación y cuyo estado de vista está deshabilitado](http://scottonwriting.net/sowblog/posts/10054.aspx), para obtener más información.


Esta advertencia también se aplica a DetailsViews y FormViews.

Para agregar capacidades de eliminación en un control GridView, vaya a su etiqueta inteligente y Active la casilla Habilitar eliminación.


![Comprobar la activación de la eliminación de la casilla de verificación](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image24.png)

**Figura 10**: comprobar la activación de la eliminación de la casilla de verificación


Marque la casilla Habilitar eliminación de la etiqueta inteligente, agrega un CommandField en GridView. El CommandField representa una columna de GridView con botones para llevar a cabo una o varias de las siguientes tareas: Si selecciona un registro, editar un registro y eliminar un registro. Hemos visto anteriormente el CommandField en acción con la selección de registros en el [Master/Detail mediante un GridView maestro seleccionable con un DetailView de detalles](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) tutorial.

El CommandField contiene un número de `ShowXButton` propiedades que indican qué conjunto de botones que se muestra en el CommandField. Activando la casilla Habilitar eliminación un CommandField cuyo `ShowDeleteButton` propiedad es `True` se ha agregado a la colección de columnas de GridView.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample5.aspx)]

En este momento, créase o no, hemos terminado con la incorporación de compatibilidad de eliminación en el control GridView! Como se muestra en la figura 11, al visitar esta página a través de un explorador, una columna de botones de Delete está presente.


[![El CommandField agrega una columna de botones de eliminación](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image26.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image25.png)

**Figura 11**: el CommandField agrega una columna de botones Eliminar ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image27.png))


Si ha sido creando este tutorial desde el principio por su cuenta, al probar esta página haciendo clic en el botón Eliminar, producirá una excepción. Siga leyendo para obtener información sobre por qué se han producido estas excepciones y cómo corregirlos.

> [!NOTE]
> Si sigues mediante la descarga que acompaña a este tutorial, estos problemas ya se hayan contabilizado para. Sin embargo, lo animo a que lea los detalles siguientes para ayudar a identificar los problemas que pueden surgir y soluciones alternativas adecuadas.


Si, al intentar eliminar un producto, se produce una excepción cuyo mensaje es similar a "*ObjectDataSource 'ObjectDataSource1' no pudo encontrar un método no genérico 'DeleteProduct' que tiene parámetros: productID, original\_ ProductID*, "es probable que olvidamos eliminar la `OldValuesParameterFormatString` propiedad de ObjectDataSource. Con el `OldValuesParameterFormatString` especifica la propiedad, el origen ObjectDataSource intenta pasar tanto en `productID` y `original_ProductID` parámetros de entrada al `DeleteProduct` método. `DeleteProduct`, sin embargo, solo acepta un único parámetro de entrada, por lo tanto, la excepción. Quitar el `OldValuesParameterFormatString` propiedad (o si se establece en `{0}`) indica a ObjectDataSource para no intenta pasar el parámetro de entrada original.


[![Asegúrese de que se ha borrado la propiedad OldValuesParameterFormatString Out](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image29.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image28.png)

**Figura 12**: asegúrese de que el `OldValuesParameterFormatString` propiedad ha sido desactivada Out ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image30.png))


Incluso si se hubiera quitado el `OldValuesParameterFormatString` propiedad, todavía obtendrá una excepción al intentar eliminar un producto con el mensaje: "*la instrucción DELETE en conflicto con la restricción de referencia ' FK\_orden\_detalles \_Los productos*. " La base de datos Northwind contiene una restricción foreign key entre el `Order Details` y `Products` tabla, lo que significa que no se puede eliminar un producto del sistema si hay uno o más registros para él en el `Order Details` tabla. Puesto que todos los productos en la base de datos Northwind no tiene al menos un registro en `Order Details`, no se puede eliminar todos los productos hasta que se eliminación en primer lugar los registros de detalles de pedido asociados del producto.


[![Una restricción Foreign Key prohíbe la eliminación de productos](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image32.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image31.png)

**Figura 13**: una restricción Foreign Key prohíbe la eliminación de productos ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image33.png))


En este tutorial, vamos a eliminar todos los registros desde el `Order Details` tabla. En una aplicación real necesitaríamos cualquiera:

- Tiene otra pantalla para administrar la información de detalles de pedido
- Aumentar el `DeleteProduct` para incluir la lógica para eliminar los detalles del pedido del producto especificado (método)
- Modificar la consulta SQL utilizada por el TableAdapter para incluir la eliminación de los detalles del pedido del producto especificado

Vamos a eliminar todos los registros desde el `Order Details` tabla para evitar la restricción foreign key. Vaya al explorador de servidores en Visual Studio, haga doble clic en el `NORTHWND.MDF` nodo y elija la nueva consulta. A continuación, en la ventana de consulta, ejecute la siguiente instrucción SQL: `DELETE FROM [Order Details]`


[![Eliminar todos los registros de la tabla Order Details](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image35.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image34.png)

**Figura 14**: eliminar todos los registros de la `Order Details` tabla ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image36.png))


Después de borrarlos el `Order Details` al hacer clic en el botón Eliminar de tabla, eliminará el producto sin errores. Si al hacer clic en el botón de eliminación no elimina el producto, Active esta casilla para asegurarse de que la GridView `DataKeyNames` propiedad está establecida en el campo de clave principal (`ProductID`).

> [!NOTE]
> Al hacer clic en el botón Eliminar que habrá trastornos una devolución de datos y se elimina el registro. Esto puede ser peligroso, ya que resulta fácil accidentalmente, haga clic en el botón de eliminar la fila incorrecta. En un futuro tutorial veremos cómo agregar una confirmación del cliente al eliminar un registro.


## <a name="editing-data-with-the-gridview"></a>Edición de datos con el control GridView

Junto con la eliminación, el control GridView también proporciona compatibilidad integrada de edición de nivel de fila. Configurar un control GridView para admitir la edición, agrega una columna de botones de edición. Desde la perspectiva del usuario final, al hacer clic en las causas de botón de edición de una fila que esa fila se convierten en modificable, convirtiendo las celdas en los cuadros de texto que contiene los valores existentes y reemplazar los botones de cancelación y el botón Editar con la actualización. Después de realizar los cambios deseados, el usuario final puede haga clic en el botón Actualizar para confirmar los cambios o en el botón Cancelar para descartarlos. En cualquier caso, tras hacer clic en actualizar o cancelar la GridView vuelve a su estado de edición previamente.

Desde nuestro punto de vista como el desarrollador de páginas, cuando el usuario final hace clic en el botón Editar para una fila determinada, una devolución de datos que habrá trastornos y GridView realiza los pasos siguientes:

1. La GridView `EditItemIndex` propiedad se asigna al índice de la fila cuyo botón Edición se hizo clic
2. GridView propio Reenlaces a ObjectDataSource invocando su `Select()` (método)
3. El índice de fila que coincide con el `EditItemIndex` se representa en modo de edición"." En este modo, el botón de edición se reemplaza por los botones Actualizar y Cancelar y BoundFields cuyo `ReadOnly` propiedades son False (valor predeterminado) se representan como controles de cuadro de texto Web cuya `Text` propiedades se asignan a los valores de los campos de datos.

En este momento, el marcado se devuelve al explorador, que permite al usuario final realizar cambios en los datos de la fila. Cuando el usuario hace clic en el botón de actualización, se produce un postback y GridView realiza los pasos siguientes:

1. El ObjectDataSource `UpdateParameters` valores se asignan los valores especificados por el usuario final en la interfaz de edición de GridView
2. El ObjectDataSource `Update()` se invoca el método, actualizando el registro especificado
3. GridView propio Reenlaces a ObjectDataSource invocando su `Select()` (método)

Los valores de clave principales asignados a la `UpdateParameters` en el paso 1 proceden de los valores especificados en el `DataKeyNames` propiedad, mientras que los valores de clave no principal proceden de texto en los controles Web de cuadro de texto para la fila editada. Como con la eliminación, es fundamental que un GridView `DataKeyNames` propiedad se establezca correctamente. Si está presente, el `UpdateParameters` valor de clave principal se asignará un valor de `Nothing` en el paso 1, que a su vez no dará como resultado de los registros actualizados en el paso 2.

Funcionalidad de edición puede activarse con sólo marcar la casilla Habilitar edición de etiquetas inteligentes de GridView.


![Comprobar la activación de la casilla de verificación de edición](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image37.png)

**Figura 15**: comprobar la activación de la casilla de verificación de edición


Comprobación de la casilla de verificación Habilitar edición agregará un CommandField (si es necesario) y establezca su `ShowEditButton` propiedad `True`. Como hemos visto anteriormente, el CommandField contiene un número de `ShowXButton` propiedades que indican qué conjunto de botones que se muestra en el CommandField. Marque la casilla Habilitar edición agrega la `ShowEditButton` propiedad a la CommandField existente:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample6.aspx)]

Eso es todo para agregar compatibilidad insuficiente con la edición. Como se muestra en Figure16, la interfaz de edición es bastante tosco cada BoundField cuyo `ReadOnly` propiedad está establecida en `False` (valor predeterminado) se representa como un cuadro de texto. Esto incluye campos como `CategoryID` y `SupplierID`, que son claves con otras tablas.


[![Haga clic en botón de edición de s Chai muestra la fila en modo de edición](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image39.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image38.png)

**Figura 16**: s Chai haciendo clic en botón Editar muestra la fila en modo de edición ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image40.png))


Además de pedir a los usuarios modificar directamente los valores de clave externa, le falta la interfaz de la interfaz de edición de las maneras siguientes:

- Si el usuario escribe un `CategoryID` o `SupplierID` que no existe en la base de datos, el `UPDATE` infringen una restricción foreign key, provocando una excepción.
- La interfaz de edición no incluye ninguna validación. Si no proporciona un valor requerido (como `ProductName`), o escriba un valor de cadena donde se espera un valor numérico (como escribir "Demasiado!" en la `UnitPrice` cuadro de texto), se producirá una excepción. Un futuro tutorial examinará cómo agregar controles de validación a la interfaz de usuario de edición.
- Actualmente, *todas* campos de producto que no son de solo lectura deben incluirse en el control GridView. Si tuviéramos que quitar un campo de GridView, por ejemplo `UnitPrice`, al actualizar los datos del control GridView no establecería el `UnitPrice` `UpdateParameters` valor, lo que provocaría cambios en el registro de base de datos `UnitPrice` a un `NULL` valor. De forma similar, si un campo obligatorio, como `ProductName`, se quita de GridView, se producirá un error de la actualización con el mismo "*columna 'ProductName' no permite valores NULL*" excepción mencionados anteriormente.
- El formato de interfaz edición deja mucho que desear. El `UnitPrice` se muestra con cuatro decimales. Lo ideal es que el `CategoryID` y `SupplierID` valores contendría DropDownLists que enumeran las categorías y proveedores en el sistema.

Estos son todos los puntos débiles que tenemos en directo con para ahora, pero que se tratará en tutoriales futuros.

## <a name="inserting-editing-and-deleting-data-with-the-detailsview"></a>Insertar, editar y eliminar datos con DetailsView

Como hemos visto en los tutoriales anteriores, el control DetailsView muestra un registro a la vez y, como GridView, permite editar y eliminar del registro mostrado actualmente. Tanto la experiencia del usuario final con la edición y eliminación de elementos desde el flujo de trabajo desde el lado ASP.NET y DetailsView es idéntica del control GridView. Donde DetailsView difiere del control GridView es que también proporciona compatibilidad integrada con la inserción.

Para demostrar las capacidades de modificación de datos del control GridView, empiece agregando un DetailsView en la `Basics.aspx` página por encima del control GridView existente y enlazarlo a ObjectDataSource existente a través de la etiqueta inteligente de DetailsView. A continuación, limpiar el DetailsView `Height` y `Width` propiedades y marque la opción Habilitar paginación de la etiqueta inteligente. Para habilitar la edición, inserción y eliminación de soporte técnico, simplemente marque las casillas de verificación Habilitar edición, Habilitar inserción y Habilitar eliminación en la etiqueta inteligente.


![Configurar DetailsView al soporte técnico de edición, inserción y eliminación](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image41.png)

**Figura 17**: configurar DetailsView al soporte técnico de edición, inserción y eliminación


Como con el control GridView, adición de edición, inserción o eliminación de soporte técnico agrega un CommandField a DetailsView, como se muestra en la siguiente sintaxis declarativa:


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample7.aspx)]

Tenga en cuenta que para DetailsView el CommandField aparece al final de la colección de columnas de forma predeterminada. Puesto que los campos de DetailsView se representan como filas, el CommandField aparece como una fila con Insert, editar y eliminar botones en la parte inferior de DetailsView.


[![Configurar DetailsView al soporte técnico de edición, inserción y eliminación](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image43.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image42.png)

**Figura 18**: configurar DetailsView a soporte técnico de edición, inserción y eliminación ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image44.png))


Al hacer clic en el botón de eliminación inicia la misma secuencia de eventos al igual que con el control GridView: un postback; seguido de DetailsView rellenar su ObjectDataSource `DeleteParameters` según la `DataKeyNames` valores; y completado con una llamada de su ObjectDataSource `Delete()` método, que quita realmente el producto de la base de datos. Edición en DetailsView también funciona de forma idéntica a la del control GridView.

Para insertar, el usuario final se le presentará un nuevo botón que, al hacer clic, representa el DetailsView en "modo de inserción". Con el modo"Insertar" el nuevo botón se sustituye por botones Insertar y Cancelar y sólo esas BoundFields cuyo `InsertVisible` propiedad está establecida en `True` (valor predeterminado) se muestran. Los campos de datos identificados como campos de incremento automático, como `ProductID`, tienen sus [propiedad InsertVisible](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.insertvisible(VS.80).aspx) establecido en `False` encuadernar DetailsView al origen de datos a través de la etiqueta inteligente.

Al enlazar un origen de datos a un DetailsView a través de la etiqueta inteligente, Visual Studio establece la `InsertVisible` propiedad `False` solamente para campos de incremento automático. Campos de solo lectura, como `CategoryName` y `SupplierName`, se mostrará en la interfaz de usuario de "modo de inserción", a menos que sus `InsertVisible` propiedad se establece explícitamente en `False`. Dedique un momento para establecer estos dos campos `InsertVisible` propiedades a `False`, ya sea a través de la sintaxis declarativa de DetailsView o editar campos vincular en la etiqueta inteligente. Figura 19 muestra cómo establecer el `InsertVisible` propiedades a `False` haciendo clic en Editar campos vincular.


[![Northwind Traders ahora ofrece té Acme](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image46.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image45.png)

**Figura 19**: Northwind Traders ahora ofrece Acme té ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image47.png))


Después de establecer el `InsertVisible` propiedades, ver la `Basics.aspx` página en un explorador y haga clic en el botón nuevo. Figura 20 muestra DetailsView al agregar un nuevo bebidas, té Acme, a nuestra línea de productos.


[![Northwind Traders ahora ofrece té Acme](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image49.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image48.png)

**Figura 20**: Northwind Traders ahora ofrece Acme té ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image50.png))


Después de escribir los detalles de Acme té y hacer clic en el botón Insertar, que habrá trastornos una devolución de datos y el nuevo registro se agrega a la `Products` tabla de base de datos. Puesto que este DetailsView enumera los productos en orden con el que se encuentran en la tabla de base de datos, nos debemos página hasta el último producto para ver el nuevo producto.


[![Detalles de té Acme](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image52.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image51.png)

**Figura 21**: detalles de té Acme ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image53.png))


> [!NOTE]
> El DetailsView [propiedad CurrentMode](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.currentmode(VS.80).aspx) indica la interfaz que se muestra y puede tener uno de los siguientes valores: `Edit`, `Insert`, o `ReadOnly`. El [propiedad DefaultMode](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode(VS.80).aspx) indica el modo de DetailsView vuelva después de una edición o insertar se ha completado y es útil para mostrar un DetailsView permanentemente en la edición o modo de inserción.


El punto y haga clic en Insertar y editar las capacidades de DetailsView sufren las mismas limitaciones que el control GridView: el usuario debe escribir existente `CategoryID` y `SupplierID` valores a través de un cuadro de texto; carece de la interfaz cualquier lógica de validación; todos campos de producto que no permiten `NULL` valores o no tiene un valor predeterminado especificado en el nivel de base de datos de valor debe incluirse en la interfaz de inserción y así sucesivamente.

Las técnicas, examinaremos de extensión y mejora interfaz de edición de GridView en el futuro artículos se pueden aplicar para el control DetailsView edición e inserción de las interfaces.

## <a name="using-the-formview-for-a-more-flexible-data-modification-user-interface"></a>Uso de FormView para una interfaz de usuario de modificación de datos más Flexible

FormView ofrece compatibilidad integrada para insertar, modificar y eliminar datos, pero ya que utiliza las plantillas en lugar de campos no hay ningún lugar para agregar el BoundFields o el CommandField utilizado por los controles GridView y DetailsView para proporcionar los datos interfaz de modificación. En su lugar, esta interfaz, los controles Web para la recopilación de usuario al agregar un nuevo elemento de entrada o editar una existente, junto con el nuevo, editar, eliminar, insertar, actualizar y cancelar los botones debe agregarse manualmente a las plantillas adecuadas. Afortunadamente, Visual Studio creará automáticamente la interfaz necesaria cuando se enlazan FormView para un origen de datos a través de la lista desplegable en su etiqueta inteligente.

Para ilustrar estas técnicas, empiece agregando un FormView para el `Basics.aspx` página y, desde las etiquetas inteligentes de FormView, enlazarlo a ObjectDataSource ya ha creado. Esto generará un `EditItemTemplate`, `InsertItemTemplate`, y `ItemTemplate` para FormView con controles Web de cuadro de texto para la recopilación de entrada del usuario y los controles de botón Web para el nuevo, editar, eliminar, insertar, actualizar y cancelar los botones. Del Además, FormView `DataKeyNames` propiedad está establecida en el campo de clave principal (`ProductID`) del objeto devuelto por el origen ObjectDataSource. Por último, active la opción de habilitar la paginación en la etiqueta inteligente de FormView.

La siguiente muestra el marcado declarativo para el FormView `ItemTemplate` después de que se ha enlazado FormView a ObjectDataSource. De forma predeterminada, cada campo de valor no booleano del producto se enlaza a la `Text` propiedad de un control Web Label mientras cada campo de valor booleano (`Discontinued`) está enlazado a la `Checked` propiedad de un control de casilla de verificación Web deshabilitado. En el orden de los botones de nuevo, editar y eliminar desencadenar determinada comportamiento FormView cuando hace clic en, es imprescindible que sus `CommandName` valores se establecen en `New`, `Edit`, y `Delete`, respectivamente.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample8.aspx)]

Figura 22 muestra de FormView `ItemTemplate` cuando se ve mediante un explorador. Cada campo product aparece con los botones de nuevo, editar y eliminar en la parte inferior.


[![ItemTemplate FormView predeterminada muestra cada campo de producto junto con nuevas, modificar y eliminar botones](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image55.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image54.png)

**Figura 22**: The FormView predeterminada `ItemTemplate` enumera cada producto campo junto con nuevo, editar y eliminar botones ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image56.png))


Al igual que con GridView y DetailsView, al hacer clic en el botón Eliminar o cualquier botón, LinkButton o ImageButton cuyo `CommandName` propiedad está establecido en las causas de la eliminación de un postback, rellena de ObjectDataSource `DeleteParameters` según el FormView `DataKeyNames`valor e invoca el ObjectDataSource `Delete()` método.

Cuando se hace clic en el botón Editar que habrá trastornos una devolución de datos y se vuelve a enlazar los datos a la `EditItemTemplate`, que es responsable de representar la interfaz de edición. Esta interfaz incluye los controles Web para modificar los datos junto con los botones Actualizar y Cancelar. El valor predeterminado `EditItemTemplate` generados por Visual Studio contiene una etiqueta para los campos de incremento automático (`ProductID`), un cuadro de texto para cada campo de valor no booleano y una casilla de verificación para cada campo de valor booleano. Este comportamiento es muy similar a la BoundFields generado automáticamente en los controles GridView y DetailsView.

> [!NOTE]
> Un pequeño problema con la generación automática de FormView de la `EditItemTemplate` es que presenta TextBox Web controles para los campos que son de solo lectura, como `CategoryName` y `SupplierName`. Ahora veremos cómo para tener en cuenta esto en breve.


Controla el cuadro de texto en el `EditItemTemplate` tienen sus `Text` propiedad enlazado al valor de su campo de datos correspondiente con *enlace de datos bidireccional*. Enlace de datos bidireccional, indicado por `<%# Bind("dataField") %>`, realiza tanto el enlace de datos cuando enlace datos a la plantilla y al rellenar los parámetros de ObjectDataSource para insertar o editar registros. Es decir, cuando el usuario hace clic en el botón Editar de la `ItemTemplate`, el `Bind()` método devuelve el valor del campo de datos especificado. Después de que el usuario realiza los cambios y hace clic en la actualización, los valores envíe de nuevo que corresponden a los campos de datos especificados mediante `Bind()` se aplican a de ObjectDataSource `UpdateParameters`. Como alternativa, el enlace de datos unidireccional, indicado por `<%# Eval("dataField") %>`, solo recupera los valores de campo de datos cuando enlace datos a la plantilla y hace *no* devolver los valores introducidos por el usuario a los parámetros del origen de datos en el postback.

El siguiente marcado declarativo muestra de FormView `EditItemTemplate`. Tenga en cuenta que el `Bind()` método se utiliza en la sintaxis de enlace de datos aquí y que tienen los controles Web de botón de cancelar y actualizar sus `CommandName` propiedades establecidas en consecuencia.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample9.aspx)]

Nuestro `EditItemTemplate`, en este punto, se producirá una excepción que se produzca si se intentan volver a usarlo. El problema es que la `CategoryName` y `SupplierName` campos se representan como controles de cuadro de texto Web en el `EditItemTemplate`. O bien, necesitamos cambiar estos cuadros de texto en etiquetas o quítelas completamente. Vamos a simplemente eliminarlos completamente desde el `EditItemTemplate`.

Figura 23 FormView se muestra en un explorador después de que se ha presionado el botón Editar para Chai. Tenga en cuenta que el `SupplierName` y `CategoryName` campos que se muestran en el `ItemTemplate` ya no están presentes, como se acaba de quitar de la `EditItemTemplate`. Cuando se hace clic en el botón Actualizar FormView procede a través de la misma secuencia de pasos que los controles GridView y DetailsView.


[![De forma predeterminada el EditItemTemplate muestra cada campo Editable de productos como un cuadro de texto o la casilla de verificación](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image58.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image57.png)

**Figura 23**: de forma predeterminada el `EditItemTemplate` muestra cada producto campo Editable como un cuadro de texto o la casilla de verificación ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image59.png))


Cuando se hace clic el botón Insertar en el FormView `ItemTemplate` una devolución de datos que habrá trastornos. Sin embargo, no hay datos se enlazan a FormView porque se va a agregar un nuevo registro. El `InsertItemTemplate` interfaz incluye los controles Web para agregar un nuevo registro junto con los botones Insertar y Cancelar. El valor predeterminado `InsertItemTemplate` generados por Visual Studio contiene un cuadro de texto para cada campo de valor no booleano y una casilla para cada campo de valor booleano, similar a la autogenerado `EditItemTemplate`de la interfaz. Los controles de cuadro de texto tienen sus `Text` propiedad enlazada con el valor de su campo de datos correspondiente con el enlace de datos bidireccional.

El siguiente marcado declarativo muestra de FormView `InsertItemTemplate`. Tenga en cuenta que el `Bind()` método se utiliza en la sintaxis del enlace de datos y que los controles de insertar y cancelar botón Web tienen sus `CommandName` propiedades establecidas en consecuencia.


[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-vb/samples/sample10.aspx)]

Hay un problema con la generación automática de FormView de la `InsertItemTemplate`. En concreto, se crean los controles de cuadro de texto Web incluso para los campos que son de solo lectura, como `CategoryName` y `SupplierName`. Al igual que con el `EditItemTemplate`, es necesario quitar estos cuadros de texto desde el `InsertItemTemplate`.

Figura 24 muestra FormView en un explorador cuando se agrega un nuevo producto, Acme café. Tenga en cuenta que el `SupplierName` y `CategoryName` campos que se muestran en el `ItemTemplate` ya no están presentes, tal como se retiró. Cuando se presiona el botón de insertar la lleva a cabo a través de la misma secuencia de pasos que el control DetailsView FormView, agregar un nuevo registro a la `Products` tabla. Figura 25 muestra detalles del producto de café Acme FormView después de que se ha insertado.


[![El InsertItemTemplate dicta la interfaz de inserción de FormView](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image61.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image60.png)

**Figura 24**: el `InsertItemTemplate` dicta la interfaz de inserción de FormView ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image62.png))


[![Los detalles para el nuevo producto, café Acme, se muestran en FormView](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image64.png)](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image63.png)

**Figura 25**: los detalles para el nuevo producto, café Acme, se muestran en FormView ([haga clic aquí para ver imagen en tamaño completo](an-overview-of-inserting-updating-and-deleting-data-vb/_static/image65.png))


Al separar horizontalmente de solo lectura, edición e inserción interfaces en tres plantillas independientes, FormView permite un mayor grado de control a través de estas interfaces a la GridView y DetailsView la.

> [!NOTE]
> Como el, FormView de DetailsView `CurrentMode` propiedad indica la interfaz que se muestran y su `DefaultMode` propiedad indica la devuelve FormView para el modo después de una edición o se ha completado la inserción.


## <a name="summary"></a>Resumen

En este tutorial se examinan los aspectos básicos de insertar, modificar y eliminar datos mediante el control GridView, DetailsView y FormView. Tres de estos controles proporcionan un cierto nivel de capacidades de modificación de datos integrados que puede utilizarse sin necesidad de escribir una sola línea de código en la página ASP.NET gracias a los controles Web de datos y el origen ObjectDataSource. Sin embargo, la simple elija y haga clic en técnicas render un bastante frail y la interfaz de usuario de modificación de datos de naive. Para proporcionar validación, insertar valores mediante programación, administrar correctamente las excepciones, personalizar la interfaz de usuario y así sucesivamente, necesitaremos se basan en una serie de técnicas que se explicará en la siguientes varios tutoriales.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](limiting-data-modification-functionality-based-on-the-user-cs.md)
> [Siguiente](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)
