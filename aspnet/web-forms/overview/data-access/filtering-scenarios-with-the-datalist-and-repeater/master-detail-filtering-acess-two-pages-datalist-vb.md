---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-vb
title: Maestro y detalles de filtrado a través de dos páginas (VB) | Microsoft Docs
author: rick-anderson
description: En este tutorial, nos centramos en cómo separar un informe de maestro y detalles en dos páginas. En la página 'master' se usa un control Repeater para presentar una lista de categoría...
ms.author: riande
ms.date: 10/30/2010
ms.assetid: f1a1be2c-6fd9-4a09-916e-aa1b98d5cf17
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: 7d7be3364b4b19c89fac47875983fbb7193a36ea
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828375"
---
<a name="masterdetail-filtering-across-two-pages-vb"></a>Maestro y detalles de filtrado a través de dos páginas (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_34_VB.exe) o [descargar PDF](master-detail-filtering-acess-two-pages-datalist-vb/_static/datatutorial34vb1.pdf)

> En este tutorial, nos centramos en cómo separar un informe de maestro y detalles en dos páginas. En la página "maestra" se usa un control Repeater para presentar una lista de categorías que, al hacer clic en, llevará al usuario a la página "Detalles" donde aquellos productos que pertenecen a la categoría seleccionada se muestra un control DataList de dos columnas.


## <a name="introduction"></a>Introducción

En el [filtrado de maestro y detalles en dos páginas](../masterdetail/master-detail-filtering-across-two-pages-vb.md) tutorial, hemos visto este patrón mediante un GridView para mostrar todos los proveedores en el sistema. Este GridView incluye un campo HYPERLINK, que se representa como un vínculo a una segunda página, pasando el `SupplierID` en la cadena de consulta. La segunda página usa un control GridView para enumerar los productos suministrados por el proveedor seleccionado.

Estos informes de maestro y detalles de dos páginas pueden realizarse mediante también los controles DataList y Repeater. La única diferencia es que el control DataList ni el control Repeater proporciona compatibilidad para el control de campo Hyperlink. En su lugar, debemos agregar un control de hipervínculo Web o un elemento delimitador HTML (`<a>`) dentro del control `ItemTemplate`. El hipervínculo `NavigateUrl` propiedad o el delimitador `href` atributo, a continuación, se puede personalizar con enfoques declarativos o mediante programación.

En este tutorial, veremos un ejemplo que muestra las categorías en una lista con viñetas en una sola página mediante un control Repeater. Cada elemento de lista incluirá nombre y una descripción, la categoría con el nombre de categoría aparece como un vínculo a otra página. Al hacer clic en este vínculo se captar el usuario a la segunda página, donde un control DataList mostrará aquellos productos que pertenecen a la categoría seleccionada.

## <a name="step-1-displaying-the-categories-in-a-bulleted-list"></a>Paso 1: Mostrar las categorías en una lista con viñetas

Es el primer paso para crear los informes de maestro y detalles para que comience por mostrar los registros de "master". Por lo tanto, nuestra primera tarea consiste en mostrar las categorías en la página "maestra". Abra el `CategoryListMaster.aspx` página en el `DataListRepeaterFiltering` carpeta, agregue un control Repeater y, en la etiqueta inteligente, optar por agregar un nuevo origen ObjectDataSource. Configurar el nuevo origen ObjectDataSource para que tiene acceso a sus datos desde el `CategoriesBLL` la clase `GetCategories` (método) (consulte la figura 1).


[![Configurar el origen ObjectDataSource para usar el método de la clase CategoriesBLL GetCategories](master-detail-filtering-acess-two-pages-datalist-vb/_static/image2.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image1.png)

**Figura 1**: configurar el origen ObjectDataSource que se usarán el `CategoriesBLL` la clase `GetCategories` método ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-acess-two-pages-datalist-vb/_static/image3.png))


A continuación, defina las plantillas de Repeater, que muestra cada nombre de categoría y una descripción como un elemento en una lista con viñetas. Vamos a todavía no preocuparse por cada categoría de vínculo a la página de detalles. La siguiente muestra el marcado declarativo para el control Repeater y ObjectDataSource:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample1.aspx)]

