---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-vb
title: Principal-detalle filtrado a través de dos páginas (VB) | Documentos de Microsoft
author: rick-anderson
description: En este tutorial veremos cómo separar un informe maestro y detalles a través de dos páginas. En la página 'maestra', se utiliza un control de repetidor para presentar una lista de categoría...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2010
ms.topic: article
ms.assetid: f1a1be2c-6fd9-4a09-916e-aa1b98d5cf17
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: 2afc216de3b6894cfdd112787ab92d7483198ecc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="masterdetail-filtering-across-two-pages-vb"></a>Principal-detalle filtrado a través de dos páginas (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_34_VB.exe) o [descarga de PDF](master-detail-filtering-acess-two-pages-datalist-vb/_static/datatutorial34vb1.pdf)

> En este tutorial veremos cómo separar un informe maestro y detalles a través de dos páginas. En la página "maestra" se utiliza un control de repetidor para presentar una lista de categorías que, al hacer clic en, se guiará al usuario a la página "Detalles" en un control DataList de dos columnas muestra los productos que pertenecen a la categoría seleccionada.


## <a name="introduction"></a>Introducción

En el [filtrado de maestro y detalles a través de dos páginas](../masterdetail/master-detail-filtering-across-two-pages-vb.md) tutorial, examinamos este patrón de uso de un control GridView para mostrar todos los proveedores en el sistema. Este GridView incluye un campo HYPERLINK, que se representa como un vínculo a una segunda página, pasando a lo largo de la `SupplierID` en la cadena de consulta. La segunda página usa un control GridView para enumerar los productos suministrados por el proveedor seleccionado.

Estos informes de dos páginas principal-detalle pueden realizarse mediante controles DataList y repetidor. La única diferencia es que el control DataList ni repetidor proporciona compatibilidad para el control de campo Hyperlink. En su lugar, debemos agregar un control HyperLink Web o un elemento delimitador HTML (`<a>`) dentro del control `ItemTemplate`. El hipervínculo `NavigateUrl` propiedad o el anclaje `href` , a continuación, se puede personalizar el atributo mediante métodos declarativas o mediante programación.

En este tutorial se explorará un ejemplo que muestra las categorías en una lista con viñetas en una página con un control de repetidor. Cada elemento de lista incluye el nombre y la descripción, la categoría con el nombre de categoría aparece como un vínculo a otra página. Al hacer clic en este vínculo se captar el usuario a la segunda página, donde un control DataList mostrará aquellos productos que pertenecen a la categoría seleccionada.

## <a name="step-1-displaying-the-categories-in-a-bulleted-list"></a>Paso 1: Mostrar las categorías en una lista con viñetas

El primer paso para crear cualquier informe principal-detalle es iniciar mostrando los registros "principales". Por lo tanto, nuestra primera tarea consiste en mostrar las categorías en la página "principal". Abra la `CategoryListMaster.aspx` página en el `DataListRepeaterFiltering` carpeta, agregue un control de repetidor y, desde la etiqueta inteligente, optar por agregar una nueva ObjectDataSource. Configurar el nuevo ObjectDataSource para que tiene acceso a sus datos de la `CategoriesBLL` la clase `GetCategories` (método) (consulte la figura 1).


[![Configurar el ObjectDataSource para usar el método de la clase CategoriesBLL GetCategories](master-detail-filtering-acess-two-pages-datalist-vb/_static/image2.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image1.png)

**Figura 1**: configurar el ObjectDataSource que se utilizan los `CategoriesBLL` la clase `GetCategories` método ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-acess-two-pages-datalist-vb/_static/image3.png))


A continuación, definir plantillas del repetidor de forma que muestra cada nombre de categoría y la descripción como un elemento en una lista con viñetas. Vamos a aún no se preocupe relacionado con cada categoría de vínculo a la página de detalles. A continuación muestra el marcado declarativo para el repetidor y ObjectDataSource:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample1.aspx)]

Con este marcado completa, tómese un momento para ver el progreso a través de un explorador. Como se muestra en la figura 2, repetidor se representa como una lista con viñetas con nombre y la descripción de cada categoría.


