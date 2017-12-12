---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-vb
title: Insertar, actualizar y eliminar datos con SqlDataSource (VB) | Documentos de Microsoft
author: rick-anderson
description: "En los tutoriales anteriores hemos visto cómo permitir que el control ObjectDataSource para insertar, actualizar y eliminar datos. El control SqlDataSource admite t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: 9673bef3-892c-45ba-a7d8-0da3d6f48ec5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: 4664e15ca6afa14f072e0840354c7f5a48d97ddf
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="inserting-updating-and-deleting-data-with-the-sqldatasource-vb"></a>Insertar, actualizar y eliminar datos con SqlDataSource (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_49_VB.exe) o [descarga de PDF](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/datatutorial49vb1.pdf)

> En los tutoriales anteriores hemos visto cómo permitir que el control ObjectDataSource para insertar, actualizar y eliminar datos. El control SqlDataSource admite las mismas operaciones, pero el enfoque es diferente y este tutorial muestra cómo configurar el SqlDataSource para insertar, actualizar y eliminar datos.


## <a name="introduction"></a>Introducción

Como se describe en [una visión general de insertar, actualizar y eliminar](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md), el control GridView proporciona actualización integrada y capacidades de eliminación, mientras que los controles DetailsView y FormView incluyen insertar admiten junto con edición y eliminación de funcionalidad. Estas capacidades de modificación de datos se pueden conectar directamente en un control de origen de datos sin una línea de código que deba escribirse. [Una visión general de insertar, actualizar y eliminar](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) examinar utilizando el ObjectDataSource para facilitar Insertar, actualizar y eliminar con los controles GridView, DetailsView y FormView. Como alternativa, SqlDataSource puede usarse en lugar de ObjectDataSource.

Recuerde que para admitir Insertar, actualizar y eliminar con el ObjectDataSource que necesitábamos para especificar los métodos de la capa de objeto que se invocará para realizar la inserción, actualización o eliminación de acción. Con SqlDataSource, debemos proporcionar `INSERT`, `UPDATE`, y `DELETE` SQL instrucciones (o procedimientos almacenados) para ejecutar. Como veremos más adelante en este tutorial, estas instrucciones se pueden crear manualmente o se pueden generar automáticamente por el Asistente para configurar origen de datos de SqlDataSource s.

> [!NOTE]
> Desde que se ha había descrito ya Insertar, modificar y eliminar funciones de GridView, DetailsView y controla FormView, este tutorial se centrará en configurar el control SqlDataSource para admitir estas operaciones. Si desea perfeccionar sus conocimientos sobre la implementación de estas características dentro de la devolución de GridView, DetailsView y FormView, para los tutoriales de edición, insertar y eliminar datos, a partir de [una visión general de insertar, actualizar y eliminar](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md).


## <a name="step-1-specifyinginsertupdate-anddeletestatements"></a>Paso 1: Especificar`INSERT`,`UPDATE`, y`DELETE`instrucciones

Como se ha visto en los últimos dos tutoriales, para recuperar datos de un control SqlDataSource que necesitamos para establece dos propiedades:

1. `ConnectionString`, que especifica qué base de datos para enviar la consulta, y
2. `SelectCommand`, que especifica la instrucción de SQL "ad-hoc" o el nombre del procedimiento almacenado que se ejecutan para devolver los resultados.

Para `SelectCommand` valores con parámetros, el parámetro de valores se especifican a través de las operaciones de asignación SqlDataSource `SelectParameters` colección y puede incluir valores codificados de forma rígida, valores de origen de parámetro comunes (campos de cadena de consulta, variables de sesión, valores de control de Web, y así, sucesivamente), o se pueden asignar mediante programación. Cuando el control SqlDataSource s `Select()` método se invoca mediante programación o automáticamente desde un control Web de datos se establece una conexión a la base de datos, se asignan los valores de parámetro a la consulta y el comando se envían desactivado para el base de datos. A continuación, se devuelven los resultados como un conjunto de datos o un DataReader, dependiendo del valor del control s `DataSourceMode` propiedad.

Además de seleccionar los datos, puede utilizarse el control SqlDataSource para insertar, actualizar y eliminar datos proporcionando `INSERT`, `UPDATE`, y `DELETE` instrucciones SQL en la misma manera. Simplemente asigne el `InsertCommand`, `UpdateCommand`, y `DeleteCommand` propiedades la `INSERT`, `UPDATE`, y `DELETE` instrucciones SQL que se ejecutan. Si las instrucciones tienen parámetros (tal y como lo harían más siempre), incluirlos en el `InsertParameters`, `UpdateParameters`, y `DeleteParameters` las colecciones.

