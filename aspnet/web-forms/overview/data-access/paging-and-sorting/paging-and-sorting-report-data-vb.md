---
uid: web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-vb
title: Paginar y ordenar datos (VB) de informes | Microsoft Docs
author: rick-anderson
description: Paginación y clasificación son dos características muy comunes al mostrar datos en una aplicación en línea. En este tutorial echaremos un primer vistazo al agregar ordenación y...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: b895e37e-0e69-45cc-a7e4-17ddd2e1b38d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 6de1f78bab795ef191553bf0d58b3b29c26d7d85
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387655"
---
<a name="paging-and-sorting-report-data-vb"></a>Paginación y ordenación de datos de informe (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_24_VB.exe) o [descargar PDF](paging-and-sorting-report-data-vb/_static/datatutorial24vb1.pdf)

> Paginación y clasificación son dos características muy comunes al mostrar datos en una aplicación en línea. En este tutorial, echaremos un primer vistazo a la adición de ordenación y paginación a nuestros informes, que se tratará en tutoriales futuros.


## <a name="introduction"></a>Introducción

Paginación y clasificación son dos características muy comunes al mostrar datos en una aplicación en línea. Por ejemplo, cuando se buscan los libros en pantalla ASP.NET en una librería en línea, puede haber cientos de libros, pero el informe con una lista de los resultados de búsqueda muestra a solo diez coincidencias por página. Además, se pueden ordenar los resultados por título, precio, recuento de páginas, nombre del autor y así sucesivamente. Mientras el pasado 23 tutoriales han examinado cómo crear una gran variedad de informes, incluidas las interfaces que permiten agregar, editar y eliminar datos, se ve no examinado cómo ordenar los datos y la única ejemplos de paginación se ve visto llevan el DetailsView y FormView controles.

En este tutorial, veremos cómo agregar ordenación y paginación a nuestros informes, que pueden realizarse con sólo marcar algunas casillas de verificación. Lamentablemente, esta implementación simplista tiene sus inconvenientes que deja un poco que desear la interfaz de ordenación y las rutinas de paginación no están diseñadas para paginar eficazmente a través de grandes conjuntos de resultados. Tutoriales futuros explorará cómo superar las limitaciones de la-de-fábrica paginación y ordenación de las soluciones.

## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>Paso 1: Agregar la paginación y ordenación páginas Web del Tutorial

Antes de empezar este tutorial, permiten s primero Tómese un momento para agregar las páginas ASP.NET que necesitaremos para este tutorial y los tres siguientes. Empiece por crear una nueva carpeta en el proyecto denominado `PagingAndSorting`. A continuación, agregue las siguientes cinco páginas ASP.NET en esta carpeta, configurado para usar la página principal de mantenerlos todos `Site.master`:

- `Default.aspx`
- `SimplePagingSorting.aspx`
- `EfficientPaging.aspx`
- `SortParameter.aspx`
- `CustomSortingUI.aspx`


![Cree una carpeta PagingAndSorting y agregar las páginas del Tutorial de ASP.NET](paging-and-sorting-report-data-vb/_static/image1.png)

**Figura 1**: cree una carpeta PagingAndSorting y agregar las páginas del Tutorial de ASP.NET


A continuación, abra el `Default.aspx` página y arrastre el `SectionLevelTutorialListing.ascx` Control de usuario desde el `UserControls` carpeta a la superficie de diseño. Este Control de usuario, que hemos creado en el [páginas maestras y navegación del sitio](../introduction/master-pages-and-site-navigation-vb.md) tutorial, enumera el mapa del sitio y los tutoriales se muestra en la sección actual en una lista con viñetas.


![Agregar el Control de usuario SectionLevelTutorialListing.ascx a Default.aspx](paging-and-sorting-report-data-vb/_static/image2.png)

**Figura 2**: agregar el Control de usuario SectionLevelTutorialListing.ascx a Default.aspx


