---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
title: "Principal-detalle utilizando una lista con viñetas de registros maestros con un control DataList de detalles (VB) | Documentos de Microsoft"
author: rick-anderson
description: "En este tutorial se podrá comprimir el informe de dos páginas principal-detalle del tutorial anterior en una sola página, que muestra una lista con viñetas de nombres de categoría en t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2006
ms.topic: article
ms.assetid: ee20742f-6fb7-49a0-a009-058fe363aacb
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: 613ad1fb101a168c79310c9dc7bf731be264f889
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="masterdetail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb"></a>Principal-detalle utilizando una lista con viñetas de registros maestros con un control DataList de detalles (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_35_VB.exe) o [descarga de PDF](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/datatutorial35vb1.pdf)

> En este tutorial se podrá comprimir el informe de dos páginas principal-detalle del tutorial anterior en una sola página, que muestra una lista con viñetas de nombres de categoría en el lado izquierdo de la pantalla y productos de la categoría seleccionada a la derecha de la pantalla.


## <a name="introduction"></a>Introducción

En el [tutorial anterior](master-detail-filtering-acess-two-pages-datalist-vb.md) explicamos cómo separar un informe maestro y detalles a través de dos páginas. En la página maestra se usa un control de repetidor para representar una lista con viñetas de categorías. Cada nombre de categoría era un hipervínculo que, al hacer clic, se toman al usuario a la página de detalles, donde un control DataList de dos columnas mostró los productos que pertenecen a la categoría seleccionada.

En este tutorial se podrá comprimir el tutorial de dos páginas en una sola página, que muestra una lista con viñetas de nombres de categoría en el lado izquierdo de la pantalla con cada nombre de categoría que se representa como un control LinkButton. Al hacer clic en uno de los nombres de categoría LinkButton induce a una devolución de datos y los productos de s de la categoría seleccionada se enlaza a un control DataList de dos columnas a la derecha de la pantalla. Además de mostrar cada nombre de categoría s, repetidor de la izquierda muestra el número total de productos no existe para una categoría determinada (consulte la figura 1).


[![La categoría es nombre y el número Total de productos se muestran a la izquierda](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image2.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image1.png)

**Figura 1**: s nombre de la categoría y el número Total de productos se muestran a la izquierda ([haga clic aquí para ver la imagen a tamaño completo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image3.png))


## <a name="step-1-displaying-a-repeater-in-the-left-portion-of-the-screen"></a>Paso 1: Mostrar un repetidor en la parte izquierda de la pantalla

Para este tutorial es necesario que la lista con viñetas de categorías aparezca a la izquierda de los productos de s de la categoría seleccionada. Se puede colocar contenido en una página web mediante el estándar HTML elementos etiquetas de párrafo, espacios de no separación, `<table>` s y así sucesivamente o a través de técnicas de (CSS) de la hoja de estilos en cascada. Hasta ahora todos nuestros tutoriales han usado técnicas CSS para colocar. Cuando se compila la interfaz de usuario de navegación en la página maestra en la [páginas maestras y navegación del sitio](../introduction/master-pages-and-site-navigation-vb.md) tutorial utilizáramos *posiciones absolutas*, que indica el desplazamiento de píxel precisa para la navegación lista y el contenido principal. También se puede usar CSS para colocar un elemento a la derecha o izquierda de otro a través de *flotante*. Podemos tenemos la lista con viñetas de categorías aparecen a la izquierda de los productos de s de la categoría seleccionada por flotante repetidor a la izquierda del control DataList

Abra la `CategoriesAndProducts.aspx` página desde el `DataListRepeaterFiltering` carpeta y agregar a la página un repetidor y un control DataList. Establecer el repetidor s `ID` a `Categories` y DataList s a `CategoryProducts`. Vaya a la vista del origen y colocar los controles de repetidor y DataList dentro de sus propios `<div>` elementos. Es decir, incluya el repetidor dentro de un `<div>` elemento primero y, a continuación, el control DataList en su propio `<div>` elemento directamente después del repetidor. El código de marcado en este momento debe ser similar al siguiente:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample1.aspx)]

