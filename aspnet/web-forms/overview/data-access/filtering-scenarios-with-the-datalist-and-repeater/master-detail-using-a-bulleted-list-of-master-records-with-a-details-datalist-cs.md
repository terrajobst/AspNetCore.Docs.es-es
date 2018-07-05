---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs
title: Maestro y detalles mediante una lista con viñetas de registros maestros con un control DataList de detalles (C#) | Microsoft Docs
author: rick-anderson
description: En este tutorial se comprimirán el informe de dos páginas maestro y detalles del tutorial anterior en una sola página, que muestra una lista con viñetas de nombres de categoría en t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2006
ms.topic: article
ms.assetid: c727bb73-7b59-41a1-8dc3-623c6d69e7c2
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: 52fd8d1f04b3082250d369b30b5be4db8eac738a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370411"
---
<a name="masterdetail-using-a-bulleted-list-of-master-records-with-a-details-datalist-c"></a>Maestro y detalles mediante una lista con viñetas de registros maestros con un control DataList de detalles (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_35_CS.exe) o [descargar PDF](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/datatutorial35cs1.pdf)

> En este tutorial se comprimirán el informe de dos páginas maestro y detalles del tutorial anterior en una sola página, que muestra una lista con viñetas de nombres de categoría en el lado izquierdo de la pantalla y los productos de la categoría seleccionada a la derecha de la pantalla.


## <a name="introduction"></a>Introducción

En el [tutorial anterior](master-detail-filtering-acess-two-pages-datalist-cs.md) analizamos cómo separar un informe de maestro y detalles en dos páginas. En la página maestra se usa un control Repeater para representar una lista con viñetas de categorías. Cada nombre de categoría era un hipervínculo que, al hacer clic, podría hacer el usuario a la página de detalles, donde un control DataList de dos columnas mostró los productos que pertenecen a la categoría seleccionada.

En este tutorial se comprimirán el tutorial de dos páginas en una sola página, que muestra una lista con viñetas de nombres de categoría en el lado izquierdo de la pantalla con cada nombre de categoría que se representa como un control LinkButton. Al hacer clic en uno de los nombres de categoría LinkButtons induce una devolución de datos y los productos de s de la categoría seleccionada se enlaza a un control DataList de dos columnas de la derecha de la pantalla. Además de mostrar cada nombre de categoría s, el control Repeater a la izquierda muestra cuántos productos total hay son para una categoría determinada (consulte la figura 1).


[![La s nombre de categoría y el número Total de productos se muestran a la izquierda](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image2.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image1.png)

**Figura 1**: s nombre de la categoría y el número Total de productos se muestran a la izquierda ([haga clic aquí para ver imagen en tamaño completo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image3.png))


## <a name="step-1-displaying-a-repeater-in-the-left-portion-of-the-screen"></a>Paso 1: Mostrar un control Repeater en la parte izquierda de la pantalla

En este tutorial necesitamos tener la lista con viñetas de categorías aparecen a la izquierda de los productos de s de la categoría seleccionada. Se puede colocar contenido en una página web mediante etiquetas HTML estándar elementos párrafo, espacios de no separación `<table>` s etc. o a través de técnicas de (CSS) de la hoja de estilos en cascada. Hasta ahora todos nuestros tutoriales han usado las técnicas CSS para el posicionamiento. Cuando se compila la interfaz de usuario de navegación en nuestra página maestra en el [páginas maestras y navegación del sitio](../introduction/master-pages-and-site-navigation-cs.md) tutorial usamos *posicionamiento absoluto*, que indica el desplazamiento de píxeles precisa para el panel de navegación lista y el contenido principal. Como alternativa, se puede usar CSS para colocar un elemento a la derecha o izquierda de la otra a través de *flotante*. Podemos tener la lista con viñetas de categorías aparecen a la izquierda de los productos de s de la categoría seleccionada de forma flotante el control Repeater a la izquierda del control DataList

Abra el `CategoriesAndProducts.aspx` página desde la `DataListRepeaterFiltering` carpeta y agregar a la página un DataList y Repeater. Establecer el control Repeater s `ID` a `Categories` y el control DataList s a `CategoryProducts`. Vaya a la vista del origen y colocar los controles DataList y Repeater dentro de sus propios `<div>` elementos. Es decir, incluya el control Repeater dentro de un `<div>` elemento primero y, a continuación, el control DataList en su propio `<div>` elemento directamente después del repetidor. En este momento el marcado debe ser similar al siguiente:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample1.aspx)]

