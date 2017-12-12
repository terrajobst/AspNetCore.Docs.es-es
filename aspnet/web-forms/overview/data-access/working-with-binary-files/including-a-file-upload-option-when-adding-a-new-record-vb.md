---
uid: web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-vb
title: Incluido un archivo de opciones de carga al agregar un nuevo registro (VB) | Documentos de Microsoft
author: rick-anderson
description: "Este tutorial muestra cómo crear una interfaz Web que permite al usuario introducir datos de texto y cargar archivos binarios. Para ilustrar la t disponibles opciones..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: 5776281d-4637-4d1e-a65b-2621d2cade44
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-vb
msc.type: authoredcontent
ms.openlocfilehash: 4f49c201c71ca8f98d7e15b29f1df9a6bcd1b12e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="including-a-file-upload-option-when-adding-a-new-record-vb"></a>Incluida una opción de carga de archivo al agregar un nuevo registro (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_56_VB.exe) o [descarga de PDF](including-a-file-upload-option-when-adding-a-new-record-vb/_static/datatutorial56vb1.pdf)

> Este tutorial muestra cómo crear una interfaz Web que permite al usuario introducir datos de texto y cargar archivos binarios. Para ilustrar las opciones disponibles para almacenar datos binarios, un archivo se guarda en la base de datos mientras el otro se almacena en el sistema de archivos.


## <a name="introduction"></a>Introducción

En los dos tutoriales anteriores, analizamos técnicas para almacenar datos binarios que está asociados con el modelo de datos de aplicación s, examinado cómo usar el control FileUpload para enviar archivos desde el cliente al servidor web y hemos visto cómo presentar estos datos binarios en datos W control de EB. Se ve todavía para hablar sobre cómo asociar los datos cargados con el modelo de datos, aunque.

En este tutorial crearemos una página web para agregar una nueva categoría. Además de cuadros de texto para el nombre de categoría s y la descripción, esta página deberá incluir dos controles FileUpload uno para la nueva imagen de categoría s y otro para el folleto. Se almacenará la imagen cargada directamente en el nuevo registro s `Picture` columna, mientras que el folleto se guardará en el `~/Brochures` carpeta con la ruta de acceso al archivo guardado en el nuevo registro s `BrochurePath` columna.

Antes de crear esta nueva página web, necesitamos actualizar la arquitectura. El `CategoriesTableAdapter` consulta principal s no recupera la `Picture` columna. Por lo tanto, el generado automáticamente `Insert` método sólo tiene entradas para el `CategoryName`, `Description`, y `BrochurePath` campos. Por lo tanto, es necesario crear un método adicional de TableAdapter que solicita los cuatro `Categories` campos. La `CategoriesBLL` clase en la capa de lógica de negocios también tendrá que actualizarse.

## <a name="step-1-adding-aninsertwithpicturemethod-to-thecategoriestableadapter"></a>Paso 1: Agregar un`InsertWithPicture`método a la`CategoriesTableAdapter`

Cuando se crea el `CategoriesTableAdapter` en el [crear una capa de acceso a datos](../introduction/creating-a-data-access-layer-vb.md) tutorial, hemos configurado para generar automáticamente `INSERT`, `UPDATE`, y `DELETE` las instrucciones en función de la consulta principal. Además, se indica al TableAdapter que se emplea el enfoque directo de base de datos, que crea los métodos `Insert`, `Update`, y `Delete`. Estos métodos de ejecutan el generado automáticamente `INSERT`, `UPDATE`, y `DELETE` las instrucciones y, por lo tanto, acepta parámetros de entrada en función de las columnas devueltas por la consulta principal. En el [cargar archivos](uploading-files-vb.md) tutorial hemos aumentados el `CategoriesTableAdapter` consulta principal de s que se utilizará la `BrochurePath` columna.

