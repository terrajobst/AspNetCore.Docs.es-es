---
uid: web-forms/overview/data-access/working-with-binary-files/uploading-files-vb
title: Carga de archivos (VB) | Microsoft Docs
author: rick-anderson
description: Obtenga información sobre cómo permitir a los usuarios cargar archivos binarios (como documentos de Word o PDF) al sitio Web donde pueden almacenarse en el sistema de archivos del servidor...
ms.author: aspnetcontent
ms.date: 03/27/2007
ms.assetid: f7c00fbd-652c-433d-8ed3-0e5168a4d4df
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/uploading-files-vb
msc.type: authoredcontent
ms.openlocfilehash: 412a809de63f22fd3fddf99ad34d06c079f73fd0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802726"
---
<a name="uploading-files-vb"></a>Carga de archivos (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_54_VB.exe) o [descargar PDF](uploading-files-vb/_static/datatutorial54vb1.pdf)

> Obtenga información sobre cómo permitir a los usuarios cargar archivos binarios (como documentos de Word o PDF) al sitio Web donde pueden almacenarse en el sistema de archivos del servidor o la base de datos.


## <a name="introduction"></a>Introducción

Todos los tutoriales se ve examinar hasta ahora ha trabajado exclusivamente con datos de texto. Sin embargo, muchas aplicaciones tienen modelos de datos que capturan datos binarios y texto. Un sitio de tratamiento de fechas en línea podría permitir a los usuarios cargar una imagen para asociar su perfil. Un sitio Web de contratación podría permitir que los usuarios cargar su reanudación, como un documento de Microsoft Word o PDF.

Trabajar con datos binarios, agrega un nuevo conjunto de desafíos. Debemos decidir cómo se almacenan los datos binarios en la aplicación. La interfaz utilizada para insertar nuevos registros tiene que actualizarse para permitir al usuario cargar un archivo desde su equipo y se deben realizar pasos adicionales para mostrar o proporcionar un medio para descargar un registro s datos binarios asociados. En este tutorial y los tres siguientes, exploraremos cómo obstáculo a nivel estos desafíos. Al final de estos tutoriales se habrá creado una aplicación totalmente funcional que asocia una imagen y el folleto PDF a cada categoría. En este tutorial particular se examinará las distintas técnicas para almacenar datos binarios y explore cómo habilitar usuarios cargar un archivo desde su equipo y ha guardado en el sistema de archivos de s de servidor web.

