---
uid: web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-cs
title: Visualización de datos con ObjectDataSource (C#) | Microsoft Docs
author: rick-anderson
description: Este tutorial trata el control ObjectDataSource mediante este control que se puede enlazar los datos recuperados de BLL creada en el tutorial anterior sin havi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: af882aef-56f5-4e9a-8f95-3977fde20e74
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: 7d4d292104703f9aebf131035920a058317011af
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395391"
---
<a name="displaying-data-with-the-objectdatasource-c"></a>Visualización de datos con ObjectDataSource (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_4_CS.exe) o [descargar PDF](displaying-data-with-the-objectdatasource-cs/_static/datatutorial04cs1.pdf)

> Este tutorial trata el control ObjectDataSource mediante este control que se puede enlazar los datos recuperados de BLL creado en el tutorial anterior sin tener que escribir una línea de código.


## <a name="introduction"></a>Introducción

Con nuestra aplicación arquitectura y el sitio Web de diseño de página completa, ya estamos listos para empezar a explorar cómo realizar una serie de tareas comunes y reporting-relacionadas con los datos. En los tutoriales anteriores, hemos visto cómo enlazar mediante programación los datos de DAL y BLL en un control Web en una página ASP.NET de datos. Esta sintaxis de asignación del control Web de datos `DataSource` propiedad a los datos para mostrar y, a continuación, una llamada del control `DataBind()` método era el patrón utilizado en aplicaciones ASP.NET 1.x y puede seguir utilizándose en sus 2.0 aplicaciones. Sin embargo, los nuevos controles de origen de datos de ASP.NET 2.0 ofrecen una manera declarativa para trabajar con datos. Uso de estos controles se pueden enlazar los datos recuperados de BLL creado en el [tutorial anterior](../introduction/creating-a-business-logic-layer-cs.md) sin tener que escribir una línea de código.

ASP.NET 2.0 incluye cinco controles de origen de datos integrados [SqlDataSource](https://msdn.microsoft.com/library/dz12d98w(vs.80).aspx), [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx), [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a(en-US,VS.80).aspx), y [SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x(en-US,VS.80).aspx) aunque puede crear sus propios [controles de origen de datos personalizados](https://msdn.microsoft.com/library/default.asp?url=/library/dnvs05/html/DataSourceCon1.asp), si es necesario. Puesto que hemos desarrollado una arquitectura para nuestra aplicación del tutorial, vamos a usar ObjectDataSource con nuestras clases BLL.


![ASP.NET 2.0 incluye cinco controles de origen de datos integrados](displaying-data-with-the-objectdatasource-cs/_static/image1.png)

**Figura 1**: ASP.NET 2.0 incluye cinco controles de origen de datos integrados


ObjectDataSource actúa como un proxy para trabajar con algún otro objeto. Para configurar el origen ObjectDataSource especificamos este objeto y cómo se asignan sus métodos de ObjectDataSource subyacentes `Select`, `Insert`, `Update`, y `Delete` métodos. Una vez que se ha especificado este objeto subyacente y sus métodos asignan a de ObjectDataSource, a continuación, podemos enlazar ObjectDataSource a un control Web de datos. ASP.NET incluye datos de muchos controles Web, como el control GridView, DetailsView, RadioButtonList y DropDownList, entre otros. Durante el ciclo de vida de la página, el control Web de datos que deba tener acceso a los datos que está enlazado, que llevará a cabo mediante la invocación de su ObjectDataSource `Select` método; si el control Web de datos permiten insertar, actualizar, o eliminar, se pueden realizar llamadas a su Del origen ObjectDataSource `Insert`, `Update`, o `Delete` métodos. Estas llamadas, a continuación, se enrutan mediante el origen ObjectDataSource a los métodos del objeto subyacente adecuada como se muestra en el diagrama siguiente.


[![ObjectDataSource actúa como un Proxy](displaying-data-with-the-objectdatasource-cs/_static/image3.png)](displaying-data-with-the-objectdatasource-cs/_static/image2.png)

**Figura 2**: el origen ObjectDataSource actúa como Proxy ([haga clic aquí para ver imagen en tamaño completo](displaying-data-with-the-objectdatasource-cs/_static/image4.png))


Aunque el origen ObjectDataSource se puede usar para invocar métodos para insertar, actualizar o eliminar datos, centrémonos en la devolución de datos; próximos tutoriales se exploran el utilizando el origen ObjectDataSource y los controles Web que modifican los datos de datos.

## <a name="step-1-adding-and-configuring-the-objectdatasource-control"></a>Paso 1: Agregar y configurar el Control ObjectDataSource

Comience abriendo la `SimpleDisplay.aspx` página en el `BasicReporting` carpeta, cambie a la vista de diseño y, a continuación, arrastre un control ObjectDataSource desde el cuadro de herramientas hasta la superficie de diseño de la página. ObjectDataSource aparece como un cuadro gris en la superficie de diseño porque no genera ningún marcado; simplemente tiene acceso a datos mediante la invocación de un método de un objeto especificado. Los datos devueltos por un origen ObjectDataSource se pueden mostrar mediante un control Web, como el control GridView, DetailsView, FormView y así sucesivamente de datos.

> [!NOTE]
> También primero de puede que agregar los datos de control Web a la página y, a continuación, en su etiqueta inteligente, elija el &lt;nuevo origen de datos&gt; opción en la lista desplegable.


Para especificar el objeto subyacente de ObjectDataSource y cómo se asignan de ObjectDataSource métodos del objeto, haga clic en el vínculo Configurar origen de datos de etiqueta inteligente de ObjectDataSource.


[![Haga clic en el vínculo a origen de datos de la etiqueta inteligente de configuración](displaying-data-with-the-objectdatasource-cs/_static/image6.png)](displaying-data-with-the-objectdatasource-cs/_static/image5.png)

**Figura 3**: haga clic en el vínculo Configurar de un origen de datos de la etiqueta inteligente ([haga clic aquí para ver imagen en tamaño completo](displaying-data-with-the-objectdatasource-cs/_static/image7.png))


Se abrirá el Asistente para configurar orígenes de datos. En primer lugar, debemos especificamos el objeto que es trabajar con el origen ObjectDataSource. Si se activa la casilla "Mostrar sólo los componentes de datos", la lista desplegable de esta pantalla muestra solo los objetos que se han visto decorados con el `DataObject` atributo. Actualmente, nuestra lista incluye los TableAdapters en el conjunto de datos con tipo y las clases BLL, que hemos creado en el tutorial anterior. Si ha olvidado agregar los `DataObject` atributo a las clases de la capa de lógica empresarial no aparecen en esta lista. En ese caso, desactive la casilla "Mostrar sólo los componentes de datos" para ver todos los objetos, que deben incluir las clases BLL (junto con las otras clases en el conjunto de datos con tipo DataTables, filas de datos y así sucesivamente).

En la primera pantalla, elija el `ProductsBLL` clase en la lista desplegable y haga clic en siguiente.


[![Especifique el objeto que se usarán con el Control ObjectDataSource](displaying-data-with-the-objectdatasource-cs/_static/image9.png)](displaying-data-with-the-objectdatasource-cs/_static/image8.png)

**Figura 4**: especifique el objeto que se usarán con el ObjectDataSource Control ([haga clic aquí para ver imagen en tamaño completo](displaying-data-with-the-objectdatasource-cs/_static/image10.png))


La siguiente pantalla del asistente le pedirá que seleccione qué método debería invocar el origen ObjectDataSource. La lista desplegable enumera los métodos que devuelven datos en el objeto seleccionado en la pantalla anterior. Aquí vemos `GetProductByProductID`, `GetProducts`, `GetProductsByCategoryID`, y `GetProductsBySupplierID`. Seleccione el `GetProducts` método desde la lista desplegable y haga clic en Finalizar (si ha agregado el `DataObjectMethodAttribute` a la `ProductBLL`de métodos, como se muestra en el tutorial anterior, esta opción se seleccionará de forma predeterminada).


[![Elija el método de devolución de datos de la ficha Seleccionar](displaying-data-with-the-objectdatasource-cs/_static/image12.png)](displaying-data-with-the-objectdatasource-cs/_static/image11.png)

**Figura 5**: elija el método para devolver datos desde la pestaña seleccione ([haga clic aquí para ver imagen en tamaño completo](displaying-data-with-the-objectdatasource-cs/_static/image13.png))


## <a name="configure-the-objectdatasource-manually"></a>Configurar manualmente el origen ObjectDataSource

Asistente para configurar origen de datos de ObjectDataSource ofrece una forma rápida para especificar el objeto que usa y asociar se invocan los métodos del objeto. Sin embargo, puede configurar el origen ObjectDataSource a través de sus propiedades, mediante la ventana Propiedades o directamente en el marcado declarativo. Basta con establecer la `TypeName` propiedad al tipo del objeto subyacente que se usará y el `SelectMethod` para el método que se invoca cuando se recuperan datos.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample1.aspx)]

