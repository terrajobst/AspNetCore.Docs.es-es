---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-cs
title: "Paginación de datos de informe en un Control de repetidor (C#) o de DataList | Documentos de Microsoft"
author: rick-anderson
description: "Mientras el DataList ni repetidor paginación automática de oferta o compatibilidad con la ordenación, este tutorial muestra cómo agregar compatibilidad con la paginación al DataList o repetidor..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2006
ms.topic: article
ms.assetid: e8e0809b-25c4-4c3b-8d12-9a17048148ae
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 4952adff752ec834b8be5f190181be98a034ccfd
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="paging-report-data-in-a-datalist-or-repeater-control-c"></a>Datos de informe de paginación en un DataList o un Control de repetidor (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_44_CS.exe) o [descarga de PDF](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/datatutorial44cs1.pdf)

> Mientras el DataList ni repetidor oferta automática paginación o la ordenación de soporte técnico, este tutorial muestra cómo agregar compatibilidad con la paginación al DataList o repetidor, que permite mostrar interfaces mucho más flexible paginación y los datos.


## <a name="introduction"></a>Introducción

Paginación y la ordenación son dos características muy comunes para mostrar datos en una aplicación en línea. Por ejemplo, al buscar libros ASP.NET en una librería en línea, puede haber cientos de estos libros, pero el informe de los resultados de búsqueda enumera a solo diez coincidencias por página. Además, los resultados se pueden ordenar por título, precio, recuento de páginas, nombre del autor y así sucesivamente. Como se explicó en la [paginación y ordenar datos de informe](../paging-and-sorting/paging-and-sorting-report-data-cs.md) tutorial, los controles GridView, DetailsView y FormView todo proporciona compatibilidad integrada de paginación que se pueden habilitar en la marca de verificación de un control checkbox. GridView también incluye compatibilidad para ordenar.

Por desgracia, ni el DataList ni repetidor ofrecen paginación o admitir la ordenación automática. En este tutorial, examinaremos cómo agregar compatibilidad con la paginación al DataList o repetidor. Manualmente debemos crear la interfaz de paginación, mostrar la página apropiada de registros y recuerde la página que se está visitada a través de las devoluciones de datos. Si bien esto toma más tiempo y el código que con GridView, DetailsView o FormView, el DataList y repetidor permiten mucho más flexible paginación y datos mostrar interfaces.

> [!NOTE]
> Este tutorial se centra exclusivamente en la paginación. En el siguiente tutorial centraremos nuestra atención en Agregar capacidades de ordenación.


## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>Paso 1: Agregar la paginación y la ordenación de páginas Web del Tutorial

Antes de empezar este tutorial, permiten s primero Tómese un momento para agregar las páginas ASP.NET que se necesitará para este tutorial y siguiente. Empiece por crear una nueva carpeta en el proyecto denominado `PagingSortingDataListRepeater`. A continuación, agregue las siguientes páginas ASP.NET cinco a esta carpeta, todas ellas configurada para usar la página maestra con `Site.master`:

- `Default.aspx`
- `Paging.aspx`
- `Sorting.aspx`
- `SortingWithDefaultPaging.aspx`
- `SortingWithCustomPaging.aspx`


![Cree una carpeta PagingSortingDataListRepeater y agregar las páginas ASP.NET Tutorial](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image1.png)

**Figura 1**: crear un `PagingSortingDataListRepeater` carpeta y agregar las páginas ASP.NET Tutorial


A continuación, abra el `Default.aspx` página y arrastre el `SectionLevelTutorialListing.ascx` Control de usuario desde el `UserControls` carpeta a la superficie de diseño. Este Control de usuario, que hemos creado en el [páginas maestras y navegación del sitio](../introduction/master-pages-and-site-navigation-cs.md) tutorial, enumera el mapa del sitio y los tutoriales se muestran en la sección actual en una lista con viñetas.


