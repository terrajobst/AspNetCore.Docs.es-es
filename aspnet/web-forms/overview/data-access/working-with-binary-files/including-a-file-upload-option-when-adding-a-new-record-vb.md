---
uid: web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-vb
title: Incluido un archivo de opción de carga al agregar un nuevo registro (VB) | Microsoft Docs
author: rick-anderson
description: Este tutorial muestra cómo crear una interfaz Web que permite al usuario introducir datos de texto y cargar los archivos binarios. Para ilustrar las opciones disponibles t...
ms.author: aspnetcontent
ms.date: 03/27/2007
ms.assetid: 5776281d-4637-4d1e-a65b-2621d2cade44
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-vb
msc.type: authoredcontent
ms.openlocfilehash: eac86318136d6f28ecf6551b8b8a6b691a8969d7
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807757"
---
<a name="including-a-file-upload-option-when-adding-a-new-record-vb"></a>Incluir una opción de carga de archivo al agregar un nuevo registro (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_56_VB.exe) o [descargar PDF](including-a-file-upload-option-when-adding-a-new-record-vb/_static/datatutorial56vb1.pdf)

> Este tutorial muestra cómo crear una interfaz Web que permite al usuario introducir datos de texto y cargar los archivos binarios. Para ilustrar las opciones disponibles para almacenar datos binarios, se guardará un archivo en la base de datos mientras la otra se almacena en el sistema de archivos.


## <a name="introduction"></a>Introducción

En los dos tutoriales anteriores, analizamos las técnicas para almacenar datos binarios que está asociados con el modelo de datos de aplicación s, examinado cómo utilizar el control FileUpload para enviar archivos desde el cliente al servidor web y ha visto cómo presentar estos datos binarios en un datos W control EB. Se ve que tratan sobre cómo asociar los datos cargados con el modelo de datos, sin embargo.

En este tutorial crearemos una página web para agregar una nueva categoría. Además de los cuadros de texto para el nombre de categoría s y la descripción, esta página debe incluir dos controles FileUpload uno para la nueva imagen de categoría s y otra para el folleto. Se almacenará la imagen cargada directamente en el nuevo registro s `Picture` columna, mientras que el folleto se guardará en el `~/Brochures` carpeta con la ruta de acceso al archivo guardado en el nuevo registro s `BrochurePath` columna.

Antes de crear esta nueva página web, necesitamos actualizar la arquitectura. El `CategoriesTableAdapter` consulta principal s no recupera el `Picture` columna. Por lo tanto, el generado automáticamente `Insert` método sólo tiene entradas para el `CategoryName`, `Description`, y `BrochurePath` campos. Por lo tanto, es necesario crear un método adicional en el TableAdapter que solicita los cuatro `Categories` campos. La `CategoriesBLL` clase en la capa de lógica empresarial también tendrá que actualizarse.

## <a name="step-1-adding-aninsertwithpicturemethod-to-thecategoriestableadapter"></a>Paso 1: Agregar un`InsertWithPicture`método a la`CategoriesTableAdapter`

Cuando se crea el `CategoriesTableAdapter` en el [crear una capa de acceso a datos](../introduction/creating-a-data-access-layer-vb.md) tutorial, hemos configurado para generar automáticamente `INSERT`, `UPDATE`, y `DELETE` las instrucciones en función de la consulta principal. Además, le indicamos emplear el enfoque directo de base de datos, que crea los métodos TableAdapter `Insert`, `Update`, y `Delete`. Estos métodos ejecutan los autogenerado `INSERT`, `UPDATE`, y `DELETE` instrucciones y, por lo tanto, aceptan parámetros de entrada en función de las columnas devueltas por la consulta principal. En el [cargar archivos](uploading-files-vb.md) tutorial hemos ampliados la `CategoriesTableAdapter` consulta principal de s que se utilizará la `BrochurePath` columna.