Una vez un `InsertCommand`, `UpdateCommand`, o `DeleteCommand` se especificó un valor, la opción Habilitar inserción, Habilitar edición o Habilitar eliminación en los datos correspondientes de etiqueta inteligente de Web control s pasará a estar disponible. Para ilustrar esto, permiten s tome un ejemplo de la `Querying.aspx` página hemos creado en el [consultar datos con el SqlDataSource Control](querying-data-with-the-sqldatasource-control-vb.md) tutorial y ampliar para incluir a eliminar las capacidades.

Comience abriendo la `InsertUpdateDelete.aspx` y `Querying.aspx` páginas desde el `SqlDataSource` carpeta. Desde el diseñador en la `Querying.aspx` , seleccione el SqlDataSource y GridView desde el primer ejemplo (el `ProductsDataSource` y `GridView1` controles). Después de seleccionar los dos controles, vaya al menú Edición y elija Copiar (o simplemente escribir CTRL+c). A continuación, vaya al diseñador de `InsertUpdateDelete.aspx` y pegar en los controles. Después de haber movido los dos controles a `InsertUpdateDelete.aspx`, pruebe la página en un explorador. Debería ver los valores de la `ProductID`, `ProductName`, y `UnitPrice` columnas para todos los registros en la `Products` tabla de base de datos.


[![Se enumeran todos los productos, ordenados por ProductID](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image1.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image1.png)

**Figura 1**: todos los productos se enumeran, ordenados por `ProductID` ([haga clic aquí para ver la imagen a tamaño completo](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image2.png))


## <a name="adding-the-sqldatasource-sdeletecommandanddeleteparametersproperties"></a>Agregar las operaciones de asignación SqlDataSource`DeleteCommand`y`DeleteParameters`propiedades

En este momento tenemos una SqlDataSource que simplemente devuelve todos los registros de la `Products` tabla y un control GridView que presenta estos datos. Nuestro objetivo es ampliar este ejemplo para permitir al usuario eliminar productos a través de GridView. Para ello es preciso especificar valores para el control SqlDataSource s `DeleteCommand` y `DeleteParameters` propiedades y, a continuación, configurar GridView para admitir la eliminación.

El `DeleteCommand` y `DeleteParameters` propiedades pueden especificarse en una de varias maneras:

- A través de la sintaxis declarativa
- Desde la ventana de propiedades en el diseñador
- Desde la pantalla de procedimiento almacenado en el Asistente para configurar orígenes de datos o especificar una instrucción SQL personalizada
- Mediante el botón avanzado en el especificar columnas de una tabla de la pantalla de vista en el Asistente para configurar orígenes de datos, lo que generará automáticamente realmente el `DELETE` colección de instrucción y los parámetros SQL utilizada en el `DeleteCommand` y `DeleteParameters` propiedades

Examinaremos cómo hacer automáticamente el `DELETE` instrucción creada en el paso 2. Por ahora, permiten s utilizar la ventana de propiedades en el diseñador, aunque la opción de la sintaxis declarativa o el Asistente para configurar origen de datos funcione igual de bien.

Desde el diseñador en `InsertUpdateDelete.aspx`, haga clic en el `ProductsDataSource` SqlDataSource y, a continuación, se abrirá la ventana de propiedades (en el menú Ver, elija la ventana Propiedades o, simplemente tiene que presionar F4). Seleccione la propiedad DeleteQuery, que le llevará a un conjunto de puntos suspensivos.


![Seleccione la propiedad DeleteQuery desde la ventana Propiedades](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image2.gif)

**Figura 2**: seleccione la propiedad DeleteQuery desde la ventana Propiedades


> [!NOTE]
> T SqlDataSource tienen una propiedad de DeleteQuery. En su lugar, DeleteQuery es una combinación de la `DeleteCommand` y `DeleteParameters` propiedades y solo se muestra en la ventana Propiedades cuando se visualizan en la ventana a través del diseñador. Si se trata de la ventana Propiedades de la vista del origen, encontrará el `DeleteCommand` propiedad en su lugar.


Haga clic en el botón de puntos suspensivos en la propiedad DeleteQuery para que aparezca el cuadro de diálogo Editor de parámetros y comandos cuadro (Véase la figura 3). Desde este cuadro de diálogo puede especificar el `DELETE` instrucción SQL y especifique los parámetros. Escriba la siguiente consulta en la `DELETE` cuadro de texto de comando (ya sea manualmente o mediante el generador de consultas, si lo prefiere):

[!code-sql[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/samples/sample1.sql)]

A continuación, haga clic en el botón Actualizar parámetros para agregar el `@ProductID` parámetro a la lista de los siguientes parámetros.


[![Seleccione la propiedad DeleteQuery desde la ventana Propiedades](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image3.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image3.png)

**Figura 3**: seleccione la propiedad DeleteQuery desde la ventana Propiedades ([haga clic aquí para ver la imagen a tamaño completo](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image4.png))


Hacer *no* proporcionar un valor para este parámetro (deje su parámetro de origen en ninguno). Una vez que se agrega compatibilidad eliminando a GridView, GridView proporcionará automáticamente este valor de parámetro, utilizando el valor de su `DataKeys` colección para la fila cuyo botón Eliminar se hizo clic.

> [!NOTE]
> El nombre del parámetro usado en el `DELETE` consulta *debe* ser el mismo que el nombre de la `DataKeyNames` valor en GridView, DetailsView o FormView. Es decir, el parámetro en el `DELETE` instrucción se denomina intencionadamente `@ProductID` (en lugar de, por ejemplo, `@ID`), ya que es el nombre de columna de clave principal en la tabla Products (y, por lo tanto, el valor de DataKeyNames en GridView) `ProductID`.


Si el nombre del parámetro y `DataKeyNames` coincidencia de valor t, GridView no puede asignar automáticamente el parámetro el valor de la `DataKeys` colección.

Después de escribir la información relacionada con la eliminación en el cuadro de diálogo Editor de parámetros y comandos, haga clic en Aceptar y vaya a la vista de código fuente para examinar el marcado declarativo resultante:

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/samples/sample2.aspx)]

