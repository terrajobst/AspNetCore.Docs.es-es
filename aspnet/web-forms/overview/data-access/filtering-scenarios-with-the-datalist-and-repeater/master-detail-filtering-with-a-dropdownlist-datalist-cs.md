---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-cs
title: Principal-detalle filtrado con DropDownList (C#) | Documentos de Microsoft
author: rick-anderson
description: En este tutorial veremos cómo mostrar informes de maestro y detalles en una sola página web mediante las listas desplegables para mostrar los registros de 'master' y un control DataList en la pantalla de...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 07fa47ae-e491-4a2f-b265-d342b9ddef46
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: c84902ccf028c976246380abfaebb6a76c573603
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="masterdetail-filtering-with-a-dropdownlist-c"></a>Principal-detalle filtrado con DropDownList (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_33_CS.exe) o [descarga de PDF](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/datatutorial33cs1.pdf)

> En este tutorial veremos cómo mostrar informes de maestro y detalles en una sola página web mediante las listas desplegables para mostrar los registros "principales" y un control DataList para mostrar los detalles de"".


## <a name="introduction"></a>Introducción

El informe maestro y detalles, que hemos creado en primer lugar mediante un control GridView en la anterior [filtrado de maestro y detalles con un DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) tutorial, comienza por mostrar un conjunto de registros "principales". El usuario puede, a continuación, profundizar en uno de los registros maestros, ver, por tanto, "Detalles" de ese registro maestro. Los informes de maestro y detalles son una opción ideal para visualizar las relaciones de uno a varios y mostrar información detallada de especialmente "anchos" tablas (las que tiene una gran cantidad de columnas). Hemos analizado cómo implementar los informes de maestro y detalles mediante los controles GridView y DetailsView en tutoriales anteriores. En este tutorial y los dos siguientes, se podrá examinar estos conceptos, pero el foco sobre el uso de DataList y repetidor controles en su lugar.

En este tutorial, se examinará usando DropDownList para contener los registros "principales", con los registros de "Detalles" mostrados en un control DataList.

## <a name="step-1-adding-the-masterdetail-tutorial-web-pages"></a>Paso 1: Agregar las páginas Web del Tutorial principal-detalle

Antes de empezar este tutorial, primero dedique un momento para agregar la carpeta y las páginas ASP.NET que se necesitará para este tutorial y los dos siguientes relacionados con informes de principal-detalle utilizando los controles DataList y repetidor. Empiece por crear una nueva carpeta en el proyecto denominado `DataListRepeaterFiltering`. A continuación, agregue las siguientes páginas ASP.NET cinco a esta carpeta, todas ellas configurada para usar la página maestra con `Site.master`:

- `Default.aspx`
- `FilterByDropDownList.aspx`
- `CategoryListMaster.aspx`
- `ProductsForCategoryDetails.aspx`
- `CategoriesAndProducts.aspx`


![Cree una carpeta DataListRepeaterFiltering y agregar las páginas ASP.NET Tutorial](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image1.png)

**Figura 1**: crear un `DataListRepeaterFiltering` carpeta y agregar las páginas ASP.NET Tutorial


A continuación, abra el `Default.aspx` página y arrastre el `SectionLevelTutorialListing.ascx` Control de usuario desde el `UserControls` carpeta a la superficie de diseño. Este Control de usuario, que hemos creado en el [páginas maestras y navegación del sitio](../introduction/master-pages-and-site-navigation-cs.md) tutorial, enumera el mapa del sitio y muestra los tutoriales de la sección actual en una lista con viñetas.


[![Agregar el Control de usuario SectionLevelTutorialListing.ascx a Default.aspx](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image3.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image2.png)

**Figura 2**: agregar la `SectionLevelTutorialListing.ascx` Control de usuario a `Default.aspx` ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image4.png))


Para disponer de la pantalla de lista con viñetas los tutoriales de maestro y detalles que se va a crear, es necesario agregarlos a la asignación de sitio. Abra la `Web.sitemap` de archivos y agregue el siguiente marcado después de las marcas de nodo "Mostrar datos con el DataList y repetidor" asignación de sitio:

[!code-xml[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample1.xml)]


![Actualizar el mapa del sitio para incluir las nuevas páginas ASP.NET](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image5.png)

**Figura 3**: actualizar el mapa del sitio para incluir las nuevas páginas ASP.NET


## <a name="step-2-displaying-the-categories-in-a-dropdownlist"></a>Paso 2: Mostrar las categorías en DropDownList

