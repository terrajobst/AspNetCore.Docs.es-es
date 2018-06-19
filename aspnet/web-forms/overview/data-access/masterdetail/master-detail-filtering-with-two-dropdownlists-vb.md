---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-vb
title: Principal-detalle filtrado con dos listas desplegables (VB) | Documentos de Microsoft
author: rick-anderson
description: Este tutorial amplía la relación maestro y detalles para agregar una tercera capa, mediante dos controles de DropDownList para seleccionar la deseada recor primario y primario principal...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 11ae4f64-01ba-4823-95f4-a2fe1f84f7d7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-vb
msc.type: authoredcontent
ms.openlocfilehash: ee0232cf8f7c0533703a51a4629522fd887f216f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887231"
---
<a name="masterdetail-filtering-with-two-dropdownlists-vb"></a>Principal-detalle filtrado con dos listas desplegables (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_8_VB.exe) o [descarga de PDF](master-detail-filtering-with-two-dropdownlists-vb/_static/datatutorial08vb1.pdf)

> Este tutorial amplía la relación maestro y detalles para agregar una tercera capa, mediante dos controles de DropDownList para seleccionar los registros primarios y primario de segundo nivel deseados.


## <a name="introduction"></a>Introducción

En el [tutorial anterior](master-detail-filtering-with-a-dropdownlist-vb.md) se examina cómo mostrar un informe simple maestra/detalles usando DropDownList único rellenado con las categorías y un control GridView que muestra los productos que pertenecen a la categoría seleccionada. Este modelo de informe funciona bien cuando Mostrar registros que tienen una relación de uno a varios y se puede ampliar fácilmente para trabajar en escenarios que incluyen varias relaciones de uno a varios. Por ejemplo, un sistema de entrada de pedidos tendría tablas que corresponden a los clientes, pedidos y artículos de línea de pedido. Un cliente determinado puede tener varios pedidos a cada pedido que consta de varios elementos. Estos datos se pueden presentar al usuario con dos listas desplegables y un control GridView. El primer DropDownList tendría un elemento de lista para cada cliente en la base de datos con la segunda uno contenido que se va a los pedidos realizados por el cliente seleccionado. Un control GridView haría una lista de los elementos de línea de la orden seleccionada.

Aunque la base de datos de Northwind incluya la información de detalles de customer/order/orden canónico en su `Customers`, `Orders`, y `Order Details` tablas, estas tablas no se capturan en nuestra arquitectura. Sin embargo, todavía, imaginemos con dos listas desplegables dependientes. El primer DropDownList enumerará las categorías y el segundo los productos que pertenecen a la categoría seleccionada. DetailsView, a continuación, mostrará los detalles del producto seleccionado.

## <a name="step-1-creating-and-populating-the-categories-dropdownlist"></a>Paso 1: Crear y rellenar la categorías de DropDownList

Nuestro primer objetivo consiste en Agregar DropDownList que enumera las categorías. Estos pasos se describe detalladamente en el tutorial anterior, pero aquí se resumen por integridad.

Abra el `MasterDetailsDetails.aspx` página en el `Filtering` agregar DropDownList a la página, conjunto de carpetas, su `ID` propiedad a `Categories`y, a continuación, haga clic en el vínculo Configurar origen de datos en su etiqueta inteligente. Desde el Asistente para configuración de orígenes de datos optar por agregar un nuevo origen de datos.


[![Agregar un nuevo origen de datos para DropDownList](master-detail-filtering-with-two-dropdownlists-vb/_static/image2.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image1.png)

**Figura 1**: agregar un nuevo origen de datos para DropDownList ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-with-two-dropdownlists-vb/_static/image3.png))


El nuevo origen de datos de forma natural, debe ser un ObjectDataSource. Nombre de esta nueva ObjectDataSource `CategoriesDataSource` y hacer que se invoque el `CategoriesBLL` del objeto `GetCategories()` método.