[![Cada categoría aparece como un elemento de lista con viñetas](master-detail-filtering-acess-two-pages-datalist-vb/_static/image5.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image4.png)

**Figura 2**: cada categoría se muestra como un elemento de lista con viñetas ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-acess-two-pages-datalist-vb/_static/image6.png))


## <a name="step-2-turning-the-category-name-into-a-link-to-the-details-page"></a>Paso 2: Activar el nombre de categoría en un vínculo a la página de detalles

Para permitir que un usuario mostrar la información de "Detalles" para una categoría determinada, tenemos que agregar un vínculo a cada lista con viñetas de elemento que, al hacer clic en, se guiará al usuario a la segunda página (`ProductsForCategoryDetails.aspx`). Esta segunda página, a continuación, mostrará los productos de la categoría seleccionada con un control DataList. Para determinar la categoría cuyo vínculo se hizo clic, es necesario pasar la categoría donde ha hecho clic `CategoryID` a la segunda página a través de algún mecanismo. La forma más sencilla y más sencilla para transferir datos escalar de una página a otra es a través de la cadena de consulta, que es la opción que se usará en este tutorial. En concreto, el `ProductsForCategoryDetails.aspx` página esperará seleccionado *`categoryID`* valor que se pasará a través de un campo de cadena de consulta denominado `CategoryID`. Por ejemplo, para ver los productos de la categoría Bebidas, que tiene un `CategoryID` de 1, un usuario debe visitar `ProductsForCategoryDetails.aspx?CategoryID=1`.

Para crear un hipervínculo para cada elemento de lista con viñetas del repetidor que necesitamos agregar un control HyperLink Web o un elemento delimitador HTML (`<a>`) a la `ItemTemplate`. En escenarios donde el hipervínculo muestran el mismo para cada fila, bastará con cualquiera de los enfoques. Para repetidores, prefiero usar el elemento delimitador. Para usar el elemento delimitador, actualice ItemTemplate del repetidor para:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample2.aspx)]

Tenga en cuenta que la `CategoryID` pueden insertar directamente dentro del elemento delimitador `href` atributo; sin embargo, por lo que debe determinados delimitar el `href` valor del atributo con apóstrofos (y Nota comillas) desde el `Eval` (método) en el `href` atributo delimita la cadena (`"CategoryID"`) entre comillas. También se puede usar un control de hipervínculo Web:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample3.aspx)]

Tenga en cuenta cómo la parte de la dirección URL estática: `ProductsForCategoryDetails.aspx?CategoryID` , se anexa al resultado de `Eval("CategoryID")` directamente dentro de la sintaxis de enlace de datos mediante la concatenación de cadenas.

Una ventaja de utilizar el control de hipervínculo es que se puede tener acceso mediante programación desde el repetidor `ItemDataBound` controlador de eventos, si es necesario. Por ejemplo, puede mostrar el nombre de categoría como texto en lugar de como un vínculo para categorías con ningún producto asociado. Esta comprobación puede realizarse mediante programación en el `ItemDataBound` controlador de eventos; para categorías sin ningún productos asociados, el hipervínculo del `NavigateUrl` propiedad puede establecerse en una cadena vacía, lo que resulta en ese nombre de categoría determinado representación como texto sin formato (en lugar de como un vínculo). Hacen referencia a la [dar formato al DataList y repetidor basado en datos](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb.md) tutorial para obtener más información sobre cómo dar formato el DataList y del repetidor contenido según la lógica mediante programación a través de la `ItemDataBound` controlador de eventos.

Si están realizando este tutorial, puede utilizar el elemento delimitador o el método de control de hipervínculo en la página. Independientemente del enfoque, al ver la página a través de un explorador que cada nombre de categoría se debería presentar como un vínculo a `ProductsForCategoryDetails.aspx`, pasando el aplicable `CategoryID` valor (consulte la figura 3).


[![Los nombres de categoría ahora vinculan a ProductsForCategoryDetails.aspx](master-detail-filtering-acess-two-pages-datalist-vb/_static/image8.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image7.png)

**Figura 3**: la categoría nombres ahora vínculo a `ProductsForCategoryDetails.aspx` ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-acess-two-pages-datalist-vb/_static/image9.png))