Incluso si prefiere que el Asistente para configurar origen de datos puede haber ocasiones en que necesite configurar manualmente el origen ObjectDataSource, como el asistente muestra sólo clases creadas por el desarrollador. Si desea enlazar al origen ObjectDataSource a una clase en .NET Framework, como el [clase de pertenencia](https://msdn.microsoft.com/library/system.web.security.membership.aspx)para tener acceso a información de la cuenta de usuario, o la [clase Directory](https://msdn.microsoft.com/library/system.io.directory.aspx) para trabajar con información del sistema de archivos deberá establecer manualmente las propiedades de ObjectDataSource.

## <a name="step-2-adding-a-data-web-control-and-binding-it-to-the-objectdatasource"></a>Paso 2: Agregar un Control Web de datos y se enlaza a ObjectDataSource

Una vez que se ha agregado a la página y configurado el origen ObjectDataSource, estamos listos para agregar controles Web de datos a la página para mostrar los datos devueltos por el ObjectDataSource `Select` método. Cualquier control Web de datos se pueden enlazar a un origen ObjectDataSource; Echemos un vistazo a mostrar datos de ObjectDataSource en GridView, DetailsView y FormView.

## <a name="binding-a-gridview-to-the-objectdatasource"></a>Enlazar un control GridView a ObjectDataSource

Agregar un control GridView en el cuadro de herramientas a `SimpleDisplay.aspx`de superficie de diseño. En las etiquetas inteligentes de GridView, elija el control ObjectDataSource, que hemos agregado en el paso 1. Esto creará automáticamente un BoundField en GridView para cada propiedad devuelta por los datos desde el ObjectDataSource `Select` método (es decir, las propiedades definidas por la DataTable de productos).


[![Se ha agregado un control GridView a la página y enlazar a ObjectDataSource](displaying-data-with-the-objectdatasource-cs/_static/image15.png)](displaying-data-with-the-objectdatasource-cs/_static/image14.png)

**Figura 6**: un control GridView se ha agregado a la página y se enlaza a ObjectDataSource ([haga clic aquí para ver imagen en tamaño completo](displaying-data-with-the-objectdatasource-cs/_static/image16.png))


A continuación, puede personalizar, reorganizar o quitar BoundFields de GridView haciendo clic en la opción de editar las columnas de la etiqueta inteligente.


[![Administración BoundFields de GridView mediante el cuadro de diálogo Editar columnas](displaying-data-with-the-objectdatasource-cs/_static/image18.png)](displaying-data-with-the-objectdatasource-cs/_static/image17.png)

**Figura 7**: administración BoundFields a través de las columnas de cuadro de GridView de diálogo Editar ([haga clic aquí para ver imagen en tamaño completo](displaying-data-with-the-objectdatasource-cs/_static/image19.png))


Dedique unos minutos a modificar BoundFields de GridView, quitando el `ProductID`, `SupplierID`, `CategoryID`, `QuantityPerUnit`, `UnitsInStock`, `UnitsOnOrder`, y `ReorderLevel` BoundFields. Simplemente seleccione la BoundField en la lista en la parte inferior izquierda y haga clic en el botón Eliminar (la X roja) para quitarlos. A continuación, reorganice el BoundFields para que el `CategoryName` y `SupplierName` BoundFields preceder el `UnitPrice` BoundField seleccionando estos BoundFields y haga clic en la flecha arriba. Establecer el `HeaderText` las propiedades de los restantes BoundFields a `Products`, `Category`, `Supplier`, y `Price`, respectivamente. A continuación, tiene la `Price` BoundField con formato como una moneda estableciendo el BoundField `HtmlEncode` propiedad en False y su `DataFormatString` propiedad `{0:c}`. Por último, Alinear horizontalmente el `Price` a la derecha y el `Discontinued` casilla de verificación en el centro a través de la `ItemStyle` / `HorizontalAlign` propiedad.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample2.aspx)]