Nuestro informe principal-detalle enumerará las categorías de DropDownList, con productos del elemento de lista seleccionado que se muestran más abajo en la página en un control DataList. La primera tarea por delante de nosotros, a continuación, es que las categorías que se muestran en DropDownList. Comience abriendo la `FilterByDropDownList.aspx` página en el `DataListRepeaterFiltering` carpeta y arrastre un DropDownList desde el cuadro de herramientas hasta el Diseñador de la página. A continuación, establezca el DropDownList `ID` propiedad `Categories`. Haga clic en el vínculo Elegir origen de datos de etiqueta inteligente de DropDownList y crear un nuevo origen ObjectDataSource denominado `CategoriesDataSource`.


[![Agregar un nuevo origen ObjectDataSource denominado CategoriesDataSource](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image7.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image6.png)

**Figura 4**: agregue un nuevo ObjectDataSource denominado `CategoriesDataSource` ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image8.png))


Configurar el nuevo ObjectDataSource de tal manera que invoca el `CategoriesBLL` la clase `GetCategories()` método. Después de configurar el ObjectDataSource necesitamos especificar qué campo del origen de datos se debe mostrar en DropDownList y que uno debería estar asociado como el valor de cada elemento de lista. Tiene la `CategoryName` campo como la presentación y `CategoryID` como el valor de cada elemento de lista.


[![Tiene la presentación de DropDownList el campo de nombre de categoría y CategoryID de uso como el valor](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image10.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image9.png)

**Figura 5**: tiene la presentación de DropDownList el `CategoryName` campo y Use `CategoryID` como el valor ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image11.png))


En este momento tenemos un control DropDownList que se rellena con los registros de la `Categories` tabla (todos lleva a cabo en torno a seis segundos). La figura 6 muestra nuestro progreso hasta el momento cuando se ven a través de un explorador.


[![Un cuadro desplegable enumera las categorías actuales](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image13.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image12.png)

**Figura 6**: un cuadro desplegable enumera las categorías actuales ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image14.png))


## <a name="step-2-adding-the-products-datalist"></a>Paso 2: Agregar al control DataList de productos

El último paso de nuestro informe principal-detalle es enumerar los productos asociados con la categoría seleccionada. Para ello, agregue un control DataList a la página y crear un nuevo origen ObjectDataSource denominado `ProductsByCategoryDataSource`. Tiene la `ProductsByCategoryDataSource` control recupere sus datos desde el `ProductsBLL` la clase `GetProductsByCategoryID(categoryID)` método. Puesto que este informe principal-detalle es de solo lectura, elija la que opción el (ninguno) en las pestañas INSERT, UPDATE y DELETE.


[![Seleccione el método GetProductsByCategoryID(categoryID)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image16.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image15.png)

**Figura 7**: seleccione la `GetProductsByCategoryID(categoryID)` método ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image17.png))


Después de hacer clic en siguiente, el Asistente ObjectDataSource nos solicita para el origen del valor de la `GetProductsByCategoryID(categoryID)` del método *`categoryID`* parámetro. Para usar el valor de seleccionado `categories` DropDownList elemento establezca el origen de parámetro para el Control y la ControlID a `Categories`.


[![Establece el parámetro categoryID en el valor de las categorías de DropDownList](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image19.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image18.png)

**Figura 8**: establecer el *`categoryID`* parámetro en el valor de la `Categories` DropDownList ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image20.png))


Tras completar el Asistente para configurar orígenes de datos, Visual Studio generará automáticamente un `ItemTemplate` para el control DataList que muestra el nombre y el valor de cada campo de datos. Vamos a mejorar el control DataList usar en su lugar un `ItemTemplate` que muestre solo el nombre del producto, categoría, proveedor, cantidad por unidad y el precio junto con un `SeparatorTemplate` que inserta un `<hr>` elemento entre cada elemento. Voy a usar el `ItemTemplate` de ejemplo en el [mostrar datos con los controles de repetidor y DataList](../displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs.md) tutorial, pero puede usar cualquier formato de plantilla encuentra más visualmente atractivo.

Después de realizar estos cambios, su DataList y marcado de su ObjectDataSource deben ser similares al siguiente:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample2.aspx)]

Tómese un momento para desproteger nuestro progreso en un explorador. Al visitar primero la página, se muestran los productos que pertenecen a la categoría seleccionada (bebidas) (como se muestra en la figura 9), pero cambiar DropDownList no actualiza los datos. Esto es porque se debe producir una devolución de datos para que el control DataList actualizar. Para lograr esto se puede establece la DropDownList `AutoPostBack` propiedad `true` o agregar un control de botón Web a la página. Para este tutorial, he optado por establecer la DropDownList `AutoPostBack` propiedad `true`.

Las figuras 9 y 10 se muestra el informe maestro y detalles en acción.


