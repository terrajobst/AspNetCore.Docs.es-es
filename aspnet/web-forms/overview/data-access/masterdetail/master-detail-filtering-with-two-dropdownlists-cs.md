---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-cs
title: Maestro y detalles de filtrado con dos DropDownLists (C#) | Microsoft Docs
author: rick-anderson
description: Este tutorial amplía la relación principal-detalle para agregar una tercera capa, mediante dos controles de DropDownList para seleccionar la deseada registros primario y el elemento primario principal...
ms.author: aspnetcontent
ms.date: 03/31/2010
ms.assetid: ac4b0d77-4816-4ded-afd0-88dab667aedd
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-cs
msc.type: authoredcontent
ms.openlocfilehash: f8f216689494e0f80902c42f425883558c1e21ce
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37842262"
---
<a name="masterdetail-filtering-with-two-dropdownlists-c"></a>Maestro y detalles de filtrado con dos DropDownLists (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_8_CS.exe) o [descargar PDF](master-detail-filtering-with-two-dropdownlists-cs/_static/datatutorial08cs1.pdf)

> Este tutorial amplía la relación principal-detalle para agregar una tercera capa, mediante dos controles de DropDownList para seleccionar los registros primarios y el primario de segundo nivel deseados.


## <a name="introduction"></a>Introducción

En el [tutorial anterior](master-detail-filtering-with-a-dropdownlist-cs.md) hemos visto cómo mostrar un informe de detalles y maestras simple usando DropDownList único rellenado con las categorías y GridView que muestra los productos que pertenecen a la categoría seleccionada. Este modelo de informe funciona bien cuando la visualización de registros que tienen una relación de uno a varios y se pueden ampliar fácilmente para que funcione para escenarios que incluyen varias relaciones de uno a varios. Por ejemplo, un sistema de entrada de pedidos tendría tablas que corresponden a los clientes, pedidos y artículos de línea de pedido. Un cliente determinado puede tener varios pedidos de cada pedido que consta de varios elementos. Estos datos se pueden presentar al usuario con dos DropDownLists y GridView. El primer DropDownList tendría un elemento de lista para cada cliente en la base de datos con la segunda uno contenido que se va a los pedidos realizados por el cliente seleccionado. Un control GridView haría una lista de los elementos de línea del pedido seleccionado.

Mientras que la base de datos Northwind incluyen la información de detalles de pedido/customer/order canónico en su `Customers`, `Orders`, y `Order Details` tablas, estas tablas no se capturan en nuestra arquitectura. Sin embargo, aún podemos mostrar con dos DropDownLists dependientes. La primera DropDownList enumerará las categorías y el segundo los productos que pertenecen a la categoría seleccionada. DetailsView, a continuación, mostrará los detalles del producto seleccionado.

## <a name="step-1-creating-and-populating-the-categories-dropdownlist"></a>Paso 1: Crear y rellenar la DropDownList categorías

Nuestro primer objetivo es agregar la DropDownList que enumera las categorías. Estos pasos se describe detalladamente en el tutorial anterior, pero se resumen aquí por integridad.

Abra el `MasterDetailsDetails.aspx` página en el `Filtering` agregar DropDownList a la página de conjunto de carpetas, su `ID` propiedad a `Categories`y, a continuación, haga clic en el vínculo Configurar origen de datos en su etiqueta inteligente. Desde el Asistente para configuración de orígenes de datos optar por agregar un nuevo origen de datos.


[![Agregar un nuevo origen de datos para la DropDownList](master-detail-filtering-with-two-dropdownlists-cs/_static/image2.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image1.png)

**Figura 1**: agregar un nuevo origen de datos para la DropDownList ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-with-two-dropdownlists-cs/_static/image3.png))


El nuevo origen de datos de forma natural, debe ser un origen ObjectDataSource. Nombre de este nuevo origen ObjectDataSource `CategoriesDataSource` y hacer que se invoque la `CategoriesBLL` del objeto `GetCategories()` método.


