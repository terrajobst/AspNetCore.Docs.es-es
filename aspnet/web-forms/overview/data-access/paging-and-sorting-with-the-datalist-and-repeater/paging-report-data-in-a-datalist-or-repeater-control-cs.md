---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-cs
title: Paginación de datos del informe en un Control DataList o Repeater (C#) | Microsoft Docs
author: rick-anderson
description: Mientras el control DataList ni Repeater oferta la paginación automática o admitir la ordenación, este tutorial muestra cómo agregar compatibilidad con la paginación para el control DataList o Repeater...
ms.author: aspnetcontent
ms.date: 11/13/2006
ms.assetid: e8e0809b-25c4-4c3b-8d12-9a17048148ae
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 90df9c096b5411201da35b7076fdd3cd9b1f86d1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37842324"
---
<a name="paging-report-data-in-a-datalist-or-repeater-control-c"></a>Paginar datos de informe en un Control DataList o Repeater (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_44_CS.exe) o [descargar PDF](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/datatutorial44cs1.pdf)

> Mientras el control DataList ni Repeater oferta automática paginación o la clasificación de soporte técnico, este tutorial muestra cómo agregar compatibilidad con la paginación al DataList o Repeater, lo que permite mostrar interfaces mucho más flexible datos y paginación.


## <a name="introduction"></a>Introducción

Paginación y clasificación son dos características muy comunes al mostrar datos en una aplicación en línea. Por ejemplo, cuando se buscan los libros en pantalla ASP.NET en una librería en línea, puede haber cientos de libros, pero el informe con una lista de los resultados de búsqueda muestra a solo diez coincidencias por página. Además, se pueden ordenar los resultados por título, precio, recuento de páginas, nombre del autor y así sucesivamente. Como se explicó en la [paginación y ordenación de datos de informe](../paging-and-sorting/paging-and-sorting-report-data-cs.md) tutorial, los controles GridView, DetailsView y FormView proporcionan compatibilidad integrada de paginación que se puede habilitar en el paso de un control checkbox. El control GridView también incluye compatibilidad para ordenar.

Por desgracia, ni el control DataList ni Repeater ofrecen paginación automática o admitir la ordenación. En este tutorial, examinaremos cómo agregar compatibilidad con la paginación para el control DataList o Repeater. Manualmente debemos crear la interfaz de paginación, mostrar la página apropiada de registros y recuerde que la página que se visita a través de las devoluciones de datos. Si bien esto toma más tiempo y el código que con el control GridView, DetailsView o FormView, los controles DataList y Repeater permiten paginación mucho más flexible y los datos mostrar interfaces.

> [!NOTE]
> En este tutorial se centra exclusivamente en la paginación. En el siguiente tutorial, centraremos nuestra atención a la adición de capacidades de ordenación.


## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>Paso 1: Agregar la paginación y ordenación páginas Web del Tutorial

Antes de empezar este tutorial, permiten s primero Tómese un momento para agregar las páginas ASP.NET que necesitaremos para este tutorial y la siguiente. Empiece por crear una nueva carpeta en el proyecto denominado `PagingSortingDataListRepeater`. A continuación, agregue las siguientes cinco páginas ASP.NET en esta carpeta, configurado para usar la página principal de mantenerlos todos `Site.master`:

- `Default.aspx`
- `Paging.aspx`
- `Sorting.aspx`
- `SortingWithDefaultPaging.aspx`
- `SortingWithCustomPaging.aspx`


![Cree una carpeta PagingSortingDataListRepeater y agregar las páginas del Tutorial de ASP.NET](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image1.png)

**Figura 1**: crear un `PagingSortingDataListRepeater` carpeta y agregue las páginas del Tutorial de ASP.NET


A continuación, abra el `Default.aspx` página y arrastre el `SectionLevelTutorialListing.ascx` Control de usuario desde el `UserControls` carpeta a la superficie de diseño. Este Control de usuario, que hemos creado en el [páginas maestras y navegación del sitio](../introduction/master-pages-and-site-navigation-cs.md) tutorial, enumera el mapa del sitio y los tutoriales se muestra en la sección actual en una lista con viñetas.


[![Agregar el Control de usuario SectionLevelTutorialListing.ascx a Default.aspx](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image3.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image2.png)

**Figura 2**: agregar el `SectionLevelTutorialListing.ascx` Control de usuario `Default.aspx` ([haga clic aquí para ver imagen en tamaño completo](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image4.png))


Con el fin de que la lista con viñetas muestre la paginación y ordenación tutoriales que crearemos, deberá agregarlos al mapa del sitio. Abra el `Web.sitemap` archivo y agregue el siguiente marcado después de la edición y eliminación con el marcado de nodo de mapa de sitio de DataList:


[!code-xml[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample1.xml)]


![Actualizar el mapa del sitio para incluir las nuevas páginas de ASP.NET](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image5.png)

**Figura 3**: actualizar el mapa del sitio para incluir las nuevas páginas de ASP.NET


## <a name="a-review-of-paging"></a>Una revisión de paginación

En los tutoriales anteriores, hemos visto cómo paginar los datos en los controles GridView, DetailsView y FormView. Estos tres controles ofrecen una forma sencilla de paginación llamado *paginación predeterminada* que se pueden implementar con sólo marcar la opción de habilitar la paginación en la etiqueta inteligente del control s. Con la paginación de forma predeterminada, cada vez que se solicita una página de datos en la primera página visite o cuando el usuario navega a otra página de datos del control GridView, DetailsView, o volver a solicitudes de control FormView *todas* de los datos desde el ObjectDataSource. A continuación, recortes fuera del conjunto de registros que deben mostrarse si el índice de la página solicitada y el número de registros que se muestran por página. Analizamos la paginación predeterminada con detalle en la [paginación y ordenación de datos de informe](../paging-and-sorting/paging-and-sorting-report-data-cs.md) tutorial.

Puesto que la paginación predeterminada solicita volver a todos los registros de cada página, no resulta práctico al paginar a través de lo suficientemente grandes cantidades de datos. Por ejemplo, imagine la paginación a través de 50.000 registros con un tamaño de página de 10. Cada vez que el usuario se mueve a una página nueva, 50.000 registros de todos los se deben recuperar de la base de datos, aunque se muestra solo diez de ellos.

*Paginación personalizada* soluciona los problemas de rendimiento de la paginación predeterminada seleccionando solo en el preciso subconjunto de registros que deben mostrarse en la página solicitada. Al implementar la paginación personalizada, nos debemos escribir la consulta SQL que eficazmente devolverá solo el conjunto de registros correcto. Hemos visto cómo crear una consulta con SQL Server 2005 s nuevo [ `ROW_NUMBER()` palabra clave](http://www.4guysfromrolla.com/webtech/010406-1.shtml) en el [eficazmente paginación a través de grandes cantidades de datos](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md) tutorial.

Para implementar la paginación predeterminada en los controles DataList o Repeater, podemos usar el [ `PagedDataSource` clase](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.aspx) como un contenedor en torno a la `ProductsDataTable` cuyo contenido se que se va a paginar. El `PagedDataSource` clase tiene un `DataSource` propiedad que se puede asignar a cualquier objeto enumerable y [ `PageSize` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.pagesize.aspx) y [ `CurrentPageIndex` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.currentpageindex.aspx) propiedades que indican el número de registros a Mostrar por página y el índice de la página actual. Una vez que se han establecido estas propiedades, el `PagedDataSource` puede usarse como origen de datos de cualquier control Web de datos. El `PagedDataSource`, cuando se enumera, le devuelven solo el subconjunto adecuado de los registros de su interior `DataSource` según la `PageSize` y `CurrentPageIndex` propiedades. Figura 4 representa la funcionalidad de la `PagedDataSource` clase.


![El PagedDataSource ajusta un objeto Enumerable con una interfaz paginable](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image6.png)

**Figura 4**: el `PagedDataSource` ajusta un objeto Enumerable con una interfaz paginable


La `PagedDataSource` objeto puede ser creado y configurado directamente desde la capa de lógica empresarial enlazado a un control DataList o Repeater a través de un origen ObjectDataSource, o puede crearse y configuran directamente en la clase de código subyacente de s de la página ASP.NET. Si se usa el último enfoque, debemos renunciar a través de ObjectDataSource y en su lugar, enlazar los datos paginados para el control DataList o Repeater mediante programación.

La `PagedDataSource` objeto también tiene propiedades para admitir la paginación personalizada. Sin embargo, nos podemos omitir mediante un `PagedDataSource` para la paginación personalizada dado que ya tenemos métodos BLL los `ProductsBLL` clase diseñada para la paginación personalizada que devuelven los registros precisos que se va a mostrar.

En este tutorial, echaremos un vistazo a la implementación de la paginación predeterminada en un control DataList agregando un nuevo método a la `ProductsBLL` clase que devuelve configurado adecuadamente `PagedDataSource` objeto. En el siguiente tutorial, veremos cómo usar la paginación personalizada.

## <a name="step-2-adding-a-default-paging-method-in-the-business-logic-layer"></a>Paso 2: Agregar un método de paginación de forma predeterminada en la capa de lógica de negocios

El `ProductsBLL` clase actualmente tiene un método para devolver toda la información de producto `GetProducts()` y otro para devolver un subconjunto determinado de productos en un índice de inicio `GetProductsPaged(startRowIndex, maximumRows)`. Con la paginación de forma predeterminada, el control GridView, DetailsView y FormView controla todo el uso del `GetProducts()` método para recuperar todos los productos, pero, a continuación, use un `PagedDataSource` internamente para mostrar solo el subconjunto correcto de registros. Para replicar esta funcionalidad con los controles DataList y Repeater, podemos crear un nuevo método en el BLL que imita este comportamiento.

Agregue un método a la `ProductsBLL` clase denominada `GetProductsAsPagedDataSource` que toma dos parámetros de entrada de enteros:

- `pageIndex` el índice de la página para mostrar, indizado en cero, y
- `pageSize` el número de registros que se muestran por página.

`GetProductsAsPagedDataSource` empieza recuperando *todas* los registros de `GetProducts()`. A continuación, se crea un `PagedDataSource` objeto, estableciendo su `CurrentPageIndex` y `PageSize` propiedades a los valores del pasado de `pageIndex` y `pageSize` parámetros. El método concluye devolviendo esto configurado `PagedDataSource`:


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample2.cs)]

## <a name="step-3-displaying-product-information-in-a-datalist-using-default-paging"></a>Paso 3: Mostrar información de producto en un control DataList de utilizar la paginación predeterminada

Con el `GetProductsAsPagedDataSource` método agregado a la `ProductsBLL` (clase), ahora podemos crear un control DataList o Repeater que proporciona la paginación predeterminada. Comience abriendo la `Paging.aspx` página en el `PagingSortingDataListRepeater` carpeta y arrastre un control DataList desde el cuadro de herramientas hasta el diseñador, establecer el control DataList s `ID` propiedad `ProductsDefaultPaging`. En la etiqueta inteligente de DataList s, cree un nuevo origen ObjectDataSource denominado `ProductsDefaultPagingDataSource` y configúrelo para que recupera datos mediante el `GetProductsAsPagedDataSource` método.


[![Crear un origen ObjectDataSource y configúrelo para utilizar el GetProductsAsPagedDataSource (...) (método)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image8.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image7.png)

**Figura 5**: crear un origen ObjectDataSource y configurarlo para usar el `GetProductsAsPagedDataSource` `()` método ([haga clic aquí para ver imagen en tamaño completo](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image9.png))


Establecer las listas desplegables en la actualización, INSERCIÓN y eliminar pestañas en (None).


[![Establecer la lista desplegable se enumeran en la actualización, INSERCIÓN y eliminar pestañas en (None)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image11.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image10.png)

**Figura 6**: establecer la lista desplegable se enumeran en la actualización, INSERCIÓN y eliminar pestañas en (None) ([haga clic aquí para ver imagen en tamaño completo](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image12.png))


Puesto que el `GetProductsAsPagedDataSource` método espera dos parámetros de entrada, el Asistente nos pide el origen de estos valores de parámetro.

El índice de página y los valores de tamaño de página se deben recordar en las devoluciones. Pueden almacenarse en el estado de vista, conserva en la cadena de consulta, almacenados en variables de sesión o recuerda utilizando otra técnica. En este tutorial vamos a usar la cadena de consulta, que tiene la ventaja de permitir que una página determinada de datos que se va a ser un marcador.

En concreto, utilice el pageIndex de campos de cadena de consulta y pageSize de la `pageIndex` y `pageSize` parámetros, respectivamente (consulte la figura 7). Dedique un momento para establecer los valores predeterminados para estos parámetros, como los valores de cadena de consulta ganados t estar presente cuando un usuario visita esta página por primera vez. Para `pageIndex`, establecer el valor predeterminado en 0 (lo que se mostrará la primera página de datos) y `pageSize` valor predeterminado de s a 4.


[![Use la cadena de consulta como origen para los parámetros pageIndex y pageSize](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image14.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image13.png)

**Figura 7**: usar la cadena de consulta como origen para la `pageIndex` y `pageSize` parámetros ([haga clic aquí para ver imagen en tamaño completo](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image15.png))


Después de configurar el origen ObjectDataSource, Visual Studio crea automáticamente un `ItemTemplate` para el control DataList. Personalizar el `ItemTemplate` para que se muestran el nombre de producto s, categoría y proveedor. También establece el control DataList s `RepeatColumns` propiedad en 2, su `Width` al 100% y su `ItemStyle` s `Width` al 50%. Esta configuración de ancho proporcionará espacios iguales para las dos columnas.

Después de realizar estos cambios, el marcado de s DataList y ObjectDataSource debe tener un aspecto similar al siguiente:


[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample3.aspx)]

> [!NOTE]
> Puesto que nosotros no realizan todas las actualizaciones o funcionalidad de eliminación en este tutorial, puede deshabilitar el estado de vista de DataList s para reducir el tamaño de la página representada.


Cuando inicialmente ni a visitar esta página a través de un explorador, el `pageIndex` ni `pageSize` se proporcionan parámetros de cadena de consulta. Por lo tanto, se usan los valores predeterminados de 0 y 4. Como se muestra en la figura 8, esto da como resultado un control DataList que muestra los cuatro primeros productos.


[![Se muestran los primeros cuatro productos](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image17.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image16.png)

**Figura 8**: se muestran los primeros cuatro productos ([haga clic aquí para ver imagen en tamaño completo](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image18.png))


Sin una interfaz de paginación, allí s sencillo actualmente no significa que un usuario navegue a la segunda página de datos. Vamos a crear una interfaz de paginación en el paso 4. Por ahora, sin embargo, paginación sólo es posible especificando los criterios de paginación directamente en la cadena de consulta. Por ejemplo, para ver la segunda página, cambie la dirección URL en la barra de direcciones del explorador s desde `Paging.aspx` a `Paging.aspx?pageIndex=2` y presione ENTRAR. Esto hace que la segunda página de datos que se mostrará (consulte la figura 9).


[![Se muestra la segunda página de datos](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image20.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image19.png)

**Figura 9**: se muestra la segunda página de datos ([haga clic aquí para ver imagen en tamaño completo](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image21.png))


## <a name="step-4-creating-the-paging-interface"></a>Paso 4: Creación de la interfaz de paginación

Hay una variedad de distintas interfaces de paginación que se pueden implementar. Los controles GridView, DetailsView y FormView proporcionan cuatro interfaces diferentes para elegir entre:

- **A continuación, anterior** los usuarios pueden mover una página a la vez, con lo siguiente o anterior.
- **A continuación, primero, último, anterior** además de los botones siguiente y anterior, esta interfaz incluye botones de primer y último para moverse a la primera o última página.
- **Numérico** enumera los números de página en la interfaz de paginación, lo que permite a un usuario ir directamente a una página determinada.
- **Numérico, en primer lugar, última** además de los números de página numérica, incluye botones para moverse a la primera o última página.

Para los controles DataList y Repeater, somos responsables de decidir tras una interfaz de paginación y su implementación. Esto implica la creación de los controles Web necesarios en la página y mostrar la página solicitada cuando se hace clic en un botón determinado de interfaz de paginación. Además, ciertos controles de interfaz de paginación que deba estar deshabilitado. Por ejemplo, al ver la primera página de datos mediante la siguiente, anterior, en primer lugar, por última vez la interfaz, se deshabilitarán los botones de la primera y la anterior.

Para este tutorial, let s use la siguiente, anterior, en primer lugar, por última vez la interfaz. Agregue cuatro controles de botón Web a la página y establezca sus `ID` s a `FirstPage`, `PrevPage`, `NextPage`, y `LastPage`. Establecer el `Text` propiedades a &lt; &lt; primero, &lt; Prev, junto &gt;y el último &gt; &gt; .


[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample4.aspx)]

A continuación, cree un `Click` controlador de eventos para cada uno de estos botones. En un momento, agregaremos el código necesario para mostrar la página solicitada.

## <a name="remembering-the-total-number-of-records-being-paged-through"></a>Teniendo en cuenta el número Total de registros que se va a paginar a través de

Independientemente de la interfaz de paginación seleccionada, es necesario calcular y recordar el número total de registros que se va a paginar a través. El número total de filas (en combinación con el tamaño de página) determina el número total de páginas de datos que se va a se paginan a través, que determina lo que se agregan o se habilitan los controles de interfaz de paginación. En la siguiente, anterior, primero, último interfaz que estamos creando, el número de páginas se utiliza de dos maneras:

- Para determinar si nos estamos viendo la última página, en cuyo caso se deshabilitan los botones siguiente y último.
- Si el usuario hace clic en el último botón que necesitamos captar ellos a la última página, cuyo índice es uno menor que la página cuenta.

El número de páginas se calcula como el límite máximo del recuento de filas total dividido por el tamaño de página. Por ejemplo, si nos estamos paginar 79 registros con cuatro registros por página, el número de páginas es 20 (el límite superior de 79 / 4). Si se está usando la interfaz de paginación numérico, esta información nos informa sobre el número de botones numéricos de página para mostrar; Si nuestra interfaz de paginación incluye los botones siguiente o último, el número de páginas se utiliza para determinar cuándo se debe deshabilitar los botones siguiente o último.

Si la interfaz de paginación incluye un botón última, es imperativo que el número total de registros que se va a paginar a través se recuerdan a través de las devoluciones de datos para que cuando se hace clic en el último botón podemos determinar el último índice de página. Para facilitar esto, cree un `TotalRowCount` propiedad de la clase de código subyacente de s de la página ASP.NET que se conserva su valor para el estado de vista:


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample5.cs)]

Además `TotalRowCount`, tómese un minuto para crear propiedades de nivel de página de solo lectura para acceder fácilmente al índice de la página, el tamaño de página y el recuento de páginas:


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample6.cs)]

