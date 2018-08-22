---
uid: web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-cs
title: Mostrar datos binarios en la Web de datos controla (C#) | Microsoft Docs
author: rick-anderson
description: En este tutorial, nos centramos en las opciones para presentar datos binarios en una página Web, incluida la visualización de un archivo de imagen y el aprovisionamiento de un vínculo 'Descargar' f...
ms.author: riande
ms.date: 03/27/2007
ms.assetid: 5cbeb9f8-5f92-4ba8-87ae-0b4d460ae6d4
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 50d7f8eceb4772c628f7f6ef71f110de03dd9348
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835887"
---
<a name="displaying-binary-data-in-the-data-web-controls-c"></a>Mostrar datos binarios en los controles Web de datos (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_55_CS.exe) o [descargar PDF](displaying-binary-data-in-the-data-web-controls-cs/_static/datatutorial55cs1.pdf)

> En este tutorial, nos centramos en las opciones para presentar datos binarios en una página Web, incluida la visualización de un archivo de imagen y el aprovisionamiento de un vínculo "Descargar" para un archivo PDF.


## <a name="introduction"></a>Introducción

En el tutorial anterior hemos explorado las dos técnicas para asociar datos binarios con un modelo de datos de aplicación s subyacente y usa el control FileUpload para cargar archivos desde un explorador en el sistema de archivos de s de servidor web. Se ve aún para ver cómo asociar los datos binarios cargados con el modelo de datos. Es decir, después de un archivo se ha cargado y guardado en el sistema de archivos, una ruta de acceso al archivo debe almacenarse en el registro de base de datos adecuada. Si se va a almacenar los datos directamente en la base de datos, a continuación, los datos binarios cargados no se deben guardar en el sistema de archivos, pero deben insertarse en la base de datos.

Antes de adentrarnos en asociar los datos con el modelo de datos, sin embargo, permiten s primer vistazo a cómo proporcionar los datos binarios para el usuario final. Presentar los datos de texto es bastante sencillo, pero ¿cómo se deben presentar datos binarios? Depende, por supuesto, el tipo de datos binarios. Para las imágenes, que es probable que queremos mostrar la imagen. para archivos PDF, documentos de Microsoft Word, archivos ZIP y otros tipos de datos binarios, que proporciona un vínculo de descarga es probablemente más apropiado.

En este tutorial de que veremos cómo presentar los datos binarios junto con sus datos de texto asociado con los datos controles Web como GridView y DetailsView. En el siguiente tutorial, centraremos nuestra atención para asociar un archivo cargado a la base de datos.

## <a name="step-1-providingbrochurepathvalues"></a>Paso 1: Proporcionar`BrochurePath`valores

El `Picture` columna en el `Categories` tabla ya contiene datos binarios para las distintas imágenes de categoría. En concreto, la `Picture` columna para cada registro contiene el contenido binario de una imagen de mapa de bits granuladas, baja calidad, 16 colores. Cada imagen de la categoría es 172 píxeles de anchos y 120 píxeles de alto y consume aproximadamente 11 KB. Novedades más, el contenido binario en el `Picture` columna incluye un 78 bytes [OLE](http://en.wikipedia.org/wiki/Object_Linking_and_Embedding) encabezado que debe eliminarse antes de mostrar la imagen. Esta información de encabezado está presente porque la base de datos Northwind tiene sus raíces en Microsoft Access. En el acceso, los datos binarios se almacenan con el tipo de datos del objeto OLE, que agrega este encabezado. Por ahora, veremos cómo retirar los encabezados de estas imágenes de baja calidad con el fin de mostrar la imagen. En un futuro tutorial crearemos una interfaz para actualizar una categoría s `Picture` columna y reemplazar estas imágenes de mapa de bits que usan encabezados OLE con las imágenes JPG equivalente sin los encabezados OLE innecesarios.

En el tutorial anterior, vimos cómo usar el control FileUpload. Por lo tanto, puede continuar y agregar los archivos de catálogo para el sistema de archivos de s de servidor web. Al hacerlo, sin embargo, no se actualiza el `BrochurePath` columna en el `Categories` tabla. En el siguiente tutorial veremos cómo lograr esto, pero por ahora necesitamos proporcionar manualmente los valores de esta columna.

En esta descarga tutorial s encontrará siete archivos de folleto PDF en el `~/Brochures` carpeta, uno para cada una de las categorías, excepto los Mariscos. Omití intencionadamente agregando un folleto mariscos para ilustrar cómo tratar escenarios donde no todos los registros tienen datos binarios asociados. Para actualizar el `Categories` con estos valores de tabla, haga doble clic en el `Categories` nodo del explorador de servidores y elija Mostrar datos de tabla. A continuación, escriba las rutas de acceso virtuales a los archivos de catálogo para cada categoría que tiene un folleto, como se muestra en la figura 1. Dado que no hay ningún catálogo para la categoría mariscos, deje su `BrochurePath` valor de columna s como `NULL`.


[![Especifique manualmente los valores para la columna de categorías tabla s BrochurePath](displaying-binary-data-in-the-data-web-controls-cs/_static/image1.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image1.png)

**Figura 1**: especifique manualmente los valores para el `Categories` tabla s `BrochurePath` columna ([haga clic aquí para ver imagen en tamaño completo](displaying-binary-data-in-the-data-web-controls-cs/_static/image2.png))


## <a name="step-2-providing-a-download-link-for-the-brochures-in-a-gridview"></a>Paso 2: Proporcionar un vínculo de descarga para los folletos en un control GridView

Con el `BrochurePath` valores proporcionados para el `Categories` tabla, que está listo para crear un control GridView que muestra cada categoría junto con un vínculo para descargar el folleto categoría s. En el paso 4, ampliaremos esta GridView para mostrar también la imagen de la categoría s.

Iniciar, arrastre un control GridView en el cuadro de herramientas hasta el Diseñador de la `DisplayOrDownloadData.aspx` página en el `BinaryData` carpeta. Establezca la s GridView `ID` a `Categories` y a través de la etiqueta inteligente de GridView s, elija para enlazarla a un nuevo origen de datos. En concreto, enlazarlo a un origen ObjectDataSource denominado `CategoriesDataSource` que recupera datos mediante el `CategoriesBLL` objeto s `GetCategories()` método.


[![Crear un nuevo origen ObjectDataSource denominado CategoriesDataSource](displaying-binary-data-in-the-data-web-controls-cs/_static/image2.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image3.png)

**Figura 2**: crear un nuevo origen ObjectDataSource denominado `CategoriesDataSource` ([haga clic aquí para ver imagen en tamaño completo](displaying-binary-data-in-the-data-web-controls-cs/_static/image4.png))


[![Configurar el origen ObjectDataSource para usar la clase CategoriesBLL](displaying-binary-data-in-the-data-web-controls-cs/_static/image3.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image5.png)

**Figura 3**: configurar el origen ObjectDataSource que se usarán el `CategoriesBLL` clase ([haga clic aquí para ver imagen en tamaño completo](displaying-binary-data-in-the-data-web-controls-cs/_static/image6.png))


[![Recuperar la lista de categorías mediante el método GetCategories()](displaying-binary-data-in-the-data-web-controls-cs/_static/image4.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image7.png)

**Figura 4**: recuperar la lista de categorías mediante el `GetCategories()` método ([haga clic aquí para ver imagen en tamaño completo](displaying-binary-data-in-the-data-web-controls-cs/_static/image8.png))


Después de completar el Asistente para configurar orígenes de datos, Visual Studio agregará automáticamente un BoundField a la `Categories` GridView para la `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`, y `BrochurePath` `DataColumn` s. Continuar y eliminar el `NumberOfProducts` BoundField desde el `GetCategories()` consulta método s no recuperar esta información. Quitar también el `CategoryID` BoundField y cambiar el nombre de la `CategoryName` y `BrochurePath` BoundFields `HeaderText` propiedades a la categoría y el folleto, respectivamente. Después de realizar estos cambios, el marcado declarativo de s GridView y ObjectDataSource debe ser similar al siguiente:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample1.aspx)]

Ver esta página a través de un explorador (consulte la figura 5). Se muestra cada una de las ocho categorías. Las siete categorías con `BrochurePath` valores tienen la `BrochurePath` valor mostrado en el BoundField respectivo. Mariscos, que tiene un `NULL` valor para su `BrochurePath`, muestra una celda vacía.


[![Se muestra cada categoría s nombre, descripción y valor BrochurePath](displaying-binary-data-in-the-data-web-controls-cs/_static/image5.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image9.png)

**Figura 5**: cada categoría s nombre, descripción, y `BrochurePath` valor aparece ([haga clic aquí para ver imagen en tamaño completo](displaying-binary-data-in-the-data-web-controls-cs/_static/image10.png))


En lugar de mostrar el texto de la `BrochurePath` columna, desea crear un vínculo al folleto. Para lograr esto, quite el `BrochurePath` BoundField y reemplazarlo por un campo Hyperlink. Establecer la nueva s campo HYPERLINK `HeaderText` propiedad folleto, su `Text` propiedad a la vista de catálogo y su `DataNavigateUrlFields` propiedad `BrochurePath`.


![Agregar un campo HYPERLINK para BrochurePath](displaying-binary-data-in-the-data-web-controls-cs/_static/image6.gif)

**Figura 6**: agregar un campo HYPERLINK para `BrochurePath`


Esto agregará una columna de vínculos en el control GridView, como se muestra en la figura 7. Al hacer clic en un vínculo de vista folleto se mostrar el PDF directamente en el explorador o pedir al usuario que descargue el archivo, dependiendo de si está instalado un lector de PDF y la configuración del explorador s.


[![Una categoría s folleto puede verse haciendo clic en el vínculo de folleto de vista](displaying-binary-data-in-the-data-web-controls-cs/_static/image7.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image11.png)

**Figura 7**: s categoría un folleto pueden verse haciendo clic en el vínculo de folleto de vista ([haga clic aquí para ver imagen en tamaño completo](displaying-binary-data-in-the-data-web-controls-cs/_static/image12.png))


[![Se muestra la categoría s folleto PDF](displaying-binary-data-in-the-data-web-controls-cs/_static/image8.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image13.png)

**Figura 8**: s de la categoría que se muestra el folleto PDF ([haga clic aquí para ver imagen en tamaño completo](displaying-binary-data-in-the-data-web-controls-cs/_static/image14.png))


## <a name="hiding-the-view-brochure-text-for-categories-without-a-brochure"></a>Ocultar el texto del folleto de vista de categorías sin un folleto

Como se muestra en la figura 7, el `BrochurePath` campo HYPERLINK muestra su `Text` el valor de propiedad (vista de catálogo) para todos los registros, independientemente de si allí s que no es`NULL` valor `BrochurePath`. Por supuesto, si `BrochurePath` es `NULL`, a continuación, se muestra el vínculo como de sólo texto, como sucede con la categoría mariscos (consulte la vuelta a la figura 7). En lugar de mostrar el texto de la vista de catálogo, sería bueno tener esas categorías sin un `BrochurePath` valor muestran algún texto alternativo, como No disponible de folleto.

Para proporcionar este comportamiento, debemos usar TemplateField cuyo contenido se genera mediante una llamada a un método de página que emite el resultado adecuado según la `BrochurePath` valor. En primer lugar exploramos esta técnica de formato en el [uso de TemplateFields en el GridView Control](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) tutorial.

Convertir el campo HYPERLINK en TemplateField seleccionando el `BrochurePath` campo Hyperlink y, a continuación, haga clic en la función Convert este campo en TemplateField vinculan en el cuadro de diálogo Editar columnas.


![Convertir el campo HYPERLINK en TemplateField](displaying-binary-data-in-the-data-web-controls-cs/_static/image9.gif)

**Figura 9**: convertir el campo HYPERLINK en TemplateField


Esto creará un TemplateField con un `ItemTemplate` que contiene un hipervínculo de Web control cuya `NavigateUrl` propiedad está enlazada a la `BrochurePath` valor. Reemplace este marcado con una llamada al método `GenerateBrochureLink`, pasando el valor de `BrochurePath`:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample2.aspx)]

A continuación, cree un `protected` método ASP.NET página clase de código subyacente de s denominada `GenerateBrochureLink` que devuelve un `string` y acepta un `object` como un parámetro de entrada.


[!code-csharp[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample3.cs)]

Este método determina si en el pasado `object` valor es una base de datos `NULL` y, si es así, devuelve un mensaje que indica que la categoría carece de un folleto. En caso contrario, si hay un `BrochurePath` valor, se muestra en un hipervínculo. Observe que si el `BrochurePath` valor es presentarlos s pasado el [ `ResolveUrl(url)` método](https://msdn.microsoft.com/library/system.web.ui.control.resolveurl.aspx). Este método resuelve en el pasado *url*, reemplazando el `~` carácter con la ruta de acceso virtual adecuada. Por ejemplo, si la aplicación se ha modificado en `/Tutorial55`, `ResolveUrl("~/Brochures/Meats.pdf")` devolverá `/Tutorial55/Brochures/Meat.pdf`.

Figura 10 muestra la página después de aplicar estos cambios. Tenga en cuenta que la categoría mariscos s `BrochurePath` campo ahora muestra el texto No folleto disponibles.


[![Los contadores de folleto de texto No se muestra para las categorías sin un folleto](displaying-binary-data-in-the-data-web-controls-cs/_static/image10.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image15.png)

**Figura 10**: se muestra el texto No folleto disponible para las categorías sin un folleto ([haga clic aquí para ver imagen en tamaño completo](displaying-binary-data-in-the-data-web-controls-cs/_static/image16.png))


## <a name="step-3-adding-a-web-page-to-display-a-category-s-picture"></a>Paso 3: Agregar una página Web para mostrar una imagen de la categoría s

Cuando un usuario visita una página ASP.NET, que reciben la s HTML de la página de ASP.NET. La HTML recibida es simplemente texto y no contener cualquier dato binario. Cualquier dato binario adicional, como imágenes, archivos de sonido, las aplicaciones de Macromedia Flash, vídeos de Windows Media Player incrustado y así sucesivamente, existe como recursos independientes en el servidor web. El código HTML contiene referencias a estos archivos, pero no incluye el contenido real de los archivos.

Por ejemplo, en HTML el `<img>` elemento se usa para hacer referencia a una imagen, con el `src` atributo al que apunta al archivo de imagen de este modo:


[!code-html[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample4.html)]

Cuando un explorador recibe este código HTML, realiza otra solicitud al servidor web para recuperar el contenido binario del archivo de imagen, que muestra a continuación, en el explorador. El mismo concepto se aplica a cualquier dato binario. En el paso 2, no se envió el folleto hacia abajo en el explorador como parte del marcado de página s HTML. En su lugar, el HTML representado proporciona hipervínculos que, al hacer clic, provocaba que el explorador solicitar el documento PDF directamente.

Para mostrar o permitir que los usuarios descargar datos binarios que se encuentra en la base de datos, es necesario crear una página web independiente que devuelve los datos. Para nuestra aplicación, solo un campo de datos binarios allí s almacenado directamente en la imagen de la base de datos la s de categoría. Por lo tanto, necesitamos una página que, cuando se llama, devuelve los datos de imagen para una categoría determinada.

Agregue una nueva página ASP.NET para la `BinaryData` carpeta denominada `DisplayCategoryPicture.aspx`. Al hacerlo, deje desactivada la casilla de verificación Seleccionar la página maestra. Esta página se espera un `CategoryID` valor en la cadena de consulta y devuelve los datos binarios de esa categoría s `Picture` columna. Puesto que esta página devuelve datos binarios y nada más, no es necesario ningún marcado en la sección HTML. Por lo tanto, haga clic en la pestaña de origen en la esquina inferior izquierda y quitar todas las revisiones de página s, excepto para el `<%@ Page %>` directiva. Es decir, `DisplayCategoryPicture.aspx` marcado declarativo s debe constar de una sola línea:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample5.aspx)]

Si ve el `MasterPageFile` atributo la `<%@ Page %>` directiva, quítela.

En la clase de código subyacente de la página s, agregue el código siguiente a la `Page_Load` controlador de eventos:


[!code-csharp[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample6.cs)]

Este código se inicia mediante la lectura de la `CategoryID` valor de cadena de consulta en una variable denominada `categoryID`. A continuación, se recuperan los datos de la imagen mediante una llamada a la `CategoriesBLL` clase s `GetCategoryWithBinaryDataByCategoryID(categoryID)` método. Estos datos se devuelven al cliente mediante el uso de la `Response.BinaryWrite(data)` método, pero antes de llama a esto, el `Picture` se debe quitar el encabezado de columna valor s OLE. Esto se logra creando un `byte` matriz denominada `strippedImageData` que va a contener precisamente 78 caracteres menor que lo que está en la `Picture` columna. El [ `Array.Copy` método](https://msdn.microsoft.com/library/z50k9bft.aspx) se usa para copiar los datos de `category.Picture` empezar en posición 78 a `strippedImageData`.

El `Response.ContentType` propiedad especifica el [tipo MIME](http://en.wikipedia.org/wiki/MIME) del contenido que se devuelve para que el explorador sabe cómo se representan. Puesto que la `Categories` tabla s `Picture` la columna es una imagen de mapa de bits, el tipo MIME del mapa de bits se usa aquí (image/bmp). Si se omite el tipo MIME, la mayoría de los exploradores se seguirá mostrando la imagen correctamente porque puede deducir el tipo según el contenido de los datos en la s de archivo de imagen binarios. Sin embargo, lo recomendable incluir el MIME de s escribir cuando sea posible. Consulte la [sitio Web de Internet Assigned Numbers Authority s](http://www.iana.org/) para obtener una lista completa de [tipos de medio MIME](http://www.iana.org/assignments/media-types/).

Con esta página creada, se puede ver la imagen de una determinada categoría s visitando `DisplayCategoryPicture.aspx?CategoryID=categoryID`. Figura 11 muestra la imagen de s de la categoría Bebidas, que puede verse desde `DisplayCategoryPicture.aspx?CategoryID=1`.


[![La s de la categoría bebidas que se muestra la imagen](displaying-binary-data-in-the-data-web-controls-cs/_static/image11.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image17.png)

**Figura 11**: s de la categoría de bebidas que se muestra la imagen ([haga clic aquí para ver imagen en tamaño completo](displaying-binary-data-in-the-data-web-controls-cs/_static/image18.png))


If, cuando se visita `DisplayCategoryPicture.aspx?CategoryID=categoryID`, obtendrá una excepción que se lee no se puede convertir el objeto de tipo "System.DBNull" al tipo 'System.Byte []', hay dos cosas que puedan estar causando esto. En primer lugar, el `Categories` tabla s `Picture` columna Permitir `NULL` valores. El `DisplayCategoryPicture.aspx` página, sin embargo, se supone que hay que no es`NULL` valor presente. El `Picture` propiedad de la `CategoriesDataTable` no se puede acceder directamente si tiene un `NULL` valor. Si desea permitir `NULL` valores para la `Picture` columna, d puede incluir la siguiente condición:


[!code-csharp[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample7.cs)]

El código anterior supone que existen algunos con nombre de archivo de imagen de s `NoPictureAvailable.gif` en el `Images` carpeta que desea mostrar para esas categorías sin una imagen.

También se podría producir esta excepción si el `CategoriesTableAdapter` s `GetCategoryWithBinaryDataByCategoryID` método s `SELECT` instrucción ha vuelto a la lista de columnas de la consulta principal s, lo que puede suceder si usa instrucciones SQL ad hoc y ve vuelva a ejecuta el Asistente para la s de TableAdapter consulta principal. Compruebe que `GetCategoryWithBinaryDataByCategoryID` método s `SELECT` instrucción todavía incluye la `Picture` columna.

> [!NOTE]
> Cada vez que el `DisplayCategoryPicture.aspx` es visitadas, la base de datos se accede y se devuelven los datos de imagen de la categoría especificada s. Si t categoría s imagen realizados cambiado desde que el usuario lo haya visto por última vez, sin embargo, esto es esfuerzo en vano. Afortunadamente, HTTP permite *condicional Obtiene*. Con una operación GET condicional, el cliente que realiza la solicitud HTTP se envía a lo largo de un [ `If-Modified-Since` encabezado HTTP](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html) que proporciona la fecha y hora que el cliente recuperó por última vez este recurso desde el servidor web. Si no ha cambiado el contenido ya que esto especifica la fecha, el servidor web puede responder con un [código de estado no modificado (304)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) y renuncian a devolver el contenido del recurso solicitado. En resumen, esta técnica evita tener que enviar el contenido de un recurso si no se ha modificado desde que el cliente tiene acceso por última vez el servidor web.


Para implementar este comportamiento, sin embargo, requiere que se agregue un `PictureLastModified` columna a la `Categories` tabla para capturar cuando el `Picture` columna se actualizó por última vez, así como código para comprobar la `If-Modified-Since` encabezado. Para obtener más información sobre la `If-Modified-Since` encabezado y el flujo de trabajo GET condicional, consulte [HTTP GET condicional para los Hackers RSS](http://fishbowl.pastiche.org/2002/10/21/http_conditional_get_for_rss_hackers) y [A Look más profunda al realizar solicitudes HTTP en una página ASP.NET](http://aspnet.4guysfromrolla.com/articles/122204-1.aspx).

## <a name="step-4-displaying-the-category-pictures-in-a-gridview"></a>Paso 4: Mostrar las imágenes de categoría en un control GridView

Ahora que tenemos una página web para mostrar la imagen de una determinada categoría s, podemos mostrar mediante el [control imagen Web](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/image.aspx) o HTML `<img>` elemento al que apunta a `DisplayCategoryPicture.aspx?CategoryID=categoryID`. Imágenes cuya dirección URL viene determinada por la base de datos se pueden mostrar en el control GridView o DetailsView mediante ImageField. Contiene ImageField `DataImageUrlField` y `DataImageUrlFormatString` propiedades que funcionan como las operaciones de asignación campo HYPERLINK `DataNavigateUrlFields` y `DataNavigateUrlFormatString` propiedades.

Permiten s aumentar la `Categories` GridView en `DisplayOrDownloadData.aspx` agregando un ImageField para mostrar cada imagen de la categoría s. Simplemente agregue ImageField y establezca su `DataImageUrlField` y `DataImageUrlFormatString` propiedades a `CategoryID` y `DisplayCategoryPicture.aspx?CategoryID={0}`, respectivamente. Esto creará una columna de GridView que presenta un `<img>` elemento cuyo `src` las referencias de atributo `DisplayCategoryPicture.aspx?CategoryID={0}`, donde {0} se reemplaza por la fila GridView s `CategoryID` valor.


![Agregar un ImageField en GridView](displaying-binary-data-in-the-data-web-controls-cs/_static/image12.gif)

**Figura 12**: agregar un ImageField en GridView


Después de agregar ImageField, su sintaxis declarativa del control GridView s debería parezca aliviar siguientes:


[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-cs/samples/sample8.aspx)]

Dedique un momento para ver esta página a través de un explorador. Tenga en cuenta cómo cada registro ahora incluye una imagen de la categoría.


[![La categoría s imagen se muestra para cada fila](displaying-binary-data-in-the-data-web-controls-cs/_static/image13.gif)](displaying-binary-data-in-the-data-web-controls-cs/_static/image19.png)

**Figura 13**: s de la categoría que se muestra la imagen para cada fila ([haga clic aquí para ver imagen en tamaño completo](displaying-binary-data-in-the-data-web-controls-cs/_static/image20.png))


## <a name="summary"></a>Resumen

En este tutorial hemos visto cómo presentar los datos binarios. ¿Cómo se presentan los datos depende del tipo de datos. Para los archivos de folleto PDF, se ofrece al usuario un folleto de vista de vínculo que, al hacer clic, tardó el usuario directamente en el archivo PDF. Para la imagen de s categoría, se crea por primera vez una página para recuperar y devolver los datos binarios de la base de datos y, a continuación, utiliza esa página para mostrar cada imagen de s de categoría en un control GridView.

Ahora que hemos examinado ve cómo mostrar datos binarios, que está listo para examinar cómo realizar la inserción, actualizaciones y eliminaciones en la base de datos con los datos binarios. En el siguiente tutorial daremos un vistazo a cómo asociar un archivo cargado a su registro de base de datos correspondiente. En el tutorial después de eso, veremos cómo actualizar los datos binarios existentes, así como la eliminación de los datos binarios cuando se quita su registro asociado.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Los revisores para este tutorial fueron Teresa Murphy y Dave Gardner. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](uploading-files-cs.md)
> [Siguiente](including-a-file-upload-option-when-adding-a-new-record-cs.md)
