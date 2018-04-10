---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-cs
title: Principal-detalle filtrado a través de dos páginas (C#) | Documentos de Microsoft
author: rick-anderson
description: En este tutorial implementaremos este patrón mediante un control GridView para enumerar los proveedores en la base de datos. Cada fila del proveedor de GridView contendrá una p...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 552d2d50-fe73-4153-9a7f-2b379bec4625
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 0858bf9c8dc380898647293825145654ac1dbc34
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="masterdetail-filtering-across-two-pages-c"></a>Principal-detalle filtrado a través de dos páginas (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_9_CS.exe) o [descarga de PDF](master-detail-filtering-across-two-pages-cs/_static/datatutorial09cs1.pdf)

> En este tutorial implementaremos este patrón mediante un control GridView para enumerar los proveedores en la base de datos. Cada fila del proveedor de GridView contendrá un vínculo de ver los productos que, al hacer clic en, se guiará al usuario a una página independiente muestra los productos para el proveedor seleccionado.


## <a name="introduction"></a>Introducción

En los dos tutoriales anteriores hemos visto cómo [mostrar informes de maestro y detalles en una sola página de web con listas desplegables](master-detail-filtering-with-a-dropdownlist-cs.md) a [mostrar los registros "principales" y un control GridView o DetailsView](master-detail-filtering-with-two-dropdownlists-cs.md) para mostrar el " Detalles". Otro patrón común que se utiliza para los informes de maestro y detalles es que los registros maestros en una página web y los detalles mostrados en otro. Un sitio Web del foro, como [los foros de ASP.NET](https://forums.asp.net/), es un buen ejemplo de este patrón en la práctica. Los foros de ASP.NET están compuestos por una variedad de foros de introducción, formularios Web Forms, controles de presentación de datos y así sucesivamente. Cada foro se compone de muchos subprocesos y cada subproceso se compone de un número de entradas. En la página principal de foros de ASP.NET, se enumeran los foros. Al hacer clic en un foro de inmediato a un `ShowForum.aspx` página, que muestra los subprocesos de foro. Del mismo modo, al hacer clic en un subproceso para ir a `ShowPost.aspx`, que muestra las entradas para el subproceso que se hizo clic.

En este tutorial implementaremos este patrón mediante un control GridView para enumerar los proveedores en la base de datos. Cada fila del proveedor de GridView contendrá un vínculo de ver los productos que, al hacer clic en, se guiará al usuario a una página independiente muestra los productos para el proveedor seleccionado.

## <a name="step-1-addingsupplierlistmasteraspxandproductsforsupplierdetailsaspxpages-to-thefilteringfolder"></a>Paso 1: Agregar`SupplierListMaster.aspx`y`ProductsForSupplierDetails.aspx`páginas para el`Filtering`carpeta

Al definir el diseño de página en el tercer tutorial agregamos un número de páginas de "inicio" en la `BasicReporting`, `Filtering`, y `CustomFormatting` carpetas. Sin embargo, se no agregó una página de inicio para este tutorial en ese momento, así que dedique un momento para agregar dos páginas nuevas a la `Filtering` carpeta: `SupplierListMaster.aspx` y `ProductsForSupplierDetails.aspx`. `SupplierListMaster.aspx` mostrará una lista de los registros "principales" (los proveedores) al `ProductsForSupplierDetails.aspx` mostrará los productos para el proveedor seleccionado.

Al crear estas dos páginas nuevas ser ciertas asociarlos con el `Site.master` página maestra.


![Agregar las páginas de ProductsForSupplierDetails.aspx y SupplierListMaster.aspx a la carpeta de filtrado](master-detail-filtering-across-two-pages-cs/_static/image1.png)

**Figura 1**: agregar la `SupplierListMaster.aspx` y `ProductsForSupplierDetails.aspx` páginas para el `Filtering` carpeta


Además, al agregar nuevas páginas al proyecto, asegúrese de actualizar el archivo de asignación de sitio `Web.sitemap`, según corresponda. Para este tutorial basta con agregar el `SupplierListMaster.aspx` página para la asignación de sitio mediante el siguiente contenido XML como un elemento secundario de los informes de filtrado `<siteMapNode>` elemento:


[!code-xml[Main](master-detail-filtering-across-two-pages-cs/samples/sample1.xml)]

> [!NOTE]
> Puede ayudar a automatizar el proceso de actualizar el archivo de asignación de sitio al agregar nuevos ASP.NET páginas mediante [K. Scott Allen](http://odetocode.com/Blogs/scott/)del libre Visual Studio [macros de mapa de sitio](http://odetocode.com/Blogs/scott/archive/2005/11/29/2537.aspx).


## <a name="step-2-displaying-the-supplier-list-insupplierlistmasteraspx"></a>Paso 2: Mostrar la lista de proveedores en`SupplierListMaster.aspx`

Con el `SupplierListMaster.aspx` y `ProductsForSupplierDetails.aspx` páginas creadas, el paso siguiente consiste en crear GridView de proveedores en `SupplierListMaster.aspx`. Agregar un control GridView a la página y enlazarla a un nuevo origen ObjectDataSource. Debe usar este ObjectDataSource el `SuppliersBLL` la clase `GetSuppliers()` método para devolver todos los proveedores.


[![Seleccione la clase SuppliersBLL](master-detail-filtering-across-two-pages-cs/_static/image3.png)](master-detail-filtering-across-two-pages-cs/_static/image2.png)

**Figura 2**: seleccione la `SuppliersBLL` clase ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-across-two-pages-cs/_static/image4.png))


[![Configurar el ObjectDataSource para utilizar el método GetSuppliers()](master-detail-filtering-across-two-pages-cs/_static/image6.png)](master-detail-filtering-across-two-pages-cs/_static/image5.png)

**Figura 3**: configurar el ObjectDataSource que se utilizan los `GetSuppliers()` método ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-across-two-pages-cs/_static/image7.png))