[![Optar por usar la clase CategoriesBLL](master-detail-filtering-with-two-dropdownlists-cs/_static/image5.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image4.png)

**Figura 2**: optar por usar la `CategoriesBLL` clase ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-with-two-dropdownlists-cs/_static/image6.png))


[![Configurar el origen ObjectDataSource para usar el método GetCategories()](master-detail-filtering-with-two-dropdownlists-cs/_static/image8.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image7.png)

**Figura 3**: configurar el origen ObjectDataSource que se usarán el `GetCategories()` método ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-with-two-dropdownlists-cs/_static/image9.png))


Después de configurar el origen ObjectDataSource todavía es necesario especificar qué campo del origen de datos se debe mostrar en el `Categories` DropDownList y cuál debe estar configurado como el valor del elemento de lista. Establecer el `CategoryName` campo como la presentación y `CategoryID` como el valor de cada elemento de lista.


[![Tiene la presentación de DropDownList el campo CategoryName y Use CategoryID como valor](master-detail-filtering-with-two-dropdownlists-cs/_static/image11.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image10.png)

**Figura 4**: tiene la presentación de DropDownList el `CategoryName` campo y Use `CategoryID` como el valor ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-with-two-dropdownlists-cs/_static/image12.png))


En este momento, tenemos un control DropDownList (`Categories`) que se rellena con los registros de la `Categories` tabla. Cuando el usuario elige una nueva categoría de la lista desplegable desearemos una devolución de datos con el fin de actualizar el producto DropDownList que vamos a crear en el paso 2. Por lo tanto, active la opción de Habilitar AutoPostBack desde el `categories` etiqueta inteligente de DropDownList.


[![Habilitar AutoPostBack para categorías DropDownList](master-detail-filtering-with-two-dropdownlists-cs/_static/image14.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image13.png)

**Figura 5**: Habilitar AutoPostBack para el `Categories` DropDownList ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-with-two-dropdownlists-cs/_static/image15.png))


## <a name="step-2-displaying-the-selected-categorys-products-in-a-second-dropdownlist"></a>Paso 2: Mostrar los productos de la categoría seleccionada en un segundo DropDownList

Con el `Categories` DropDownList completado, el siguiente paso consiste en Mostrar DropDownList de productos que pertenecen a la categoría seleccionada. Para lograr esto, agregue otro DropDownList a la página llamada `ProductsByCategory`. Igual que con el `Categories` DropDownList, cree un nuevo origen ObjectDataSource para el `ProductsByCategory` DropDownList denominado `ProductsByCategoryDataSource`.


[![Agregar un nuevo origen de datos para ProductsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-cs/_static/image17.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image16.png)

**Figura 6**: agregar un nuevo origen de datos para el `ProductsByCategory` DropDownList ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-with-two-dropdownlists-cs/_static/image18.png))


[![Crear un nuevo origen ObjectDataSource denominado ProductsByCategoryDataSource](master-detail-filtering-with-two-dropdownlists-cs/_static/image20.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image19.png)

**Figura 7**: crear un nuevo origen ObjectDataSource denominado `ProductsByCategoryDataSource` ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-with-two-dropdownlists-cs/_static/image21.png))


Puesto que la `ProductsByCategory` necesidades DropDownList para mostrar solo aquellos productos que pertenecen a la categoría seleccionada tiene el origen ObjectDataSource invocar el `GetProductsByCategoryID(categoryID)` método desde el `ProductsBLL` objeto.


[![Optar por usar la clase ProductsBLL](master-detail-filtering-with-two-dropdownlists-cs/_static/image23.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image22.png)

**Figura 8**: optar por usar la `ProductsBLL` clase ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-with-two-dropdownlists-cs/_static/image24.png))


[![Configurar el origen ObjectDataSource para usar el método GetProductsByCategoryID(categoryID)](master-detail-filtering-with-two-dropdownlists-cs/_static/image26.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image25.png)

**Figura 9**: configurar el origen ObjectDataSource que se usarán el `GetProductsByCategoryID(categoryID)` método ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-with-two-dropdownlists-cs/_static/image27.png))


