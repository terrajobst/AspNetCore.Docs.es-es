---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-vb
title: Ordenar datos en un Control DataList o Repeater (VB) | Microsoft Docs
author: rick-anderson
description: En este tutorial, examinaremos cómo incluir compatibilidad en los controles DataList y Repeater para ordenar, así como cómo construir un DataList o Repeater pueden cuyos datos...
ms.author: riande
ms.date: 11/13/2006
ms.assetid: 97c13898-0741-45f9-b3fa-7540ab1679e6
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: ad940afd03b66c17a4d8b1e5c727c317022fbc0a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836347"
---
<a name="sorting-data-in-a-datalist-or-repeater-control-vb"></a>Ordenar datos en un Control DataList o Repeater (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_45_VB.exe) o [descargar PDF](sorting-data-in-a-datalist-or-repeater-control-vb/_static/datatutorial45vb1.pdf)

> En este tutorial, examinaremos cómo incluir compatibilidad en los controles DataList y Repeater para ordenar, así como cómo construir un DataList o Repeater cuyos datos se pueden paginar y ordenados.


## <a name="introduction"></a>Introducción

En el [tutorial anterior](paging-report-data-in-a-datalist-or-repeater-control-vb.md) examinamos cómo agregar compatibilidad con la paginación a un control DataList. Hemos creado un nuevo método en el `ProductsBLL` clase (`GetProductsAsPagedDataSource`) que devuelve un `PagedDataSource` objeto. Cuando se enlaza a un control DataList o Repeater, el control DataList o Repeater mostraría solo en la página solicitada de datos. Esta técnica es similar al que se usa internamente por los controles GridView, DetailsView y FormView para proporcionar su funcionalidad de paginación predeterminada integrada.

Además de ofrecer compatibilidad con la paginación, el control GridView también incluye de fábrica admitir la ordenación. El control DataList ni Repeater proporciona funcionalidad de ordenación integrada; Sin embargo, se pueden agregar características de ordenación con un poco de código. En este tutorial, examinaremos cómo incluir compatibilidad en los controles DataList y Repeater para ordenar, así como cómo construir un DataList o Repeater cuyos datos se pueden paginar y ordenados.

## <a name="a-review-of-sorting"></a>Una revisión de ordenación

Como hemos visto en el [paginación y ordenación de datos de informe](../paging-and-sorting/paging-and-sorting-report-data-vb.md) tutorial, el control GridView proporciona fábrica admitir la ordenación. Cada campo GridView puede tener asociado un `SortExpression`, lo que indica que el campo de datos por el que se va a ordenar los datos. Cuando la s GridView `AllowSorting` propiedad está establecida en `true`, cada campo de GridView que tiene un `SortExpression` valor de propiedad tiene el encabezado se representa como un control LinkButton. Cuando un usuario hace clic en un encabezado determinado de s de campo de GridView, se produce un postback y los datos están ordenados según el campo donde ha hecho clic s `SortExpression`.

El control GridView tiene un `SortExpression` propiedad, que almacena el `SortExpression` del campo GridView se ordenan los datos. Además, un `SortDirection` propiedad indica si los datos se ordenan en orden ascendente o descendente (si un usuario hace clic en un vínculo de encabezado de campo s GridView concreto dos veces seguidas, el criterio de ordenación se alterna).

Cuando el control GridView se enlaza a su control de origen de datos, lo manda su `SortExpression` y `SortDirection` propiedades a los datos de control de código fuente. El control de origen de datos recupera los datos y, a continuación, se ordena según proporcionado `SortExpression` y `SortDirection` propiedades. Después de ordenar los datos, el control de origen de datos lo devuelve a la GridView.

Para replicar esta funcionalidad con los controles DataList o Repeater, debemos:

- Crear una interfaz de ordenación
- Recuerde que el campo de datos para ordenar por y si se debe ordenar en orden ascendente o descendente
- Indicar a ObjectDataSource para ordenar los datos por un campo de datos concreto

Trataremos estas tres tareas en los pasos 3 y 4. A continuación, examinaremos cómo incluir tanto paginación y ordenación de soporte técnico en un control DataList o Repeater.

## <a name="step-2-displaying-the-products-in-a-repeater"></a>Paso 2: Mostrar los productos en un control Repeater

Antes de que preocuparse por implementar cualquiera de la funcionalidad relacionada con la ordenación, permiten s comienza con una lista de los productos en un control Repeater. Comience abriendo la `Sorting.aspx` página en el `PagingSortingDataListRepeater` carpeta. Agregar un control Repeater a la página web, establecer su `ID` propiedad `SortableProducts`. En la etiqueta inteligente del control Repeater s, cree un nuevo origen ObjectDataSource denominado `ProductsDataSource` y configúrelo para recuperar los datos de la `ProductsBLL` clase s `GetProducts()` método. Seleccione la que opción la (ninguno) desde las listas desplegables en las fichas de INSERT, UPDATE y DELETE.


