---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-vb
title: Insertar, actualizar y eliminar datos con SqlDataSource (VB) | Microsoft Docs
author: rick-anderson
description: En los tutoriales anteriores, hemos aprendido cómo permitir que el control ObjectDataSource de inserción, actualización y eliminación de datos. El control SqlDataSource admite t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: 9673bef3-892c-45ba-a7d8-0da3d6f48ec5
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: 2d4c98da314a5a774d9c82280fe9b1949ef4c337
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363946"
---
<a name="inserting-updating-and-deleting-data-with-the-sqldatasource-vb"></a>Insertar, actualizar y eliminar datos con SqlDataSource (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_49_VB.exe) o [descargar PDF](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/datatutorial49vb1.pdf)

> En los tutoriales anteriores, hemos aprendido cómo permitir que el control ObjectDataSource de inserción, actualización y eliminación de datos. El control SqlDataSource es compatible con las mismas operaciones, pero el enfoque es diferente y, en este tutorial se muestra cómo configurar SqlDataSource para insertar, actualizar y eliminar datos.


## <a name="introduction"></a>Introducción

Como se describe en [una visión general de insertar, actualizar y eliminar](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md), el control GridView ofrece integrados actualizar y eliminar capacidades, mientras que los controles DetailsView y FormView incluyen Insertar junto con edición y eliminación de funcionalidad. Estas capacidades de modificación de datos se pueden conectar directamente en un control de origen de datos sin una línea de código que necesitan que se va a escribir. [Una visión general de insertar, actualizar y eliminar](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) examinar utilizando el origen ObjectDataSource para facilitar Insertar, actualizar y eliminar con los controles GridView, DetailsView y FormView. Como alternativa, SqlDataSource puede usarse en lugar de ObjectDataSource.

Recuerde para admitir Insertar, actualizar y eliminar, con el origen ObjectDataSource que necesitábamos para especificar los métodos de la capa de objeto que se invocará para realizar la inserción, actualización o eliminación de la acción. Con SqlDataSource, necesitamos proporcionar `INSERT`, `UPDATE`, y `DELETE` SQL instrucciones (o procedimientos almacenados) para ejecutar. Como veremos en este tutorial, estas instrucciones se pueden crear manualmente o pueden generarse automáticamente mediante el Asistente para configurar origen de datos de SqlDataSource s.

> [!NOTE]
> Desde que creamos ve discutió Insertar, editar y eliminar las capacidades del control GridView, DetailsView y FormView controles, en este tutorial se centrará en la configuración del control SqlDataSource para admitir estas operaciones. Si necesita perfeccionar sus conocimientos sobre la implementación de estas características dentro del control GridView, DetailsView y FormView, la devolución para los tutoriales de edición, insertar y eliminar datos, empezando por [una visión general de insertar, actualizar y eliminar](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md).


## <a name="step-1-specifyinginsertupdate-anddeletestatements"></a>Paso 1: Especificar`INSERT`,`UPDATE`, y`DELETE`instrucciones

Como se ve que se muestra en los dos últimos tutoriales, para recuperar datos de un control SqlDataSource, que es necesario establece dos propiedades:

1. `ConnectionString`, que especifica qué base de datos para enviar la consulta, y
2. `SelectCommand`, que especifica la instrucción de SQL ad hoc o el nombre del procedimiento almacenado que se ejecutan para devolver los resultados.

Para `SelectCommand` valores con parámetros, el parámetro de valores se especifican a través de las operaciones de asignación SqlDataSource `SelectParameters` colección y puede incluir valores codificados de forma rígida, valores de origen de parámetro comunes (campos de cadena de consulta, variables de sesión, valores de control Web, y así, sucesivamente), o se pueden asignar mediante programación. Cuando el control SqlDataSource s `Select()` método se invoca mediante programación o automáticamente desde un control Web de datos se establece una conexión a la base de datos, se asignan los valores de parámetro a la consulta y el comando se transporte desactivar al base de datos. A continuación, se devuelven los resultados como un conjunto de datos o un DataReader, dependiendo del valor del control s `DataSourceMode` propiedad.

