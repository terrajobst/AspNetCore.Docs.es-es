---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-cs
title: Patrón de filtrado y detalles con DropDownList (C#) | Microsoft Docs
author: rick-anderson
description: En este tutorial veremos cómo mostrar informes de maestro y detalles en una única página web con listas desplegables para mostrar los registros de 'master' y un control DataList para mostrar...
ms.author: aspnetcontent
ms.date: 07/18/2007
ms.assetid: 07fa47ae-e491-4a2f-b265-d342b9ddef46
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: ed2631e49786c81075099cca6941d98ba3b67e37
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818257"
---
<a name="masterdetail-filtering-with-a-dropdownlist-c"></a>Patrón de filtrado y detalles con DropDownList (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_33_CS.exe) o [descargar PDF](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/datatutorial33cs1.pdf)

> En este tutorial veremos cómo mostrar informes de maestro y detalles en una única página web con listas desplegables para mostrar los registros de "maestros" y un control DataList para mostrar "Detalles".


## <a name="introduction"></a>Introducción

El informe de maestro y detalles, que se crea por primera vez mediante un GridView anteriormente [filtrado de maestro y detalles con un DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) comienzo del tutorial, mostrando un conjunto de registros "maestros". El usuario puede, a continuación, profundizar en uno de los registros maestros, ver, por tanto, "Detalles" de ese registro maestro. Los informes de maestro y detalles son una opción ideal para visualizar las relaciones uno a varios y para mostrar información detallada de especialmente las tablas "anchas" (las que tiene una gran cantidad de columnas). Hemos analizado cómo implementar los informes de maestro y detalles con los controles GridView y DetailsView en los tutoriales anteriores. En este tutorial y los dos siguientes, se le vuelva a examinar estos conceptos, pero el enfoque sobre el uso de controles DataList y Repeater se controla en su lugar.

En este tutorial, buscaremos usando DropDownList para contener los registros de "master", con los registros de "Detalles" mostrados en un control DataList.

## <a name="step-1-adding-the-masterdetail-tutorial-web-pages"></a>Paso 1: Agregar las páginas Web del Tutorial de maestro y detalles

Antes de empezar este tutorial, primero dedique un momento para agregar la carpeta y las páginas ASP.NET que necesitaremos para este tutorial y los dos siguientes trabajar con informes de maestro y detalles mediante los controles DataList y Repeater. Empiece por crear una nueva carpeta en el proyecto denominado `DataListRepeaterFiltering`. A continuación, agregue las siguientes cinco páginas ASP.NET en esta carpeta, configurado para usar la página principal de mantenerlos todos `Site.master`:

- `Default.aspx`
- `FilterByDropDownList.aspx`
- `CategoryListMaster.aspx`
- `ProductsForCategoryDetails.aspx`
- `CategoriesAndProducts.aspx`


![Cree una carpeta DataListRepeaterFiltering y agregar las páginas del Tutorial de ASP.NET](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image1.png)

**Figura 1**: crear un `DataListRepeaterFiltering` carpeta y agregue las páginas del Tutorial de ASP.NET


A continuación, abra el `Default.aspx` página y arrastre el `SectionLevelTutorialListing.ascx` Control de usuario desde el `UserControls` carpeta a la superficie de diseño. Este Control de usuario, que hemos creado en el [páginas maestras y navegación del sitio](../introduction/master-pages-and-site-navigation-cs.md) tutorial, enumera el mapa del sitio y los tutoriales se muestra en la sección actual en una lista con viñetas.


[![Agregar el Control de usuario SectionLevelTutorialListing.ascx a Default.aspx](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image3.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image2.png)

**Figura 2**: agregar el `SectionLevelTutorialListing.ascx` Control de usuario `Default.aspx` ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image4.png))


Para tener la visualización de la lista con viñetas los tutoriales de principal-detalle que crearemos, es necesario agregarlos al mapa del sitio. Abra el `Web.sitemap` archivo y agregue el siguiente marcado después de las marcas de nodo "Mostrar datos con los controles DataList y Repeater" asignación de sitio:

[!code-xml[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample1.xml)]


![Actualizar el mapa del sitio para incluir las nuevas páginas de ASP.NET](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image5.png)

**Figura 3**: actualizar el mapa del sitio para incluir las nuevas páginas de ASP.NET


## <a name="step-2-displaying-the-categories-in-a-dropdownlist"></a>Paso 2: Mostrar las categorías en DropDownList