[![Agregar el Control de usuario SectionLevelTutorialListing.ascx a Default.aspx](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image3.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image2.png)

**Figura 2**: agregar la `SectionLevelTutorialListing.ascx` Control de usuario a `Default.aspx` ([haga clic aquí para ver la imagen a tamaño completo](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image4.png))


Para hacer que la lista con viñetas muestre la paginación y la ordenación tutoriales que se va a crear, es necesario agregarlos a la asignación de sitio. Abra la `Web.sitemap` de archivos y agregue el siguiente marcado después de la edición y eliminación con el marcado de nodo DataList asignación de sitio:


[!code-xml[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample1.xml)]


![Actualizar el mapa del sitio para incluir las nuevas páginas ASP.NET](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image5.png)

**Figura 3**: actualizar el mapa del sitio para incluir las nuevas páginas ASP.NET


## <a name="a-review-of-paging"></a>Una revisión de paginación

En los tutoriales anteriores hemos visto cómo desplazarse a través de los datos en los controles GridView, DetailsView y FormView. Estos tres controles ofrecen una forma sencilla de paginación llama *paginación predeterminada* que se puede implementar simplemente comprobando la opción de habilitar la paginación en la etiqueta inteligente de control s. Con la paginación de manera predeterminada, cada vez que se solicita una página de datos en la primera página visite o cuando el usuario navega a otra página de datos del control GridView, DetailsView, o volver a solicitudes de control de FormView *todos los* de los datos de la ObjectDataSource. A continuación, recortes del conjunto determinado de registros que deben mostrarse según el índice de la página solicitada y el número de registros mostrados por página. Analizamos la paginación predeterminada con detalle en la [paginación y ordenar datos de informe](../paging-and-sorting/paging-and-sorting-report-data-cs.md) tutorial.

Puesto que la paginación predeterminada solicita volver a todos los registros para cada página, no es práctico, al paginar a través de suficientemente grandes cantidades de datos. Imagine, por ejemplo, la paginación a través de 50.000 registros con un tamaño de página de 10. Cada vez que el usuario se mueve a una nueva página, 50.000 todos los registros se deben recuperar de la base de datos, aunque diez solo de ellos se muestra.

*Paginación personalizada* permite solucionar los problemas de rendimiento de paginación de manera predeterminada, arrastrar sólo el subconjunto preciso de registros que deben mostrarse en la página solicitada. Al implementar la paginación personalizada, debemos escribimos la consulta SQL que eficazmente devolverá solo el conjunto de registros correcto. Hemos visto cómo crear una consulta con SQL Server 2005 s nuevo [ `ROW_NUMBER()` palabra clave](http://www.4guysfromrolla.com/webtech/010406-1.shtml) en el [eficazmente paginar a través de grandes cantidades de datos](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md) tutorial.

Para implementar la paginación de manera predeterminada en los controles DataList o repetidor, podemos usar la [ `PagedDataSource` clase](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.aspx) como un contenedor alrededor de la `ProductsDataTable` cuyo contenido se que se va a paginar. El `PagedDataSource` clase tiene un `DataSource` propiedad que se pueden asignar a cualquier objeto enumerable y [ `PageSize` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.pagesize.aspx) y [ `CurrentPageIndex` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.currentpageindex.aspx) propiedades que indican el número de registros a Mostrar por página y el índice de la página actual. Una vez que se hayan establecido estas propiedades, el `PagedDataSource` puede utilizarse como origen de datos de cualquier control Web de datos. El `PagedDataSource`, al tipo enumerado le devuelve solo el subconjunto de registros de su interior adecuado `DataSource` tomando como base la `PageSize` y `CurrentPageIndex` propiedades. La figura 4 representa la funcionalidad de la `PagedDataSource` clase.


![El PagedDataSource contiene un objeto Enumerable con una interfaz paginable](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image6.png)

**Figura 4**: la `PagedDataSource` contiene un objeto Enumerable con una interfaz paginable


La `PagedDataSource` objeto puede ser creado y configurado directamente desde la capa de lógica de negocios y enlazado a un DataList o repetidor a través de un ObjectDataSource, o puede ser creados y configurados directamente en la clase de código subyacente ASP.NET page s. Si se utiliza el último enfoque, debemos renunciar a través de ObjectDataSource y en su lugar, enlazar datos paginados al DataList o repetidor mediante programación.

La `PagedDataSource` objeto también tiene propiedades que admiten la paginación personalizada. Sin embargo, se puede omitir con un `PagedDataSource` para la paginación personalizada porque ya tenemos métodos BLL en la `ProductsBLL` clase diseñada para la paginación personalizada que devuelven los registros precisos que se va a mostrar.

En este tutorial se examinará implementar la paginación de manera predeterminada en un control DataList agregando un nuevo método para el `ProductsBLL` clase que devuelve configurado adecuadamente `PagedDataSource` objeto. En el siguiente tutorial, veremos cómo utilizar la paginación personalizada.

## <a name="step-2-adding-a-default-paging-method-in-the-business-logic-layer"></a>Paso 2: Agregar un método de paginación de manera predeterminada en la capa de lógica de negocios

El `ProductsBLL` clase actualmente tiene un método para devolver toda la información de producto `GetProducts()` y otro para devolver un subconjunto determinado de productos en un índice de inicio `GetProductsPaged(startRowIndex, maximumRows)`. Con la paginación de manera predeterminada, el control GridView, DetailsView y FormView controla todo, use la `GetProducts()` método para recuperar todos los productos, pero, a continuación, use un `PagedDataSource` internamente para mostrar solo el subconjunto correcto de registros. Para replicar esta funcionalidad con los controles DataList y repetidor, podemos crear un nuevo método en el BLL que imita este comportamiento.

Agregue un método a la `ProductsBLL` clase denominada `GetProductsAsPagedDataSource` que toma dos parámetros de entrada de enteros:

- `pageIndex`el índice de la página para mostrar, se indizan a partir de cero y
- `pageSize`el número de registros mostrados por página.

`GetProductsAsPagedDataSource`se inicia mediante la recuperación de *todos los* los registros de `GetProducts()`. A continuación, se crea un `PagedDataSource` objeto, estableciendo su `CurrentPageIndex` y `PageSize` propiedades en los valores del pasado `pageIndex` y `pageSize` parámetros. El método concluye devolviendo esto configurado `PagedDataSource`:


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample2.cs)]

