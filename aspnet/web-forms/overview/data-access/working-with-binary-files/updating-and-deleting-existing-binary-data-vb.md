---
uid: web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-vb
title: Actualización y eliminación de los datos binarios existentes (VB) | Microsoft Docs
author: rick-anderson
description: En los tutoriales anteriores, hemos visto cómo el control GridView simplifica el proceso editar y eliminar datos de texto. En este tutorial se puede ver cómo el control GridView que también...
ms.author: riande
ms.date: 03/27/2007
ms.assetid: 3a052ced-9cf5-47b8-a400-934f0b687c26
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 9a0f9e5ebb8bc8227e869f2af085394262a31663
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828411"
---
<a name="updating-and-deleting-existing-binary-data-vb"></a>Actualización y eliminación de los datos binarios existentes (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_57_VB.exe) o [descargar PDF](updating-and-deleting-existing-binary-data-vb/_static/datatutorial57vb1.pdf)

> En los tutoriales anteriores, hemos visto cómo el control GridView simplifica el proceso editar y eliminar datos de texto. En este tutorial se muestra cómo el control GridView también permite editar y eliminar datos binarios, si esos datos binarios se guardan en la base de datos o almacenados en el sistema de archivos.


## <a name="introduction"></a>Introducción

A través de los tres últimos tutoriales se ha agregado un poco de funcionalidad para trabajar con datos binarios. Comenzamos agregando un `BrochurePath` columna a la `Categories` de tabla y actualiza la arquitectura en consecuencia. También agregamos métodos de capa de acceso a datos y capa de lógica empresarial para trabajar con las categorías existentes de la tabla s `Picture` columna, que contiene los s contenido binario de un archivo de imagen. Hemos creado las páginas web para presentar los datos binarios en un control GridView en un vínculo de descarga para el folleto, con la imagen de la categoría s se muestra en un `<img>` elemento y se ha agregado un DetailsView para permitir que los usuarios agregar una nueva categoría y cargar sus datos de catálogo y de imagen.

Todo lo que queda para su implementación es la capacidad de editar y eliminar categorías existentes, que se va a realizar en este tutorial usa la GridView s integradas edición y eliminación de características. Al editar una categoría, el usuario podrá opcionalmente, puede cargar una nueva imagen o tienen la categoría seguir usando uno existente. Obtener el catálogo, puede elegir usar el catálogo existente, para cargar un folleto de nuevo, o para indicar que la categoría ya no tiene un folleto asociado con él. Introducción s Let!

## <a name="step-1-updating-the-data-access-layer"></a>Paso 1: Actualización de la capa de acceso a datos

La capa DAL se autogenerado `Insert`, `Update`, y `Delete` métodos, pero estos métodos se generaron según el `CategoriesTableAdapter` consulta principal s, que no incluye la `Picture` columna. Por lo tanto, el `Insert` y `Update` métodos no incluyen parámetros para especificar los datos binarios de la imagen de la categoría s. Como se hizo en el [tutorial anterior](including-a-file-upload-option-when-adding-a-new-record-vb.md), debemos crear un nuevo método de TableAdapter para actualizar el `Categories` tabla al especificar los datos binarios.

Abra el conjunto de datos con tipo y, desde el diseñador, haga doble clic en el `CategoriesTableAdapter` encabezado s y elija Agregar consulta en el menú contextual para launche el TableAdapter Query Configuration Wizard. Este asistente se inicia porque nos pide cómo la consulta de TableAdapter debe tener acceso a la base de datos. Elija usar instrucciones SQL y haga clic en siguiente. El siguiente paso le pide el tipo de consulta que se genere. Desde que creamos re creando una consulta para agregar un nuevo registro a la `Categories` de tabla, elija la actualización y haga clic en siguiente.


[![Seleccione la opción de actualización](updating-and-deleting-existing-binary-data-vb/_static/image2.png)](updating-and-deleting-existing-binary-data-vb/_static/image1.png)

**Figura 1**: seleccione la opción de actualización ([haga clic aquí para ver imagen en tamaño completo](updating-and-deleting-existing-binary-data-vb/_static/image3.png))


Ahora tenemos que especificar el `UPDATE` instrucción SQL. El Asistente sugiere automáticamente un `UPDATE` instrucción correspondiente a la consulta principal de TableAdapter s (que actualiza el `CategoryName`, `Description`, y `BrochurePath` valores). Cambie la instrucción para que el `Picture` columna se incluye junto con un `@Picture` parámetro, de este modo:


[!code-sql[Main](updating-and-deleting-existing-binary-data-vb/samples/sample1.sql)]

La última pantalla del asistente nos pide al nombre del nuevo método del TableAdapter. Escriba `UpdateWithPicture` y haga clic en Finalizar.


[![Nombre de la nueva UpdateWithPicture TableAdapter (método)](updating-and-deleting-existing-binary-data-vb/_static/image5.png)](updating-and-deleting-existing-binary-data-vb/_static/image4.png)

**Figura 2**: nombre del nuevo método TableAdapter `UpdateWithPicture` ([haga clic aquí para ver imagen en tamaño completo](updating-and-deleting-existing-binary-data-vb/_static/image6.png))


## <a name="step-2-adding-the-business-logic-layer-methods"></a>Paso 2: Agregar métodos de nivel de lógica de negocios

Además de actualizar la capa DAL, deberá actualizar la capa BLL para incluir métodos para actualizar y eliminar una categoría. Estos son los métodos que se invocará desde la capa de presentación.