Con el fin de que la lista con viñetas muestre la paginación y ordenación tutoriales que crearemos, deberá agregarlos al mapa del sitio. Abra el `Web.sitemap` archivo y agregue el siguiente marcado después de las marcas de nodo de mapa de sitio edición, inserción y eliminación:


[!code-xml[Main](paging-and-sorting-report-data-vb/samples/sample1.xml)]


![Actualizar el mapa del sitio para incluir las nuevas páginas de ASP.NET](paging-and-sorting-report-data-vb/_static/image3.png)

**Figura 3**: actualizar el mapa del sitio para incluir las nuevas páginas de ASP.NET


## <a name="step-2-displaying-product-information-in-a-gridview"></a>Paso 2: Mostrar información de producto en un control GridView

Antes de que realmente se implemente la paginación y capacidades de ordenación, permiten s en primer lugar cree un estándar no-srotable, no paginable GridView que muestra la información del producto. Esta es una tarea se ve hecho muchas veces a lo largo de esta serie de tutoriales para que estos pasos debe estar familiarizado. Comience abriendo la `SimplePagingSorting.aspx` página y arrastre un control GridView del cuadro de herramientas hasta el diseñador, establecer su `ID` propiedad `Products`. A continuación, cree un nuevo origen ObjectDataSource que usa la clase ProductsBLL s `GetProducts()` método para devolver toda la información de producto.


![Recuperar información acerca de todos los productos con el método GetProducts()](paging-and-sorting-report-data-vb/_static/image4.png)

**Figura 4**: recuperar información acerca de todos los productos con el método GetProducts()


Puesto que este informe es un informe de solo lectura, hay s no tiene que asignar la s ObjectDataSource `Insert()`, `Update()`, o `Delete()` métodos correspondiente `ProductsBLL` métodos; por lo tanto, elegir para la actualización, INSERCIÓN, (ninguno) en la lista desplegable ELIMINAR pestañas y.


![Elija el (ninguno) opción en la lista desplegable en la actualización, INSERCIÓN y eliminar las fichas](paging-and-sorting-report-data-vb/_static/image5.png)

**Figura 5**: elija el (ninguno) opción en la lista desplegable en la actualización, INSERCIÓN y eliminar las fichas


A continuación, permiten s personalizar los campos de s GridView para que se muestren solo los nombres de productos, proveedores, categorías, precios y Estados no incluidos. Además, puede aplicar formato a cualquier nivel de campo cambia, por ejemplo, ajustar el `HeaderText` propiedades o dar formato a los precios como una moneda. Después de estos cambios, el marcado declarativo de GridView s debe ser similar al siguiente:


[!code-aspx[Main](paging-and-sorting-report-data-vb/samples/sample2.aspx)]

Figura 6 muestra nuestro progreso hasta ahora, cuando se ve mediante un explorador. Tenga en cuenta que la página muestra todos los productos en una pantalla, que muestra cada nombre de producto s, categoría, proveedor, precio y no incluye el estado.


[![Cada uno de los productos enumerados](paging-and-sorting-report-data-vb/_static/image7.png)](paging-and-sorting-report-data-vb/_static/image6.png)

**Figura 6**: se enumeran cada uno de los productos ([haga clic aquí para ver imagen en tamaño completo](paging-and-sorting-report-data-vb/_static/image8.png))


## <a name="step-3-adding-paging-support"></a>Paso 3: Agregar compatibilidad con la paginación

Enumerar *todas* de los productos en una pantalla puede dar lugar a la sobrecarga de información para el usuario examinando los datos. Para ayudar a que los resultados sean más fáciles de administrar, podemos dividir los datos en las páginas de datos más pequeños y permitir al usuario paso a paso a través de la página datos a la vez. Para lograr esto simplemente marque la casilla de verificación Habilitar paginación de la etiqueta inteligente de GridView s (Esto establece la s GridView [ `AllowPaging` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowpaging.aspx) a `true`).


