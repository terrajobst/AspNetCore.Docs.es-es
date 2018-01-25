---
uid: web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-cs
title: Actualizar y eliminar los datos binarios existentes (C#) | Documentos de Microsoft
author: rick-anderson
description: "En los tutoriales anteriores hemos visto cómo el control GridView simplifica el proceso modificar y eliminar datos de texto. En este tutorial se muestra cómo el control GridView que también..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: 35798f21-1606-434b-83f8-30166906ef49
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-cs
msc.type: authoredcontent
ms.openlocfilehash: f2fca1e91720fba0215e12b1a1894a3a31e86b5c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="updating-and-deleting-existing-binary-data-c"></a>Actualizar y eliminar los datos binarios existentes (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_57_CS.exe) o [descarga de PDF](updating-and-deleting-existing-binary-data-cs/_static/datatutorial57cs1.pdf)

> En los tutoriales anteriores hemos visto cómo el control GridView simplifica el proceso modificar y eliminar datos de texto. En este tutorial se muestra cómo el control GridView también hace posible modificar y eliminar datos binarios, si esos datos binarios se guardan en la base de datos o almacenados en el sistema de archivos.


## <a name="introduction"></a>Introducción

En los últimos tres tutoriales se ha agregado un poco de funcionalidad para trabajar con datos binarios. Hemos iniciado mediante la adición de un `BrochurePath` columna a la `Categories` de tabla y actualiza la arquitectura según corresponda. También hemos agregado capa de acceso a datos y capa de lógica empresarial métodos para trabajar con las categorías existentes de la tabla s `Picture` columna, que contiene las operaciones de asignación contenido binario de un archivo de imagen. Hemos creado páginas web para presentar los datos binarios en un control GridView un vínculo de descarga para el folleto, con la imagen de la categoría s se muestra en un `<img>` elemento y se ha agregado un DetailsView para permitir a los usuarios agregar una nueva categoría y cargar sus datos folleto e incluye una descripción.

Todo lo que queda para que se implementen es la capacidad de editar y eliminar categorías existentes, que se podrá realizar en este tutorial con GridView s integradas edición y eliminación de características. Al editar una categoría, el usuario podrá cargar una nueva imagen, opcionalmente, o tienen la categoría seguir usando el ya existente. Si desea obtener el catálogo, puede elegir usar el folleto existente, para cargar un folleto de nuevo, o para indicar que la categoría ya no tiene un folleto asociado a él. Permiten s empiece a trabajar.

## <a name="step-1-updating-the-data-access-layer"></a>Paso 1: Actualizar la capa de acceso a datos

La capa DAL se autogenerada `Insert`, `Update`, y `Delete` métodos, pero estos métodos se generan basándose en la `CategoriesTableAdapter` consulta principal s, que no incluye la `Picture` columna. Por lo tanto, la `Insert` y `Update` métodos no incluyen parámetros para especificar los datos binarios de la imagen de la categoría s. Al igual que hicimos la [tutorial anterior](including-a-file-upload-option-when-adding-a-new-record-cs.md), debemos crear un nuevo método de TableAdapter para actualizar la `Categories` tabla al especificar los datos binarios.

Abra el conjunto de datos con tipo y, en el diseñador, haga doble clic en el `CategoriesTableAdapter` encabezado s y elija Agregar consulta en el menú contextual para launche el Asistente para configuración de consultas de TableAdapter. Este asistente se inicia mediante nos pide que forma la consulta de TableAdapter debe tener acceso a la base de datos. Elija usar instrucciones SQL y haga clic en siguiente. El paso siguiente solicita el tipo de consulta que se genere. Desde que se re creando una consulta para agregar un nuevo registro a la `Categories` de la tabla, elija la actualización y haga clic en siguiente.


[![Seleccione la opción de actualización](updating-and-deleting-existing-binary-data-cs/_static/image1.gif)](updating-and-deleting-existing-binary-data-cs/_static/image1.png)

**Figura 1**: seleccione la opción de actualización ([haga clic aquí para ver la imagen a tamaño completo](updating-and-deleting-existing-binary-data-cs/_static/image2.png))


Ahora debemos especificar la `UPDATE` instrucción SQL. El Asistente sugiere automáticamente un `UPDATE` instrucción correspondiente a la consulta principal de TableAdapter s (uno que actualiza el `CategoryName`, `Description`, y `BrochurePath` valores). Cambie la instrucción para que la `Picture` las columnas se incluyen junto con un `@Picture` parámetro, de este modo:


[!code-sql[Main](updating-and-deleting-existing-binary-data-cs/samples/sample1.sql)]

La pantalla final del asistente le pide que el nombre del nuevo método de TableAdapter. Escriba `UpdateWithPicture` y haga clic en Finalizar.


[![Nombre de la nueva UpdateWithPicture TableAdapter (método)](updating-and-deleting-existing-binary-data-cs/_static/image2.gif)](updating-and-deleting-existing-binary-data-cs/_static/image3.png)

**Figura 2**: nombre del nuevo método de TableAdapter `UpdateWithPicture` ([haga clic aquí para ver la imagen a tamaño completo](updating-and-deleting-existing-binary-data-cs/_static/image4.png))


## <a name="step-2-adding-the-business-logic-layer-methods"></a>Paso 2: Agregar los métodos de capa de lógica de negocios

Además de actualizar la capa DAL, deberá actualizar el BLL para incluir métodos para actualizar y eliminar una categoría. Éstos son los métodos que se invocará en el nivel de presentación.

Para eliminar una categoría, podemos usar la `CategoriesTableAdapter` s autogenerada `Delete` método. Agregue el método siguiente a la `CategoriesBLL` clase:


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample2.cs)]