Para hacer flotar el control Repeater a la izquierda del control DataList, debemos usar la `float` atributo de estilo CSS, así:


[!code-html[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample2.html)]

El `float: left;` flota la primera `<div>` elemento a la izquierda de la otra. El `width` y `padding-right` configuración indica el primer `<div>` s `width` y se agrega relleno entre el `<div>` contenido del elemento s y su margen derecho. Para obtener más información sobre flotantes elementos en CSS desproteger el [Floatutorial](http://css.maxdesign.com.au/floatutorial/).

En lugar de especificar la configuración de estilo directamente a través de la primera `<p>` elemento s `style` atributo, permiten s en su lugar, cree una nueva clase CSS en `Styles.css` denominado `FloatLeft`:


[!code-css[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample3.css)]

A continuación, podemos reemplazar el `<div>` con `<div class="FloatLeft">`.

Después de agregar la clase CSS y configurar el marcado en el `CategoriesAndProducts.aspx` página, vaya al diseñador. Debería ver el control Repeater flotante a la izquierda del control DataList (aunque derecha ambos solo aparecen ahora como candados desde que se ve aún para configurar sus orígenes de datos o plantillas).


[![El control Repeater flota hacia la izquierda del control DataList](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image5.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image4.png)

**Figura 2**: el repetidor flota hacia la izquierda del control DataList ([haga clic aquí para ver imagen en tamaño completo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image6.png))


## <a name="step-2-determining-the-number-of-products-for-each-category"></a>Paso 2: Determinar el número de productos para cada categoría

Con los controles DataList y Repeater s que rodea el marcado completo, está listo para enlazar los datos de categoría para el control Repeater controlamos. Sin embargo, como se muestra en la lista con viñetas de categorías en la figura 1, además de cada nombre de categoría s también es necesario mostrar el número de productos asociados con la categoría. Para obtener acceso a esta información podemos:

- **Determinar esta información de la clase de código subyacente ASP.NET page s.** Dada una determinada *`categoryID`* podemos determinar el número de productos asociados mediante una llamada a la `ProductsBLL` clase s `GetProductsByCategoryID(categoryID)` método. Este método devuelve un `ProductsDataTable` cuyo `Count` propiedad indica cuántos `ProductsRow` s existe, que es el número de productos para el elemento especificado *`categoryID`*. Podemos crear un `ItemDataBound` controlador de eventos para el control Repeater que, para cada categoría enlazado a Repeater, llama a la `ProductsBLL` clase s `GetProductsByCategoryID(categoryID)` método e incluye su recuento en la salida.
- **Actualización de la `CategoriesDataTable` del conjunto de datos con tipo para incluir un `NumberOfProducts` columna.** A continuación, podemos actualizar el `GetCategories()` método en el `CategoriesDataTable` para incluir esta información o bien, o bien, deje `GetCategories()` como-es y cree un nuevo `CategoriesDataTable` método llamado `GetCategoriesAndNumberOfProducts()`.

Permiten s explorar ambas técnicas. El primer enfoque es más fácil de implementar porque no queremos t necesidad de actualizar la capa de acceso a datos; Sin embargo, requiere más de las comunicaciones con la base de datos. La llamada a la `ProductsBLL` clase s `GetProductsByCategoryID(categoryID)` método en el `ItemDataBound` controlador de eventos agrega una llamada de la base de datos adicional para cada categoría que se muestra en el control Repeater. Con esta técnica hay *N* + 1 base de datos de llamadas, donde *N* es el número de categorías que se muestran en el control Repeater. Con el segundo enfoque, se devuelve el recuento de productos con información acerca de cada categoría de la `CategoriesBLL` clase s `GetCategories()` (o `GetCategoriesAndNumberOfProducts()`) método, lo que resulta en un solo viaje a la base de datos.

## <a name="determining-the-number-of-products-in-the-itemdatabound-event-handler"></a>Determinar el número de productos en el controlador de eventos ItemDataBound

Determinar el número de productos de cada categoría en el control Repeater s `ItemDataBound` controlador de eventos no requiere ninguna modificación en la capa de acceso a datos existentes. Se pueden realizar todas las modificaciones directamente en el `CategoriesAndProducts.aspx` página. Empiece agregando un nuevo origen ObjectDataSource denominado `CategoriesDataSource` a través de la etiqueta inteligente del control Repeater s. A continuación, configure el `CategoriesDataSource` ObjectDataSource, por lo que TI recupera sus datos desde el `CategoriesBLL` clase s `GetCategories()` método.


[![Configurar el origen ObjectDataSource para usar la clase CategoriesBLL s GetCategories() (método)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image8.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image7.png)

**Figura 3**: configurar el origen ObjectDataSource que se usarán el `CategoriesBLL` clase s `GetCategories()` método ([haga clic aquí para ver imagen en tamaño completo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image9.png))


Cada elemento de la `Categories` Repeater debe puede hacer clic en y, al hacer clic, hacer que el `CategoryProducts` DataList para mostrar los productos de la categoría seleccionada. Esto puede realizarse mediante la realización de un hipervínculo, vincular a esta misma página de cada categoría (`CategoriesAndProducts.aspx`), pero pasando el `CategoryID` a través de la cadena de consulta, al igual que vimos en el tutorial anterior. La ventaja de este enfoque es que una página que muestra los productos de una categoría determinada s puede ser un marcador e indizada por un motor de búsqueda.

Como alternativa, podemos hacer que cada categoría de un control LinkButton, que es el enfoque que usaremos en este tutorial. Se representa en el explorador del usuario s como un hipervínculo LinkButton pero, al hacer clic, provoca una devolución de datos; en el postback, las operaciones de asignación DataList ObjectDataSource debe actualizarse para mostrar los productos que pertenecen a la categoría seleccionada. Para este tutorial, utilizando un hipervínculo tiene más sentido que el uso de un control LinkButton; Sin embargo, puede haber otros escenarios donde es más conveniente utilizar un control LinkButton. Aunque el enfoque de hipervínculo sería ideal para este ejemplo, permiten s explican cómo utilizar el LinkButton en su lugar. Como veremos, utilizar un control LinkButton presenta algunos desafíos que no se producirá en caso contrario, con un hipervínculo. Por lo tanto, el uso de un control LinkButton en este tutorial se resalte estos desafíos y ayudan a proporcionar soluciones para los escenarios donde nos conviene usar un control LinkButton en lugar de un hipervínculo.

> [!NOTE]
> Se recomienda que repita este tutorial con un control de hipervínculo o `<a>` elemento LinkButton en lugar de.


El marcado siguiente muestra la sintaxis declarativa para el control Repeater y ObjectDataSource. Tenga en cuenta que las plantillas de s Repeater representan una lista con viñetas con cada elemento como un control LinkButton:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample4.aspx)]

> [!NOTE]
> En este tutorial, el control Repeater debe tener su estado de vista habilitado (tenga en cuenta la omisión de la `EnableViewState="False"` desde la sintaxis declarativa del control Repeater s). En el paso 3 crearemos un controlador de eventos para el control Repeater s `ItemCommand` eventos en el que actualizaremos el control DataList s s ObjectDataSource `SelectParameters` colección. El control Repeater s `ItemCommand`; no obstante, ganó el fuego t si se deshabilita el estado de vista. Consulte [más difíciles A una pregunta ASP.NET](http://scottonwriting.net/sowblog/posts/1263.aspx) y [su solución](http://scottonwriting.net/sowBlog/posts/1268.aspx) para obtener más información sobre por qué el estado de vista debe estar habilitado para un control Repeater s `ItemCommand` activación del evento.


El control LinkButton con el `ID` el valor de propiedad `ViewCategory` no tiene su `Text` conjunto de propiedades. Si hubiéramos sólo deseamos mostrar el nombre de categoría, habría establecemos la propiedad Text de forma declarativa, mediante la sintaxis de enlace de datos, así:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample5.aspx)]