[![Active la casilla de verificación Habilitar paginación para agregar compatibilidad con la paginación](paging-and-sorting-report-data-vb/_static/image10.png)](paging-and-sorting-report-data-vb/_static/image9.png)

**Figura 7**: Active la casilla de la paginación de habilitar para agregar compatibilidad de paginación ([haga clic aquí para ver imagen en tamaño completo](paging-and-sorting-report-data-vb/_static/image11.png))


Habilitar paginación limita el número de registros que se muestran por página y se agrega un *interfaz de paginación* en GridView. La interfaz de paginación de forma predeterminada, que se muestra en la figura 7, es una serie de números de página, lo que permite al usuario desplazarse rápidamente de una página de datos a otro. Esta interfaz de paginación debe resultarle familiar, como se ve visto al agregar compatibilidad con la paginación para los controles DetailsView y FormView en tutoriales anteriores.

El DetailsView y FormView controles muestran solo un único registro por página. El control GridView, sin embargo, consulta su [ `PageSize` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.pagesize.aspx) para determinar cuántos registros para mostrar por página (esta propiedad tiene como valor predeterminado un valor de 10).

Esta interfaz de paginación GridView, DetailsView y FormView s puede personalizarse mediante las siguientes propiedades:

- `PagerStyle` indica la información de estilo para la interfaz de paginación; puede especificar opciones como `BackColor`, `ForeColor`, `CssClass`, `HorizontalAlign`, y así sucesivamente.
- `PagerSettings` contiene una serie de propiedades que puede personalizar la funcionalidad de la interfaz de paginación. `PageButtonCount` indica el número máximo de números de página numérico que se muestra en la interfaz de paginación (el valor predeterminado es 10); el [ `Mode` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pagersettings.mode.aspx) indica cómo la interfaz de paginación funciona y se puede establecer en: 

    - `NextPrevious` muestra un botones siguiente y anterior, que permite al usuario paso a paso o retroceder una página a la vez
    - `NextPreviousFirstLast` Además de los botones siguiente y anterior, primero y último botones también se incluyen, que permite al usuario desplazarse rápidamente a la primera o última página de datos
    - `Numeric` muestra una serie de números de página, que permite al usuario inmediatamente salte a cualquier página
    - `NumericFirstLast` Además de los números de página, incluye los botones First y Last, que permite al usuario desplazarse rápidamente a la primera o última página de datos. solo se muestran los botones de primer o último, si no caben todos los números de página numérica

Además, el control GridView, DetailsView y FormView todas ofrecen la `PageIndex` y `PageCount` propiedades, que indican la página actual que se está viendo y el número total de páginas de datos, respectivamente. El `PageIndex` propiedad se indiza empezando por 0, lo que significa que al ver la primera página de datos `PageIndex` será igual a 0. `PageCount`, por otro lado, se inicia el recuento en 1, lo que significa que `PageIndex` está limitado a los valores comprendidos entre 0 y `PageCount - 1`.

Permiten s dedique un momento para mejorar la apariencia predeterminada de nuestra interfaz de paginación s GridView. En concreto, permite que s tengan la interfaz de paginación alineado a la derecha con un fondo gris claro. En lugar de establecer estas propiedades directamente a través de las operaciones de asignación GridView `PagerStyle` propiedad, s permiten crear una clase CSS en `Styles.css` denominado `PagerRowStyle` y, a continuación, asigne el `PagerStyle` s `CssClass` propiedad a través de nuestro lema. Comience abriendo `Styles.css` y definición de clase agregando el código CSS siguiente:


[!code-css[Main](paging-and-sorting-report-data-vb/samples/sample3.css)]