Para eliminar una categoría, podemos usar el `CategoriesTableAdapter` s autogenerado `Delete` método. Agregue el método siguiente a la `CategoriesBLL` clase:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample2.vb)]

Para este tutorial, s permiten crear dos métodos para actualizar una categoría: uno que espera que los datos de imagen binarios e invoca el `UpdateWithPicture` método que acabamos de agregar a la `CategoriesTableAdapter` y otro que acepta solo el `CategoryName`, `Description`y `BrochurePath`valores y utiliza `CategoriesTableAdapter` clase s autogenerado `Update` instrucción. Las razones para usar dos métodos es que en algunas circunstancias, un usuario podría actualizar la imagen de s categoría junto con sus otros campos, en el que caso el usuario tendrá que cargar la nueva imagen. A continuación, se pueden usar los datos binarios de la imagen cargada s en el `UPDATE` instrucción. En otros casos, el usuario solo puede estar interesado en actualizar, por ejemplo, el nombre y descripción. Pero si el `UPDATE` instrucción espera a que los datos binarios para el `Picture` también la columna y, después, nos d debe proporcionar dicha información resulte. Esto requeriría un viaje adicional a la base de datos para devolver los datos de imagen para el registro que se está editando. Por lo tanto, queremos que dos `UPDATE` métodos. La capa de lógica empresarial determinará cuál usar según si se suministra los datos de imagen al actualizar la categoría.

Para facilitar esto, agregue dos métodos a la `CategoriesBLL` clase ambos denominado `UpdateCategory`. La primera de ellas debe aceptar tres `String` s, una `Byte` matriz y un `Integer` como entrada parámetros; el segundo, tres `String` s y una `Integer`. El `String` son parámetros de entrada para el nombre de categoría s, descripción y ruta de acceso de archivo de folleto, la `Byte` matriz es para el contenido binario de la imagen de la categoría s y el `Integer` identifica el `CategoryID` del registro para la actualización. Tenga en cuenta que la primera sobrecarga invoca el segundo si pasa del `Byte` matriz es `Nothing`:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample3.vb)]

## <a name="step-3-copying-over-the-insert-and-view-functionality"></a>Paso 3: Copiar a través de la inserción y la funcionalidad de vista

En el [tutorial anterior](including-a-file-upload-option-when-adding-a-new-record-vb.md) hemos creado una página denominada `UploadInDetailsView.aspx` que enumera todas las categorías en un control GridView y proporciona un DetailsView para agregar nuevas categorías en el sistema. En este tutorial se extenderá el control GridView para incluir la edición y eliminación de soporte técnico. En lugar de continuar para que funcione desde `UploadInDetailsView.aspx`, s permiten colocar en su lugar los cambios de este tutorial s en el `UpdatingAndDeleting.aspx` página de la misma carpeta, `~/BinaryData`. Copie y pegue el marcado declarativo y de código de `UploadInDetailsView.aspx` a `UpdatingAndDeleting.aspx`.

Comience abriendo la `UploadInDetailsView.aspx` página. Copie toda la sintaxis declarativa en el `<asp:Content>` elemento, como se muestra en la figura 3. A continuación, abra `UpdatingAndDeleting.aspx` y pegue este marcado dentro de su `<asp:Content>` elemento. De forma similar, copie el código de la `UploadInDetailsView.aspx` página de la clase de código subyacente de s a `UpdatingAndDeleting.aspx`.


[![Copie el marcado declarativo de UploadInDetailsView.aspx](updating-and-deleting-existing-binary-data-vb/_static/image8.png)](updating-and-deleting-existing-binary-data-vb/_static/image7.png)

**Figura 3**: copiar el marcado declarativo de `UploadInDetailsView.aspx` ([haga clic aquí para ver imagen en tamaño completo](updating-and-deleting-existing-binary-data-vb/_static/image9.png))


Después de copiar en el marcado declarativo y el código, visite `UpdatingAndDeleting.aspx`. Debería ver la misma salida y tienen la misma experiencia de usuario igual que con `UploadInDetailsView.aspx` página desde el tutorial anterior.

## <a name="step-4-adding-deleting-support-to-the-objectdatasource-and-gridview"></a>Paso 4: Adición de eliminación de soporte técnico para el origen ObjectDataSource y GridView

Como se explicó en la [una visión general de insertar, actualizar y eliminar datos](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) tutorial, GridView proporciona capacidades integradas de eliminación y se pueden habilitar estas funcionalidades en el paso de una casilla de verificación si la cuadrícula s subyacente origen de datos admite la eliminación. ObjectDataSource GridView está enlazado actualmente a (`CategoriesDataSource`) no admite la eliminación.

Para solucionar este problema, haga clic en la opción de configurar origen de datos de la etiqueta inteligente de ObjectDataSource s para iniciar al asistente. La primera pantalla muestra que el ObjectDataSource está configurado para trabajar con el `CategoriesBLL` clase. Presione a siguiente. Actualmente, solo el origen ObjectDataSource s `InsertMethod` y `SelectMethod` se especifican las propiedades. Sin embargo, el asistente rellena automáticamente las listas desplegables en las fichas de UPDATE y DELETE con la `UpdateCategory` y `DeleteCategory` métodos, respectivamente. Esto es porque en el `CategoriesBLL` clase se marcan estos métodos mediante la `DataObjectMethodAttribute` como los métodos predeterminados para actualizar y eliminar.

