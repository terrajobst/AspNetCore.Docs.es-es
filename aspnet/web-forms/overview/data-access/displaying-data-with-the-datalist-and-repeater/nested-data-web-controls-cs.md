---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-cs
title: Datos anidados Web controles (C#) | Microsoft Docs
author: rick-anderson
description: En este tutorial que exploraremos cómo usar un Repeater anidado dentro de otro Repeater. Los ejemplos ilustran cómo rellenar el control Repeater interno ambos d...
ms.author: riande
ms.date: 09/13/2006
ms.assetid: ad3cb0ec-26cf-42d7-b81b-184a34ec9f86
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 032321b5cf5323058c114e652512854f9866d447
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835952"
---
<a name="nested-data-web-controls-c"></a>Controles Web de datos anidados (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_32_CS.exe) o [descargar PDF](nested-data-web-controls-cs/_static/datatutorial32cs1.pdf)

> En este tutorial que exploraremos cómo usar un Repeater anidado dentro de otro Repeater. Los ejemplos ilustran cómo rellenar la interno Repeater mediante declaración y mediante programación.


## <a name="introduction"></a>Introducción

Además de HTML estático y la sintaxis de enlace de datos, también pueden incluir plantillas de controles Web y controles de usuario. Estos controles Web pueden tener sus propiedades asignadas a través de la sintaxis declarativa, de enlace de datos, o puede tener acceso mediante programación en los controladores de eventos de servidor adecuado.

Mediante la incrustación de controles dentro de una plantilla, se puede personalizar la experiencia del usuario y la apariencia y mejorada. Por ejemplo, en el [uso de TemplateFields en el GridView Control](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) tutorial, vimos cómo personalizar la presentación de GridView s mediante la adición de un control de calendario en TemplateField para mostrar un empleado fecha de contratación s; en el [agregar Los controles de validación a las Interfaces de inserción y edición](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) y [personalizar la interfaz de modificación de datos](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) tutoriales, hemos visto cómo personalizar la edición e inserción interfaces mediante la adición de validación los controles, cuadros de texto, listas desplegables y otros controles Web.

Las plantillas también pueden contener otros controles Web de datos. Es decir, podemos tenemos un control DataList que contiene otro control DataList (o Repeater o GridView o DetailsView y así sucesivamente) dentro de sus plantillas. El desafío de esta interfaz enlaza los datos apropiados para el control Web de datos internos. Existen varios enfoques diferentes, que abarcan desde las opciones declarativas a través de ObjectDataSource mediante programación los.

En este tutorial que exploraremos cómo usar un Repeater anidado dentro de otro Repeater. El control Repeater externo contendrá un elemento para cada categoría en la base de datos, mostrar el nombre de categoría s y la descripción. Cada elemento de categoría s Repeater interna mostrará la información de cada producto que pertenecen a esa categoría (consulte la figura 1) en una lista con viñetas. Nuestros ejemplos ilustran cómo rellenar la interno Repeater mediante declaración y mediante programación.


[![Cada categoría, junto con sus productos, se muestran](nested-data-web-controls-cs/_static/image2.png)](nested-data-web-controls-cs/_static/image1.png)

**Figura 1**: cada categoría, junto con sus productos, se enumeran ([haga clic aquí para ver imagen en tamaño completo](nested-data-web-controls-cs/_static/image3.png))


## <a name="step-1-creating-the-category-listing"></a>Paso 1: Creación de la lista de categoría

Cuando la creación de una página que usa anidados controles Web de datos, le resulte útil para el diseño, crear y probar el control Web de datos más externa en primer lugar, sin preocuparse incluso control anidado interno. Por lo tanto, que pueda s empezar a lo largo de los pasos necesarios para agregar un control Repeater a la página que muestra el nombre y descripción de cada categoría.

Comience abriendo la `NestedControls.aspx` página en el `DataListRepeaterBasics` carpeta y agregue un control Repeater a la página de configuración de su `ID` propiedad `CategoryList`. En la etiqueta inteligente de Repeater s, optar por crear un nuevo origen ObjectDataSource denominado `CategoriesDataSource`.


[![Nombre de la nueva ObjectDataSource CategoriesDataSource](nested-data-web-controls-cs/_static/image5.png)](nested-data-web-controls-cs/_static/image4.png)

**Figura 2**: nombre del nuevo origen ObjectDataSource `CategoriesDataSource` ([haga clic aquí para ver imagen en tamaño completo](nested-data-web-controls-cs/_static/image6.png))


Configurar el origen ObjectDataSource para que extrae sus datos de la `CategoriesBLL` clase s `GetCategories` método.


[![Configurar el origen ObjectDataSource para usar el método de clase CategoriesBLL s GetCategories](nested-data-web-controls-cs/_static/image8.png)](nested-data-web-controls-cs/_static/image7.png)

**Figura 3**: configurar el origen ObjectDataSource que se usarán el `CategoriesBLL` clase s `GetCategories` método ([haga clic aquí para ver imagen en tamaño completo](nested-data-web-controls-cs/_static/image9.png))


Para especificar la plantilla de repetidor s contenido necesitamos ir a la vista del origen y escribir manualmente la sintaxis declarativa. Agregar un `ItemTemplate` que muestra el nombre de categoría s en un `<h4>` elemento y la descripción de categoría s en un elemento de párrafo (`<p>`). Además, permiten s separar cada categoría con una regla horizontal (`<hr>`). Después de realizar estos cambios en la página debe contener sintaxis declarativa para el control Repeater y ObjectDataSource que es similar al siguiente:


[!code-aspx[Main](nested-data-web-controls-cs/samples/sample1.aspx)]

Figura 4 muestra nuestro progreso cuando se ve mediante un explorador.


[![Se muestran cada s nombre de categoría y descripción, separada por una regla Horizontal](nested-data-web-controls-cs/_static/image11.png)](nested-data-web-controls-cs/_static/image10.png)

**Figura 4**: s nombre de cada categoría y descripción se muestra, separados por una regla Horizontal ([haga clic aquí para ver imagen en tamaño completo](nested-data-web-controls-cs/_static/image12.png))


## <a name="step-2-adding-the-nested-product-repeater"></a>Paso 2: Agregar producto anidada Repeater

Con la categoría de lista completa, la siguiente tarea es agregar un control Repeater para la `CategoryList` s `ItemTemplate` que muestra información sobre los productos que pertenecen a la categoría correspondiente. Hay varias maneras que podemos recuperar los datos para este Repeater interna, dos de los cuales exploraremos en breve. Por ahora, s permiten crear solo los productos Repeater dentro de la `CategoryList` Repeater s `ItemTemplate`. En concreto, permite que s tengan la presentación Repeater cada producto en una lista con viñetas con cada elemento de la lista incluye el nombre de producto s y el precio del producto.

Para crear este Repeater se tengan que escribir manualmente la sintaxis declarativa del control Repeater s interna y las plantillas en el `CategoryList` s `ItemTemplate`. Agregue el siguiente marcado dentro de la `CategoryList` Repeater s `ItemTemplate`:


[!code-aspx[Main](nested-data-web-controls-cs/samples/sample2.aspx)]

## <a name="step-3-binding-the-category-specific-products-to-the-productsbycategorylist-repeater"></a>Paso 3: Enlace de los productos de específica de la categoría a Repeater ProductsByCategoryList

Si visita la página a través de un explorador en este momento, la pantalla será el mismo que en la figura 4 porque se ve aún para enlazar los datos para el control Repeater. Hay varias maneras de que podemos tomar los registros de producto apropiadas y enlazarlos a Repeater, algo más eficaces que otros. El principal desafío es obtener los productos adecuados para la categoría especificada.

Los datos que se va a enlazar al control Repeater interno o bien pueden obtenerse mediante declaración, a través de un origen ObjectDataSource en el `CategoryList` Repeater s `ItemTemplate`, o mediante programación, desde la página de código subyacente de s de la página ASP.NET. De forma similar, estos datos pueden estar limitados por el control Repeater interno ya sea mediante declaración - mediante el control Repeater interno s `DataSourceID` propiedad o a través de la sintaxis de enlace de datos declarativo, o mediante programación haciendo referencia a Repeater interno en el `CategoryList` Repeater s `ItemDataBound` controlador de eventos, establecer mediante programación su `DataSource` propiedad y una llamada a su `DataBind()` método. Permiten s explorar cada uno de estos enfoques.

## <a name="accessing-the-data-declaratively-with-an-objectdatasource-control-and-theitemdataboundevent-handler"></a>Acceso a los datos mediante declaración con un Control ObjectDataSource y`ItemDataBound`controlador de eventos

Desde que se ha utilizado el origen ObjectDataSource ampliamente a lo largo de esta serie de tutoriales, la elección más natural para tener acceso a datos para este ejemplo es quedarse con el origen ObjectDataSource. El `ProductsBLL` clase tiene un `GetProductsByCategoryID(categoryID)` método que devuelve información acerca de los productos que pertenecen a la especificada *`categoryID`*. Por lo tanto, podemos agregar un origen ObjectDataSource a los `CategoryList` Repeater s `ItemTemplate` y configurarla para acceder a sus datos desde este método de clase s.

Desafortunadamente, el repetidor permitir sus plantillas de a editarse a través de la vista de diseño, por lo que necesitamos agregar manualmente la sintaxis declarativa para este control ObjectDataSource. La sintaxis siguiente muestra la `CategoryList` Repeater s `ItemTemplate` después de agregar este nuevo origen ObjectDataSource (`ProductsByCategoryDataSource`):


[!code-aspx[Main](nested-data-web-controls-cs/samples/sample3.aspx)]

Cuando se usa el enfoque de ObjectDataSource se debe establecer el `ProductsByCategoryList` Repeater s `DataSourceID` propiedad a la `ID` del origen ObjectDataSource (`ProductsByCategoryDataSource`). Además, tenga en cuenta que nuestro ObjectDataSource tiene un `<asp:Parameter>` elemento que especifica el *`categoryID`* valor que se pasarán a la `GetProductsByCategoryID(categoryID)` método. Pero, ¿cómo se especifica este valor? Idealmente, d, podremos basta con establecer la `DefaultValue` propiedad de la `<asp:Parameter>` elemento mediante sintaxis de enlace de datos, así:


[!code-aspx[Main](nested-data-web-controls-cs/samples/sample4.aspx)]

Por desgracia, la sintaxis de enlace de datos solo es válida en los controles que tienen un `DataBinding` eventos. La `Parameter` clase no tiene este tipo de evento y, por tanto, la sintaxis anterior es válida y se producirá un error en tiempo de ejecución.

Para establecer este valor, es necesario crear un controlador de eventos para el `CategoryList` Repeater s `ItemDataBound` eventos. Recuerde que el `ItemDataBound` evento se desencadena una vez para cada elemento enlazado a Repeater. Por lo tanto, cada vez que este evento se desencadena para el control Repeater externo, podemos asignamos actual `CategoryID` valor para el `ProductsByCategoryDataSource` ObjectDataSource s `CategoryID` parámetro.

Crear un controlador de eventos para el `CategoryList` Repeater s `ItemDataBound` evento con el código siguiente:


[!code-csharp[Main](nested-data-web-controls-cs/samples/sample5.cs)]

Este controlador de eventos inicia asegurándose de que nos re tratar con datos de un elemento en lugar del elemento de encabezado, pie de página o separador. A continuación, hacemos referencia real `CategoriesRow` instancia que solo se ha enlazado a la actual `RepeaterItem`. Por último, hacemos referencia al origen ObjectDataSource en el `ItemTemplate` y asignar su `CategoryID` valor de parámetro para el `CategoryID` del actual `RepeaterItem`.

Con este controlador de eventos, el `ProductsByCategoryList` Repeater en cada `RepeaterItem` está enlazado a esos productos en el `RepeaterItem` categoría s. Figura 5 muestra una captura de pantalla de la salida resultante.


[![El control Repeater externo enumera cada categoría; Lo interna enumera los productos de la categoría](nested-data-web-controls-cs/_static/image14.png)](nested-data-web-controls-cs/_static/image13.png)

**Figura 5**: The externa Repeater enumera cada categoría; las listas de uno interno los productos de esa categoría ([haga clic aquí para ver imagen en tamaño completo](nested-data-web-controls-cs/_static/image15.png))


## <a name="accessing-the-products-by-category-data-programmatically"></a>Acceso a los productos por categoría datos mediante programación

En lugar de un origen ObjectDataSource para recuperar los productos para la categoría actual, podríamos creamos un método en la clase de código subyacente ASP.NET página s (o en el `App_Code` carpeta o en un proyecto de biblioteca de clases independiente) que devuelve el conjunto adecuado de productos cuando se pasa en un `CategoryID`. Imagine que teníamos que este tipo de método en la clase de código subyacente de s de la página ASP.NET y que se denomina `GetProductsInCategory(categoryID)`. Con este método en su lugar podríamos enlazamos los productos para la categoría actual a Repeater interno con la siguiente sintaxis declarativa:


[!code-aspx[Main](nested-data-web-controls-cs/samples/sample6.aspx)]

El control Repeater s `DataSource` propiedad utiliza la sintaxis de enlace de datos para indicar que sus datos proceden de la `GetProductsInCategory(categoryID)` método. Puesto que `Eval("CategoryID")` devuelve un valor de tipo `Object`, se puede convertir el objeto a un `Integer` antes de pasarlo en el `GetProductsInCategory(categoryID)` método. Tenga en cuenta que el `CategoryID` a los que accede este a través del enlace de datos de la sintaxis es la `CategoryID` en el *externa* Repeater (`CategoryList`), la s enlazados a los registros de la `Categories` tabla. Por lo tanto, sabemos que `CategoryID` no puede ser una base de datos `NULL` valor, que es la razón por la podemos convertir ciegamente el `Eval` método sin comprobar si se re tratar con un `DBNull`.

Con este enfoque, es necesario crear la `GetProductsInCategory(categoryID)` método y recuperar el conjunto adecuado de productos dada proporcionado *`categoryID`*. Podemos hacer esto devolviendo simplemente la `ProductsDataTable` devuelto por la `ProductsBLL` clase s `GetProductsByCategoryID(categoryID)` método. Permitir s crear el `GetProductsInCategory(categoryID)` método en la clase de código subyacente para nuestra `NestedControls.aspx` página. Lo hacen mediante el código siguiente:


[!code-csharp[Main](nested-data-web-controls-cs/samples/sample7.cs)]

Este método simplemente crea una instancia de la `ProductsBLL` método y devuelve los resultados de la `GetProductsByCategoryID(categoryID)` método. Tenga en cuenta que el método debe marcarse `Public` o `Protected`; si el método se marca `Private`, no será accesible desde el marcado declarativo de s de la página ASP.NET.

Después de realizar estos cambios para usar esta técnica nueva, dedique un momento para ver la página mediante un explorador. La salida debería ser idéntica a la salida cuando se utiliza ObjectDataSource y `ItemDataBound` enfoque de controlador de eventos (consulte volver figura 5 para ver la captura de pantalla).

> [!NOTE]
> Esto puede parecer tareas improductivas para crear el `GetProductsInCategory(categoryID)` método en la clase de código subyacente de s de la página ASP.NET. Después de todo, este método simplemente crea una instancia de la `ProductsBLL` clase y devuelve los resultados de su `GetProductsByCategoryID(categoryID)` método. ¿Por qué no simplemente llamar a este método directamente desde la sintaxis de enlace de datos en el control Repeater interna, como: `DataSource='<%# ProductsBLL.GetProductsByCategoryID((int)(Eval("CategoryID"))) %>'`? Aunque esta sintaxis ganó funcionará con la implementación actual de la `ProductsBLL` clase (puesto que el `GetProductsByCategoryID(categoryID)` es un método de instancia), podría modificar `ProductsBLL` para incluir una variable static `GetProductsByCategoryID(categoryID)` método o que la clase incluya estático`Instance()` método para devolver una nueva instancia de la `ProductsBLL` clase.


Aunque estas modificaciones eliminaría la necesidad de la `GetProductsInCategory(categoryID)` método en la clase de código subyacente ASP.NET página s, el método de clase de código subyacente nos proporciona mayor flexibilidad para trabajar con los datos recuperados, como veremos en breve.

## <a name="retrieving-all-of-the-product-information-at-once"></a>Recuperación de toda la información del producto a la vez

Las dos técnicas anterior se ve examinan tomar esos productos para la categoría actual mediante una llamada a la `ProductsBLL` clase s `GetProductsByCategoryID(categoryID)` método (el primer enfoque lo hicieron a través de un origen ObjectDataSource, el segundo a través de la `GetProductsInCategory(categoryID)` método en el clase de código subyacente). Cada vez que se invoca este método, las llamadas de la capa de lógica de negocios hasta la capa de acceso a datos, que consulta la base de datos con una instrucción SQL que devuelva filas desde el `Products` tabla cuyos `CategoryID` campo coincide con el parámetro de entrada proporcionado.

Dado *N* categorías en el sistema, este enfoque reduce *N* + 1 llamadas a la consulta de una base de datos de la base de datos para obtener todas las categorías y, a continuación, *N* llamadas para obtener los productos específico de cada categoría. Sin embargo, podemos, recuperamos todos los datos necesarios en solo dos bases de datos llama a una llamada para obtener todas las categorías y otra para obtener todos los productos. Una vez que tenemos todos los productos, podemos filtrar por lo que aquellos productos que solo los productos de coincidencia actual `CategoryID` están enlazados a esa categoría s Repeater interna.

Para proporcionar esta funcionalidad, solo se necesita realizar una pequeña modificación en el `GetProductsInCategory(categoryID)` método en la clase de código subyacente de s de la página ASP.NET. En lugar de devolver ciegamente los resultados de la `ProductsBLL` clase s `GetProductsByCategoryID(categoryID)` método, en su lugar puede acceder primero al *todos los* de los productos (Si conocemos t sido acceso ya) y, a continuación, devolver simplemente la vista filtrada de la productos basados en el pasado en `CategoryID`.


[!code-csharp[Main](nested-data-web-controls-cs/samples/sample8.cs)]

Tenga en cuenta la adición de la variable de nivel de página, `allProducts`. Contiene información acerca de todos los productos y se rellena la primera vez el `GetProductsInCategory(categoryID)` se invoca el método. Después de asegurarse de que el `allProducts` objeto se crea y rellena, el método filtra los resultados de s de DataTable que solo las filas cuya `CategoryID` coincide con el especificado `CategoryID` son accesibles. Este enfoque reduce el número de veces que se tiene acceso a la base de datos de *N* + 1 hasta dos.

Esta mejora no presenta ningún cambio al marcado representado de la página, ni aporta volver menos registros que el otro enfoque. Simplemente se reduce el número de llamadas a la base de datos.

> [!NOTE]
> Uno podría motivo intuitiva que reduce el número de accesos de base de datos sin dudas mejora el rendimiento. Sin embargo, esto podría no ser el caso. Si tiene un gran número de productos cuyo `CategoryID` es `NULL`, por ejemplo, la llamada a la `GetProducts` método devuelve un número de productos que nunca se muestran. Además, devuelve todos los productos puede ser excesivo si se van a mostrar solo un subconjunto de las categorías, que puede suceder si ha implementado la paginación.


Como siempre, lo que respecta a analizar el rendimiento de estas dos técnicas, la medida reír solo es ejecutar las pruebas controladas adaptadas para los escenarios de casos de aplicación s comunes.

## <a name="summary"></a>Resumen

En este tutorial hemos visto cómo se anidan un datos control Web dentro de otra, específicamente examinando la manera en que un Repeater externo muestre un elemento para cada categoría con una lista de los productos de cada categoría en una lista con viñetas de Repeater interna. El desafío principal en la creación de una interfaz de usuario anidado radica en acceder y enlace los datos correctos para el control Web de datos interna. Hay una variedad de técnicas disponibles, que dos de los cuales se examinan en este tutorial. El primer enfoque examinado usa un origen ObjectDataSource en el control Web s de datos exterior `ItemTemplate` que estaba enlazado al control Web interna de datos a través de su `DataSourceID` propiedad. La segunda técnica tener acceso a los datos a través de un método en la clase de código subyacente ASP.NET page s. Este método, a continuación, se puede enlazar a los datos internos de control Web s `DataSource` propiedad mediante la sintaxis de enlace de datos.

Mientras que la interfaz de usuario anidados examinada en este tutorial usa un control Repeater anidado dentro de un control Repeater, estas técnicas se pueden extender a otros controles de datos Web. Puede anidar un control Repeater dentro de un control GridView o un control GridView dentro de un control DataList y así sucesivamente.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Los revisores para este tutorial fueron Zack Jones y Liz Shulok. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](showing-multiple-records-per-row-with-the-datalist-control-cs.md)
> [Siguiente](displaying-data-with-the-datalist-and-repeater-controls-vb.md)