## <a name="step-3-displaying-product-information-in-a-datalist-using-default-paging"></a>Paso 3: Mostrar información de producto en un control DataList con paginación predeterminada

Con el `GetProductsAsPagedDataSource` método agregado a la `ProductsBLL` (clase), ahora puede crear un DataList o repetidor que proporciona la paginación de manera predeterminada. Comience abriendo la `Paging.aspx` página en el `PagingSortingDataListRepeater` carpeta y arrastre un control DataList desde el cuadro de herramientas hasta el diseñador, establecer el control DataList s `ID` propiedad `ProductsDefaultPaging`. Desde la etiqueta inteligente de DataList s, crear un nuevo origen ObjectDataSource denominado `ProductsDefaultPagingDataSource` y configúrelo para que recupera datos mediante el `GetProductsAsPagedDataSource` método.


[![Crear un origen ObjectDataSource y configúrelo para utilizar el GetProductsAsPagedDataSource (...) (método)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image8.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image7.png)

**Figura 5**: cree un ObjectDataSource y configúrelo para utilizar el `GetProductsAsPagedDataSource` `()` método ([haga clic aquí para ver la imagen a tamaño completo](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image9.png))


Establece las listas de lista desplegable en la actualización, INSERCIÓN y eliminar las fichas (ninguno).


[![Establecer la lista desplegable que se enumeran en la actualización, INSERCIÓN y eliminar las fichas (ninguno)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image11.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image10.png)

**Figura 6**: establecer la lista desplegable que se enumeran en la actualización, INSERCIÓN y eliminar las fichas (ninguno) ([haga clic aquí para ver la imagen a tamaño completo](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image12.png))


Puesto que la `GetProductsAsPagedDataSource` método espera dos parámetros de entrada, el asistente le pide a nosotros para el origen de estos valores de parámetro.

El índice de la página y los valores de tamaño de página se deben recordar a través de las devoluciones de datos. Pueden almacenarse en el estado de vista, guardado en la cadena de consulta, almacenan en variables de sesión o recuerdan utilizando prefiere otra técnica. Para este tutorial vamos a usar la cadena de consulta, que tiene la ventaja de permitir que una página determinada de datos que se marcaron.

En concreto, utilice el querystring campos pageIndex y pageSize para la `pageIndex` y `pageSize` parámetros, respectivamente (consulte la figura 7). Tómese un momento para establecer los valores predeterminados para estos parámetros, como los valores de cadena de consulta won t estar presente cuando un usuario visita por primera vez esta página. Para `pageIndex`, establecer el valor predeterminado en 0 (lo que se mostrará la primera página de datos) y `pageSize` valor predeterminado de s a 4.


[![Utilizar la cadena de consulta como origen para los parámetros de pageIndex y pageSize](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image14.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image13.png)

**Figura 7**: utilizar la cadena de consulta como el origen para el `pageIndex` y `pageSize` parámetros ([haga clic aquí para ver la imagen a tamaño completo](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image15.png))


Después de configurar el ObjectDataSource, Visual Studio crea automáticamente un `ItemTemplate` para el control DataList. Personalizar el `ItemTemplate` para que se muestren solo el nombre de producto s, categoría y el proveedor. Establecer el control DataList s `RepeatColumns` propiedad a 2, su `Width` al 100% y su `ItemStyle` s `Width` al 50%. Esta configuración de ancho proporcionará igual espaciado de las dos columnas.

Después de realizar estos cambios, el marcado de s DataList y ObjectDataSource debe ser similar al siguiente:


[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample3.aspx)]

> [!NOTE]
> Puesto que no se está realizando una actualización o funcionalidad de eliminación en este tutorial, puede deshabilitar el estado de vista de DataList s para reducir el tamaño de la página representada.


Cuando inicialmente visitar esta página a través de un explorador, ni el `pageIndex` ni `pageSize` se proporcionan parámetros de cadena de consulta. Por lo tanto, se usan los valores predeterminados de 0 y 4. Como se muestra en la figura 8, esto da como resultado en un control DataList que muestra los cuatro primeros productos.


[![Se muestran los primeros cuatro productos](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image17.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image16.png)

**Figura 8**: se muestran los primeros cuatro productos ([haga clic aquí para ver la imagen a tamaño completo](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image18.png))


Sin una interfaz de paginación, allí s sencillo actualmente no significa que un usuario navegue a la segunda página de datos. Vamos a crear una interfaz de paginación en el paso 4. Por ahora, sin embargo, la paginación solo pueden realizarse especificando los criterios de paginación directamente en la cadena de consulta. Por ejemplo, para ver la segunda página, cambie la dirección URL en la barra de direcciones del explorador s de `Paging.aspx` a `Paging.aspx?pageIndex=2` y presione ENTRAR. Esto hace que la segunda página de datos que se mostrará (consulte la figura 9).


[![Se muestra la segunda página de datos](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image20.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image19.png)

**Figura 9**: se muestra la página de datos ([haga clic aquí para ver la imagen a tamaño completo](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image21.png))


## <a name="step-4-creating-the-paging-interface"></a>Paso 4: Creación de la interfaz de paginación

Hay una variedad de distintas interfaces de paginación que se pueden implementar. Los controles GridView, DetailsView y FormView proporcionan cuatro interfaces diferentes para elegir entre:

- **A continuación, anterior** los usuarios pueden mover una página a la vez, con lo siguiente o anterior.
- **Siguiente, anterior, primero, último** además de los botones siguiente y anterior, esta interfaz incluye botones primero y último para desplazarse a la primera o última página.
- **Numérico** muestra los números de página en la interfaz de paginación, lo que permite a un usuario ir directamente a una página determinada.
- **Numérico, en primer lugar, último** además de los números de página numérica, incluye botones para ir a la primera o última página.

Para obtener el DataList y repetidor, somos responsables de decidir en una interfaz de paginación e implementarlo. Esto implica la creación de los controles Web necesarios en la página y mostrar la página solicitada cuando se hace clic en un botón de interfaz concreto de paginación. Además, ciertos controles de interfaz de paginación pueden que va a deshabilitar. Por ejemplo, al ver la primera página de datos mediante la siguiente, anterior, en primer lugar, última interfaz, se deshabilitarán los botones de la primera y la anterior.

Para este tutorial, s permiten usar la siguiente, anterior, en primer lugar, última interfaz. Agregue cuatro controles de botón Web a la página y establezca sus `ID` s a `FirstPage`, `PrevPage`, `NextPage`, y `LastPage`. Establecer el `Text` propiedades para &lt; &lt; en primer lugar, &lt; Prev, siguiente &gt;y el último &gt; &gt; .


[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample4.aspx)]

A continuación, cree un `Click` controlador de eventos para cada uno de estos botones. En un momento en que se agregará el código necesario para mostrar la página solicitada.

## <a name="remembering-the-total-number-of-records-being-paged-through"></a>Teniendo en cuenta el número Total de registros que se va a paginar a través de

Independientemente de la interfaz de paginación seleccionada, es necesario calcular y recordar el número total de registros que se va a paginar a través de. El recuento de filas total (en combinación con el tamaño de página) determina el número total de páginas de datos que se va a se paginan a través, que determina lo que se agregan o se habilitan los controles de interfaz de paginación. En el siguiente, anterior, primero, último interfaz que estamos creando, el recuento de páginas se utiliza de dos maneras:

- Para determinar si se está viendo es la última página, en cuyo caso se deshabilitan los botones siguiente y último.
- Si el usuario hace clic en el último botón que necesitamos captar ellos hasta el último número de página, cuyo índice es una menor que la página.

El recuento de páginas se calcula como el valor ceiling del recuento de filas total dividido por el tamaño de página. Por ejemplo, si se estamos paginar a través de 79 registros con cuatro registros por página, el recuento de páginas es 20 (el límite superior de 79 / 4). Si se está usando la interfaz de paginación numérico, esta información indica en cuanto a cuántos botones numéricos de página para mostrar; Si la interfaz de paginación incluye botones siguiente o último, el recuento de páginas se utiliza para determinar cuándo se debe deshabilitar los botones siguiente o último.

Si la interfaz de paginación incluye un botón última, es imperativo que el número total de registros que se va a paginar a través de se recuerdan a través de las devoluciones de datos para que cuando se hace clic en el último botón podemos determinar el último índice de la página. Para facilitar esto, cree un `TotalRowCount` propiedad de la clase de código subyacente ASP.NET página s que se conserva su valor para el estado de vista:


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample5.cs)]

Además `TotalRowCount`, tómese un minuto para crear propiedades de nivel de página de solo lectura para acceder fácilmente al índice de la página, el tamaño de página y recuento de páginas:


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample6.cs)]