Puesto que la `CategoriesTableAdapter` no hace referencia la consulta principal s el `Picture` columna, nos podemos agregar un nuevo registro ni actualizar un registro existente con un valor para el `Picture` columna. Para capturar esta información, o bien podemos crear un nuevo método en TableAdapter que se utiliza específicamente para insertar un registro con datos binarios o podemos personalizar la autogenerado `INSERT` instrucción. El problema con la personalización de la autogenerado `INSERT` instrucción es que nos se corre el riesgo nuestro personalizaciones sobrescribir por el asistente. Por ejemplo, imagine que se personaliza la `INSERT` instrucción para incluir el uso de la `Picture` columna. Esto actualizará los TableAdapter s `Insert` método para incluir un parámetro de entrada adicional para los datos binarios de imagen s s de categoría. A continuación, podríamos crear un método en la capa de lógica empresarial para usar este método DAL e invocar este método BLL a través de la capa de presentación y todo funcionaría maravillosamente. Es decir, hasta la próxima vez que hemos configurado el TableAdapter mediante el Asistente para configuración de TableAdapter. Tan pronto como complete el asistente, nuestro personalizaciones a la `INSERT` se sobrescribirán la instrucción, el `Insert` método revertiría a su formato antiguo y ya no se compilará el código!

> [!NOTE]
> Esta serie de molestias es un problema al utilizar procedimientos almacenados en lugar de instrucciones SQL ad hoc. Un futuro tutorial explorará el uso de procedimientos almacenados en lugar de instrucciones SQL ad hoc en la capa de acceso a datos.


Para evitar esta posibilidad dolor de cabeza, en lugar de personalización de las instrucciones SQL generadas automáticamente permite s e en su lugar, cree un nuevo método del TableAdapter. Este método, denominado `InsertWithPicture`, acepte los valores para el `CategoryName`, `Description`, `BrochurePath`, y `Picture` columnas y ejecutar un `INSERT` instrucción que almacena los cuatro valores de un nuevo registro.

Abra el conjunto de datos con tipo y, desde el diseñador, haga doble clic en el `CategoriesTableAdapter` encabezado s y elija Agregar consulta en el menú contextual. Esto inicia al Asistente para configuración de TableAdapter Query, que comienza con la que nos pregunta cómo la consulta de TableAdapter debe tener acceso a la base de datos. Elija usar instrucciones SQL y haga clic en siguiente. El siguiente paso le pide el tipo de consulta que se genere. Desde que creamos re creando una consulta para agregar un nuevo registro a la `Categories` de tabla, elija Insertar y haga clic en siguiente.


[![Seleccione la opción de INSERCIÓN](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image1.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image1.png)

**Figura 1**: seleccione la opción Insertar ([haga clic aquí para ver imagen en tamaño completo](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image2.png))


Ahora tenemos que especificar el `INSERT` instrucción SQL. El Asistente sugiere automáticamente un `INSERT` instrucción correspondiente a la consulta principal de TableAdapter s. En este caso, lo s un `INSERT` instrucción que se inserta el `CategoryName`, `Description`, y `BrochurePath` valores. La instrucción Update para que la `Picture` columna se incluye junto con un `@Picture` parámetro, de este modo:


[!code-sql[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample1.sql)]

La última pantalla del asistente nos pide al nombre del nuevo método del TableAdapter. Escriba `InsertWithPicture` y haga clic en Finalizar.


[![Nombre de la nueva InsertWithPicture TableAdapter (método)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image2.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image3.png)

**Figura 2**: nombre del nuevo método TableAdapter `InsertWithPicture` ([haga clic aquí para ver imagen en tamaño completo](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image4.png))


## <a name="step-2-updating-the-business-logic-layer"></a>Paso 2: Actualización de la capa de lógica de negocios

Puesto que la capa de presentación sólo debe interactuar con la capa de lógica empresarial en lugar de omitir para ir directamente a la capa de acceso a datos, es necesario crear un método BLL que invoca el método DAL que acabamos de crear (`InsertWithPicture`). Para este tutorial, cree un método en el `CategoriesBLL` clase denominada `InsertWithPicture` que acepta como entrada tres `String` s y un `Byte` matriz. El `String` son parámetros de entrada para el nombre de categoría s, descripción y ruta de acceso de archivo de folleto, mientras que el `Byte` matriz es para el contenido binario de la imagen de la categoría s. Como se muestra en el código siguiente, este método BLL invoca el método DAL correspondiente:


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample2.vb)]