En el último paso del asistente es necesario especificar el valor de la *`categoryID`* parámetro. Asignar este parámetro para el elemento seleccionado de la `Categories` DropDownList.


[![Extraer el valor del parámetro categoryID de la lista desplegable de categorías](master-detail-filtering-with-two-dropdownlists-cs/_static/image29.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image28.png)

**Figura 10**: extraer el *`categoryID`* valor del parámetro de la `Categories` DropDownList ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-with-two-dropdownlists-cs/_static/image30.png))


Con el origen ObjectDataSource configurado, todo lo que queda es especificar qué campos de origen de datos se usan para la presentación y el valor de los elementos de la DropDownList. Mostrar el `ProductName` campo y use el `ProductID` campo como valor.


[![Especifique los campos de origen de datos utilizados para texto y las propiedades de valor de ListItems de DropDownList](master-detail-filtering-with-two-dropdownlists-cs/_static/image32.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image31.png)

**Figura 11**: especificar los campos de origen de datos usan para la DropDownList `ListItem` s' `Text` y `Value` propiedades ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-with-two-dropdownlists-cs/_static/image33.png))


Con el origen ObjectDataSource y `ProductsByCategory` DropDownList configurado nuestra página mostrará dos DropDownLists: la primera mostrará una lista de todas las categorías mientras el segundo mostrará una lista de los productos que pertenecen a la categoría seleccionada. Cuando el usuario selecciona una nueva categoría de la primera DropDownList, surgirán una devolución de datos y se repuntar DropDownList segundo, que muestra los productos que pertenecen a la categoría recién seleccionada. Figuras 12 y 13 show `MasterDetailsDetails.aspx` en acción cuando se ve mediante un explorador.


[![Cuando se visita primero la página, se selecciona la categoría Bebidas](master-detail-filtering-with-two-dropdownlists-cs/_static/image35.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image34.png)

**Figura 12**: cuando se visita primero la página, se selecciona la categoría bebidas ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-with-two-dropdownlists-cs/_static/image36.png))


[![Elegir una categoría diferente muestra productos de la nueva categoría](master-detail-filtering-with-two-dropdownlists-cs/_static/image38.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image37.png)

**Figura 13**: elección de una muestra de categoría diferentes productos de la categoría nueva ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-with-two-dropdownlists-cs/_static/image39.png))


Actualmente la `productsByCategory` DropDownList, cuando cambia, *no* producen un postback. Sin embargo, deseamos una devolución de datos que se produzca una vez que se agregue un DetailsView para mostrar los detalles del producto seleccionado (paso 3). Por consiguiente, compruebe la casilla de verificación Habilitar AutoPostBack desde el `productsByCategory` etiqueta inteligente de DropDownList.


[![Habilitar la característica AutoPostBack para el productsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-cs/_static/image41.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image40.png)

**Figura 14**: habilitar la característica AutoPostBack para el `productsByCategory` DropDownList ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-with-two-dropdownlists-cs/_static/image42.png))


## <a name="step-3-using-a-detailsview-to-display-details-for-the-selected-product"></a>Paso 3: Uso de DetailsView para mostrar los detalles del producto seleccionado

El último paso es mostrar los detalles del producto seleccionado en DetailsView. Para lograr esto, agregue un DetailsView a la página, establezca su `ID` propiedad `ProductDetails`y crear un nuevo origen ObjectDataSource para ella. Configurar este origen ObjectDataSource para extraer sus datos de la `ProductsBLL` la clase `GetProductByProductID(productID)` método utilizando el valor seleccionado de la `ProductsByCategory` DropDownList para el valor de la *`productID`* parámetro.


[![Optar por usar la clase ProductsBLL](master-detail-filtering-with-two-dropdownlists-cs/_static/image44.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image43.png)

**Figura 15**: optar por usar la `ProductsBLL` clase ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-with-two-dropdownlists-cs/_static/image45.png))