[![Elija usar la clase CategoriesBLL](master-detail-filtering-with-two-dropdownlists-vb/_static/image5.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image4.png)

**Figura 2**: optar por usar la `CategoriesBLL` clase ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-with-two-dropdownlists-vb/_static/image6.png))


[![Configurar el ObjectDataSource para utilizar el método GetCategories()](master-detail-filtering-with-two-dropdownlists-vb/_static/image8.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image7.png)

**Figura 3**: configurar el ObjectDataSource que se utilizan los `GetCategories()` método ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-with-two-dropdownlists-vb/_static/image9.png))


Después de configurar el ObjectDataSource todavía es preciso especificar qué campo del origen de datos se debe mostrar en el `Categories` DropDownList y cuál debe configurarse como el valor para el elemento de lista. Establecer el `CategoryName` campo como la presentación y `CategoryID` como el valor de cada elemento de lista.


[![Tiene la presentación de DropDownList el campo de nombre de categoría y CategoryID de uso como el valor](master-detail-filtering-with-two-dropdownlists-vb/_static/image11.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image10.png)

**Figura 4**: tiene la presentación de DropDownList el `CategoryName` campo y Use `CategoryID` como el valor ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-with-two-dropdownlists-vb/_static/image12.png))


En este momento tenemos un control DropDownList (`Categories`) que se rellena con los registros de la `Categories` tabla. Cuando el usuario elige una nueva categoría de la lista desplegable que le deseamos una devolución de datos que se produzca con el fin de actualizar el producto DropDownList que vamos a crear en el paso 2. Por lo tanto, active la opción de Habilitar AutoPostBack desde la `categories` etiqueta inteligente de DropDownList.


[![Habilitar AutoPostBack para la categorías de DropDownList](master-detail-filtering-with-two-dropdownlists-vb/_static/image14.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image13.png)

**Figura 5**: Habilitar AutoPostBack para la `Categories` DropDownList ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-with-two-dropdownlists-vb/_static/image15.png))


## <a name="step-2-displaying-the-selected-categorys-products-in-a-second-dropdownlist"></a>Paso 2: Mostrar los productos de la categoría seleccionada en una segunda DropDownList

Con el `Categories` DropDownList completado, el paso siguiente consiste en Mostrar DropDownList de productos que pertenecen a la categoría seleccionada. Para ello, agregue otro DropDownList a la página denominada `ProductsByCategory`. Al igual que con la `Categories` DropDownList, crear un nuevo origen ObjectDataSource para la `ProductsByCategory` DropDownList denominado `ProductsByCategoryDataSource`.


[![Agregar un nuevo origen de datos para ProductsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-vb/_static/image17.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image16.png)

**Figura 6**: agregar un nuevo origen de datos para la `ProductsByCategory` DropDownList ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-with-two-dropdownlists-vb/_static/image18.png))


[![Crear un nuevo origen ObjectDataSource denominado ProductsByCategoryDataSource](master-detail-filtering-with-two-dropdownlists-vb/_static/image20.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image19.png)

**Figura 7**: crear un nuevo ObjectDataSource denominado `ProductsByCategoryDataSource` ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-with-two-dropdownlists-vb/_static/image21.png))


Puesto que la `ProductsByCategory` DropDownList necesidades para mostrar únicamente los productos que pertenecen a la categoría seleccionada tiene el ObjectDataSource invocar la `GetProductsByCategoryID(categoryID)` método desde el `ProductsBLL` objeto.


[![Elija usar la clase ProductsBLL](master-detail-filtering-with-two-dropdownlists-vb/_static/image23.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image22.png)

**Figura 8**: optar por usar la `ProductsBLL` clase ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-with-two-dropdownlists-vb/_static/image24.png))


[![Configurar el ObjectDataSource para utilizar el método GetProductsByCategoryID(categoryID)](master-detail-filtering-with-two-dropdownlists-vb/_static/image26.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image25.png)

**Figura 9**: configurar el ObjectDataSource que se utilizan los `GetProductsByCategoryID(categoryID)` método ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-with-two-dropdownlists-vb/_static/image27.png))