Además de seleccionar datos, puede utilizarse el control SqlDataSource para insertar, actualizar y eliminar datos proporcionando `INSERT`, `UPDATE`, y `DELETE` instrucciones SQL en la misma manera. Simplemente asigne el `InsertCommand`, `UpdateCommand`, y `DeleteCommand` propiedades el `INSERT`, `UPDATE`, y `DELETE` instrucciones SQL para ejecutar. Si las instrucciones tienen parámetros (como aparecerán casi siempre), incluirlos en el `InsertParameters`, `UpdateParameters`, y `DeleteParameters` colecciones.

Una vez un `InsertCommand`, `UpdateCommand`, o `DeleteCommand` se ha especificado el valor, la opción Habilitar inserción, Habilitar edición o Habilitar eliminación en los datos correspondientes de etiqueta inteligente del control s Web estará disponible. Para ilustrar esto, permiten s dar un ejemplo de la `Querying.aspx` página creada en el [consultar datos con el SqlDataSource Control](querying-data-with-the-sqldatasource-control-vb.md) tutorial y ampliar para incluir a eliminar las capacidades.

Comience abriendo la `InsertUpdateDelete.aspx` y `Querying.aspx` páginas desde el `SqlDataSource` carpeta. Desde el diseñador en el `Querying.aspx` , seleccione el SqlDataSource y GridView del primer ejemplo (el `ProductsDataSource` y `GridView1` controles). Después de seleccionar los dos controles, vaya al menú de edición y elija Copiar (o simplemente presione Ctrl + C). A continuación, vaya al diseñador de `InsertUpdateDelete.aspx` y pegar en los controles. Después de haber movido a través de los dos controles para `InsertUpdateDelete.aspx`, pruebe la página en un explorador. Debería ver los valores de la `ProductID`, `ProductName`, y `UnitPrice` columnas para todos los registros en el `Products` tabla de base de datos.


[![Se muestran todos los productos, ordenados por ProductID](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image1.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image1.png)

**Figura 1**: se enumeran todos los productos, ordenados por `ProductID` ([haga clic aquí para ver imagen en tamaño completo](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image2.png))


## <a name="adding-the-sqldatasource-sdeletecommandanddeleteparametersproperties"></a>Agregar la s SqlDataSource`DeleteCommand`y`DeleteParameters`propiedades

En este momento tenemos una SqlDataSource que simplemente devuelve todos los registros desde el `Products` tabla y un control GridView que procesa estos datos. Nuestro objetivo es ampliar este ejemplo para permitir al usuario eliminar productos a través del control GridView. Para lograr esto es necesario especificar valores para el control SqlDataSource s `DeleteCommand` y `DeleteParameters` propiedades y, a continuación, configurar el control GridView para admitir la eliminación.

El `DeleteCommand` y `DeleteParameters` las propiedades se pueden especificar de varias maneras:

- Mediante la sintaxis declarativa
- Desde la ventana Propiedades en el diseñador
- Desde la pantalla de procedimiento almacenado en el Asistente para configurar orígenes de datos o especificar una instrucción SQL personalizada
- Mediante el botón avanzado en el especificar columnas de una tabla de la pantalla de vista en el Asistente para configurar orígenes de datos, lo que realmente genera automáticamente el `DELETE` colección instrucción y los parámetros SQL utilizada en el `DeleteCommand` y `DeleteParameters` propiedades

Examinaremos cómo tienen automáticamente la `DELETE` instrucción creada en el paso 2. Por ahora, permiten s use la ventana Propiedades en el diseñador, aunque el Asistente para configurar el origen de datos o la opción de sintaxis declarativa funcionaría igual de bien.

Desde el diseñador en `InsertUpdateDelete.aspx`, haga clic en el `ProductsDataSource` SqlDataSource y, a continuación, se abrirá la ventana de propiedades (en el menú Ver, elija la ventana Propiedades o, simplemente tiene que presionar F4). Seleccione la propiedad DeleteQuery, lo que muestra un conjunto de puntos suspensivos.


![Seleccione la propiedad DeleteQuery desde la ventana Propiedades](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image2.gif)

**Figura 2**: seleccione la propiedad DeleteQuery desde la ventana Propiedades


> [!NOTE]
> T SqlDataSource tienen una propiedad DeleteQuery. En su lugar, DeleteQuery es una combinación de la `DeleteCommand` y `DeleteParameters` propiedades y solo se muestra en la ventana Propiedades cuando se ve la ventana a través del diseñador. Si está buscando en la ventana Propiedades de la vista del origen, encontrará el `DeleteCommand` propiedad en su lugar.


Haga clic en el botón de puntos suspensivos en la propiedad DeleteQuery para que aparezca el cuadro de diálogo Editor de parámetros y comandos cuadro (consulte la figura 3). Desde este cuadro de diálogo puede especificar el `DELETE` instrucción SQL y especifique los parámetros. Escriba la siguiente consulta en el `DELETE` cuadro de texto de comando (ya sea manualmente o mediante el generador de consultas, si lo prefiere):

[!code-sql[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/samples/sample1.sql)]

A continuación, haga clic en el botón Actualizar parámetros para agregar el `@ProductID` parámetro a la lista de los parámetros siguientes.


[![Seleccione la propiedad DeleteQuery desde la ventana Propiedades](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image3.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image3.png)

**Figura 3**: seleccione la propiedad DeleteQuery desde la ventana Propiedades ([haga clic aquí para ver imagen en tamaño completo](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image4.png))


Hacer *no* proporcionar un valor para este parámetro (deje su parámetro de origen en ninguno). Una vez que se agrega compatibilidad eliminar en el control GridView, el control GridView proporcionará automáticamente este valor de parámetro con el valor de su `DataKeys` colección para la fila cuyo botón Delete se hizo clic.

> [!NOTE]
> El nombre del parámetro usado en el `DELETE` consulta *debe* ser el mismo que el nombre de la `DataKeyNames` valor en el control GridView, DetailsView o FormView. Es decir, el parámetro en el `DELETE` instrucción se denomina intencionadamente `@ProductID` (en lugar de, por ejemplo, `@ID`), ya que es el nombre de columna de clave principal en la tabla Products (y, por tanto, el valor DataKeyNames en GridView) `ProductID`.


Si el nombre del parámetro y `DataKeyNames` coincidencia de valor t, el control GridView no puede asignar automáticamente el parámetro el valor de la `DataKeys` colección.

Después de escribir la información relacionada con la eliminación en el cuadro de diálogo Editor de parámetros y comandos, haga clic en Aceptar y vaya a la vista de código fuente para examinar el marcado declarativo resultante:

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/samples/sample2.aspx)]