Por ahora, establecer la lista de desplegable pestaña s UPDATE (ninguna), pero deje la lista de desplegable eliminar pestaña s establecida en `DeleteCategory`. Se tendrá que volver a este asistente en el paso 6 para agregar compatibilidad con la actualización.


[![Configurar el origen ObjectDataSource para usar el método DeleteCategory](updating-and-deleting-existing-binary-data-vb/_static/image11.png)](updating-and-deleting-existing-binary-data-vb/_static/image10.png)

**Figura 4**: configurar el origen ObjectDataSource que se usarán el `DeleteCategory` método ([haga clic aquí para ver imagen en tamaño completo](updating-and-deleting-existing-binary-data-vb/_static/image12.png))


> [!NOTE]
> Al finalizar al asistente, Visual Studio puede pedirle si desea actualizar campos y las claves, que volverá a generar los datos Web controla los campos. Elija No, porque si elige Sí, se sobrescribirá cualquier personalización de campo que se ha realizado.


ObjectDataSource ahora incluirá un valor para su `DeleteMethod` propiedad, así como un `DeleteParameter`. Recuerde que al utilizar el Asistente para especificar los métodos, Visual Studio establece la s ObjectDataSource `OldValuesParameterFormatString` propiedad `original_{0}`, que provoca problemas con la actualización y eliminación de las invocaciones de método. Por lo tanto, borre esta propiedad completamente o restablecer el valor predeterminado, `{0}`. Si necesita actualizar la memoria en esta propiedad de ObjectDataSource, consulte el [una visión general de insertar, actualizar y eliminar datos](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) tutorial.

Después de completar el asistente y corregir la `OldValuesParameterFormatString`, el marcado declarativo de ObjectDataSource s debe ser similar a lo siguiente:


[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample4.aspx)]

Después de configurar el origen ObjectDataSource, agregar capacidades de eliminación en el control GridView activando la casilla Habilitar eliminación de la etiqueta inteligente de s GridView. Esto agregará un CommandField a GridView cuyo `ShowDeleteButton` propiedad está establecida en `True`.


[![Habilitar la compatibilidad para la eliminación en el control GridView](updating-and-deleting-existing-binary-data-vb/_static/image14.png)](updating-and-deleting-existing-binary-data-vb/_static/image13.png)

**Figura 5**: habilitar la compatibilidad con la eliminación en el control GridView ([haga clic aquí para ver imagen en tamaño completo](updating-and-deleting-existing-binary-data-vb/_static/image15.png))


Dedique un momento para probar la funcionalidad de eliminación. Hay una clave externa entre la `Products` tabla s `CategoryID` y `Categories` tabla s `CategoryID`, por lo que obtendrá una excepción de infracción de restricción de clave externa si se intenta eliminar cualquiera de las primeras ocho categorías. Para probar esta funcionalidad horizontalmente, agregue una nueva categoría, que proporciona un folleto y la imagen. La categoría de pruebas, se muestra en la figura 6, incluye un archivo de catálogo de prueba denominado `Test.pdf` y una imagen de prueba. Figura 7 muestra el control GridView una vez agregada la categoría de pruebas.


[![Agregar una categoría de pruebas con una imagen y el folleto](updating-and-deleting-existing-binary-data-vb/_static/image17.png)](updating-and-deleting-existing-binary-data-vb/_static/image16.png)

**Figura 6**: agregar una categoría de pruebas con una imagen y el folleto ([haga clic aquí para ver imagen en tamaño completo](updating-and-deleting-existing-binary-data-vb/_static/image18.png))


[![Después de insertar la categoría de pruebas, se muestra en el control GridView](updating-and-deleting-existing-binary-data-vb/_static/image20.png)](updating-and-deleting-existing-binary-data-vb/_static/image19.png)

**Figura 7**: después de insertar la categoría de pruebas, se muestra en el control GridView ([haga clic aquí para ver imagen en tamaño completo](updating-and-deleting-existing-binary-data-vb/_static/image21.png))


En Visual Studio, actualice el Explorador de soluciones. Ahora debería ver un archivo nuevo en el `~/Brochures` carpeta, `Test.pdf` (consulte la figura 8).

A continuación, haga clic en el vínculo Eliminar en la fila de la categoría de pruebas, provoca que la página de devolución de datos y el `CategoriesBLL` clase s `DeleteCategory` método de activación. Esto invocará la s DAL `Delete` método, que lo adecuado `DELETE` instrucción para enviarse a la base de datos. A continuación, se vuelve a enlazar los datos en el control GridView y el marcado que se envía al cliente con la categoría de pruebas ya no está presente.

Mientras el flujo de trabajo de eliminación quitó correctamente el registro de la categoría de pruebas desde la `Categories` tabla, no quitó el archivo de catálogo del sistema de archivos de s de servidor web. Actualice el Explorador de soluciones y verá que `Test.pdf` siga estando en el `~/Brochures` carpeta.


![No se eliminó el archivo Test.pdf desde el sistema de archivos de s de servidor Web](updating-and-deleting-existing-binary-data-vb/_static/image1.gif)

**Figura 8**: el `Test.pdf` no se eliminó el archivo del sistema de archivos de s de servidor Web


## <a name="step-5-removing-the-deleted-category-s-brochure-file"></a>Paso 5: Quitar el archivo de folleto s categoría eliminada

