---
uid: web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-vb
title: Mostrar datos binarios en la Web de datos controla (VB) | Documentos de Microsoft
author: rick-anderson
description: "En este tutorial veremos las opciones para presentar datos binarios en una página Web, incluida la presentación de un archivo de imagen y el aprovisionamiento de un vínculo de 'Descargar' f..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: 9201656a-e1c2-4020-824b-18fb632d2925
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 1b9dadbfb82790a08a25a5c0f759b733cb59eb60
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="displaying-binary-data-in-the-data-web-controls-vb"></a>Mostrar datos binarios en los controles Web de datos (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_55_VB.exe) o [descarga de PDF](displaying-binary-data-in-the-data-web-controls-vb/_static/datatutorial55vb1.pdf)

> En este tutorial veremos las opciones para presentar datos binarios en una página Web, incluida la presentación de un archivo de imagen y el aprovisionamiento de un vínculo de 'Descargar' para un archivo PDF.


## <a name="introduction"></a>Introducción

En el tutorial anterior se explorar las dos técnicas para asociar datos binarios con un modelo de datos de aplicación s subyacente y se usa el control FileUpload para cargar archivos desde un explorador en el sistema de archivos de s de servidor web. Se ve todavía para ver cómo asociar los datos binarios cargados con el modelo de datos. Es decir, después de haber cargado un archivo y se guarda en el sistema de archivos, una ruta de acceso al archivo debe almacenarse en el registro de base de datos adecuada. Si se va a almacenar los datos directamente en la base de datos, a continuación, los datos binarios cargados no deben guardarse en el sistema de archivos, pero deben insertarse en la base de datos.

Antes de adentrarnos en asociar los datos con el modelo de datos, sin embargo, permiten s mirar primero cómo proporcionar los datos binarios para el usuario final. Presentar datos de texto es bastante sencillo, pero ¿cómo se deben presentar datos binarios? Depende, por supuesto, el tipo de datos binarios. Para imágenes, es probable que desee mostrar la imagen; para archivos PDF, documentos de Microsoft Word, archivos .zip y otros tipos de datos binarios, que proporciona un vínculo de descarga es probablemente más apropiada.

En este tutorial, veremos cómo presentar los datos binarios junto con sus datos de texto asociado con los datos de controles Web como la GridView y DetailsView. En el siguiente tutorial centraremos nuestro atención para asociar un archivo cargado a la base de datos.

## <a name="step-1-providingbrochurepathvalues"></a>Paso 1: Proporcionar`BrochurePath`valores

