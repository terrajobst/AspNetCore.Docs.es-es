---
uid: web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-vb
title: "Informan de paginación y ordenar datos (VB) | Documentos de Microsoft"
author: rick-anderson
description: "Paginación y la ordenación son dos características muy comunes para mostrar datos en una aplicación en línea. En este tutorial echaremos un primer vistazo a agregar ordenación y..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: b895e37e-0e69-45cc-a7e4-17ddd2e1b38d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 2ef1bb0b68a46535e3320834a0374b9a4f66182c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="paging-and-sorting-report-data-vb"></a>Paginación y la ordenación de datos de informe (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_24_VB.exe) o [descarga de PDF](paging-and-sorting-report-data-vb/_static/datatutorial24vb1.pdf)

> Paginación y la ordenación son dos características muy comunes para mostrar datos en una aplicación en línea. En este tutorial, echaremos un primer vistazo a la adición de ordenación y paginación a nuestros informes, que, a continuación, crearemos en tutoriales futuros.


## <a name="introduction"></a>Introducción

Paginación y la ordenación son dos características muy comunes para mostrar datos en una aplicación en línea. Por ejemplo, al buscar libros ASP.NET en una librería en línea, puede haber cientos de estos libros, pero el informe de los resultados de búsqueda enumera a solo diez coincidencias por página. Además, los resultados se pueden ordenar por título, precio, recuento de páginas, nombre del autor y así sucesivamente. Mientras que los últimos 23 tutoriales examinar cómo crear una variedad de informes, incluidas las interfaces que permiten agregar, editar y eliminar datos, se ha examinado no cómo ordenar los datos y la única paginación ejemplos se ha visto llevan el DetailsView y FormView controles.

En este tutorial veremos cómo agregar ordenación y paginación a nuestros informes, que se pueden efectuar activando las casillas de verificación unos simplemente. Por desgracia, esta implementación simplista tiene sus desventajas que sale un poco en que se desee de la interfaz de ordenación y las rutinas de paginación no están diseñadas para paginar eficazmente a través de grandes conjuntos de resultados. Tutoriales futuros explorará cómo superar las limitaciones de la salida-de-predeterminada paginación y la ordenación de las soluciones.

## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>Paso 1: Agregar la paginación y la ordenación de páginas Web del Tutorial

Antes de empezar este tutorial, permiten s primero Tómese un momento para agregar las páginas ASP.NET que se necesitará para este tutorial y los tres siguientes. Empiece por crear una nueva carpeta en el proyecto denominado `PagingAndSorting`. A continuación, agregue las siguientes páginas ASP.NET cinco a esta carpeta, todas ellas configurada para usar la página maestra con `Site.master`:

- `Default.aspx`
- `SimplePagingSorting.aspx`
- `EfficientPaging.aspx`
- `SortParameter.aspx`
- `CustomSortingUI.aspx`


![Cree una carpeta PagingAndSorting y agregar las páginas ASP.NET Tutorial](paging-and-sorting-report-data-vb/_static/image1.png)

**Figura 1**: cree una carpeta PagingAndSorting y agregar las páginas ASP.NET Tutorial


A continuación, abra el `Default.aspx` página y arrastre el `SectionLevelTutorialListing.ascx` Control de usuario desde el `UserControls` carpeta a la superficie de diseño. Este Control de usuario, que hemos creado en el [páginas maestras y navegación del sitio](../introduction/master-pages-and-site-navigation-vb.md) tutorial, enumera el mapa del sitio y los tutoriales se muestran en la sección actual en una lista con viñetas.


![Agregar el Control de usuario SectionLevelTutorialListing.ascx a Default.aspx](paging-and-sorting-report-data-vb/_static/image2.png)

**Figura 2**: agregar el Control de usuario SectionLevelTutorialListing.ascx a Default.aspx