En el paso final del asistente es preciso especificar el valor de la *`categoryID`* parámetro. Asignar este parámetro para el elemento seleccionado de la `Categories` DropDownList.


[![Extraer el valor del parámetro categoryID de la lista desplegable de categorías](master-detail-filtering-with-two-dropdownlists-vb/_static/image29.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image28.png)

**Figura 10**: extraer el *`categoryID`* valor del parámetro de la `Categories` DropDownList ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-with-two-dropdownlists-vb/_static/image30.png))


Con ObjectDataSource configurado, lo único que queda es especificar qué campos del origen de datos se usan para la presentación y el valor de los elementos de DropDownList. Mostrar la `ProductName` campo y use la `ProductID` campo como valor.


[![Especificar los campos de origen de datos utilizados para texto y las propiedades de valor de ListItems del DropDownList](master-detail-filtering-with-two-dropdownlists-vb/_static/image32.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image31.png)

**Figura 11**: especificar los campos de origen de datos utilizan para la DropDownList `ListItem` s' `Text` y `Value` propiedades ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-with-two-dropdownlists-vb/_static/image33.png))


Con el ObjectDataSource y `ProductsByCategory` DropDownList configurado nuestra página mostrará dos listas desplegables: la primera mostrará una lista de todas las categorías de mientras que el segundo mostrará una lista de los productos que pertenecen a la categoría seleccionada. Cuando el usuario selecciona una nueva categoría en el primer DropDownList, surgirán una devolución de datos y el segundo DropDownList se se vuelve a enlazar, que muestra los productos que pertenecen a la categoría recién seleccionada. Figuras 12 y 13 mostrar `MasterDetailsDetails.aspx` en acción cuando se ven a través de un explorador.


[![Cuando visita primero a la página, se selecciona la categoría de bebidas](master-detail-filtering-with-two-dropdownlists-vb/_static/image35.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image34.png)

**Figura 12**: al primero visitar la página, se selecciona la categoría bebidas ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-with-two-dropdownlists-vb/_static/image36.png))


[![Si se elige una categoría diferente muestra los productos de la categoría nueva](master-detail-filtering-with-two-dropdownlists-vb/_static/image38.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image37.png)

**Figura 13**: elegir un diferentes pantallas de categoría productos a la nueva categoría ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-with-two-dropdownlists-vb/_static/image39.png))


Actualmente el `productsByCategory` DropDownList, si se cambian, *no* provocado un postback. Sin embargo, queremos una devolución de datos que se produzca una vez que se agrega un DetailsView para ver los detalles del producto seleccionado (paso 3). Por lo tanto, active la casilla de verificación Habilitar AutoPostBack desde la `productsByCategory` etiqueta inteligente de DropDownList.


[![Habilitar la característica de AutoPostBack para el productsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-vb/_static/image41.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image40.png)

**Figura 14**: habilitar la característica de AutoPostBack para la `productsByCategory` DropDownList ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-with-two-dropdownlists-vb/_static/image42.png))


## <a name="step-3-using-a-detailsview-to-display-details-for-the-selected-product"></a>Paso 3: Usar DetailsView para mostrar detalles del producto seleccionado

Es el último paso mostrar los detalles del producto seleccionado en un DetailsView. Para lograr esto, agregue un DetailsView a la página, establezca su `ID` propiedad `ProductDetails`y crear un nuevo origen ObjectDataSource para él. Configurar este ObjectDataSource para extraer los datos de la `ProductsBLL` la clase `GetProductByProductID(productID)` método utilizando el valor seleccionado de la `ProductsByCategory` DropDownList para el valor de la *`productID`* parámetro.


[![Elija usar la clase ProductsBLL](master-detail-filtering-with-two-dropdownlists-vb/_static/image44.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image43.png)

**Figura 15**: optar por usar la `ProductsBLL` clase ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-with-two-dropdownlists-vb/_static/image45.png))