Tenga en cuenta la adición de la `DeleteCommand` propiedad así como el `<DeleteParameters>` sección y el objeto de parámetro denominado `productID`.

## <a name="configuring-the-gridview-for-deleting"></a>Configuración del control GridView para eliminar

Con el `DeleteCommand` agrega la propiedad, la etiqueta inteligente de GridView s ahora contiene la opción Habilitar eliminación. Continúe y Active esta casilla. Como se describe en [una visión general de insertar, actualizar y eliminar](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md), esto hace que el control GridView agregar un CommandField con su `ShowDeleteButton` propiedad establecida en `True`. Como en la figura 4 muestra, cuando se visita la página a través de un explorador se incluye un botón Eliminar. Pruebe esta página horizontalmente mediante la eliminación de algunos productos.


[![Cada fila GridView ahora incluye un botón Eliminar](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image4.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image5.png)

**Figura 4**: cada fila GridView ahora incluye un botón Eliminar ([haga clic aquí para ver imagen en tamaño completo](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image6.png))


Al hacer clic en un botón de eliminación, se produce un postback, el control GridView se asigna el `ProductID` parámetro el valor de la `DataKeys` valor de la colección para la fila cuyo botón Delete se hizo clic e invoca la s SqlDataSource `Delete()` método. El control SqlDataSource, a continuación, se conecta a la base de datos y ejecuta el `DELETE` instrucción. El control GridView, a continuación, vuelve a enlazar de SqlDataSource, obtener y mostrar el conjunto actual de productos (que ya no incluye el registro recién eliminados).

> [!NOTE]
> Puesto que el control GridView se utiliza su `DataKeys` colección para rellenar los parámetros de SqlDataSource, lo s fundamental que la s GridView `DataKeyNames` propiedad se establece en las columnas que constituyen la clave principal y que la s SqlDataSource `SelectCommand` devuelve Estas columnas. Además, lo importante que el parámetro de nombre de la s SqlDataSource `DeleteCommand` está establecido en `@ProductID`. Si el `DataKeyNames` no está establecida la propiedad o el parámetro no se denomina `@ProductsID`, al hacer clic en el botón Eliminar provocará una devolución de datos, pero t ganado realmente eliminar cualquier registro.


Figura 5 se muestra gráficamente esta interacción. Vuelva a consultar el [examinando los eventos asociados con la inserción, actualización y eliminación](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) tutorial para obtener una explicación más detallada de la cadena de eventos asociados con la inserción, actualización y eliminación de un control Web de datos.


![Al hacer clic en el botón Eliminar en el control GridView invoca el método de Delete() s SqlDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image5.gif)

**Figura 5**: al hacer clic en el botón Eliminar en el control GridView se invoca la s SqlDataSource `Delete()` (método)


## <a name="step-2-automatically-generating-theinsertupdate-anddeletestatements"></a>Paso 2: Generar automáticamente el`INSERT`,`UPDATE`, y`DELETE`instrucciones

Como paso 1 examinado, `INSERT`, `UPDATE`, y `DELETE` instrucciones SQL se pueden especificar a través de la ventana Propiedades o la sintaxis declarativa del control s. Sin embargo, este enfoque requiere que manualmente eliminamos las instrucciones SQL a mano, que puede resultar más monótonas y propensas a errores. Afortunadamente, el Asistente para configurar orígenes de datos proporciona una opción para que la `INSERT`, `UPDATE`, y `DELETE` instrucciones generadas automáticamente cuando se usa el especificar columnas de una tabla de la pantalla de vista.

Permiten s explorar esta opción de generación automática. Agregar un DetailsView al diseñador en `InsertUpdateDelete.aspx` y establezca su `ID` propiedad `ManageProducts`. A continuación, en la etiqueta inteligente de DetailsView s, optar por crear un nuevo origen de datos y crear un SqlDataSource denominado `ManageProductsDataSource`.


[![Crear un nuevo SqlDataSource denominado ManageProductsDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image6.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image7.png)

**Figura 6**: crear un nuevo SqlDataSource denominado `ManageProductsDataSource` ([haga clic aquí para ver imagen en tamaño completo](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image8.png))


Desde el Asistente para configurar origen de datos, optar por usar el `NORTHWINDConnectionString` connection string y haga clic en siguiente. Desde la configuración de la pantalla de la instrucción Select, deje el especificar columnas en un botón de opción de tabla o vista seleccionado y elija el `Products` tabla en la lista desplegable. Seleccione el `ProductID`, `ProductName`, `UnitPrice`, y `Discontinued` columnas en la lista de la casilla de verificación.


[![Uso de la tabla Products, devolver el ProductID, ProductName, UnitPrice y las columnas no incluidas](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image7.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image9.png)

**Figura 7**: mediante la `Products` de tabla, se devuelve el `ProductID`, `ProductName`, `UnitPrice`, y `Discontinued` columnas ([haga clic aquí para ver imagen en tamaño completo](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image10.png))


Para generar automáticamente `INSERT`, `UPDATE`, y `DELETE` instrucciones basadas en la tabla seleccionada y las columnas, haga clic en el botón Opciones avanzadas y compruebe la generar `INSERT`, `UPDATE`, y `DELETE` casilla de verificación de las instrucciones.


![Active la casilla de verificación de instrucciones generar INSERT, UPDATE y DELETE](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image8.gif)

**Figura 8**: comprobar la generar `INSERT`, `UPDATE`, y `DELETE` instrucciones Checkbox


Generate `INSERT`, `UPDATE`, y `DELETE` instrucciones casilla sólo estará activarse si la tabla seleccionada tiene una clave principal y la columna de clave principal (o columnas) se incluyen en la lista de columnas devueltas. Casilla de verificación Usar simultaneidad optimista, que se convierte en seleccionable una vez Generate `INSERT`, `UPDATE`, y `DELETE` casilla de verificación de las instrucciones se han comprobado, se aumentan la `WHERE` cláusulas en resultante `UPDATE` y `DELETE` instrucciones para proporcionar control de simultaneidad optimista. Por ahora, deje esta casilla desactivada; Examinaremos la simultaneidad optimista con el control SqlDataSource en el siguiente tutorial.

Después de comprobar la generar `INSERT`, `UPDATE`, y `DELETE` casilla de verificación de las instrucciones, haga clic en Aceptar para volver a la pantalla Configurar instrucción Select, a continuación, haga clic en siguiente y, a continuación, finalice, para completar el Asistente para configurar orígenes de datos. Al finalizar el asistente, Visual Studio agregará BoundFields a DetailsView para el `ProductID`, `ProductName`, y `UnitPrice` columnas y un CampoCasillaVerificación para la `Discontinued` columna. En la etiqueta inteligente s DetailsView, active la opción Habilitar paginación para que el usuario al visitar esta página puede recorrer los productos. Borrar también las operaciones de asignación DetailsView `Width` y `Height` propiedades.

Observe que la etiqueta inteligente tiene las opciones Habilitar inserción, Habilitar edición y Habilitar eliminación disponibles. Esto es porque SqlDataSource contiene valores para su `InsertCommand`, `UpdateCommand`, y `DeleteCommand`, como se muestra en la siguiente sintaxis declarativa:

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/samples/sample3.aspx)]

Tenga en cuenta cómo el control SqlDataSource ha tenido valores se establecen automáticamente para su `InsertCommand`, `UpdateCommand`, y `DeleteCommand` propiedades. El conjunto de columnas que se hace referenciado en el `InsertCommand` y `UpdateCommand` propiedades se basan en los de la `SELECT` instrucción. Es decir, en lugar de tener *cada* columna productos en el `InsertCommand` y `UpdateCommand`, hay solo las columnas especificadas en el `SelectCommand` (menos `ProductID`, que se omite porque se s un [ `IDENTITY` columna](http://www.sqlteam.com/item.asp?ItemID=102), cuyo valor no puede cambiarse cuando edita y que se asigna automáticamente al insertar). Además, para cada parámetro en el `InsertCommand`, `UpdateCommand`, y `DeleteCommand` propiedades hay parámetros correspondientes en el `InsertParameters`, `UpdateParameters`, y `DeleteParameters` colecciones.

Para activar las características de modificación de datos de DetailsView s, compruebe el Habilitar inserción, Habilitar edición y las opciones Habilitar eliminación en su etiqueta inteligente. Esto agrega un CommandField con su `ShowInsertButton`, `ShowEditButton`, y `ShowDeleteButton` propiedades establecidas en `True`.

Visite la página en un explorador y tenga en cuenta la edición, eliminación y nuevos botones que se incluyen en DetailsView. Al hacer clic en el botón Editar convierte DetailsView en modo de edición, que muestra cada BoundField cuyo `ReadOnly` propiedad está establecida en `False` (valor predeterminado) como un cuadro de texto y CheckBoxField como una casilla de verificación.


[![Las operaciones de asignación DetailsView predeterminado de interfaz de edición](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image9.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image11.png)

**Figura 9**: s DetailsView de la interfaz de edición predeterminada ([haga clic aquí para ver imagen en tamaño completo](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image12.png))


De forma similar, puede eliminar el producto seleccionado o agregar un nuevo producto en el sistema. Puesto que la `InsertCommand` instrucción solo funciona con el `ProductName`, `UnitPrice`, y `Discontinued` columnas, las demás columnas tienen ya sea `NULL` o su valor predeterminado asignado por la base de datos con insert. Al igual que con el origen ObjectDataSource, si la `InsertCommand` no tiene ninguna tabla de base de datos las columnas que no t permiten `NULL` s y don t tienen un valor predeterminado, se producirá un error SQL al intentar ejecutar el `INSERT` instrucción.

> [!NOTE]
> La s DetailsView insertar y editar interfaces carecen de algún tipo de personalización o validación. Para agregar controles de validación o para personalizar las interfaces, deberá convertir la BoundFields TemplateFields. Hacer referencia a la [agregar controles de validación a las Interfaces de inserción y edición](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) y [personalizar la interfaz de modificación de datos](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) tutoriales para obtener más información.


Además, tenga en cuenta que para actualizar y eliminar, DetailsView usa el producto actual s `DataKey` valor, que solo está presente si el `DataKeyNames` se configura la propiedad. Si edita o elimina parece que no tienen ningún efecto, asegúrese de que el `DataKeyNames` se establece la propiedad.

## <a name="limitations-of-automatically-generating-sql-statements"></a>Limitaciones de generación automática de instrucciones SQL

Desde la generar `INSERT`, `UPDATE`, y `DELETE` opción extractos solo está disponible al seleccionar las columnas de una tabla, para aquellas consultas más complejas tendrá que escribir su propio `INSERT`, `UPDATE`, y `DELETE` instrucciones como hicimos en el paso 1. Normalmente, SQL `SELECT` usan instrucciones `JOIN` s para devolver datos de una o varias tablas de búsqueda para la visualización (como volver a poner el `Categories` tabla s `CategoryName` campo al mostrar información del producto). Al mismo tiempo, deseamos permitir al usuario editar, actualizar o insertar datos en la tabla principal (`Products`, en este caso).

Mientras el `INSERT`, `UPDATE`, y `DELETE` instrucciones puede especificarse manualmente, considere la siguiente sugerencia para ahorrar tiempo. Configurar inicialmente SqlDataSource para que vuelva extrae datos de únicamente la `Products` tabla. Utilice el Configurar origen de datos asistente s especificar columnas de una tabla o vista de pantalla para que se puede generar automáticamente el `INSERT`, `UPDATE`, y `DELETE` instrucciones. A continuación, después de completar al asistente, elija Configurar el SelectQuery desde la ventana Propiedades (o, como alternativa, vuelva a la opción de procedimiento almacenado o el Asistente para configurar origen de datos, pero la especificación de una instrucción SQL personalizada). A continuación, actualice el `SELECT` instrucción para incluir el `JOIN` sintaxis. Esta técnica ofrece las ventajas de ahorro de tiempo de las instrucciones SQL generadas automáticamente y permite más personalizada `SELECT` instrucción.

Otra limitación de generar automáticamente el `INSERT`, `UPDATE`, y `DELETE` instrucciones es que las columnas de la `INSERT` y `UPDATE` instrucciones se basan en las columnas devueltas por la `SELECT` instrucción. Podemos necesitamos actualizar o insertar los campos de más o menos, sin embargo. Por ejemplo, en el ejemplo del paso 2, quizás queremos tener la `UnitPrice` BoundField ser de solo lectura. En ese caso, t no debe aparecer en el `UpdateCommand`. O quizás deseamos establecer el valor de un campo de tabla que no aparece en el control GridView. Por ejemplo, al agregar un nuevo registro es posible que queramos la `QuantityPerUnit` valor establecido en la lista de tareas.

Si se requieren estas personalizaciones, deberá realizar manualmente, ya sea a través de la ventana Propiedades, especifique una instrucción SQL personalizada o la opción de procedimiento almacenado en el asistente o a través de la sintaxis declarativa.

> [!NOTE]
> Al agregar parámetros que no tienen campos correspondientes en los datos de control Web, tenga en cuenta que tendrá estos valores de parámetros que se pueden asignar valores de alguna manera. Estos valores pueden ser: codificado de forma rígida directamente en el `InsertCommand` o `UpdateCommand`; pueden proceder de algún origen predefinido (la cadena de consulta, el estado de sesión, controles Web en la página y así sucesivamente); o se puede asignar mediante programación, como hemos visto en el tutorial anterior.


## <a name="summary"></a>Resumen

En el orden de los datos de que controles Web para utilizar sus integradas de inserción, edición y eliminación de recursos, el control de origen de datos se enlazan a debe ofrecer esta funcionalidad. Para SqlDataSource, esto significa que `INSERT`, `UPDATE`, y `DELETE` instrucciones SQL deben estar asignadas a la `InsertCommand`, `UpdateCommand`, y `DeleteCommand` propiedades. Estas propiedades y las colecciones de parámetros correspondientes, pueden agregar manualmente o se genera automáticamente mediante el Asistente para configurar orígenes de datos. En este tutorial se examinan ambas técnicas.

Examinamos utiliza simultaneidad optimista con ObjectDataSource en el [implementar simultaneidad optimista](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) tutorial. El control SqlDataSource también proporciona compatibilidad con la simultaneidad optimista. Como se indicó en el paso 2, cuando se genera automáticamente el `INSERT`, `UPDATE`, y `DELETE` instrucciones, el asistente ofrece una opción de usar simultaneidad optimista. Como veremos en el siguiente tutorial, usar simultaneidad optimista con SqlDataSource modifica el `WHERE` cláusulas en el `UPDATE` y `DELETE` instrucciones para asegurarse de que los valores de las demás columnas conocemos t cambiado desde que los datos por última vez se muestra en la página.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](using-parameterized-queries-with-the-sqldatasource-vb.md)
> [Siguiente](implementing-optimistic-concurrency-with-the-sqldatasource-vb.md)