[![Se han personalizado BoundFields prvku GridView.](displaying-data-with-the-objectdatasource-cs/_static/image21.png)](displaying-data-with-the-objectdatasource-cs/_static/image20.png)

**Figura 8**: se han personalizado el control GridView BoundFields ([haga clic aquí para ver imagen en tamaño completo](displaying-data-with-the-objectdatasource-cs/_static/image22.png))


## <a name="using-themes-for-a-consistent-look"></a>Uso de los temas para una apariencia coherente

Estos tutoriales se esfuerzan por quitar cualquier configuración de estilo de nivel de control, en lugar de utilizar las hojas de estilos en cascada definido en un archivo externo siempre que sea posible. El `Styles.css` contiene el archivo `DataWebControlStyle`, `HeaderStyle`, `RowStyle`, y `AlternatingRowStyle` clases CSS que se deben utilizar para dictar la apariencia de los datos Web controles que se usan en estos tutoriales. Para ello, podríamos establecemos la GridView `CssClass` propiedad `DataWebControlStyle`y su `HeaderStyle`, `RowStyle`, y `AlternatingRowStyle` propiedades `CssClass` propiedades según corresponda.

Si se establecen estas `CssClass` control agregado a nuestros tutoriales de Web de propiedades en el control Web sería necesario que recuerde establecer explícitamente estos valores de propiedad para cada uno de los datos. Es un enfoque más manejable definir las propiedades de CSS de forma predeterminada para el control GridView, DetailsView y FormView controla el uso de un tema. Un tema es una colección de valores de las propiedades de nivel de control, imágenes y las clases CSS que se pueden aplicar a las páginas a través de un sitio para aplicar una apariencia común.