## <a name="determining-the-total-number-of-records-being-paged-through"></a>Determinar el número Total de registros que se va a paginar a través de

El `PagedDataSource` devolvió un objeto de las operaciones de asignación ObjectDataSource `Select()` método tiene dentro de él *todos los* de los registros de productos, aunque solo un subconjunto de ellos se muestran en el control DataList. El `PagedDataSource` s [ `Count` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.count.aspx) devuelve solo el número de elementos que se mostrará en el control DataList; la [ `DataSourceCount` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.datasourcecount.aspx) devuelve el número total de elementos dentro de la `PagedDataSource`. Por lo tanto, es necesario asignar la página s ASP.NET `TotalRowCount` el valor de la propiedad de la `PagedDataSource` s `DataSourceCount` propiedad.

Para ello, cree un controlador de eventos para el s ObjectDataSource `Selected` eventos. En el `Selected` controlador de eventos que se tiene acceso al valor devuelto de las operaciones de asignación ObjectDataSource `Select()` método en este caso, el `PagedDataSource`.


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample7.cs)]

## <a name="displaying-the-requested-page-of-data"></a>Mostrar la página solicitada de datos

Cuando el usuario hace clic en uno de los botones en la interfaz de paginación, es necesario mostrar la página solicitada de datos. Puesto que se especifican los parámetros de paginación a través de la cadena de consulta para mostrar la página solicitada de datos use `Response.Redirect(url)` para que el usuario s explorador volver a solicitar la `Paging.aspx` página con los parámetros de paginación adecuados. Por ejemplo, para mostrar la segunda página de datos, se podría redirigir al usuario `Paging.aspx?pageIndex=1`.