> [!NOTE]
> Datos binarios que forma parte de un modelo de datos de aplicación s se denominan a veces un [BLOB](http://en.wikipedia.org/wiki/Binary_large_object), acrónimo de objetos binarios grandes. En estos tutoriales he elegido usar los datos binarios de terminología, aunque el término BLOB es sinónimo.


## <a name="step-1-creating-the-working-with-binary-data-web-pages"></a>Paso 1: Crear el trabajo con datos binarios Web Pages

Antes de comenzar a explorar los desafíos asociados con la adición de compatibilidad con datos binarios, permiten s primero dedique un momento para crear las páginas ASP.NET en nuestro proyecto de sitio Web que necesitaremos para este tutorial y los tres siguientes. Empiece por agregar una nueva carpeta denominada `BinaryData`. A continuación, agregue las siguientes páginas ASP.NET a la carpeta y asegúrese de asociar cada página con el `Site.master` página maestra:

- `Default.aspx`
- `FileUpload.aspx`
- `DisplayOrDownloadData.aspx`
- `UploadInDetailsView.aspx`
- `UpdatingAndDeleting.aspx`


![Agregar las páginas ASP.NET para los tutoriales relacionados con los datos binarios](uploading-files-vb/_static/image1.gif)

**Figura 1**: agregar las páginas ASP.NET para los tutoriales relacionados con los datos binarios


Al igual que en las demás carpetas `Default.aspx` en el `BinaryData` carpeta mostrará una lista de los tutoriales en su sección. Recuerde que el `SectionLevelTutorialListing.ascx` Control de usuario proporciona esta funcionalidad. Por lo tanto, agrega este Control de usuario a `Default.aspx` arrastrándolo desde el Explorador de soluciones en la página de vista de diseño de s.


[![Agregar el Control de usuario SectionLevelTutorialListing.ascx a Default.aspx](uploading-files-vb/_static/image2.gif)](uploading-files-vb/_static/image1.png)

**Figura 2**: agregar el `SectionLevelTutorialListing.ascx` Control de usuario `Default.aspx` ([haga clic aquí para ver imagen en tamaño completo](uploading-files-vb/_static/image2.png))


Por último, agregue estas páginas como entradas para el `Web.sitemap` archivo. En concreto, agregue el siguiente marcado después de la mejora el control GridView `<siteMapNode>`:


[!code-xml[Main](uploading-files-vb/samples/sample1.xml)]

Después de actualizar `Web.sitemap`, dedique un momento para ver el sitio Web de tutoriales a través de un explorador. El menú de la izquierda ahora incluye elementos para el trabajo con los tutoriales de datos binarios.


![El mapa del sitio incluye ahora entradas para el trabajo con los tutoriales de datos binarios](uploading-files-vb/_static/image3.gif)

**Figura 3**: el mapa del sitio incluye ahora entradas para el trabajo con los tutoriales de datos binarios


## <a name="step-2-deciding-where-to-store-the-binary-data"></a>Paso 2: Decidir dónde Store los datos binarios

Datos binarios que está asociados con el modelo de datos de aplicación s pueden almacenarse en uno de estos dos lugares: en el sistema de archivos de s de servidor web con una referencia al archivo almacenado en la base de datos; o directamente dentro de la base de datos (consulte la figura 4). Cada enfoque tiene su propio conjunto de ventajas y desventajas y merece una explicación más detallada.


[![Se pueden almacenar datos binarios en el sistema de archivos o directamente en la base de datos](uploading-files-vb/_static/image4.gif)](uploading-files-vb/_static/image3.png)

**Figura 4**: se pueden almacenar datos binarios en el sistema de archivos o directamente en la base de datos ([haga clic aquí para ver imagen en tamaño completo](uploading-files-vb/_static/image4.png))


Imagine que deseaba ampliar la base de datos Northwind para asociar una imagen de cada producto. Una opción sería almacenar estos archivos de imagen en el sistema de archivos web server s y registre la ruta de acceso en el `Products` tabla. Con este enfoque, d agregamos un `ImagePath` columna a la `Products` tabla de tipo `varchar(200)`, tal vez. Cuando un usuario carga una imagen de Chai, esa imagen podría almacenarse en el sistema de archivos de s de servidor web en `~/Images/Tea.jpg`, donde `~` representa la ruta de acceso física s. Es decir, si el sitio web se ha modificado en la ruta de acceso física `C:\Websites\Northwind\`, `~/Images/Tea.jpg` sería equivalente a `C:\Websites\Northwind\Images\Tea.jpg`. Después de cargar el archivo de imagen, d actualizamos el registro Chai en el `Products` tabla para que su `ImagePath` hace referencia a la ruta de acceso de la nueva imagen de la columna. Podríamos usar `~/Images/Tea.jpg` o simplemente `Tea.jpg` si decidimos que todas las imágenes de producto se colocarían en la aplicación s `Images` carpeta.

Las principales ventajas de almacenar los datos binarios en el sistema de archivos son:

- **Facilidad de implementación** como veremos en breve, almacenar y recuperar datos binarios almacenados directamente dentro de la base de datos implica un poco más código cuando se trabaja con datos mediante el sistema de archivos. Además, para que un usuario ver o descargar datos binarios se debe presentar con una dirección URL a los datos. Si los datos residen en el sistema de archivos de s de servidor web, la dirección URL es sencilla. Si los datos se almacenan en la base de datos, sin embargo, las páginas web tiene que crearse que recuperará y devolver los datos de la base de datos.
- **Ampliar el acceso a los datos binarios** los datos binarios que deba ser accesible para otros servicios o aplicaciones, las que no se extraen los datos de la base de datos. Por ejemplo, las imágenes asociadas con cada producto también necesite que estén disponibles para los usuarios a través de [FTP](http://en.wikipedia.org/wiki/File_Transfer_Protocol), en cuyo caso d queremos almacenar los datos binarios en el sistema de archivos.
- **Rendimiento** si los datos binarios se almacenan en el sistema de archivos, la congestión de red y a petición entre el servidor de base de datos y el servidor web será menor que si se almacenan los datos binarios directamente dentro de la base de datos.

El principal inconveniente de almacenar datos binarios en el sistema de archivos es que éste separa los datos de la base de datos. Si se elimina un registro de la `Products` tabla, el archivo asociado en el sistema de archivos de s de servidor web no se elimina automáticamente. Nos debemos escribir código adicional para eliminar el archivo o el sistema de archivos se llenarse con archivos huérfanos, sin usar. Además, cuando la copia de seguridad de la base de datos, nos debemos Asegúrese de realizar copias de seguridad de los datos binarios asociados en el sistema de archivos. Mover la base de datos a otro sitio o servidor puede causar problemas similares.

Como alternativa, se pueden almacenar datos binarios directamente en una base de datos de Microsoft SQL Server 2005 mediante la creación de una columna de tipo `varbinary`. Al igual que con otros tipos de datos de longitud variable, puede especificar una longitud máxima de los datos binarios que se pueden almacenar en esta columna. Por ejemplo, para reservar al menos 5.000 bytes usar `varbinary(5000)`; `varbinary(MAX)` permite que el tamaño máximo de almacenamiento, aproximadamente 2 GB.

La principal ventaja de almacenar datos binarios directamente en la base de datos es el estrecho acoplamiento entre los datos binarios y el registro de base de datos. Esto simplifica en gran medida las tareas de administración de base de datos, como las copias de seguridad o mover la base de datos a un sitio o servidor diferente. Además, eliminar automáticamente un registro elimina los datos binarios correspondientes. También son más sutiles ventajas de almacenar los datos binarios en la base de datos. Consulte [almacenar binario archivos directamente en la base de datos con ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/120606-1.aspx) para obtener una explicación más detallada.

> [!NOTE]
> En Microsoft SQL Server 2000 y versiones anteriores, el `varbinary` tipo de datos tiene un límite máximo de 8.000 bytes. Para almacenar hasta 2 GB de datos binarios el [ `image` tipo de datos](https://msdn.microsoft.com/library/ms187993.aspx) debe usarse en su lugar. Con la adición de `MAX` en SQL Server 2005, sin embargo, el `image` tipo de datos está desusado. Lo s todavía admite para versiones anteriores, pero Microsoft ha anunciado que el `image` tipo de datos se quitará en una versión futura de SQL Server.


Si está trabajando con un modelo de datos más antiguo, es posible que vea el `image` tipo de datos. La base de datos Northwind s `Categories` tabla tiene un `Picture` columna que se puede usar para almacenar los datos binarios de un archivo de imagen para la categoría. Puesto que la base de datos Northwind tiene sus raíces en Microsoft Access y las versiones anteriores de SQL Server, esta columna es de tipo `image`.

En este tutorial y los tres siguientes, vamos a usar ambos enfoques. El `Categories` tabla ya tiene un `Picture` columna para almacenar el contenido binario de una imagen de la categoría. Vamos a agregar una columna adicional, `BrochurePath`, para almacenar una ruta de acceso a un archivo PDF en el sistema de archivos de s de servidor web que se puede usar para proporcionar información general de la categoría de calidad de impresión y perfeccionada.

## <a name="step-3-adding-thebrochurepathcolumn-to-thecategoriestable"></a>Paso 3: Agregar el`BrochurePath`columna a la`Categories`tabla

La tabla Categories tiene actualmente solo cuatro columnas: `CategoryID`, `CategoryName`, `Description`, y `Picture`. Además de estos campos, necesitamos agregar una nueva que apuntará el folleto categoría s (si existe). Para agregar esta columna, vaya al explorador de servidores, profundizar en las tablas, haga doble clic en el `Categories` de tabla y elija Abrir definición de tabla (consulte la figura 5). Si no ve el Explorador de servidores, aparezca seleccionando la opción de explorador de servidores en el menú Ver, o presione Ctrl + Alt + S.

Agregue un nuevo `varchar(200)` columna a la `Categories` tabla denominada `BrochurePath` y permite `NULL` s y haga clic en el icono Guardar (o presione Ctrl + S).


[![Agregar una columna BrochurePath a la tabla de categorías](uploading-files-vb/_static/image5.gif)](uploading-files-vb/_static/image5.png)

**Figura 5**: agregar un `BrochurePath` columna a la `Categories` tabla ([haga clic aquí para ver imagen en tamaño completo](uploading-files-vb/_static/image6.png))


## <a name="step-4-updating-the-architecture-to-use-thepictureandbrochurepathcolumns"></a>Paso 4: Actualizar la arquitectura para usar el`Picture`y`BrochurePath`columnas

El `CategoriesDataTable` en la capa de acceso a datos (DAL) actualmente tiene cuatro `DataColumn` s definido: `CategoryID`, `CategoryName`, `Description`, y `NumberOfProducts`. Cuando se diseñó originalmente esta DataTable en el [crear una capa de acceso a datos](../introduction/creating-a-data-access-layer-vb.md) tutorial, el `CategoriesDataTable` sólo tenía las tres primeras columnas; el `NumberOfProducts` se ha agregado la columna en la [principal-detalle utilizando un viñetas Lista de registros maestros con un control DataList de detalles](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md) tutorial.

Como se describe en *crear una capa de acceso a datos*, las DataTables del DataSet con tipo componen los objetos de negocio. Los TableAdapters son responsables de la comunicación con la base de datos y rellenar los objetos de negocio con los resultados de consulta. El `CategoriesDataTable` se rellena con el `CategoriesTableAdapter`, que tiene tres métodos de recuperación de datos:

- `GetCategories()` ejecuta la consulta principal de TableAdapter s y devuelve el `CategoryID`, `CategoryName`, y `Description` campos de todos los registros en el `Categories` tabla. La consulta principal es que se usa en el autogenerado `Insert` y `Update` métodos.
- `GetCategoryByCategoryID(categoryID)` Devuelve el `CategoryID`, `CategoryName`, y `Description` campos de la categoría cuyo `CategoryID` es igual a *categoryID*.
- `GetCategoriesAndNumberOfProducts()` -Devuelve el `CategoryID`, `CategoryName`, y `Description` campos para todos los registros en el `Categories` tabla. También usa una subconsulta para devolver el número de productos asociados con cada categoría.

Tenga en cuenta que ninguna de estas consultas devuelven el `Categories` tabla s `Picture` o `BrochurePath` columnas; ni hace el `CategoriesDataTable` proporcionar `DataColumn` s para estos campos. Para poder trabajar con la imagen y `BrochurePath` propiedades, se deberá agregar a la `CategoriesDataTable` y, a continuación, actualice el `CategoriesTableAdapter` clase para devolver estas columnas.

## <a name="adding-thepictureandbrochurepathdatacolumn-s"></a>Agregar el`Picture`y`BrochurePath``DataColumn` s

Empiece por agregar estas dos columnas a la `CategoriesDataTable`. Haga doble clic en el `CategoriesDataTable` encabezado s, seleccione Agregar en el menú contextual y, a continuación, elija la opción de columna. Esto creará un nuevo `DataColumn` en la tabla de datos denominada `Column1`. Cambiar el nombre de esta columna a `Picture`. En la ventana Propiedades, establezca la `DataColumn` s `DataType` propiedad `System.Byte[]` (Esto no es una opción en la lista desplegable; debe escribir en).


[![Crear una imagen de DataColumn denominado cuyo tipo de datos es System.Byte](uploading-files-vb/_static/image6.gif)](uploading-files-vb/_static/image7.png)

**Figura 6**: crear un `DataColumn` con nombre `Picture` cuyo `DataType` es `System.Byte[]` ([haga clic aquí para ver imagen en tamaño completo](uploading-files-vb/_static/image8.png))


Agregue otro `DataColumn` a la DataTable, asígnele el nombre `BrochurePath` con el valor predeterminado `DataType` valor (`System.String`).

## <a name="returning-thepictureandbrochurepathvalues-from-the-tableadapter"></a>Devolver el`Picture`y`BrochurePath`los valores de TableAdapter

Con estos dos `DataColumn` agregadas a la `CategoriesDataTable`, que está listo para actualizar el `CategoriesTableAdapter`. Podríamos tener ambos de estos valores de columna devueltos en la consulta principal de TableAdapter, pero esto podría devolver los datos binarios cada vez el `GetCategories()` se invocó el método. En su lugar, s permiten actualizar la consulta principal de TableAdapter para devolver `BrochurePath` y cree un método de recuperación de datos adicionales que devuelve una categoría determinada s `Picture` columna.

Para actualizar la consulta de TableAdapter principal, haga doble clic en el `CategoriesTableAdapter` encabezado s y elija la opción de configurar en el menú contextual. Se abrirá el Asistente de configuración de adaptador de tabla que se ve en una serie de tutoriales anteriores. Actualice la consulta para devolver el `BrochurePath` y haga clic en Finalizar.


[![Actualizar la lista de columnas en la instrucción SELECT para devolver también BrochurePath](uploading-files-vb/_static/image7.gif)](uploading-files-vb/_static/image9.png)

**Figura 7**: actualizar la lista de columnas en el `SELECT` instrucción para devolver también `BrochurePath` ([haga clic aquí para ver imagen en tamaño completo](uploading-files-vb/_static/image10.png))


Al usar instrucciones SQL ad-hoc del TableAdapter, actualizar la lista de columnas en la consulta principal actualiza la lista de columnas para todos los `SELECT` métodos de consulta en el TableAdapter. Esto significa que el `GetCategoryByCategoryID(categoryID)` método se ha actualizado para devolver el `BrochurePath` columna, que sería lo que se pretendía. Sin embargo, también actualiza la lista de columnas en el `GetCategoriesAndNumberOfProducts()` método, quitar la subconsulta que devuelve el número de productos de cada categoría! Por lo tanto, es necesario actualizar este método s `SELECT` consulta. Haga doble clic en el `GetCategoriesAndNumberOfProducts()` método, elija Configurar y revertir el `SELECT` consulta a su valor original:


[!code-sql[Main](uploading-files-vb/samples/sample2.sql)]

A continuación, cree un nuevo método de TableAdapter que devuelva una categoría determinada s `Picture` valor de la columna. Haga doble clic en el `CategoriesTableAdapter` encabezado s y elija la opción de Agregar consulta para iniciar el Asistente para configuración de consulta de TableAdapter. El primer paso de este asistente nos pregunta que si desea consultar los datos mediante una instrucción de SQL ad hoc, un nuevo procedimiento almacenado, o uno ya existente. Seleccione Usar instrucciones SQL y haga clic en siguiente. Puesto que se van a devolver una fila, seleccione el que devuelve la opción de filas del segundo paso.


[![Seleccione la opción instrucciones de uso de SQL](uploading-files-vb/_static/image8.gif)](uploading-files-vb/_static/image11.png)

**Figura 8**: seleccione la opción instrucciones de uso de SQL ([haga clic aquí para ver imagen en tamaño completo](uploading-files-vb/_static/image12.png))


[![Puesto que la consulta devolverá un registro de la tabla Categories, elija SELECT que devuelve filas](uploading-files-vb/_static/image9.gif)](uploading-files-vb/_static/image13.png)

**Figura 9**: puesto que la consulta devolverá un registro de la tabla Categories, elija Seleccionar que devuelve filas ([haga clic aquí para ver imagen en tamaño completo](uploading-files-vb/_static/image14.png))


En el tercer paso, escriba la siguiente consulta SQL y haga clic en siguiente:


[!code-sql[Main](uploading-files-vb/samples/sample3.sql)]

El último paso es elegir el nombre para el nuevo método. Use `FillCategoryWithBinaryDataByCategoryID` y `GetCategoryWithBinaryDataByCategoryID` para rellenar una DataTable y devuelven un objeto DataTable patrones, respectivamente. Haga clic en Finalizar para completar al asistente.


[![Elija los nombres de los métodos de TableAdapter s](uploading-files-vb/_static/image10.gif)](uploading-files-vb/_static/image15.png)

**Figura 10**: elija los nombres de los métodos de TableAdapter s ([haga clic aquí para ver imagen en tamaño completo](uploading-files-vb/_static/image16.png))


> [!NOTE]
> Después de completar al Asistente para configuración de consulta de adaptador de tabla es posible que vea un cuadro de diálogo que le informa de que el texto del nuevo comando devuelve datos con un esquema diferente desde el esquema de la consulta principal. En resumen, el Asistente está teniendo en cuenta que la consulta principal de TableAdapter s `GetCategories()` devuelve un esquema diferente a la que acabamos de crear. Pero esto es lo que queremos, por lo que puede pasar por alto este mensaje.


Además, tenga en cuenta que si usa instrucciones SQL ad hoc y usar el Asistente para cambiar la consulta principal de TableAdapter s en algún momento posterior en el tiempo, modificará la `GetCategoryWithBinaryDataByCategoryID` método s `SELECT` lista de columnas de la instrucción s para incluir solo las columnas de la la consulta principal (es decir, quitará la `Picture` columna de la consulta). Tendrá que actualizar manualmente la lista de columnas para devolver el `Picture` columna, similar a lo que hicimos con la `GetCategoriesAndNumberOfProducts()` método anteriormente en este paso.

Después de agregar los dos `DataColumn` s para el `CategoriesDataTable` y `GetCategoryWithBinaryDataByCategoryID` método a la `CategoriesTableAdapter`, estas clases en el Diseñador de DataSet con tipo deben ser similar a la captura de pantalla en la figura 11.


![El Diseñador de DataSet incluye las nuevas columnas y el método](uploading-files-vb/_static/image11.gif)

**Figura 11**: el Diseñador de DataSet incluye las nuevas columnas y el método


## <a name="updating-the-business-logic-layer-bll"></a>Actualización de la capa de lógica empresarial (BLL)

Con la capa DAL actualizada, todo lo que queda es aumentar la capa de lógica empresarial (BLL) para incluir un método para el nuevo `CategoriesTableAdapter` método. Agregue el método siguiente a la `CategoriesBLL` clase:


[!code-vb[Main](uploading-files-vb/samples/sample4.vb)]

## <a name="step-5-uploading-a-file-from-the-client-to-the-web-server"></a>Paso 5: Cargar un archivo desde el cliente al servidor Web

Cuando recopile datos binarios, a menudo, estos datos se proporcionan por un usuario final. Para capturar esta información, el usuario debe ser capaz de cargar un archivo desde su equipo al servidor web. Los datos cargados, a continuación, deben integrarse con el modelo de datos, lo que puede significar que guardar el archivo en el sistema de archivos de s de servidor web y agregar una ruta de acceso al archivo en la base de datos o escribir el contenido binario directamente en la base de datos. En este paso daremos un vistazo a cómo permitir que un usuario cargar archivos desde su equipo al servidor. En el siguiente tutorial, centraremos nuestra atención en el archivo cargado la integración con el modelo de datos.

ASP.NET 2.0 s nuevo [control FileUpload Web](https://msdn.microsoft.com/library/ms227677(VS.80).aspx) proporciona un mecanismo para que los usuarios enviar un archivo desde su equipo al servidor web. El control FileUpload se representa como un `<input>` elemento cuyo `type` atributo está establecido en el archivo, que los exploradores muestran como un cuadro de texto con un botón Examinar. Al hacer clic en el botón Examinar muestra un cuadro de diálogo desde el que el usuario puede seleccionar un archivo. Cuando se devuelve el formulario, el contenido de s del archivo seleccionado se envía junto con la devolución de datos. En el lado servidor, información sobre el archivo cargado es accesible a través de las propiedades de s control FileUpload.

Para demostrar la carga de archivos, abra el `FileUpload.aspx` página en el `BinaryData` carpeta, arrastre un control FileUpload desde el cuadro de herramientas hasta el diseñador y establezca el control s `ID` propiedad `UploadTest`. A continuación, agregue un control de botón Web establecer su `ID` y `Text` propiedades a `UploadButton` y cargar el archivo seleccionado, respectivamente. Por último, coloque un control Web de la etiqueta debajo del botón, desactive su `Text` propiedad y establezca su `ID` propiedad `UploadDetails`.


[![Agregar un Control FileUpload a la página de ASP.NET](uploading-files-vb/_static/image12.gif)](uploading-files-vb/_static/image17.png)

**Figura 12**: agregar un Control FileUpload a la página de ASP.NET ([haga clic aquí para ver imagen en tamaño completo](uploading-files-vb/_static/image18.png))


Figura 13 se muestra esta página cuando se ve mediante un explorador. Tenga en cuenta que al hacer clic en el botón Examinar abre un cuadro de diálogo de selección de archivo, que permite al usuario seleccionar un archivo desde su equipo. Una vez que se ha seleccionado un archivo, haga clic en el botón Cargar archivo seleccionada hace que una devolución de datos que envía el contenido binario de s de archivo seleccionado en el servidor web.


[![El usuario puede seleccionar un archivo para cargarlo desde su equipo al servidor](uploading-files-vb/_static/image13.gif)](uploading-files-vb/_static/image19.png)

**Figura 13**: el usuario puede seleccionar un archivo para cargar desde su equipo al servidor ([haga clic aquí para ver imagen en tamaño completo](uploading-files-vb/_static/image20.png))


En el postback, se puede guardar el archivo cargado en el sistema de archivos o sus datos binarios pueden actuar directamente a través de un Stream. En este ejemplo, permitir s crear un `~/Brochures` carpeta y guarde allí el archivo cargado. Empiece agregando el `Brochures` carpeta en el sitio como una subcarpeta del directorio raíz. A continuación, cree un controlador de eventos para el `UploadButton` s `Click` eventos y agregue el código siguiente:


[!code-vb[Main](uploading-files-vb/samples/sample5.vb)]

El control FileUpload proporciona una serie de propiedades para trabajar con los datos cargados. Por ejemplo, el [ `HasFile` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.hasfile.aspx) indica si se ha cargado un archivo por el usuario, mientras que el [ `FileBytes` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.filebytes.aspx) proporciona acceso a los datos binarios cargados como una matriz de bytes. El `Click` se inicia el controlador de eventos al garantizar que se ha cargado un archivo. Si se ha cargado un archivo, la etiqueta muestra el nombre del archivo cargado, su tamaño en bytes y su tipo de contenido.

> [!NOTE]
> Para asegurarse de que el usuario carga un archivo que se puede comprobar el `HasFile` propiedad y mostrar una advertencia si se s `False`, o bien puede usar el [control RequiredFieldValidator](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/validation/default.aspx) en su lugar.


La s FileUpload `SaveAs(filePath)` guarda el archivo cargado especificado *filePath*. *filePath* debe ser un *ruta de acceso física* (`C:\Websites\Brochures\SomeFile.pdf`) en lugar de un *virtual* *ruta* (`/Brochures/SomeFile.pdf`). El [ `Server.MapPath(virtPath)` método](https://msdn.microsoft.com/library/system.web.httpserverutility.mappath.aspx) toma una ruta de acceso virtual y devuelve su ruta de acceso física correspondiente. En este caso, es la ruta de acceso virtual `~/Brochures/fileName`, donde *fileName* es el nombre del archivo cargado. Consulte [usando Server.MapPath](http://www.4guysfromrolla.com/webtech/121799-1.shtml) para obtener más información sobre las rutas de acceso virtuales y físicas y el uso `Server.MapPath`.

Después de completar la `Click` controlador de eventos, dedique un momento para probar la página en un explorador. Haga clic en el botón Examinar y seleccionar un archivo de disco duro y, a continuación, haga clic en el botón Cargar archivo seleccionado. La devolución de datos se enviará el contenido del archivo seleccionado en el servidor web, que, a continuación, se mostrará información sobre el archivo antes de guardarlo en el `~/Brochures` carpeta. Después de cargar el archivo, vuelva a Visual Studio y haga clic en el botón Actualizar en el Explorador de soluciones. Debería ver el archivo que acaba de cargar en la carpeta ~/Brochures!


[![Se ha cargado el EvolutionValley.jpg de archivo en el servidor Web](uploading-files-vb/_static/image14.gif)](uploading-files-vb/_static/image21.png)

**Figura 14**: el archivo `EvolutionValley.jpg` se ha cargado en el servidor Web ([haga clic aquí para ver imagen en tamaño completo](uploading-files-vb/_static/image22.png))


![EvolutionValley.jpg se guardó en la carpeta ~/Brochures](uploading-files-vb/_static/image15.gif)

**Figura 15**: `EvolutionValley.jpg` se guardó en el `~/Brochures` carpeta


## <a name="subtleties-with-saving-uploaded-files-to-the-file-system"></a>Sutilezas con guardar los archivos cargados en el sistema de archivos

Hay varios matices que deben solucionarse al guardar la carga de archivos en el sistema de archivos de s de servidor web. Primero, allí el problema de seguridad. Para guardar un archivo en el sistema de archivos, el contexto de seguridad en el que se ejecuta la página ASP.NET debe tener permisos de escritura. El servidor Web de desarrollo de ASP.NET se ejecuta en el contexto de su cuenta de usuario actual. Si usas s de Microsoft Internet Information Services (IIS) como el servidor web, el contexto de seguridad depende de la versión de IIS y su configuración.

Otro desafío de guardar archivos en el sistema de archivos se gira en torno a la denominación de los archivos. Actualmente, nuestra página guarda todos los archivos cargados en el `~/Brochures` directorio con el mismo nombre que el archivo en el equipo cliente s. Si un usuario carga un folleto con el nombre `Brochure.pdf`, el archivo se guardará como `~/Brochure/Brochure.pdf`. Pero ¿qué ocurre si el usuario B en algún momento posterior carga un archivo de catálogo diferente que tenga el mismo nombre de archivo (`Brochure.pdf`)? Con el código ahora, tenemos un archivo s, se sobrescribirá con la carga de usuario B s el usuario.

Existen varias técnicas para resolver conflictos de nombre de archivo. Una opción consiste en prohibir la carga de un archivo si ya existe uno con el mismo nombre. Con este enfoque, cuando el usuario B intenta cargar un archivo denominado `Brochure.pdf`, el sistema no podría guardar su archivo y en su lugar mostrará un mensaje que informa el usuario B para cambiar el nombre del archivo e inténtelo de nuevo. Otro enfoque consiste en Guardar el archivo con un nombre de archivo único, lo que sería un [identificador único global (GUID)](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) o el valor de las correspondientes columnas de clave principal de s registros de base de datos (suponiendo que la carga está asociada con un fila concreta en el modelo de datos). En el siguiente tutorial exploraremos estas opciones con más detalle.

## <a name="challenges-involved-with-very-large-amounts-of-binary-data"></a>Desafíos implicados con grandes cantidades de datos binarios

Estos tutoriales se supone que los datos binarios que se capturan están modestos de tamaño. Trabaja con grandes cantidades de archivos de datos binarios que están varios megabytes o más grande introduce nuevos desafíos que quedan fuera del ámbito de estos tutoriales. Por ejemplo, de forma predeterminada ASP.NET rechazará las cargas de más de 4 MB, aunque se pueden configurar a través de la [ `<httpRuntime>` elemento](https://msdn.microsoft.com/library/e1f13641.aspx) en `Web.config`. IIS impone su propio limitaciones de tamaño de carga de archivo demasiado. Consulte [tamaño del archivo de carga de IIS](http://vandamme.typepad.com/development/2005/09/iis_upload_file.html) para obtener más información. Además, el tiempo necesario para cargar archivos de gran tamaño puede superar el valor predeterminado 110 segundos de que espera para una solicitud de ASP.NET. También hay problemas de memoria y rendimiento que surgen al trabajar con archivos grandes.

El control FileUpload resulta práctico para grandes cargas de archivos. Como el contenido del archivo s se publican en el servidor, el usuario final debe esperar pacientemente sin la confirmación que se lleva a cabo su carga. Esto no es tanto un problema cuando se trabaja con archivos más pequeños que se pueden cargar en unos segundos, pero pueden ser un problema cuando se trabaja con archivos de mayor tamaño que pueden tardar minutos en cargarse. Hay una variedad de otros fabricantes archivo controles de carga que se ajusten mejor para controlar grandes cargas y muchos de estos proveedores ofrecen indicadores de progreso y ActiveX cargar administradores que presentan una experiencia de usuario mucho más precisos.

Si la aplicación necesita administrar archivos de gran tamaño, deberá investigar los desafíos con cuidado y encontrar las soluciones adecuadas para sus necesidades concretas.

## <a name="summary"></a>Resumen

Creación de una aplicación que necesita para capturar los datos binarios presenta a una serie de desafíos. En este tutorial hemos explorado los dos primeros: decidir dónde almacenar los datos binarios y permite que un usuario cargar contenido binario a través de una página web. A través de los tres tutoriales siguientes, veremos cómo asociar los datos cargados con un registro en la base de datos, así como cómo mostrar los datos binarios junto con sus campos de datos de texto.

Feliz programación.

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Uso de tipos de datos de valores grandes](https://msdn.microsoft.com/library/ms178158.aspx)
- [Tutoriales del Control fileUpload](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/fileupload.aspx)
- [El Control de servidor ASP.NET 2.0 FileUpload](http://www.wrox.com/WileyCDA/Section/id-292158.html)
- [El lado oscuro de cargas de archivos](http://www.aspnetresources.com/articles/dark_side_of_file_uploads.aspx)

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Los revisores para este tutorial fueron Teresa Murphy y Bernadette Leigh. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](updating-and-deleting-existing-binary-data-cs.md)
> [Siguiente](displaying-binary-data-in-the-data-web-controls-vb.md)