Para hacer que la lista con viñetas muestre la paginación y la ordenación tutoriales que se va a crear, es necesario agregarlos a la asignación de sitio. Abra la `Web.sitemap` de archivos y agregue el siguiente marcado después de las marcas de nodo de la asignación de sitio edición, inserción y eliminación:


[!code-xml[Main](paging-and-sorting-report-data-vb/samples/sample1.xml)]


![Actualizar el mapa del sitio para incluir las nuevas páginas ASP.NET](paging-and-sorting-report-data-vb/_static/image3.png)

**Figura 3**: actualizar el mapa del sitio para incluir las nuevas páginas ASP.NET


## <a name="step-2-displaying-product-information-in-a-gridview"></a>Paso 2: Mostrar información del producto en un control GridView

Antes de que realmente implementamos paginación y capacidades de ordenación, permiten s crear primero un estándar no-srotable, no paginable GridView que muestra la información de producto. Se trata de una tarea se ha terminado antes de muchas veces a lo largo de esta serie de tutoriales por lo que estos pasos debe estar familiarizado. Comience abriendo la `SimplePagingSorting.aspx` página y arrastre un control GridView desde el cuadro de herramientas hasta el diseñador, establecer su `ID` propiedad `Products`. A continuación, cree un nuevo origen ObjectDataSource que usa la clase ProductsBLL s `GetProducts()` método para devolver toda la información de producto.


![Recuperar información sobre todos los productos con el método GetProducts()](paging-and-sorting-report-data-vb/_static/image4.png)

**Figura 4**: recuperar información sobre todos los productos con el método GetProducts()


Puesto que este informe es un informe de solo lectura, s ahí ya no tiene que asignar las operaciones de asignación ObjectDataSource `Insert()`, `Update()`, o `Delete()` métodos correspondiente `ProductsBLL` métodos; por lo tanto, elija (ninguno) en la lista desplegable para la actualización, INSERCIÓN, y borrar tabulaciones.


![Elija el (ninguno) opción en la lista desplegable en la actualización, INSERCIÓN y eliminar pestañas](paging-and-sorting-report-data-vb/_static/image5.png)

**Figura 5**: elija el (ninguno) opción en la lista desplegable en la actualización, INSERCIÓN y eliminar pestañas


A continuación, permiten s personalizar los campos de s GridView para que se muestren solo los nombres de productos, proveedores, categorías, los precios y Estados no incluidas. Además, no dude en cualquier formato de nivel de campo cambia, por ejemplo, ajustar el `HeaderText` propiedades o dar formato a los precios como una moneda. Después de estos cambios, el código de marcado de GridView s declarativa debe ser similar al siguiente:


[!code-aspx[Main](paging-and-sorting-report-data-vb/samples/sample2.aspx)]

La figura 6 muestra nuestro progreso hasta el momento cuando se ven a través de un explorador. Tenga en cuenta que la página muestra todos los productos en una pantalla, que muestra cada nombre de producto s, categoría, proveedor, precio y no incluye el estado.


[![Cada uno de los productos se enumeran](paging-and-sorting-report-data-vb/_static/image7.png)](paging-and-sorting-report-data-vb/_static/image6.png)

**Figura 6**: cada uno de los productos se enumeran ([haga clic aquí para ver la imagen a tamaño completo](paging-and-sorting-report-data-vb/_static/image8.png))


## <a name="step-3-adding-paging-support"></a>Paso 3: Agregar compatibilidad con la paginación

Enumerar *todos los* de los productos en una sola pantalla puede dar lugar a la sobrecarga de información para el usuario examina los datos. Con el fin de que los resultados sean más fáciles de administrar, podemos dividir los datos en las páginas más pequeños de datos y permite al usuario recorrer la datos página a la vez. Para lograr esto solo tiene que activar la casilla de verificación Habilitar paginación de la etiqueta inteligente de GridView s (Esto establece la s GridView [ `AllowPaging` propiedad](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.allowpaging.aspx) a `true`).