Sin embargo, queremos mostrar el nombre de categoría s *y* el número de productos que pertenecen a esa categoría. Esta información se puede recuperar desde el control Repeater s `ItemDataBound` controlador de eventos mediante una llamada a la `ProductBLL` clase s `GetCategoriesByProductID(categoryID)` método y determinar cuántos registros se devuelven en el cuadro `ProductsDataTable`, como el código siguiente se muestra:


[!code-csharp[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample6.cs)]

Empezamos por lo que garantiza que se está trabajando con un elemento de datos (uno cuyo `ItemType` es `Item` o `AlternatingItem`) y, a continuación, hacer referencia a la `CategoriesRow` instancia que solo se ha enlazado a la actual `RepeaterItem`. A continuación, determinar la cantidad de productos de esta categoría mediante la creación de una instancia de la `ProductsBLL` clase, una llamada a su `GetCategoriesByProductID(categoryID)` método y determinar el número de registros devueltos usando la `Count` propiedad. Por último, el `ViewCategory` LinkButton en ItemTemplate es references y su `Text` propiedad está establecida en *CategoryName* (*NumberOfProductsInCategory*), donde  *NumberOfProductsInCategory* se da formato como un número con cero posiciones decimales.

> [!NOTE]
> Como alternativa, podríamos agregaron una *formato función* a la clase de código subyacente de s de la página ASP.NET que acepta una categoría s `CategoryName` y `CategoryID` valores y devuelve el `CategoryName` concatenado con el número de productos de la categoría (según lo determinado por una llamada a la `GetCategoriesByProductID(categoryID)` método). Los resultados de dicha función de formato se podrían asignar mediante declaración a la propiedad de texto, reemplazando la necesidad de s de LinkButton el `ItemDataBound` controlador de eventos. Hacer referencia a la [uso de TemplateFields en el GridView Control](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) o [dar formato a los controles DataList y Repeater basándose en datos](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs.md) tutoriales para obtener más información sobre el uso de funciones de formato.