Es necesario incluir un vínculo denominado Ver productos en cada fila de GridView que, al hacer clic, lleva al usuario a `ProductsForSupplierDetails.aspx` pasar en la fila seleccionada `SupplierID` valor a través de la cadena de consulta. Por ejemplo, si el usuario hace clic en el vínculo Ver productos para el proveedor Comercial Tasmania (que tiene un `SupplierID` valor 4), deben enviarse a `ProductsForSupplierDetails.aspx?SupplierID=4`.

Para ello, agregue un [campo HYPERLINK](https://msdn.microsoft.com/library/system.web.ui.webcontrols.hyperlinkfield.aspx) a GridView, que agrega un hipervínculo para cada fila de GridView. Iniciar haciendo clic en el vínculo Editar columnas de etiqueta inteligente de GridView. A continuación, seleccione el campo Hyperlink de la lista en la parte superior izquierda y haga clic en Agregar para incluir el campo HYPERLINK en lista de campos de GridView.


[![Agregar un campo Hyperlink a GridView](master-detail-filtering-across-two-pages-cs/_static/image9.png)](master-detail-filtering-across-two-pages-cs/_static/image8.png)

**Figura 4**: agregar un campo Hyperlink a GridView ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-across-two-pages-cs/_static/image10.png))


El campo HYPERLINK puede configurarse para usar el mismo texto o valores en cada fila de GridView el vínculo dirección URL, o puede basar estos valores en los valores de datos enlazados a cada fila determinada. Para especificar una variable static valor a través de todas las filas, usar el campo de Hyperlink `Text` o `NavigateUrl` propiedades. Puesto que deseamos que el texto del vínculo a ser el mismo para todas las filas, establezca el campo de Hyperlink `Text` propiedad para ver los productos.


[![Establecer la propiedad de texto del campo de Hyperlink para ver los productos](master-detail-filtering-across-two-pages-cs/_static/image12.png)](master-detail-filtering-across-two-pages-cs/_static/image11.png)

**Figura 5**: establecer el campo de Hyperlink `Text` propiedad para ver los productos ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-across-two-pages-cs/_static/image13.png))


Para establecer el texto o los valores de dirección URL se basen en los datos subyacentes enlazados a la fila de GridView, especifique el texto de los datos de campos o valores de dirección URL deben extraerse de en el `DataTextField` o `DataNavigateUrlFields` propiedades. `DataTextField` solo puede establecerse en un único campo de datos; `DataNavigateUrlFields`, sin embargo, se puede establecer en una lista delimitada por comas de los campos de datos. Con frecuencia, es necesario basar el texto o la dirección URL en una combinación del valor de campo de datos de la fila actual y algún marcado estático. En este tutorial, por ejemplo, queremos que la dirección URL de los vínculos del campo de Hyperlink sea `ProductsForSupplierDetails.aspx?SupplierID=supplierID`, donde *`supplierID`* es la fila de cada GridView `SupplierID` valor. Observe que necesitamos estáticos y controladas por datos valores aquí: el `ProductsForSupplierDetails.aspx?SupplierID=` parte de la dirección URL del vínculo es estático, mientras que la *`supplierID`* parte está controlada por datos como su valor es propio de cada fila `SupplierID` valor.