## <a name="step-3-listing-the-products-that-belong-to-the-selected-category"></a>Paso 3: Enumerar los productos que pertenecen a la categoría seleccionada

Con el `CategoryListMaster.aspx` página completa, estamos listos para centrarse en la implementación de la página "Detalles", `ProductsForCategoryDetails.aspx`. Abrir esta página, arrastre un control DataList desde el cuadro de herramientas hasta el diseñador y establezca su `ID` propiedad `ProductsInCategory`. A continuación, de etiquetas inteligentes del control DataList optar por agregar una nueva ObjectDataSource a la página, asígnele el nombre `ProductsInCategoryDataSource`. Configurarlo de modo que llama a la `ProductsBLL` la clase `GetProductsByCategoryID(categoryID)` método; se establece la lista desplegable que se enumera en las pestañas INSERT, UPDATE y DELETE para (ninguno).


[![Configurar el ObjectDataSource para usar GetProductsByCategoryID(categoryID) método la clase ProductsBLL](master-detail-filtering-acess-two-pages-datalist-vb/_static/image11.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image10.png)

**Figura 4**: configurar el ObjectDataSource que se utilizan los `ProductsBLL` la clase `GetProductsByCategoryID(categoryID)` método ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-acess-two-pages-datalist-vb/_static/image12.png))


Puesto que la `GetProductsByCategoryID(categoryID)` método acepta un parámetro de entrada (*`categoryID`*), el Asistente para orígenes de datos elija nos ofrece una oportunidad para especificar el origen del parámetro. Establezca el origen del parámetro QueryString mediante el QueryStringField `CategoryID`.


[![Use el campo de cadena de consulta CategoryID como el origen del parámetro](master-detail-filtering-acess-two-pages-datalist-vb/_static/image14.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image13.png)

**Figura 5**: usar el Querystring Field `CategoryID` como el origen del parámetro ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-acess-two-pages-datalist-vb/_static/image15.png))


Como hemos visto en tutoriales anteriores, después de completar el asistente Elegir origen de datos, Visual Studio crea automáticamente un `ItemTemplate` para el control DataList que enumera cada nombre de campo de datos y el valor. Reemplazar esta plantilla por una que enumera sólo el nombre del producto, proveedor y precio. Además, establezca el DataList `RepeatColumns` propiedad en 2. Después de estos cambios, su DataList y de ObjectDataSource marcado declarativo debe ser similar al siguiente:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample4.aspx)]

Para ver esta página en acción, inicie desde la `CategoryListMaster.aspx` página; a continuación, haga clic en un vínculo en la lista con viñetas de categorías. Esto le llevará a `ProductsForCategoryDetails.aspx`, pasando a lo largo de la `CategoryID` a través de la cadena de consulta. El `ProductsInCategoryDataSource` ObjectDataSource en `ProductsForCategoryDetails.aspx` , a continuación, obtendrá aquellos productos de la categoría especificada y mostrarlos en el control DataList, que presenta dos productos por cada fila. Figura 6 se muestra una captura de pantalla de `ProductsForCategoryDetails.aspx` cuando se visualizan las bebidas.


[![Se muestran las bebidas, dos por fila](master-detail-filtering-acess-two-pages-datalist-vb/_static/image17.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image16.png)

**Figura 6**: se muestran las bebidas, dos por fila ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-acess-two-pages-datalist-vb/_static/image18.png))


## <a name="step-4-displaying-category-information-on-productsforcategorydetailsaspx"></a>Paso 4: Mostrar información de categorías en ProductsForCategoryDetails.aspx

Cuando un usuario hace clic en una categoría en `CategoryListMaster.aspx`, que se tomaron `ProductsForCategoryDetails.aspx` y muestra los productos que pertenecen a la categoría seleccionada. Sin embargo, en `ProductsForCategoryDetails.aspx` no hay ningún indicaciones visuales en cuanto a qué categoría se ha seleccionado. Un usuario que pretende haga clic en bebidas pero condimentos accidentalmente donde ha hecho clic, no tiene manera de ser consciente de su error una vez que llegan `ProductsForCategoryDetails.aspx`. Para mitigar este posible problema, podemos mostrar información acerca de la categoría seleccionada, su nombre y descripción, en la parte superior de la `ProductsForCategoryDetails.aspx` página.