> [!NOTE]
> Asegúrese de que ha guardado el conjunto de datos con tipo antes de agregar el `InsertWithPicture` método a la capa BLL. Puesto que la `CategoriesTableAdapter` código de la clase es generado automáticamente basándose en el conjunto de datos con tipo, si don t primero guarde los cambios en el conjunto de datos con tipo el `Adapter` propiedad ganó t saber sobre el `InsertWithPicture` método.


## <a name="step-3-listing-the-existing-categories-and-their-binary-data"></a>Paso 3: Enumerar las categorías existentes y sus datos binarios

En este tutorial crearemos una página que permite que un usuario final agregar una nueva categoría al sistema, que proporciona una imagen y un folleto para la nueva categoría. En el [tutorial anterior](displaying-binary-data-in-the-data-web-controls-vb.md) se usa un control GridView con un TemplateField y ImageField para mostrar cada nombre de categoría s, descripción, imagen y un vínculo para descargar su folleto. Permiten s replicar esa funcionalidad para este tutorial, la creación de una página que enumera todas las categorías existentes y permite crear otros nuevos.

Comience abriendo la `DisplayOrDownload.aspx` página desde la `BinaryData` carpeta. Vaya a la vista del origen y copie la GridView y ObjectDataSource s sintaxis declarativa, pegándolo en la `<asp:Content>` elemento `UploadInDetailsView.aspx`. Además, no olvide copiar a través de la `GenerateBrochureLink` método de la clase de código subyacente de `DisplayOrDownload.aspx` a `UploadInDetailsView.aspx`.