Para indicar una combinación de valores estáticos y controlados por datos, utilice la `DataTextFormatString` y `DataNavigateUrlFormatString` propiedades. En estas propiedades, escriba el marcado estático según sea necesario y, a continuación, use el marcador `{0}` donde desea que el valor del campo especificado en el `DataTextField` o `DataNavigateUrlFields` propiedades que aparecen. Si el `DataNavigateUrlFields` propiedad tiene uso especificado de varios campos `{0}` donde desea que el primer valor del campo insertado, `{1}` para el segundo valor de campo y así sucesivamente.

Aplicar esto en nuestro tutorial, es necesario establecer la `DataNavigateUrlFields` propiedad `SupplierID`, ya que es el campo de datos cuyo valor es necesario personalizar según una fila por fila, y el `DataNavigateUrlFormatString` propiedad `ProductsForSupplierDetails.aspx?SupplierID={0}`.


[![Configurar el campo HYPERLINK para incluir la dirección URL de vínculo adecuado según la columna SupplierID](master-detail-filtering-across-two-pages-cs/_static/image15.png)](master-detail-filtering-across-two-pages-cs/_static/image14.png)

**Figura 6**: configurar el campo HYPERLINK para incluir el apropiado vínculo dirección URL basada en la `SupplierID` ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-across-two-pages-cs/_static/image16.png))


Después de agregar el campo HYPERLINK, no dude en Personalizar y reordenar los campos de GridView. El marcado siguiente muestra el control GridView después de que he realizado algunas pequeñas personalizaciones de nivel de campo.


[!code-aspx[Main](master-detail-filtering-across-two-pages-cs/samples/sample2.aspx)]

Tómese un momento para ver la `SupplierListMaster.aspx` página a través de un explorador. Como se muestra en la figura 7, la página actualmente enumera todos los proveedores con un vínculo Ver productos. Al hacer clic en Ver productos vínculo le llevará a `ProductsForSupplierDetails.aspx`, pasando a lo largo del proveedor `SupplierID` en la cadena de consulta.


[![Cada fila de proveedor contiene un vínculo de productos de vista](master-detail-filtering-across-two-pages-cs/_static/image18.png)](master-detail-filtering-across-two-pages-cs/_static/image17.png)

**Figura 7**: cada fila de proveedor contiene un vínculo de productos de vista ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-across-two-pages-cs/_static/image19.png))


## <a name="step-3-listing-the-suppliers-products-inproductsforsupplierdetailsaspx"></a>Paso 3: Enumerar los productos del proveedor en`ProductsForSupplierDetails.aspx`

En este momento la `SupplierListMaster.aspx` página envía a los usuarios `ProductsForSupplierDetails.aspx`, pasando el proveedor seleccionado `SupplierID` en la cadena de consulta. Paso final del tutorial es mostrar los productos en un control GridView en `ProductsForSupplierDetails.aspx` cuyo `SupplierID` es igual a la `SupplierID` pasa a través de la cadena de consulta. Para realizar esta guía de inicio mediante la adición de un control GridView a la `ProductsForSupplierDetails.aspx` página, con un nuevo control ObjectDataSource denominado `ProductsBySupplierDataSource` que invoca la `GetProductsBySupplierID(supplierID)` método desde el `ProductsBLL` clase.


[![Agregar un nuevo origen ObjectDataSource denominado ProductsBySupplierDataSource](master-detail-filtering-across-two-pages-cs/_static/image21.png)](master-detail-filtering-across-two-pages-cs/_static/image20.png)

**Figura 8**: agregue un nuevo ObjectDataSource denominado `ProductsBySupplierDataSource` ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-across-two-pages-cs/_static/image22.png))


[![Seleccione la clase ProductsBLL](master-detail-filtering-across-two-pages-cs/_static/image24.png)](master-detail-filtering-across-two-pages-cs/_static/image23.png)

**Figura 9**: seleccione la `ProductsBLL` clase ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-across-two-pages-cs/_static/image25.png))


[![Tiene el ObjectDataSource invocar el método GetProductsBySupplierID(supplierID)](master-detail-filtering-across-two-pages-cs/_static/image27.png)](master-detail-filtering-across-two-pages-cs/_static/image26.png)