Para este tutorial, s permiten crear dos métodos para actualizar una categoría: uno que espera que los datos de imagen binarios e invoca el `UpdateWithPicture` método hemos agregado a la `CategoriesTableAdapter` y otro que acepta simplemente el `CategoryName`, `Description`y `BrochurePath`valores y utiliza `CategoriesTableAdapter` clase s autogenerada `Update` instrucción. La lógica subyacente utilizando dos métodos es que, en algunas circunstancias, un usuario podría interesarle actualizar la imagen de s categoría junto con sus otros campos, en el que caso, el usuario tendrá que cargar la nueva imagen. Los datos binarios de la imagen cargada s, a continuación, pueden usarse en el `UPDATE` instrucción. En otros casos, el usuario sólo podría estar interesado en actualizar, diga el nombre y descripción. Pero si el `UPDATE` instrucción espera los datos binarios de la `Picture` columna y, a continuación, se necesita d proporcionar dicha información resulte. Esto requeriría un viaje adicional a la base de datos para devolver los datos de imagen para el registro que se está editando. Por lo tanto, queremos dos `UPDATE` métodos. La capa de lógica empresarial determinar cuál usar según si se suministra los datos de imagen al actualizar la categoría.

Para facilitar esto, agregue dos métodos para la `CategoriesBLL` (clase), ambos con el nombre `UpdateCategory`. La primera de ellas debe aceptar tres `string` s, un `byte` matriz y un `int` como entrada parámetros; el segundo, solo tres `string` s y un `int`. El `string` son parámetros de entrada para el nombre de la categoría s, la descripción y la ruta de acceso de archivo de folleto, la `byte` matriz es para el contenido binario de la imagen de la categoría s y el `int` identifica el `CategoryID` del registro que desea actualizar. Observe que la primera sobrecarga invoca el segundo si pasa del `byte` matriz es `null`:


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample3.cs)]

## <a name="step-3-copying-over-the-insert-and-view-functionality"></a>Paso 3: Copiar sobre la inserción y la funcionalidad de vista

En el [tutorial anterior](including-a-file-upload-option-when-adding-a-new-record-cs.md) creamos una página denominada `UploadInDetailsView.aspx` que muestran todas las categorías en un control GridView y muestran un DetailsView para agregar nuevas categorías para el sistema. En este tutorial se extenderá GridView para incluir la edición y eliminación de soporte técnico. En lugar de sigue trabajando desde `UploadInDetailsView.aspx`, permiten s en su lugar, coloque esta cambios tutorial s en el `UpdatingAndDeleting.aspx` página de la misma carpeta, `~/BinaryData`. Copie y pegue el marcado declarativo y el código de `UploadInDetailsView.aspx` a `UpdatingAndDeleting.aspx`.

Comience abriendo la `UploadInDetailsView.aspx` página. Copiar la sintaxis declarativa dentro de la `<asp:Content>` elemento, tal como se muestra en la figura 3. A continuación, abra `UpdatingAndDeleting.aspx` y pegue este marcado dentro de su `<asp:Content>` elemento. De forma similar, copie el código de la `UploadInDetailsView.aspx` página de la clase de código subyacente de s a `UpdatingAndDeleting.aspx`.


[![Copie el marcado declarativo de UploadInDetailsView.aspx](updating-and-deleting-existing-binary-data-cs/_static/image3.gif)](updating-and-deleting-existing-binary-data-cs/_static/image5.png)

**Figura 3**: copie el marcado declarativo de `UploadInDetailsView.aspx` ([haga clic aquí para ver la imagen a tamaño completo](updating-and-deleting-existing-binary-data-cs/_static/image6.png))


Una vez copiado en el marcado declarativo y el código, visite `UpdatingAndDeleting.aspx`. Debería ver el mismo resultado y tener la misma experiencia de usuario al igual que con `UploadInDetailsView.aspx` página en el tutorial anterior.

## <a name="step-4-adding-deleting-support-to-the-objectdatasource-and-gridview"></a>Paso 4: Agregar la eliminación de soporte técnico para el ObjectDataSource y GridView

Como se explicó en la [una visión general de insertar, actualizar y eliminar datos](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) tutorial, GridView proporciona capacidades de eliminación integradas y estas capacidades pueden habilitarse en la marca de verificación de una casilla de verificación si la cuadrícula s subyacente origen de datos admite la eliminación. ObjectDataSource GridView está enlazado actualmente a (`CategoriesDataSource`) no admite la eliminación.

Para solucionar esto, haga clic en la opción de configurar origen de datos de la etiqueta inteligente de ObjectDataSource s para iniciar al asistente. La primera pantalla muestra que el ObjectDataSource está configurado para trabajar con la `CategoriesBLL` clase. Presione a siguiente. Actualmente, solo el ObjectDataSource s `InsertMethod` y `SelectMethod` se especifican propiedades. Sin embargo, el asistente rellena automáticamente las listas desplegables en las pestañas UPDATE y DELETE con la `UpdateCategory` y `DeleteCategory` métodos, respectivamente. Esto es porque en el `CategoriesBLL` clase se marcan estos métodos mediante la `DataObjectMethodAttribute` como los métodos predeterminados para la actualización y eliminación.

Por ahora, establecer la lista de desplegable de ficha s de actualización para (ninguno), pero deje la lista de desplegable Eliminar ficha s establecida en `DeleteCategory`. Se tendrá que volver a este asistente en el paso 6 para agregar compatibilidad con la actualización.