Una de las desventajas de almacenar datos binarios externos a la base de datos es que se deben realizar pasos adicionales para limpiar estos archivos cuando se elimina el registro de base de datos asociada. El control GridView y ObjectDataSource proporcionan los eventos que activan tanto antes como después de haber realizado el comando de eliminación. Realmente es necesario crear controladores de eventos para los eventos anteriores y posteriores a la acción. Antes de la `Categories` se elimina el registro es necesario determinar su ruta de acceso del archivo s PDF, pero no queremos t desea eliminar el archivo PDF antes de elimina la categoría en el caso hay alguna excepción y no se elimina la categoría.

La s GridView [ `RowDeleting` eventos](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx) se desencadena antes de que se ha invocado el comando de eliminación de ObjectDataSource s, mientras su [ `RowDeleted` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleted.aspx) se activa tras. Crear controladores de eventos para estos dos eventos con el código siguiente:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample5.vb)]

En el `RowDeleting` controlador de eventos, el `CategoryID` de la fila que se va a eliminar se obtiene de GridView s `DataKeys` colección, que se puede acceder en este controlador de eventos a través de la `e.Keys` colección. A continuación, el `CategoriesBLL` clase s `GetCategoryByCategoryID(categoryID)` se invoca para devolver información acerca del registro que se va a eliminar. Si el valor devuelto `CategoriesDataRow` tiene el objeto que no es`NULL``BrochurePath` valor, a continuación, se almacena en la variable de página `deletedCategorysPdfPath` para que se puede eliminar el archivo en el `RowDeleted` controlador de eventos.

> [!NOTE]
> En lugar de recuperar el `BrochurePath` los detalles de la `Categories` registrar que se va a eliminar en el `RowDeleting` controlador de eventos, podríamos haber también agregamos la `BrochurePath` al s GridView `DataKeyNames` propiedad y obtener acceso el valor del registro s a través de la `e.Keys` colección. Si lo hace, podría ligeramente aumentar el tamaño del estado de vista GridView s, pero podría reducir la cantidad de código necesario y guardar una carrera en la base de datos.


Después de ObjectDataSource se ha invocado el comando delete subyacente de s, la s GridView `RowDeleted` se activa de controlador de eventos. Si no había ninguna excepción al eliminar los datos y hay un valor para `deletedCategorysPdfPath`, a continuación, el PDF se elimina del sistema de archivos. Tenga en cuenta que este código adicional no es necesario para limpiar los datos binarios de categoría s asociados a su imagen. S porque los datos de imagen se almacenan directamente en la base de datos, eliminando así la `Categories` fila elimina también esos datos de imagen de la categoría s.

Después de agregar los controladores de eventos de dos, vuelva a ejecutar este caso de prueba. Al eliminar la categoría, también se elimina su PDF asociado.

Actualizar un registro s asociados los datos binarios existentes proporciona algunos desafíos interesantes. El resto de este tutorial se adentra en Agregar capacidades de actualización para el catálogo y la imagen. Paso 6 explora las técnicas para actualizar la información de folleto mientras paso 7 examina la actualización de la imagen.

## <a name="step-6-updating-a-category-s-brochure"></a>Paso 6: Actualización de un folleto de s de categoría

Como se describe en el tutorial de una visión general de insertar, actualizar y eliminar datos, el control GridView ofrece integrado compatibilidad de edición de nivel de fila que puede ser implementada por el "Tick" de una casilla de verificación si su origen de datos subyacente está configurado correctamente. Actualmente, el `CategoriesDataSource` ObjectDataSource aún no está configurado para incluir la actualización de soporte técnico, por lo que permiten s que, en Agregar.

Haga clic en el vínculo Configurar origen de datos desde el Asistente para la s de ObjectDataSource y continúe con el segundo paso. Debido la `DataObjectMethodAttribute` utilizados en `CategoriesBLL`, la lista desplegable de actualización se rellena automáticamente con el `UpdateCategory` sobrecarga que acepta cuatro parámetros de entrada (para todas las columnas pero `Picture`). Puede cambiarlo para que use la sobrecarga con cinco parámetros.


[![Configurar el origen ObjectDataSource para usar el método UpdateCategory que incluya un parámetro de imagen](updating-and-deleting-existing-binary-data-vb/_static/image23.png)](updating-and-deleting-existing-binary-data-vb/_static/image22.png)

**Figura 9**: configurar el origen ObjectDataSource que se usarán el `UpdateCategory` método que incluye un parámetro para `Picture` ([haga clic aquí para ver imagen en tamaño completo](updating-and-deleting-existing-binary-data-vb/_static/image24.png))


ObjectDataSource ahora incluirá un valor para su `UpdateMethod` propiedad, así como la correspondiente `UpdateParameter` s. Como se indicó en el paso 4, Visual Studio establece la s ObjectDataSource `OldValuesParameterFormatString` propiedad `original_{0}` cuando se usa el Asistente para configurar orígenes de datos. Esto causar problemas con la actualización y eliminará las invocaciones de método. Por lo tanto, borre esta propiedad completamente o restablecer el valor predeterminado, `{0}`.

Después de completar el asistente y corregir la `OldValuesParameterFormatString`, el marcado declarativo de ObjectDataSource s debería ser similar al siguiente:


[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample6.aspx)]

Para activar las características de edición de GridView s integradas, active la opción Habilitar edición de la etiqueta inteligente de s GridView. Esto establecerá la s CommandField `ShowEditButton` propiedad `True`, lo que la adición de un botón de edición (y actualizar y cancelar los botones de la fila que se está editando).