[![Configurar el ObjectDataSource para utilizar el método GetProductByProductID(productID)](master-detail-filtering-with-two-dropdownlists-vb/_static/image47.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image46.png)

**Figura 16**: configurar el ObjectDataSource que se utilizan los `GetProductByProductID(productID)` método ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-with-two-dropdownlists-vb/_static/image48.png))


[![Extraer el valor del parámetro productID de ProductsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-vb/_static/image50.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image49.png)

**Figura 17**: extraer el *`productID`* valor del parámetro de la `ProductsByCategory` DropDownList ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-with-two-dropdownlists-vb/_static/image51.png))


Puede mostrar cualquiera de los campos disponibles en la `ProductDetails` DetailsView. He optado por eliminar la `ProductID`, `SupplierID`, y `CategoryID` campos y cambiar el orden y con el formato de los campos restantes. Además, ha retirado la DetailsView `Height` y `Width` propiedades, lo que permite la DetailsView expandir el ancho necesario para mostrar mejor sus datos en lugar de tener limitada a un tamaño especificado. El marcado completo aparece a continuación:


[!code-aspx[Main](master-detail-filtering-with-two-dropdownlists-vb/samples/sample1.aspx)]

Tómese un momento para probar la `MasterDetailsDetails.aspx` página en un explorador. A primera vista puede parecer que todo funciona según sea necesario, pero no hay un pequeño problema. Cuando se elige una nueva categoría la `ProductsByCategory` DropDownList se actualiza para incluir los productos de la categoría seleccionada, pero la `ProductDetails` DetailsView continuó a mostrar la información de producto anterior. DetailsView se actualiza cuando se elige un producto diferente para la categoría seleccionada. Además, si probar exhaustivamente suficientemente, encontrará que si elige continuamente nuevas categorías (por ejemplo, elegir bebidas desde el `Categories` DropDownList y, a continuación, condimentos, a continuación, repostería) cada selección de la categoría hace que la `ProductDetails`DetailsView que actualizarse.

Con el fin de concretar este problema, echemos un vistazo a un ejemplo concreto. Cuando visite la página por primera vez, se selecciona la categoría bebidas y los productos relacionados se cargan en el `ProductsByCategory` DropDownList. Chai es el producto seleccionado y sus detalles se muestran en la `ProductDetails` DetailsView, tal como se muestra en la figura 18.


[![Detalles del producto seleccionado se muestran en un DetailsView](master-detail-filtering-with-two-dropdownlists-vb/_static/image53.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image52.png)

**Figura 18**: detalles de la seleccionada del producto se muestran en un DetailsView ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-with-two-dropdownlists-vb/_static/image54.png))


Si cambia la selección de la categoría de bebidas en condimentos, se produce un postback y `ProductsByCategory` DropDownList se actualiza en consecuencia, pero el DetailsView aún muestra detalles de Chai.


[![Detalles de previamente seleccionada del producto son sigue mostrando](master-detail-filtering-with-two-dropdownlists-vb/_static/image56.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image55.png)

**Figura 19**: detalles de la previamente seleccionada del producto son sigue mostrando ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-with-two-dropdownlists-vb/_static/image57.png))


Un nuevo producto en la lista de selección actualiza DetailsView según lo previsto. Si se elige una categoría nueva después de cambiar el producto, DetailsView nuevo no actualiza. Sin embargo, si selecciona una nueva categoría en lugar de elegir un nuevo producto, podría actualizar DetailsView. ¿Que en el mundo está sucediendo aquí?

El problema es un problema de tiempo de ciclo de vida de la página. Cada vez que se solicita una página pasa a través de una serie de pasos como su representación. En uno de estos pasos ObjectDataSource controla Compruebe si alguno de sus `SelectParameters` valores han cambiado. Si es así, el control Web de datos enlazado a ObjectDataSource sabe que necesita actualizar su presentación. Por ejemplo, cuando se selecciona una nueva categoría, el `ProductsByCategoryDataSource` ObjectDataSource detecta que han cambiado sus valores de parámetro y el `ProductsByCategory` DropDownList, vuelve a enlazar, obtener los productos para la categoría seleccionada.