[![Configurar el ObjectDataSource para utilizar el método DeleteCategory](updating-and-deleting-existing-binary-data-cs/_static/image4.gif)](updating-and-deleting-existing-binary-data-cs/_static/image7.png)

**Figura 4**: configurar el ObjectDataSource que se utilizan los `DeleteCategory` método ([haga clic aquí para ver la imagen a tamaño completo](updating-and-deleting-existing-binary-data-cs/_static/image8.png))


> [!NOTE]
> Al finalizar al asistente, Visual Studio puede preguntar si desea actualizar campos y claves, que volverá a generar los datos Web controla campos. Elija No, porque si elige Sí, se sobrescribirá cualquier personalización de campo que haya hecho.


ObjectDataSource ahora incluirá un valor para su `DeleteMethod` propiedad, así como un `DeleteParameter`. Recuerde que al utilizar el Asistente para especificar los métodos, Visual Studio establece la s ObjectDataSource `OldValuesParameterFormatString` propiedad `original_{0}`, lo que provoca problemas con la actualización y eliminar las invocaciones de método. Por lo tanto, esta propiedad borrar por completo o restablecer el valor predeterminado, `{0}`. Si tiene que actualizar la memoria en esta propiedad ObjectDataSource, consulte el [una visión general de insertar, actualizar y eliminar datos](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) tutorial.

Después de completar el asistente y corregir la `OldValuesParameterFormatString`, el marcado declarativo de ObjectDataSource s debería asemejarse similar al siguiente:


[!code-aspx[Main](updating-and-deleting-existing-binary-data-cs/samples/sample4.aspx)]

Después de configurar el ObjectDataSource, agregar capacidades de eliminación a GridView activando la casilla de verificación Habilitar eliminación de la etiqueta inteligente de s de GridView. Esto agregará una CommandField a GridView cuyo `ShowDeleteButton` propiedad está establecida en `true`.


[![Habilitar la compatibilidad para eliminar en el control GridView](updating-and-deleting-existing-binary-data-cs/_static/image5.gif)](updating-and-deleting-existing-binary-data-cs/_static/image9.png)

**Figura 5**: habilitar la compatibilidad con eliminación de GridView ([haga clic aquí para ver la imagen a tamaño completo](updating-and-deleting-existing-binary-data-cs/_static/image10.png))


Tómese un momento para probar la funcionalidad de eliminación. Hay una clave externa entre la `Products` tabla s `CategoryID` y `Categories` tabla s `CategoryID`, por lo que se le devolverá una excepción de infracción de restricción de clave externa si se intenta eliminar cualquiera de las primeras ocho categorías. Para probar esta funcionalidad out, agregue una nueva categoría, proporcionando un folleto y la imagen. La categoría de pruebas, se muestra en la figura 6, incluye un archivo de catálogo de prueba denominado `Test.pdf` y una imagen de prueba. La figura 7 muestra GridView después de que se ha agregado la categoría de pruebas.


[![Agregar una categoría de prueba con un folleto y una imagen](updating-and-deleting-existing-binary-data-cs/_static/image6.gif)](updating-and-deleting-existing-binary-data-cs/_static/image11.png)

**Figura 6**: agregar una categoría de prueba con una imagen y el folleto ([haga clic aquí para ver la imagen a tamaño completo](updating-and-deleting-existing-binary-data-cs/_static/image12.png))


[![Después de insertar la categoría de pruebas, se muestra en el control GridView](updating-and-deleting-existing-binary-data-cs/_static/image7.gif)](updating-and-deleting-existing-binary-data-cs/_static/image13.png)

**Figura 7**: después de insertar la categoría de pruebas, se muestra en el control GridView ([haga clic aquí para ver la imagen a tamaño completo](updating-and-deleting-existing-binary-data-cs/_static/image14.png))


En Visual Studio, actualice el Explorador de soluciones. Ahora debería ver un archivo nuevo en el `~/Brochures` carpeta, `Test.pdf` (consulte la figura 8).

A continuación, haga clic en el vínculo Eliminar en la fila de la categoría de pruebas, provoca que la página de devolución de datos y la `CategoriesBLL` clase s. `DeleteCategory` método que se active. Esto invocará a la capa DAL s `Delete` método, provocando la correspondiente `DELETE` instrucción para enviarse a la base de datos. Los datos, a continuación, se vuelve a enlazar a GridView y el marcado que se envía al cliente con la categoría de pruebas ya no está presente.

Mientras el flujo de trabajo de eliminación ha quitado correctamente el registro de la categoría de prueba desde el `Categories` tabla, no ha quitado su archivo de catálogo del sistema de archivos de web server s. Actualice el Explorador de soluciones y verá que `Test.pdf` continúen estando situados la `~/Brochures` carpeta.


![No se eliminó el archivo Test.pdf desde el sistema de archivos de s de servidor Web](updating-and-deleting-existing-binary-data-cs/_static/image8.gif)

**Figura 8**: el `Test.pdf` no se eliminó el archivo del sistema de archivos de s de servidor Web


## <a name="step-5-removing-the-deleted-category-s-brochure-file"></a>Paso 5: Quitar el archivo de folleto s categoría eliminada