## <a name="determining-the-total-number-of-records-being-paged-through"></a>Determinar el número Total de registros que se va a paginar a través de

El `PagedDataSource` devuelve el objeto de origen ObjectDataSource s `Select()` método tiene dentro de él *todas* de los registros de productos, aunque solo un subconjunto de ellos se muestran en el control DataList. El `PagedDataSource` s [ `Count` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.count.aspx) devuelve solo el número de elementos que se mostrará en el control DataList; el [ `DataSourceCount` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.datasourcecount.aspx) devuelve el número total de elementos dentro de la `PagedDataSource`. Por lo tanto, necesitamos asignar páginas de ASP.NET `TotalRowCount` el valor de propiedad de la `PagedDataSource` s `DataSourceCount` propiedad.

Para ello, cree un controlador de eventos para el s ObjectDataSource `Selected` eventos. En el `Selected` controlador de eventos que se tiene acceso al valor devuelto de la s ObjectDataSource `Select()` método en este caso, el `PagedDataSource`.


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample7.cs)]

## <a name="displaying-the-requested-page-of-data"></a>Mostrar la página solicitada de datos

Cuando el usuario hace clic en uno de los botones en la interfaz de paginación, es necesario mostrar la página solicitada de datos. Puesto que se especifican los parámetros de paginación a través de la cadena de consulta para mostrar la página solicitada de datos, use `Response.Redirect(url)` para que el usuario s explorador vuelve a solicitar la `Paging.aspx` página con los parámetros de paginación adecuados. Por ejemplo, para mostrar la segunda página de datos, se redirigiría al usuario `Paging.aspx?pageIndex=1`.