[![Configurar el control GridView a la edición de soporte técnico](updating-and-deleting-existing-binary-data-vb/_static/image26.png)](updating-and-deleting-existing-binary-data-vb/_static/image25.png)

**Figura 10**: configurar el control GridView a la edición de soporte técnico ([haga clic aquí para ver imagen en tamaño completo](updating-and-deleting-existing-binary-data-vb/_static/image27.png))


Visite la página a través de un explorador y haga clic en uno de la fila s botones de edición. El `CategoryName` y `Description` BoundFields se representan como cuadros de texto. El `BrochurePath` TemplateField le falta un `EditItemTemplate`, por lo que sigue mostrando su `ItemTemplate` un vínculo al folleto. El `Picture` ImageField representa como un cuadro de texto cuya `Text` propiedad se asigna el valor de la s ImageField `DataImageUrlField` valor, en este caso `CategoryID`.


[![El control GridView no tiene una interfaz de edición para BrochurePath](updating-and-deleting-existing-binary-data-vb/_static/image29.png)](updating-and-deleting-existing-binary-data-vb/_static/image28.png)

**Figura 11**: GridView carece de una interfaz de edición para `BrochurePath` ([haga clic aquí para ver imagen en tamaño completo](updating-and-deleting-existing-binary-data-vb/_static/image30.png))


## <a name="customizing-thebrochurepaths-editing-interface"></a>Personalizar el`BrochurePath`s interfaz de edición

Es necesario crear una interfaz de edición para el `BrochurePath` TemplateField, que permite al usuario cualquiera:

- Deje el folleto categoría s como-es,
- Actualizar el folleto categoría s mediante la carga de un folleto de nuevo, o
- Quitar por completo el folleto categoría s (en el caso de que la categoría ya no tiene un catálogo de asociado).

También necesitamos actualizar la `Picture` ImageField interfaz de edición de s, pero nos pondremos en esto en el paso 7.

En la etiqueta inteligente de GridView s, haga clic en el vínculo Editar plantillas y seleccione el `BrochurePath` TemplateField s `EditItemTemplate` en la lista desplegable. Agregar un control RadioButtonList Web a esta plantilla, estableciendo su `ID` propiedad `BrochureOptions` y su `AutoPostBack` propiedad `True`. En la ventana Propiedades, haga clic en el botón de puntos suspensivos en el `Items` propiedad, que se abrirá el `ListItem` Editor de la colección. Agregue las tres opciones siguientes con `Value` s 1, 2 y 3, respectivamente:

- Usar el catálogo actual
- Quitar el catálogo actual
- Cargar nuevo folleto

Configurar el primer `ListItem` s `Selected` propiedad `True`.


![Agregar tres ListItems a RadioButtonList](updating-and-deleting-existing-binary-data-vb/_static/image2.gif)

**Figura 12**: agregue tres `ListItem` s a RadioButtonList


Bajo RadioButtonList, agregue un control FileUpload denominado `BrochureUpload`. Establezca su `Visible` propiedad `False`.


[![Agregar un RadioButtonList y Control FileUpload EditItemTemplate](updating-and-deleting-existing-binary-data-vb/_static/image32.png)](updating-and-deleting-existing-binary-data-vb/_static/image31.png)

**Figura 13**: agregar un RadioButtonList y Control FileUpload para el `EditItemTemplate` ([haga clic aquí para ver imagen en tamaño completo](updating-and-deleting-existing-binary-data-vb/_static/image33.png))


Este RadioButtonList proporciona las tres opciones para el usuario. La idea es que se mostrará el control FileUpload únicamente si se selecciona la última opción, la carga de nuevo el folleto. Para ello, cree un controlador de eventos para el s RadioButtonList `SelectedIndexChanged` eventos y agregue el código siguiente:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample7.vb)]

Puesto que los controles RadioButtonList y FileUpload están dentro de una plantilla, tenemos que escribir un fragmento de código para estos controles de acceso mediante programación. El `SelectedIndexChanged` controlador de eventos se pasa una referencia de RadioButtonList en el `sender` parámetro de entrada. Para obtener el control FileUpload, debemos obtener el elemento primario s de RadioButtonList control y use la `FindControl("controlID")` método a partir de ahí. Una vez que tenemos una referencia a los controles de la FileUpload y RadioButtonList, el FileUpload controlar s `Visible` propiedad está establecida en `True` solo si la s RadioButtonList `SelectedValue` es igual a 3, que es el `Value` para el folleto de carga nueva `ListItem`.

Con este código en su lugar, tómese un momento para probar la interfaz de edición. Haga clic en el botón Editar para una fila. Inicialmente, se debe seleccionar la opción de folleto actual de uso. Cambiar el índice seleccionado, produce un postback. Si se selecciona la tercera opción, se muestra el control FileUpload, en caso contrario, está oculta. Figura 14 se muestra la interfaz de edición cuando primero se hace clic en el botón Editar; Figura 15 muestra la interfaz después de selecciona la opción de folleto de nueva carga.


[![Inicialmente, el folleto actual de uso que está seleccionada](updating-and-deleting-existing-binary-data-vb/_static/image35.png)](updating-and-deleting-existing-binary-data-vb/_static/image34.png)