Una de las desventajas de almacenar datos binarios externos a la base de datos es que se deben realizar pasos adicionales para limpiar estos archivos cuando se elimina el registro de base de datos asociada. La GridView y ObjectDataSource proporcionan eventos que se desencadenan antes y después de que se ha realizado el comando de eliminación. Se necesita realmente crear controladores de eventos para los eventos anteriores y posteriores a la acción. Antes de la `Categories` se elimina el registro es necesario determinar su ruta de acceso del archivo s PDF, pero no queremos t desea eliminar el archivo PDF antes de elimina la categoría en caso de que hay alguna excepción y no se elimina la categoría.

Las operaciones de asignación GridView [ `RowDeleting` eventos](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx) se desencadena antes de que se invocó el comando de eliminación de ObjectDataSource s, mientras su [ `RowDeleted` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleted.aspx) se activa después de. Crear controladores de eventos para estos dos eventos usando el siguiente código:


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample5.cs)]

En el `RowDeleting` controlador de eventos, el `CategoryID` de la fila que se va a eliminar es obtener un elemento de la s GridView `DataKeys` colección, que se puede acceder en este controlador de eventos a través de la `e.Keys` colección. Después, el `CategoriesBLL` clase s `GetCategoryByCategoryID(categoryID)` se invoca para devolver información acerca del registro que se va a eliminar. Si el valor devuelto `CategoriesDataRow` objeto tiene no`NULL``BrochurePath` valor se almacena en la variable de página `deletedCategorysPdfPath` para que se puede eliminar el archivo en el `RowDeleted` controlador de eventos.

> [!NOTE]
> En lugar de recuperar el `BrochurePath` detalles de la `Categories` registrar que se va a eliminar en la `RowDeleting` controlador de eventos, podríamos haber también agregamos la `BrochurePath` para las operaciones de asignación GridView `DataKeyNames` propiedad y acceder a él el valor de registro s a través de la `e.Keys` colección. Si lo hace, ligeramente aumentarán el tamaño del estado de la vista de GridView s, pero podría reducir la cantidad de código necesario y guardar un viaje a la base de datos.


Después de ObjectDataSource se invocó el comando delete subyacente de s, las operaciones de asignación GridView `RowDeleted` se activa de controlador de eventos. Si no había ninguna excepción de eliminación de los datos y hay un valor para `deletedCategorysPdfPath`, a continuación, se elimina el PDF del sistema de archivos. Tenga en cuenta que este código adicional no es necesario para limpiar los datos binarios de categoría s asociados a su imagen. S porque los datos de imagen se almacenan directamente en la base de datos, por lo que al eliminar el `Categories` fila también elimina los datos de imagen de esa categoría s.

Después de agregar los controladores de eventos de dos, vuelva a ejecutar este caso de prueba. Al eliminar la categoría, también se elimina su PDF asociado.

La actualización de datos binarios registros que están asociadas existentes proporciona algunos desafíos interesantes. El resto de este tutorial se profundiza en Agregar capacidades de actualización para el folleto y la imagen. Paso 6 explora las técnicas para actualizar la información de catálogo mientras examina actualizar la imagen del paso 7.

## <a name="step-6-updating-a-category-s-brochure"></a>Paso 6: Actualizar un folleto de s de categoría

Como se describe en el [una visión general de insertar, actualizar y eliminar datos](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) tutorial, GridView ofrece integrada compatibilidad de edición de nivel de fila que puede implementarse mediante la marca de verificación de una casilla de verificación si su origen de datos subyacente es ha configurado correctamente. Actualmente, el `CategoriesDataSource` ObjectDataSource aún no está configurada para que se incluyen la actualización de soporte técnico, por lo que permiten s que en Agregar.

Haga clic en el vínculo Configurar origen de datos desde el Asistente para la s de ObjectDataSource y continúe con el segundo paso. Debido la `DataObjectMethodAttribute` utilizados en `CategoriesBLL`, automáticamente se debe rellenar la lista desplegable de actualización con el `UpdateCategory` sobrecarga que acepta cuatro parámetros de entrada (para todas las columnas pero `Picture`). Cambiar esto para que utilice la sobrecarga con cinco parámetros.


[![Configurar el ObjectDataSource para utilizar el método UpdateCategory que incluya un parámetro de imagen](updating-and-deleting-existing-binary-data-cs/_static/image9.gif)](updating-and-deleting-existing-binary-data-cs/_static/image15.png)

**Figura 9**: configurar el ObjectDataSource que se utilizan los `UpdateCategory` método que incluya un parámetro para `Picture` ([haga clic aquí para ver la imagen a tamaño completo](updating-and-deleting-existing-binary-data-cs/_static/image16.png))


ObjectDataSource ahora incluirá un valor para su `UpdateMethod` propiedad, así como la correspondiente `UpdateParameter` s. Como se indicó en el paso 4, Visual Studio establece la s ObjectDataSource `OldValuesParameterFormatString` propiedad `original_{0}` cuando se usa el Asistente para configurar orígenes de datos. Esto provocará problemas con la actualización y eliminar las invocaciones de método. Por lo tanto, esta propiedad borrar por completo o restablecer el valor predeterminado, `{0}`.

Después de completar el asistente y corregir la `OldValuesParameterFormatString`, el marcado declarativo de ObjectDataSource s debería ser similar al siguiente:


[!code-aspx[Main](updating-and-deleting-existing-binary-data-cs/samples/sample6.aspx)]

Para activar las características de edición de GridView s integradas, active la opción Habilitar edición de la etiqueta inteligente de s de GridView. Esta acción establecerá la s CommandField `ShowEditButton` propiedad `true`, da lugar a la adición de un botón Editar (y botones Actualizar y Cancelar de la fila que se está edita).