El problema que surge en esta situación es que se produce el punto en el ciclo de vida de la página que el ObjectDataSources buscan parámetros modificados *antes de* volver a enlazar los controles Web de datos asociados. Por lo tanto, al seleccionar una nueva categoría la `ProductsByCategoryDataSource` ObjectDataSource detecta un cambio en el valor del parámetro. ObjectDataSource utilizado por el `ProductDetails` DetailsView, sin embargo, no tenga en cuenta los cambios porque el `ProductsByCategory` DropDownList aún no se vuelve a enlazar. Más adelante en el ciclo de vida del `ProductsByCategory` DropDownList, vuelve a enlazar a su ObjectDataSource, arrastrar los productos para la categoría recién seleccionada. Mientras el `ProductsByCategory` ha cambiado el valor de DropDownList, el `ProductDetails` ObjectDataSource de DetailsView ya ha realizado la comprobación del valor de parámetro; por lo tanto, DetailsView muestra los resultados anteriores. Esta interacción se representa en la figura 20.


[![Cambia el valor de ProductsByCategory DropDownList después ObjectDataSource de ProductDetails DetailsView comprueba los cambios](master-detail-filtering-with-two-dropdownlists-vb/_static/image59.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image58.png)

**Figura 20**: el `ProductsByCategory` DropDownList valor cambios tras la `ProductDetails` ObjectDataSource comprueba de DetailsView los cambios ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-with-two-dropdownlists-vb/_static/image60.png))


Para solucionar esto es necesario volver a enlazar explícitamente el `ProductDetails` DetailsView después de la `ProductsByCategory` DropDownList se ha enlazado. Podemos lograr esto mediante una llamada a la `ProductDetails` de DetailsView `DataBind()` método cuando el `ProductsByCategory` de DropDownList `DataBound` desencadena el evento. Agregue el siguiente código de controlador de eventos para el `MasterDetailsDetails.aspx` clase de código subyacente de la página (hacen referencia a la "[establecer mediante programación los valores de parámetro de ObjectDataSource](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)" para obtener una explicación sobre cómo agregar un controlador de eventos):


[!code-vb[Main](master-detail-filtering-with-two-dropdownlists-vb/samples/sample2.vb)]

Después de esta llamada explícita a la `ProductDetails` de DetailsView `DataBind()` se ha agregado el método, el tutorial funciona según lo previsto. Aspectos destacados de figura 21 ¿cómo cambiado puede solucionar el problema anterior.


[![ProductDetails DetailsView es desencadena el evento del explícitamente actualiza cuando el ProductsByCategory DropDownList enlace de datos](master-detail-filtering-with-two-dropdownlists-vb/_static/image62.png)](master-detail-filtering-with-two-dropdownlists-vb/_static/image61.png)

**Figura 21**: el `ProductDetails` DetailsView resulta explícitamente actualiza el `ProductsByCategory` del DropDownList `DataBound` desencadena el evento ([haga clic aquí para ver la imagen a tamaño completo](master-detail-filtering-with-two-dropdownlists-vb/_static/image63.png))


## <a name="summary"></a>Resumen

DropDownList sirve como un elemento de la interfaz de usuario ideal para los informes de maestro y detalles donde hay una relación uno a varios entre los registros maestro y detalles. En el tutorial anterior, hemos visto cómo usar un único DropDownList para filtrar los productos que se muestran en la categoría seleccionada. En este tutorial reemplazamos GridView de productos con un DropDownList y utiliza un DetailsView para mostrar los detalles del producto seleccionado. Los conceptos tratados en este tutorial se pueden extender con facilidad a los modelos de datos que implican varias relaciones de uno a varios, como clientes, pedidos y elementos del pedido. En general, siempre puede agregar un DropDownList para cada una de las entidades de "uno" de las relaciones de uno a varios.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Revisor inicial para este tutorial era Hilton Giesenow. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](master-detail-filtering-with-a-dropdownlist-vb.md)
> [Siguiente](master-detail-filtering-across-two-pages-vb.md)