Nuestro tema no incluye las imágenes o archivos CSS (no se cerrará la hoja de estilos `Styles.css` como-, definido en la carpeta raíz de la aplicación web), pero incluirá dos máscaras. Una máscara es un archivo que define las propiedades predeterminadas de un control Web. En concreto, tendremos un archivo de máscaras para los controles GridView y DetailsView, que indica el valor predeterminado `CssClass`-propiedades relacionadas.

Empiece agregando un nuevo archivo de máscara al proyecto denominado `GridView.skin` con el botón secundario en el nombre del proyecto en el Explorador de soluciones y eligiendo Agregar nuevo elemento.


[![Agregue un archivo de máscara denominado GridView.skin](displaying-data-with-the-objectdatasource-cs/_static/image24.png)](displaying-data-with-the-objectdatasource-cs/_static/image23.png)

**Figura 9**: agregar un archivo de máscara denominado `GridView.skin` ([haga clic aquí para ver imagen en tamaño completo](displaying-data-with-the-objectdatasource-cs/_static/image25.png))


Archivos de máscara deben ubicarse en un tema, que se encuentran en el `App_Themes` carpeta. Dado que aún no contamos con dicha carpeta, Visual Studio ofrecerá Póngase a crearla para que podamos al agregar la máscara de nuestra primera. Haga clic en Sí para crear el `App_Theme` carpeta y coloque el nuevo `GridView.skin` archivo no existe.