Para hacer flotar repetidor a la izquierda del control DataList, debemos usar la `float` atributo de estilo CSS, como:


[!code-html[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample2.html)]

El `float: left;` flote hasta llegar al primer `<div>` elemento a la izquierda de la segunda. El `width` y `padding-right` configuración indica la primera `<div>` s `width` y se agrega el relleno entre el `<div>` contenido del elemento s y el margen derecho. Para obtener más información sobre flotante elementos en CSS desproteger la [Floatutorial](http://css.maxdesign.com.au/floatutorial/).

En lugar de especificar la configuración de estilo directamente a través de la primera `<p>` elemento s `style` atributo, permiten s en su lugar, cree una nueva clase CSS en `Styles.css` denominado `FloatLeft`:


[!code-css[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample3.css)]

A continuación, podemos reemplazar la `<div>` con `<div class="FloatLeft">`.

Después de agregar la clase CSS y configurar el marcado en el `CategoriesAndProducts.aspx` página, vaya al diseñador. Debería ver el repetidor flotante a la izquierda del control DataList (aunque derecha ahora ambos solo aparecen como gris cuadros desde que se ve todavía para configurar sus orígenes de datos o plantillas).


[![Repetidor flota hacia la izquierda del control DataList](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image5.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image4.png)

**Figura 2**: el repetidor flota hacia la izquierda del control DataList ([haga clic aquí para ver la imagen a tamaño completo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image6.png))


## <a name="step-2-determining-the-number-of-products-for-each-category"></a>Paso 2: Determinar el número de productos para cada categoría

Con la s repetidor y DataList que rodea el marcado completo, re listo para enlazar los datos de categoría a repetidor controla de manera centralizada. Sin embargo, como se muestra en la lista con viñetas de categorías en la figura 1, además de cada nombre de categoría s también es necesario mostrar el número de productos asociados con la categoría. Para obtener acceso a esta información se puede:

- **Determinar esta información de la clase de código subyacente ASP.NET page s.** Dada una determinada  *`categoryID`*  podemos determinar el número de productos asociados mediante una llamada a la `ProductsBLL` clase s. `GetProductsByCategoryID(categoryID)` método. Este método devuelve un `ProductsDataTable` cuyos `Count` propiedad indica cuántos `ProductsRow` s existe, que es el número de productos para el elemento especificado  *`categoryID`* . Podemos crear una `ItemDataBound` controlador de eventos para el repetidor que, para cada categoría enlazado a repetidor, llama a la `ProductsBLL` clase s. `GetProductsByCategoryID(categoryID)` método e incluye el recuento en la salida.
- **Actualización de la `CategoriesDataTable` en el conjunto de datos con tipo para incluir un `NumberOfProducts` columna.** A continuación, podemos actualizar el `GetCategories()` método en el `CategoriesDataTable` para incluir esta información o, o bien, deje `GetCategories()` como-es y crear un nuevo `CategoriesDataTable` método llamado `GetCategoriesAndNumberOfProducts()`.

Permiten s explorar ambas técnicas. El primer enfoque es más fácil de implementar porque no queremos t es necesario actualizar la capa de acceso a datos; Sin embargo, requiere más de las comunicaciones con la base de datos. La llamada a la `ProductsBLL` clase s `GetProductsByCategoryID(categoryID)` método en el `ItemDataBound` controlador de eventos agrega una llamada de la base de datos adicional para cada categoría de muestra del repetidor. Con esta técnica hay *N* + 1 base de datos de llamadas, donde *N* es el número de categorías que se muestran en el repetidor. Con el segundo método, se devuelve el recuento de productos con información acerca de cada categoría de la `CategoriesBLL` clase s `GetCategories()` (o `GetCategoriesAndNumberOfProducts()`) método, lo que resulta en un solo de ida y vuelta a la base de datos.

## <a name="determining-the-number-of-products-in-the-itemdatabound-event-handler"></a>Determinar el número de productos en el controlador de evento ItemDataBound

Determinar el número de productos para cada categoría de repetidor s `ItemDataBound` controlador de eventos no requiere ninguna modificación en la capa de acceso a datos existente. Se pueden realizar todas las modificaciones directamente en la `CategoriesAndProducts.aspx` página. Empiece agregando un nuevo ObjectDataSource denominado `CategoriesDataSource` a través de la etiqueta inteligente de repetidor s. A continuación, configure la `CategoriesDataSource` ObjectDataSource por lo que TI recupera sus datos de la `CategoriesBLL` clase s. `GetCategories()` método.


[![Configurar el ObjectDataSource para utilizar la clase CategoriesBLL s GetCategories() (método)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image8.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image7.png)

**Figura 3**: configurar el ObjectDataSource que se utilizan los `CategoriesBLL` clase s `GetCategories()` método ([haga clic aquí para ver la imagen a tamaño completo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image9.png))


Cada elemento de la `Categories` repetidor debe ser seleccionable y, al hacer clic, provocar la `CategoryProducts` DataList para mostrar los productos de la categoría seleccionada. Esto puede realizarse mediante la realización de cada categoría de un hipervínculo, vincular a esta misma página (`CategoriesAndProducts.aspx`), pero pasando el `CategoryID` a través de la cadena de consulta, mucho como vimos en el tutorial anterior. La ventaja de este enfoque es que una página que muestra los productos de una categoría determinada s puede estar marcada e indizada por un motor de búsqueda.

Como alternativa, podemos hacer que cada categoría de un control LinkButton, que es el enfoque que usaremos para este tutorial. Se representa en el explorador del usuario s como un hipervínculo LinkButton pero, al hacer clic, induce a una devolución de datos; en la devolución de datos, las operaciones de asignación DataList ObjectDataSource tiene que actualizarse para mostrar los productos que pertenecen a la categoría seleccionada. Para este tutorial, utilizando un hipervínculo resulta más apropiado que el uso de un control LinkButton; Sin embargo, puede haber otros escenarios donde es más conveniente utilizar un control LinkButton. Mientras que el enfoque de hipervínculo es ideal para este ejemplo, permiten s en su lugar, explican cómo utilizar el control LinkButton. Como veremos, utilizar un control LinkButton presenta algunos desafíos que en caso contrario, no se producirá con un hipervínculo. Por lo tanto, el uso un LinkButton en este tutorial se resalte estos desafíos y ayudar a proporcionar soluciones para estos escenarios donde podemos deseamos usar un control LinkButton en lugar de un hipervínculo.

> [!NOTE]
> Se recomienda repetir este tutorial con un control de hipervínculo o `<a>` elemento en lugar de LinkButton.


El marcado siguiente muestra la sintaxis declarativa para repetidor y ObjectDataSource. Tenga en cuenta que las plantillas de s repetidor representan una lista con viñetas con cada elemento como un control LinkButton:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample4.aspx)]

> [!NOTE]
> Para este tutorial repetidor debe tener su estado de vista habilitado (tenga en cuenta la omisión de la `EnableViewState="False"` de la sintaxis declarativa de repetidor s). En el paso 3 se creará un controlador de eventos para el repetidor s `ItemCommand` eventos en el que se va a actualizar DataList s s ObjectDataSource `SelectParameters` colección. Repetidor s `ItemCommand`, sin embargo, ganó incendio t si el estado de vista está deshabilitado. Vea [más difíciles de A una pregunta de ASP.NET](http://scottonwriting.net/sowblog/posts/1263.aspx) y [su solución](http://scottonwriting.net/sowBlog/posts/1268.aspx) para obtener más información sobre por qué el estado de vista debe estar habilitado para un repetidor s `ItemCommand` activación del evento.


LinkButton con el `ID` valor de propiedad de `ViewCategory` no tiene su `Text` conjunto de propiedades. Si hubiéramos Queríamos saber el nombre de categoría para mostrar, se habría establezca la propiedad Text mediante declaración, mediante la sintaxis de enlace de datos, como:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample5.aspx)]