A continuación, abra el `GridView.skin` de archivos en el `DataWebControls` carpeta dentro de la `App_Themes` carpeta. Como se explicó en la *páginas maestras y navegación del sitio* tutoriales, máscara archivos pueden utilizarse para especificar los valores de propiedad predeterminados para un control Web. Por lo tanto, aumentar la configuración existente para incluir la configuración de la `PagerStyle` s `CssClass` propiedad `PagerRowStyle`. Además, s permiten configurar la interfaz de paginación para mostrar a lo sumo los botones de cinco páginas numéricas con el `NumericFirstLast` interfaz de paginación.


[!code-aspx[Main](paging-and-sorting-report-data-vb/samples/sample4.aspx)]

## <a name="the-paging-user-experience"></a>La experiencia de usuario de paginación

Figura 8 se muestra la página web cuando visita a través del explorador después la casilla de verificación Habilitar paginación de GridView s se ha protegido y el `PagerStyle` y `PagerSettings` las configuraciones realizadas mediante la `GridView.skin` archivo. Tenga en cuenta cómo solo diez registros se muestran y la interfaz de paginación indica que nos estamos viendo la primera página de datos.


[![Con la paginación está habilitada, se muestran solo un subconjunto de los registros a la vez](paging-and-sorting-report-data-vb/_static/image13.png)](paging-and-sorting-report-data-vb/_static/image12.png)

**Figura 8**: con paginación habilitado, se muestran solo un subconjunto de los registros a la vez ([haga clic aquí para ver imagen en tamaño completo](paging-and-sorting-report-data-vb/_static/image14.png))


Cuando el usuario hace clic en uno de los números de página en la interfaz de paginación, una devolución de datos que habrá trastornos y recarga de la página que muestra que la página s registros solicitados. Figura 9 muestra los resultados tras la participación para ver la última página de datos. Tenga en cuenta que la página final tiene solo un registro; Esto es porque hay 81 registros en total, lo que resulta en ocho páginas de 10 registros por página además una página con un único registro.


[![Al hacer clic en un número de página hace que una devolución de datos y muestra el subconjunto adecuado de registros](paging-and-sorting-report-data-vb/_static/image16.png)](paging-and-sorting-report-data-vb/_static/image15.png)

**Figura 9**: al hacer clic en un número de página hace que una devolución de datos y muestra el subconjunto de registros adecuados ([haga clic aquí para ver imagen en tamaño completo](paging-and-sorting-report-data-vb/_static/image17.png))


## <a name="paging-s-server-side-workflow"></a>El flujo de trabajo de paginación s del servidor

Cuando el usuario final hace clic en un botón de la interfaz de paginación, una devolución de datos que habrá trastornos y comienza el flujo de trabajo de servidor siguiente:

1. El control GridView s (o DetailsView o FormView) `PageIndexChanging` desencadena el evento
2. Volver a solicitudes de ObjectDataSource *todos los* de los datos de la capa BLL; la s GridView `PageIndex` y `PageSize` se usan los valores de propiedad para determinar qué registros devueltos desde la capa BLL deben mostrarse en el control GridView
3. Las operaciones de asignación GridView `PageIndexChanged` desencadena el evento

En el paso 2, el origen ObjectDataSource solicita volver a todos los datos desde su origen de datos. Este estilo de paginación se conoce comúnmente como *paginación predeterminada*, tal y como el comportamiento de paginación s utiliza de forma predeterminada al establecer el `AllowPaging` propiedad `true`. No tiene valor predeterminado paginación otra vez los datos de control Web recupera todos los registros para cada página de datos, aunque solo un subconjunto de registros se representan realmente en el código HTML que s que se envía al explorador. A menos que la base de datos se almacena en caché por el nivel de lógica empresarial o ObjectDataSource, paginación predeterminada es viable para conjuntos de resultados lo suficientemente grande o para aplicaciones web con muchos usuarios simultáneos.

En el siguiente tutorial examinaremos cómo implementar *paginación personalizada*. Con la paginación personalizada puede indicar específicamente a ObjectDataSource para recuperar solo el conjunto de registros necesarios para la página solicitada de datos. Como puede imaginar, la paginación personalizada mejora considerablemente la eficacia de paginación a través de grandes conjuntos de resultados.

