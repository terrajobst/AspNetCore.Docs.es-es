---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-cs
title: Ordenar datos en un Control de repetidor (C#) o de DataList | Documentos de Microsoft
author: rick-anderson
description: "En este tutorial, examinaremos cómo incluir compatibilidad DataList y repetidor para ordenar, así como cómo construir un DataList o repetidor cuyos datos pueden..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2006
ms.topic: article
ms.assetid: f52c302a-1b7c-46fe-8a13-8412c95cbf6d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: cfd0cdb0afe3bf71686715c0b1891adfbbd5019a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="sorting-data-in-a-datalist-or-repeater-control-c"></a>Ordenar datos en un DataList o un Control de repetidor (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_45_CS.exe) o [descarga de PDF](sorting-data-in-a-datalist-or-repeater-control-cs/_static/datatutorial45cs1.pdf)

> En este tutorial, examinaremos cómo incluir compatibilidad DataList y repetidor para ordenar, así como cómo construir un DataList o repetidor cuyos datos se pueden paginar y ordenar.


## <a name="introduction"></a>Introducción

En el [tutorial anterior](paging-report-data-in-a-datalist-or-repeater-control-cs.md) se examina cómo agregar compatibilidad con la paginación a un control DataList. Hemos creado un nuevo método en el `ProductsBLL` clase (`GetProductsAsPagedDataSource`) que devuelve un `PagedDataSource` objeto. Cuando se enlaza a un DataList o repetidor, el DataList o repetidor mostraría solo la página solicitada de datos. Esta técnica es similar a lo que se usa internamente por los controles GridView, DetailsView y FormView para proporcionar su funcionalidad de paginación predefinidas.

Además de ofrecer compatibilidad con la paginación, el control GridView también incluye innovadoras admitir la ordenación. El DataList ni repetidor proporciona funcionalidad integrada de ordenación; Sin embargo, se pueden agregar características de ordenación con un poco de código. En este tutorial, examinaremos cómo incluir compatibilidad DataList y repetidor para ordenar, así como cómo construir un DataList o repetidor cuyos datos se pueden paginar y ordenar.

## <a name="a-review-of-sorting"></a>Una revisión de ordenación

Como vimos en el [paginación y ordenar datos de informe](../paging-and-sorting/paging-and-sorting-report-data-cs.md) tutorial, el control GridView proporciona listas para su compatibilidad con la ordenación. Cada campo de GridView puede tener asociado un `SortExpression`, lo que indica el campo de datos por el que se va a ordenar los datos. Cuando las operaciones de asignación GridView `AllowSorting` propiedad está establecida en `true`, cada campo de GridView que tiene un `SortExpression` valor de propiedad tiene el encabezado se representa como un control LinkButton. Cuando un usuario hace clic en un encabezado de s de campo determinado de GridView, se produce una devolución de datos y los datos se ordenan según el campo donde ha hecho clic s `SortExpression`.

El control GridView tiene un `SortExpression` propiedad, que almacena el `SortExpression` del campo GridView se ordenan los datos. Además, un `SortDirection` propiedad indica si los datos se ordenen en orden ascendente o descendente (si un usuario hace clic en un vínculo de encabezado de campo s GridView concreto dos veces seguidas, el criterio de ordenación se alterna).

Cuando se enlaza el control GridView a su control de origen de datos, entrega su `SortExpression` y `SortDirection` propiedades a los datos de control de código fuente. El control de origen de datos recupera los datos y, a continuación, se ordena según el proporcionado `SortExpression` y `SortDirection` propiedades. Después de ordenar los datos, el control de origen de datos lo devuelve a la GridView.

Para replicar esta funcionalidad con los controles DataList o repetidor, se debe:

- Crear una interfaz de ordenación
- Recuerde que el campo de datos para ordenar por y si se debe ordenar en orden ascendente o descendente
- Indicar el ObjectDataSource para ordenar los datos por un campo de datos concreto

Abordaremos estas tres tareas en los pasos 3 y 4. A continuación, examinaremos cómo incluir tanto paginación y la ordenación de soporte técnico en un DataList o repetidor.

## <a name="step-2-displaying-the-products-in-a-repeater"></a>Paso 2: Mostrar los productos en un repetidor

Antes de que nos preocupamos por implementar cualquiera de la funcionalidad relacionada con la ordenación, permiten s comienza con una lista de los productos en un control de repetidor. Comience abriendo la `Sorting.aspx` página en el `PagingSortingDataListRepeater` carpeta. Agregar un control de repetidor a la página web, establecer su `ID` propiedad `SortableProducts`. Desde la etiqueta inteligente de repetidor s, crear un nuevo origen ObjectDataSource denominado `ProductsDataSource` y configúrelo para recuperar los datos de la `ProductsBLL` clase s. `GetProducts()` método. Seleccione la que opción el (ninguno) desde las listas desplegables en las pestañas INSERT, UPDATE y DELETE.


[![Cree un ObjectDataSource y configúrelo para utilizar el método GetProductsAsPagedDataSource()](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image2.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image1.png)

**Figura 1**: cree un ObjectDataSource y configúrelo para utilizar el `GetProductsAsPagedDataSource()` método ([haga clic aquí para ver la imagen a tamaño completo](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image3.png))


[![Establecer la lista desplegable que se enumeran en la actualización, INSERCIÓN y eliminar las fichas (ninguno)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image5.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image4.png)

**Figura 2**: establecer la lista desplegable que se enumeran en la actualización, INSERCIÓN y eliminar las fichas (ninguno) ([haga clic aquí para ver la imagen a tamaño completo](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image6.png))


A diferencia de con el control DataList, Visual Studio no crea automáticamente un `ItemTemplate` para el control de repetidor después de enlazar a un origen de datos. Además, debemos agregar esto `ItemTemplate` mediante declaración, como la etiqueta inteligente de repetidor control s no tiene la opción de editar plantillas que se encuentra en el control DataList s. S permiten usar el mismo `ItemTemplate` desde el tutorial anterior, que muestra el nombre de producto s, el proveedor y la categoría.

Después de agregar el `ItemTemplate`, el marcado declarativo de s repetidor y ObjectDataSource debe ser similar al siguiente:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample1.aspx)]

La figura 3 muestra esta página cuando se ve mediante un explorador.


[![Se muestra cada s nombre de proveedor y la categoría de producto](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image8.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image7.png)

**Figura 3**: s nombre de cada producto, proveedor y la categoría se muestra ([haga clic aquí para ver la imagen a tamaño completo](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image9.png))


## <a name="step-3-instructing-the-objectdatasource-to-sort-the-data"></a>Paso 3: Indicar el ObjectDataSource para ordenar los datos

Para ordenar los datos mostrados en el repetidor, es necesario informar a ObjectDataSource de la expresión de ordenación por el que se deben ordenar los datos. Antes de que el ObjectDataSource recupera sus datos, en primer lugar desencadena su [ `Selecting` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting.aspx), que proporciona una oportunidad para que podamos especificar una expresión de ordenación. El `Selecting` controlador de eventos se pasa un objeto de tipo [ `ObjectDataSourceSelectingEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.aspx), que tiene una propiedad denominada [ `Arguments` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.arguments.aspx) de tipo [ `DataSourceSelectArguments` ](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx). El `DataSourceSelectArguments` clase está diseñada para pasar las solicitudes relacionadas con datos de un consumidor de datos para el control de origen de datos e incluye una [ `SortExpression` propiedad](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.sortexpression.aspx).

Para pasar información de ordenación de la página ASP.NET a ObjectDataSource, crear un controlador de eventos para el `Selecting` evento y utilice el código siguiente:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample2.cs)]

El *sortExpression* se debe asignar el nombre del campo de datos para ordenar los datos por (por ejemplo, ProductName) valor. No hay ninguna propiedad relacionados con la dirección de ordenación, por lo que si desea ordenar los datos en orden descendente, se anexa la cadena DESC a la *sortExpression* valor (por ejemplo, ProductName DESC).

Voy a probar algunos distintos valores codificados de forma rígida para *sortExpression* y los resultados de pruebas en un explorador. Como se muestra en la figura 4, cuando se usa ProductName DESC como el *sortExpression*, los productos se ordenan por su nombre en orden alfabético inverso.


[![Los productos se ordenan por su nombre en orden alfabético inverso](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image11.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image10.png)

**Figura 4**: los productos se ordenan por su nombre en orden alfabético inverso ([haga clic aquí para ver la imagen a tamaño completo](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image12.png))


## <a name="step-4-creating-the-sorting-interface-and-remembering-the-sort-expression-and-direction"></a>Paso 4: Creación de la interfaz de ordenación y teniendo en cuenta la expresión de ordenación y la dirección

Activar la ordenación compatibilidad en GridView convierte cada texto de encabezado de campo que se puede ordenar s en un control LinkButton que, al hacer clic, ordena los datos según corresponda. Dicha una interfaz de ordenación tiene sentido para el control GridView, donde sus claridad diseño de los datos en columnas. Para los controles DataList y repetidor, sin embargo, se necesita una interfaz de ordenación diferente. Una interfaz común de ordenación para obtener una lista de datos (en lugar de una cuadrícula de datos), es una lista desplegable que proporciona los campos por el que se pueden ordenar los datos. Permiten s implementar dicha interfaz para este tutorial.

Agregar un control DropDownList Web anterior la `SortableProducts` repetidor y establezca su `ID` propiedad `SortBy`. En la ventana Propiedades, haga clic en el botón de puntos suspensivos en el `Items` propiedad para abrir el Editor de colección ListItem. Agregar `ListItem` s para ordenar los datos por la `ProductName`, `CategoryName`, y `SupplierName` campos. Agregar un `ListItem` para ordenar los productos por su nombre en orden alfabético inverso.

El `ListItem` `Text` propiedades pueden establecerse en cualquier valor (por ejemplo, nombre), pero la `Value` propiedades deben establecerse en el nombre del campo de datos (por ejemplo, ProductName). Para ordenar los resultados en orden descendente, anexe la cadena DESC al nombre del campo de datos, como ProductName DESC.


![Agregar un ListItem para cada uno de los campos de datos que se puede ordenar](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image13.png)

**Figura 5**: agregar un `ListItem` para cada uno de los campos de datos que se puede ordenar


Por último, agregue un control de botón Web a la derecha de DropDownList. Establecer su `ID` a `RefreshRepeater` y su `Text` propiedad en la actualización.

Después de crear el `ListItem` s y agregar el botón de actualización, la sintaxis declarativa de s DropDownList y botón debe ser similar al siguiente:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample3.aspx)]

Con la ordenación DropDownList completa, a continuación deberá actualizar la s ObjectDataSource `Selecting` controlador de eventos para que use seleccionado `SortBy``ListItem` s `Value` propiedad en lugar de una expresión de ordenación codificado de forma rígida.


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample4.cs)]

En este punto al visitar la página de primero los productos inicialmente se ordenarán por el `ProductName` campo de datos, tal y como s el `SortBy` `ListItem` seleccionada de forma predeterminada (consulte la figura 6). Al seleccionar una opción como categoría de ordenación diferente y haciendo clic en actualizar provocará una devolución de datos y volver a ordenar los datos por el nombre de categoría, como se muestra en la figura 7.


[![Los productos son inicialmente se ordenan por su nombre](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image15.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image14.png)

**Figura 6**: los productos están inicialmente se ordenan por su nombre ([haga clic aquí para ver la imagen a tamaño completo](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image16.png))


[![Los productos están ahora ordenados por categoría](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image18.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image17.png)

**Figura 7**: los productos están ahora ordenados por categoría ([haga clic aquí para ver la imagen a tamaño completo](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image19.png))


> [!NOTE]
> Haga clic en el botón de actualización hace que los datos automáticamente esté ordenada de nuevo porque se ha deshabilitado el estado de vista de repetidor s, por lo que el repetidor a enlazarse a su origen de datos en cada postback. Si se ha dejado el estado de vista de repetidor s está habilitado, cambiar la ordenación desplegable lista won t tenido ningún efecto en el criterio de ordenación. Para solucionar esto, cree un controlador de eventos para el botón actualizar s `Click` reenlace repetidor a su origen de datos y eventos (mediante una llamada a repetidor s `DataBind()` método).


## <a name="remembering-the-sort-expression-and-direction"></a>Teniendo en cuenta la expresión de ordenación y la dirección

Al crear un DataList o repetidor que se puede ordenar en una página que no ordenación relacionados con las devoluciones de datos pueden producirse, lo imperativo s que se recuerdan la expresión de ordenación y la dirección a través de las devoluciones de datos. Por ejemplo, imagine que se han actualizado repetidor en este tutorial debe incluir un botón Eliminar a cada objeto product. Cuando el usuario hace clic en el botón Eliminar d ejecutamos algo de código para eliminar el producto seleccionado y, a continuación, volver a enlazar los datos del repetidor. Si no se conservan los detalles de ordenación a través de la devolución de datos, los datos mostrados en pantalla se restablecerá el orden original.

Para este tutorial, DropDownList implícitamente guarda a la ordenación expresión y la dirección en su estado de vista para nosotros. Si estuviéramos utilizando una interfaz de ordenación diferente, uno con LinkButton digamos, que proporcionan las distintas opciones de ordenación d debemos tener en cuenta que para recordar el criterio de ordenación en las devoluciones de datos. Esto puede realizarse mediante el almacenamiento de los parámetros de ordenación en el estado de vista de página s, incluyendo el parámetro de ordenación en la cadena de consulta, o a través de otra técnica de persistencia de estado.

Ejemplos de futuras de este tutorial explorar cómo conservar los detalles de ordenación en el estado de vista de página s.

## <a name="step-5-adding-sorting-support-to-a-datalist-that-uses-default-paging"></a>Paso 5: Agregar compatibilidad para ordenar en un control DataList que usa la paginación predeterminada

En el [tutorial anterior](paging-report-data-in-a-datalist-or-repeater-control-cs.md) se examina cómo implementar la paginación de forma predeterminada con un control DataList. Permiten s ampliar este ejemplo anterior para incluir la posibilidad de ordenar los datos paginados. Comience abriendo la `SortingWithDefaultPaging.aspx` y `Paging.aspx` páginas en el `PagingSortingDataListRepeater` carpeta. Desde el `Paging.aspx` página, haga clic en el botón de origen para ver el marcado declarativo de la página s. Copie el texto seleccionado (consulte la figura 8) y péguelo en el marcado declarativo de `SortingWithDefaultPaging.aspx` entre el `<asp:Content>` etiquetas.


[![Replicar el marcado declarativo en la &lt;asp: Content&gt; etiquetas de Paging.aspx a SortingWithDefaultPaging.aspx](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image21.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image20.png)

**Figura 8**: replicar el marcado declarativo en la `<asp:Content>` etiquetas de `Paging.aspx` a `SortingWithDefaultPaging.aspx` ([haga clic aquí para ver la imagen a tamaño completo](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image22.png))


Una vez copiado el marcado declarativo, copie los métodos y propiedades en el `Paging.aspx` página de la clase de código subyacente de s a la clase de código subyacente para `SortingWithDefaultPaging.aspx`. A continuación, tómese un momento para ver la `SortingWithDefaultPaging.aspx` página en un explorador. Deben presentar la misma funcionalidad y apariencia a medida que `Paging.aspx`.

## <a name="enhancing-productsbll-to-include-a-default-paging-and-sorting-method"></a>Mejorar ProductsBLL para incluir el valor predeterminado de paginación y la ordenación (método)

En el tutorial anterior, creamos un `GetProductsAsPagedDataSource(pageIndex, pageSize)` método en el `ProductsBLL` clase que devuelve un `PagedDataSource` objeto. Esto `PagedDataSource` objeto rellenó con *todos los* de los productos (a través de las operaciones de asignación BLL `GetProducts()` método), pero cuando se enlaza al control DataList solo los registros correspondientes a los especificados *pageIndex* y *pageSize* se muestran los parámetros de entrada.

Anteriormente en este tutorial, hemos agregado compatibilidad con la ordenación mediante la especificación de la expresión de ordenación de las operaciones de asignación ObjectDataSource `Selecting` controlador de eventos. Esto funciona bien cuando el ObjectDataSource se devuelve un objeto que se puedan ordenar, al igual que el `ProductsDataTable` devuelto por la `GetProducts()` método. Sin embargo, el `PagedDataSource` objeto devuelto por la `GetProductsAsPagedDataSource` método no admite la ordenación de su origen de datos interna. En su lugar, se necesita ordenar los resultados devueltos por la `GetProducts()` método *antes de* colocamos el `PagedDataSource`.

Para ello, cree un nuevo método en el `ProductsBLL` (clase), `GetProductsSortedAsPagedDataSource(sortExpression, pageIndex, pageSize)`. Para ordenar el `ProductsDataTable` devuelto por la `GetProducts()` (método), especifique la `Sort` propiedad de su valor predeterminado `DataTableView`:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample5.cs)]

El `GetProductsSortedAsPagedDataSource` método difiere ligeramente de la `GetProductsAsPagedDataSource` método creado en el tutorial anterior. En concreto, `GetProductsSortedAsPagedDataSource` acepta un parámetro de entrada adicional `sortExpression` y asigna este valor a la `Sort` propiedad de la `ProductDataTable` s `DefaultView`. Unas pocas líneas de código más adelante, el `PagedDataSource` objeto s origen de datos se asigna el `ProductDataTable` s `DefaultView`.

## <a name="calling-the-getproductssortedaspageddatasource-method-and-specifying-the-value-for-the-sortexpression-input-parameter"></a>Una llamada al método GetProductsSortedAsPagedDataSource y especificando el valor para el parámetro de entrada SortExpression

Con el `GetProductsSortedAsPagedDataSource` método completo, el siguiente paso es proporcionar el valor para este parámetro. ObjectDataSource en `SortingWithDefaultPaging.aspx` está actualmente configurado para llamar a la `GetProductsAsPagedDataSource` método y pasa dos parámetros de entrada a través de sus dos `QueryStringParameters`, que se especifican en el `SelectParameters` colección. Estos dos `QueryStringParameters` indican que el origen de la `GetProductsAsPagedDataSource` método s *pageIndex* y *pageSize* parámetros proceden de los campos de cadena de consulta `pageIndex` y `pageSize`.

¿Desea actualizar la s ObjectDataSource `SelectMethod` propiedad por lo que TI, invoca el nuevo `GetProductsSortedAsPagedDataSource` método. A continuación, agregue un nuevo `QueryStringParameter` para que la *sortExpression* parámetro de entrada se obtiene acceso desde el campo de cadena de consulta `sortExpression`. Establecer el `QueryStringParameter` s `DefaultValue` ProductName.

Después de estos cambios, el marcado declarativo de ObjectDataSource s debería ser similar:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample6.aspx)]

En este momento, la `SortingWithDefaultPaging.aspx` página ordenará los resultados alfabéticamente por el nombre del producto (consulte la figura 9). Esto es porque, de forma predeterminada, se pasa un valor de ProductName como el `GetProductsSortedAsPagedDataSource` método s *sortExpression* parámetro.


[![De forma predeterminada, los resultados se ordenan por ProductName](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image24.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image23.png)

**Figura 9**: de forma predeterminada, los resultados se ordenan por `ProductName` ([haga clic aquí para ver la imagen a tamaño completo](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image25.png))


Si agrega manualmente un `sortExpression` campo de cadena de consulta como `SortingWithDefaultPaging.aspx?sortExpression=CategoryName` los resultados se ordenarán según los valores especificados `sortExpression`. Sin embargo, esto `sortExpression` parámetro no se incluye en la cadena de consulta cuando se mueve a otra página de datos. De hecho, al hacer clic en la página siguiente o último botones toma nos nuevo a `Paging.aspx`! Además, s hay actualmente ninguna ordenación de la interfaz. La única manera de que un usuario puede cambiar el criterio de ordenación de los datos paginados es mediante la manipulación de la cadena de consulta directamente.

## <a name="creating-the-sorting-interface"></a>Creación de la interfaz de ordenación

En primer lugar deberá actualizar el `RedirectUser` método para enviar al usuario `SortingWithDefaultPaging.aspx` (en lugar de `Paging.aspx`) e incluir el `sortExpression` valor en la cadena de consulta. También debemos agregar un solo lectura, nivel de página denominado `SortExpression` propiedad. Esta propiedad, similar a la `PageIndex` y `PageSize` propiedades creadas en el tutorial anterior, se devuelve el valor de la `sortExpression` campo de cadena de consulta, si existe y el valor predeterminado (ProductName) en caso contrario.

Actualmente el `RedirectUser` método acepta solo un único parámetro de entrada el índice de la página para mostrar. Sin embargo, puede haber veces cuando desea volver a redirigir al usuario a una página concreta de datos mediante una expresión de ordenación que no sea de qué s especificado en la cadena de consulta. En un momento, vamos a crear la interfaz de ordenación para esta página, que incluirá una serie de controles de botón Web para ordenar los datos por una columna especificada. Cuando se hace clic en uno de estos botones, queremos redirigirá al usuario pasando el valor de la expresión de ordenación adecuado. Para proporcionar esta funcionalidad, cree dos versiones de la `RedirectUser` método. La primera de ellas debe aceptar el índice de página que se muestran, mientras que la segunda acepta la expresión de índice y de ordenación de la página.


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample7.cs)]

En el primer ejemplo de este tutorial, creamos una interfaz ordenación usando DropDownList. En este ejemplo, s permiten usar tres controles de botón Web situados por encima del control DataList uno para la ordenación por `ProductName`, uno para `CategoryName`y otro para `SupplierName`. Agregue los tres controles de botón Web, establecer sus `ID` y `Text` propiedades correctamente:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample8.aspx)]

A continuación, cree un `Click` controlador de eventos para cada uno de ellos. Deben llamar los controladores de eventos el `RedirectUser` método, devolver el usuario a la primera página con la expresión de ordenación adecuado.


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample9.cs)]

Al visitar primero la página, los datos se ordenan alfabéticamente por nombre de producto (hacen referencia a la figura 9). Haga clic en el botón siguiente para avanzar a la segunda página de datos y, a continuación, haga clic en el criterio de ordenación mediante el botón de categoría. Esto nos devuelve a la primera página de datos, ordenados por nombre de categoría (consulte la figura 10). Del mismo modo, al hacer clic en el criterio de ordenación por botón proveedor ordena los datos por el proveedor a partir de la primera página de datos. Tal y como se paginan los datos a través de, se recuerda la opción de ordenación. Figura 11 muestra la página después de la ordenación por categoría y, a continuación, avanzar a la página decimotercer de datos.


[![Los productos se ordenan por categoría](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image27.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image26.png)

**Figura 10**: los productos se ordenan por categoría ([haga clic aquí para ver la imagen a tamaño completo](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image28.png))


[![La expresión de ordenación es recuerdan al paginar a través de los datos](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image30.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image29.png)

**Figura 11**: la expresión de ordenación es recuerdan al paginar a través de los datos ([haga clic aquí para ver la imagen a tamaño completo](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image31.png))


## <a name="step-6-custom-paging-through-records-in-a-repeater"></a>Paso 6: Paginación personalizada a través de registros en un repetidor

En el ejemplo de DataList examina en el paso 5 páginas a través de sus datos mediante la técnica de paginación predeterminada ineficaz. Al paginar a través de suficientemente grandes cantidades de datos, es imprescindible que se utiliza la paginación personalizada. En el [eficazmente paginar a través de grandes cantidades de datos](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md) y [ordenar datos personalizados de bloque paginado](../paging-and-sorting/sorting-custom-paged-data-cs.md) tutoriales, se examinan las diferencias entre predeterminado y paginación personalizada y métodos creados en la capa BLL para utilización de paginación personalizada y ordenar datos paginados personalizados. En concreto, en estos dos tutoriales anteriores, agregamos los siguientes tres métodos para la `ProductsBLL` clase:

- `GetProductsPaged(startRowIndex, maximumRows)`Devuelve un subconjunto específico de registros a partir de *startRowIndex* y no superar *maximumRows*.
- `GetProductsPagedAndSorted(sortExpression, startRowIndex, maximumRows)`Devuelve un subconjunto determinado de registros ordenados según los valores especificados *sortExpression* parámetro de entrada.
- `TotalNumberOfProducts()`proporciona el número total de registros en la `Products` tabla de base de datos.

Estos métodos se pueden utilizar la página y ordenar los datos mediante un control DataList o repetidor eficazmente. Para ilustrar esto, permiten s empiece por crear un control de repetidor con compatibilidad con la paginación personalizada; a continuación, vamos a agregar capacidades de ordenación.

Abra el `SortingWithCustomPaging.aspx` página en el `PagingSortingDataListRepeater` carpeta y agregue un repetidor a la página, estableciendo su `ID` propiedad a `Products`. Desde la etiqueta inteligente de repetidor s, crear un nuevo origen ObjectDataSource denominado `ProductsDataSource`. Configúrelo para seleccionar sus datos de la `ProductsBLL` clase s. `GetProductsPaged` método.


[![Configurar el ObjectDataSource para utilizar el método de GetProductsPaged ProductsBLL clase s.](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image33.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image32.png)

**Figura 12**: configurar el ObjectDataSource que se utilizan los `ProductsBLL` clase s `GetProductsPaged` método ([haga clic aquí para ver la imagen a tamaño completo](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image34.png))


Establece las listas de lista desplegable en la actualización, INSERCIÓN y eliminar las fichas (ninguno) y, a continuación, haga clic en el botón siguiente. El Asistente para configurar orígenes de datos ahora solicita los orígenes de la `GetProductsPaged` método s *startRowIndex* y *maximumRows* parámetros de entrada. En realidad, se omiten estos parámetros de entrada. En su lugar, el *startRowIndex* y *maximumRows* valores se pasará en el `Arguments` propiedad en las operaciones de asignación ObjectDataSource `Selecting` controlador de eventos, al igual que de cómo se especifica el *sortExpression* en esta demostración primer tutorial s. Por lo tanto, deje el parámetro source listas desplegables en el Asistente para configurar en ninguno.


[![Deje los orígenes de parámetro en None](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image36.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image35.png)

**Figura 13**: deje el conjunto de orígenes de parámetro en None ([haga clic aquí para ver la imagen a tamaño completo](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image37.png))


> [!NOTE]
> Hacer *no* establecer la s ObjectDataSource `EnablePaging` propiedad `true`. Esto hará que el ObjectDataSource incluir automáticamente su propio *startRowIndex* y *maximumRows* parámetros para el `SelectMethod` lista de parámetros de s existente. El `EnablePaging` propiedad es útil cuando el enlace personalizado paginado datos a un control GridView, DetailsView o FormView porque estos controles esperan cierto comportamiento de ObjectDataSource s solo está disponible cuando `EnablePaging` propiedad es `true`. Dado que tenemos que agregar manualmente la compatibilidad con la paginación para los DataList y repetidor, deje esta propiedad establecida en `false` (valor predeterminado), tal y como se le platos preparados en la funcionalidad necesaria directamente dentro de nuestra página de ASP.NET.


Por último, defina el repetidor s `ItemTemplate` para que se muestren el nombre de producto s, la categoría y el proveedor. Después de estos cambios, la sintaxis declarativa de s repetidor y ObjectDataSource debe ser similar al siguiente:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample10.aspx)]

Tómese un momento para visitar la página a través de un explorador y observe que no se devuelve ningún registro. Esto es porque se ha todavía para especificar el *startRowIndex* y *maximumRows* valores de parámetro; por lo tanto, los valores de 0 se han pasado en para ambos. Para especificar estos valores, cree un controlador de eventos para el s ObjectDataSource `Selecting` eventos y establezca estos parámetros valores mediante programación para valores codificados de forma rígida de 0 y 5, respectivamente:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample11.cs)]

Con este cambio, la página, cuando se ven a través de un explorador, muestra los cinco primeros productos.


[![Se muestran los primeros cinco registros](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image39.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image38.png)

**Figura 14**: se muestran los primeros cinco registros ([haga clic aquí para ver la imagen a tamaño completo](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image40.png))


> [!NOTE]
> Los productos enumerados en la figura 14 parecen estar ordenados por nombre de producto porque el `GetProductsPaged` procedimiento almacenado que realiza la consulta de paginación personalizada eficaz ordena los resultados por `ProductName`.


Con el fin de permitir al usuario paso a paso a través de las páginas, es necesario realizar un seguimiento del índice de fila inicial y el número máximo de filas y recordar estos valores a través de las devoluciones de datos. En el ejemplo de paginación de manera predeterminada se utilizan los campos de cadena de consulta para conservar estos valores; en esta demostración, permiten s conservar esta información en el estado de vista de página s. Cree las dos propiedades siguientes:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample12.cs)]

A continuación, actualice el código en el controlador de eventos de selección para que utilice la `StartRowIndex` y `MaximumRows` propiedades en lugar de los valores codificados de forma rígida de 0 y 5:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample13.cs)]

En este momento nuestra página sigue mostrando solo los primeros cinco registros. Sin embargo, con estas propiedades en su lugar, se re listo para crear la interfaz de paginación.

## <a name="adding-the-paging-interface"></a>Agregar la interfaz de paginación

S permiten utilizarán el mismo primero, anterior, siguiente, la última de paginación de la interfaz se utiliza en el ejemplo de paginación de forma predeterminada, incluida la Web de etiqueta se está viendo el control que muestra qué página de datos y existe el número total de páginas. Agregue las cuatro controles Web de botón y etiqueta situada debajo de repetidor.


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample14.aspx)]

A continuación, cree `Click` controladores de eventos para los cuatro botones. Cuando se hace clic en uno de estos botones, deberá actualizar el `StartRowIndex` y volver a enlazar los datos del repetidor. El código para los botones primero, anterior y siguiente es bastante sencillo, pero para el último botón ¿cómo determinamos el índice de fila de inicio de la última página de datos? Para calcular este índice, así como para poder determinar que si se deben habilitar los botones siguiente y último, necesitamos saber el número de registros en total que se va a se paginan a través de. Podemos determinar esto mediante una llamada a la `ProductsBLL` clase s. `TotalNumberOfProducts()` método. S permiten crear una propiedad de solo lectura, el nivel de página denominada `TotalRowCount` que devuelve los resultados de la `TotalNumberOfProducts()` método:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample15.cs)]

Con esta propiedad ahora podemos determinar el último índice de fila de inicio de la página s. En concreto, se s el resultado entero de la `TotalRowCount` menos 1 dividido por `MaximumRows`, multiplicado por `MaximumRows`. Ahora podemos escribir el `Click` controladores de eventos para los cuatro botones de la interfaz de paginación:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample16.cs)]

Por último, es necesario deshabilitar los botones primero y anterior en la interfaz de paginación al ver la primera página de datos y los botones siguiente y último al ver la última página. Para ello, agregue el código siguiente a la s ObjectDataSource `Selecting` controlador de eventos:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample17.cs)]

Después de agregar estos `Click` controladores de eventos y el código para habilitar o deshabilitar los elementos de la interfaz de paginación basados en el índice de fila actual de inicio, la página de prueba en un explorador. Tal y como muestra la figura 15, al primero visitar la página de la primera y botones anterior le están deshabilitados. Haga clic en siguiente se muestra la segunda página de datos, mientras que al hacer clic en la última muestra la última página (vea las figuras 16 y 17). Al ver la última página de datos se deshabilitan los botones de la siguiente y el último.


[![Los botones anterior y último se deshabilitan al ver la primera página de productos](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image42.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image41.png)

**Figura 15**: botones último y la anterior se deshabilitan al ver la primera página de productos ([haga clic aquí para ver la imagen a tamaño completo](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image43.png))


[![La segunda página de productos son muestra](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image45.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image44.png)

**Figura 16**: la segunda página de los productos son muestra ([haga clic aquí para ver la imagen a tamaño completo](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image46.png))


[![Al hacer clic en el último muestra la última página de datos](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image48.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image47.png)

**Figura 17**: haga clic en la última muestra la última página de datos ([haga clic aquí para ver la imagen a tamaño completo](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image49.png))


## <a name="step-7-including-sorting-support-with-the-custom-paged-repeater"></a>Paso 7: Incluida la compatibilidad con la opción de instalación para ordenar paginado repetidor

Ahora que se ha implementado la paginación personalizada, re listo para incluir la ordenación admitimos. El `ProductsBLL` clase s `GetProductsPagedAndSorted` método tiene el mismo *startRowIndex* y *maximumRows* parámetros como de entrada `GetProductsPaged`, pero permite más  *sortExpression* parámetro de entrada. Para usar el `GetProductsPagedAndSorted` método de `SortingWithCustomPaging.aspx`, tenemos que realizar los pasos siguientes:

1. Cambiar la s ObjectDataSource `SelectMethod` propiedad de `GetProductsPaged` a `GetProductsPagedAndSorted`.
2. Agregar un *sortExpression* `Parameter` objeto para las operaciones de asignación ObjectDataSource `SelectParameters` colección.
3. Crear una privada, el nivel de página `SortExpression` propiedad que se conserva su valor en las devoluciones a través del estado de vista de página s.
4. ¿Desea actualizar la s ObjectDataSource `Selecting` controlador de eventos para asignar la s ObjectDataSource *sortExpression* parámetro el valor del nivel de página `SortExpression` propiedad.
5. Crear la interfaz de ordenación.

Iniciar mediante la actualización de las operaciones de asignación ObjectDataSource `SelectMethod` propiedad y agregando un *sortExpression* `Parameter`. Asegúrese de que el *sortExpression* `Parameter` s `Type` propiedad está establecida en `String`. Después de completar estas tareas en primer lugar dos, el marcado declarativo de ObjectDataSource s debería ser similar al siguiente:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample18.aspx)]

A continuación, necesitamos un nivel de página `SortExpression` propiedad cuyo valor se serializa en el estado de vista. Si no se ha establecido ningún valor de la expresión de ordenación, utilice ProductName como valor predeterminado:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample19.cs)]

Antes de que el ObjectDataSource invoca el `GetProductsPagedAndSorted` método es necesario establecer la *sortExpression* `Parameter` en el valor de la `SortExpression` propiedad. En el `Selecting` controlador de eventos, agregue la siguiente línea de código:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample20.cs)]

Todo lo que queda es implementar la interfaz de ordenación. Igual que hicimos en el último ejemplo, permiten s tiene la interfaz de ordenación que se implementa mediante tres controles de botón Web que permiten al usuario ordenar los resultados por nombre de producto, categoría o proveedor.


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample21.aspx)]

Crear `Click` controladores de eventos para estos tres controles de botón. En el evento de restablecimiento de controlador, el `StartRowIndex` en 0, establezca el `SortExpression` al valor apropiado y vuelva a enlazar los datos a repetidor:


[!code-csharp[Main](sorting-data-in-a-datalist-or-repeater-control-cs/samples/sample22.cs)]

S todo es a él. Mientras hay una serie de pasos para obtener la paginación personalizada y la ordenación implementado, los pasos son muy similares a las necesarias para la paginación de manera predeterminada. Figura 18 muestra los productos al ver la última página de datos cuando se ordenan por categoría.


[![Se muestra la última página de datos, Sorted por categoría,](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image51.png)](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image50.png)

**Figura 18**: se muestra la última página de datos de, Sorted por categoría, ([haga clic aquí para ver la imagen a tamaño completo](sorting-data-in-a-datalist-or-repeater-control-cs/_static/image52.png))


> [!NOTE]
> En los ejemplos anteriores, al ordenar por el proveedor que NombreProveedor se utiliza como la expresión de ordenación. Sin embargo, para la implementación de paginación personalizada, es necesario usar CompanyName. Esto es porque el procedimiento almacenado responsable de implementar la paginación personalizada `GetProductsPagedAndSorted` pasa la expresión de ordenación en el `ROW_NUMBER()` palabra clave, el `ROW_NUMBER()` palabra clave requiere el nombre de columna real en lugar de un alias. Por lo tanto, debemos usar `CompanyName` (el nombre de la columna en la `Suppliers` tabla) en lugar de lo alias que se usa en la `SELECT` consulta (`SupplierName`) para la expresión de ordenación.


## <a name="summary"></a>Resumen

Ni el DataList ni repetidor ofrecen compatibilidad integrada con la ordenación, pero con un poco de código y una interfaz de ordenación personalizada, puede agregar esta funcionalidad. Al implementar la ordenación, pero no de paginación, la expresión de ordenación se puede especificar mediante el `DataSourceSelectArguments` objeto pasado en las operaciones de asignación ObjectDataSource `Select` método. Esto `DataSourceSelectArguments` objeto s `SortExpression` propiedad se puede asignar en las operaciones de asignación ObjectDataSource `Selecting` controlador de eventos.

Para agregar capacidades de ordenación a un DataList o repetidor que ya proporciona la compatibilidad con la paginación, el enfoque más sencillo consiste en Personalizar la capa de lógica de negocios para que incluya un método que acepta una expresión de ordenación. Esta información, a continuación, se puede pasar en un parámetro en las operaciones de asignación ObjectDataSource `SelectParameters`.

Este tutorial completa el examen de paginación y la ordenación con los controles DataList y repetidor. Nuestro tutorial siguiente y última examinará cómo agregar controles de botón Web a las plantillas de s DataList y repetidor con el fin de proporcionar funcionalidades personalizadas, iniciada por el usuario en una base por cada elemento.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Revisor inicial para este tutorial era David Suru. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](paging-report-data-in-a-datalist-or-repeater-control-cs.md)
[Siguiente](paging-report-data-in-a-datalist-or-repeater-control-vb.md)