[![Configurar el control GridView que admiten la edición](updating-and-deleting-existing-binary-data-cs/_static/image10.gif)](updating-and-deleting-existing-binary-data-cs/_static/image17.png)

**Figura 10**: configurar el control GridView que admiten la edición ([haga clic aquí para ver la imagen a tamaño completo](updating-and-deleting-existing-binary-data-cs/_static/image18.png))


Visite la página a través de un explorador y haga clic en una de las filas de botones Editar. El `CategoryName` y `Description` BoundFields se representan como cuadros de texto. El `BrochurePath` TemplateField no tiene un `EditItemTemplate`, por lo que seguirá mostrando su `ItemTemplate` un vínculo al folleto. El `Picture` ImageField representa como un cuadro de texto cuyo `Text` propiedad se asigna el valor de las operaciones de asignación ImageField `DataImageUrlField` valor, en este caso `CategoryID`.


[![GridView no tiene una interfaz de edición para BrochurePath](updating-and-deleting-existing-binary-data-cs/_static/image11.gif)](updating-and-deleting-existing-binary-data-cs/_static/image19.png)

**Figura 11**: GridView no tiene una interfaz de edición para `BrochurePath` ([haga clic aquí para ver la imagen a tamaño completo](updating-and-deleting-existing-binary-data-cs/_static/image20.png))


## <a name="customizing-thebrochurepaths-editing-interface"></a>Personalizar el`BrochurePath`s interfaz de edición

Se necesita crear una interfaz de edición para el `BrochurePath` TemplateField, que permite al usuario uno:

- Deje el folleto categoría s como-es,
- Actualizar el folleto categoría s mediante la carga de un folleto de nuevo, o
- Quite por completo el folleto categoría s (en el caso de que la categoría ya no tiene un folleto asociado).

También es necesario actualizar el `Picture` interfaz de edición ImageField s, pero se podrá hacer esto en el paso 7.

Desde la etiqueta inteligente de GridView s, haga clic en el vínculo Editar plantillas y seleccione el `BrochurePath` TemplateField s `EditItemTemplate` en la lista desplegable. Agregar un control RadioButtonList Web a esta plantilla, establecer su `ID` propiedad `BrochureOptions` y su `AutoPostBack` propiedad `true`. En la ventana Propiedades, haga clic en el botón de puntos suspensivos en el `Items` propiedad, que le llevará a la `ListItem` Editor de la colección. Agregue las tres opciones siguientes con `Value` s 1, 2 y 3, respectivamente:

- Usar folleto actual
- Quitar folleto actual
- Cargar nuevo folleto

Configurar el primer `ListItem` s `Selected` propiedad `true`.


![Agregar tres ListItems a RadioButtonList](updating-and-deleting-existing-binary-data-cs/_static/image12.gif)

**Figura 12**: agregar tres `ListItem` s a RadioButtonList


Bajo RadioButtonList, agregue un control FileUpload denominado `BrochureUpload`. Establecer su `Visible` propiedad `false`.


[![Agregar un RadioButtonList y Control FileUpload a EditItemTemplate](updating-and-deleting-existing-binary-data-cs/_static/image13.gif)](updating-and-deleting-existing-binary-data-cs/_static/image21.png)

**Figura 13**: agregar un RadioButtonList y Control FileUpload a la `EditItemTemplate` ([haga clic aquí para ver la imagen a tamaño completo](updating-and-deleting-existing-binary-data-cs/_static/image22.png))


Este RadioButtonList proporciona las tres opciones para el usuario. La idea es que el control FileUpload se mostrarán solo si se selecciona la última opción, carga nuevo folleto. Para ello, cree un controlador de eventos para el s RadioButtonList `SelectedIndexChanged` eventos y agregue el código siguiente:


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample7.cs)]

Dado que los controles RadioButtonList y FileUpload están dentro de una plantilla, se tiene que escribir un poco de código para obtener acceso mediante programación a estos controles. El `SelectedIndexChanged` controlador de eventos se pasa una referencia de RadioButtonList en el `sender` parámetro de entrada. Para obtener el control FileUpload, debemos obtener el elemento primario s de RadioButtonList control y use la `FindControl("controlID")` método a partir de ahí. Una vez que tenemos una referencia a los controles RadioButtonList tanto de FileUpload, el FileUpload controlar s `Visible` propiedad está establecida en `true` solo si las operaciones de asignación RadioButtonList `SelectedValue` es igual a 3, que es el `Value` para cargar nuevo folleto `ListItem`.

Con este código en su lugar, tómese un momento para probar la interfaz de edición. Haga clic en el botón Editar para una fila. Inicialmente, debe seleccionarse la opción de folleto actual de uso. El índice seleccionado el cambio, produce un postback. Si se selecciona la tercera opción, se muestra el control FileUpload, en caso contrario, está oculta. La figura 14 muestra la interfaz de edición cuando primero se hace clic en el botón Editar; Figura 15 muestra la interfaz después de que está seleccionada la opción de carga nueva folleto.


[![Inicialmente, el folleto actual de uso que está seleccionada la opción](updating-and-deleting-existing-binary-data-cs/_static/image14.gif)](updating-and-deleting-existing-binary-data-cs/_static/image23.png)

**Figura 14**: inicialmente, el folleto actual de uso está seleccionada ([haga clic aquí para ver la imagen a tamaño completo](updating-and-deleting-existing-binary-data-cs/_static/image24.png))