> [!NOTE]
> Mientras que la paginación predeterminada no es adecuada cuando la paginación a través de conjuntos de resultados lo suficientemente grande, o para sitios con muchos usuarios simultáneos, tenga en cuenta que la paginación personalizada requiere cambios más esfuerzo para implementar y no es tan sencilla como activar una casilla (como el valor predeterminado es paginación). Por lo tanto, la paginación predeterminada puede ser la opción ideal para sitios Web pequeños, el tráfico de baja o cuando se establece paginación de resultados relativamente pequeño, tal y como s mucho más fácil y rápido implementar.


Por ejemplo, si se sabe que nunca tendremos más de 100 productos en nuestra base de datos, la ganancia de rendimiento mínimo disfrutada por la paginación personalizada es probable que se desplaza por el esfuerzo necesario para implementarla. Si, sin embargo, un día tengamos miles o decenas de miles de productos, *no* implementar la paginación personalizada podría dificultar en gran medida la escalabilidad de nuestra aplicación.

## <a name="step-4-customizing-the-paging-experience"></a>Paso 4: Personalizar la experiencia de paginación

Los controles Web de datos proporcionan una serie de propiedades que puede usarse para mejorar la experiencia de usuario s paginación. El `PageCount` propiedad, por ejemplo, indica cuántas páginas total no existe, mientras que el `PageIndex` propiedad indica la página actual que se visita y se puede establecer para desplazarse rápidamente de un usuario a una página específica. Para ilustrar cómo usar estas propiedades para mejorar la experiencia del usuario s paginación, permiten s Agregar una etiqueta de control Web de nuestra página que informa al usuario qué página re visita actualmente, junto con un control DropDownList que les permite saltar rápidamente a una página dada .

En primer lugar, agregue un control Web de la etiqueta a la página, establezca su `ID` propiedad `PagingInformation`y borrar su `Text` propiedad. A continuación, cree un controlador de eventos para el s GridView `DataBound` eventos y agregue el código siguiente:


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample5.vb)]

Asigna este controlador de eventos el `PagingInformation` etiqueta s `Text` propiedad a un mensaje que informa al usuario la página que actualmente están visitando `Products.PageIndex + 1` fuera del número total de páginas `Products.PageCount` (se agrega 1 a la `Products.PageIndex` propiedad porque `PageIndex` se indizan empezando por 0). Elegí la asignar esta etiqueta s `Text` propiedad en el `DataBound` controlador de eventos en contraposición a la `PageIndexChanged` controlador de eventos porque el `DataBound` evento se desencadena cada vez que se enlazan datos al control GridView, mientras que el `PageIndexChanged` sólo el controlador de eventos se desencadena cuando se cambia el índice de página. Cuando el control GridView inicialmente tiene datos enlazados en la primera página visita, el `PageIndexChanging` se activan t (mientras que el `DataBound` evento).

Con esta versión, el usuario ahora se muestra un mensaje que indica qué página está visitando y es el número total de páginas de datos no existe.


[![Se muestran el número de página actual y el número Total de páginas](paging-and-sorting-report-data-vb/_static/image19.png)](paging-and-sorting-report-data-vb/_static/image18.png)

**Figura 10**: el número de página actual y el número Total de páginas se muestran ([haga clic aquí para ver imagen en tamaño completo](paging-and-sorting-report-data-vb/_static/image20.png))


Además del control de etiqueta permiten s también agregar un control DropDownList que enumera los números de página en el control GridView con la página está viendo actualmente seleccionada. La idea aquí es que el usuario puede desplazarse rápidamente desde la página actual a otro simplemente seleccionando el nuevo índice de página de la lista desplegable. Empiece agregando un DropDownList hasta el diseñador, establecer su `ID` propiedad `PageList` y activa la opción de Habilitar AutoPostBack desde su etiqueta inteligente.