[![Active la casilla de paginación Enable para agregar compatibilidad con la paginación](paging-and-sorting-report-data-vb/_static/image10.png)](paging-and-sorting-report-data-vb/_static/image9.png)

**Figura 7**: Active la casilla de la paginación de habilitar para agregar compatibilidad de paginación ([haga clic aquí para ver la imagen a tamaño completo](paging-and-sorting-report-data-vb/_static/image11.png))


Habilitar paginación limita el número de registros mostrados por página y se agrega un *interfaz paginación* en GridView. La interfaz de paginación de forma predeterminada, que se muestra en la figura 7, es una serie de números de página, que permite al usuario navegar rápidamente de una página de datos a otra. Esta interfaz de paginación debe resultarle familiar, como se ha visto al agregar compatibilidad con la paginación para los controles DetailsView y FormView en tutoriales anteriores.

Controles de la DetailsView y FormView mostrar solo un único registro por página. El control GridView, sin embargo, consulta su [ `PageSize` propiedad](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.gridview.pagesize.aspx) para determinar cuántos registros para mostrar por página (esta propiedad tiene como valor predeterminado un valor de 10).

Esta interfaz de paginación GridView, DetailsView y FormView s se puede personalizar mediante las siguientes propiedades:

- `PagerStyle`indica la información de estilo para la interfaz de paginación; puede especificar la configuración, como `BackColor`, `ForeColor`, `CssClass`, `HorizontalAlign`, y así sucesivamente.
- `PagerSettings`contiene un grupo de propiedades que puede personalizar la funcionalidad de la interfaz de paginación; `PageButtonCount` indica el número máximo de números de página numéricos mostrados en la interfaz de paginación (el valor predeterminado es 10); el [ `Mode` propiedad](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.pagersettings.mode.aspx) indica cómo la interfaz de paginación funciona y se puede establecer en: 

    - `NextPrevious`muestra un botones siguiente y anterior, que permite al usuario paso a paso avanzar o retroceder una página a la vez
    - `NextPreviousFirstLast`Además de los botones siguiente y anterior, primero y último botones también se incluyen, que permite al usuario desplazarse rápidamente a la primera o última página de datos
    - `Numeric`muestra una serie de números de página, que permite al usuario inmediatamente ir directamente a cualquier página
    - `NumericFirstLast`Además de los números de página incluye botones de primera y última, que permite al usuario desplazarse rápidamente a la primera o última página de datos; solo se muestran los botones de primer o último si no caben todos los números de página numérica

Además, el control GridView, DetailsView y FormView todas ofrecen la `PageIndex` y `PageCount` propiedades, lo que indica la página actual que se está visualizando y el número total de páginas de datos, respectivamente. El `PageIndex` propiedad se indiza empezando por 0, lo que significa que, al ver la primera página de datos `PageIndex` será igual a 0. `PageCount`, por otro lado, inicia recuento en 1, lo que significa que `PageIndex` está limitado a los valores entre 0 y `PageCount - 1`.

Permiten s Tómese un momento para mejorar la apariencia predeterminada de la interfaz de paginación de GridView s. En concreto, permite que s tengan la interfaz de paginación alineado a la derecha con un fondo gris claro. En lugar de establecer estas propiedades directamente a través de las operaciones de asignación GridView `PagerStyle` propiedad, s permiten crear una clase CSS en `Styles.css` denominado `PagerRowStyle` y, a continuación, asigne el `PagerStyle` s `CssClass` propiedad a través de nuestro tema. Comience abriendo `Styles.css` y definición de clase agregando el código CSS siguiente:


[!code-css[Main](paging-and-sorting-report-data-vb/samples/sample3.css)]

A continuación, abra el `GridView.skin` un archivo en el `DataWebControls` carpeta dentro de la `App_Themes` carpeta. Como se explicó en la *páginas maestras y navegación del sitio* tutoriales, máscara archivos pueden utilizarse para especificar los valores de propiedad predeterminados para un control Web. Por lo tanto, aumentar las opciones existentes para incluir la configuración de la `PagerStyle` s `CssClass` propiedad `PagerRowStyle`. Además, s permiten configurar la interfaz de paginación a mostrar a lo sumo botones cinco páginas numéricas mediante la `NumericFirstLast` interfaz de paginación.