Después de agregar este controlador de eventos, dedique un momento para probar la página a través de un explorador. Tenga en cuenta cómo cada categoría aparece en una lista con viñetas, mostrar el nombre de categoría s y el número de productos asociados con la categoría (consulte la figura 4).


[![Se muestran cada s nombre de categoría y el número de productos](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image11.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image10.png)

**Figura 4**: s nombre de cada categoría y el número de productos se muestran ([haga clic aquí para ver imagen en tamaño completo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image12.png))


## <a name="updating-thecategoriesdatatableandcategoriestableadapterto-include-the-number-of-products-for-each-category"></a>Actualizando el`CategoriesDataTable`y`CategoriesTableAdapter`para incluir el número de productos para cada categoría

En lugar de determinar el número de productos de cada categoría tal y como s enlazado a Repeater, podemos simplificar el proceso ajustando el `CategoriesDataTable` y `CategoriesTableAdapter` en la capa de acceso a datos para incluir esta información de forma nativa. Para lograr esto, debemos agregar una nueva columna a `CategoriesDataTable` para contener el número de productos asociados. Para agregar una nueva columna a un objeto DataTable, abra el conjunto de datos con tipo (`App_Code\DAL\Northwind.xsd`), haga doble clic en la DataTable para modificar y elija Agregar o columna. Agregar una nueva columna a la `CategoriesDataTable` (consulte la figura 5).


[![Agregar una nueva columna a la CategoriesDataSource](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image14.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image13.png)

**Figura 5**: agregar una nueva columna a la `CategoriesDataSource` ([haga clic aquí para ver imagen en tamaño completo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image15.png))


Esto agregará una nueva columna denominada `Column1`, que se puede cambiar simplemente escribiendo un nombre distinto. Cambiar el nombre de esta nueva columna a `NumberOfProducts`. A continuación, se debe configurar las propiedades de esta columna s. Haga clic en la nueva columna y vaya a la ventana Propiedades. Cambiar la columna s `DataType` propiedad desde `System.String` a `System.Int32` y establezca el `ReadOnly` propiedad `True`, como se muestra en la figura 6.


![Establecer las propiedades de solo lectura de la nueva columna y tipo de datos](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image16.png)

**Figura 6**: establecer el `DataType` y `ReadOnly` propiedades de la nueva columna


Mientras el `CategoriesDataTable` tiene ahora un `NumberOfProducts` columna, su valor no se establece por cualquiera de las consultas de s TableAdapter correspondientes. Podemos actualizar el `GetCategories()` devuelto de método para devolver esta información si deseamos que dicha información cada vez que se recupera información de categoría. Si, sin embargo, únicamente se necesita tomar el número de productos asociados para las categorías en raras ocasiones (por ejemplo, tan solo para este tutorial), entonces podemos dejar `GetCategories()` como-es y cree un nuevo método que devuelve esta información. S permiten usar este último enfoque, crear un nuevo método denominado `GetCategoriesAndNumberOfProducts()`.

Para agregar este nuevo `GetCategoriesAndNumberOfProducts()` método, el botón secundario en el `CategoriesTableAdapter` y elija la nueva consulta. Se abrirá el TableAdapter consultar a Configuration Wizard, que se ve que usa varias veces en los tutoriales anteriores. Para este método, inicie al asistente que indica que la consulta utiliza una instrucción SQL ad hoc que devuelve filas.


[![Crear el método mediante una instrucción de SQL Ad Hoc](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image18.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image17.png)

**Figura 7**: crear el método mediante una instrucción de SQL Ad Hoc ([haga clic aquí para ver imagen en tamaño completo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image19.png))


[![La instrucción SQL devuelve filas](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image21.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image20.png)

**Figura 8**: The SQL instrucción devuelve filas ([haga clic aquí para ver imagen en tamaño completo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image22.png))


La siguiente pantalla del asistente pide us para la consulta que se utilizará. Para devolver cada categoría s `CategoryID`, `CategoryName`, y `Description` campos, junto con el número de productos asociados a la categoría, use el siguiente `SELECT` instrucción:


[!code-sql[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample7.sql)]


[![Especifique la consulta que Use](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image24.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image23.png)

**Figura 9**: especifique la consulta que Use ([haga clic aquí para ver imagen en tamaño completo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image25.png))


Tenga en cuenta que la subconsulta que calcula el número de productos asociados con la categoría es un alias como `NumberOfProducts`. Esta coincidencia nomenclatura hace que el valor devuelto por esta subconsulta se asocie a la `CategoriesDataTable` s `NumberOfProducts` columna.

Después de escribir esta consulta, el último paso es elegir el nombre para el nuevo método. Use `FillWithNumberOfProducts` y `GetCategoriesAndNumberOfProducts` para rellenar una DataTable y devuelven un objeto DataTable patrones, respectivamente.


[![Nombre de la nueva FillWithNumberOfProducts de métodos de TableAdapter s y GetCategoriesAndNumberOfProducts](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image27.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image26.png)

**Figura 10**: nombre de los métodos de nuevo TableAdapter s `FillWithNumberOfProducts` y `GetCategoriesAndNumberOfProducts` ([haga clic aquí para ver imagen en tamaño completo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image28.png))


En este momento la capa de acceso a datos se ha ampliado para incluir el número de productos por categoría. Puesto que nuestro de capa de presentación enruta todas las llamadas a la capa DAL a través de una capa de lógica de negocios independientes se debe agregar el correspondiente `GetCategoriesAndNumberOfProducts` método a la `CategoriesBLL` clase:


[!code-csharp[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample8.cs)]

Con DAL y BLL completa, hemos re está listo para enlazar estos datos para el `Categories` Repeater en `CategoriesAndProducts.aspx`! Si ve ya ha creado un origen ObjectDataSource para el control Repeater desde el determinar el número de productos en el `ItemDataBound` sección del controlador de eventos, eliminar este origen ObjectDataSource y quitar el control Repeater s `DataSourceID` propiedad también configuración; sin líos el Repeater s `ItemDataBound` evento desde el controlador de eventos mediante la eliminación de la `Handles Categories.OnItemDataBound` sintaxis en la clase de código subyacente ASP.NET.

Con el control Repeater en su estado original, agregue un nuevo origen ObjectDataSource denominado `CategoriesDataSource` a través de la etiqueta inteligente del control Repeater s. Configurar el origen ObjectDataSource para usar el `CategoriesBLL` (clase), pero en lugar de tener usar el `GetCategories()` método, haya que usar `GetCategoriesAndNumberOfProducts()` en su lugar (consulte la figura 11).


[![Configurar el origen ObjectDataSource para usar el método GetCategoriesAndNumberOfProducts](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image30.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image29.png)

**Figura 11**: configurar el origen ObjectDataSource que se usarán el `GetCategoriesAndNumberOfProducts` método ([haga clic aquí para ver imagen en tamaño completo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image31.png))


A continuación, actualice el `ItemTemplate` para que las operaciones de asignación LinkButton `Text` propiedad se asigna mediante declaración usando sintaxis de enlace de datos e incluye tanto el `CategoryName` y `NumberOfProducts` campos de datos. El marcado declarativo completo para el control Repeater y `CategoriesDataSource` ObjectDataSource seguimiento:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample9.aspx)]

El resultado representado mediante la actualización de la capa DAL para incluir un `NumberOfProducts` columna es lo mismo que usar el `ItemDataBound` enfoque de controlador de eventos (hacer referencia a la figura 4 para ver una pantalla de captura del repetidor que muestra los nombres de categoría y el número de productos).

## <a name="step-3-displaying-the-selected-category-s-products"></a>Paso 3: Mostrar los productos de s de la categoría seleccionada

En este momento, tenemos la `Categories` Repeater mostrar la lista de categorías, junto con el número de productos en cada categoría. El control Repeater usa un LinkButton para cada categoría que, al hacer clic, hace una devolución de datos, en el que elija se necesita mostrar los productos de la categoría seleccionada en el `CategoryProducts` DataList.

Un desafío que nos encontramos es cómo hacer que el control DataList mostrar solo esos productos para la categoría seleccionada. En el [Master/Detail mediante un GridView maestro seleccionable con un DetailsView detalles](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) tutorial hemos visto cómo crear un control GridView cuyas filas se pudieron seleccionar, con la fila seleccionada s detalles que se muestran en DetailsView en la misma página. El control GridView ObjectDataSource devolverá información sobre todos los productos con el `ProductsBLL` s `GetProducts()` método mientras la s DetailsView ObjectDataSource recupera información sobre el producto seleccionado a través la `GetProductsByProductID(productID)` método. El *`productID`* el valor del parámetro se proporciona mediante declaración mediante la asociación con el valor de la s GridView `SelectedValue` propiedad. Por desgracia, no tiene el control Repeater un `SelectedValue` propiedad y no puede actuar como un origen de los parámetros.

> [!NOTE]
> Esto es uno de los desafíos que aparecen cuando se usa el control LinkButton en un control Repeater. Si hubiéramos utilizado un hipervínculo para pasar el `CategoryID` a través de la cadena de consulta en su lugar, podríamos usar ese campo de cadena de consulta como origen para el valor del parámetro.


Antes de que preocuparse por la falta de un `SelectedValue` para el control Repeater, sin embargo, let de la propiedad s primero enlazar el control DataList a un origen ObjectDataSource y especificar su `ItemTemplate`.

En la etiqueta inteligente de DataList s, optar por agregar un nuevo origen ObjectDataSource denominado `CategoryProductsDataSource` y configúrelo para utilizar el `ProductsBLL` clase s `GetProductsByCategoryID(categoryID)` método. Puesto que el control DataList en este tutorial ofrece una interfaz de solo lectura, no dude en establecer las listas desplegables en la instrucción INSERT, UPDATE y eliminar las pestañas en (None).


[![Configurar el origen ObjectDataSource para usar el método de clase ProductsBLL s GetProductsByCategoryID(categoryID)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image33.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image32.png)

**Figura 12**: configurar el origen ObjectDataSource para uso `ProductsBLL` clase s `GetProductsByCategoryID(categoryID)` método ([haga clic aquí para ver imagen en tamaño completo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image34.png))


Puesto que la `GetProductsByCategoryID(categoryID)` método espera un parámetro de entrada (*`categoryID`*), el Asistente para configurar orígenes de datos nos permite especificar el origen del parámetro s. Tenían se han enumerado las categorías en un control DataList o en un control GridView, d establecemos la lista desplegable de origen de parámetro en el Control y la ControlID a la `ID` de los datos de control Web. Sin embargo, como la falta de Repeater un `SelectedValue` no se puede usar como un origen de los parámetros de propiedad. Si comprueba, encontrará que la lista desplegable iDControl solo contiene un control `ID``CategoryProducts`, el `ID` del control DataList.

Por ahora, establezca la lista desplegable de origen de parámetro en ninguno. Terminará asignar mediante programación el valor de este parámetro cuando una categoría que se hace clic en LinkButton en el control Repeater.


[![Hacer no especificar un origen de los parámetros para el parámetro categoryID](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image36.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image35.png)

**Figura 13**: no se especifica un origen de los parámetros para el *`categoryID`* parámetro ([haga clic aquí para ver imagen en tamaño completo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image37.png))


Después de completar el Asistente para configurar orígenes de datos, Visual Studio genera automáticamente el control DataList s `ItemTemplate`. Reemplace este valor predeterminado `ItemTemplate` con la plantilla se usa en el tutorial anterior; además, establezca el control DataList s `RepeatColumns` propiedad en 2. Después de realizar estos cambios en el marcado declarativo para los controles DataList y su origen ObjectDataSource asociado debe ser similar al siguiente:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample10.aspx)]

Actualmente, el `CategoryProductsDataSource` ObjectDataSource s *`categoryID`* parámetro nunca se establece, por lo que no hay productos se muestran al ver la página. Lo que debemos hacer es establecer este valor de parámetro en función de la `CategoryID` de la categoría donde ha hecho clic en el control Repeater. Esto presenta dos desafíos: en primer lugar, ¿cómo se determina cuando un control LinkButton en el control Repeater s `ItemTemplate` se ha hecho clic en él; y el segundo, cómo podemos determinar el `CategoryID` de la categoría correspondiente se hizo clic cuyo LinkButton?

Igual que los controles de botón e ImageButton LinkButton tiene un `Click` eventos y un [ `Command` eventos](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.command.aspx). El `Click` evento está diseñado para simplemente tenga en cuenta que el control LinkButton se ha hecho clic. En ocasiones, sin embargo, además de tener en cuenta que se ha hecho clic LinkButton también debemos pasar cierta información adicional para el controlador de eventos. Si es así, la s LinkButton [ `CommandName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandname.aspx) y [ `CommandArgument` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandargument.aspx) propiedades se pueden asignar a esta información adicional. A continuación, cuando se hace clic en LinkButton, su `Command` desencadena el evento (en lugar de su `Click` eventos) y el controlador de eventos se pasa los valores de la `CommandName` y `CommandArgument` propiedades.

Cuando un `Command` evento se genera desde dentro de una plantilla en el control Repeater, la s Repeater [ `ItemCommand` eventos](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeater.itemcommand.aspx) se activa y se pasa el `CommandName` y `CommandArgument` valores de LinkButton hace clic en él (o botón o ImageButton). Por lo tanto, para determinar cuándo se ha hecho clic una categoría LinkButton en el control Repeater, necesitamos hacer lo siguiente:

1. Establecer el `CommandName` propiedad de LinkButton en el control Repeater s `ItemTemplate` a algún valor (ha utilizado ListProducts). Si establece esta configuración `CommandName` valor, la s LinkButton `Command` evento se desencadena cuando se hace clic en LinkButton.
2. Establezca la s LinkButton `CommandArgument` en el valor del elemento actual s `CategoryID`.
3. Crear un controlador de eventos para el control Repeater s `ItemCommand` eventos. Conjunto de eventos de controlador, el `CategoryProductsDataSource` ObjectDataSource s `CategoryID` parámetro en el valor de en el pasado `CommandArgument`.

La siguiente `ItemTemplate` marcado para categorías Repeater implementa los pasos 1 y 2. Tenga en cuenta cómo el `CommandArgument` se asigna el valor del elemento de datos s `CategoryID` mediante la sintaxis de enlace de datos:


[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample11.aspx)]

Cada vez que la creación de un `ItemCommand` controlador de eventos, es recomendable comprobar siempre primero entrante `CommandName` valor porque *cualquier* `Command` evento desencadenado por *cualquier* Button, LinkButton, o ImageButton del control Repeater provocará la `ItemCommand` activación del evento. Aunque actualmente solo tenemos uno tal LinkButton ahora, en el futuro nos (u otro programador de nuestro equipo) podría agregar controles de botón adicionales de Web a Repeater que, al hacer clic, genera el mismo `ItemCommand` controlador de eventos. Por lo tanto, lo s mejor siempre, asegúrese de comprobar la `CommandName` propiedad y continuar solo con la lógica de programación si coincide con el valor esperado.

Después de asegurarse de que en el pasado `CommandName` valor es igual a ListProducts, el controlador de eventos, a continuación, asigna la `CategoryProductsDataSource` ObjectDataSource s `CategoryID` parámetro en el valor de en el pasado `CommandArgument`. Esta modificación al origen ObjectDataSource s `SelectParameters` automáticamente hace que el control DataList a sí misma volver a enlazar al origen de datos, que muestra los productos de la categoría recién seleccionada.


[!code-csharp[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/samples/sample12.cs)]

Con estas incorporaciones, nuestro tutorial es completa. Dedique un momento para comenzar a probarlo en un explorador. Figura 14 se muestra la pantalla cuando se visita primero la página. Puesto que todavía debe seleccionarse una categoría, no hay productos se muestran. Al hacer clic en una categoría, como generar, muestra los productos en la categoría de producto en una vista en dos columnas (consulte la figura 15).


[![No hay productos son muestra al primer visitar la página](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image39.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image38.png)

**Figura 14**: productos No están muestra al primer visitar la página ([haga clic aquí para ver imagen en tamaño completo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image40.png))


[![Al hacer clic en las listas de categoría de producir los productos correspondientes a la derecha](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image42.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image41.png)

**Figura 15**: al hacer clic en la categoría de productos se enumeran los productos coincidentes a la derecha ([haga clic aquí para ver imagen en tamaño completo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs/_static/image43.png))


## <a name="summary"></a>Resumen

Como hemos visto en este tutorial y el anterior, los informes de maestro y detalles pueden estar repartidos por dos páginas o consolidaron en uno. Muestra un informe de maestro y detalles en una sola página, sin embargo, presenta algunos desafíos acerca de cómo mejor el patrón de diseño y los registros de detalles de la página. En el *Master/Detail mediante un GridView maestro seleccionable con un DetailsView detalles* tutorial teníamos los registros de detalles aparecerá por encima de los registros maestros; en este tutorial se usan técnicas CSS para tener el valor float de registros maestros para el a la izquierda de los detalles.

Además de mostrar los informes de maestro y detalles, también se ha tenido la oportunidad para explorar cómo recuperar el número de productos asociados con cada categoría, así como la forma realizar la lógica del lado servidor cuando un control LinkButton (o botón o ImageButton) se hace clic en desde un Control de repetición.

Este tutorial finaliza el examen de los informes de maestro y detalles con los controles DataList y Repeater. El siguiente conjunto de tutoriales explica cómo agregar, modificar y eliminar capacidades para el control DataList.

Feliz programación.

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Floatutorial](http://css.maxdesign.com.au/floatutorial/) un tutorial sobre flotante elementos CSS con CSS
- [Posicionamiento de CSS](http://www.brainjar.com/css/positioning/) obtener más información sobre la colocación de los elementos con CSS
- [Diseño horizontal contenido con HTML](http://www.w3schools.com/html/html_layout.asp) mediante `<table>` s y otros elementos HTML para el posicionamiento

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Revisor de clientes potencial para este tutorial era Zack Jones. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](master-detail-filtering-acess-two-pages-datalist-cs.md)
> [Siguiente](master-detail-filtering-with-a-dropdownlist-datalist-vb.md)