Para ello, agregue un FormView sobre el control de repetidor en `ProductsForCategoryDetails.aspx`. A continuación, agregue un nuevo ObjectDataSource a la página de etiquetas inteligentes de FormView denominado `CategoryDataSource` y configúrelo para utilizar el `CategoriesBLL` la clase `GetCategoryByCategoryID(categoryID)` método.


[![Acceso a información sobre la categoría a través de GetCategoryByCategoryID(categoryID) método la clase CategoriesBLL](master-detail-filtering-acess-two-pages-datalist-vb/_static/image20.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image19.png)

**Figura 7**: acceder a información sobre la categoría a través de la `CategoriesBLL` la clase `GetCategoryByCategoryID(categoryID)` método ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-acess-two-pages-datalist-vb/_static/image21.png))


Al igual que con la `ProductsInCategoryDataSource` ObjectDataSource agregado en el paso 3, el `CategoryDataSource`del Asistente para configurar orígenes de datos nos pide un origen para el `GetCategoryByCategoryID(categoryID)` el parámetro de entrada del método. Utilizar exactamente la misma configuración que antes, establezca el origen de parámetro en la cadena de consulta y el valor de QueryStringField en `CategoryID` (hacen referencia a la figura 5).

Después de completar el asistente, Visual Studio crea automáticamente un `ItemTemplate`, `EditItemTemplate`, y `InsertItemTemplate` de FormView. Puesto que se ofrecen una interfaz de solo lectura, no dude en quitar el `EditItemTemplate` y `InsertItemTemplate`. Además, puede personalizar el FormView `ItemTemplate`. Después de quitar las plantillas superfluas y personalizar la plantilla ItemTemplate, su FormView y de ObjectDataSource marcado declarativo debe ser similar al siguiente:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample5.aspx)]

La figura 8 muestra una captura de pantalla cuando viendo esta página a través de un explorador.

> [!NOTE]
> Además de FormView, también he agregado un control de hipervínculo anteriormente FormView que le llevará al usuario a la lista de categorías (`CategoryListMaster.aspx`). No dudes para colocar este vínculo en otra parte ni para omitirlo.


[![Información de categorías es ahora se muestra en la parte superior de la página](master-detail-filtering-acess-two-pages-datalist-vb/_static/image23.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image22.png)

**Figura 8**: información de categorías es ahora se muestra en la parte superior de la página ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-acess-two-pages-datalist-vb/_static/image24.png))


## <a name="step-5-displaying-a-message-if-no-products-belong-to-the-selected-category"></a>Paso 5: Mostrar un mensaje si no hay productos pertenecen a la categoría seleccionada

La `CategoryListMaster.aspx` página enumera todas las categorías en el sistema, con independencia de si hay productos asociados. Si un usuario hace clic en una categoría con ningún producto asociado, el control DataList en `ProductsForCategoryDetails.aspx` no se representarán, como su origen de datos no tendrán ningún elemento. Como hemos visto en los últimos tutoriales, GridView proporciona un `EmptyDataText` propiedad que puede utilizarse para especificar un mensaje de texto para mostrar si no hay ningún registro en su origen de datos. Por desgracia, ni el DataList ni repetidor tiene este tipo de propiedad.

Para mostrar un mensaje que informa al usuario que no hay productos coincidentes para la categoría seleccionada, tenemos que agregar una etiqueta de control a la página cuya `Text` propiedad se asigna el mensaje que se mostrará en caso de que no hay productos coincidentes. A continuación, es necesario establecer mediante programación sus `Visible` propiedad en función de si el control DataList contiene elementos.

Para lograr esto, empiece por agregar una etiqueta que aparece debajo de DataList. Establecer su `ID` propiedad `NoProductsMessage` y su `Text` propiedad a "No hay productos para la categoría seleccionada..." A continuación, es necesario establecer mediante programación esta etiqueta `Visible` propiedad en función de si los datos se enlazarán a la `ProductsInCategory` DataList. Esta asignación se debe realizar después de que los datos se ha enlazado al control DataList. De GridView, DetailsView y FormView, podríamos crear un controlador de eventos para el control `DataBound` evento, que se desencadena después de enlace de datos se ha completado. Sin embargo, el control DataList ni repetidor tiene un `DataBound` eventos disponibles.