[![Crear un origen ObjectDataSource y configúrelo para utilizar el método GetProductsAsPagedDataSource()](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image2.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image1.png)

**Figura 1**: crear un origen ObjectDataSource y configurarlo para usar el `GetProductsAsPagedDataSource()` método ([haga clic aquí para ver imagen en tamaño completo](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image3.png))


[![Establecer la lista desplegable se enumeran en la actualización, INSERCIÓN y eliminar pestañas en (None)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image5.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image4.png)

**Figura 2**: establecer la lista desplegable se enumeran en la actualización, INSERCIÓN y eliminar pestañas en (None) ([haga clic aquí para ver imagen en tamaño completo](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image6.png))


A diferencia de con el control DataList, Visual Studio no crea automáticamente un `ItemTemplate` para el control Repeater después de enlazarla a un origen de datos. Además, debemos agregar esto `ItemTemplate` mediante declaración, como la etiqueta inteligente del control s Repeater no tiene la opción de editar plantillas que se encuentra en el control DataList s. S permiten usar el mismo `ItemTemplate` desde el tutorial anterior, que muestra el nombre de producto s, proveedor y categoría.

Después de agregar el `ItemTemplate`, el marcado declarativo de s ObjectDataSource y Repeater debe ser similar al siguiente:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample1.aspx)]

Figura 3 se muestra esta página cuando se ve mediante un explorador.


