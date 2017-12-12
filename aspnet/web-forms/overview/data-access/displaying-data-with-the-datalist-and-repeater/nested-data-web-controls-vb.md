---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-vb
title: Los datos anidados Web controles (VB) | Documentos de Microsoft
author: rick-anderson
description: "En este tutorial que exploraremos cómo usar un repetidor anidado dentro de otro repetidor. Los ejemplos ilustran cómo rellenar el repetidor interno ambos d..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/13/2006
ms.topic: article
ms.assetid: 8b7fcf7b-722b-498d-a4e4-7c93701e0c95
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 944f208d6fe4f9fde13b530fb236ecc69ff5e9cd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="nested-data-web-controls-vb"></a>Controles Web de datos anidadas (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_32_VB.exe) o [descarga de PDF](nested-data-web-controls-vb/_static/datatutorial32vb1.pdf)

> En este tutorial que exploraremos cómo usar un repetidor anidado dentro de otro repetidor. Los ejemplos ilustran cómo rellenar el repetidor interno mediante declaración y mediante programación.


## <a name="introduction"></a>Introducción

Además de HTML estático y la sintaxis de enlace de datos, también pueden incluir plantillas de controles Web y controles de usuario. Estos controles Web pueden tener sus propiedades asignados a través de la sintaxis declarativa, de enlace de datos, o puede tener acceso mediante programación en los controladores de eventos de servidor adecuado.

Mediante la incorporación de controles dentro de una plantilla, puede personalizarse y mejorada con respecto a la apariencia y experiencia del usuario. Por ejemplo, en la [utilizando TemplateFields en el GridView Control](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) tutorial, hemos visto cómo personalizar la presentación de s GridView agregando un control de calendario en TemplateField para mostrar un empleado fecha de contratación de s; en el [agregar Controles de validación a las Interfaces de insertar y editar](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) y [personalizar la interfaz de modificación de datos](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) tutoriales, hemos visto cómo personalizar la edición e interfaces de inserción mediante la adición de validación controles, cuadros de texto, listas desplegables y otros controles Web.

Plantillas también pueden contener otros controles Web de datos. Es decir, podemos tenemos un DataList que contiene otro DataList (o repetidor o GridView o DetailsView y así sucesivamente) dentro de sus plantillas. El desafío con dicha interfaz enlaza los datos apropiados para el control Web de datos internos. Hay diferentes enfoques disponibles, que van desde opciones declarativas a través de ObjectDataSource los mediante programación.

En este tutorial que exploraremos cómo usar un repetidor anidado dentro de otro repetidor. Repetidor externo contendrá un elemento para cada categoría de la base de datos, mostrar el nombre de categoría s y la descripción. Cada elemento de categoría s repetidor interna mostrará la información de cada producto que pertenecen a esa categoría (consulte la figura 1) en una lista con viñetas. Nuestros ejemplos ilustran cómo rellenar el repetidor interno mediante declaración y mediante programación.


[![Cada categoría, junto con sus productos, se muestran](nested-data-web-controls-vb/_static/image2.png)](nested-data-web-controls-vb/_static/image1.png)

**Figura 1**: cada categoría, junto con sus productos, se enumeran ([haga clic aquí para ver la imagen a tamaño completo](nested-data-web-controls-vb/_static/image3.png))


## <a name="step-1-creating-the-category-listing"></a>Paso 1: Crear la lista de categoría

Al compilar una página que usa controles de Web de datos anidados, resultar conveniente diseño, crear y probar el control de Web de datos más externa en primer lugar, sin preocuparse incluso el control anidado interno. Por lo tanto, permiten s iniciar recorriendo los pasos necesarios para agregar un repetidor a la página que muestra el nombre y la descripción de cada categoría.

Comience abriendo la `NestedControls.aspx` página en el `DataListRepeaterBasics` carpeta y agregar un control de repetidor a la página de configuración de su `ID` propiedad `CategoryList`. Desde la etiqueta inteligente de repetidor s, optar por crear un nuevo origen ObjectDataSource denominado `CategoriesDataSource`.


[![Nombre de la nueva ObjectDataSource CategoriesDataSource](nested-data-web-controls-vb/_static/image5.png)](nested-data-web-controls-vb/_static/image4.png)