**Figura 14**: inicialmente, el folleto actual de uso está seleccionada ([haga clic aquí para ver imagen en tamaño completo](updating-and-deleting-existing-binary-data-vb/_static/image36.png))


[![Elegir el folleto de carga nueva opción muestra el Control FileUpload](updating-and-deleting-existing-binary-data-vb/_static/image38.png)](updating-and-deleting-existing-binary-data-vb/_static/image37.png)

**Figura 15**: elegir el folleto de carga nueva opción muestra el Control FileUpload ([haga clic aquí para ver imagen en tamaño completo](updating-and-deleting-existing-binary-data-vb/_static/image39.png))


## <a name="saving-the-brochure-file-and-updating-thebrochurepathcolumn"></a>Guardando el folleto de archivo y actualice la`BrochurePath`columna

Cuando se hace clic en el botón de actualización de GridView s, su `RowUpdating` desencadena el evento. Se invoca el comando de actualización de s ObjectDataSource y, a continuación, la s GridView `RowUpdated` desencadena el evento. Al igual que con el flujo de trabajo de eliminación, necesitamos crear controladores de eventos para estos dos eventos. En el `RowUpdating` controlador de eventos, necesitamos determinar qué acción realizar según la `SelectedValue` de la `BrochureOptions` RadioButtonList:

- Si el `SelectedValue` es 1, que queremos seguir usando el mismo `BrochurePath` configuración. Por lo tanto, es necesario establecer la s ObjectDataSource `brochurePath` parámetro existente `BrochurePath` valor del registro que se está actualizando. Las operaciones de asignación ObjectDataSource `brochurePath` puede establecerse mediante el parámetro `e.NewValues["brochurePath"] = value`.
- Si el `SelectedValue` es 2 y, después, queremos establecer el registro s `BrochurePath` valor `NULL`. Esto puede realizarse estableciendo el s ObjectDataSource `brochurePath` parámetro `Nothing`, que da como resultado una base de datos `NULL` que se usan en el `UPDATE` instrucción. Si hay un archivo de catálogo existente que se va a quitar, se debe eliminar el archivo existente. Sin embargo, solo queremos hacer esto si la actualización se complete sin producir una excepción.
- Si el `SelectedValue` es 3, entonces queremos asegurarse de que el usuario ha cargado un archivo PDF y, a continuación, guardarlo en el sistema de archivos y actualizar el registro de s `BrochurePath` valor de la columna. Además, si hay un archivo de catálogo existente que se va a reemplazar, se debe eliminar el archivo anterior. Sin embargo, solo queremos hacer esto si la actualización se complete sin producir una excepción.

Los pasos necesarios para completar cuando la s RadioButtonList `SelectedValue` es 3 son casi idénticas a las utilizadas por las operaciones de asignación DetailsView `ItemInserting` controlador de eventos. Este controlador de eventos se ejecuta cuando se agrega un nuevo registro de categoría desde el control DetailsView se ha agregado en el [tutorial anterior](including-a-file-upload-option-when-adding-a-new-record-vb.md). Por lo tanto, corresponde a nosotros para refactorizar esta funcionalidad horizontalmente en métodos independientes. En concreto, me cambié a probar la funcionalidad común a dos métodos:

- `ProcessBrochureUpload(FileUpload, out bool)` acepta como entrada una instancia del control FileUpload y un valor de salida booleano que especifica si debe continuar la operación de eliminación o edición o si debe cancelarse debido a un error de validación. Este método devuelve la ruta de acceso al archivo guardado o `null` si no se guardó ningún archivo.
- `DeleteRememberedBrochurePath` Elimina el archivo especificado por la ruta de acceso en la variable de página `deletedCategorysPdfPath` si `deletedCategorysPdfPath` no `null`.

El código para estos dos métodos sigue. Tenga en cuenta la similitud entre `ProcessBrochureUpload` y la s DetailsView `ItemInserting` controlador de eventos desde el tutorial anterior. En este tutorial, he actualizado los controladores de eventos de s DetailsView para utilizar estos nuevos métodos. Descargue el código asociado con este tutorial para ver las modificaciones en los controladores de eventos de s DetailsView.


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample8.vb)]

Las operaciones de asignación GridView `RowUpdating` y `RowUpdated` usar controladores de eventos el `ProcessBrochureUpload` y `DeleteRememberedBrochurePath` métodos, como se muestra en el código siguiente:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample9.vb)]

Tenga en cuenta cómo el `RowUpdating` controlador de eventos usa una serie de instrucciones condicionales para realizar las acciones adecuadas según la `BrochureOptions` RadioButtonList s `SelectedValue` valor de propiedad.

Con este código en su lugar, puede editar una categoría y tiene usar su folleto actual, no usar ningún catálogo o cargar uno nuevo. Continúe y pruébelo. Establecer puntos de interrupción el `RowUpdating` y `RowUpdated` controladores de eventos para hacerse una idea del flujo de trabajo.

## <a name="step-7-uploading-a-new-picture"></a>Paso 7: Cargar una nueva imagen