Para facilitar esto, cree un `RedirectUser(sendUserToPageIndex)` método que redirige al usuario a `Paging.aspx?pageIndex=sendUserToPageIndex`. A continuación, llame a este método desde el botón cuatro `Click` controladores de eventos. En el `FirstPage` `Click` controlador de eventos, llamada `RedirectUser(0)`, enviarlos a la primera página; en el `PrevPage` `Click` controlador de eventos, use `PageIndex - 1` como el índice de página; y así sucesivamente.


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample8.cs)]

Con la `Click` completar los controladores de eventos, pueden paginar a través de los registros de DataList s haciendo clic en los botones. Dedique un momento para probarlo.

## <a name="disabling-paging-interface-controls"></a>Deshabilitación de los controles de interfaz de paginación

Actualmente, los cuatro botones están habilitados, independientemente de la página que se está viendo. Sin embargo, deseamos deshabilitar los botones primero y anterior cuando se muestre la primera página de datos y los botones siguiente y último cuando se muestre la última página. El `PagedDataSource` objeto devuelto por la s ObjectDataSource `Select()` método tiene propiedades [ `IsFirstPage` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.isfirstpage.aspx) y [ `IsLastPage` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.islastpage.aspx) que podemos examinar para determinar si lo que está viendo la primera o última página de datos.