A continuación, volver a la `DataBound` controlador de eventos y agregue el código siguiente:


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample6.vb)]

Este código comienza desactivando los elementos de la `PageList` DropDownList. Esto puede parecer superflua, puesto que una t no sería espera que el número de páginas que se va a cambiar, pero otros usuarios pueden usar simultáneamente el sistema, agregar o quitar registros de la `Products` tabla. Como las inserciones o eliminaciones podrían alterar el número de páginas de datos.

A continuación, necesitamos crear de nuevo los números de página y tiene el que se asigna a la GridView actual `PageIndex` seleccionada de forma predeterminada. Lograrlo con un bucle desde 0 hasta `PageCount - 1`, agregando un nuevo `ListItem` en cada iteración y estableciendo su `Selected` propiedad en true si el índice de iteración actual es igual a la s GridView `PageIndex` propiedad.

Por último, necesitamos crear un controlador de eventos para el s DropDownList `SelectedIndexChanged` evento, que se desencadena cada vez que el usuario seleccione un elemento diferente en la lista. Para crear este controlador de eventos, simplemente haga doble clic en DropDownList en el diseñador, a continuación, agregue el código siguiente:


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample7.vb)]

Como se muestra en la figura 11, cambiando solamente la s GridView `PageIndex` propiedad hace que los datos que se va a enlazar a GridView. En la s GridView `DataBound` controlador de eventos, DropDownList adecuado `ListItem` está seleccionada.


[![El usuario es dirigirá automáticamente a la sexta página al seleccionar el elemento de lista desplegable de página 6](paging-and-sorting-report-data-vb/_static/image22.png)](paging-and-sorting-report-data-vb/_static/image21.png)

**Figura 11**: el usuario es dirigirá automáticamente a la sexta página al seleccionar el elemento de lista desplegable de página 6 ([haga clic aquí para ver imagen en tamaño completo](paging-and-sorting-report-data-vb/_static/image23.png))


## <a name="step-5-adding-bi-directional-sorting-support"></a>Paso 5: Agregar compatibilidad con la ordenación bidireccionales

Agregar compatibilidad con la ordenación bidireccional es tan sencillo como agregar compatibilidad con la paginación simplemente marque la opción de habilitar la ordenación de la etiqueta inteligente de s de GridView (que establece la s GridView [ `AllowSorting` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowsorting.aspx) a `true`). Esto representa cada uno de los encabezados de los campos de s GridView como LinkButtons que, al hacer clic en, producen una devolución de datos y devolver los datos ordenados por la columna seleccionada en orden ascendente. Vuelva a hacer clic en el mismo encabezado LinkButton reordena los datos en orden descendente.

> [!NOTE]
> Si usa un nivel de acceso de datos personalizado en lugar de un conjunto de datos con tipo, no puede tener una opción de habilitar la ordenación en la etiqueta inteligente de s GridView. Solo GridView enlazado a orígenes de datos que admiten de forma nativa ordenación tiene esta casilla de verificación disponible. El conjunto de datos con tipo proporciona compatibilidad con la ordenación del cuadro, ya que la DataTable de ADO.NET proporciona un `Sort` método que, cuando se invoca, ordena la s DataTable DataRows utilizando los criterios especificados.


Si la capa DAL no devuelve objetos que admiten la ordenación que se debe configurar el origen ObjectDataSource para pasar información de ordenación a la capa de lógica empresarial, que puede ordenar los datos o hacer que los datos ordenada en forma nativa por la capa DAL. Exploraremos cómo ordenar los datos en la lógica de negocios y los niveles de acceso a datos en un futuro tutorial.