[![Elegir el folleto de carga nueva opción muestra el Control FileUpload](updating-and-deleting-existing-binary-data-cs/_static/image15.gif)](updating-and-deleting-existing-binary-data-cs/_static/image25.png)

**Figura 15**: elegir el folleto de carga nueva opción muestra el Control FileUpload ([haga clic aquí para ver la imagen a tamaño completo](updating-and-deleting-existing-binary-data-cs/_static/image26.png))


## <a name="saving-the-brochure-file-and-updating-thebrochurepathcolumn"></a>Guardar el folleto archivo y actualice la`BrochurePath`columna

Cuando se hace clic en el botón de actualización de s GridView, su `RowUpdating` desencadena el evento. ObjectDataSource se invoca el comando de actualización de s y, a continuación, las operaciones de asignación GridView `RowUpdated` desencadena el evento. Al igual que con el flujo de trabajo eliminando, necesitamos crear controladores de eventos para estos dos eventos. En el `RowUpdating` controlador de eventos, es necesario determinar qué acción realizar basándose en el `SelectedValue` de la `BrochureOptions` RadioButtonList:

- Si el `SelectedValue` es 1, desea seguir usando el mismo `BrochurePath` configuración. Por lo tanto, es necesario establecer la s ObjectDataSource `brochurePath` parámetro existente `BrochurePath` valor del registro que se está actualizando. Las operaciones de asignación ObjectDataSource `brochurePath` puede establecerse utilizando el parámetro `e.NewValues["brochurePath"] = value`.
- Si el `SelectedValue` es 2, entonces desea establecer el registro s `BrochurePath` valor a `NULL`. Esto puede realizarse mediante el establecimiento de las operaciones de asignación ObjectDataSource `brochurePath` parámetro `Nothing`, que da como resultado una base de datos `NULL` se utiliza en el `UPDATE` instrucción. Si hay un archivo de catálogo existente que se va a quitar, es necesario eliminar el archivo existente. Sin embargo, solamente queremos hacer esto si la actualización se realiza sin que se produzca una excepción.
- Si el `SelectedValue` es 3, a continuación, desea asegurarse de que el usuario ha cargado un archivo PDF y, a continuación, guardarlo en el sistema de archivos y actualizar el registro s `BrochurePath` valor de la columna. Además, si hay un archivo de catálogo existente que se va a reemplazar, es necesario eliminar el archivo anterior. Sin embargo, solamente queremos hacer esto si la actualización se realiza sin que se produzca una excepción.

Los pasos necesarios para completar cuando las operaciones de asignación RadioButtonList `SelectedValue` es 3 son prácticamente idénticos a los usados por las operaciones de asignación DetailsView `ItemInserting` controlador de eventos. Este controlador de eventos se ejecuta cuando se agrega un nuevo registro de categoría desde el control de DetailsView que hemos agregado en el [tutorial anterior](including-a-file-upload-option-when-adding-a-new-record-cs.md). Por consiguiente, responsabilidad nos refactorizar esta funcionalidad out en métodos independientes. En concreto, ha sacado de la funcionalidad común en dos métodos:

- `ProcessBrochureUpload(FileUpload, out bool)`acepta como entrada una instancia del control FileUpload y un valor booleano de salida que especifica si debe continuar la operación de eliminación o editar o si se debe cancelar debido a un error de validación. Este método devuelve la ruta de acceso al archivo guardado o `null` si se ha guardado ningún archivo.
- `DeleteRememberedBrochurePath`Elimina el archivo especificado por la ruta de acceso en la variable de página `deletedCategorysPdfPath` si `deletedCategorysPdfPath` no es `null`.

A continuación se muestra el código de estos dos métodos. Tenga en cuenta la similitud entre `ProcessBrochureUpload` y las operaciones de asignación DetailsView `ItemInserting` controlador de eventos en el tutorial anterior. En este tutorial he actualizado los controladores de eventos de DetailsView s para utilizar estos nuevos métodos. Descargar el código asociado con este tutorial para ver las modificaciones en los controladores de eventos de DetailsView s.


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample8.cs)]

Las operaciones de asignación GridView `RowUpdating` y `RowUpdated` controladores de eventos utilizan la `ProcessBrochureUpload` y `DeleteRememberedBrochurePath` métodos, como se muestra en el código siguiente:


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample9.cs)]

Tenga en cuenta cómo el `RowUpdating` controlador de eventos usa una serie de instrucciones condicionales para realizar las acciones apropiadas en función de la `BrochureOptions` RadioButtonList s `SelectedValue` valor de propiedad.

Con este código en su lugar, puede editar una categoría y que utiliza su folleto actual, no use ningún folleto o cargar uno nuevo. Continúe y pruébelo. Establecer puntos de interrupción la `RowUpdating` y `RowUpdated` controladores de eventos para hacerse una idea del flujo de trabajo.

## <a name="step-7-uploading-a-new-picture"></a>Paso 7: Cargar una imagen nueva

El `Picture` s ImageField editar interfaz se representan como un cuadro de texto que se rellena con el valor de su `DataImageUrlField` propiedad. Durante el flujo de trabajo de edición, GridView pasa un parámetro a ObjectDataSource con el nombre del parámetro s el valor de las operaciones de asignación ImageField `DataImageUrlField` el valor especificado en el cuadro de texto en la interfaz de edición de valor de propiedad y el parámetro s. Este comportamiento es adecuado cuando se guarda la imagen como un archivo en el sistema de archivos y el `DataImageUrlField` contiene la dirección URL completa de la imagen. Con estas circunstancias, la interfaz de edición muestra la dirección URL de imagen s en el cuadro de texto que el usuario puede cambiar y ha guardado en la base de datos. Concede, este interfaz predeterminada tipo t permite al usuario cargar una nueva imagen, pero lo que les permiten cambiar la dirección URL de la imagen desde el valor actual a otro. Para este tutorial, sin embargo, el valor predeterminado de la s ImageField edición interfaz no es suficiente porque el `Picture` datos binarios se almacenan directamente en la base de datos y el `DataImageUrlField` propiedad contiene solo los `CategoryID`.