Puesto que la `CategoriesTableAdapter` no hace referencia la consulta principal s el `Picture` columna, se puede agregar un nuevo registro ni actualizar un registro existente con un valor para la `Picture` columna. Para capturar esta información, ya sea podemos crear un nuevo método en TableAdapter que se utiliza específicamente para insertar un registro con datos binarios o podemos personalizar el generado automáticamente `INSERT` instrucción. El problema con la personalización de la autogenerada `INSERT` instrucción es que es el riesgo de nuestro personalizaciones sobrescribe por el asistente. Por ejemplo, imagine que se personaliza la `INSERT` instrucción deben incluir el uso de la `Picture` columna. Esto actualizaría TableAdapters `Insert` método para incluir un parámetro de entrada adicional para los datos binarios de imagen s categoría s. A continuación, se puede crear un método en la capa de lógica empresarial para utilizar este método DAL e invocar este método BLL a través de la capa de presentación y todo lo que funcionaría muy. Es decir, hasta la próxima vez que hemos configurado el TableAdapter mediante el Asistente para configuración de TableAdapter. En cuanto el asistente termine, nuestro personalizaciones en el `INSERT` se sobrescribiría la instrucción, el `Insert` método revertiría a su formato antiguo y ya no se compilará el código!

> [!NOTE]
> Esta serie de molestias es un problema cuando utilice procedimientos almacenados en lugar de instrucciones SQL ad hoc. Un tutorial futuras explorará mediante procedimientos almacenados en lugar de instrucciones SQL ad hoc en la capa de acceso a datos.


Para evitar esta posibilidad dolor de cabeza, en lugar de personalizar las instrucciones SQL generadas automáticamente permite s e en su lugar, cree un nuevo método para el objeto TableAdapter. Este método, denominado `InsertWithPicture`, acepte los valores para la `CategoryName`, `Description`, `BrochurePath`, y `Picture` columnas y ejecutar un `INSERT` instrucción que almacena los cuatro valores en un nuevo registro.

Abra el conjunto de datos con tipo y, en el diseñador, haga doble clic en el `CategoriesTableAdapter` encabezado s y elija Agregar consulta en el menú contextual. Esto inicia al Asistente de configuración de consulta de TableAdapter, que comienza por nos pide que forma la consulta de TableAdapter debe tener acceso a la base de datos. Elija usar instrucciones SQL y haga clic en siguiente. El paso siguiente solicita el tipo de consulta que se genere. Desde que se re creando una consulta para agregar un nuevo registro a la `Categories` de la tabla, elija Insertar y haga clic en siguiente.


[![Seleccione la opción de INSERCIÓN](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image1.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image1.png)

**Figura 1**: seleccione la opción Insertar ([haga clic aquí para ver la imagen a tamaño completo](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image2.png))


Ahora debemos especificar la `INSERT` instrucción SQL. El Asistente sugiere automáticamente un `INSERT` instrucción correspondiente a la consulta principal de TableAdapter s. En este caso, se s un `INSERT` instrucción que inserta la `CategoryName`, `Description`, y `BrochurePath` valores. La instrucción Update para que la `Picture` las columnas se incluyen junto con un `@Picture` parámetro, de este modo:


[!code-sql[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample1.sql)]

La pantalla final del asistente le pide que el nombre del nuevo método de TableAdapter. Escriba `InsertWithPicture` y haga clic en Finalizar.


[![Nombre de la nueva InsertWithPicture TableAdapter (método)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image2.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image3.png)

**Figura 2**: nombre del nuevo método de TableAdapter `InsertWithPicture` ([haga clic aquí para ver la imagen a tamaño completo](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image4.png))


## <a name="step-2-updating-the-business-logic-layer"></a>Paso 2: Actualizar la capa de lógica de negocios

Puesto que el nivel de presentación sólo debe interactuar con la capa de lógica de negocios en lugar de omitirlo para ir directamente a la capa de acceso a datos, es necesario crear un método BLL que invoca el método de la capa DAL que acabamos de crear (`InsertWithPicture`). Para este tutorial, cree un método en el `CategoriesBLL` clase denominada `InsertWithPicture` que acepta como entrada tres `String` s y un `Byte` matriz. El `String` son parámetros de entrada para el nombre de la categoría s, la descripción y la ruta de acceso de archivo de folleto, mientras el `Byte` matriz es para el contenido binario de la imagen de la categoría s. Como se muestra en el código siguiente, este método BLL invoca al método correspondiente de la capa DAL:


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample2.vb)]