Con este marcado completa, dedique un momento para ver nuestro progreso a través de un explorador. Como se muestra en la figura 2, el control Repeater se representa como una lista con viñetas que se muestra el nombre y una descripción de cada categoría.


[![Cada categoría se muestra como un elemento de lista con viñetas](master-detail-filtering-acess-two-pages-datalist-vb/_static/image5.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image4.png)

**Figura 2**: cada categoría se muestra como un elemento de lista con viñetas ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-acess-two-pages-datalist-vb/_static/image6.png))


## <a name="step-2-turning-the-category-name-into-a-link-to-the-details-page"></a>Paso 2: Convertir el nombre de categoría en un vínculo a la página de detalles

Para permitir que un usuario mostrar la información de "Detalles" para una categoría determinada, necesitamos agregar un vínculo a cada lista con viñetas de elemento que, al hacer clic en, llevará al usuario a la segunda página (`ProductsForCategoryDetails.aspx`). Esta segunda página, a continuación, mostrará los productos de la categoría seleccionada con un control DataList. Con el fin de determinar la categoría cuyo vínculo se hizo clic, necesitamos pasar la categoría donde ha hecho clic `CategoryID` a la segunda página a través de algún mecanismo. La forma más sencilla y más sencilla para transferir datos escalar de una página a otra es a través de la cadena de consulta, que es la opción que usaremos en este tutorial. En concreto, el `ProductsForCategoryDetails.aspx` página esperará seleccionado *`categoryID`* valor va a pasar un campo de cadena de consulta denominado `CategoryID`. Por ejemplo, para ver los productos para la categoría Bebidas, que tiene un `CategoryID` de 1, un usuario visitarán `ProductsForCategoryDetails.aspx?CategoryID=1`.

Para crear un hipervínculo para cada elemento de lista con viñetas en el control Repeater es necesario agregar un control de hipervínculo Web o un elemento delimitador HTML (`<a>`) a la `ItemTemplate`. En escenarios donde el hipervínculo muestran la misma para cada fila, bastará con cualquiera de los enfoques. Para repetidores, prefiero usar el elemento delimitador. Para usar el elemento delimitador, actualice ItemTemplate de Repeater para:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample2.aspx)]

Tenga en cuenta que el `CategoryID` se puede insertar directamente dentro del elemento delimitador `href` atributo; sin embargo, para así que determinados delimitar el `href` valor del atributo con apóstrofos (y las comillas de nota) desde el `Eval` método dentro de la `href` atributo delimita la cadena (`"CategoryID"`) con las comillas. Como alternativa, se puede usar un control de hipervínculo Web:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample3.aspx)]

Tenga en cuenta cómo la parte de la dirección URL estática: `ProductsForCategoryDetails.aspx?CategoryID` , se anexa al resultado de `Eval("CategoryID")` directamente dentro de la sintaxis de enlace de datos mediante la concatenación de cadenas.

Una ventaja de usar el control de hipervínculo es que se puede tener acceso mediante programación desde el repetidor `ItemDataBound` controlador de eventos, si es necesario. Por ejemplo, puede mostrar el nombre de categoría como texto en lugar de como un vínculo para las categorías con ningún producto asociado. Esta comprobación se podría realizar mediante programación en el `ItemDataBound` controlador de eventos; para categorías no productos asociados, el hipervínculo del `NavigateUrl` propiedad puede establecerse en una cadena vacía, lo que resulta en ese nombre de categoría en particular representación como texto sin formato (en lugar de como un vínculo). Vuelva a consultar el [dar formato a los controles DataList y Repeater basándose en datos](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb.md) tutorial para obtener más información en el formato del contenido de los controles DataList y Repeater en función de la lógica de programación a través de la `ItemDataBound` controlador de eventos.

Si está siguiendo el tutorial, no dude en usar el elemento delimitador o el método de control de hipervínculo en la página. Independientemente del enfoque, al ver la página a través de un explorador en cada nombre de categoría se debe representar como un vínculo a `ProductsForCategoryDetails.aspx`, pasando aplicable `CategoryID` valor (consulte la figura 3).


[![Los nombres de categoría ahora vinculan a ProductsForCategoryDetails.aspx](master-detail-filtering-acess-two-pages-datalist-vb/_static/image8.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image7.png)

**Figura 3**: la categoría nombres ahora vínculo a `ProductsForCategoryDetails.aspx` ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-acess-two-pages-datalist-vb/_static/image9.png))