Sin embargo, queremos que aparezca el nombre de categoría s *y* el número de productos que pertenecen a esa categoría. Esta información se puede recuperar desde el repetidor s `ItemDataBound` controlador de eventos mediante la realización de una llamada a la `ProductBLL` clase s `GetCategoriesByProductID(categoryID)` método y determinar cuántos registros se devuelven en el cuadro `ProductsDataTable`, como el código siguiente se muestran:


[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample6.vb)]

Empezamos asegurándose de que se está trabajando con un elemento de datos (uno cuyo `ItemType` es `Item` o `AlternatingItem`) y, a continuación, hacer referencia a la `CategoriesRow` instancia que solo se ha enlazado a la actual `RepeaterItem`. A continuación, se determina el número de productos para esta categoría mediante la creación de una instancia de la `ProductsBLL` de la clase, una llamada a su `GetCategoriesByProductID(categoryID)` método y determinar el número de registros devueltos usando la `Count` propiedad. Por último, el `ViewCategory` LinkButton en ItemTemplate es references y su `Text` propiedad está establecida en *CategoryName* (*NumberOfProductsInCategory*), donde  *NumberOfProductsInCategory* se da formato como un número con cero posiciones decimales.

> [!NOTE]
> Como alternativa, podríamos haber agregamos una *formato función* a la clase de código subyacente ASP.NET página s que acepta una categoría s `CategoryName` y `CategoryID` valores y devuelve el `CategoryName` concatenado con el número de productos de la categoría (según lo determinado por una llamada a la `GetCategoriesByProductID(categoryID)` método). Los resultados de una función de este formato se pudieron asignar mediante declaración a las operaciones de asignación LinkButton Text (propiedad) reemplaza la necesidad de la `ItemDataBound` controlador de eventos. Hacer referencia a la [utilizando TemplateFields en el GridView Control](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) o [dar formato al DataList y repetidor basado en datos](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb.md) tutoriales para obtener más información sobre el uso de funciones de formato.