> [!NOTE]
> Asegúrese de que ha guardado el conjunto de datos con tipo antes de agregar el `InsertWithPicture` método a la capa BLL. Puesto que la `CategoriesTableAdapter` código de la clase es generado automáticamente tomando como base el conjunto de datos con tipo, si don t primero guarde los cambios en el conjunto de datos con tipo la `Adapter` propiedad won t saber la `InsertWithPicture` método.


## <a name="step-3-listing-the-existing-categories-and-their-binary-data"></a>Paso 3: Enumerar las categorías existentes y sus datos binarios

En este tutorial crearemos una página que permite a un usuario final agregar una nueva categoría en el sistema, proporcionando una imagen y el folleto para la nueva categoría. En el [tutorial anterior](displaying-binary-data-in-the-data-web-controls-vb.md) se usa un control GridView con un TemplateField y ImageField para mostrar cada nombre de categoría s, descripción, imagen y un vínculo para descargar el folleto. Permiten s replicar esa funcionalidad para este tutorial, para crear una página que enumera todas las categorías existentes y permite nuevos a crearse.

Comience abriendo la `DisplayOrDownload.aspx` página desde el `BinaryData` carpeta. Vaya a la vista del origen y copiar la GridView y ObjectDataSource s sintaxis declarativa, pegarlo en el `<asp:Content>` elemento `UploadInDetailsView.aspx`. Además, no olvide copiar sobre la `GenerateBrochureLink` método de la clase de código subyacente de `DisplayOrDownload.aspx` a `UploadInDetailsView.aspx`.