La ordenación LinkButtons se representan como hipervínculos HTML, cuyos colores actuales (azules para un vínculo no visitado y rojo oscuro para un vínculo visitado) entren en conflicto con el color de fondo de la fila de encabezado. En su lugar, permita que s tiene todos los vínculos del encabezado de fila muestran en blanco, independientemente de si se ve sido visitado o no. Esto puede realizarse agregando lo siguiente a la `Styles.css` clase:


[!code-css[Main](paging-and-sorting-report-data-vb/samples/sample8.css)]

Esta sintaxis indica que se utilice texto en blanco al mostrar los hipervínculos dentro de un elemento que usa la clase HeaderStyle.

Después de esta adición de CSS, cuando se visita la página a través del explorador de la pantalla debe ser similar a la figura 12. En concreto, la figura 12 muestra los resultados después de que se ha hecho clic el vínculo de encabezado s del campo de precio.


[![Los resultados están ordenados por el precio unitario en orden ascendente](paging-and-sorting-report-data-vb/_static/image25.png)](paging-and-sorting-report-data-vb/_static/image24.png)

**Figura 12**: los resultados están ordenados por el precio unitario en orden ascendente ([haga clic aquí para ver imagen en tamaño completo](paging-and-sorting-report-data-vb/_static/image26.png))


## <a name="examining-the-sorting-workflow"></a>Examinar el flujo de trabajo de ordenación

GridView todos los campos del BoundField, CampoCasillaVerificación, TemplateField y así sucesivamente tienen un `SortExpression` propiedad que indica la expresión que se debe usar para ordenar los datos cuando se hace clic en el campo s vínculo del encabezado de ordenación. El control GridView también tiene un `SortExpression` propiedad. Cuando un encabezado de ordenación que se hace clic en LinkButton, GridView asigna ese campo s `SortExpression` valor a su `SortExpression` propiedad. A continuación, los datos se vuelve a recuperarse ObjectDataSource y se ordenan según la s GridView `SortExpression` propiedad. La siguiente lista detalla la secuencia de pasos que ocurre cuando un usuario final ordena los datos en un control GridView:

1. Las operaciones de asignación GridView [evento Sorting](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorting(VS.80).aspx) se activa
2. Las operaciones de asignación GridView [ `SortExpression` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx) está establecido en el `SortExpression` del campo cuyo encabezado de ordenación que se hizo clic en LinkButton
3. Volver a recupera todos los datos de la capa BLL de ObjectDataSource y, a continuación, ordena los datos mediante la s de GridView `SortExpression`
4. Las operaciones de asignación GridView `PageIndex` propiedad se restablece a 0, lo que significa que al ordenar el usuario se devuelve a la primera página de datos (suponiendo que se ha implementado la compatibilidad con la paginación)
5. Las operaciones de asignación GridView [ `Sorted` evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorted(VS.80).aspx) se activa

Al igual que con la paginación de forma predeterminada, la opción de ordenación predeterminada recupera volver a *todas* los registros de la capa BLL. Cuando se usa la ordenación sin paginar o cuando se usa la ordenación con no predeterminado de paginación, allí s ninguna forma de sortear este impacto (aparte de la base de datos de almacenamiento en caché) en el rendimiento. Sin embargo, como veremos en un futuro tutorial lo posible ordenar los datos de forma eficaz cuando se usa paginación personalizada.

Al enlazar un origen ObjectDataSource en GridView a través de la lista desplegable en la etiqueta inteligente de GridView s, cada campo de GridView tiene automáticamente su `SortExpression` propiedad asignada al nombre del campo de datos en el `ProductsRow` clase. Por ejemplo, el `ProductName` BoundField s `SortExpression` está establecido en `ProductName`, como se muestra en el marcado declarativo siguiente:


[!code-aspx[Main](paging-and-sorting-report-data-vb/samples/sample9.aspx)]