Después de agregar este controlador de eventos, tómese un momento para probar la página a través de un explorador. Tenga en cuenta cómo se muestra cada categoría en una lista con viñetas, mostrar el nombre de categoría s y el número de productos asociados con la categoría (consulte la figura 4).


[![Se muestran cada s nombre de categoría y el número de productos](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image11.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image10.png)

**Figura 4**: se muestran cada categoría s nombre y número de productos ([haga clic aquí para ver la imagen a tamaño completo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image12.png))


## <a name="updating-thecategoriesdatatableandcategoriestableadapterto-include-the-number-of-products-for-each-category"></a>Actualizando el`CategoriesDataTable`y`CategoriesTableAdapter`para que incluya el número de productos para cada categoría

En lugar de determinar el número de productos de cada categoría tal y como s se enlazan a repetidor, podemos simplificar el proceso mediante el ajuste de la `CategoriesDataTable` y `CategoriesTableAdapter` en la capa de acceso a datos para incluir esta información de forma nativa. Para lograr esto, debemos agregar una nueva columna a `CategoriesDataTable` para contener el número de productos asociados. Para agregar una nueva columna a una tabla de datos, abra el conjunto de datos con tipo (`App_Code\DAL\Northwind.xsd`), haga doble clic en la tabla de datos para modificar y elija Agregar / columna. Agregar una nueva columna a la `CategoriesDataTable` (Véase la figura 5).


[![Agregar una nueva columna a la CategoriesDataSource](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image14.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image13.png)

**Figura 5**: agregar una nueva columna a la `CategoriesDataSource` ([haga clic aquí para ver la imagen a tamaño completo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image15.png))


Esto agregará una nueva columna denominada `Column1`, que puede cambiar escribiendo simplemente en un nombre diferente. Cambiar el nombre de esta nueva columna a `NumberOfProducts`. A continuación, necesitamos configurar estas propiedades de columna s. Haga clic en la nueva columna y vaya a la ventana Propiedades. Cambiar la columna s `DataType` propiedad de `System.String` a `System.Int32` y establezca el `ReadOnly` propiedad `True`, tal y como se muestra en la figura 6.


![Establecer las propiedades de solo lectura de la nueva columna y el tipo de datos](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image16.png)

**Figura 6**: establecer el `DataType` y `ReadOnly` propiedades de la nueva columna


Mientras el `CategoriesDataTable` tiene ahora un `NumberOfProducts` columna, su valor no se establece por cualquiera de las consultas de s TableAdapter correspondientes. Podemos actualizar el `GetCategories()` método para devolver esta información si deseamos que dicha información devuelve cada vez que se recupera información de categoría. Si, sin embargo, únicamente se necesita obtener el número de productos asociados para las categorías en algunos casos (por ejemplo, como en este tutorial), podemos dejar `GetCategories()` como-es y cree un nuevo método que devuelve esta información. S permiten usar este último enfoque, crear un nuevo método denominado `GetCategoriesAndNumberOfProducts()`.

Para agregar este nuevo `GetCategoriesAndNumberOfProducts()` método, el botón secundario en el `CategoriesTableAdapter` y elija la nueva consulta. Se abrirá el TableAdapter consultar a Asistente de configuración, que se ve usa varias veces en los tutoriales anteriores. Para este método, inicie al asistente que indica que la consulta utiliza una instrucción SQL ad hoc que devuelve filas.


[![Cree el método con una instrucción de SQL Ad Hoc](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image18.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image17.png)

**Figura 7**: crear el método mediante una instrucción de SQL Ad Hoc ([haga clic aquí para ver la imagen a tamaño completo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image19.png))


[![La instrucción SQL devuelve filas](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image21.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image20.png)

**Figura 8**: The SQL instrucción devuelve filas ([haga clic aquí para ver la imagen a tamaño completo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image22.png))


La siguiente pantalla del asistente pide us para la consulta que se utilizará. Para devolver cada categoría s `CategoryID`, `CategoryName`, y `Description` campos, junto con el número de productos asociados con la categoría, utilizan la siguiente `SELECT` instrucción:


[!code-sql[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample7.sql)]


[![Especifique la consulta que se utilizan](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image24.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image23.png)

**Figura 9**: especifique la consulta que se utilizan ([haga clic aquí para ver la imagen a tamaño completo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image25.png))


Tenga en cuenta que la subconsulta que calcula el número de productos asociados con la categoría es un alias como `NumberOfProducts`. Esta coincidencia nomenclatura hace que el valor devuelto por esta subconsulta para asociar a la `CategoriesDataTable` s `NumberOfProducts` columna.

Después de escribir esta consulta, el último paso es elegir el nombre para el nuevo método. Use `FillWithNumberOfProducts` y `GetCategoriesAndNumberOfProducts` para el relleno una DataTable y devolver un DataTable patrones, respectivamente.


[![Nombre de la nueva FillWithNumberOfProducts de métodos de TableAdapter s y GetCategoriesAndNumberOfProducts](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image27.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image26.png)

**Figura 10**: el nombre de los métodos de TableAdapter nueva s `FillWithNumberOfProducts` y `GetCategoriesAndNumberOfProducts` ([haga clic aquí para ver la imagen a tamaño completo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image28.png))


En este punto, la capa de acceso a datos se ha ampliado para incluir el número de productos por categoría. Puesto que nuestro de nivel de presentación enruta todas las llamadas a la capa DAL a través de una capa de lógica empresarial independiente que necesitamos agregar correspondiente `GetCategoriesAndNumberOfProducts` método a la `CategoriesBLL` clase:


[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample8.vb)]

Con la capa DAL y BLL completa, se re está listo para enlazar estos datos a la `Categories` repetidor en `CategoriesAndProducts.aspx`! Si se ha creado ya un ObjectDataSource para repetidor desde el determinar el número de productos en la `ItemDataBound` sección del controlador de eventos, elimine este ObjectDataSource y quitar repetidor s `DataSourceID` propiedad también configuración; sin líos el Repetidor s `ItemDataBound` evento desde el controlador de eventos mediante la eliminación de la `Handles Categories.OnItemDataBound` sintaxis de la clase de código subyacente ASP.NET.

Con repetidor su estado original, agregar un nuevo origen ObjectDataSource denominado `CategoriesDataSource` a través de la etiqueta inteligente de repetidor s. Configurar el ObjectDataSource para usar el `CategoriesBLL` (clase), pero en lugar de tener usar el `GetCategories()` método, se lo use `GetCategoriesAndNumberOfProducts()` en su lugar (consulte la figura 11).


[![Configurar el ObjectDataSource para utilizar el método GetCategoriesAndNumberOfProducts](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image30.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image29.png)

**Figura 11**: configurar el ObjectDataSource que se utilizan los `GetCategoriesAndNumberOfProducts` método ([haga clic aquí para ver la imagen a tamaño completo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image31.png))


A continuación, actualice el `ItemTemplate` para que las operaciones de asignación LinkButton `Text` propiedad se asigna mediante declaración con la sintaxis de enlace de datos e incluye tanto el `CategoryName` y `NumberOfProducts` campos de datos. El marcado declarativo completo para repetidor y `CategoriesDataSource` ObjectDataSource seguimiento:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample9.aspx)]

El resultado representado mediante la actualización de la capa DAL para incluir un `NumberOfProducts` columna es lo mismo que usar el `ItemDataBound` enfoque de controlador de eventos (hacen referencia a la figura 4 para ver la pantalla de captura del repetidor que muestra los nombres de categoría y el número de productos).

## <a name="step-3-displaying-the-selected-category-s-products"></a>Paso 3: Mostrar los productos de s de la categoría seleccionada

En este momento tenemos el `Categories` repetidor mostrar la lista de categorías junto con el número de productos en cada categoría. Repetidor usa un LinkButton para cada categoría que, al hacer clic, hace una devolución de datos, en la que señalan se necesitan para mostrar los productos de la categoría seleccionada en el `CategoryProducts` DataList.

Presenta la dificultad nos encontramos es cómo hacer que el control DataList mostrar solo los productos para la categoría seleccionada. En el [principal/detalle con un GridView maestro seleccionable DetailsView detalles](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md) tutorial hemos visto cómo crear un control GridView cuyas filas se han podido seleccionar, con la fila seleccionada s detalles que se muestran en un DetailsView en la misma página. Las operaciones de asignación GridView ObjectDataSource devuelve información acerca de todos los productos con el `ProductsBLL` s `GetProducts()` método mientras la s de DetailsView ObjectDataSource recupera información sobre el uso de producto seleccionado el `GetProductsByProductID(productID)` método. El  *`productID`*  el valor del parámetro se proporciona mediante declaración mediante la asociación con el valor de las operaciones de asignación GridView `SelectedValue` propiedad. Por desgracia, repetidor no tiene un `SelectedValue` propiedad y no puede actuar como un origen del parámetro.

> [!NOTE]
> Este es uno de los desafíos que aparecen cuando se usa el control LinkButton en un repetidor. Habíamos utiliza un hipervínculo para pasar el `CategoryID` a través de la cadena de consulta en su lugar, podríamos usar ese campo de cadena de consulta como origen para el valor del parámetro.


Antes de que nos preocupamos por la falta de un `SelectedValue` de repetidor, sin embargo, let de la propiedad s enlazar el control DataList a un origen ObjectDataSource en primer lugar y especificar su `ItemTemplate`.

Desde la etiqueta inteligente de DataList s, optar por agregar una nueva ObjectDataSource denominado `CategoryProductsDataSource` y configúrelo para utilizar el `ProductsBLL` clase s. `GetProductsByCategoryID(categoryID)` método. Puesto que el control DataList en este tutorial ofrece una interfaz de solo lectura, no dude en establecer las listas desplegables en la instrucción INSERT, UPDATE y eliminar las fichas (ninguno).


[![Configurar el ObjectDataSource para usar la clase ProductsBLL s GetProductsByCategoryID(categoryID) (método)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image33.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image32.png)

**Figura 12**: configurar el ObjectDataSource que se utilizan `ProductsBLL` clase s `GetProductsByCategoryID(categoryID)` método ([haga clic aquí para ver la imagen a tamaño completo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image34.png))


Puesto que la `GetProductsByCategoryID(categoryID)` método espera un parámetro de entrada (*`categoryID`*), el Asistente para configurar orígenes de datos nos permite especificar el origen de s de parámetro. Tenían las categorías se han incluido en un control GridView o un control DataList, d establecemos la lista desplegable de origen de parámetro para el Control y la ControlID a la `ID` de los datos de control Web. Sin embargo, como la falta de repetidor un `SelectedValue` no se puede usar como un origen de los parámetros de propiedad. Si se selecciona, encontrará que la lista desplegable de ControlID sólo contiene un control `ID``CategoryProducts`, el `ID` del control DataList.

Por ahora, establezca la lista desplegable de origen de parámetro en ninguno. Se terminará mediante programación asignar este valor de parámetro cuando una categoría que se hace clic en LinkButton del repetidor.


[![Hacer no especificar un origen de parámetro para el parámetro categoryID](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image36.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image35.png)

**Figura 13**: no se especifique un origen de parámetro para la  *`categoryID`*  parámetro ([haga clic aquí para ver la imagen a tamaño completo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image37.png))


Después de completar el Asistente para configurar orígenes de datos, Visual Studio genera automáticamente el control DataList s `ItemTemplate`. Reemplace este valor predeterminado `ItemTemplate` con la plantilla se utilizado en el tutorial anterior; además, establezca la DataList s `RepeatColumns` propiedad en 2. Después de realizar estos cambios en el marcado declarativo para su DataList y su ObjectDataSource asociado debe ser similar al siguiente:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample10.aspx)]

Actualmente, el `CategoryProductsDataSource` ObjectDataSource s  *`categoryID`*  parámetro nunca se establece, por lo que no hay productos se muestran al ver la página. Lo que debemos hacer es establecer este valor de parámetro en función de la `CategoryID` de la categoría donde ha hecho clic del control de repetidor. Esto presenta dos desafíos: en primer lugar, cómo se determina cuando un LinkButton del repetidor s `ItemTemplate` se ha presionado; y el segundo, ¿cómo podemos determinar el `CategoryID` de la categoría correspondiente se ha hecho clic cuyo LinkButton?

LinkButton similar a los controles de botón y ImageButton tiene un `Click` eventos y un [ `Command` eventos](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.command.aspx). El `Click` evento está diseñado para simplemente tenga en cuenta que se ha hecho clic el LinkButton. En ocasiones, sin embargo, además de tener en cuenta que se ha hecho clic el LinkButton también es necesario pasar cierta información adicional para el controlador de eventos. Si este es el caso, las operaciones de asignación LinkButton [ `CommandName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandname.aspx) y [ `CommandArgument` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandargument.aspx) propiedades se pueden asignar esta información adicional. A continuación, cuando se hace clic en el control LinkButton, su `Command` desencadena el evento (en lugar de su `Click` evento) y el controlador de eventos se pasa los valores de la `CommandName` y `CommandArgument` propiedades.

Cuando un `Command` evento se provoca desde dentro de una plantilla de repetidor, las operaciones de asignación repetidor [ `ItemCommand` eventos](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeater.itemcommand.aspx) se activa y se pasa el `CommandName` y `CommandArgument` valores de lo clic LinkButton (o botón o ImageButton). Por lo tanto, para determinar cuándo se ha hecho clic una categoría LinkButton del repetidor, necesitamos hacer lo siguiente:

1. Establecer el `CommandName` propiedad de LinkButton del repetidor s `ItemTemplate` a algún valor (se ha utilizado ListProducts). Estableciendo este `CommandName` valor, las operaciones de asignación LinkButton `Command` evento se desencadena cuando se hace clic en el control LinkButton.
2. Establecer la s LinkButton `CommandArgument` en el valor del elemento actual s `CategoryID`.
3. Crear un controlador de eventos para el repetidor s `ItemCommand` eventos. Conjunto de eventos de controlador, el `CategoryProductsDataSource` ObjectDataSource s `CategoryID` parámetro en el valor del pasado `CommandArgument`.

El siguiente `ItemTemplate` marcado para categorías repetidor implementa los pasos 1 y 2. Tenga en cuenta cómo el `CommandArgument` valor se asigna el elemento de datos s `CategoryID` mediante sintaxis de enlace de datos:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample11.aspx)]

Cada vez que crea un `ItemCommand` controlador de eventos, es recomendable comprobar siempre primero entrante `CommandName` valor porque *cualquier* `Command` evento desencadenado por *cualquier* Button, LinkButton, o ImageButton dentro del repetidor provocará la `ItemCommand` activación del evento. Aunque actualmente solo tenemos un tal LinkButton ahora, en el futuro se (u otro programador en el equipo) ¿agregar controles de botón adicionales de Web a repetidor que, al hacer clic, genera los mismos `ItemCommand` controlador de eventos. Por lo tanto, se s mejor Asegúrese siempre de que ha activado el `CommandName` propiedad y continuar solo con la lógica de programación si coincide con el valor esperado.

Después de asegurarse de que en el pasado `CommandName` valor es igual a ListProducts, el controlador de eventos, a continuación, asigna la `CategoryProductsDataSource` ObjectDataSource s `CategoryID` parámetro en el valor del pasado `CommandArgument`. Esta modificación a las operaciones de asignación ObjectDataSource `SelectParameters` automáticamente hace que el control DataList a sí mismo enlazar al origen de datos, que muestra los productos de la categoría recién seleccionada.


[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample12.vb)]

Con estas adiciones, nuestro tutorial es completa. Tómese un momento para probarla en un explorador. Figura 14 muestra la pantalla al primero visitar la página. Dado que aún no ha seleccionarse una categoría, no se muestran productos. Al hacer clic en una categoría, como crear, muestra los productos en la categoría de producto en una vista de dos columnas (consulte la figura 15).


[![No hay productos son muestra al primer visitar la página](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image39.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image38.png)

**Figura 14**: los productos No son muestra al primer visitar la página ([haga clic aquí para ver la imagen a tamaño completo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image40.png))


[![Al hacer clic en las listas de categorías de productos los productos correspondientes a la derecha](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image42.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image41.png)

**Figura 15**: al hacer clic en la categoría de productos se enumeran los productos de búsqueda de coincidencias a la derecha ([haga clic aquí para ver la imagen a tamaño completo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image43.png))


## <a name="summary"></a>Resumen

Como hemos visto en este tutorial y la anterior, principal-detalle informes pueden estar repartidos por dos páginas o consolidaron en uno. Mostrar un informe de maestro y detalles en una sola página, sin embargo, presenta algunos desafíos sobre cómo mejor para escribir registros de detalles en la página y el patrón de diseño. En el *principal/detalle con un GridView maestro seleccionable DetailsView detalles* tutorial tuvimos que los registros de detalles aparecen encima de los registros maestros; en este tutorial se usan técnicas CSS para tener la float registros maestros a la a la izquierda de los detalles.

Además de mostrar informes maestra/detalles, también tuvimos la oportunidad de explorar cómo recuperar el número de productos asociados con cada categoría, así como el modo realizar la lógica de servidor cuando cuando un LinkButton (botón o ImageButton) cuando se hace clic en desde un Repetidor.

Este tutorial completa nuestro examen de los informes de maestro y detalles con la DataList y repetidor. El siguiente conjunto de tutoriales ilustrará cómo agregar, editar y eliminar funciones para el control DataList.

Feliz programación.

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Floatutorial](http://css.maxdesign.com.au/floatutorial/) un tutorial sobre flotante elementos CSS con CSS
- [Posicionamiento de CSS](http://www.brainjar.com/css/positioning/) obtener más información sobre la colocación de los elementos con CSS
- [Para presentar Out contenido con HTML](http://www.w3schools.com/html/html_layout.asp) con `<table>` s y otros elementos HTML para determinar la posición

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Revisor inicial para este tutorial era Zack Jones. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](master-detail-filtering-acess-two-pages-datalist-vb.md)