[!code-aspx[Main](paging-and-sorting-report-data-vb/samples/sample4.aspx)]

## <a name="the-paging-user-experience"></a>La experiencia de usuario de paginación

La figura 8 muestra la página web cuando visita a través de un explorador después de que la casilla de verificación Habilitar paginación de GridView s se ha comprobado y `PagerStyle` y `PagerSettings` configuraciones se realizaron a través de la `GridView.skin` archivo. Tenga en cuenta cómo únicos diez registros se muestran y la interfaz de paginación indica que se está viendo la primera página de datos.


[![Con la paginación está habilitada, se muestran solo un subconjunto de los registros a la vez](paging-and-sorting-report-data-vb/_static/image13.png)](paging-and-sorting-report-data-vb/_static/image12.png)

**Figura 8**: con paginación habilitado, solo un subconjunto de los registros que se muestran a la vez ([haga clic aquí para ver la imagen a tamaño completo](paging-and-sorting-report-data-vb/_static/image14.png))


Cuando el usuario hace clic en uno de los números de página en la interfaz de paginación, seguidamente, tiene lugar una devolución de datos y recarga de la página que muestra que los registros de página s solicitados. En la ilustración 9 se muestran los resultados tras la participación para ver la última página de datos. Tenga en cuenta que la página final solo tiene un registro; Esto es porque hay 81 registros en total, lo que da lugar a ocho páginas de 10 registros por página además una página con un solo registro.


[![Al hacer clic en un número de página provoca una devolución de datos y muestra el subconjunto apropiado de registros](paging-and-sorting-report-data-vb/_static/image16.png)](paging-and-sorting-report-data-vb/_static/image15.png)

**Figura 9**: al hacer clic en un número de página provoca una devolución de datos y muestra el subconjunto de registros correspondientes ([haga clic aquí para ver la imagen a tamaño completo](paging-and-sorting-report-data-vb/_static/image17.png))


## <a name="paging-s-server-side-workflow"></a>El flujo de trabajo de paginación s del servidor

Cuando el usuario final hace clic en un botón de la interfaz de paginación, seguidamente, tiene lugar una devolución de datos y comienza el siguiente flujo de trabajo de servidor:

1. La s de GridView (o DetailsView o FormView) `PageIndexChanging` desencadena el evento
2. ObjectDataSource volver a las solicitudes *todos los* de los datos de la capa BLL; la s GridView `PageIndex` y `PageSize` valores de propiedad se utilizan para determinar los registros devueltos por la capa BLL es necesario que se mostrará en el control GridView
3. Las operaciones de asignación GridView `PageIndexChanged` desencadena el evento

En el paso 2, ObjectDataSource solicita volver a todos los datos de su origen de datos. Este estilo de paginación se conoce normalmente como *paginación predeterminada*, tal y como el comportamiento de paginación usa de forma predeterminada al configurar el `AllowPaging` propiedad `true`. No tiene valor predeterminado pagine los datos en el control de Web naively recupera todos los registros para cada página de datos, incluso si solo un subconjunto de registros se representan realmente en el código HTML que s que se envía al explorador. A menos que la base de datos se almacena en caché el BLL o ObjectDataSource, paginación predeterminada no es viable para suficientemente grandes conjuntos de resultados o para aplicaciones web con muchos usuarios simultáneos.

En el siguiente tutorial examinaremos cómo implementar *paginación personalizada*. Con la paginación personalizada puede indicar específicamente el ObjectDataSource para recuperar solo el conjunto de registros necesarios para la página solicitada de datos. Como puede imaginar, la paginación personalizada mejora considerablemente la eficacia de paginación a través de grandes conjuntos de resultados.