[![Copie y pegue la sintaxis declarativa de DisplayOrDownload.aspx a UploadInDetailsView.aspx](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image3.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image5.png)

**Figura 3**: copiar y pegar la sintaxis declarativa de `DisplayOrDownload.aspx` a `UploadInDetailsView.aspx` ([haga clic aquí para ver imagen en tamaño completo](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image6.png))


Después de copiar la sintaxis declarativa y `GenerateBrochureLink` método a través de la `UploadInDetailsView.aspx` página, vea la página a través de un explorador para asegurarse de que todo lo que se copia correctamente. Debería ver un GridView las ocho categorías de lista que incluye un vínculo para descargar el folleto, así como la imagen de la categoría s.


[![Ahora debería ver cada categoría junto con sus datos binarios](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image4.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image7.png)

**Figura 4**: se debe ahora vea cada categoría junto con sus datos binarios ([haga clic aquí para ver imagen en tamaño completo](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image8.png))


## <a name="step-4-configuring-thecategoriesdatasourceto-support-inserting"></a>Paso 4: Configurar el`CategoriesDataSource`para insertar de soporte técnico

El `CategoriesDataSource` ObjectDataSource utilizado por el `Categories` GridView actualmente no proporciona la capacidad para insertar datos. Para admitir la inserción a través de este control de origen de datos, es necesario asignar su `Insert` método a un método en su objeto subyacente, `CategoriesBLL`. En particular, queremos que se asigne a la `CategoriesBLL` método que agregamos en el paso 2 `InsertWithPicture`.

Iniciar, haga clic en el vínculo Configurar origen de datos de la etiqueta inteligente de s de ObjectDataSource. La primera pantalla muestra el objeto de origen de datos está configurado para trabajar, `CategoriesBLL`. Deje esta opción como-está y haga clic en siguiente para pasar a la pantalla de definir los métodos de datos. Mover a la ficha Insertar y elija el `InsertWithPicture` método en la lista desplegable. Haga clic en Finalizar para completar al asistente.


[![Configurar el origen ObjectDataSource para usar el método InsertWithPicture](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image5.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image9.png)

**Figura 5**: configurar el origen ObjectDataSource para usar el `InsertWithPicture` método ([haga clic aquí para ver imagen en tamaño completo](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image10.png))


> [!NOTE]
> Al finalizar al asistente, Visual Studio puede pedirle si desea actualizar campos y las claves, que volverá a generar los datos Web controla los campos. Elija No, porque si elige Sí, se sobrescribirá cualquier personalización de campo que se ha realizado.


Después de completar el asistente, el origen ObjectDataSource ahora incluirá un valor para su `InsertMethod` propiedad como `InsertParameters` para las columnas de la categoría de cuatro, como el siguiente marcado declarativo muestra:


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample3.aspx)]

## <a name="step-5-creating-the-inserting-interface"></a>Paso 5: Creación de la interfaz de inserción

En primer lugar como se explica en el [una visión general de insertar, actualizar y eliminar datos](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md), el control DetailsView proporciona una interfaz de inserción integrada que puede utilizarse cuando se trabaja con un control de origen de datos que admite la inserción. Permitir s Agregar un control DetailsView a esta página por encima de GridView que representará permanentemente su interfaz de inserción, lo que permite al usuario agregar rápidamente una nueva categoría. Al agregar una nueva categoría en DetailsView, GridView debajo de ella se actualice automáticamente y mostrar la nueva categoría.

Inicio arrastrando un DetailsView desde el cuadro de herramientas hasta el diseñador por encima del control GridView, establecer su `ID` propiedad `NewCategory` y borrarlos el `Height` y `Width` los valores de propiedad. En la etiqueta inteligente s DetailsView, enlazarlo a existente `CategoriesDataSource` y, a continuación, active la casilla Habilitar inserción.


[![Enlazar el CategoriesDataSource DetailsView y Habilitar inserción](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image6.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image11.png)

**Figura 6**: enlazar a DetailsView el `CategoriesDataSource` y Habilitar inserción ([haga clic aquí para ver imagen en tamaño completo](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image12.png))


Para representar permanentemente DetailsView en la interfaz de inserción, establezca su `DefaultMode` propiedad `Insert`.

Tenga en cuenta que DetailsView tiene cinco BoundFields `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, y `BrochurePath` aunque el `CategoryID` BoundField no se representa en la interfaz de inserción porque su `InsertVisible` propiedad está establecida en `False`. Estos BoundFields existe porque son las columnas devueltas por la `GetCategories()` método, que es lo que el ObjectDataSource invoca para recuperar sus datos. Para insertar, sin embargo, no queremos t desea permitir al usuario especificar un valor para `NumberOfProducts`. Además, es necesario para que puedan cargar una imagen para la nueva categoría así como cargar un archivo PDF para el folleto.

Quitar el `NumberOfProducts` BoundField del DetailsView completamente y actualice el `HeaderText` propiedades de la `CategoryName` y `BrochurePath` BoundFields a la categoría y el folleto, respectivamente. A continuación, convertir la `BrochurePath` BoundField en TemplateField y agregar un nuevo TemplateField para la imagen, lo que ofrece esta nueva TemplateField un `HeaderText` valor de la imagen. Mover el `Picture` TemplateField para que esté entre el `BrochurePath` TemplateField y CommandField.


![Enlazar el CategoriesDataSource DetailsView y Habilitar inserción](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image7.gif)

**Figura 7**: enlazar DetailsView para el `CategoriesDataSource` y Habilitar inserción


Si ha convertido el `BrochurePath` BoundField en TemplateField a través del cuadro de diálogo Editar campos, TemplateField incluye un `ItemTemplate`, `EditItemTemplate`, y `InsertItemTemplate`. Solo el `InsertItemTemplate` es necesario, sin embargo, por lo que no dude en quitar las dos plantillas. En este momento su sintaxis declarativa de DetailsView s debería ser similar al siguiente:


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample4.aspx)]

## <a name="adding-fileupload-controls-for-the-brochure-and-picture-fields"></a>Agregar controles FileUpload para los campos de imagen y un folleto

Actualmente, el `BrochurePath` TemplateField s `InsertItemTemplate` contiene un cuadro de texto, mientras que el `Picture` TemplateField no contiene ninguna plantilla. Es necesario actualizar estas dos s TemplateField `InsertItemTemplate` para utilizar controles FileUpload.

En la etiqueta inteligente de DetailsView s, elija la opción de editar plantillas y, a continuación, seleccione el `BrochurePath` TemplateField s `InsertItemTemplate` en la lista desplegable. Quite el cuadro de texto y, a continuación, arrastre un control FileUpload desde el cuadro de herramientas en la plantilla. Establecer el control FileUpload s `ID` a `BrochureUpload`. De forma similar, agregue un control FileUpload para el `Picture` TemplateField s `InsertItemTemplate`. Establecer este control FileUpload s `ID` a `PictureUpload`.


[![Agregar un Control FileUpload a la InsertItemTemplate](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image8.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image13.png)

**Figura 8**: agregar un Control FileUpload el `InsertItemTemplate` ([haga clic aquí para ver imagen en tamaño completo](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image14.png))


Después de realizar estas adiciones, la sintaxis declarativa de dos TemplateField s será:


[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample5.aspx)]

Cuando un usuario agrega una nueva categoría, queremos asegurarnos de que el folleto y la imagen son del tipo de archivo correcto. Obtener el catálogo, el usuario debe proporcionar un archivo PDF. ¿Para la imagen, necesitamos que el usuario puede cargar un archivo de imagen, pero se permiten *cualquier* archivo o sólo los archivos de imagen de un tipo determinado, por ejemplo, con formato GIF o jpg de la imagen? Para permitir diferentes tipos de archivo, d necesitamos ampliar el `Categories` esquema para incluir una columna que captura el tipo de archivo para que este tipo se puede enviar al cliente a través de `Response.ContentType` en `DisplayCategoryPicture.aspx`. Puesto que no queremos t tiene este tipo de columna, sería recomendable restringir a los usuarios para que solo proporciona un tipo de archivo de imagen específico. El `Categories` imágenes existentes de la tabla s son mapas de bits, pero jpg están un formato de archivo más adecuado para las imágenes que sirven a través de Internet.

Si un usuario carga un tipo de archivo incorrecto, se debe cancelar la inserción y mostrará un mensaje que indica el problema. Agregue un control Web de la etiqueta debajo de DetailsView. Establecer su `ID` propiedad `UploadWarning`, desactive out su `Text` establecer la propiedad, el `CssClass` propiedad a la advertencia y la `Visible` y `EnableViewState` propiedades para `False`. El `Warning` clase CSS que se define en `Styles.css` y representa el texto en una fuente grande, rojo, cursiva, negrita.

> [!NOTE]
> Idealmente, el `CategoryName` y `Description` BoundFields se convertiría TemplateFields y sus interfaces de inserción personalizadas. El `Description` insertar interfaz, por ejemplo, sería probablemente adaptarla mejor a través de un cuadro de texto multilínea. Y, puesto que la `CategoryName` columna no acepta `NULL` valores, se debe agregar un control RequiredFieldValidator para asegurarse de que el usuario proporciona un valor para el nuevo nombre de categoría s. Estos pasos se dejan como un ejercicio para el lector. Vuelva a consultar [personalizar la interfaz de modificación de datos](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) para una visión detallada de aumento de las interfaces de modificación de datos.


## <a name="step-6-saving-the-uploaded-brochure-to-the-web-server-s-file-system"></a>Paso 6: Guardar el folleto cargado en el sistema de archivos de s de servidor Web

Cuando el usuario especifica los valores para una nueva categoría y hace clic en el botón de inserción, se produce un postback y se desarrolla el flujo de trabajo de inserción. Primero, la s DetailsView [ `ItemInserting` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.iteminserting.aspx) se activa. A continuación, la s ObjectDataSource `Insert()` se invoca el método, que da como resultado un nuevo registro que se agrega a la `Categories` tabla. Después de eso, la s DetailsView [ `ItemInserted` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.iteminserted.aspx) se activa.

Antes de las operaciones de asignación ObjectDataSource `Insert()` se invoca el método, debemos en primer lugar asegúrese de que los tipos de archivo adecuados se han cargado por el usuario y, a continuación, guarde el folleto PDF en el sistema de archivos de s de servidor web. Crear un controlador de eventos para el s DetailsView `ItemInserting` eventos y agregue el código siguiente:


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample6.vb)]

El controlador de eventos se inicia haciendo referencia a la `BrochureUpload` control FileUpload desde las plantillas de s DetailsView. A continuación, si se ha cargado un folleto, se examina la extensión de s del archivo cargado. Si no es la extensión. Se muestra el PDF, a continuación, una advertencia, se cancela la inserción y finaliza la ejecución del controlador de eventos.

> [!NOTE]
> Depender de la extensión de s del archivo cargado no es una técnica descuidada para asegurarse de que el archivo cargado no es un documento PDF. El usuario podría tener un documento PDF válido con la extensión `.Brochure`, o podría tomar un documento que no son PDF y dado que un `.pdf` extensión. El contenido binario del archivo s necesitaría examinarse mediante programación con el fin de comprobar el tipo de archivo de forma más concluyente. Tales métodos completas, sin embargo, a menudo son excesivos; comprobación de la extensión es suficiente para la mayoría de los escenarios.


Como se describe en el [cargar archivos](uploading-files-vb.md) tutorial, debe tener cuidado al guardar archivos al sistema de archivos para esa carga de un usuario s no sobrescribe s otro. Para este tutorial, se intentará usar el mismo nombre que el archivo cargado. Si ya existe un archivo en el `~/Brochures` directorio con ese mismo nombre de archivo, sin embargo, se deberá anexar un número al final hasta que se encuentra un nombre único. Por ejemplo, si el usuario carga un archivo de catálogo denominado `Meats.pdf`, pero ya hay un archivo denominado `Meats.pdf` en el `~/Brochures` carpeta, cambiaremos el nombre del archivo guardado para `Meats-1.pdf`. Si no existe que intentaremos `Meats-2.pdf`, y así sucesivamente, hasta que se encuentra un nombre de archivo único.

El siguiente código utiliza el [ `File.Exists(path)` método](https://msdn.microsoft.com/library/system.io.file.exists.aspx) para determinar si ya existe un archivo con el nombre de archivo especificado. Si es así, continúa probar los nuevos nombres de archivo para el folleto hasta que no se encuentra ningún conflicto.


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample7.vb)]

Una vez que se ha encontrado un nombre de archivo válido, el archivo debe guardarse en el sistema de archivos y la s ObjectDataSource `brochurePath``InsertParameter` valor debe actualizarse para que este nombre de archivo se escribe en la base de datos. Como vimos en el *cargar archivos* tutorial, el archivo puede guardarse con el control FileUpload s `SaveAs(path)` método. Para actualizar la s ObjectDataSource `brochurePath` parámetro, use el `e.Values` colección.


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample8.vb)]

## <a name="step-7-saving-the-uploaded-picture-to-the-database"></a>Paso 7: Guardar la imagen cargada en la base de datos

Para almacenar la imagen cargada en el nuevo `Categories` registros, se debe asignar el contenido binario cargado al origen ObjectDataSource s `picture` parámetro en la s DetailsView `ItemInserting` eventos. Sin embargo, antes de realizar esta asignación, es necesario para asegurarse de que la imagen cargada es un JPG y no algún otro tipo de imagen. Como en el paso 6, permiten usar la extensión de archivo de imagen cargados s para determinar su tipo de s.

Mientras el `Categories` table permite `NULL` valores para la `Picture` columna, todas las categorías actualmente tiene una imagen. Permiten s obligar al usuario para proporcionar una imagen al agregar una nueva categoría a través de esta página. El código siguiente se comprueba para asegurarse de que se ha cargado una imagen y que tiene una extensión adecuada.


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample9.vb)]

Este código debe colocarse *antes* el código del paso 6 para que si hay un problema con la carga de imagen, el controlador de eventos finalizará antes de que el archivo de catálogo se guarda en el sistema de archivos.

Suponiendo que se ha cargado un archivo adecuado, puede asignar el contenido binario cargado para el valor del parámetro imagen con la siguiente línea de código:


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample10.vb)]

## <a name="the-completeiteminsertingevent-handler"></a>La completa`ItemInserting`controlador de eventos

Por integridad, aquí está el `ItemInserting` controlador de eventos en su totalidad:


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample11.vb)]

## <a name="step-8-fixing-thedisplaycategorypictureaspxpage"></a>Paso 8: Corregir la`DisplayCategoryPicture.aspx`página

Let s dedique un momento para probar la interfaz de inserción y `ItemInserting` controlador de eventos que se creó durante los últimos pasos. Visite el `UploadInDetailsView.aspx` página a través de un explorador y el intento de agregar una categoría, pero omita la imagen o especificar una imagen que no son JPG o un folleto que no son PDF. En cualquiera de estos casos, se mostrará un mensaje de error y cancela el flujo de trabajo de inserción.


[![Es de un mensaje de advertencia aparece si se carga un tipo de archivo no válido](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image9.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image15.png)

**Figura 9**: un mensaje de advertencia es muestra si se carga un tipo de archivo no válido ([haga clic aquí para ver imagen en tamaño completo](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image16.png))


Una vez haya comprobado que la página requiere una imagen que se cargará y t ganado aceptan archivos PDF no o que no son JPG, agregue una nueva categoría válido imagen JPG, dejar vacío el campo de folleto. Después de hacer clic en el botón de inserción, la página se devolución de datos y se agregará un nuevo registro a la `Categories` tabla con el contenido binario de la imagen cargada s almacenada directamente en la base de datos. El control GridView se actualiza y muestra una fila para la categoría recién agregada, pero, como se muestra en la figura 10, la nueva imagen de s de categoría no se represente correctamente.


[![La nueva categoría s que no se muestra la imagen](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image10.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image17.png)

**Figura 10**: no se muestra la imagen de s de la nueva categoría ([haga clic aquí para ver imagen en tamaño completo](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image18.png))


La razón no se muestra la nueva imagen es que la `DisplayCategoryPicture.aspx` página que devuelve una imagen de la categoría especificada s está configurada para procesar los mapas de bits que tienen un encabezado OLE. Este encabezado 78 bytes se elimina de la `Picture` hacer una copia del contenido binario de la columna s antes de enviarlos al cliente. Pero el archivo JPG que se acaba de cargar para la nueva categoría no tiene este encabezado OLE; por lo tanto, se quitan de los datos binarios de imagen s los bytes válidas, es necesarios.

Puesto que ahora hay dos mapas de bits con encabezados OLE y los archivos JPEG en la `Categories` tabla, es necesario actualizar `DisplayCategoryPicture.aspx` para que lo hace el encabezado OLE quitando las ocho categorías originales y se omite esta eliminación para los registros más recientes de la categoría. En nuestro siguiente tutorial examinaremos cómo actualizar una imagen existente de registro s y se actualizará todas las imágenes de la categoría anterior para que sean jpg. Por ahora, sin embargo, puede usar el siguiente código en `DisplayCategoryPicture.aspx` para quitar los encabezados OLE solo para esas ocho categorías originales:


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample12.vb)]

Con este cambio, ahora se representa correctamente la imagen JPG en GridView.


[![Las imágenes JPG de nuevas categorías son represente correctamente](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image11.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image19.png)

**Figura 11**: las imágenes JPG de nuevas categorías son represente correctamente ([haga clic aquí para ver imagen en tamaño completo](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image20.png))


## <a name="step-9-deleting-the-brochure-in-the-face-of-an-exception"></a>Paso 9: Eliminar el folleto en caso de una excepción

Uno de los desafíos de almacenar datos binarios en el sistema de archivos de s de servidor web es que introduce una desconexión entre el modelo de datos y sus datos binarios. Por lo tanto, cada vez que se elimina un registro, los datos binarios correspondientes en el sistema de archivos también deben quitarse. Esto puede entran en juego cuando se insertan, también. Considere el siguiente escenario: un usuario agrega una nueva categoría, especificar una imagen válida y un folleto. Tras hacer clic en el botón de inserción, se produce un postback y la s DetailsView `ItemInserting` desencadena el evento, guardando el folleto para el sistema de archivos de s de servidor web. A continuación, la s ObjectDataSource `Insert()` se invoca el método, que llama a la `CategoriesBLL` clase s `InsertWithPicture` método que llama el `CategoriesTableAdapter` s `InsertWithPicture` método.

Ahora, ¿qué ocurre si la base de datos está sin conexión o si hay un error en la `INSERT` instrucción SQL? Claramente la INSERCIÓN se producirá un error, por lo que no hay ninguna fila de la categoría nueva se agregará a la base de datos. Pero aún tendremos el archivo cargado folleto sentado en el sistema de archivos de s de servidor web. Este archivo debe eliminarse en el caso de una excepción durante el flujo de trabajo de inserción.

Como se explicó anteriormente en el [BLL - control y excepciones de nivel DAL en una página ASP.NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md) tutorial, cuando se produce una excepción desde dentro de las profundidades de la arquitectura afecta al funcionamiento a través de las distintas capas. En la capa de presentación, podemos determinar si se ha producido una excepción de s DetailsView `ItemInserted` eventos. Este controlador de eventos también proporciona los valores de las operaciones de asignación ObjectDataSource `InsertParameters`. Por lo tanto, podemos crear un controlador de eventos para el `ItemInserted` eventos que comprueba si se ha producido una excepción y, si es así, elimina el archivo especificado por la s ObjectDataSource `brochurePath` parámetro:


[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample13.vb)]

## <a name="summary"></a>Resumen

Hay una serie de pasos que deben realizarse con el fin de proporcionar una interfaz basada en web para agregar los registros que incluyen datos binarios. Si se va a almacenar los datos binarios directamente en la base de datos, lo más probable es que deberá actualizar la arquitectura, la adición de métodos específicos para controlar el caso donde se insertan los datos binarios. Una vez que se ha actualizado la arquitectura, el siguiente paso es crear la interfaz de inserción, que puede realizarse mediante un DetailsView en la que se ha personalizado para incluir un control FileUpload para cada campo de datos binarios. Los datos cargados, a continuación, se guarda en el sistema de archivos de s de servidor web o asignados a un parámetro de origen de datos en la s DetailsView `ItemInserting` controlador de eventos.

Guardar datos binarios en el sistema de archivos requiere más planeación que guardar los datos directamente en la base de datos. Debe elegir un esquema de nomenclatura con el fin de evitar una carga de usuario s sobrescribiendo s otro. Además, se deben realizar pasos adicionales para eliminar el archivo cargado si se produce un error en la inserción de base de datos.

Ahora tenemos la capacidad de agregar nuevas categorías para el sistema con un folleto y la imagen, pero se ve aún a mirar cómo actualizar un categoría s los datos binarios existentes o quitar correctamente los datos binarios para una categoría eliminada. Exploraremos estos dos temas en el siguiente tutorial.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Los revisores para este tutorial fueron Dave Gardner, Teresa Murphy y Bernadette Leigh. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](displaying-binary-data-in-the-data-web-controls-vb.md)
> [Siguiente](updating-and-deleting-existing-binary-data-vb.md)