Un campo puede configurarse para que lo s no ordenable borrando su `SortExpression` propiedad (asignándolo a una cadena vacía). Para ilustrar esto, imagine que nos t debería permitir que nuestros clientes ordenar nuestros productos por precio. El `UnitPrice` BoundField s `SortExpression` propiedad se puede quitar o desde el marcado declarativo mediante el cuadro de diálogo campos (que se puede acceder haciendo clic en el vínculo Editar columnas en la etiqueta inteligente de GridView s).


![Los resultados están ordenados por el precio unitario en orden ascendente](paging-and-sorting-report-data-vb/_static/image27.png)

**Figura 13**: los resultados están ordenados por el precio unitario en orden ascendente


Una vez el `SortExpression` se ha quitado la propiedad para el `UnitPrice` BoundField, el encabezado se representa como texto en lugar de como un vínculo, con lo que impide que los usuarios ordenar los datos por el precio.


[![Cuando se quita la propiedad SortExpression, los usuarios ya No pueden ordenar los productos por precio](paging-and-sorting-report-data-vb/_static/image29.png)](paging-and-sorting-report-data-vb/_static/image28.png)

**Figura 14**: mediante la eliminación de la propiedad SortExpression, los usuarios ya No pueden ordenar los productos por precio ([haga clic aquí para ver imagen en tamaño completo](paging-and-sorting-report-data-vb/_static/image30.png))


## <a name="programmatically-sorting-the-gridview"></a>Ordenación mediante programación el control GridView

También puede ordenar el contenido del control GridView mediante programación utilizando la s GridView [ `Sort` método](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sort.aspx). Basta con pasar el `SortExpression` valor para ordenar por junto con el [ `SortDirection` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sortdirection.aspx) (`Ascending` o `Descending`), y los datos de s GridView estará ordenados de nuevo.

Imagine que el motivo se ha desactivado la ordenación por la `UnitPrice` porque se preocupa de que nuestros clientes comprarían simplemente solo los productos de precios más bajos. Sin embargo, deseamos animarles a comprar los productos más caros, por lo que nos d como que puedan ordenar los productos por precio, pero solo desde el precio mayor al menos.

Para lograr esto agregar un control de botón Web a la página, establezca su `ID` propiedad `SortPriceDescending`y su `Text` propiedad para ordenar por precio. A continuación, cree un controlador de eventos para el botón s `Click` eventos haciendo doble clic en el control de botón en el diseñador. Agregue el código siguiente al controlador de eventos:


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample10.vb)]

Al hacer clic en este botón, se devuelve al usuario a la primera página con los productos ordenados por el precio de más costosas en menos costosa (vea la figura 15).


[![Al hacer clic en el botón ordena los productos desde el más costoso al menos](paging-and-sorting-report-data-vb/_static/image32.png)](paging-and-sorting-report-data-vb/_static/image31.png)

**Figura 15**: al hacer clic en el botón ordena los productos desde el más caro al menos ([haga clic aquí para ver imagen en tamaño completo](paging-and-sorting-report-data-vb/_static/image33.png))


## <a name="summary"></a>Resumen

En este tutorial que hemos visto cómo implementar predeterminada paginación y clasificación de capacidades, ambos de los cuales eran tan fáciles como activar una casilla! Cuando un usuario ordena o páginas de datos, se desarrolla un flujo de trabajo similar:

1. Una devolución de datos que habrá trastornos
2. Desencadena el evento de nivel de los datos de control Web s previamente (`PageIndexChanging` o `Sorting`)
3. Volver a todos los datos se recuperan por el origen ObjectDataSource
4. Los datos de control Web s posterior al nivel desencadena el evento (`PageIndexChanged` o `Sorted`)

Mientras implementa paginación básica y la ordenación es muy sencilla, se debe ejercer más esfuerzo para usar la paginación personalizada más eficiente o para mejorar aún más la interfaz de ordenación o paginación. Tutoriales futuros explorará estos temas.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](creating-a-customized-sorting-user-interface-cs.md)
> [Siguiente](efficiently-paging-through-large-amounts-of-data-vb.md)