[![Deje que Visual Studio cree la carpeta App_Theme](displaying-data-with-the-objectdatasource-cs/_static/image27.png)](displaying-data-with-the-objectdatasource-cs/_static/image26.png)

**Figura 10**: permitir que Visual Studio crea el `App_Theme` carpeta ([haga clic aquí para ver imagen en tamaño completo](displaying-data-with-the-objectdatasource-cs/_static/image28.png))


Esto creará un nuevo tema en el `App_Themes` carpeta denominada GridView con el archivo de máscara `GridView.skin`.


![El tema de GridView tiene se han agregado a la carpeta App_Theme](displaying-data-with-the-objectdatasource-cs/_static/image29.png)

**Figura 11**: el tema de GridView se ha agregado a la `App_Theme` carpeta


Cambie el nombre del tema de GridView DataWebControls (haga doble clic en la carpeta de control GridView en la `App_Theme` carpeta y seleccione Cambiar nombre). A continuación, escriba el siguiente marcado en el `GridView.skin` archivo:


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample3.aspx)]

Esto define las propiedades predeterminadas para el `CssClass`-relacionadas con las propiedades para cualquier control GridView en cualquier página que utiliza el tema DataWebControls. Vamos a agregar otra máscara para DetailsView, datos de un control Web que vamos a usar en breve. Agregar un nuevo aspecto para el tema DataWebControls denominado `DetailsView.skin` y agregue el siguiente marcado:


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample4.aspx)]

Con nuestro tema definido, el último paso es aplicar el tema a nuestra página ASP.NET. Puede aplicar un tema en cada página por página o para todas las páginas de un sitio Web. Vamos a usar este tema para todas las páginas en el sitio Web. Para ello, agregue el siguiente marcado para `Web.config`del `<system.web>` sección:


[!code-xml[Main](displaying-data-with-the-objectdatasource-cs/samples/sample5.xml)]