## <a name="step-3-listing-the-products-that-belong-to-the-selected-category"></a>Paso 3: Lista de los productos que pertenecen a la categoría seleccionada

Con el `CategoryListMaster.aspx` página completa, ya estamos listos para prestar atención a la implementación de la página "Detalles", `ProductsForCategoryDetails.aspx`. Abra esta página, arrastre un control DataList de cuadro de herramientas hasta el diseñador y establezca su `ID` propiedad `ProductsInCategory`. A continuación, desde la etiqueta inteligente de DataList optar por agregar un nuevo origen ObjectDataSource a la página, asígnele el nombre `ProductsInCategoryDataSource`. Configurarlo de forma que llama a la `ProductsBLL` la clase `GetProductsByCategoryID(categoryID)` método; el conjunto de la lista desplegable se enumera en las pestañas INSERT, UPDATE y DELETE en (None).


[![Configurar el origen ObjectDataSource para usar el método de la clase ProductsBLL GetProductsByCategoryID(categoryID)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image11.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image10.png)

**Figura 4**: configurar el origen ObjectDataSource que se usarán el `ProductsBLL` la clase `GetProductsByCategoryID(categoryID)` método ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-acess-two-pages-datalist-vb/_static/image12.png))


Puesto que la `GetProductsByCategoryID(categoryID)` método acepta un parámetro de entrada (*`categoryID`*), el Asistente para orígenes de datos elija nos ofrece una oportunidad para especificar el origen del parámetro. Establecer el origen de los parámetros para la cadena de consulta mediante el QueryStringField `CategoryID`.


[![Utilice el CategoryID de campo de cadena de consulta como origen del parámetro](master-detail-filtering-acess-two-pages-datalist-vb/_static/image14.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image13.png)

**Figura 5**: usar el Querystring Field `CategoryID` como origen del parámetro ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-acess-two-pages-datalist-vb/_static/image15.png))


Como hemos visto en los tutoriales anteriores después de completar el Asistente para elegir origen de datos, Visual Studio crea automáticamente un `ItemTemplate` para el control DataList que enumera cada nombre de campo de datos y el valor. Reemplace esta plantilla con el que se muestran sólo nombre del producto, proveedor y precio. Además, establezca el DataList `RepeatColumns` propiedad en 2. Después de estos cambios, los controles DataList y de ObjectDataSource marcado declarativo debe ser similar al siguiente:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample4.aspx)]

Para ver esta página en acción, inicie desde el `CategoryListMaster.aspx` página; a continuación, haga clic en un vínculo en la lista con viñetas de categorías. Si lo hace, le llevará a `ProductsForCategoryDetails.aspx`, pasando a lo largo de la `CategoryID` a través de la cadena de consulta. El `ProductsInCategoryDataSource` ObjectDataSource en `ProductsForCategoryDetails.aspx` , a continuación, obtendrá aquellos productos para la categoría especificada y mostrarlos en el control DataList, que representa los dos productos por cada fila. Figura 6 se muestra una captura de pantalla de `ProductsForCategoryDetails.aspx` cuando se visualizan las bebidas.


[![Se muestran las bebidas, dos por fila](master-detail-filtering-acess-two-pages-datalist-vb/_static/image17.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image16.png)

**Figura 6**: se muestran las bebidas, dos por fila ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-acess-two-pages-datalist-vb/_static/image18.png))


## <a name="step-4-displaying-category-information-on-productsforcategorydetailsaspx"></a>Paso 4: Mostrar información de categoría en ProductsForCategoryDetails.aspx

Cuando un usuario hace clic en una categoría en `CategoryListMaster.aspx`, que se tomaron para `ProductsForCategoryDetails.aspx` y muestra los productos que pertenecen a la categoría seleccionada. Sin embargo, en `ProductsForCategoryDetails.aspx` no hay indicaciones visuales sobre qué categoría se ha seleccionado. Un usuario que deseaba hacer clic en bebidas, pero los condimentos accidentalmente hace clic en él, no tiene manera de darse cuenta sus errores cuando alcanzan `ProductsForCategoryDetails.aspx`. Para mitigar este posible problema, podemos mostrar información acerca de la categoría seleccionada, su nombre y descripción, en la parte superior de la `ProductsForCategoryDetails.aspx` página.