**Figura 10**: tiene la invocación de ObjectDataSource el `GetProductsBySupplierID(supplierID)` método ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-across-two-pages-cs/_static/image28.png))


El último paso del Asistente para configurar origen de datos le pide que proporcione el origen de la `GetProductsBySupplierID(supplierID)` del método *`supplierID`* parámetro. Para utilizar el valor de cadena de consulta, establezca el origen de parámetro en la cadena de consulta y escriba el nombre del valor de cadena de consulta para utilizar en el cuadro de texto QueryStringField (`SupplierID`).


[![Rellenar el valor del parámetro del valor de cadena de consulta SupplierID supplierID](master-detail-filtering-across-two-pages-cs/_static/image30.png)](master-detail-filtering-across-two-pages-cs/_static/image29.png)

**Figura 11**: rellenar el *`supplierID`* valor del parámetro de la `SupplierID` valor de cadena de consulta ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-across-two-pages-cs/_static/image31.png))


Eso es todo lo! La figura 12 muestra el `ProductsForSupplierDetails.aspx` página cuando visita haciendo clic en el vínculo de Comercial Tasmania desde `SupplierListMaster.aspx`.


[![Se muestran los productos suministrados por Tokyo Traders](master-detail-filtering-across-two-pages-cs/_static/image33.png)](master-detail-filtering-across-two-pages-cs/_static/image32.png)

**Figura 12**: se muestran los productos suministrados por Tokyo Traders ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-across-two-pages-cs/_static/image34.png))


## <a name="displaying-supplier-information-inproductsforsupplierdetailsaspx"></a>Mostrar información de proveedor en`ProductsForSupplierDetails.aspx`

Como se muestra en la figura 12, el `ProductsForSupplierDetails.aspx` página muestra una lista con los productos suministrados por el `SupplierID` especificado en la cadena de consulta. Alguien le envió directamente a esta página, sin embargo, no sabría que figura 12 aparece productos comercial de Tasmania. Para solucionar este problema podemos mostrar información del proveedor en esta página también.

Empiece agregando un FormView por encima de los productos de GridView. Crear un nuevo control ObjectDataSource denominado `SuppliersDataSource` que invoca la `SuppliersBLL` la clase `GetSupplierBySupplierID(supplierID)` método.


[![Seleccione la clase SuppliersBLL](master-detail-filtering-across-two-pages-cs/_static/image36.png)](master-detail-filtering-across-two-pages-cs/_static/image35.png)

**Figura 13**: seleccione la `SuppliersBLL` clase ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-across-two-pages-cs/_static/image37.png))


[![Tiene el ObjectDataSource invocar el método GetSupplierBySupplierID(supplierID)](master-detail-filtering-across-two-pages-cs/_static/image39.png)](master-detail-filtering-across-two-pages-cs/_static/image38.png)

**Figura 14**: tiene la invocación de ObjectDataSource el `GetSupplierBySupplierID(supplierID)` método ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-across-two-pages-cs/_static/image40.png))


Al igual que con la `ProductsBySupplierDataSource`, tiene la *`supplierID`* parámetro asignado el valor de la `SupplierID` valor de cadena de consulta.


[![Rellenar el valor del parámetro del valor de cadena de consulta SupplierID supplierID](master-detail-filtering-across-two-pages-cs/_static/image42.png)](master-detail-filtering-across-two-pages-cs/_static/image41.png)

**Figura 15**: rellenar el *`supplierID`* valor del parámetro de la `SupplierID` valor de cadena de consulta ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-across-two-pages-cs/_static/image43.png))


Cuando se enlazan FormView a ObjectDataSource en la vista Diseño, Visual Studio creará automáticamente el FormView `ItemTemplate`, `InsertItemTemplate`, y `EditItemTemplate` con controles Web Label y TextBox para cada uno de los campos de datos devueltos por el ObjectDataSource. Puesto que queremos mostrar proveedor información Llamarme quitar el `InsertItemTemplate` y `EditItemTemplate`. A continuación, edite el ItemTemplate para que se muestre el nombre de la compañía del proveedor en un `<h3>` elemento y la dirección, ciudad, país y número de teléfono bajo el nombre de la compañía. Como alternativa, puede establecer manualmente el FormView `DataSourceID` y crear el `ItemTemplate` marcado, como hicimos en el "[mostrar datos con el ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md)" tutorial.

Después de estas modificaciones marcado declarativo de FormView debe ser similar al siguiente:


[!code-aspx[Main](master-detail-filtering-across-two-pages-cs/samples/sample3.aspx)]