Para este ejemplo concreto, podemos asignamos la etiqueta `Visible` propiedad en el `Page_Load` controlador de eventos, ya que los datos se habrá asignados al control DataList antes de la página `Load` eventos. Sin embargo, este enfoque no funciona en el caso general, como los datos de ObjectDataSource podrían enlazarse al control DataList más adelante en el ciclo de vida de la página. Por ejemplo, si los datos mostrados se basan en el valor de otro control, como es cuando se muestra un informe de principal-detalle usando DropDownList para contener los registros "principales", los datos pueden no vuelve a enlazar al control Web de datos hasta que el `PreRender` fase en la ciclo de vida de la página.

Una solución que funcionará para todos los casos consiste en asignar el `Visible` propiedad `False` en el DataList `ItemDataBound` (o `ItemCreated`) el controlador de eventos cuando se enlaza a un tipo de elemento de `Item` o `AlternatingItem`. En este caso sabemos que hay datos de al menos un elemento en el origen de datos y, por tanto, puede ocultar la `NoProductsMessage` etiqueta. Además de este controlador de eventos, también es necesario un controlador de eventos para el DataList `DataBinding` eventos, donde se inicialice la etiqueta `Visible` propiedad `True`. Puesto que la `DataBinding` evento se desencadena antes de la `ItemDataBound` del eventos, la etiqueta `Visible` propiedad se establecerá inicialmente en `True`; si no hay ningún elemento de datos, sin embargo, se establecerá `False`. El código siguiente implementa esta lógica:

[!code-vb[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample6.vb)]

Todas las categorías en la base de datos de Northwind están asociados a uno o varios productos. Para probar esta característica, he ajusta manualmente la base de datos Northwind para este tutorial, reasignar todos los productos asociados a la categoría de productos (`CategoryID` = 7) a la categoría Seafood (`CategoryID` = 8). Esto puede realizarse desde el Explorador de servidores mediante la elección de la nueva consulta y con los siguientes `UPDATE` instrucción:

[!code-sql[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample7.sql)]

Después de actualizar la base de datos según corresponda, volver a la `CategoryListMaster.aspx` página y haga clic en el vínculo producen. Puesto que ya no son los productos que pertenecen a la categoría de productos, debería ver la "No hay productos para la categoría seleccionada..." mensaje, tal como se muestra en la figura 9.


[![Se muestra un mensaje si no hay ningún productos pertenecientes a la categoría seleccionada](master-detail-filtering-acess-two-pages-datalist-vb/_static/image26.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image25.png)

**Figura 9**: se muestra un mensaje si no hay ningún productos pertenecientes a la categoría seleccionada ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-acess-two-pages-datalist-vb/_static/image27.png))


## <a name="summary"></a>Resumen

Mientras los informes de maestro y detalles pueden mostrar registros maestros y de detalle en una sola página, en muchos sitios Web se dividen en dos páginas web. En este tutorial explicamos cómo implementar estos informes principal-detalle manteniendo las categorías que figuran en una lista con viñetas utilizando un repetidor en la página web "maestra" y los productos asociados que aparecen en la página "Detalles". Cada elemento de lista en la página web principal incluye un vínculo a la página de detalles que se pasa a lo largo de la fila `CategoryID` valor.

En la página de detalles se logró la recuperación de los productos para el proveedor especificado a través de la `ProductsBLL` la clase `GetProductsByCategoryID(categoryID)` método. El *`categoryID`* el valor del parámetro se especifica mediante declaración con el `CategoryID` valor de cadena de consulta como origen del parámetro. También explicamos cómo mostrar detalles de la categoría en la página de detalles mediante un FormView y cómo mostrar un mensaje si no hubiera ningún productos que pertenecen a la categoría seleccionada.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a...

Esta serie de tutoriales se revisó por varios revisores útiles. Los revisores iniciales para este tutorial eran Zack Jones y Liz Shulok. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](master-detail-filtering-with-a-dropdownlist-datalist-vb.md)
> [Siguiente](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md)