Para ello, agregue un FormView encima del control Repeater en `ProductsForCategoryDetails.aspx`. A continuación, agregue un nuevo origen ObjectDataSource a la página de etiquetas inteligentes de FormView denominado `CategoryDataSource` y configúrelo para utilizar el `CategoriesBLL` la clase `GetCategoryByCategoryID(categoryID)` método.


[![Acceso a información sobre la categoría a través de GetCategoryByCategoryID(categoryID) método la clase CategoriesBLL](master-detail-filtering-acess-two-pages-datalist-vb/_static/image20.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image19.png)

**Figura 7**: acceder a información sobre la categoría a través de la `CategoriesBLL` la clase `GetCategoryByCategoryID(categoryID)` método ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-acess-two-pages-datalist-vb/_static/image21.png))


Igual que con el `ProductsInCategoryDataSource` ObjectDataSource agregado en el paso 3, el `CategoryDataSource`del Asistente para configurar el origen de datos nos pide un origen para el `GetCategoryByCategoryID(categoryID)` el parámetro de entrada del método. Usar exactamente la misma configuración que antes, establezca el origen de parámetro en la cadena de consulta y el valor de QueryStringField en `CategoryID` (consulte la vuelta a la figura 5).

Después de completar el asistente, Visual Studio crea automáticamente un `ItemTemplate`, `EditItemTemplate`, y `InsertItemTemplate` de FormView. Puesto que proporcionamos una interfaz de solo lectura, no dude en quitar el `EditItemTemplate` y `InsertItemTemplate`. Además, no dude en Personalizar de FormView `ItemTemplate`. Después de quitar las plantillas superfluas y personalizar la plantilla ItemTemplate, su FormView y de ObjectDataSource marcado declarativo debe ser similar al siguiente:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample5.aspx)]

Figura 8 muestra una captura de pantalla cuando viendo esta página a través de un explorador.

> [!NOTE]
> Además de FormView, he agregado un control de hipervínculo anteriormente FormView que llevará al usuario a la lista de categorías (`CategoryListMaster.aspx`). Puede colocar este vínculo en otro lugar o para omitirlo.


[![Información de categorías es ahora se muestra en la parte superior de la página](master-detail-filtering-acess-two-pages-datalist-vb/_static/image23.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image22.png)

**Figura 8**: información de categorías es ahora se muestra en la parte superior de la página ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-acess-two-pages-datalist-vb/_static/image24.png))


## <a name="step-5-displaying-a-message-if-no-products-belong-to-the-selected-category"></a>Paso 5: Mostrar un mensaje si no hay productos que pertenecen a la categoría seleccionada

El `CategoryListMaster.aspx` página enumera todas las categorías en el sistema, independientemente de si hay algún productos asociados. Si un usuario hace clic en una categoría con ningún productos asociados, el control DataList en `ProductsForCategoryDetails.aspx` no se representará, como su origen de datos no tendrán ningún elemento. Como hemos visto en los tutoriales anteriores, el control GridView proporciona un `EmptyDataText` propiedad que puede utilizarse para especificar un mensaje de texto para mostrar si no hay ningún registro en su origen de datos. Por desgracia, ni el control DataList ni Repeater tiene dicha propiedad.

Con el fin de mostrar un mensaje que informa al usuario que no hay ningún producto coincidente para la categoría seleccionada, tenemos que agregar una etiqueta de control a la página cuya `Text` propiedad se asigna el mensaje que se mostrará en caso de que no hay ningún producto coincidente. A continuación, debemos establecer mediante programación su `Visible` propiedad en función de si el control DataList contiene todos los elementos.

Para lograr esto, empiece por agregar una etiqueta bajo el control DataList. Establezca su `ID` propiedad `NoProductsMessage` y su `Text` propiedad en "No hay productos para la categoría seleccionada..." A continuación, se debe establecer mediante programación esta etiqueta `Visible` propiedad en función de si los datos se enlazan con el `ProductsInCategory` DataList. Esta asignación se debe realizar después de que los datos se ha enlazado con el control DataList. Para el control GridView, DetailsView y FormView, podríamos crear un controlador de eventos para el control `DataBound` eventos, que se desencadena después de que ha completado el enlace de datos. Sin embargo, el control DataList ni el control Repeater tiene un `DataBound` eventos disponibles.