Para entender mejor lo que sucede en nuestro tutorial cuando un usuario edita una fila con un ImageField, considere el ejemplo siguiente: un usuario edita una fila con `CategoryID` 10, provocando la `Picture` ImageField para representar como un cuadro de texto con el valor 10. Imagine que el usuario cambia el valor de este cuadro de texto en 50 y hace clic en el botón de actualización. Se produce un postback y GridView crea inicialmente un parámetro denominado `CategoryID` con el valor 50. Sin embargo, antes de que el control GridView envíe este parámetro (y la `CategoryName` y `Description` parámetros), agrega en los valores de la `DataKeys` colección. Por lo tanto, sobrescribe la `CategoryID` parámetro con la actual fila s subyacente `CategoryID` valor 10. En resumen, las operaciones de asignación ImageField interfaz de edición no tiene ningún efecto en el flujo de trabajo de edición para este tutorial porque los nombres de las operaciones de asignación ImageField `DataImageUrlField` propiedad y la cuadrícula s `DataKey` valor son iguales.

Si bien la ImageField hace fácil mostrar una imagen basada en la base de datos, no queremos t desea proporcionar un cuadro de texto en la interfaz de edición. En su lugar, desea ofrecer un control FileUpload que el usuario final puede usar para cambiar la imagen de s de categoría. A diferencia de la `BrochurePath` valor para estos tutoriales se ha decidido requerir que cada categoría debe tener una imagen. Por lo tanto, no queremos t necesario para permitir al usuario indicar que no hay ninguna imagen asociada al usuario puede cargar una nueva imagen o dejar la imagen actual como-es.

Para personalizar la interfaz de edición ImageField s, se debe convertir en TemplateField. Desde la etiqueta inteligente de GridView s, haga clic en el vínculo Editar columnas, seleccione el ImageField y haga clic en la función Convert este campo en un vínculo de TemplateField.


![Convertir el ImageField en TemplateField](updating-and-deleting-existing-binary-data-cs/_static/image16.gif)

**Figura 16**: convertir la ImageField en TemplateField


Convertir el ImageField en TemplateField de esta manera, genera TemplateField con dos plantillas. Como se muestra en la siguiente sintaxis declarativa, el `ItemTemplate` contiene una imagen de Web control cuya `ImageUrl` propiedad se asigna mediante sintaxis de enlace de datos basándose en las operaciones de asignación ImageField `DataImageUrlField` y `DataImageUrlFormatString` propiedades. El `EditItemTemplate` contiene un cuadro de texto cuyo `Text` propiedad se enlaza con el valor especificado por el `DataImageUrlField` propiedad.


[!code-aspx[Main](updating-and-deleting-existing-binary-data-cs/samples/sample10.aspx)]

Deberá actualizar el `EditItemTemplate` para usar un control FileUpload. Acceso a vínculos desde la etiqueta inteligente de s haga clic en Editar plantillas de GridView y, a continuación, seleccione la `Picture` TemplateField s `EditItemTemplate` en la lista desplegable. En la plantilla debería ver un cuadro de texto quitar esta. A continuación, arrastre un control FileUpload del cuadro de herramientas en la plantilla, establecer su `ID` a `PictureUpload`. Agregar el texto para cambiar la imagen de la categoría s, especifique una nueva imagen. Para mantener la imagen de la categoría s las mismas, deje el campo vacío a la plantilla.


[![Agregar un Control FileUpload a EditItemTemplate](updating-and-deleting-existing-binary-data-cs/_static/image17.gif)](updating-and-deleting-existing-binary-data-cs/_static/image27.png)

**Figura 17**: agregar un Control FileUpload a la `EditItemTemplate` ([haga clic aquí para ver la imagen a tamaño completo](updating-and-deleting-existing-binary-data-cs/_static/image28.png))


Después de personalizar la interfaz de edición, ver el progreso en un explorador. Al ver una fila en modo de solo lectura, se muestra la imagen de s categoría tal y como estaba antes, pero al hacer clic en el botón de edición, la columna de imagen representa como texto con un control FileUpload.


[![La interfaz de edición incluye un Control FileUpload](updating-and-deleting-existing-binary-data-cs/_static/image18.gif)](updating-and-deleting-existing-binary-data-cs/_static/image29.png)

**Figura 18**: la interfaz de edición incluye un Control FileUpload ([haga clic aquí para ver la imagen a tamaño completo](updating-and-deleting-existing-binary-data-cs/_static/image30.png))


Recuerde que el ObjectDataSource está configurado para llamar a la `CategoriesBLL` clase s `UpdateCategory` método que acepta como entrada los datos binarios de la imagen como un `byte` matriz. Si esta matriz tiene un `null` valor, sin embargo, la alternativa `UpdateCategory` sobrecarga se llama, qué problemas la `UPDATE` instrucción SQL que no modifica la `Picture` columna, con lo que deja la categoría s actual imagen intactos. Por lo tanto, en las operaciones de asignación GridView `RowUpdating` controlador de eventos, es necesario hacer referencia mediante programación el `PictureUpload` FileUpload controlar y determinar si se ha cargado un archivo. Si no se ha cargado, hacemos *no* desea especificar un valor para el `picture` parámetro. Por otro lado, si un archivo se ha cargado en el `PictureUpload` control FileUpload, que deseamos asegurarnos de que es un archivo JPG. Si lo es, podemos enviar su contenido binario a ObjectDataSource a través de la `picture` parámetro.