Agregue lo siguiente a la s ObjectDataSource `Selected` controlador de eventos:


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample9.cs)]

Con esta versión, se deshabilitarán los botones primero y anterior al ver la primera página, mientras que los botones siguiente y último se deshabilitarán al ver la última página.

S permiten completar la interfaz de paginación por informar al usuario qué página re viendo y existen el número total de páginas. Agregue un control Web de la etiqueta a la página y establezca su `ID` propiedad `CurrentPageNumber`. Establezca su `Text` propiedad ObjectDataSource s seleccionados controlador de eventos tal que incluye la página actual que se está viendo (`PageIndex + 1`) y el número total de páginas (`PageCount`).


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample10.cs)]

La figura 10 muestra `Paging.aspx` cuando visita por primera vez. Puesto que la cadena de consulta está vacía, el control DataList el valor predeterminado es que muestra los cuatro primeros productos; los botones primero y anterior están deshabilitados. Haga clic en siguiente muestra los siguientes cuatro registros (consulte la figura 11); Ahora se habilitan los botones primero y anterior.


[![Se muestra la primera página de datos](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image23.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image22.png)

**Figura 10**: se muestra la primera página de datos ([haga clic aquí para ver imagen en tamaño completo](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image24.png))


[![Se muestra la segunda página de datos](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image26.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image25.png)

**Figura 11**: se muestra la segunda página de datos ([haga clic aquí para ver imagen en tamaño completo](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image27.png))


> [!NOTE]
> La interfaz de paginación se puede mejorar aún más, ya que permite al usuario especificar el número de páginas para ver por página. Por ejemplo, DropDownList se puede agregar opciones de tamaño de página de listado como 5, 10, 25, 50 y todos. Al seleccionar un tamaño de página, el usuario tendría que ser redirigido a `Paging.aspx?pageIndex=0&pageSize=selectedPageSize`. Salgo de la implementación de esta mejora como un ejercicio para el lector.


## <a name="using-custom-paging"></a>La paginación personalizada

Las páginas de DataList a través de sus datos mediante la técnica de paginación predeterminada ineficaz. Cuando la paginación a través de lo suficientemente grandes cantidades de datos, es imperativo que se utiliza la paginación personalizada. Aunque los detalles de implementación difieren ligeramente, los conceptos de implementar la paginación personalizada en un control DataList son el mismo que con la paginación predeterminada. Con la paginación personalizada, utilice el `ProductBLL` clase s `GetProductsPaged` método (en lugar de `GetProductsAsPagedDataSource`). Como se describe en el [eficazmente paginación a través de grandes cantidades de datos](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md) tutorial, `GetProductsPaged` debe pasarse el inicio fila índice y el número máximo de filas que se va a devolver. Estos parámetros se pueden mantener a través de la cadena de consulta, al igual que el `pageIndex` y `pageSize` parámetros que se usan de forma predeterminada en la paginación.

Desde ahí s ningún `PagedDataSource` con paginación personalizada, deben usarse técnicas alternativas para determinar el número total de registros que se va a paginar a través y si nos re mostrando la primera o última página de datos. El `TotalNumberOfProducts()` método en el `ProductsBLL` clase devuelve el número total de productos que se va a paginar a través. Para determinar si se está viendo la primera página de datos, examine el índice de fila inicial si es cero, a continuación, se está viendo la primera página. Se está viendo la última página, si el índice de fila inicial más el número máximo de filas a devolver es mayor o igual que el número total de registros que se va a paginar a través.

Exploraremos implementar la paginación personalizada con más detalle en el siguiente tutorial.

## <a name="summary"></a>Resumen

Mientras que el control DataList ni Repeater ofrece el fuera de la compatibilidad con paginación se encuentra en el control GridView, DetailsView y FormView controla, se puede agregar esta funcionalidad con el mínimo esfuerzo. Es la manera más fácil de implementar la paginación predeterminada encapsular todo el conjunto de productos dentro de un `PagedDataSource` y, a continuación, enlazar la `PagedDataSource` para el control DataList o Repeater. En este tutorial se ha agregado el `GetProductsAsPagedDataSource` método a la `ProductsBLL` clase para devolver el `PagedDataSource`. El `ProductsBLL` ya contiene los métodos necesarios para la paginación personalizada `GetProductsPaged` y `TotalNumberOfProducts`.

Además de recuperar el conjunto de registros que deben mostrarse para la paginación personalizada, o bien, de todos los registros en un `PagedDataSource` para la paginación de forma predeterminada, también se debe agregar manualmente la interfaz de paginación. En este tutorial, creamos un siguiente, anterior, primero por última vez la interfaz con cuatro controles de botón Web. Además, se agrega un control de etiqueta muestra el número de página actual y el número total de páginas.

En el siguiente tutorial veremos cómo agregar compatibilidad con la ordenación para los controles DataList y Repeater. También veremos cómo crear a un control DataList que se puede paginar y ordenar (con ejemplos de uso predeterminada y la paginación personalizada).

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Los revisores para este tutorial fueron Liz Shulok, Ken Pespisa y Bernadette Leigh. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Siguiente](sorting-data-in-a-datalist-or-repeater-control-cs.md)