En este ejemplo concreto, podemos asignamos la etiqueta `Visible` propiedad en el `Page_Load` controlador de eventos, ya que habrá asignados los datos para el control DataList antes de la página `Load` eventos. Sin embargo, este enfoque no funcionaría en el caso general, como los datos desde el origen ObjectDataSource pueden estar enlazados al control DataList más adelante en el ciclo de vida de la página. Por ejemplo, si los datos mostrados se basan en el valor de otro control, como tal al mostrar un informe de maestro y detalles usando DropDownList para contener los registros de "master", los datos pueden no vuelve a enlazar al control Web de datos hasta el `PreRender` almacenar provisionalmente en el ciclo de vida de la página.

Una solución que funcionará para todos los casos consiste en asignar el `Visible` propiedad `False` en el control de DataList `ItemDataBound` (o `ItemCreated`) controlador de eventos cuando un tipo de elemento de enlace `Item` o `AlternatingItem`. En este caso se sabe que no hay datos al menos un elemento del origen de datos y, por tanto, puede ocultar la `NoProductsMessage` etiqueta. Además de este controlador de eventos, también es necesario un controlador de eventos para el control de DataList `DataBinding` eventos, donde se inicialice la etiqueta `Visible` propiedad `True`. Puesto que la `DataBinding` evento se desencadena antes de la `ItemDataBound` del eventos, la etiqueta `Visible` propiedad se establecerá inicialmente en `True`; si no hay ningún elemento de datos, sin embargo, se establecerá en `False`. El código siguiente implementa esta lógica:

[!code-vb[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample6.vb)]

Todas las categorías en la base de datos Northwind se asocian con uno o varios productos. Para probar esta característica, he ajustado manualmente la base de datos Northwind para este tutorial, la reasignación de todos los productos asociados con la categoría de producto (`CategoryID` = 7) a la categoría mariscos (`CategoryID` = 8). Esto puede realizarse desde el Explorador de servidores, elija la nueva consulta y con los siguientes `UPDATE` instrucción:

[!code-sql[Main](master-detail-filtering-acess-two-pages-datalist-vb/samples/sample7.sql)]

Después de actualizar la base de datos en consecuencia, volver a la `CategoryListMaster.aspx` página y haga clic en el vínculo de producir. Puesto que ya no hay productos que pertenecen a la categoría de productos, debería ver el mensaje "Hay productos para la categoría seleccionada...", tal como se muestra en la figura 9.


[![Se muestra un mensaje si hay n productos pertenecientes a la categoría seleccionada](master-detail-filtering-acess-two-pages-datalist-vb/_static/image26.png)](master-detail-filtering-acess-two-pages-datalist-vb/_static/image25.png)

**Figura 9**: se muestra un mensaje si hay n productos pertenecientes a la categoría seleccionada ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-acess-two-pages-datalist-vb/_static/image27.png))


## <a name="summary"></a>Resumen

Mientras los informes de maestro y detalles pueden mostrar tanto los registros maestro como detalle en una sola página, en muchos sitios Web se dividen en dos páginas web. En este tutorial tratamos cómo implementar un informe de maestro y detalles haciendo que las categorías enumeradas en una lista con viñetas con un control Repeater en la página web "principal" y los productos asociados aparecen en la página "Detalles". Cada elemento de lista en la página web principal contiene un vínculo a la página de detalles que se pasa a lo largo de la fila `CategoryID` valor.

En la página de detalles de la recuperación de esos productos para el proveedor especificado se lograba a través de la `ProductsBLL` la clase `GetProductsByCategoryID(categoryID)` método. El *`categoryID`* se especificó el valor del parámetro mediante declaración utilizando el `CategoryID` valor de cadena de consulta como origen del parámetro. También analizamos cómo mostrar detalles de la categoría en la página de detalles mediante un FormView y cómo mostrar un mensaje si no hubiera ningún productos que pertenecen a la categoría seleccionada.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a...

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Los revisores para este tutorial fueron Zack Jones y Liz Shulok. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](master-detail-filtering-with-a-dropdownlist-datalist-vb.md)
> [Siguiente](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md)