Para facilitar esto, cree un `RedirectUser(sendUserToPageIndex)` método que redirige al usuario a `Paging.aspx?pageIndex=sendUserToPageIndex`. A continuación, llamar a este método desde el botón cuatro `Click` controladores de eventos. En el `FirstPage` `Click` controlador de eventos, llamada `RedirectUser(0)`, enviarlos a la primera página; en el `PrevPage` `Click` controlador de eventos, use `PageIndex - 1` como el índice de la página; y así sucesivamente.


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample8.cs)]

Con la `Click` completar controladores de eventos, los registros de DataList s se pueden enviar mensajes a través de, haga clic en los botones. ¡Tómese un momento para probarlo!

## <a name="disabling-paging-interface-controls"></a>Desactivación de la búsqueda de controles de interfaz

Actualmente, los cuatro botones están habilitados, independientemente de la página que se está visualiza. Sin embargo, debe deshabilitar los botones primero y anterior al mostrar la primera página de datos y los botones siguiente y último al mostrar la última página. El `PagedDataSource` objeto devuelto por la s ObjectDataSource `Select()` método tiene propiedades [ `IsFirstPage` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.isfirstpage.aspx) y [ `IsLastPage` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.islastpage.aspx) que podemos examinar para determinar si se está viendo la primera o última página de datos.