[![Configurar el origen ObjectDataSource para usar el método GetProductByProductID(productID)](master-detail-filtering-with-two-dropdownlists-cs/_static/image47.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image46.png)

**Figura 16**: configurar el origen ObjectDataSource que se usarán el `GetProductByProductID(productID)` método ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-with-two-dropdownlists-cs/_static/image48.png))


[![Extraer el valor del parámetro productID de ProductsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-cs/_static/image50.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image49.png)

**Figura 17**: extraer el *`productID`* valor del parámetro de la `ProductsByCategory` DropDownList ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-with-two-dropdownlists-cs/_static/image51.png))


Puede elegir mostrar cualquiera de los campos disponibles en DetailsView. He optado por eliminar la `ProductID`, `SupplierID`, y `CategoryID` campos y reordenar los campos restantes con el formato. Además, borra el DetailsView `Height` y `Width` propiedades, lo que permite la DetailsView expandir el ancho necesario para obtener la mejor visualización de sus datos en lugar de tener limita a un tamaño especificado. A continuación aparece el marcado completo:


[!code-aspx[Main](master-detail-filtering-with-two-dropdownlists-cs/samples/sample1.aspx)]

Dedique un momento para probar la `MasterDetailsDetails.aspx` página en un explorador. En un principio puede parecer que todo funciona según sea necesario, pero hay un pequeño problema. Cuando se elige una nueva categoría la `ProductsByCategory` DropDownList se actualiza para incluir esos productos para la categoría seleccionada, pero el `ProductDetails` DetailsView continuado mostrar la información de producto anteriores. DetailsView se actualiza cuando se elige un producto diferente para la categoría seleccionada. Además, si probar exhaustivamente suficientemente, encontrará que si elige continuamente nuevas categorías (por ejemplo, elegir bebidas desde el `Categories` DropDownList, a continuación, condimentos, a continuación, repostería) cada otra selección de categoría hace que el `ProductDetails`DetailsView en actualizarse.

Para ayudar a concretar este problema, echemos un vistazo a un ejemplo concreto. Primera vez que visite la página Seleccionar la categoría bebidas y los productos relacionados se cargan en el `ProductsByCategory` DropDownList. Chai es el producto seleccionado y sus detalles se muestran en el `ProductDetails` DetailsView, tal como se muestra en la figura 18.


[![Detalles del producto seleccionado se muestran en DetailsView](master-detail-filtering-with-two-dropdownlists-cs/_static/image53.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image52.png)

**Figura 18**: detalles de The Selected Product se muestran en DetailsView ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-with-two-dropdownlists-cs/_static/image54.png))


Si cambia la selección de categoría de bebidas a condimentos, se produce un postback y `ProductsByCategory` DropDownList se actualiza en consecuencia, pero DetailsView muestra detalles de Chai.


[![Detalles de previamente seleccionado del producto son sigue mostrando](master-detail-filtering-with-two-dropdownlists-cs/_static/image56.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image55.png)

**Figura 19**: los The seleccionado previamente detalles de producto son sigue mostrando ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-with-two-dropdownlists-cs/_static/image57.png))


Un nuevo producto en la lista de selección actualiza DetailsView según lo previsto. Si elige una categoría nueva después de cambiar el producto, DetailsView nuevo no actualiza. Sin embargo, si en lugar de elegir un nuevo producto ha seleccionado una nueva categoría, se actualizarían DetailsView. ¿Lo que en el mundo está ocurriendo aquí?

El problema es un problema de tiempo de ciclo de vida de la página. Cada vez que se solicita una página, que procede a través de una serie de pasos como su representación. En uno de estos pasos ObjectDataSource controla Compruebe si alguno de sus `SelectParameters` valores han cambiado. Si es así, el control Web de datos enlazado a ObjectDataSource sabe que necesita para actualizar su presentación. Por ejemplo, cuando se selecciona una nueva categoría, el `ProductsByCategoryDataSource` ObjectDataSource detecta que se han modificado sus valores de parámetro y el `ProductsByCategory` DropDownList vuelve a enlazar, obtención de los productos para la categoría seleccionada.