[![Copie y pegue la sintaxis declarativa de DisplayOrDownload.aspx a UploadInDetailsView.aspx](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image3.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image5.png)

**Figura 3**: copiar y pegar la sintaxis declarativa de `DisplayOrDownload.aspx` a `UploadInDetailsView.aspx` ([haga clic aquí para ver la imagen a tamaño completo](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image6.png))


Después de copiar la sintaxis declarativa y `GenerateBrochureLink` método sobre la `UploadInDetailsView.aspx` página, vea la página a través de un explorador para asegurarse de que todo lo que se copió correctamente. Debería ver una lista las ocho categorías de GridView que incluye un vínculo para descargar el folleto, así como la imagen de la categoría s.


[![Ahora debería ver cada categoría junto con sus datos binarios](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image4.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image7.png)

**Figura 4**: se debe ahora vea cada categoría a lo largo de con sus datos binarios ([haga clic aquí para ver la imagen a tamaño completo](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image8.png))


## <a name="step-4-configuring-thecategoriesdatasourceto-support-inserting"></a>Paso 4: Configurar el`CategoriesDataSource`a la inserción de soporte técnico

El `CategoriesDataSource` ObjectDataSource utilizado por el `Categories` GridView actualmente no proporciona la capacidad para insertar datos. Para admitir Insertar a través de este control de origen de datos, debe asignar su `Insert` método a un método en su objeto subyacente, `CategoriesBLL`. En concreto, queremos para asignarla a la `CategoriesBLL` método se agregó en el paso 2, `InsertWithPicture`.

Iniciar haciendo clic en el vínculo Configurar origen de datos de la etiqueta inteligente de ObjectDataSource s. La primera pantalla muestra el objeto de origen de datos está configurado para trabajar con, `CategoriesBLL`. Deje esta configuración como-está y haga clic en siguiente para avanzar a la pantalla de definir métodos de datos. Ir a la pestaña Insertar y elegir el `InsertWithPicture` método en la lista desplegable. Haga clic en Finalizar para completar al asistente.


[![Configurar el ObjectDataSource para utilizar el método InsertWithPicture](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image5.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image9.png)

**Figura 5**: configurar el ObjectDataSource para usar el `InsertWithPicture` método ([haga clic aquí para ver la imagen a tamaño completo](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image10.png))


> [!NOTE]
> Al finalizar al asistente, Visual Studio puede preguntar si desea actualizar campos y claves, que volverá a generar los datos Web controla campos. Elija No, porque si elige Sí, se sobrescribirá cualquier personalización de campo que haya hecho.


Después de completar el asistente, ObjectDataSource ahora incluirá un valor para su `InsertMethod` propiedad como `InsertParameters` para las columnas de la categoría de cuatro, como el siguiente marcado declarativo muestra:


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample3.aspx)]

## <a name="step-5-creating-the-inserting-interface"></a>Paso 5: Creación de la interfaz de inserción

Se tratan como primero en el [una visión general de insertar, actualizar y eliminar datos](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md), el control DetailsView proporciona una interfaz de insertar integrada que se puede utilizar cuando se trabaja con un control de origen de datos que admite la inserción. Permiten s Agregar un control DetailsView a esta página por encima de GridView que permanentemente presenta su interfaz de inserción, lo que permite al usuario agregar rápidamente una categoría nueva. Al agregar una nueva categoría en DetailsView, GridView situado debajo de él automáticamente actualizar y mostrar la nueva categoría.

Inicio arrastrando un DetailsView desde el cuadro de herramientas en el diseñador, encima del control GridView, establecer su `ID` propiedad `NewCategory` y borrando la `Height` y `Width` valores de propiedad. Desde la etiqueta inteligente de DetailsView s, enlazarla a las existentes `CategoriesDataSource` y, a continuación, active la casilla Habilitar inserción.


[![Enlazar el DetailsView a la CategoriesDataSource y Habilitar inserción](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image6.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image11.png)

**Figura 6**: enlazar DetailsView a la `CategoriesDataSource` y Habilitar inserción ([haga clic aquí para ver la imagen a tamaño completo](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image12.png))


Para representar permanentemente DetailsView en la interfaz de inserción, establezca su `DefaultMode` propiedad `Insert`.

Tenga en cuenta que la DetailsView tiene cinco BoundFields `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, y `BrochurePath` aunque el `CategoryID` BoundField no se representa en la interfaz de inserción porque su `InsertVisible` propiedad está establecida en `False`. Estos BoundFields existe porque son las columnas devueltas por la `GetCategories()` método, que es lo que el ObjectDataSource invoca para recuperar sus datos. Para insertar, sin embargo, no queremos t desea permitir al usuario especificar un valor para `NumberOfProducts`. Además, es necesario para que puedan cargar una imagen para la nueva categoría así como cargar un archivo PDF para el folleto.

Quitar el `NumberOfProducts` BoundField del DetailsView completamente y actualice la `HeaderText` propiedades de la `CategoryName` y `BrochurePath` BoundFields a categorías y de folleto, respectivamente. A continuación, convertir el `BrochurePath` BoundField en TemplateField y agregar TemplateField nuevo para la imagen, lo que proporciona este nuevo TemplateField un `HeaderText` valor de imagen. Mover el `Picture` TemplateField para que esté entre el `BrochurePath` TemplateField y CommandField.


![Enlazar el DetailsView a la CategoriesDataSource y Habilitar inserción](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image7.gif)

**Figura 7**: enlazar DetailsView a la `CategoriesDataSource` y Habilitar inserción


Si ha convertido el `BrochurePath` BoundField en TemplateField a través del cuadro de diálogo Editar campos, TemplateField incluye un `ItemTemplate`, `EditItemTemplate`, y `InsertItemTemplate`. Solo el `InsertItemTemplate` es necesario, sin embargo, por lo que no dude en quitar las dos plantillas. En este momento la sintaxis declarativa de DetailsView s debería ser similar al siguiente:


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample4.aspx)]

## <a name="adding-fileupload-controls-for-the-brochure-and-picture-fields"></a>Agregar controles FileUpload el folleto y los campos de imagen

Actualmente, el `BrochurePath` TemplateField s `InsertItemTemplate` contiene un cuadro de texto, mientras que la `Picture` TemplateField no contiene ninguna plantilla. Es necesario actualizar estas dos s TemplateField `InsertItemTemplate` para utilizar controles FileUpload.

Desde la etiqueta inteligente de DetailsView s, elija la opción de editar plantillas y, a continuación, seleccione la `BrochurePath` TemplateField s `InsertItemTemplate` en la lista desplegable. Quite el cuadro de texto y, a continuación, arrastre un control FileUpload del cuadro de herramientas en la plantilla. Establecer el control FileUpload s `ID` a `BrochureUpload`. De forma similar, agregar un control FileUpload a la `Picture` TemplateField s `InsertItemTemplate`. Establecer este control FileUpload s `ID` a `PictureUpload`.


[![Agregar un Control FileUpload a la InsertItemTemplate](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image8.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image13.png)

**Figura 8**: agregar un Control FileUpload a la `InsertItemTemplate` ([haga clic aquí para ver la imagen a tamaño completo](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image14.png))


Después de realizar estas adiciones, la sintaxis declarativa de dos TemplateField s será:


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample5.aspx)]

Cuando un usuario agrega una nueva categoría, debe asegurarse de que el folleto y la imagen son del tipo de archivo correcto. Si desea obtener el catálogo, el usuario debe proporcionar un archivo PDF. ¿Para la imagen, es necesario que el usuario puede cargar un archivo de imagen, pero se permiten *cualquier* la imagen de archivo o solo los archivos de imagen de un tipo determinado, como el formato GIF o jpg? Para permitir que distintos tipos de archivos, d debe extender la `Categories` esquema debe incluir una columna que captura el tipo de archivo para que este tipo puede enviarse al cliente a través de `Response.ContentType` en `DisplayCategoryPicture.aspx`. Puesto que no queremos t tiene este tipo de columna, sería conveniente restringir a los usuarios a proporcionar únicamente un tipo de archivo de imagen específico. El `Categories` imágenes existentes de la tabla s son mapas de bits, pero jpg es un formato de archivo más adecuado para las imágenes que sirven a través de Internet.

Si un usuario carga un tipo de archivo incorrecto, es necesario cancelar la inserción y se muestra un mensaje que indica el problema. Agregar un control Web Label debajo de DetailsView. Establecer su `ID` propiedad `UploadWarning`, desactive out su `Text` establecer la propiedad, el `CssClass` propiedad a advertencia y la `Visible` y `EnableViewState` propiedades para `False`. El `Warning` clase CSS que se define en `Styles.css` y representa el texto en una fuente grande, rojo, cursiva, negrita.

> [!NOTE]
> Idealmente, el `CategoryName` y `Description` BoundFields se convertiría TemplateFields y sus interfaces de inserción personalizadas. El `Description` insertar interfaz, por ejemplo, podría es probable que se ajusten mejor a través de un cuadro de texto de varias líneas. Y, puesto que la `CategoryName` columna no acepta `NULL` valores, se debe agregar un control RequiredFieldValidator para asegurarse de que el usuario proporciona un valor para el nuevo nombre de categoría s. Estos pasos se dejan como un ejercicio para el lector. Hacen referencia a [personalizar la interfaz de modificación de datos](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) para obtener información detallada sobre aumentar las interfaces de modificación de datos.


## <a name="step-6-saving-the-uploaded-brochure-to-the-web-server-s-file-system"></a>Paso 6: Guardar el folleto cargado en el sistema de archivos de s de servidor Web

Cuando el usuario especifica los valores para una nueva categoría y hace clic en el botón Insertar, se produce un postback y se abre el flujo de trabajo de inserción. Primero, las operaciones de asignación DetailsView [ `ItemInserting` evento](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.detailsview.iteminserting.aspx) se activa. Siguiente, la s ObjectDataSource `Insert()` se invoca el método, que da como resultado un nuevo registro que se va a agregar a la `Categories` tabla. Después de eso, las operaciones de asignación DetailsView [ `ItemInserted` evento](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.detailsview.iteminserted.aspx) se activa.

Antes de las operaciones de asignación ObjectDataSource `Insert()` se invoca el método, debemos primero asegúrese de que los tipos de archivo adecuado cargados por el usuario y, a continuación, guardar el folleto PDF en el sistema de archivos de s de servidor web. Crear un controlador de eventos para el s DetailsView `ItemInserting` eventos y agregue el código siguiente:


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample6.vb)]

El controlador de eventos inicia haciendo referencia a la `BrochureUpload` control FileUpload desde las plantillas de s DetailsView. A continuación, si se ha cargado un folleto, se examina la extensión de archivo cargado s. Si no es la extensión. PDF, a continuación, una advertencia se muestra, se cancela la inserción y finaliza la ejecución del controlador de eventos.

> [!NOTE]
> Confiar en la extensión de archivo cargado s no es una técnica segura para asegurarse de que el archivo cargado es un documento PDF. El usuario podría tener un documento PDF válido con la extensión `.Brochure`, o puede tomar un documento PDF no y proporcionado un `.pdf` extensión. El contenido binario del archivo s tendría que ser examinada mediante programación con el fin de comprobar el tipo de archivo de forma más concluyente. Estos enfoques exhaustivos, sin embargo, son a menudo excesivos; comprobación de la extensión es suficiente para la mayoría de los escenarios.


Como se describe en el [cargar archivos](uploading-files-vb.md) tutorial, debe tener cuidado al guardar los archivos en el sistema de archivos para esa carga de un usuario s no sobrescribe s otro. Para este tutorial, se intentará usar el mismo nombre que el archivo cargado. Si ya existe un archivo en el `~/Brochures` directorio con ese mismo nombre de archivo, sin embargo, se le anexa un número al final hasta que se encuentre un nombre único. Por ejemplo, si el usuario carga un archivo de catálogo denominado `Meats.pdf`, pero ya hay un archivo denominado `Meats.pdf` en el `~/Brochures` carpeta, se cambiará el nombre de archivo guardado para `Meats-1.pdf`. Si existe, intentaremos `Meats-2.pdf`, y así sucesivamente, hasta que se encuentra un nombre de archivo único.

El siguiente código utiliza el [ `File.Exists(path)` método](https://msdn.microsoft.com/en-us/library/system.io.file.exists.aspx) para determinar si ya existe un archivo con el nombre de archivo especificado. Si es así, continúa probar los nuevos nombres de archivo para el folleto hasta que no se encuentra ningún conflicto.


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample7.vb)]

Una vez que se ha encontrado un nombre de archivo válido, el archivo debe guardarse en el sistema de archivos y las operaciones de asignación ObjectDataSource `brochurePath``InsertParameter` valor debe actualizarse para que este nombre de archivo se escribe en la base de datos. Como hemos visto en la *cargar archivos* tutorial, el archivo puede guardarse con el control FileUpload s `SaveAs(path)` método. Para actualizar las operaciones de asignación ObjectDataSource `brochurePath` parámetro, use el `e.Values` colección.


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample8.vb)]

## <a name="step-7-saving-the-uploaded-picture-to-the-database"></a>Paso 7: Guardar la imagen cargada en la base de datos

Para almacenar la imagen cargada en el nuevo `Categories` registros, es necesario asignar el contenido binario cargado a las operaciones de asignación ObjectDataSource `picture` parámetro en las operaciones de asignación DetailsView `ItemInserting` eventos. Sin embargo, antes de realizar esta asignación, es necesario para asegurarse de que la imagen cargada es un JPG y no algún otro tipo de imagen. Como en el paso 6, permiten s utilizar la extensión de archivo de imagen cargada s para determinar su tipo.

Mientras el `Categories` tabla permite `NULL` los valores para la `Picture` columna, todas las categorías actualmente tiene una imagen. Permiten s obligar al usuario a proporcionar una imagen al agregar una nueva categoría a través de esta página. El código siguiente se comprueba para asegurarse de que se ha cargado una imagen y que tiene una extensión adecuada.


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample9.vb)]

Este código debe colocarse *antes de* el código del paso 6 para que si hay un problema con la carga de la imagen, el controlador de eventos finalizará antes de guarda el archivo de catálogo en el sistema de archivos.

Suponiendo que se ha cargado un archivo adecuado, asigne el contenido binario cargado para el valor del parámetro imagen con la siguiente línea de código:


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample10.vb)]

## <a name="the-completeiteminsertingevent-handler"></a>La sección completa`ItemInserting`controlador de eventos

Por integridad, este es el `ItemInserting` controlador de eventos en su totalidad:


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample11.vb)]

## <a name="step-8-fixing-thedisplaycategorypictureaspxpage"></a>Paso 8: Corregir la`DisplayCategoryPicture.aspx`página

Permiten s Tómese un momento para probar la interfaz de inserción y `ItemInserting` controlador de eventos que se creó durante los últimos pasos. Visite la `UploadInDetailsView.aspx` página a través de un explorador y el intento de agregar una categoría, pero omita la imagen, o especificar una imagen JPG no o un folleto no PDF. En cualquiera de estos casos, se mostrará un mensaje de error y cancela el flujo de trabajo de inserción.


[![Un mensaje de advertencia es muestra si se carga un tipo de archivo no válido](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image9.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image15.png)

**Figura 9**: un mensaje de advertencia es muestra si se carga un tipo de archivo no válido ([haga clic aquí para ver la imagen a tamaño completo](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image16.png))


Una vez haya comprobado que la página requiere una imagen que se cargan y t ganado aceptan archivos PDF no son o no JPG, agregue una nueva categoría con una imagen JPG válida, si deja el campo de folleto vacío. Después de hacer clic en el botón Insertar, la página se devolución de datos y se agregará un nuevo registro a la `Categories` tabla con el contenido binario de imagen cargada s almacenada directamente en la base de datos. GridView se actualiza y muestra una fila de la categoría recién agregada, pero, como se muestra en la figura 10, la nueva imagen s de categoría no se representa correctamente.


[![S que no se muestra la imagen a la nueva categoría](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image10.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image17.png)

**Figura 10**: s de la nueva categoría no se muestra la imagen ([haga clic aquí para ver la imagen a tamaño completo](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image18.png))


La razón no se muestra la nueva imagen es que la `DisplayCategoryPicture.aspx` página que devuelve una imagen de la categoría especificada s está configurada para procesar los mapas de bits que tienen un encabezado OLE. Este encabezado 78 bytes se extrae de la `Picture` hacer copia de contenido binario de columna s antes de enviarlos al cliente. Pero el archivo JPG que se acaba de cargar para la nueva categoría no tiene este encabezado OLE; por lo tanto, bytes válidos, es necesarios que se quitan de los datos binarios de imagen s.

Dado que ahora hay dos mapas de bits con encabezados OLE y los archivos JPEG en la `Categories` tabla, necesitamos actualizar `DisplayCategoryPicture.aspx` por lo que no el encabezado OLE quitando las ocho categorías originales y omite esta eliminación de los registros de la categoría más reciente. En nuestro tutorial siguiente examinaremos cómo actualizar una imagen de registro s existente y se actualizará todas las fotografías de categoría anterior para que resulten jpg. Por ahora, sin embargo, utilice el siguiente código en `DisplayCategoryPicture.aspx` para retirar los encabezados OLE solo para esas categorías de ocho originales:


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample12.vb)]

Con este cambio, ahora se representa correctamente la imagen JPG en GridView.


[![Las imágenes JPG para las nuevas categorías son represente correctamente](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image11.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image19.png)

**Figura 11**: el imágenes JPG para nuevas categorías son represente correctamente ([haga clic aquí para ver la imagen a tamaño completo](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image20.png))


## <a name="step-9-deleting-the-brochure-in-the-face-of-an-exception"></a>Paso 9: Eliminando el folleto en el caso de una excepción

Uno de los desafíos de almacenar datos binarios en el sistema de archivos de s de servidor web es que presenta una desconexión entre el modelo de datos y sus datos binarios. Por lo tanto, cada vez que se elimina un registro, también deben quitarse los correspondientes datos binarios en el sistema de archivos. Esto puede entran en juego al insertar, también. Considere el siguiente escenario: un usuario agrega una nueva categoría, especificar un folleto e imagen válida. Al hacer clic en el botón Insertar, se produce un postback y las operaciones de asignación DetailsView `ItemInserting` desencadena el evento, guardar el folleto para el sistema de archivos de s de servidor web. Siguiente, la s ObjectDataSource `Insert()` se invoca el método, que llama el `CategoriesBLL` clase s `InsertWithPicture` método, que llama a la `CategoriesTableAdapter` s `InsertWithPicture` método.

Ahora, ¿qué ocurre si la base de datos está sin conexión o si hay un error en la `INSERT` instrucción SQL? Claramente la INSERCIÓN se producirá un error, por lo que ninguna fila de la categoría nueva se agregará a la base de datos. Pero todavía tenemos el archivo cargado folleto sentado en el sistema de archivos de s de servidor web. Este archivo debe eliminarse en el caso de una excepción durante el flujo de trabajo de inserción.

Según lo descrito anteriormente en la [BLL - control y las excepciones de nivel de la capa DAL de una página ASP.NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md) tutorial, cuando se produce una excepción desde dentro de las profundidades de la arquitectura afecta al funcionamiento a través de las distintas capas. En la capa de presentación, podemos determinar si se ha producido una excepción de las operaciones de asignación DetailsView `ItemInserted` eventos. Este controlador de eventos también proporciona los valores de las operaciones de asignación ObjectDataSource `InsertParameters`. Por lo tanto, podemos crear un controlador de eventos para el `ItemInserted` eventos que comprueba si se produjo una excepción y, si es así, elimina el archivo especificado por la s ObjectDataSource `brochurePath` parámetro:


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample13.vb)]

## <a name="summary"></a>Resumen

Hay una serie de pasos que deben realizarse con el fin de proporcionar una interfaz basada en web para agregar los registros que incluyen datos binarios. Si se va a almacenar los datos binarios directamente en la base de datos, lo más probable es que necesite actualizar la arquitectura, agregar métodos específicos para controlar el caso donde se insertan los datos binarios. Una vez que se ha actualizado la arquitectura, el siguiente paso es crear la interfaz de inserción, que puede realizarse mediante un DetailsView que se ha personalizado para que incluya un control FileUpload para cada campo de datos binarios. Los datos cargados, a continuación, se guarda en el sistema de archivos de web server s o asignados a un parámetro de origen de datos en las operaciones de asignación DetailsView `ItemInserting` controlador de eventos.

Guardar datos binarios en el sistema de archivos requiere una mayor planeación que guardar los datos directamente en la base de datos. Un esquema de nomenclatura se debe elegir con el fin de evitar una carga de usuario s sobrescribiendo s otro. Además, se deben realizar pasos adicionales para eliminar el archivo cargado si se produce un error en la inserción de base de datos.

Ahora tiene la capacidad de agregar nuevas categorías para el sistema con un folleto y la imagen, pero se ve todavía para observar cómo actualizar un categoría s binarios los datos existentes o cómo se quitan los datos binarios de una categoría se eliminó correctamente. Exploraremos estos dos temas en el tutorial siguiente.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [ *SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Los revisores iniciales para este tutorial eran Dave Gardner, Teresa Murphy y Bernadette Leigh. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](displaying-binary-data-in-the-data-web-controls-vb.md)
[Siguiente](updating-and-deleting-existing-binary-data-vb.md)