Agregue lo siguiente a las operaciones de asignación ObjectDataSource `Selected` controlador de eventos:


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample9.cs)]

Con esta adición, se deshabilitarán los botones primero y anterior al ver la primera página, mientras que los botones siguiente y último se deshabilitará al ver la última página.

S permiten completar la interfaz de paginación por que informa al usuario qué página re viendo actualmente y existe el número total de páginas. Agregue un control Web Label a la página y establezca su `ID` propiedad `CurrentPageNumber`. Establecer su `Text` propiedad ObjectDataSource s seleccionados controlador de eventos tal que incluye la página actual que se está viendo (`PageIndex + 1`) y el número total de páginas (`PageCount`).


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample10.cs)]

La figura 10 muestra `Paging.aspx` cuando visita por primera vez. Puesto que la cadena de consulta está vacía, el control DataList usa de forma predeterminada que muestra los cuatro primeros productos; los botones primero y anterior están deshabilitados. Haga clic en siguiente muestra los siguientes cuatro registros (consulte la figura 11); Ahora se habilitan los botones primero y anterior.


[![Se muestra la primera página de datos](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image23.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image22.png)

**Figura 10**: la primera página de los datos se muestran ([haga clic aquí para ver la imagen a tamaño completo](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image24.png))


[![Se muestra la segunda página de datos](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image26.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image25.png)

**Figura 11**: se muestra la página de datos ([haga clic aquí para ver la imagen a tamaño completo](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image27.png))


> [!NOTE]
> Puede mejorar aún más la interfaz de paginación al permitir al usuario especificar el número de páginas para ver por página. Por ejemplo, DropDownList podría agregarse opciones de tamaño de página de lista como 5, 10, 25, 50 y todos. Al seleccionar un tamaño de página, el usuario tendría que ser redirigido a `Paging.aspx?pageIndex=0&pageSize=selectedPageSize`. Se sale de la implementación de esta mejora como un ejercicio para el lector.


## <a name="using-custom-paging"></a>Use la paginación personalizada

Las páginas de DataList a través de sus datos mediante la técnica de paginación predeterminada ineficaz. Al paginar a través de suficientemente grandes cantidades de datos, es imprescindible que se utiliza la paginación personalizada. Aunque los detalles de implementación varían ligeramente, los conceptos de implementación de la paginación personalizada en un control DataList son el mismo que con la paginación de manera predeterminada. Con la paginación personalizada, use la `ProductBLL` clase s `GetProductsPaged` (método) (en lugar de `GetProductsAsPagedDataSource`). Como se describe en el [eficazmente paginar a través de grandes cantidades de datos](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md) tutorial, `GetProductsPaged` debe pasarse el inicio fila índice y el número máximo de filas que se va a devolver. Estos parámetros se pueden mantener a través de la cadena de consulta, al igual que el `pageIndex` y `pageSize` parámetros que se usan de forma predeterminada en la paginación.

Desde ahí s no `PagedDataSource` con la paginación personalizada, deben utilizarse técnicas alternativas para determinar el número total de registros que se va a paginar a través y si se re mostrar la primera o última página de datos. El `TotalNumberOfProducts()` método en la `ProductsBLL` clase devuelve el número total de productos que se va a paginar a través de. Para determinar si está viendo la primera página de datos, examine el índice de fila inicial si es cero, a continuación, se está viendo la primera página. Se está viendo la última página si el índice de fila inicial más el número máximo de filas a devolver es mayor o igual que el número total de registros que se va a paginar a través de.

Exploraremos implementar la paginación personalizada con más detalle en el siguiente tutorial.

## <a name="summary"></a>Resumen

Mientras el DataList ni repetidor ofrece fuera de la compatibilidad con la paginación de cuadro que se encuentra en el control GridView, DetailsView y controla FormView, esta funcionalidad se puede agregar con el mínimo esfuerzo. La manera más fácil de implementar la paginación predeterminada consiste en encapsular todo el conjunto de productos de un `PagedDataSource` y, a continuación, enlazar la `PagedDataSource` al DataList o repetidor. En este tutorial agregamos la `GetProductsAsPagedDataSource` método a la `ProductsBLL` clase para devolver el `PagedDataSource`. El `ProductsBLL` clase ya contiene los métodos necesarios para la paginación personalizada `GetProductsPaged` y `TotalNumberOfProducts`.

Además de recuperar el conjunto de registros que se va a mostrar para la paginación personalizada, o bien, de todos los registros de una `PagedDataSource` para la paginación de manera predeterminada, también es necesario agregar manualmente la interfaz de paginación. Para este tutorial, creamos un siguiente, anterior, en primer lugar, la última interfaz con cuatro controles de botón Web. Además, se agrega un control de etiqueta muestra el número de página actual y el número total de páginas.

En el siguiente tutorial veremos cómo agregar compatibilidad con la ordenación al DataList y repetidor. También veremos cómo crear a un control DataList que puede paginar y ordenar (con ejemplos de uso predeterminada y la paginación personalizada).

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Revisores para este tutorial fueron Liz Shulok, Ken Pespisa y Bernadette Leigh. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Siguiente](sorting-data-in-a-datalist-or-repeater-control-cs.md)