El problema que surge en esta situación es que se produzca el punto del ciclo de vida de la página que el ObjectDataSource buscan parámetros modificados *antes* de reenlace de los controles Web de datos asociados. Por lo tanto, al seleccionar una nueva categoría el `ProductsByCategoryDataSource` ObjectDataSource detecta un cambio en su valor de parámetro. ObjectDataSource utilizado por el `ProductDetails` DetailsView, sin embargo, no tenga en cuenta los cambios porque el `ProductsByCategory` DropDownList aún tiene que volver a enlazarse. Más adelante en el ciclo de vida del `ProductsByCategory` DropDownList vuelve a enlazar a su origen ObjectDataSource, tomar los productos para la categoría recién seleccionada. Mientras el `ProductsByCategory` ha cambiado el valor de DropDownList, el `ProductDetails` ObjectDataSource de DetailsView ya lo ha hecho su comprobación de valores de parámetro; por lo tanto, DetailsView muestra sus resultados anteriores. Esta interacción se muestra en la figura 20.


[![El valor de ProductsByCategory DropDownList cambia después ObjectDataSource de ProductDetails DetailsView comprueba los cambios](master-detail-filtering-with-two-dropdownlists-cs/_static/image59.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image58.png)

**Figura 20**: el `ProductsByCategory` cambios de valor de DropDownList después la `ProductDetails` ObjectDataSource comprueba de DetailsView los cambios ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-with-two-dropdownlists-cs/_static/image60.png))


Para solucionar esto es necesario volver a enlazar explícitamente el `ProductDetails` DetailsView después el `ProductsByCategory` DropDownList se ha enlazado. Podemos lograrlo mediante una llamada a la `ProductDetails` ovládacího prvku DetailsView `DataBind()` método cuando el `ProductsByCategory` de DropDownList `DataBound` desencadena el evento. Agregue el siguiente código de controlador de eventos para el `MasterDetailsDetails.aspx` clase de código subyacente de la página (consulte la "[configurar valores de parámetro de ObjectDataSource mediante programación](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)" para obtener una explicación sobre cómo agregar un controlador de eventos):


[!code-csharp[Main](master-detail-filtering-with-two-dropdownlists-cs/samples/sample2.cs)]

Después de esta llamada explícita a la `ProductDetails` ovládacího prvku DetailsView `DataBind()` se ha agregado el método, el tutorial funciona según lo previsto. Información destacada de la figura 21 cómo esto cambiado remediar nuestro problema anterior.


[![ProductDetails DetailsView es desencadena el evento del explícitamente actualiza cuando el ProductsByCategory DropDownList enlace de datos](master-detail-filtering-with-two-dropdownlists-cs/_static/image62.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image61.png)

**Figura 21**: el `ProductDetails` DetailsView es explícitamente actualiza cuando el `ProductsByCategory` de DropDownList `DataBound` desencadena el evento ([haga clic aquí para ver imagen en tamaño completo](master-detail-filtering-with-two-dropdownlists-cs/_static/image63.png))


## <a name="summary"></a>Resumen

DropDownList sirve como un elemento de interfaz de usuario ideal para los informes de maestro y detalles donde hay una relación uno a varios entre los registros maestro y detalles. En el tutorial anterior, vimos cómo usar un único DropDownList para filtrar los productos que se muestra la categoría seleccionada. En este tutorial se reemplaza el control GridView de productos con un DropDownList y utiliza un DetailsView para mostrar los detalles del producto seleccionado. Los conceptos tratados en este tutorial se pueden ampliar fácilmente a los modelos de datos que implican varias relaciones de uno a varios, como clientes, pedidos y artículos del pedido. En general, siempre puede agregar un DropDownList para cada una de las entidades en las relaciones uno a varios "uno".

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Revisor de clientes potencial para este tutorial era Hilton Giesenow. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](master-detail-filtering-with-a-dropdownlist-cs.md)
> [Siguiente](master-detail-filtering-across-two-pages-cs.md)