[![Se muestra cada s nombre de proveedor y categoría de producto](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image8.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image7.png)

**Figura 3**: s nombre de cada producto, proveedor y categoría se muestra ([haga clic aquí para ver imagen en tamaño completo](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image9.png))


## <a name="step-3-instructing-the-objectdatasource-to-sort-the-data"></a>Paso 3: Indicar el origen ObjectDataSource para ordenar los datos

Para ordenar los datos mostrados en el control Repeater, es necesario informar a ObjectDataSource de la expresión de ordenación por el que se deben ordenar los datos. Antes de que el origen ObjectDataSource recupera sus datos, en primer lugar desencadena su [ `Selecting` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting.aspx), que proporciona una oportunidad especificar una expresión de ordenación. El `Selecting` controlador de eventos se pasa un objeto de tipo [ `ObjectDataSourceSelectingEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.aspx), que tiene una propiedad denominada [ `Arguments` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.arguments.aspx) typu [ `DataSourceSelectArguments` ](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx). El `DataSourceSelectArguments` clase está diseñada para pasar las solicitudes relacionadas con los datos de un consumidor de datos para el control de origen de datos e incluye un [ `SortExpression` propiedad](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.sortexpression.aspx).

Para pasar información de ordenación en la página ASP.NET para el origen ObjectDataSource, cree un controlador de eventos para el `Selecting` evento y utilice el siguiente código:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample2.vb)]

El *sortExpression* valor debe asignarse el nombre del campo de datos para ordenar los datos por (por ejemplo, ProductName). No hay ninguna propiedad relacionada con la dirección de ordenación, por lo que si desea ordenar los datos en orden descendente, anexe la cadena DESC a la *sortExpression* valor (por ejemplo, ProductName DESC).

Siga adelante y pruebe algunos valores codificados de forma rígida distintos para *sortExpression* y los resultados de pruebas en un explorador. Como se muestra en la figura 4, cuando se usa ProductName DESC como el *sortExpression*, los productos se ordenan por su nombre en orden alfabético inverso.


[![Los productos se ordenan por su nombre en orden alfabético inverso](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image11.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image10.png)

**Figura 4**: los productos se ordenan por su nombre en orden alfabético inverso ([haga clic aquí para ver imagen en tamaño completo](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image12.png))


## <a name="step-4-creating-the-sorting-interface-and-remembering-the-sort-expression-and-direction"></a>Paso 4: Creación de la interfaz de ordenación y recordar la expresión de ordenación y la dirección

Activar la ordenación de soporte técnico en el control GridView convierte el texto del encabezado de cada campo que se puede ordenar s en un control LinkButton que, al hacer clic, ordena los datos según corresponda. Este tipo una interfaz de ordenación tiene sentido para el control GridView, donde los datos está perfectamente colocados en las columnas. Para los controles DataList y Repeater, sin embargo, se necesita una interfaz de ordenación diferente. Una interfaz común de ordenación para obtener una lista de datos (en lugar de una cuadrícula de datos), es una lista desplegable que proporciona los campos que se pueden ordenar los datos. Permiten s implementar esta interfaz para este tutorial.

Agregar un control DropDownList Web anterior el `SortableProducts` Repeater y establezca su `ID` propiedad `SortBy`. En la ventana Propiedades, haga clic en el botón de puntos suspensivos en el `Items` propiedad para abrir el Editor de colección ListItem. Agregar `ListItem` s para ordenar los datos por la `ProductName`, `CategoryName`, y `SupplierName` campos. Agregue también un `ListItem` para ordenar los productos por su nombre en orden alfabético inverso.

El `ListItem` `Text` propiedades se pueden establecer en cualquier valor (por ejemplo, nombre), pero la `Value` propiedades deben establecerse en el nombre del campo de datos (por ejemplo, ProductName). Para ordenar los resultados en orden descendente, anexe la cadena DESC para el nombre de campo de datos, como ProductName DESC.


![Agregar un ListItem para cada uno de los campos de datos que se puede ordenar](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image13.png)

**Figura 5**: agregar un `ListItem` para cada uno de los campos de datos que se puede ordenar


Por último, agregue un control de botón Web a la derecha de la DropDownList. Establezca su `ID` a `RefreshRepeater` y su `Text` propiedad para la actualización.

Después de crear el `ListItem` s y agregar el botón de actualización, la sintaxis declarativa de s DropDownList y botón debe ser similar al siguiente:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample3.aspx)]

Con la ordenación DropDownList completa, a continuación se debe actualizar la s ObjectDataSource `Selecting` controlador de eventos para que use seleccionado `SortBy``ListItem` s `Value` propiedad en lugar de una expresión de ordenación codificado de forma rígida.


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample4.vb)]

En este punto, cuando se visita la página de primero los productos inicialmente se ordenarán por el `ProductName` campo de datos, tal y como s el `SortBy` `ListItem` seleccionada de forma predeterminada (consulte la figura 6). Seleccionar una opción como categoría de ordenación diferente y hacer clic en actualizar se producen un postback y volver a ordenar los datos por el nombre de categoría, como se muestra en la figura 7.


[![Los productos son inicialmente se ordenan por su nombre](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image15.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image14.png)

**Figura 6**: los productos son inicialmente se ordenan por su nombre ([haga clic aquí para ver imagen en tamaño completo](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image16.png))


[![Los productos están ahora ordenados por categoría](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image18.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image17.png)

**Figura 7**: los productos están ahora ordenados por categoría ([haga clic aquí para ver imagen en tamaño completo](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image19.png))


> [!NOTE]
> Al hacer clic en el botón de actualización hace que los datos automáticamente se ordenada de nuevo porque se ha deshabilitado el estado de vista de Repeater s, por lo que el control Repeater para volver a enlazar al origen de datos en cada devolución de datos. Si ve deja el estado de vista de Repeater s está habilitado, cambiar la ordenación desplegable lista ganó t tendrá ningún efecto en el criterio de ordenación. Para solucionar este problema, cree un controlador de eventos para el botón Actualizar de s `Click` eventos y volver a vincular el control Repeater a su origen de datos (mediante una llamada a Repeater s `DataBind()` método).


## <a name="remembering-the-sort-expression-and-direction"></a>Recordar la expresión de ordenación y la dirección

Al crear un DataList o Repeater que se puede ordenar en una página que no ordenación relacionados con las devoluciones de datos se pueden producir, lo imperativo s que se recuerdan la expresión de ordenación y la dirección a través de las devoluciones de datos. Por ejemplo, imagine que se ha actualizado el control Repeater en este tutorial para incluir un botón de eliminación con cada producto. Cuando el usuario hace clic en el botón Eliminar d ejecutamos algo de código para eliminar el producto seleccionado y, a continuación, volver a enlazar los datos para el control Repeater. Si no se conservan los detalles de ordenación a través de la devolución de datos, se revertirán los datos mostrados en pantalla en el criterio de ordenación original.

Para este tutorial, DropDownList guarda implícitamente la ordenación expresión y la dirección en su estado de vista para nosotros. Si usamos una interfaz de ordenación diferente, uno con, por ejemplo, LinkButtons que proporcionan las distintas opciones de ordenación d necesitamos tener cuidado para recordar el criterio de ordenación entre las devoluciones de datos. Esto puede realizarse mediante el almacenamiento de los parámetros de ordenación en el estado de vista de página s, incluyendo el parámetro de ordenación en la cadena de consulta, o a través de otra técnica de persistencia de estado.

Ejemplos de futuras de este tutorial exploración cómo conservar los detalles de ordenación en el estado de vista de página s.

## <a name="step-5-adding-sorting-support-to-a-datalist-that-uses-default-paging"></a>Paso 5: Agregar compatibilidad para ordenar en un control DataList que utiliza la paginación predeterminada

En el [tutorial anterior](paging-report-data-in-a-datalist-or-repeater-control-vb.md) hemos visto cómo implementar la paginación predeterminada con un control DataList. Permiten s ampliar este ejemplo anterior para incluir la capacidad de ordenar los datos paginados. Comience abriendo la `SortingWithDefaultPaging.aspx` y `Paging.aspx` páginas en el `PagingSortingDataListRepeater` carpeta. Desde el `Paging.aspx` página, haga clic en el botón fuente para ver el marcado declarativo de la página s. Copie el texto seleccionado (consulte la figura 8) y péguelo en el marcado declarativo de `SortingWithDefaultPaging.aspx` entre el `<asp:Content>` etiquetas.


[![Replicar el marcado declarativo en la &lt;asp: Content&gt; etiquetas de Paging.aspx a SortingWithDefaultPaging.aspx](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image21.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image20.png)

**Figura 8**: replicar el marcado declarativo en la `<asp:Content>` etiquetas de `Paging.aspx` a `SortingWithDefaultPaging.aspx` ([haga clic aquí para ver imagen en tamaño completo](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image22.png))


Después de copiar el marcado declarativo, copie los métodos y propiedades en el `Paging.aspx` página de la clase de código subyacente de s a la clase de código subyacente para `SortingWithDefaultPaging.aspx`. A continuación, tómese un momento para ver el `SortingWithDefaultPaging.aspx` página en un explorador. Deben presentar la misma funcionalidad y apariencia a medida que `Paging.aspx`.

## <a name="enhancing-productsbll-to-include-a-default-paging-and-sorting-method"></a>Mejorar ProductsBLL para incluir un valor predeterminado de paginación y ordenación (método)

En el tutorial anterior, creamos un `GetProductsAsPagedDataSource(pageIndex, pageSize)` método en el `ProductsBLL` clase que devuelve un `PagedDataSource` objeto. Esto `PagedDataSource` objeto se rellenó con *todas* de los productos (a través de la s BLL `GetProducts()` método), pero cuando se enlaza al control DataList solo aquellos registros correspondiente al especificado *pageIndex* y *pageSize* se mostraron los parámetros de entrada.

Anteriormente en este tutorial, hemos agregado compatibilidad con la ordenación mediante la especificación de la expresión de ordenación del origen ObjectDataSource s `Selecting` controlador de eventos. Esto funciona bien cuando el origen ObjectDataSource se devuelve un objeto que se puedan ordenar, como el `ProductsDataTable` devuelto por la `GetProducts()` método. Sin embargo, el `PagedDataSource` objeto devuelto por la `GetProductsAsPagedDataSource` método no admite la ordenación de su origen de datos interna. En su lugar, es necesario ordenar los resultados devueltos desde el `GetProducts()` método *antes* se coloca en el `PagedDataSource`.

Para ello, cree un nuevo método en el `ProductsBLL` (clase), `GetProductsSortedAsPagedDataSource(sortExpression, pageIndex, pageSize)`. Para ordenar el `ProductsDataTable` devuelto por la `GetProducts()` método, especifique el `Sort` propiedad de su valor predeterminado `DataTableView`:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample5.vb)]

El `GetProductsSortedAsPagedDataSource` método difiere ligeramente de la `GetProductsAsPagedDataSource` método creado en el tutorial anterior. En concreto, `GetProductsSortedAsPagedDataSource` acepta un parámetro de entrada adicional `sortExpression` y asigna este valor para el `Sort` propiedad de la `ProductDataTable` s `DefaultView`. Unas pocas líneas de código más adelante, el `PagedDataSource` s DataSource del objeto se asigna el `ProductDataTable` s `DefaultView`.

## <a name="calling-the-getproductssortedaspageddatasource-method-and-specifying-the-value-for-the-sortexpression-input-parameter"></a>Una llamada al método GetProductsSortedAsPagedDataSource y especificar un valor para el parámetro de entrada SortExpression

Con el `GetProductsSortedAsPagedDataSource` método completo, el siguiente paso es proporcionar el valor para este parámetro. ObjectDataSource en `SortingWithDefaultPaging.aspx` está configurado actualmente para llamar a la `GetProductsAsPagedDataSource` método y pasa dos parámetros de entrada a través de sus dos `QueryStringParameters`, que se especifican en el `SelectParameters` colección. Estos dos `QueryStringParameters` indicar que el origen de la `GetProductsAsPagedDataSource` método s *pageIndex* y *pageSize* parámetros proceden de los campos de cadena de consulta `pageIndex` y `pageSize`.

Actualizar la s ObjectDataSource `SelectMethod` propiedad, por lo que TI invoca el nuevo `GetProductsSortedAsPagedDataSource` método. A continuación, agregue un nuevo `QueryStringParameter` para que el *sortExpression* parámetro de entrada se obtiene acceso desde el campo de cadena de consulta `sortExpression`. Establecer el `QueryStringParameter` s `DefaultValue` ProductName.

Después de estos cambios, el marcado declarativo de ObjectDataSource s aspecto:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample6.aspx)]

En este momento, el `SortingWithDefaultPaging.aspx` página ordenará los resultados alfabéticamente por nombre de producto (consulte la figura 9). Esto es porque, de forma predeterminada, se pasa un valor de ProductName como el `GetProductsSortedAsPagedDataSource` método s *sortExpression* parámetro.


[![De forma predeterminada, los resultados se ordenan por ProductName](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image24.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image23.png)

**Figura 9**: de forma predeterminada, los resultados se ordenan por `ProductName` ([haga clic aquí para ver imagen en tamaño completo](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image25.png))


Si agrega manualmente un `sortExpression` campo querystring como `SortingWithDefaultPaging.aspx?sortExpression=CategoryName` los resultados se ordenarán por el `sortExpression`. Sin embargo, esto `sortExpression` parámetro no se incluye en la cadena de consulta cuando se mueve a otra página de datos. De hecho, al hacer clic en la página siguiente o último botones toma nos al `Paging.aspx`! Además, s hay actualmente ninguna ordenación de la interfaz. La única forma de que un usuario puede cambiar el criterio de ordenación de los datos paginados es manipular directamente la cadena de consulta.

## <a name="creating-the-sorting-interface"></a>Creación de la interfaz de ordenación

En primer lugar deberá actualizar el `RedirectUser` método envíe al usuario `SortingWithDefaultPaging.aspx` (en lugar de `Paging.aspx`) e incluir el `sortExpression` valor en la cadena de consulta. También debemos agregar un de sólo lectura, nivel de página denominado `SortExpression` propiedad. Esta propiedad, similar a la `PageIndex` y `PageSize` propiedades creadas en el tutorial anterior, se devuelve el valor de la `sortExpression` campo de cadena de consulta, si existe y el valor predeterminado (ProductName) en caso contrario.

Actualmente la `RedirectUser` método acepta solo un único parámetro de entrada del índice de la página para mostrar. Sin embargo, puede haber ocasiones en que desee redirigir al usuario a una página determinada de datos mediante una expresión de ordenación que no sea de qué s especificado en la cadena de consulta. En un momento, vamos a crear la interfaz de ordenación para esta página, que incluirá una serie de controles Web de botón para ordenar los datos por una columna especificada. Cuando se hace clic en uno de esos botones, queremos redirigir al usuario pasando el valor de la expresión de ordenación adecuado. Para proporcionar esta funcionalidad, cree dos versiones de la `RedirectUser` método. La primera de ellas debe aceptar el índice de página para mostrar, mientras que la segunda acepta la expresión de índice y de ordenación de la página.


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample7.vb)]

En el primer ejemplo de este tutorial, creamos una interfaz de ordenación con un DropDownList. En este ejemplo, s permiten usar tres controles de botón Web colocados sobre el control DataList uno para ordenar por `ProductName`, uno para `CategoryName`y otro para `SupplierName`. Agregue los tres controles de botón Web, establecer sus `ID` y `Text` propiedades correctamente:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample8.aspx)]

A continuación, cree un `Click` controlador de eventos para cada uno. Deben llamar los controladores de eventos el `RedirectUser` método, que se devuelve al usuario a la primera página con la expresión de ordenación adecuado.


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample9.vb)]

Cuando se visita primero la página, los datos están ordenados alfabéticamente por nombre de producto (consulte la vuelta a la figura 9). Haga clic en el botón siguiente para avanzar a la segunda página de datos y, a continuación, haga clic en el criterio de ordenación mediante el botón de categoría. Esto nos devuelve a la primera página de datos, ordenados por nombre de categoría (consulte la figura 10). Del mismo modo, al hacer clic en el criterio de ordenación mediante el botón de proveedor ordena los datos por el proveedor a partir de la primera página de datos. Como los datos están paginados a través, se recuerda la opción de ordenación. Figura 11 muestra la página después de ordenar por categoría y, a continuación, avanzar a la página decimotercer de datos.


[![Los productos se ordenan por categoría](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image27.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image26.png)

**Figura 10**: los productos se ordenan por categoría ([haga clic aquí para ver imagen en tamaño completo](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image28.png))


[![La expresión de ordenación es recuerdan cuando paginación a través de los datos](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image30.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image29.png)

**Figura 11**: la expresión de ordenación es recuerdan cuando paginación a través de los datos ([haga clic aquí para ver imagen en tamaño completo](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image31.png))


## <a name="step-6-custom-paging-through-records-in-a-repeater"></a>Paso 6: Paginación personalizada a través de registros en un control Repeater

El ejemplo de DataList examina en el paso 5 páginas a través de sus datos mediante la técnica de paginación predeterminada ineficaz. Cuando la paginación a través de lo suficientemente grandes cantidades de datos, es imperativo que se utiliza la paginación personalizada. En el [eficazmente paginar a través de grandes cantidades de datos](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) y [ordenar datos paginados personalizados](../paging-and-sorting/sorting-custom-paged-data-vb.md) tutoriales, examinamos las diferencias entre predeterminado y la paginación personalizada y los métodos creados en el nivel de lógica empresarial para uso personalizado de paginar y ordenar datos paginados personalizados. En concreto, en estos dos tutoriales anteriores se han agregado los tres métodos siguientes a la `ProductsBLL` clase:

- `GetProductsPaged(startRowIndex, maximumRows)` Devuelve un subconjunto determinado de registros a partir de *startRowIndex* y que no supere *maximumRows*.
- `GetProductsPagedAndSorted(sortExpression, startRowIndex, maximumRows)` Devuelve un subconjunto específico de registros ordenados por especificado *sortExpression* parámetro de entrada.
- `TotalNumberOfProducts()` proporciona el número total de registros en el `Products` tabla de base de datos.

Estos métodos se pueden usar la página de forma eficiente y ordenar los datos mediante un control DataList o Repeater. Para ilustrar esto, permiten s empiece por crear un control Repeater con compatibilidad con la paginación personalizada; a continuación, vamos a agregar las funcionalidades de ordenación.

Abra el `SortingWithCustomPaging.aspx` página en el `PagingSortingDataListRepeater` carpeta y agregar un control Repeater a la página de configuración su `ID` propiedad `Products`. En la etiqueta inteligente del control Repeater s, cree un nuevo origen ObjectDataSource denominado `ProductsDataSource`. Configúrelo para seleccionar sus datos de la `ProductsBLL` clase s `GetProductsPaged` método.


[![Configurar el origen ObjectDataSource para usar el método de clase ProductsBLL s GetProductsPaged](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image33.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image32.png)

**Figura 12**: configurar el origen ObjectDataSource que se usarán el `ProductsBLL` clase s `GetProductsPaged` método ([haga clic aquí para ver imagen en tamaño completo](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image34.png))


Establecer las listas desplegables en la actualización, INSERCIÓN y eliminar las fichas (ninguno) y, a continuación, haga clic en el botón siguiente. Ahora solicita el Asistente para configurar orígenes de datos para los orígenes de la `GetProductsPaged` método s *startRowIndex* y *maximumRows* parámetros de entrada. En realidad, se omiten estos parámetros de entrada. En su lugar, el *startRowIndex* y *maximumRows* valores se pasará en el `Arguments` propiedad en la s ObjectDataSource `Selecting` controlador de eventos, al igual que cómo especificamos la *sortExpression* en esta demostración primer tutorial s. Por lo tanto, deje el parámetro source listas desplegables en el Asistente para configurar en ninguno.


[![Deje los orígenes de parámetro en None](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image36.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image35.png)

**Figura 13**: dejar los orígenes de parámetro establecido en None ([haga clic aquí para ver imagen en tamaño completo](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image37.png))


> [!NOTE]
> Hacer *no* establecer la s ObjectDataSource `EnablePaging` propiedad `true`. Esto hará que el ObjectDataSource incluir automáticamente su propio *startRowIndex* y *maximumRows* parámetros para el `SelectMethod` lista de parámetros existente s. El `EnablePaging` propiedad es útil cuando el enlace personalizado paginar datos a un control GridView, DetailsView o FormView porque estos controles esperan ciertos comportamientos de ObjectDataSource s solo está disponible cuando `EnablePaging` propiedad es `true`. Puesto que tenemos que agregar manualmente la compatibilidad con la paginación para los controles DataList y Repeater, deje esta propiedad establecida en `false` (valor predeterminado), tal como se podrá integrar en la funcionalidad necesaria directamente en nuestra página ASP.NET.


Por último, defina el control Repeater s `ItemTemplate` para que se muestran el nombre de producto s, la categoría y el proveedor. Después de estos cambios, la sintaxis declarativa del control Repeater y ObjectDataSource s debe ser similar al siguiente:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample10.aspx)]

Dedique un momento para visitar la página a través de un explorador y tenga en cuenta que se devuelve ningún registro. Esto es porque se ve aún para especificar el *startRowIndex* y *maximumRows* valores de parámetro; por lo tanto, los valores de 0 se han transmitido para ambos. Para especificar estos valores, cree un controlador de eventos para el s ObjectDataSource `Selecting` evento y establecer estos parámetros de valores mediante programación a los valores codificados de forma rígida de 0 y 5, respectivamente:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample11.vb)]

Con este cambio, la página, cuando se ve mediante un explorador, muestra los cinco primeros productos.


[![Se muestran los primeros cinco registros](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image39.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image38.png)

**Figura 14**: se muestran los primeros cinco registros ([haga clic aquí para ver imagen en tamaño completo](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image40.png))


> [!NOTE]
> Los productos enumerados en la figura 14 ocurrir se ordene por nombre de producto, porque el `GetProductsPaged` procedimiento almacenado que realiza la consulta de paginación personalizada eficaz ordena los resultados por `ProductName`.


Para permitir al usuario paso a paso a través de las páginas, es necesario realizar un seguimiento de los índices de fila de inicial y el número máximo de filas y recordar estos valores entre las devoluciones de datos. En el ejemplo de paginación predeterminada se utilizan campos de cadena de consulta para conservar estos valores; para esta demostración, permiten s conservar esta información en el estado de vista de página s. Cree las dos propiedades siguientes:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample12.vb)]

A continuación, actualice el código en el controlador de eventos cuando se selecciona para que utilice el `StartRowIndex` y `MaximumRows` propiedades en lugar de los valores codificados de forma rígida de 0 y 5:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample13.vb)]

En este momento nuestra página sigue mostrando solo los primeros cinco registros. Sin embargo, con estas propiedades en su lugar, se está listo para crear la interfaz de paginación.

## <a name="adding-the-paging-interface"></a>Adición de la interfaz de paginación

S Let emplean el mismo primer, anterior, siguiente por última vez la paginación de la interfaz se utiliza en el ejemplo de paginación de forma predeterminada, incluida la Web de la etiqueta de control que muestra qué página de datos se está viendo y existen el número total de páginas. Agregue los cuatro controles Web de botón y etiqueta debajo del repetidor.


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample14.aspx)]

A continuación, cree `Click` controladores de eventos para los cuatro botones. Cuando se hace clic en uno de estos botones, deberá actualizar el `StartRowIndex` y volver a enlazar los datos para el control Repeater. El código para los botones de la primera, anterior y siguiente es bastante sencillo, pero para el último botón ¿cómo se determina el índice de fila inicial de la última página de datos? Para calcular este índice así como ser capaz de determinar que si se deben habilitar los botones siguiente y último, necesitamos saber cuántos registros en total que se va a se paginan a través. Podemos determinar esto mediante una llamada a la `ProductsBLL` clase s `TotalNumberOfProducts()` método. S permiten crear una propiedad de solo lectura, el nivel de página denominada `TotalRowCount` que devuelve los resultados de la `TotalNumberOfProducts()` método:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample15.vb)]

Con esta propiedad ahora podemos determinar el último índice de fila de s de la página Inicio. En concreto, lo s el resultado entero de la `TotalRowCount` menos 1 dividido entre `MaximumRows`, multiplicado por `MaximumRows`. Ahora podemos escribir el `Click` controladores de eventos para los cuatro botones de la interfaz de paginación:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample16.vb)]

Por último, es necesario deshabilitar los botones anterior y primero en la interfaz de paginación al ver la primera página de datos y los botones siguiente y último al visualizar la última página. Para ello, agregue el código siguiente al origen ObjectDataSource s `Selecting` controlador de eventos:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample17.vb)]

Después de agregar estos `Click` controladores de eventos y el código para habilitar o deshabilitar los elementos de interfaz de paginación basados en el índice de fila actual de inicio, la página de prueba en un explorador. Figura 15 se muestra, cuando se visita la página de la primera de primero y los botones anterior le están deshabilitadas. Haga clic en siguiente se muestra la segunda página de datos, mientras que al hacer clic en la última muestra la última página (consulte las figuras 16 y 17). Al ver la última página de datos se deshabilitan los botones de la siguiente y el último.


[![Se deshabilitan los botones anterior y último al visualizar la primera página de productos](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image42.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image41.png)

**Figura 15**: botones último y el anterior se deshabilitan al ver la primera página de productos ([haga clic aquí para ver imagen en tamaño completo](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image43.png))


[![La segunda página de productos son muestra](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image45.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image44.png)

**Figura 16**: la segunda página de los productos son muestra ([haga clic aquí para ver imagen en tamaño completo](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image46.png))


[![Al hacer clic en el último muestra la última página de datos](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image48.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image47.png)

**Figura 17**: al hacer clic en la última muestra la página de datos Final ([haga clic aquí para ver imagen en tamaño completo](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image49.png))


## <a name="step-7-including-sorting-support-with-the-custom-paged-repeater"></a>Paso 7: Incluidas la compatibilidad con personalizado para ordenar paginado Repeater

Ahora que se ha implementado la paginación personalizada, está listo para incluir la ordenación admitimos. El `ProductsBLL` clase s `GetProductsPagedAndSorted` método tiene el mismo *startRowIndex* y *maximumRows* parámetros como entrada `GetProductsPaged`, pero permite más  *sortExpression* parámetro de entrada. Para usar el `GetProductsPagedAndSorted` método desde `SortingWithCustomPaging.aspx`, es necesario realizar los pasos siguientes:

1. Cambiar la s ObjectDataSource `SelectMethod` propiedad desde `GetProductsPaged` a `GetProductsPagedAndSorted`.
2. Agregar un *sortExpression* `Parameter` objeto al origen ObjectDataSource s `SelectParameters` colección.
3. Crear una privada, el nivel de página `SortExpression` propiedad que se conserve su valor entre las devoluciones de datos a través del estado de vista de página s.
4. Actualizar la s ObjectDataSource `Selecting` controlador de eventos para asignar la s ObjectDataSource *sortExpression* parámetro el valor de nivel de página `SortExpression` propiedad.
5. Crear la interfaz de ordenación.

Empiece actualizando la s ObjectDataSource `SelectMethod` propiedad y agregando un *sortExpression* `Parameter`. Asegúrese de que el *sortExpression* `Parameter` s `Type` propiedad está establecida en `String`. Después de completar estas dos primeras tareas, el marcado declarativo de ObjectDataSource s debe ser similar al siguiente:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample18.aspx)]

A continuación, necesitamos un nivel de página `SortExpression` propiedad cuyo valor se serializa en el estado de vista. Si no se ha establecido ningún valor de la expresión de ordenación, use ProductName como el valor predeterminado:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample19.vb)]

Antes de que el origen ObjectDataSource invoca el `GetProductsPagedAndSorted` método es necesario establecer la *sortExpression* `Parameter` al valor de la `SortExpression` propiedad. En el `Selecting` controlador de eventos, agregue la siguiente línea de código:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample20.vb)]

Todo lo que queda es implementar la interfaz de ordenación. Como hicimos en el último ejemplo, permiten s tiene la interfaz de ordenación implementada mediante tres controles de botón Web que permiten al usuario ordenar los resultados por nombre de producto, categoría o proveedor.


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample21.aspx)]

Crear `Click` controladores de eventos para estos tres controles de botón. En el evento de restablecimiento de controlador, el `StartRowIndex` en 0, establezca el `SortExpression` para el valor adecuado y volver a vincular los datos para el control Repeater:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample22.vb)]

¡Y ya está todo s! Cuando no existía una serie de pasos para obtener la paginación personalizada y la ordenación implementado, los pasos fueron muy similares a aquellas necesarias para la paginación predeterminada. Figura 18 se muestra los productos al ver la última página de datos cuando se ordenan por categoría.


[![Se muestra la última página de datos, ordenado por categoría,](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image51.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image50.png)

**Figura 18**: última página de datos, ordenado por categoría, se muestra ([haga clic aquí para ver imagen en tamaño completo](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image52.png))


> [!NOTE]
> En los ejemplos anteriores, al ordenar por el proveedor que NombreProveedor se usó como la expresión de ordenación. Sin embargo, para la implementación de la paginación personalizada, debemos usar CompanyName. Esto es porque el procedimiento almacenado responsable de implementar la paginación personalizada `GetProductsPagedAndSorted` pasa la expresión de ordenación en el `ROW_NUMBER()` palabra clave, el `ROW_NUMBER()` requiere la palabra clave el nombre de columna real en lugar de un alias. Por lo tanto, debemos usar `CompanyName` (el nombre de la columna en la `Suppliers` tabla) en lugar de lo alias que se usa en el `SELECT` consulta (`SupplierName`) para la expresión de ordenación.


## <a name="summary"></a>Resumen

Ni el control DataList ni Repeater ofrecen compatibilidad integrada con la ordenación, pero con un poco de código y una interfaz de ordenación personalizada, puede agregar esta funcionalidad. Al implementar la ordenación, pero no de paginación, la expresión de ordenación se puede especificar mediante el `DataSourceSelectArguments` objeto pasa a la s ObjectDataSource `Select` método. Esto `DataSourceSelectArguments` objeto s `SortExpression` propiedad se puede asignar en la s ObjectDataSource `Selecting` controlador de eventos.

Para agregar capacidades de ordenación a un control DataList o Repeater que ya proporciona la compatibilidad con la paginación, el enfoque más sencillo consiste en Personalizar la capa de lógica empresarial para que incluya un método que acepta una expresión de ordenación. Esta información, a continuación, se puede pasar a través de un parámetro en la s ObjectDataSource `SelectParameters`.

En este tutorial se completa un examen nuestro de paginación y ordenación con los controles DataList y Repeater. Nuestro tutorial siguiente y último examinará cómo agregar controles de botón Web a las plantillas de controles DataList y Repeater s con el fin de proporcionar alguna funcionalidad personalizada, iniciada por el usuario en cada documento.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Revisor de clientes potencial para este tutorial era David Suru. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](paging-report-data-in-a-datalist-or-repeater-control-vb.md)