[![Al visitar primero la página, se muestran los productos de bebidas](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image22.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image21.png)

**Figura 9**: al primero, visite la página, se muestran los productos de bebidas ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image23.png))


[![Al seleccionar un producto nuevo (crear) automáticamente, una devolución de datos, actualizar el control DataList](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image25.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image24.png)

**Figura 10**: seleccionar automáticamente un nuevo producto (producen) provoca una devolución de datos, actualizar el control DataList ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image26.png))


## <a name="adding-a----choose-a-category----list-item"></a>Agregar un elemento de lista ", elija una categoría:"

Al visitar primero la `FilterByDropDownList.aspx` página las categorías primer elemento de lista del DropDownList (bebidas) está activada de forma predeterminada, que muestra los productos de bebidas en el control DataList. En el *filtrado de maestro y detalles con un DropDownList* tutorial hemos agregado una opción "--Elija una categoría:" a los controles de DropDownList que estaba seleccionada de forma predeterminada y, cuando se selecciona, muestra *todos los* de la productos en la base de datos. Este enfoque era administrable, al enumerar los productos en un control GridView, ya que cada fila del producto alcanzaba un una pequeña cantidad de espacio en pantalla. Sin embargo, con el control DataList, información de cada producto consume un fragmento mucho mayor de la pantalla. Vamos a seguir agregar una opción "--Elija una categoría:" y hacer que se selecciona de forma predeterminada, pero en lugar de tener mostrar todos los productos cuando se selecciona, vamos a configurarlo para que no se muestre ningún producto.

Para agregar un nuevo elemento de lista a los controles DropDownList, vaya a la ventana Propiedades y haga clic en el botón de puntos suspensivos en el `Items` propiedad. Agregar un nuevo elemento de lista con el `Text` "--Elija una categoría:" y el `Value` `0`.


![Agregar un](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image27.png)

**Figura 11**: agregar un elemento de lista ", elija una categoría:"


Como alternativa, puede agregar el elemento de lista, agregue el siguiente marcado al DropDownList:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample3.aspx)]

Además, es necesario establecer el control DropDownList `AppendDataBoundItems` a `true` porque si se establece en `false` (valor predeterminado), cuando las categorías se enlazan a los controles DropDownList de ObjectDataSource sobrescribirán cualquier lista agregado manualmente elementos.


![Establezca la propiedad AppendDataBoundItems en True](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image28.png)

**Figura 12**: establecer el `AppendDataBoundItems` propiedad en True


El motivo que elegimos el valor `0` para la lista ", elija una categoría:" elemento es porque no hay ninguna categoría en el sistema con un valor de `0`, por lo tanto, no se devolverá ningún registro de producto cuando se selecciona el elemento de lista ", elija una categoría--". Para confirmarlo, tómese un momento para visitar la página a través de un explorador. Como se muestra en la figura 13, cuando inicialmente viendo la página se selecciona el elemento de lista ", elija una categoría:" y no se muestra ningún producto.


[![Cuando el](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image30.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image29.png)

**Figura 13**: cuando se selecciona el elemento de lista ", elija una categoría:", los productos No se muestran ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image31.png))


Si en su lugar se mostraría *todos los* de los productos cuando se selecciona la opción "--Elija una categoría:", utilice un valor de `-1` en su lugar. El lector astuto recordará esa copia en el *filtrado de maestro y detalles con un DropDownList* tutorial se han actualizado los `ProductsBLL` la clase `GetProductsByCategoryID(categoryID)` método para que si un *`categoryID`* valor de `-1` se pasó en todos los productos que se han devuelto registros.

## <a name="summary"></a>Resumen

Al mostrar datos relacionados con jerárquicamente, suele ayudar a presentar los datos de uso de informes de maestro y detalles, desde el que el usuario puede iniciar examina los datos de la parte superior de la jerarquía y profundizar en los detalles. En este tutorial se examina la creación de un informe de principal-detalle simple que muestra los productos de la categoría seleccionada. Esto se logra usando DropDownList para la lista de categorías y un control DataList para los productos que pertenecen a la categoría seleccionada.

En el siguiente tutorial se examinará separar los registros maestro y detalles a través de dos páginas. En la primera página, se mostrará una lista de registros "principales", con un vínculo para ver los detalles. Al hacer clic en el vínculo se captar el usuario a la segunda página, que mostrará los detalles para el registro maestro seleccionado.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a...

Esta serie de tutoriales se revisó por varios revisores útiles. Revisor inicial para este tutorial era Randy Schmidt. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Siguiente](master-detail-filtering-acess-two-pages-datalist-cs.md)