El `Picture` s ImageField edición interfaz representa como un cuadro de texto que se rellena con el valor de su `DataImageUrlField` propiedad. Durante el flujo de trabajo de edición, el control GridView pasa un parámetro a ObjectDataSource con el nombre del parámetro s el valor de la s ImageField `DataImageUrlField` el valor especificado en el cuadro de texto en la interfaz de edición de valor de propiedad y el parámetro s. Este comportamiento es adecuado cuando se guarda la imagen como un archivo en el sistema de archivos y el `DataImageUrlField` contiene la dirección URL completa de la imagen. Con tales circunstancias, la interfaz de edición muestra la dirección URL de imagen s en el cuadro de texto, que el usuario puede cambiar y ha guardado en la base de datos. Concedido, este tipo t predeterminada interfaz permite al usuario cargar una imagen nueva, pero les permiten cambiar la dirección URL de la imagen desde el valor actual a otro. Para este tutorial, sin embargo, el valor predeterminado de la s ImageField interfaz de edición no es suficiente porque la `Picture` datos binarios se almacenan directamente en la base de datos y el `DataImageUrlField` propiedad contiene solamente el `CategoryID`.

Para entender mejor lo que sucede en nuestro tutorial cuando un usuario edita una fila con un ImageField, considere el siguiente ejemplo: un usuario edita una fila con `CategoryID` 10, provocando la `Picture` ImageField para representar como un cuadro de texto con el valor 10. Imagine que el usuario cambia el valor de este cuadro de texto en 50 y hace clic en el botón Actualizar. Se produce un postback y el control GridView crea inicialmente un parámetro denominado `CategoryID` con el valor 50. Sin embargo, antes de que el control GridView envíe este parámetro (y el `CategoryName` y `Description` parámetros), agrega los valores de la `DataKeys` colección. Por lo tanto, sobrescribe la `CategoryID` parámetro con la fila s subyacente actual `CategoryID` valor 10. En resumen, la s ImageField interfaz de edición no tiene ningún efecto en el flujo de trabajo de edición para este tutorial porque los nombres de las operaciones de asignación ImageField `DataImageUrlField` propiedad y la cuadrícula s `DataKey` valor son iguales.

Aunque ImageField facilita el mostrar una imagen basada en la base de datos, no queremos t desea proporcionar un cuadro de texto en la interfaz de edición. En su lugar, desea ofrecer un control FileUpload que el usuario final puede usar para cambiar la imagen de s de la categoría. A diferencia de la `BrochurePath` valor para estos tutoriales se ve decidido requerir que cada categoría debe tener una imagen. Por lo tanto, no queremos t necesario para permitir al usuario indicar que no hay ninguna imagen asociada, el usuario puede cargar una nueva imagen o dejar la imagen actual como-es.

Para personalizar la interfaz de edición de s ImageField, es necesario convertirlas en TemplateField. En la etiqueta inteligente de GridView s, haga clic en el vínculo Editar columnas y haga clic en la función Convert este campo en un vínculo TemplateField ImageField.


![Convertir ImageField en TemplateField](updating-and-deleting-existing-binary-data-vb/_static/image3.gif)

**Figura 16**: convertir ImageField en TemplateField


Convertir ImageField en TemplateField de esta manera, genera TemplateField con dos plantillas. Como se muestra en la siguiente sintaxis declarativa, el `ItemTemplate` contiene una imagen de Web control cuya `ImageUrl` propiedad se asigna mediante la sintaxis de enlace de datos en función de las operaciones de asignación ImageField `DataImageUrlField` y `DataImageUrlFormatString` propiedades. El `EditItemTemplate` contiene un cuadro de texto cuyo `Text` propiedad se enlaza al valor especificado por el `DataImageUrlField` propiedad.


[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample10.aspx)]

Es necesario actualizar el `EditItemTemplate` para usar un control FileUpload. Desde la etiqueta inteligente s haga clic en Editar plantillas de GridView vincular y, a continuación, seleccione el `Picture` TemplateField s `EditItemTemplate` en la lista desplegable. En la plantilla debería ver un cuadro de texto quitar esto. A continuación, arrastre un control FileUpload desde el cuadro de herramientas en la plantilla, estableciendo su `ID` a `PictureUpload`. Agregue también el texto para cambiar la imagen de la categoría s, especifique una nueva imagen. Para mantener la imagen de la categoría s las mismas, deje el campo vacío a la plantilla.


[![Agregar un Control FileUpload a EditItemTemplate](updating-and-deleting-existing-binary-data-vb/_static/image41.png)](updating-and-deleting-existing-binary-data-vb/_static/image40.png)

**Figura 17**: agregar un Control FileUpload el `EditItemTemplate` ([haga clic aquí para ver imagen en tamaño completo](updating-and-deleting-existing-binary-data-vb/_static/image42.png))


Después de personalizar la interfaz de edición, ver su progreso en un explorador. Al ver una fila en modo de solo lectura, se muestra la imagen de s categoría como era antes, pero al hacer clic en el botón de edición, la columna de imagen representa como texto con un control FileUpload.


[![La interfaz de edición incluye un Control FileUpload](updating-and-deleting-existing-binary-data-vb/_static/image44.png)](updating-and-deleting-existing-binary-data-vb/_static/image43.png)

**Figura 18**: la interfaz de edición incluye un Control FileUpload ([haga clic aquí para ver imagen en tamaño completo](updating-and-deleting-existing-binary-data-vb/_static/image45.png))