**Figura 2**: nombre de la nueva ObjectDataSource `CategoriesDataSource` ([haga clic aquí para ver la imagen a tamaño completo](nested-data-web-controls-vb/_static/image6.png))


Configurar el ObjectDataSource para que extraen los datos de la `CategoriesBLL` clase s. `GetCategories` método.


[![Configurar el ObjectDataSource para utilizar el método de GetCategories CategoriesBLL clase s.](nested-data-web-controls-vb/_static/image8.png)](nested-data-web-controls-vb/_static/image7.png)

**Figura 3**: configurar el ObjectDataSource que se utilizan los `CategoriesBLL` clase s `GetCategories` método ([haga clic aquí para ver la imagen a tamaño completo](nested-data-web-controls-vb/_static/image9.png))


Para especificar la plantilla de repetidor s contenido es necesario volver a la vista de origen y escribir manualmente la sintaxis declarativa. Agregar un `ItemTemplate` que muestra el nombre de categoría s en un `<h4>` elemento y la descripción de la categoría s en un elemento de párrafo (`<p>`). Además, permiten s separar cada categoría con una regla horizontal (`<hr>`). Después de realizar estos cambios la página debe contener la sintaxis declarativa para el repetidor y ObjectDataSource que es similar al siguiente:


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample1.aspx)]

La figura 4 muestra nuestro progreso cuando se ven a través de un explorador.


[![Se muestran cada categoría s nombre y descripción, separada por una regla Horizontal](nested-data-web-controls-vb/_static/image11.png)](nested-data-web-controls-vb/_static/image10.png)

**Figura 4**: se muestra cada categoría s nombre y una descripción, separada por una regla Horizontal ([haga clic aquí para ver la imagen a tamaño completo](nested-data-web-controls-vb/_static/image12.png))


## <a name="step-2-adding-the-nested-product-repeater"></a>Paso 2: Agregar repetidor producto anidados

Con la lista completa de categoría, la siguiente tarea consiste en Agregar un repetidor para la `CategoryList` s `ItemTemplate` que muestra información acerca de los productos que pertenecen a la categoría correspondiente. Hay varias maneras de que podemos recuperar los datos para este repetidor interna, dos de los cuales se explorará en breve. Por ahora, permiten s basta con crear los productos repetidor dentro de la `CategoryList` repetidor s `ItemTemplate`. En concreto, permite que s tengan la presentación repetidor cada producto en una lista con viñetas con cada uno de elemento de lista incluido el nombre de producto s y el precio del producto.

Para crear este repetidor es necesario escribir manualmente la sintaxis declarativa de repetidor s interna y plantillas en el `CategoryList` s `ItemTemplate`. Agregue el siguiente marcado dentro de la `CategoryList` repetidor s `ItemTemplate`:


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample2.aspx)]

## <a name="step-3-binding-the-category-specific-products-to-the-productsbycategorylist-repeater"></a>Paso 3: Enlazar los productos específico de la categoría a repetidor ProductsByCategoryList

Si visita la página a través de un explorador en este momento, la pantalla será el mismo que en la figura 4 porque se ve todavía para enlazar los datos a repetidor. Hay varias maneras de que podemos obtener los registros de producto correspondiente y enlazarlas a repetidor, algo más eficaz que otros. El desafío más importante aquí es recibir los productos adecuados para la categoría especificada.

Los datos que se va a enlazar al control de repetidor interno o bien pueden obtenerse mediante declaración, a través de un origen ObjectDataSource en el `CategoryList` repetidor s `ItemTemplate`, o mediante programación, desde la página de código subyacente de página s ASP.NET. De forma similar, estos datos pueden estar limitados por repetidor interno ya sea mediante declaración - a través del repetidor interno s `DataSourceID` propiedad o mediante la sintaxis declarativa de enlace de datos, o mediante programación, que hacen referencia a repetidor interno en la `CategoryList` repetidor s `ItemDataBound` controlador de eventos, establecer mediante programación sus `DataSource` propiedad y llamar a su `DataBind()` método. Permiten s explorar cada uno de estos enfoques.