> [!NOTE]
> Mientras paginación predeterminada no es adecuada al paginar a través de conjuntos de resultados lo suficientemente grande, o para sitios con muchos usuarios simultáneos, tenga en cuenta que la paginación personalizada requiere más cambios y esfuerzo para implementar y no es tan simple como una casilla de verificación de (como el valor predeterminado es paginación). Por lo tanto, la paginación predeterminada puede ser la opción ideal para sitios Web pequeños y poco tráfico o cuando se establece la paginación a través de resultados relativamente pequeño, tal y como s mucho más fácil y rápido implementar.


Por ejemplo, si se sabe que nunca podrá tener más de 100 productos en nuestra base de datos, el aumento de rendimiento mínimo disfrutado por la paginación personalizada es probable que se desplaza por el esfuerzo necesario para implementarlo. Si, sin embargo, un día tengamos miles o decenas de miles de productos, *no* implementar la paginación personalizada se obstáculos en gran medida la escalabilidad de la aplicación.

## <a name="step-4-customizing-the-paging-experience"></a>Paso 4: Personalizar la experiencia de paginación

Los controles Web de datos proporcionan varias propiedades que puede usarse para mejorar la experiencia de usuario s paginación. El `PageCount` propiedad, por ejemplo, indica el número total de páginas no existe, mientras que la `PageIndex` propiedad indica la página actual que se está visitada y puede establecerse para desplazarse rápidamente de un usuario a una página específica. Para ilustrar cómo utilizar estas propiedades mejoran la experiencia de usuario s paginación, permiten s Agregar una etiqueta de control Web de nuestra página que informa al usuario qué página re visita actualmente, junto con un control DropDownList que les permite saltar rápidamente a cualquier página especificada .

En primer lugar, agregue un control Web Label a la página, establezca su `ID` propiedad `PagingInformation`y borrar su `Text` propiedad. A continuación, cree un controlador de eventos para el s GridView `DataBound` eventos y agregue el código siguiente:


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample5.vb)]

Asigna este controlador de eventos el `PagingInformation` etiqueta s `Text` propiedad a un mensaje que informa al usuario de la página que están visitando actualmente `Products.PageIndex + 1` fuera el número total de páginas `Products.PageCount` (agregamos 1 para el `Products.PageIndex` propiedad porque `PageIndex` se indizan empezando por 0). Que deseaba asignar esta etiqueta s `Text` propiedad en el `DataBound` controlador de eventos en contraposición a la `PageIndexChanged` controlador de eventos porque el `DataBound` evento se desencadena cada vez que están enlazado a datos en GridView, mientras que la `PageIndexChanged` sólo el controlador de eventos se desencadena cuando se cambia el índice de la página. Visitan cuando GridView es inicialmente los datos enlazados en la primera página, el `PageIndexChanging` activan t (mientras que el `DataBound` evento).

Con esta adición, el usuario no se muestra un mensaje que indica qué página está visitando y es el número total de páginas de datos no existe.


[![Se muestran el número de página actual y el número Total de páginas](paging-and-sorting-report-data-vb/_static/image19.png)](paging-and-sorting-report-data-vb/_static/image18.png)

**Figura 10**: el número de página actual y el número Total de páginas se muestran ([haga clic aquí para ver la imagen a tamaño completo](paging-and-sorting-report-data-vb/_static/image20.png))


Además del control de etiqueta, permiten s también agregar un control DropDownList que enumera los números de página en el control GridView con la página está viendo actualmente seleccionada. La idea es que el usuario puede saltar rápidamente desde la página actual a otro, simplemente seleccione el nuevo índice de la página de la lista desplegable. Empiece agregando un DropDownList hasta el diseñador, establecer su `ID` propiedad `PageList` y la comprobación de la opción de Habilitar AutoPostBack desde su etiqueta inteligente.

A continuación, volver a la `DataBound` controlador de eventos y agregue el código siguiente:


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample6.vb)]