El `Picture` columna en el `Categories` tabla ya contiene datos binarios para las diferentes imágenes de categoría. En concreto, la `Picture` columna para cada registro contiene el contenido binario de una imagen de mapa de bits muestra granulado, baja calidad, 16 colores. Cada imagen de la categoría es 172 píxeles de ancho y 120 píxeles de alto y consume aproximadamente 11 KB. Novedades más, el contenido binario en el `Picture` columna incluye un byte 78 [OLE](http://en.wikipedia.org/wiki/Object_Linking_and_Embedding) encabezado que debe eliminarse antes de mostrar la imagen. Esta información de encabezado está presente, puesto que la base de datos Northwind tiene sus raíces en Microsoft Access. En Access, los datos binarios se almacenan con el tipo de datos de objeto OLE, que elementos de fijación de este encabezado. Por ahora, veremos cómo retirar los encabezados de estas imágenes de baja calidad con el fin de mostrar la imagen. En un futuro tutorial vamos a crear una interfaz para actualizar una categoría s `Picture` columna y reemplazar estas imágenes de mapa de bits que utilizan encabezados OLE con equivalente imágenes JPG sin los encabezados OLE innecesarios.

En el tutorial anterior, hemos visto cómo utilizar el control FileUpload. Por lo tanto, puede seguir adelante y agregar archivos de catálogo para el sistema de archivos de s de servidor web. Al hacerlo, sin embargo, no actualiza el `BrochurePath` columna en el `Categories` tabla. En el siguiente tutorial veremos cómo lograr este objetivo, pero por ahora necesitamos proporcionar manualmente los valores para esta columna.

En esta descarga tutorial s encontrará siete archivos de folleto PDF en el `~/Brochures` carpeta, uno para cada una de las categorías excepto Seafood. Omite intencionadamente agregando un folleto Seafood para ilustrar cómo controlar escenarios donde no todos los registros tienen asociados datos binarios. Para actualizar la `Categories` con estos valores de tabla, haga doble clic en el `Categories` nodo del explorador de servidores y elegir mostrar datos de tabla. A continuación, escriba las rutas de acceso virtuales a los archivos de catálogo para cada categoría que tiene un folleto, como se muestra en la figura 1. Dado que no hay ningún catálogo para la categoría Seafood, deje su `BrochurePath` valor de columna s como `NULL`.


[![Especifique manualmente los valores de la columna de categorías tabla s BrochurePath](displaying-binary-data-in-the-data-web-controls-vb/_static/image1.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image1.png)

**Figura 1**: especifique manualmente los valores para la `Categories` tabla s `BrochurePath` columna ([haga clic aquí para ver la imagen a tamaño completo](displaying-binary-data-in-the-data-web-controls-vb/_static/image2.png))


## <a name="step-2-providing-a-download-link-for-the-brochures-in-a-gridview"></a>Paso 2: Proporcionar un vínculo de descarga para los folletos en un control GridView

Con el `BrochurePath` valores proporcionados para el `Categories` tabla, se re listo para crear un control GridView que muestra cada categoría junto con un vínculo para descargar el folleto categoría s. En el paso 4, ampliaremos esta GridView para mostrar la imagen de la categoría s.

Iniciar, arrastre un control GridView desde el cuadro de herramientas hasta el Diseñador de la `DisplayOrDownloadData.aspx` página en el `BinaryData` carpeta. Establecer la s GridView `ID` a `Categories` y a través de la etiqueta inteligente de GridView s, elegir enlazar a un nuevo origen de datos. En concreto, enlazarlo a un origen ObjectDataSource denominado `CategoriesDataSource` que recupera datos mediante el `CategoriesBLL` objeto s `GetCategories()` método.


[![Crear un nuevo origen ObjectDataSource denominado CategoriesDataSource](displaying-binary-data-in-the-data-web-controls-vb/_static/image2.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image3.png)

**Figura 2**: crear un nuevo ObjectDataSource denominado `CategoriesDataSource` ([haga clic aquí para ver la imagen a tamaño completo](displaying-binary-data-in-the-data-web-controls-vb/_static/image4.png))


[![Configurar el ObjectDataSource para utilizar la clase CategoriesBLL](displaying-binary-data-in-the-data-web-controls-vb/_static/image3.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image5.png)

**Figura 3**: configurar el ObjectDataSource que se utilizan los `CategoriesBLL` clase ([haga clic aquí para ver la imagen a tamaño completo](displaying-binary-data-in-the-data-web-controls-vb/_static/image6.png))


[![Recuperar la lista de categorías mediante el método GetCategories()](displaying-binary-data-in-the-data-web-controls-vb/_static/image4.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image7.png)

**Figura 4**: recuperar la lista de categorías mediante el `GetCategories()` método ([haga clic aquí para ver la imagen a tamaño completo](displaying-binary-data-in-the-data-web-controls-vb/_static/image8.png))


Después de completar el Asistente para configurar orígenes de datos, Visual Studio agregará automáticamente un BoundField a la `Categories` GridView para la `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, y `BrochurePath` `DataColumn` s. Voy a quitar el `NumberOfProducts` BoundField desde el `GetCategories()` consulta de método s no recupera esta información. Quitar también la `CategoryID` BoundField y cambiar el nombre de la `CategoryName` y `BrochurePath` BoundFields `HeaderText` propiedades en categoría y el folleto, respectivamente. Después de realizar estos cambios, el marcado declarativo de s GridView y ObjectDataSource debe ser similar al siguiente:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample1.aspx)]

Ver esta página a través de un explorador (Véase la figura 5). Se muestra cada una de las ocho categorías. Las siete categorías con `BrochurePath` valores tienen la `BrochurePath` valor mostrado en el BoundField respectivo. Seafood, que tiene un `NULL` valor para su `BrochurePath`, muestra una celda vacía.


[![Se muestra cada categoría es nombre, descripción y el valor de BrochurePath](displaying-binary-data-in-the-data-web-controls-vb/_static/image5.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image9.png)

**Figura 5**: cada categoría es nombre, descripción, y `BrochurePath` valor aparece ([haga clic aquí para ver la imagen a tamaño completo](displaying-binary-data-in-the-data-web-controls-vb/_static/image10.png))


En lugar de mostrar el texto de la `BrochurePath` columna, desea crear un vínculo al folleto. Para lograr esto, quite el `BrochurePath` BoundField y reemplazarlo con un campo Hyperlink. Establecer la nueva s campo HYPERLINK `HeaderText` propiedad folleto, su `Text` propiedad para ver el folleto y su `DataNavigateUrlFields` propiedad `BrochurePath`.


![Agregar un campo HYPERLINK para BrochurePath](displaying-binary-data-in-the-data-web-controls-vb/_static/image6.gif)

**Figura 6**: agregar un campo HYPERLINK para`BrochurePath`


Esto agregará una columna de vínculos a la GridView, como se muestra en la figura 7. Al hacer clic en un vínculo de ver el folleto se mostrará el archivo PDF directamente en el explorador o pedir al usuario que descargue el archivo, dependiendo de si está instalado un lector PDF y la configuración del explorador s.


[![Una categoría s folleto puede verse haciendo clic en el vínculo de folleto de vista](displaying-binary-data-in-the-data-web-controls-vb/_static/image7.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image11.png)

**Figura 7**: s de categoría de un folleto puede verse haciendo clic en el vínculo de folleto ver ([haga clic aquí para ver la imagen a tamaño completo](displaying-binary-data-in-the-data-web-controls-vb/_static/image12.png))


[![Se muestra la categoría s folleto PDF](displaying-binary-data-in-the-data-web-controls-vb/_static/image8.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image13.png)

**Figura 8**: s de la categoría que se muestra el folleto PDF ([haga clic aquí para ver la imagen a tamaño completo](displaying-binary-data-in-the-data-web-controls-vb/_static/image14.png))


## <a name="hiding-the-view-brochure-text-for-categories-without-a-brochure"></a>Ocultar el texto de vista de catálogo para categorías sin un folleto

Tal y como se muestra en la figura 7, el `BrochurePath` campo HYPERLINK muestra su `Text` el valor de propiedad (vista de catálogo) para todos los registros, independientemente de si hay s no`NULL` valor para `BrochurePath`. Por supuesto, si `BrochurePath` es `NULL`, a continuación, el vínculo se muestra como texto, como sucede con la categoría Seafood (hacen referencia a la figura 7). En lugar de mostrar el texto de la vista de catálogo, puede ser interesante tener esas categorías sin un `BrochurePath` valor muestran algún texto alternativo, como no hay folleto disponible.

Para proporcionar este comportamiento, que debemos usar TemplateField cuyo contenido se genera mediante una llamada a un método de página que emite el resultado adecuado en función de la `BrochurePath` valor. En primer lugar analizamos este formato técnica en el [utilizando TemplateFields en el GridView Control](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) tutorial.

Convertir el campo HYPERLINK en TemplateField seleccionando el `BrochurePath` campo Hyperlink y, a continuación, haga clic en la función Convert este campo en TemplateField vinculan en el cuadro de diálogo Editar columnas.


![Convertir el campo HYPERLINK en TemplateField](displaying-binary-data-in-the-data-web-controls-vb/_static/image9.gif)

**Figura 9**: convertir el campo HYPERLINK en TemplateField


Esto creará un TemplateField con un `ItemTemplate` que contiene un hipervínculo Web control cuya `NavigateUrl` propiedad está enlazada a la `BrochurePath` valor. Reemplace este marcado con una llamada al método `GenerateBrochureLink`, pasando el valor de `BrochurePath`:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample2.aspx)]

A continuación, cree un `Protected` clase de código subyacente de s con el nombre de página de método en ASP.NET `GenerateBrochureLink` que devuelve un `String` y acepta un `Object` como un parámetro de entrada.


[!code-vb[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample3.vb)]

Este método determina si en el pasado `Object` valor es una base de datos `NULL` y, si es así, devuelve un mensaje que indica que la categoría no tiene un folleto. En caso contrario, si hay un `BrochurePath` valor, s que se muestra en un hipervínculo. Tenga en cuenta que si el `BrochurePath` valor es presentar s pasado en el [ `ResolveUrl(url)` método](https://msdn.microsoft.com/en-us/library/system.web.ui.control.resolveurl.aspx). Este método resuelve en el pasado *url*, reemplazando el `~` caracteres con la ruta de acceso virtual adecuada. Por ejemplo, si la aplicación se basa en `/Tutorial55`, `ResolveUrl("~/Brochures/Meats.pdf")` devolverá `/Tutorial55/Brochures/Meat.pdf`.

Figura 10 muestra la página después de aplicar estos cambios. Tenga en cuenta que la categoría de Seafood s `BrochurePath` campo ahora muestra el texto que no hay folleto disponible.


[![Los contadores de catálogo de texto No se muestra para las categorías sin un folleto](displaying-binary-data-in-the-data-web-controls-vb/_static/image10.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image15.png)

**Figura 10**: se muestra el texto No folleto disponible para las categorías sin un folleto ([haga clic aquí para ver la imagen a tamaño completo](displaying-binary-data-in-the-data-web-controls-vb/_static/image16.png))


## <a name="step-3-adding-a-web-page-to-display-a-category-s-picture"></a>Paso 3: Agregar una página Web para mostrar una imagen de la categoría s

Cuando un usuario visita una página ASP.NET, que reciben el s HTML de la página ASP.NET. El código HTML recibido es simplemente texto y no contiene todos los datos binarios. Los datos binarios adicionales, como imágenes, archivos de sonido, aplicaciones de Macromedia Flash, vídeos de Reproductor de Windows Media incrustado y así sucesivamente, existan como recursos independientes en el servidor web. El código HTML contiene referencias a estos archivos, pero no incluye el contenido real de los archivos.

Por ejemplo, en HTML el `<img>` elemento se utiliza para hacer referencia a una imagen, con el `src` atributo que apunta al archivo de imagen de este modo:


[!code-html[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample4.html)]

Cuando un explorador recibe este código HTML, realiza otra solicitud al servidor web para recuperar el contenido binario del archivo de imagen, que muestra a continuación, en el explorador. El mismo concepto se aplica a todos los datos binarios. En el paso 2, no se envió el folleto hacia abajo en el explorador como parte de la marca de página s HTML. En su lugar, el HTML representado proporciona hipervínculos que, al hacer clic, provocaba que el explorador solicitar el documento PDF directamente.

Para mostrar o permitir a los usuarios descargar datos binarios que se encuentra en la base de datos, es necesario crear una página web independiente que devuelve los datos. Para nuestra aplicación, sólo un campo de datos binarios allí s almacenado directamente en la imagen de la base de datos la s de categoría. Por lo tanto, necesitamos una página que, cuando se llama, devuelve los datos de imagen de una categoría determinada.

Agregue una nueva página ASP.NET para la `BinaryData` carpeta denominada `DisplayCategoryPicture.aspx`. Al hacerlo, deje desactivada la casilla de verificación Seleccionar la página maestra. Esta página espera un `CategoryID` valor de la cadena de consulta y devuelve los datos binarios de esa categoría s `Picture` columna. Puesto que esta página devuelve datos binarios y nada más, no necesita todas las marcas en la sección HTML. Por lo tanto, haga clic en la ficha de origen en la esquina inferior izquierda y quitar todas las revisiones de página s excepto el `<%@ Page %>` directiva. Es decir, `DisplayCategoryPicture.aspx` marcado declarativo s debería consistir en una sola línea:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample5.aspx)]

Si ve el `MasterPageFile` de atributo en la `<%@ Page %>` directiva, quítela.

En la clase de código subyacente de la página s, agregue el código siguiente a la `Page_Load` controlador de eventos:


[!code-vb[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample6.vb)]

Este código se inicia mediante la lectura de la `CategoryID` valor de cadena de consulta en una variable denominada `categoryID`. A continuación, los datos de imagen se recuperan mediante una llamada a la `CategoriesBLL` clase s. `GetCategoryWithBinaryDataByCategoryID(categoryID)` método. Estos datos se devuelven al cliente mediante la `Response.BinaryWrite(data)` (método), pero antes de que se llama a esto, el `Picture` se debe quitar el encabezado de columna valor s OLE. Esto se logra mediante la creación de un `Byte` matriz denominada `strippedImageData` que contendrá exactamente 78 caracteres menor que lo que aparece en la `Picture` columna. El [ `Array.Copy` método](https://msdn.microsoft.com/en-us/library/z50k9bft.aspx) se utiliza para copiar los datos de `category.Picture` empezando en la posición 78 a `strippedImageData`.

El `Response.ContentType` propiedad especifica la [tipo MIME](http://en.wikipedia.org/wiki/MIME) del contenido que se devuelve para que el explorador sabe cómo hacerla. Puesto que la `Categories` tabla s `Picture` la columna es una imagen de mapa de bits, el tipo MIME del mapa de bits se usa aquí (image/bmp). Si se omite el tipo MIME, mayoría de los exploradores se seguirá mostrando la imagen correctamente porque puede deducir el tipo según el contenido de los datos en la s de archivo de imagen binarios. Sin embargo, se s prudente incluir el MIME escribir cuando sea posible. Consulte la [sitio Web de Internet Assigned Numbers Authority s](http://www.iana.org/) para obtener una lista completa de [tipos de medio MIME](http://www.iana.org/assignments/media-types/).

Con esta página creada, se puede ver una imagen de la categoría determinada s visitando `DisplayCategoryPicture.aspx?CategoryID=categoryID`. La figura 11 muestra la imagen de la categoría s de bebidas, que puede verse desde `DisplayCategoryPicture.aspx?CategoryID=1`.


[![Las operaciones de asignación categoría bebidas que se debe mostrar la imagen](displaying-binary-data-in-the-data-web-controls-vb/_static/image11.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image17.png)

**Figura 11**: s de la categoría Bebidas se debe mostrar la imagen ([haga clic aquí para ver la imagen a tamaño completo](displaying-binary-data-in-the-data-web-controls-vb/_static/image18.png))


Si es, al visitar `DisplayCategoryPicture.aspx?CategoryID=categoryID`, obtendrá una excepción que se lee no se puede convertir un objeto de tipo "System.DBNull" al tipo 'System.Byte []', hay dos cosas que puedan estar causando esto. En primer lugar, el `Categories` tabla s `Picture` columna permite `NULL` valores. El `DisplayCategoryPicture.aspx` página, sin embargo, se supone que hay no`NULL` valor presente. El `Picture` propiedad de la `CategoriesDataTable` no se puede tener acceso directamente a si tiene un `NULL` valor. Si desea permitir `NULL` los valores para la `Picture` columna, d desea incluir la siguiente condición:


[!code-vb[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample7.vb)]

El código anterior supone que existen algunos con el nombre de archivo de imagen de s `NoPictureAvailable.gif` en el `Images` carpeta que desea mostrar para esas categorías sin una imagen.

También se podría producir esta excepción si el `CategoriesTableAdapter` s `GetCategoryWithBinaryDataByCategoryID` método s `SELECT` instrucción ha vuelto a la lista de columnas de la consulta principal s, lo cual puede suceder si está utilizando instrucciones SQL ad hoc y ve vuelva a ejecuta el Asistente para la s de TableAdapter consulta principal. Asegúrese de que `GetCategoryWithBinaryDataByCategoryID` método s `SELECT` instrucción sigue incluyendo la `Picture` columna.

> [!NOTE]
> Cada vez que la `DisplayCategoryPicture.aspx` es visitada, la base de datos se tiene acceso y se devuelven los datos de imagen de la categoría especificada s. Si t categoría s imagen todavía no ha cambiado desde la última visita el usuario, sin embargo, esto es derrochar esfuerzos. Afortunadamente, HTTP permite *condicional Obtiene*. Con una operación GET condicional, el cliente que realiza la solicitud HTTP envía a lo largo de un [ `If-Modified-Since` encabezado HTTP](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html) que proporciona la fecha y hora de última vez que el cliente recuperó este recurso desde el servidor web. Si el contenido no ha cambiado desde que se especifica este valor de fecha, el servidor web puede responder con una [código de estado no modificado (304)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) renunciar devolviendo el contenido del recurso solicitado. En resumen, esta técnica evita el servidor web de tener que enviar el nuevo contenido de un recurso si no se ha modificado desde que el cliente última accedió a él.


Para implementar este comportamiento, sin embargo, requiere que se agregue un `PictureLastModified` columna a la `Categories` tabla que se va a capturar cuando el `Picture` columna se actualizó por última vez, así como código para comprobar la `If-Modified-Since` encabezado. Para obtener más información sobre la `If-Modified-Since` encabezado y el flujo de trabajo GET condicional, vea [HTTP GET condicional para los Hackers RSS](http://fishbowl.pastiche.org/2002/10/21/http_conditional_get_for_rss_hackers) y [A más profunda mirar realizar solicitudes HTTP en una página ASP.NET](http://aspnet.4guysfromrolla.com/articles/122204-1.aspx).

## <a name="step-4-displaying-the-category-pictures-in-a-gridview"></a>Paso 4: Mostrar las imágenes de categoría en un control GridView

Ahora que tenemos una página web para mostrar una imagen de la categoría determinada s, podemos mostrar mediante el [control Image Web](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/image.aspx) o HTML `<img>` elemento al que apunta a `DisplayCategoryPicture.aspx?CategoryID=categoryID`. Imágenes cuya dirección URL viene determinado por la base de datos se pueden mostrar en la GridView o DetailsView mediante el ImageField. Contiene el ImageField `DataImageUrlField` y `DataImageUrlFormatString` propiedades que funcionan como las operaciones de asignación campo HYPERLINK `DataNavigateUrlFields` y `DataNavigateUrlFormatString` propiedades.

Permiten s aumentar la `Categories` GridView en `DisplayOrDownloadData.aspx` agregando un ImageField para mostrar cada imagen de la categoría s. Basta con agregar el ImageField y establecer su `DataImageUrlField` y `DataImageUrlFormatString` propiedades `CategoryID` y `DisplayCategoryPicture.aspx?CategoryID={0}`, respectivamente. Esto creará una columna de GridView que presenta un `<img>` elemento cuyo `src` las referencias de atributo `DisplayCategoryPicture.aspx?CategoryID={0}`, donde {0} se reemplaza con la fila de GridView s `CategoryID` valor.


![Agregar un ImageField a GridView](displaying-binary-data-in-the-data-web-controls-vb/_static/image12.gif)

**Figura 12**: agregar un ImageField a GridView


Después de agregar el ImageField, debe ser la sintaxis declarativa de GridView s como soothe siguientes:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample8.aspx)]

Tómese un momento para ver esta página a través de un explorador. Tenga en cuenta cómo cada registro ahora incluye una imagen de la categoría.


[![La categoría s imagen se muestra para cada fila](displaying-binary-data-in-the-data-web-controls-vb/_static/image13.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image19.png)

**Figura 13**: s de categoría de la imagen se muestra para cada fila ([haga clic aquí para ver la imagen a tamaño completo](displaying-binary-data-in-the-data-web-controls-vb/_static/image20.png))


## <a name="summary"></a>Resumen

En este tutorial se examina cómo presentar los datos binarios. Cómo se presentan los datos depende del tipo de datos. Para los archivos de folleto PDF, nos ofreció al usuario un folleto de vista de vínculo que, al hacer clic, el usuario se ha tardado directamente en el archivo PDF. Para la imagen de s categoría, se crea por primera vez una página para recuperar y devolver los datos binarios de la base de datos y, a continuación, usa esa página para mostrar cada imagen de s de la categoría de un control GridView.

Ahora que nos ha examinado cómo mostrar los datos binarios, se re listo para examinar cómo realizar la inserción, actualizaciones y eliminaciones en la base de datos con los datos binarios. En el siguiente tutorial se examinará cómo asociar un archivo cargado a su registro de base de datos correspondiente. En el tutorial después de eso, veremos cómo actualizar los datos binarios existentes, así como cómo eliminar los datos binarios cuando se quita su registro asociado.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [ *SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Los revisores iniciales para este tutorial fueron Teresa Murphy y Dave Gardner. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](uploading-files-vb.md)
[Siguiente](including-a-file-upload-option-when-adding-a-new-record-vb.md)