## <a name="accessing-the-data-declaratively-with-an-objectdatasource-control-and-theitemdataboundevent-handler"></a>Acceso a los datos mediante declaración con un Control ObjectDataSource y`ItemDataBound`controlador de eventos

Desde que se ha utilizado el ObjectDataSource ampliamente a lo largo de esta serie de tutoriales, la elección más natural para tener acceso a datos de este ejemplo es opte por ObjectDataSource. El `ProductsBLL` clase tiene un `GetProductsByCategoryID(categoryID)` método que devuelve información acerca de los productos que pertenecen a los especificados  *`categoryID`* . Por lo tanto, podemos agregar un ObjectDataSource a la `CategoryList` repetidor s `ItemTemplate` y configúrelo para tener acceso a sus datos desde este método de clase s.

Desafortunadamente, el repetidor permitir sus plantillas para editarse a través de la vista de diseño, por lo que necesitamos agregar la sintaxis declarativa para este control ObjectDataSource a mano. La siguiente sintaxis se muestra la `CategoryList` repetidor s `ItemTemplate` después de agregar esta nueva ObjectDataSource (`ProductsByCategoryDataSource`):


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample3.aspx)]

Cuando se usa el enfoque de ObjectDataSource es necesario establecer la `ProductsByCategoryList` repetidor s `DataSourceID` propiedad a la `ID` de ObjectDataSource (`ProductsByCategoryDataSource`). Además, tenga en cuenta que nuestro ObjectDataSource tiene un `<asp:Parameter>` elemento que especifica la  *`categoryID`*  valor que se pasará en el `GetProductsByCategoryID(categoryID)` método. Pero, ¿cómo se especifica este valor? Idealmente, d podremos basta con establecer la `DefaultValue` propiedad de la `<asp:Parameter>` elemento mediante sintaxis de enlace de datos, como:


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample4.aspx)]

Desafortunadamente, la sintaxis de enlace de datos solo es válida en los controles que tienen un `DataBinding` eventos. La `Parameter` clase no tiene este tipo de evento y, por tanto, la sintaxis anterior es válida y se producirá un error en tiempo de ejecución.

Para establecer este valor, es necesario crear un controlador de eventos para el `CategoryList` repetidor s `ItemDataBound` eventos. Recuerde que el `ItemDataBound` evento se desencadena una vez para cada elemento enlazado a repetidor. Por lo tanto, cada vez que este evento se desencadena para repetidor externo, podemos asignamos actual `CategoryID` valor para el `ProductsByCategoryDataSource` ObjectDataSource s `CategoryID` parámetro.

Crear un controlador de eventos para el `CategoryList` repetidor s `ItemDataBound` evento con el código siguiente:


[!code-vb[Main](nested-data-web-controls-vb/samples/sample5.vb)]

Este controlador de eventos inicia asegurándose de que se vuelve a tratar con datos de elemento en lugar del elemento de encabezado, pie de página o separador. A continuación, se hacen referencia a los datos reales `CategoriesRow` instancia que solo se ha enlazado a la actual `RepeaterItem`. Por último, se hace referencia el ObjectDataSource en el `ItemTemplate` y asignar su `CategoryID` valor de parámetro para la `CategoryID` del elemento actual `RepeaterItem`.

Con este controlador de eventos, el `ProductsByCategoryList` repetidor en cada `RepeaterItem` está enlazado a los productos en la `RepeaterItem` categoría s. Figura 5 se muestra una captura de pantalla de la salida resultante.


[![Repetidor externo enumera cada categoría; Lo interna enumera los productos de esa categoría](nested-data-web-controls-vb/_static/image14.png)](nested-data-web-controls-vb/_static/image13.png)

**Figura 5**: la externa repetidor enumera cada categoría; las listas de una interna los productos de esa categoría ([haga clic aquí para ver la imagen a tamaño completo](nested-data-web-controls-vb/_static/image15.png))


## <a name="accessing-the-products-by-category-data-programmatically"></a>Acceder a los productos por categoría datos mediante programación

En lugar de usar un ObjectDataSource para recuperar los productos de la categoría actual, no se pudo crear un método en nuestra clase de código subyacente ASP.NET página s (o en la `App_Code` carpeta o en un proyecto de biblioteca de clases independiente) que devuelve el conjunto adecuado de productos cuando se pasa en un `CategoryID`. Imagine que tuvimos que este tipo de método en la clase de código subyacente de la página s ASP.NET y que se denomina `GetProductsInCategory(categoryID)`. Con este método en su lugar, podríamos enlazamos los productos de la categoría actual repetidor interno utilizando la sintaxis declarativa siguiente:


[!code-aspx[Main](nested-data-web-controls-vb/samples/sample6.aspx)]

Repetidor s `DataSource` propiedad utiliza la sintaxis de enlace de datos para indicar que sus datos proceden de la `GetProductsInCategory(categoryID)` método. Puesto que `Eval("CategoryID")` devuelve un valor de tipo `Object`, se convierte el objeto a una `Integer` antes de pasarlo a la `GetProductsInCategory(categoryID)` método. Tenga en cuenta que la `CategoryID` a los que acceda a continuación mediante el enlace de datos de la sintaxis es la `CategoryID` en el *externa* repetidor (`CategoryList`), lo que s se enlazan a los registros de la `Categories` tabla. Por lo tanto, sabemos que `CategoryID` no puede ser una base de datos `NULL` valor, que es la razón por la que se puede convertir a ciegas los `Eval` método sin comprobar si se vuelve a tratar con un `DBNull`.

Con este enfoque, se necesita crear el `GetProductsInCategory(categoryID)` método y recuperar el conjunto adecuado de productos dado proporcionado  *`categoryID`* . Podemos hacer esto al devolver simplemente la `ProductsDataTable` devuelto por la `ProductsBLL` clase s. `GetProductsByCategoryID(categoryID)` método. Permiten s crear el `GetProductsInCategory(categoryID)` método en la clase de código subyacente para nuestro `NestedControls.aspx` página. Lo hacen mediante el código siguiente:


[!code-vb[Main](nested-data-web-controls-vb/samples/sample7.vb)]

Este método simplemente crea una instancia de la `ProductsBLL` método y devuelve los resultados de la `GetProductsByCategoryID(categoryID)` método. Tenga en cuenta que el método debe marcarse `Public` o `Protected`; si el método está marcado como `Private`, no será accesible desde el marcado declarativo de páginas ASP.NET.

Después de realizar estos cambios para usar esta técnica nueva, tómese un momento para ver la página a través de un explorador. El resultado debería ser idéntico a la salida cuando se usa el ObjectDataSource y `ItemDataBound` enfoque de controlador de eventos (hacen referencia a la figura 5 para ver la captura de pantalla).

> [!NOTE]
> Puede parecer busywork para crear el `GetProductsInCategory(categoryID)` método en la clase de código subyacente ASP.NET page s. Después de todo, este método simplemente crea una instancia de la `ProductsBLL` clase y devuelve los resultados de sus `GetProductsByCategoryID(categoryID)` método. ¿Por qué no llame a este método directamente desde la sintaxis de enlace de datos del repetidor interna, como: `DataSource='<%# ProductsBLL.GetProductsByCategoryID(CType(Eval("CategoryID"), Integer)) %>'`? Aunque esta sintaxis won trabajo t con la implementación actual de la `ProductsBLL` clase (puesto que la `GetProductsByCategoryID(categoryID)` método es un método de instancia), puede modificar `ProductsBLL` para incluir una variable static `GetProductsByCategoryID(categoryID)` método o que la clase incluya estático `Instance()` método para devolver una nueva instancia de la `ProductsBLL` clase.


Mientras estas modificaciones eliminaría la necesidad de que el `GetProductsInCategory(categoryID)` método en la clase de código subyacente ASP.NET page s, el método de clase de código subyacente nos proporciona mayor flexibilidad para trabajar con los datos recuperados, como veremos en breve.

## <a name="retrieving-all-of-the-product-information-at-once"></a>Recuperar toda la información de producto a la vez

Las dos técnicas anterior se ha examinado agarre los productos de la categoría actual mediante la realización de una llamada a la `ProductsBLL` clase s `GetProductsByCategoryID(categoryID)` método (el primer enfoque lo hacían mediante un ObjectDataSource, el segundo a través de la `GetProductsInCategory(categoryID)` método en el clase de código subyacente). Cada vez que se invoca este método, las llamadas de la capa de lógica empresarial a la capa de acceso a datos, que consulta la base de datos con una instrucción SQL que devuelve filas de la `Products` tabla cuyos `CategoryID` campo coincide con el parámetro de entrada proporcionado.