Nuestro informe de maestro y detalles mostrará las categorías de DropDownList, con los productos del elemento de lista seleccionado aparece más abajo en la página en un control DataList. La primera tarea antes que nosotros, a continuación, es que las categorías mostradas en DropDownList. Comience abriendo la `FilterByDropDownList.aspx` página en el `DataListRepeaterFiltering` carpeta y arrastre un DropDownList del cuadro de herramientas al diseñador de la página. A continuación, establezca la DropDownList `ID` propiedad `Categories`. Haga clic en el vínculo Elegir origen de datos de etiqueta inteligente de DropDownList y cree un nuevo origen ObjectDataSource denominado `CategoriesDataSource`.


[![Agregar un nuevo origen ObjectDataSource denominado CategoriesDataSource](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image7.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image6.png)

**Figura 4**: agregar un nuevo origen ObjectDataSource denominado `CategoriesDataSource` ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image8.png))


Configure el nuevo origen ObjectDataSource de manera que invoca el `CategoriesBLL` la clase `GetCategories()` método. Después de configurar ObjectDataSource necesitamos especificar qué campo del origen de datos se debe mostrar en DropDownList y que uno se debe asociar como el valor de cada elemento de lista. Tiene la `CategoryName` campo como la presentación y `CategoryID` como el valor de cada elemento de lista.


[![Tiene la presentación de DropDownList el campo CategoryName y Use CategoryID como valor](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image10.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image9.png)

**Figura 5**: tiene la presentación de DropDownList el `CategoryName` campo y Use `CategoryID` como el valor ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image11.png))


En este momento, tenemos un control DropDownList que se rellena con los registros de la `Categories` tabla (todo lleva a cabo de seis segundos aproximadamente). Figura 6 muestra nuestro progreso hasta ahora, cuando se ve mediante un explorador.


[![Una lista desplegable enumera las categorías actuales](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image13.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image12.png)

**Figura 6**: un cuadro desplegable enumera las categorías actuales ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image14.png))


## <a name="step-2-adding-the-products-datalist"></a>Paso 2: Agregar al control DataList de productos

El último paso de nuestro informe de maestro y detalles es enumerar los productos asociados con la categoría seleccionada. Para ello, agregue un control DataList a la página y crear un nuevo origen ObjectDataSource denominado `ProductsByCategoryDataSource`. Tiene la `ProductsByCategoryDataSource` control recuperar sus datos desde el `ProductsBLL` la clase `GetProductsByCategoryID(categoryID)` método. Puesto que este informe de maestro y detalles es de solo lectura, elija la que opción la (ninguno) en las fichas de INSERT, UPDATE y DELETE.


[![Seleccione el método GetProductsByCategoryID(categoryID)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image16.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image15.png)

**Figura 7**: seleccione el `GetProductsByCategoryID(categoryID)` método ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image17.png))


Después de hacer clic en siguiente, el ObjectDataSource wizard nos pide el origen del valor de la `GetProductsByCategoryID(categoryID)` del método *`categoryID`* parámetro. Para usar el valor del seleccionado `categories` DropDownList elemento establezca el origen de parámetro para el Control y la ControlID a `Categories`.


[![Establece el parámetro categoryID en el valor de DropDownList categorías](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image19.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image18.png)

**Figura 8**: establecer el *`categoryID`* parámetro en el valor de la `Categories` DropDownList ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image20.png))


Al finalizar el asistente Configurar origen de datos, Visual Studio generará automáticamente un `ItemTemplate` para el control DataList que muestra el nombre y valor de cada campo de datos. Vamos a mejorar el control DataList usar en su lugar un `ItemTemplate` que muestra sólo el nombre del producto, categoría, proveedor, cantidad por unidad y el precio junto con un `SeparatorTemplate` que inyecta un `<hr>` elemento entre cada elemento. Voy a usar el `ItemTemplate` desde un ejemplo en el [mostrar datos con los controles DataList y Repeater](../displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-cs.md) tutorial, pero puede usar cualquier marcado de plantilla que se encuentre más atractivo.

Después de realizar estos cambios, los controles DataList y marcado de su ObjectDataSource deben ser similares al siguiente:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample2.aspx)]

Dedique un momento para comprobar nuestro progreso en un explorador. Cuando en primer lugar, visite la página, se muestran los productos que pertenecen a la categoría seleccionada (bebidas) (como se muestra en la figura 9), pero cambiar DropDownList no actualiza los datos. Esto es porque se debe producir una devolución de datos para que el control DataList actualizar. Para lograr esto se puede establece la DropDownList `AutoPostBack` propiedad `true` o agregar un control de botón Web a la página. Para este tutorial, he optado por establecer la DropDownList `AutoPostBack` propiedad `true`.

Las figuras 9 y 10 se muestra el informe de maestro y detalles en acción.