Al igual que con el código utilizado en el paso 6, gran parte del código necesario especificar ningún parámetro ya existe en la s DetailsView `ItemInserting` controlador de eventos. Por lo tanto, se ha refactorizado la funcionalidad común en un nuevo método `ValidPictureUpload`y actualiza el `ItemInserting` controlador de eventos que se usa este método.

Agregue el código siguiente al principio de las operaciones de asignación GridView `RowUpdating` controlador de eventos. Lo importante que este código preceder el código que guarda el archivo de catálogo porque no queremos t desea guardar el folleto en el sistema de archivos del servidor s web si se carga un archivo de imagen no válido.


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample11.cs)]

El `ValidPictureUpload(FileUpload)` método toma un control FileUpload como su único parámetro de entrada y revisa la extensión del archivo cargado s para asegurarse de que el archivo cargado es un JPG; sólo se invoca si se carga un archivo de imagen. Si se cargó ningún archivo, el parámetro de imagen es no establecido y, por lo tanto, utiliza su valor predeterminado de `null`. Si se carga una imagen y `ValidPictureUpload` devuelve `true`, `picture` parámetro se asigna los datos binarios de la imagen cargada; si el método devuelve `false`, se cancela el flujo de trabajo de actualización y sale del controlador de eventos.

El `ValidPictureUpload(FileUpload)` código del método, que se ha refactorizado desde las operaciones de asignación DetailsView `ItemInserting` sigue el controlador de eventos:


[!code-csharp[Main](updating-and-deleting-existing-binary-data-cs/samples/sample12.cs)]

## <a name="step-8-replacing-the-original-categories-pictures-with-jpgs"></a>Paso 8: Reemplazar las imágenes originales de categorías con jpg

Recuerde que las imágenes originales de ocho categorías son archivos de mapa de bits que se encapsulan en un encabezado OLE. Ahora que hemos agregado la capacidad de editar una imagen de registro s existente, tómese un momento para reemplazar estos mapas de bits con jpg. Si desea seguir usando las imágenes de la categoría actual, se puede convertir a jpg realizando los pasos siguientes:

1. Guardar las imágenes de mapa de bits en el disco duro. Visite la `UpdatingAndDeleting.aspx` página en el explorador y para cada una de las primeras ocho categorías, haga doble clic en la imagen y optar por guardar la imagen.
2. Abra la imagen en el editor de imágenes de elección. Puede utilizar Microsoft Paint, por ejemplo.
3. Guarde el mapa de bits como una imagen JPG.
4. Actualizar la imagen de la categoría s a través de la interfaz de edición, mediante el archivo JPG.

Después de editar una categoría y cargar la imagen JPG, la imagen no se representará en el explorador porque la `DisplayCategoryPicture.aspx` página es quitar los primeros 78 bytes de las imágenes de las primeras ocho categorías. Solucionar este problema quitando el código que realiza la eliminación de encabezado OLE. Una vez hecho esto, el `DisplayCategoryPicture.aspx``Page_Load` controlador de eventos debe tener solo el código siguiente:


[!code-vb[Main](updating-and-deleting-existing-binary-data-cs/samples/sample13.vb)]

> [!NOTE]
> La `UpdatingAndDeleting.aspx` página s insertar y modificar interfaces podría usar un poco más trabajo. El `CategoryName` y `Description` BoundFields DetailsView y GridView se debe convertir en TemplateFields. Puesto que `CategoryName` no permite `NULL` valores, se debe agregar un control RequiredFieldValidator. Y el `Description` probablemente se debe convertir el cuadro de texto en un cuadro de texto de varias líneas. Dejar estas últimas pinceladas como un ejercicio para usted.


## <a name="summary"></a>Resumen

Este tutorial completa nuestro aspecto al trabajar con datos binarios. En este tutorial y los tres anteriores, hemos visto cómo binarios datos pueden almacenarse en el sistema de archivos o directamente dentro de la base de datos. Un usuario proporciona datos binarios en el sistema, seleccione un archivo en su unidad de disco duro y se carga al servidor web, donde se pueda almacenar en el sistema de archivos o insertar en la base de datos. ASP.NET 2.0 incluye un control FileUpload que hace que proporciona dicha interfaz tan fácil como arrastrar y colocar. Sin embargo, como se indicó en el [cargar archivos](uploading-files-cs.md) tutorial, el control FileUpload solo es ideal para las cargas de archivo relativamente pequeño, lo ideal es que no supere un megabyte. También hemos examinado cómo asociar los datos cargados con el modelo de datos subyacente, así como editar y eliminar los datos binarios de los registros existentes.

El siguiente conjunto de tutoriales explora varias técnicas de almacenamiento en caché. Almacenamiento en caché proporciona un medio para mejorar una aplicación s el rendimiento general mediante la obtención de los resultados de operaciones costosas y almacenarlas en una ubicación que se puede acceder más rápidamente.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Revisor inicial para este tutorial era Teresa Murphy. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](including-a-file-upload-option-when-adding-a-new-record-cs.md)
[Siguiente](uploading-files-vb.md)