Recuerde que el ObjectDataSource está configurado para llamar a la `CategoriesBLL` clase s `UpdateCategory` método que acepta como entrada los datos binarios de la imagen como un `Byte` matriz. Si esta matriz es `Nothing`, sin embargo, alternativo `UpdateCategory` sobrecarga se llama, qué problemas el `UPDATE` instrucción SQL que no modifica la `Picture` columna, por tanto, dejará la categoría s actual imagen intactos. Por lo tanto, en la s GridView `RowUpdating` controlador de eventos se debe hacer referencia mediante programación el `PictureUpload` FileUpload controlar y determinar si se ha cargado un archivo. Si uno no se cargó, hacemos *no* desea especificar un valor para el `picture` parámetro. Por otro lado, si se ha cargado un archivo en el `PictureUpload` control FileUpload, queremos asegurarnos de que es un archivo JPG. Si lo es, a continuación, podemos enviar su contenido binario a ObjectDataSource a través de la `picture` parámetro.

Al igual que con el código usado en el paso 6, gran parte del código necesario aquí ya existe en la s DetailsView `ItemInserting` controlador de eventos. Por lo tanto, se ve refactorizó la funcionalidad común en un nuevo método `ValidPictureUpload`y actualiza el `ItemInserting` controlador de eventos al utilizar este método.

Agregue el código siguiente al principio de la s GridView `RowUpdating` controlador de eventos. Lo importante que este código preceder el código que guarda el archivo de catálogo, ya que no queremos t desea guardar el folleto para el sistema de archivos del servidor s web si se carga un archivo de imagen no válido.


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample11.vb)]

El `ValidPictureUpload(FileUpload)` método toma un control FileUpload como su único parámetro de entrada y comprueba la extensión de archivo cargado s para asegurarse de que el archivo cargado no es un JPG; solo se llama si se carga un archivo de imagen. Si se ha cargado ningún archivo, entonces es no establece el parámetro de imagen y, por tanto, utiliza su valor predeterminado de `Nothing`. Si se ha cargado una imagen y `ValidPictureUpload` devuelve `True`, `picture` parámetro se asigna a los datos binarios de la imagen cargada; si el método devuelve `False`, se cancela el flujo de trabajo de actualización y sale del controlador de eventos.

El `ValidPictureUpload(FileUpload)` código del método, que se ha refactorizado de s DetailsView `ItemInserting` sigue el controlador de eventos:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample12.vb)]

## <a name="step-8-replacing-the-original-categories-pictures-with-jpgs"></a>Paso 8: Reemplazar las imágenes originales de las categorías con jpg

Recuerde que las imágenes originales de ocho categorías son archivos de mapa de bits que se encapsulan en un encabezado OLE. Ahora que hemos agregado la capacidad de editar una imagen existente de registro s, dedique un momento para reemplazar estos mapas de bits jpg. Si desea seguir usando las imágenes de la categoría actual, se puede convertir a jpg realizando los pasos siguientes:

1. Guardar las imágenes de mapa de bits en el disco duro. Visite el `UpdatingAndDeleting.aspx` de página en el explorador y para cada una de las ocho primeras categorías, haga doble clic en la imagen y elegir guardar la imagen.
2. Abra la imagen en el editor de imágenes de elección. Puede usar Microsoft Paint, por ejemplo.
3. Guarde el mapa de bits como una imagen JPG.
4. Actualizar la imagen de la categoría s a través de la interfaz de edición, mediante el archivo JPG.

Después de editar una categoría y cargar la imagen JPG, la imagen no se representará en el explorador porque el `DisplayCategoryPicture.aspx` página se haya eliminado los primeros bytes 78 desde las imágenes de las primeras ocho categorías. Solucionar este problema quitando el código que realiza la eliminación de encabezado OLE. Una vez hecho esto, el `DisplayCategoryPicture.aspx``Page_Load` controlador de eventos debe tener solo el código siguiente:


[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample13.vb)]

> [!NOTE]
> El `UpdatingAndDeleting.aspx` página s insertar y editar interfaces podría usar algo más de trabajo. El `CategoryName` y `Description` BoundFields en GridView y DetailsView se debe convertir en TemplateFields. Puesto que `CategoryName` no permite `NULL` valores, se debe agregar un control RequiredFieldValidator. Y el `Description` probablemente se debe convertir el cuadro de texto en un cuadro de texto multilínea. Dejar estos toques finales como un ejercicio para usted.


## <a name="summary"></a>Resumen

En este tutorial se complete nuestra cómo trabajar con datos binarios. En este tutorial y los tres anteriores, vimos cómo binarios datos pueden almacenarse en el sistema de archivos o directamente dentro de la base de datos. Un usuario proporciona datos binarios en el sistema, seleccione un archivo desde su unidad de disco duro y cargarlo en el servidor web, donde puede ser almacenado en el sistema de archivos o insertado en la base de datos. ASP.NET 2.0 incluye un control FileUpload que hace que proporciona esta interfaz tan fácil como arrastrar y colocar. Sin embargo, como se indicó en el [cargar archivos](uploading-files-vb.md) tutorial, el control FileUpload sólo es ideal para cargas de archivos relativamente pequeño, lo ideal es que no superan un megabyte. También hemos examinado cómo asociar los datos cargados con el modelo de datos subyacente, así como la forma de editar y eliminar los datos binarios de los registros existentes.

El siguiente conjunto de tutoriales explora diversas técnicas de almacenamiento en caché. Almacenamiento en caché proporciona un medio para mejorar una aplicación s tomando los resultados de operaciones costosas y almacenarlos en una ubicación que se puede acceder más rápidamente el rendimiento general.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Revisor de clientes potencial para este tutorial era Teresa Murphy. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](including-a-file-upload-option-when-adding-a-new-record-vb.md)