[![Cuando se visita primero la página, se muestran los productos de bebidas](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image22.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image21.png)

**Figura 9**: cuando se visita primero la página, se muestran los productos de bebidas ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image23.png))


[![Seleccionar automáticamente un nuevo producto (generar) produce un PostBack, actualizando el control DataList](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image25.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image24.png)

**Figura 10**: seleccionar automáticamente un nuevo producto (generar) produce un PostBack, actualizando el control DataList ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image26.png))


## <a name="adding-a----choose-a-category----list-item"></a>Agregar un elemento de lista "--elegir una categoría:"

Cuando se visita primero la `FilterByDropDownList.aspx` página las categorías de primer elemento de lista del DropDownList (bebidas) está seleccionada de forma predeterminada, que muestra los productos de bebidas en el control DataList. En el *filtrado de maestro y detalles con un DropDownList* tutorial hemos agregado una opción "--elegir una categoría:" al control DropDownList que estaba seleccionada de forma predeterminada y, cuando se selecciona, muestra *todas* de la productos en la base de datos. Este enfoque era fácil de administrar al enumerar los productos en un control GridView, como cada fila del producto ocupaba una pequeña cantidad de espacio en pantalla. Sin embargo, con el control DataList, información de cada producto consume un fragmento mucho mayor de la pantalla. Podemos agregar una opción "--elegir una categoría:" y encuentra activada de forma predeterminada, pero en lugar de tener mostrar todos los productos cuando se selecciona, vamos a configurarlo para que no muestre ningún producto.

Para agregar un nuevo elemento de lista al control DropDownList, vaya a la ventana Propiedades y haga clic en el botón de puntos suspensivos en el `Items` propiedad. Agregar un nuevo elemento de lista con el `Text` "--elegir una categoría:" y el `Value` `0`.


![Agregar un](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image27.png)

**Figura 11**: agregar un elemento de lista "--elegir una categoría:"


Como alternativa, puede agregar el elemento de lista agregando el siguiente marcado al control DropDownList:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-cs/samples/sample3.aspx)]

Además, se debe establecer el control DropDownList `AppendDataBoundItems` a `true` porque si se establece en `false` (valor predeterminado), cuando las categorías se enlazan al control DropDownList de ObjectDataSource sobrescribirá cualquier lista agregado manualmente elementos.


![Establezca la propiedad AppendDataBoundItems en True](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image28.png)

**Figura 12**: establecer el `AppendDataBoundItems` propiedad en True


La razón por la que elegimos el valor `0` para obtener la lista "--elegir una categoría:" elemento es porque no hay ninguna categoría en el sistema con un valor de `0`, por lo tanto, no se devolverá ningún registro del producto cuando se selecciona el elemento de lista "--elegir una categoría--". Para confirmarlo, dedique un momento para visitar la página a través de un explorador. Como se muestra en la figura 13, al ver la página se selecciona el elemento de lista "--elegir una categoría:" inicialmente y no hay productos se muestran.


[![Cuando el](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image30.png)](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image29.png)

**Figura 13**: cuando se selecciona el elemento de lista "--elegir una categoría:", los productos No se muestran ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-with-a-dropdownlist-datalist-cs/_static/image31.png))


Si en su lugar mostraría *todas* de los productos cuando se selecciona la opción "--elegir una categoría:", use un valor de `-1` en su lugar. El lector astuto recordará esa copia en el *filtrado de maestro y detalles con un DropDownList* tutorial actualizamos el `ProductsBLL` la clase `GetProductsByCategoryID(categoryID)` método para que si un *`categoryID`* valor de `-1` se pasó en todos los productos que se devolvieron los registros.

## <a name="summary"></a>Resumen

Cuando muestra datos relacionados de forma jerárquica, a menudo ayuda a presentar los datos mediante los informes de maestro/detalle, desde el que el usuario pueda empezar a examinar los datos de la parte superior de la jerarquía y explorar en profundidad los detalles. En este tutorial se examina la creación de un informe sencillo de maestro y detalles que muestre los productos de una categoría seleccionada. Esto se consigue usando DropDownList para la lista de categorías y un control DataList para los productos que pertenecen a la categoría seleccionada.

En el siguiente tutorial, buscaremos en separar los registros maestro y detalles en dos páginas. En la primera página, una lista de registros "maestros" se mostrará, con un vínculo para ver los detalles. Al hacer clic en el vínculo se captar el usuario a la segunda página, que mostrará los detalles para el registro maestro seleccionado.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a...

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Revisor de clientes potencial para este tutorial era Randy Schmidt. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Siguiente](master-detail-filtering-acess-two-pages-datalist-cs.md)