Este código se comienza mediante la desactivación de los elementos de la `PageList` DropDownList. Esto puede parecer superflua, ya que una t no sería espera que el número de páginas que se va a cambiar, pero otros usuarios pueden utilizar el sistema al mismo tiempo, agregar o quitar registros de la `Products` tabla. Las inserciones o eliminaciones podrían alterar el número de páginas de datos.

A continuación, necesitamos crear de nuevo los números de página y hacer que se asigna a la GridView actual `PageIndex` seleccionada de forma predeterminada. Esto se logra con un bucle desde 0 hasta `PageCount - 1`, agregando un nuevo `ListItem` en cada iteración y la configuración de su `Selected` propiedad como true si el índice de iteración actual es igual a las operaciones de asignación GridView `PageIndex` propiedad.

Por último, es necesario crear un controlador de eventos para el s DropDownList `SelectedIndexChanged` evento, que se activa cada vez que el usuario seleccione un elemento diferente en la lista. Para crear este controlador de eventos, simplemente haga doble clic en DropDownList en el diseñador, a continuación, agregue el código siguiente:


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample7.vb)]

Como se muestra en la figura 11, cambiando solamente la s GridView `PageIndex` propiedad hace que los datos se vuelve a enlazar a GridView. En las operaciones de asignación GridView `DataBound` controlador de eventos, DropDownList adecuado `ListItem` está seleccionada.


[![El usuario es realizada automáticamente a la sexta página al seleccionar el elemento de lista desplegable de página 6](paging-and-sorting-report-data-vb/_static/image22.png)](paging-and-sorting-report-data-vb/_static/image21.png)

**Figura 11**: el usuario es realizada automáticamente a la sexta página al seleccionar el elemento de lista desplegable de página 6 ([haga clic aquí para ver la imagen a tamaño completo](paging-and-sorting-report-data-vb/_static/image23.png))


## <a name="step-5-adding-bi-directional-sorting-support"></a>Paso 5: Agregar compatibilidad con la ordenación bidireccional

Agregar compatibilidad con la ordenación bidireccional es tan simple como agregar compatibilidad con la paginación solo tiene que activar la opción de habilitar la ordenación de la etiqueta inteligente de GridView s (que establece la s GridView [ `AllowSorting` propiedad](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.gridview.allowsorting.aspx) a `true`). Esto hace que cada uno de los encabezados de los campos de s GridView como LinkButton que, al hacer clic en, producen una devolución de datos y devolver los datos ordenados por la columna seleccionada en orden ascendente. Haga clic de nuevo en el mismo encabezado LinkButton reordena los datos en orden descendente.

> [!NOTE]
> Si utilizas una capa de acceso de datos personalizado en lugar de un conjunto de datos con tipo, puede que no tenga una opción de habilitar la ordenación en la etiqueta inteligente de s de GridView. Solo GridView enlazado a orígenes de datos que admiten de forma nativa la ordenación tiene esta casilla de verificación disponible. El conjunto de datos con tipo proporciona compatibilidad con la ordenación del cuadro, puesto que la DataTable de ADO.NET proporciona una `Sort` método que, cuando se invoca, ordena las filas de datos utilizando los criterios especificados de asignación de DataTable.


Si la capa DAL no devuelve objetos que admiten la ordenación que debe configurar el ObjectDataSource para pasar información de ordenación a la capa de lógica empresarial, que puede ordenar los datos o hacer que los datos ordenada en forma nativa por la capa DAL. Exploraremos cómo ordenar los datos en la lógica de negocios y niveles de acceso a datos en un tutorial posterior.

La ordenación LinkButton se representa como hipervínculos HTML, cuyos colores actuales (azules para un vínculo no visitado y rojo oscuro para los vínculos visitados) entren en conflicto con el color de fondo de la fila de encabezado. En su lugar, permita que s tiene todos los vínculos de la fila de encabezado aparece en blanco, independientemente de si se ha sido visitado o no. Esto puede realizarse mediante la adición de las siguientes acciones para la `Styles.css` clase:


[!code-css[Main](paging-and-sorting-report-data-vb/samples/sample8.css)]

Esta sintaxis indica que se va para usar texto en blanco al mostrar los hipervínculos dentro de un elemento que se utiliza la clase HeaderStyle.

Después de esta adición de CSS, al visitar la página a través de un explorador la pantalla debe ser similar a la figura 12. En concreto, la figura 12 muestra los resultados después de que el vínculo de encabezado de precio campo s se ha hecho clic.


[![Los resultados se hayan ordenado por el precio unitario en orden ascendente](paging-and-sorting-report-data-vb/_static/image25.png)](paging-and-sorting-report-data-vb/_static/image24.png)

**Figura 12**: los resultados se hayan ordenado por el precio unitario en orden ascendente ([haga clic aquí para ver la imagen a tamaño completo](paging-and-sorting-report-data-vb/_static/image26.png))


## <a name="examining-the-sorting-workflow"></a>Examinar el flujo de trabajo de ordenación

GridView todos los campos del BoundField, CampoCasillaVerificación, TemplateField, y así sucesivamente tienen un `SortExpression` propiedad que indica la expresión que debe usarse para ordenar los datos cuando se hace clic en ese campo s ordenación vínculo de encabezado. GridView también tiene un `SortExpression` propiedad. Cuando un encabezado de ordenación se hace clic en LinkButton GridView asigna ese campo s `SortExpression` valor a su `SortExpression` propiedad. A continuación, los datos se vuelve a recuperar de ObjectDataSource y ordenan en función de las operaciones de asignación GridView `SortExpression` propiedad. La siguiente lista detalla la secuencia de pasos cuando un usuario final ordena los datos en un control GridView:

1. Las operaciones de asignación GridView [evento Sorting](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.sorting(VS.80).aspx) se activa
2. Las operaciones de asignación GridView [ `SortExpression` propiedad](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.sortexpression.aspx) está establecido en el `SortExpression` del campo cuyo encabezado ordenación LinkButton se hizo clic
3. ObjectDataSource volver a recupera todos los datos de la capa BLL y, a continuación, ordena los datos mediante las operaciones de asignación GridView`SortExpression`
4. Las operaciones de asignación GridView `PageIndex` propiedad se restablece a 0, lo que significa que al ordenar el usuario se devuelve a la primera página de datos (suponiendo que se ha implementado la compatibilidad con la paginación)
5. Las operaciones de asignación GridView [ `Sorted` evento](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.sorted(VS.80).aspx) se activa

Al igual que con la paginación de manera predeterminada, la opción de ordenación predeterminada recupera volver a *todos los* de los registros de la capa BLL. Cuando se usa la ordenación sin paginación o cuando se usa con la ordenación no predeterminada paginación, allí s ninguna forma de evitar este rendimiento (aparte de almacenamiento en caché los datos de la base de datos). Sin embargo, como se verá en un tutorial posterior, se s posibles ordenar los datos de forma eficaz cuando se utiliza la paginación personalizada.

Al enlazar un ObjectDataSource en GridView en la lista desplegable en la etiqueta inteligente de GridView s, cada campo de GridView automáticamente tiene su `SortExpression` propiedad asignada al nombre del campo de datos en el `ProductsRow` clase. Por ejemplo, el `ProductName` BoundField s `SortExpression` se establece en `ProductName`, tal y como se muestra en el siguiente marcado declarativo:


[!code-aspx[Main](paging-and-sorting-report-data-vb/samples/sample9.aspx)]

Un campo puede configurarse para que lo s no ordenable borrando su `SortExpression` propiedad (asignándolo a una cadena vacía). Para ilustrar esto, imagine que se Professionals t ¿quiere permitir que nuestros clientes ordenar nuestros productos por precio. El `UnitPrice` BoundField s `SortExpression` se puede quitar la propiedad en el marcado declarativo o a través del cuadro de diálogo campos (que es accesible, haga clic en el vínculo Editar columnas en la etiqueta inteligente de GridView s).