Eso es todo esto! El `styleSheetTheme` configuración indica que las propiedades especificadas en el tema deben *no* invalidar las propiedades especificadas en el nivel de control. Para especificar que los valores de tema deben predominan sobre configuración del control, utilice el `theme` atributo en lugar de `styleSheetTheme`; Desafortunadamente, los valores de tema especificados a través de la `theme` atributo no aparecen en la vista de diseño de Visual Studio. Consulte [ASP.NET Themes and Skins Overview](https://msdn.microsoft.com/library/ykzx33wh.aspx) y [servidor estilos utilizando temas](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx) para obtener más información sobre los temas y máscaras; vea [Cómo: aplicar temas de ASP.NET](https://msdn.microsoft.com/library/0yy5hxdk(VS.80).aspx) para obtener más información sobre configuración de una página para que utilice un tema.


[![El control GridView muestra el nombre, categoría, proveedor, precio y discontinuo información del producto](displaying-data-with-the-objectdatasource-cs/_static/image31.png)](displaying-data-with-the-objectdatasource-cs/_static/image30.png)

**Figura 12**: GridView muestra el nombre, categoría, proveedor, precio y no incluye información del producto ([haga clic aquí para ver imagen en tamaño completo](displaying-data-with-the-objectdatasource-cs/_static/image32.png))


## <a name="displaying-one-record-at-a-time-in-the-detailsview"></a>Mostrar un registro a la vez en DetailsView

El control GridView muestra una fila para cada registro devuelto por el control de origen de datos al que está enlazado. Veces, sin embargo, cuando nos conviene mostrar un único registro o sólo un registro a la vez. El [control DetailsView](https://msdn.microsoft.com/library/s3w1w7t4.aspx) ofrece esta funcionalidad, como un elemento HTML de representación `<table>` con dos columnas y una fila por cada columna o propiedad enlazado al control. DetailsView se puede considerar como un control GridView con un único registro gira 90 grados.

Empiece agregando un control DetailsView *anteriormente* GridView en `SimpleDisplay.aspx`. A continuación, enlazarlo al mismo control ObjectDataSource como GridView. Al igual que con el control GridView, BoundField se agregarán a DetailsView para cada propiedad en el objeto devuelto por el ObjectDataSource `Select` método. La única diferencia es que se distribuyen BoundFields de DetailsView horizontalmente en lugar de verticalmente.


[![Agregar un DetailsView a la página y enlazarlo a ObjectDataSource](displaying-data-with-the-objectdatasource-cs/_static/image34.png)](displaying-data-with-the-objectdatasource-cs/_static/image33.png)

**Figura 13**: agregar un DetailsView a la página y enlazarlo a ObjectDataSource ([haga clic aquí para ver imagen en tamaño completo](displaying-data-with-the-objectdatasource-cs/_static/image35.png))


Como GridView, BoundFields de DetailsView se puede modificar para ofrecer una visualización más personalizada de los datos devueltos por el origen ObjectDataSource. Figura 14 se muestra después de su BoundFields DetailsView y `CssClass` propiedades se han configurado para que su aspecto similar al ejemplo de GridView.


[![DetailsView muestra un único registro](displaying-data-with-the-objectdatasource-cs/_static/image37.png)](displaying-data-with-the-objectdatasource-cs/_static/image36.png)

**Figura 14**: DetailsView muestra un único registro ([haga clic aquí para ver imagen en tamaño completo](displaying-data-with-the-objectdatasource-cs/_static/image38.png))


Tenga en cuenta que DetailsView muestra solo el primer registro devuelto por su origen de datos. Para permitir al usuario paso a paso a través de todos los registros, una vez, debemos habilitar la paginación para DetailsView. Para ello, vuelva a Visual Studio y Active la casilla Habilitar paginación en la etiqueta inteligente de DetailsView.


[![Habilitar la paginación en el Control DetailsView](displaying-data-with-the-objectdatasource-cs/_static/image40.png)](displaying-data-with-the-objectdatasource-cs/_static/image39.png)

**Figura 15**: habilitar la paginación en el DetailsView Control ([haga clic aquí para ver imagen en tamaño completo](displaying-data-with-the-objectdatasource-cs/_static/image41.png))


[![Con la paginación está habilitada, DetailsView permite al usuario ver cualquiera de los productos](displaying-data-with-the-objectdatasource-cs/_static/image43.png)](displaying-data-with-the-objectdatasource-cs/_static/image42.png)

**Figura 16**: con paginación habilitado, DetailsView permite al usuario ver cualquiera de los productos ([haga clic aquí para ver imagen en tamaño completo](displaying-data-with-the-objectdatasource-cs/_static/image44.png))


Hablaremos más acerca de la paginación en el futuro tutoriales.

## <a name="a-more-flexible-layout-for-showing-one-record-at-a-time"></a>Un diseño más Flexible para mostrar un registro a la vez

DetailsView es bastante rígida en cómo se muestra cada registro devuelto desde el origen ObjectDataSource. Es posible que queramos una vista más flexible de los datos. Por ejemplo, en lugar de mostrar el nombre del producto, categoría, proveedor, precio y cada información no incluida en una fila independiente, podemos queremos mostrar el nombre del producto y de precios en un `<h4>` encabezado con la información de categoría y el proveedor que aparecen a continuación el nombre y el precio de un tamaño de fuente. Y nos podremos no importa mostrar los nombres de propiedad (producto, categoría etc.) junto a los valores.

El [control FormView](https://msdn.microsoft.com/library/fyf1dk77.aspx) proporciona este nivel de personalización. En lugar de usar los campos (como GridView y DetailsView hacen), FormView utiliza plantillas, que permiten una combinación de controles Web, HTML estático, y [sintaxis de enlace de datos](http://www.15seconds.com/issue/040630.htm). Si está familiarizado con el control Repeater desde ASP.NET 1.x, se puede considerar FormView como el control Repeater para mostrar un único registro.

Agregar un control FormView para el `SimpleDisplay.aspx` superficie de diseño de la página. FormView muestra inicialmente como un bloque de color gris, que nos informa que necesitamos proporcionar, como mínimo, el control `ItemTemplate`.


[![FormView debe incluir una plantilla ItemTemplate](displaying-data-with-the-objectdatasource-cs/_static/image46.png)](displaying-data-with-the-objectdatasource-cs/_static/image45.png)

**Figura 17**: The FormView debe incluir un `ItemTemplate` ([haga clic aquí para ver imagen en tamaño completo](displaying-data-with-the-objectdatasource-cs/_static/image47.png))


Puede enlazar directamente a un control de origen de datos a través de la etiqueta inteligente de FormView, lo que creará una predeterminada FormView `ItemTemplate` automáticamente (junto con un `EditItemTemplate` y `InsertItemTemplate`, si el control ObjectDatatSource `InsertMethod` y `UpdateMethod` se establecen las propiedades). Sin embargo, en este ejemplo vamos a enlazar los datos a FormView y especificar su `ItemTemplate` manualmente. Comenzar por la configuración de FormView `DataSourceID` propiedad a la `ID` del control ObjectDataSource, `ObjectDataSource1`. A continuación, cree el `ItemTemplate` para que se muestre el nombre del producto y el precio en un `<h4>` elemento y los nombres de categoría y el remitente por debajo de ese con un tamaño de fuente.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample6.aspx)]


[![El primer producto (Chai) se muestra en un formato personalizado](displaying-data-with-the-objectdatasource-cs/_static/image49.png)](displaying-data-with-the-objectdatasource-cs/_static/image48.png)

**Figura 18**: el primer producto (Chai) se muestra en un formato personalizado ([haga clic aquí para ver imagen en tamaño completo](displaying-data-with-the-objectdatasource-cs/_static/image50.png))


El `<%# Eval(propertyName) %>` es la sintaxis de enlace de datos. El `Eval` método devuelve el valor de la propiedad especificada para el objeto actual que se enlaza al control FormView. Consulte el artículo de Alex Homer [simplificado y extendidos sintaxis enlace de datos en ASP.NET 2.0](http://www.15seconds.com/issue/040630.htm) para obtener más información sobre las ventajas y desventajas del enlace de datos.

Como DetailsView, FormView muestra solo el primer registro devuelto desde el origen ObjectDataSource. Puede habilitar la paginación en FormView para permitir que los visitantes de paso a través de los productos de uno a la vez.

## <a name="summary"></a>Resumen

Acceso y la visualización de datos de una capa de lógica empresarial pueden realizarse sin escribir una línea de código gracias al control ObjectDataSource de ASP.NET 2.0. ObjectDataSource invoca un método especificado de una clase y devuelve los resultados. Estos resultados se pueden mostrar en un control Web que está enlazado a ObjectDataSource de datos. En este tutorial analizamos enlazar los controles GridView, DetailsView y FormView a ObjectDataSource.

Hasta ahora solo hemos visto cómo usar ObjectDataSource para invocar un método sin parámetros, pero ¿qué pasa si queremos invocar un método que espera parámetros de entrada, como el `ProductBLL` la clase `GetProductsByCategoryID(categoryID)`? Para poder llamar a un método que espera uno o más parámetros nos debemos configurar ObjectDataSource para especificar los valores para estos parámetros. Veremos cómo hacerlo en nuestro [siguiente tutorial](declarative-parameters-cs.md).

Feliz programación.

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Crear sus propios controles de origen de datos](https://msdn.microsoft.com/library/ms364049.aspx)
- [Ejemplos de GridView de ASP.NET 2.0](https://msdn.microsoft.com/library/aa479339.aspx)
- [Simplificado y ampliado en ASP.NET 2.0 sintaxis de enlace de datos](http://www.15seconds.com/issue/040630.htm)
- [Temas en ASP.NET 2.0](http://www.odetocode.com/Articles/423.aspx)
- [Estilos de lado servidor mediante los temas](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx)
- [Cómo: Aplicar temas de ASP.NET mediante programación](https://msdn.microsoft.com/library/tx35bd89.aspx)

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Revisor de clientes potencial para este tutorial era Hilton Giesenow. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Siguiente](declarative-parameters-cs.md)