La figura 16 muestra una captura de pantalla de la `ProductsForSupplierDetails.aspx` página después de la información de proveedor detallada anteriormente se ha incluido.


[![La lista de productos incluye un resumen sobre el proveedor](master-detail-filtering-across-two-pages-cs/_static/image45.png)](master-detail-filtering-across-two-pages-cs/_static/image44.png)

**Figura 16**: la lista de productos incluye un resumen sobre el proveedor ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-across-two-pages-cs/_static/image46.png))


## <a name="applying-the-final-touches-for-theproductsforsupplierdetailsaspxui"></a>Aplicar la última toca para el`ProductsForSupplierDetails.aspx`interfaz de usuario

Para mejorar el usuario experiencia para este informe no existe, se muestran un par de adiciones que se deben realizar en el `ProductsForSupplierDetails.aspx` página. Actualmente la única forma de un usuario puede ir desde la `ProductsForSupplierDetails.aspx` página a la lista de proveedores es hacer clic en el botón Atrás de su explorador. Vamos a agregar un control de hipervínculo para la `ProductsForSupplierDetails.aspx` página que vínculos volver a `SupplierListMaster.aspx`, lo que proporciona otro manera para el usuario volver a la lista maestra.


[![Agregar un Control de hipervínculo para llevar al usuario volver a SupplierListMaster.aspx](master-detail-filtering-across-two-pages-cs/_static/image48.png)](master-detail-filtering-across-two-pages-cs/_static/image47.png)

**Figura 17**: agregar un Control de hipervínculo para aprovechar el nuevo usuario a `SupplierListMaster.aspx` ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-across-two-pages-cs/_static/image49.png))


Si el usuario hace clic en el vínculo Ver productos para un proveedor que no tenga los productos, el `ProductsBySupplierDataSource` ObjectDataSource en `ProductsForSupplierDetails.aspx` no devuelve ningún resultado. GridView enlazado a ObjectDataSource no representa ningún otro marcado resultante en un área en blanco de la página en el explorador del usuario. Para mayor claridad comunicarse con el usuario que no hay productos asociados con el proveedor seleccionado podemos establecer la GridView `EmptyDataText` propiedad al mensaje que desea que se muestre cuando se produzca esta situación. He establecido esta propiedad en "No hay productos suministrados por este proveedor"

De forma predeterminada, todos los proveedores en la base de datos Northwinds proporcionan al menos uno de los productos. Sin embargo, para este tutorial aunque he manualmente se ha modificado el `Products` para que el proveedor caracoles Nouveaux ya no está asociado a algún producto de la tabla. Figura 18 muestra la página de detalles para caracoles Nouveaux después de que se ha realizado este cambio.


[![Se informaren a los usuarios que el proveedor no proporciona todos los productos](master-detail-filtering-across-two-pages-cs/_static/image51.png)](master-detail-filtering-across-two-pages-cs/_static/image50.png)

**Figura 18**: los usuarios les informa de que el proveedor no proporciona ningún producto ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-across-two-pages-cs/_static/image52.png))


## <a name="summary"></a>Resumen

Mientras los informes de maestro y detalles pueden mostrar registros maestros y de detalle en una sola página, en muchos sitios Web se dividen en dos páginas web. En este tutorial explicamos cómo implementar estos informes principal-detalle haciendo que los proveedores que aparecen en un control GridView en la página web "maestra" y los productos asociados que aparecen en la página "Detalles". Cada fila del proveedor en la página web principal contiene un vínculo a la página de detalles que se pasa a lo largo de la fila `SupplierID` valor. Estos vínculos específicas de la fila se pueden agregar fácilmente utilizando campo Hyperlink de GridView.

En la página de detalles al recuperar los productos para el proveedor especificado se logra invocando la `ProductsBLL` la clase `GetProductsBySupplierID(supplierID)` método. El *`supplierID`* el valor del parámetro se especifica mediante declaración con la cadena de consulta como origen del parámetro. También explicamos cómo mostrar los detalles del proveedor en la página de detalles mediante un FormView.

Nuestro [siguiente tutorial](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) es el final en los informes de maestro y detalles. Analizaremos cómo mostrar una lista de productos en un GridView donde cada fila tiene un botón de selección. Al hacer clic en el botón Seleccionar mostrará detalles de ese producto en un control DetailsView en la misma página.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Revisor inicial para este tutorial era Hilton Giesenow. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](master-detail-filtering-with-two-dropdownlists-cs.md)
> [Siguiente](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)