![Los resultados se hayan ordenado por el precio unitario en orden ascendente](paging-and-sorting-report-data-vb/_static/image27.png)

**Figura 13**: los resultados se hayan ordenado por el precio unitario en orden ascendente


Una vez el `SortExpression` propiedad se ha quitado de la `UnitPrice` BoundField, el encabezado se representa como texto en lugar de como un vínculo, con lo que impide que los usuarios ordenar los datos por precio.


[![Mediante la eliminación de la propiedad SortExpression, los usuarios ya No pueden ordenar los productos por precio](paging-and-sorting-report-data-vb/_static/image29.png)](paging-and-sorting-report-data-vb/_static/image28.png)

**Figura 14**: mediante la eliminación de la propiedad SortExpression, los usuarios ya No pueden ordenar los productos por precio ([haga clic aquí para ver la imagen a tamaño completo](paging-and-sorting-report-data-vb/_static/image30.png))


## <a name="programmatically-sorting-the-gridview"></a>Ordenación mediante programación el control GridView

También puede ordenar el contenido del control GridView mediante programación mediante el uso de las operaciones de asignación GridView [ `Sort` método](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.sort.aspx). Basta con pasar en el `SortExpression` valor para ordenar por junto con el [ `SortDirection` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.sortdirection.aspx) (`Ascending` o `Descending`), y los datos de s GridView estará ordenados de nuevo.

Imagine que el motivo se ha desactivado la ordenación por el `UnitPrice` era porque se preocupa de que nuestros clientes comprarían simplemente solamente los productos cuyo precio más bajo. Sin embargo, queremos animarlos a comprar los productos más caros, por lo que nos d gusta que puedan ordenar los productos por precio, pero solo desde el precio mayor a menor.

Para realizar esto agregar un control de botón Web a la página, establezca su `ID` propiedad `SortPriceDescending`y su `Text` propiedad para ordenar por precio. A continuación, cree un controlador de eventos para el botón s `Click` eventos haciendo doble clic en el control de botón en el diseñador. Agregue el código siguiente al controlador de eventos:


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample10.vb)]

Al hacer clic en este botón, se devuelve al usuario a la primera página con los productos ordenados por el precio de más caros barato (vea la figura 15).


[![Haga clic en el botón ordena los productos desde el más caro que el menor](paging-and-sorting-report-data-vb/_static/image32.png)](paging-and-sorting-report-data-vb/_static/image31.png)

**Figura 15**: haga clic en el botón ordena los productos desde el más caros a la menor ([haga clic aquí para ver la imagen a tamaño completo](paging-and-sorting-report-data-vb/_static/image33.png))


## <a name="summary"></a>Resumen

En este tutorial que se ha explicado cómo implementar el valor predeterminado de paginación y capacidades de ordenación, ambos de los cuales estaban tan fáciles como activando una casilla de verificación. Cuando un usuario ordena o páginas a través de los datos, se desarrolla un flujo de trabajo similar:

1. Seguidamente, tiene lugar una devolución de datos
2. Los datos de control Web s previamente desencadena el evento de nivel (`PageIndexChanging` o `Sorting`)
3. Volver a todos los datos se recupera el ObjectDataSource
4. Los datos de control Web s posterior al nivel desencadena el evento (`PageIndexChanged` o `Sorted`)

Mientras que la implementación de paginación básica y la ordenación es muy sencilla, debe deben ejercer más esfuerzo para utilizar la paginación personalizada más eficaces o para mejorar aún más la interfaz de paginación o de ordenación. Tutoriales futuros explorarán estos temas.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [ *SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Anterior](creating-a-customized-sorting-user-interface-cs.md)
[Siguiente](efficiently-paging-through-large-amounts-of-data-vb.md)