Dado *N* categorías en el sistema, este enfoque reduce *N* + 1 llamadas a la consulta de una base de datos de la base de datos para obtener todas las categorías y, a continuación, *N* llamadas para obtener los productos específico de cada categoría. Sin embargo, podemos, recuperamos todos los datos necesarios en una llamada de llamadas de base de datos solo dos para obtener todas las categorías y otra para obtener todos los productos. Una vez que tenemos todos los productos, podemos filtrar por lo que los productos que solo los productos coincidencia actual `CategoryID` están enlazados a esa categoría s repetidor interna.

Para proporcionar esta funcionalidad, únicamente se necesita realizar una pequeña modificación en el `GetProductsInCategory(categoryID)` método en la clase de código subyacente de la página s ASP.NET. En lugar de devolver a ciegas los resultados de la `ProductsBLL` clase s `GetProductsByCategoryID(categoryID)` método, pero podemos en su lugar acceder primero *todos los* de los productos (Si conocemos t sido acceso ya) y, a continuación, devolver simplemente la vista filtrada de la productos que se basan en el pasado en `CategoryID`.


[!code-vb[Main](nested-data-web-controls-vb/samples/sample8.vb)]

Tenga en cuenta la adición de la variable de nivel de página, `allProducts`. Contiene información sobre todos los productos y se rellena la primera vez el `GetProductsInCategory(categoryID)` se invoca el método. Después de asegurarse de que el `allProducts` objeto se ha creado y rellena, el método filtra los resultados de s de DataTable de modo que solo las filas cuyo `CategoryID` coincide con la que se especifica `CategoryID` son accesibles. Este enfoque reduce el número de veces que se tiene acceso a la base de datos de *N* + 1 hasta dos.

Esta mejora no presenta ningún cambio en el marcado representado de la página ni aporta vuelve menos registros que el otro enfoque. Simplemente se reduce el número de llamadas a la base de datos.

> [!NOTE]
> Uno podría motivo intuitivamente que reducir el número de accesos a la base de datos assuredly mejora el rendimiento. Sin embargo, esto podría no ser el caso. Si tiene un gran número de productos cuyo `CategoryID` es `NULL`, por ejemplo, la llamada a la `GetProducts` método devuelve un número de productos que nunca se muestran. Además, devolver todos los productos puede ser innecesarios si se van a mostrar solo un subconjunto de las categorías, podría darse el caso si ha implementado la paginación.


Como siempre, cuando trata de analizar el rendimiento de estas dos técnicas, la medida solo infalibles es ejecutar pruebas controladas adaptadas para los escenarios de casos comunes de s de aplicación.

## <a name="summary"></a>Resumen

En este tutorial, hemos visto cómo anidar los datos de un control Web dentro de otra, específicamente examinar cómo hacer que un repetidor externa muestran un elemento para cada categoría con una lista de los productos para cada categoría en una lista con viñetas de repetidor interna. El desafío más importante en la creación de una interfaz de usuario anidado se encuentra en acceder y enlazar los datos correctos al control de Web de datos interna. Hay una variedad de técnicas disponibles, que dos de los cuales se examinan en este tutorial. El primer enfoque examinado usa un ObjectDataSource en el control Web s de datos exterior `ItemTemplate` que estaba enlazado al control Web de datos interna a través de su `DataSourceID` propiedad. La segunda técnica acceso a los datos a través de un método en la clase de código subyacente ASP.NET page s. Este método, a continuación, puede enlazarse a los datos internos control Web s `DataSource` propiedad mediante la sintaxis de enlace de datos.

Si bien la interfaz de usuario anidado examinada en este tutorial usa un repetidor anidado dentro de un repetidor, estas técnicas se pueden ampliar a los otros controles Web de datos. Puede anidar un repetidor dentro de un control GridView, o un control GridView dentro de un control DataList y así sucesivamente.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [ *SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Los revisores iniciales para este tutorial eran Zack Jones y Liz Shulok. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](showing-multiple-records-per-row-with-the-datalist-control-vb.md)