Tenga en cuenta la adición de la `DeleteCommand` propiedad, así como la `<DeleteParameters>` sección y el objeto de parámetro denominado `productID`.

## <a name="configuring-the-gridview-for-deleting"></a>Configuración del control GridView para eliminar

Con el `DeleteCommand` propiedad agrega la etiqueta inteligente de GridView s ahora contiene la opción Habilitar eliminación. Continúe y marque esta casilla de verificación. Como se describe en [una visión general de insertar, actualizar y eliminar](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md), esto hace que el control GridView agregar un CommandField con su `ShowDeleteButton` propiedad establecida en `True`. Como en la figura 4 muestra, cuando se visita la página a través de un explorador se incluye un botón Eliminar. Esta página fuera de prueba mediante la eliminación de algunos productos.


[![Cada fila de GridView incluye ahora un botón Eliminar.](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image4.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image5.png)

**Figura 4**: cada fila de GridView incluye ahora un botón Eliminar ([haga clic aquí para ver la imagen a tamaño completo](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image6.png))


Al hacer clic en un botón Eliminar, se produce un postback, GridView asigna el `ProductID` el valor del parámetro de la `DataKeys` valor de la colección para la fila cuyo botón Eliminar se hizo clic e invoca las operaciones de asignación SqlDataSource `Delete()` método. El control SqlDataSource, a continuación, se conecta a la base de datos y ejecuta el `DELETE` instrucción. GridView, a continuación, vuelve a enlazar a SqlDataSource, recibir y mostrar el conjunto actual de productos (que ya no se incluye el registro eliminado just).

> [!NOTE]
> Debido a que utiliza el control GridView su `DataKeys` colección para rellenar los parámetros SqlDataSource, lo s vitales que las operaciones de asignación GridView `DataKeyNames` propiedad se establece en las columnas que constituyen la clave principal y que las operaciones de asignación SqlDataSource `SelectCommand` devuelve Estas columnas. Además, lo importante que el parámetro de nombre en las operaciones de asignación SqlDataSource `DeleteCommand` está establecido en `@ProductID`. Si el `DataKeyNames` no está establecida la propiedad o el parámetro no se denomina `@ProductsID`, haga clic en el botón Eliminar provocará una devolución de datos, pero t ganado realmente elimina todos los registros.


Figura 5 se muestra esta interacción gráficamente. Hacen referencia a la [examinar los eventos asociados con la inserción, actualización y eliminación](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) tutorial para obtener una explicación más detallada en la cadena de eventos asociados a insertar, actualizar y eliminar de un control Web de datos.


![Haga clic en el botón Eliminar en el control GridView invoca el método de Delete() de s de SqlDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image5.gif)

**Figura 5**: haga clic en el botón Eliminar en el control GridView, se invoca la s SqlDataSource `Delete()` (método)


## <a name="step-2-automatically-generating-theinsertupdate-anddeletestatements"></a>Paso 2: Generar automáticamente el`INSERT`,`UPDATE`, y`DELETE`instrucciones

Como paso 1 examinar, `INSERT`, `UPDATE`, y `DELETE` se pueden especificar instrucciones SQL a través de la ventana Propiedades o la sintaxis declarativa del control s. Sin embargo, este enfoque requiere que manualmente eliminamos las instrucciones SQL a mano, algo que puede resultar más monótonas y propensa a errores. Afortunadamente, el Asistente para configurar orígenes de datos proporciona una opción para que la `INSERT`, `UPDATE`, y `DELETE` las instrucciones que se genera automáticamente cuando se usa el especificar columnas de una tabla de la pantalla de vista.

Permiten s explorar esta opción de generación automática. Agregar un DetailsView al diseñador en `InsertUpdateDelete.aspx` y establecer su `ID` propiedad `ManageProducts`. A continuación, desde la etiqueta inteligente de DetailsView s, optar por crear un nuevo origen de datos y crear un SqlDataSource denominado `ManageProductsDataSource`.


[![Crear un nuevo SqlDataSource denominado ManageProductsDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image6.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image7.png)

**Figura 6**: crear un nuevo SqlDataSource denominado `ManageProductsDataSource` ([haga clic aquí para ver la imagen a tamaño completo](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image8.png))


Desde el Asistente para configurar origen de datos, optar por usar la `NORTHWINDConnectionString` conexión cadena y haga clic en siguiente. Desde la configuración de la pantalla de la instrucción Select, deje especificar columnas en un botón de opción de tabla o vista seleccionado y elija la `Products` tabla en la lista desplegable. Seleccione el `ProductID`, `ProductName`, `UnitPrice`, y `Discontinued` las columnas de la lista de la casilla de verificación.


[![Con la tabla Products, devolver ProductID, ProductName, UnitPrice y columnas no incluidas](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image7.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image9.png)

**Figura 7**: mediante la `Products` tabla, devolver el `ProductID`, `ProductName`, `UnitPrice`, y `Discontinued` columnas ([haga clic aquí para ver la imagen a tamaño completo](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image10.png))


Para generar automáticamente `INSERT`, `UPDATE`, y `DELETE` instrucciones en función de la tabla seleccionada y las columnas, haga clic en el botón Opciones avanzadas y comprobar Generate `INSERT`, `UPDATE`, y `DELETE` casilla de verificación de las instrucciones.


![Active la casilla de verificación de instrucciones generar INSERT, UPDATE y DELETE](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image8.gif)

**Figura 8**: comprobar Generate `INSERT`, `UPDATE`, y `DELETE` instrucciones casilla


Generate `INSERT`, `UPDATE`, y `DELETE` instrucciones casilla sólo estará puede activar si la tabla seleccionada tiene una clave principal y la columna de clave principal (o columnas) se incluyen en la lista de columnas devueltas. Casilla de verificación Usar simultaneidad optimista, que se convierte en seleccionable una vez Generate `INSERT`, `UPDATE`, y `DELETE` casilla de verificación de las instrucciones se ha comprobado, se aumentan las `WHERE` cláusulas en el cuadro `UPDATE` y `DELETE` instrucciones para proporcionar el control de simultaneidad optimista. Por ahora, deje esta casilla no está activada; Examinaremos simultaneidad optimista con el control SqlDataSource en el tutorial siguiente.

Después de comprobar Generate `INSERT`, `UPDATE`, y `DELETE` casilla de verificación de las instrucciones, haga clic en Aceptar para volver a la pantalla Configurar instrucción Select, a continuación, haga clic en siguiente y, a continuación, finalizar, para completar el Asistente para configurar orígenes de datos. Al finalizar el asistente, Visual Studio agregará BoundFields a DetailsView para la `ProductID`, `ProductName`, y `UnitPrice` columnas y un CampoCasillaVerificación para la `Discontinued` columna. Desde la etiqueta inteligente de DetailsView s, active la opción Habilitar paginación para que el usuario accedió a esta página puede recorrer los productos. Borrar también las operaciones de asignación DetailsView `Width` y `Height` propiedades.

Observe que la etiqueta inteligente tiene las opciones Habilitar inserción, Habilitar edición y Habilitar eliminación disponibles. Esto es porque el SqlDataSource contiene valores para su `InsertCommand`, `UpdateCommand`, y `DeleteCommand`, como se muestra en la siguiente sintaxis declarativa:

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/samples/sample3.aspx)]

Tenga en cuenta cómo el control SqlDataSource ha tenido valores se establecen automáticamente para su `InsertCommand`, `UpdateCommand`, y `DeleteCommand` propiedades. El conjunto de columnas que se hace referencia en el `InsertCommand` y `UpdateCommand` propiedades se basan en los de la `SELECT` instrucción. Es decir, en lugar de tener *cada* columna de productos en la `InsertCommand` y `UpdateCommand`, hay solo las columnas especificadas en el `SelectCommand` (menos `ProductID`, que se omite porque se s un [ `IDENTITY` columna](http://www.sqlteam.com/item.asp?ItemID=102), cuyo valor no se puede cambiar cuando edita y que se asigna automáticamente al insertar). Además, para cada parámetro en el `InsertCommand`, `UpdateCommand`, y `DeleteCommand` propiedades existen los parámetros correspondientes de la `InsertParameters`, `UpdateParameters`, y `DeleteParameters` las colecciones.

Para activar las características de modificación de datos de DetailsView s, compruebe el Habilitar inserción, Habilitar edición y Habilitar eliminación opciones en su etiqueta inteligente. Esto agrega un CommandField con su `ShowInsertButton`, `ShowEditButton`, y `ShowDeleteButton` propiedades establecidas en `True`.

Visite la página en un explorador y tenga en cuenta la edición, eliminación y nuevos botones que se incluyen en DetailsView. Haga clic en el botón Editar convierte DetailsView en modo de edición, que muestra cada BoundField cuyo `ReadOnly` propiedad está establecida en `False` (valor predeterminado) como un cuadro de texto y el CampoCasillaVerificación como una casilla de verificación.


[![Las operaciones de asignación DetailsView predeterminado de interfaz de edición](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image9.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image11.png)

**Figura 9**: s DetailsView la interfaz de edición predeterminada ([haga clic aquí para ver la imagen a tamaño completo](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image12.png))


De forma similar, puede eliminar el producto seleccionado o agregar un nuevo producto en el sistema. Puesto que la `InsertCommand` instrucción solo funciona con la `ProductName`, `UnitPrice`, y `Discontinued` columnas, las demás columnas o tienen `NULL` o su valor predeterminado asignado por la base de datos con insert. Al igual que con el origen ObjectDataSource, si la `InsertCommand` falta una tabla de base de datos las columnas que don t permiten `NULL` s y don t tienen un valor predeterminado, se producirá un error SQL al intentar ejecutar el `INSERT` instrucción.

> [!NOTE]
> Las operaciones de asignación DetailsView insertar y modificar interfaces falta algún tipo de personalización o la validación. Para agregar controles de validación o personalizar las interfaces, debe convertir el BoundFields a TemplateFields. Hacer referencia a la [agregar controles de validación a las Interfaces de insertar y modificar](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) y [personalizar la interfaz de modificación de datos](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) tutoriales para obtener más información.


Además, tenga en cuenta que para actualizar y eliminar, DetailsView usa el producto actual s `DataKey` valor, que solo está presente si el `DataKeyNames` se configura la propiedad. Si edita o elimina parece que no tienen ningún efecto, asegúrese de que el `DataKeyNames` se establece la propiedad.

## <a name="limitations-of-automatically-generating-sql-statements"></a>Limitaciones de generación automática de instrucciones SQL

Desde Generate `INSERT`, `UPDATE`, y `DELETE` opción instrucciones sólo está disponible al seleccionar las columnas de una tabla, para las consultas más complejas tendrá que escribir su propio `INSERT`, `UPDATE`, y `DELETE` instrucciones como hicimos en el paso 1. Habitualmente, SQL `SELECT` usan instrucciones `JOIN` s para devolver datos de una o varias tablas de búsqueda para la visualización (como la que se devuelvan el `Categories` tabla s `CategoryName` campo para mostrar información del producto). Al mismo tiempo, podría desear permitir al usuario editar, actualizar o insertar datos en la tabla principal (`Products`, en este caso).

Mientras el `INSERT`, `UPDATE`, y `DELETE` instrucciones pueden escribirse manualmente, tenga en cuenta la siguiente sugerencia para ahorrar tiempo. Configurar inicialmente el SqlDataSource para que vuelva extrae datos de únicamente la `Products` tabla. Usar el Configurar origen de datos asistente s especificar columnas de una tabla o vista de pantalla para que puedan generar automáticamente el `INSERT`, `UPDATE`, y `DELETE` las instrucciones. A continuación, después de completar al asistente, elija Configurar el SelectQuery desde la ventana Propiedades (o, o bien, volver a la opción de procedimiento almacenado o el Asistente para configurar origen de datos, pero con la especificar una instrucción SQL personalizada). A continuación, actualice el `SELECT` instrucción deben incluir el `JOIN` sintaxis. Esta técnica ofrece las ventajas de ahorro de tiempo de las instrucciones SQL generadas automáticamente y permite más personalizada `SELECT` instrucción.

Otra limitación de generar automáticamente el `INSERT`, `UPDATE`, y `DELETE` instrucciones es que las columnas de la `INSERT` y `UPDATE` instrucciones se basan en las columnas devueltas por la `SELECT` instrucción. Podemos necesitamos actualizar o insertar campos más o menos, sin embargo. Por ejemplo, en el ejemplo del paso 2, es posible que deseamos que la `UnitPrice` BoundField sea de solo lectura. En ese caso, se t no debe aparecer en el `UpdateCommand`. O quizás deseamos establecer el valor de un campo de tabla que no aparece en el control GridView. Por ejemplo, al agregar un nuevo registro puede que convenga la `QuantityPerUnit` valor establecido en la lista de tareas.

Si se requieren estas personalizaciones, debe realizar manualmente, ya sea a través de la ventana Propiedades, especifique una instrucción SQL personalizada u opción de procedimiento almacenado en el asistente, o a través de la sintaxis declarativa.

> [!NOTE]
> Al agregar parámetros que no tienen campos correspondientes en los datos de control Web, tenga en cuenta que será necesario que los valores de estos parámetros se pueden asignar valores de alguna manera. Estos valores pueden ser: codificado de forma rígida directamente en el `InsertCommand` o `UpdateCommand`; pueden proceder de algún origen predefinido (la cadena de consulta, el estado de sesión, controles Web en la página y así sucesivamente); o se puede asignar mediante programación, como hemos visto en el tutorial anterior.


## <a name="summary"></a>Resumen

En orden de los datos de que controles Web para utilizar sus integradas de inserción, edición y eliminación de recursos, el control de origen de datos que están enlazados a debe ofrecer esta funcionalidad. Para el SqlDataSource, esto significa que `INSERT`, `UPDATE`, y `DELETE` instrucciones SQL deben estar asignadas a la `InsertCommand`, `UpdateCommand`, y `DeleteCommand` propiedades. Estas propiedades y las colecciones de parámetros correspondientes, pueden agregar manualmente o se genera automáticamente con el Asistente para configurar orígenes de datos. En este tutorial se examinan ambas técnicas.

Se examinan con simultaneidad optimista ObjectDataSource en el [implementar simultaneidad optimista](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) tutorial. El control SqlDataSource también proporciona compatibilidad con la simultaneidad optimista. Como se indicó en el paso 2, cuando se genera automáticamente el `INSERT`, `UPDATE`, y `DELETE` instrucciones, el asistente ofrece un uso de la opción de simultaneidad optimista. Como se verá en el siguiente tutorial, usar simultaneidad optimista con SqlDataSource modifica el `WHERE` cláusulas de la `UPDATE` y `DELETE` instrucciones para asegurarse de que los valores de las otras columnas conocemos t cambiado desde la últimos los datos se muestra en la página.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [ *SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Anterior](using-parameterized-queries-with-the-sqldatasource-vb.md)
[Siguiente](implementing-optimistic-concurrency-with-the-sqldatasource-vb.md)
