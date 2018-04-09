---
uid: web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-cs
title: Mostrar datos con ObjectDataSource (C#) | Documentos de Microsoft
author: rick-anderson
description: Este tutorial se examina el control ObjectDataSource con este control que se puede enlazar los datos recuperados de BLL creado en el tutorial anterior sin havi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: af882aef-56f5-4e9a-8f95-3977fde20e74
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: 6fbad218f3e02bde13b8788bf6f1eefac0c4990c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="displaying-data-with-the-objectdatasource-c"></a>Mostrar datos con ObjectDataSource (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_4_CS.exe) o [descarga de PDF](displaying-data-with-the-objectdatasource-cs/_static/datatutorial04cs1.pdf)

> Este tutorial se examina el control ObjectDataSource con este control que se puede enlazar los datos recuperados de BLL creado en el tutorial anterior, sin tener que escribir una línea de código.


## <a name="introduction"></a>Introducción

Con nuestra aplicación arquitectura y el sitio Web de diseño de página completa, se está listos para empezar a explorar cómo llevar a cabo una serie de tareas comunes y reporting-relacionadas con datos. En los tutoriales anteriores hemos visto cómo enlazar mediante programación los datos de la capa DAL y BLL a un control Web en una página ASP.NET de datos. Esta sintaxis de asignación del control Web de datos `DataSource` propiedad a los datos para mostrar y, a continuación, el control de llamada `DataBind()` método era el patrón usado en aplicaciones de ASP.NET 1.x y puede seguir utilizándose en sus 2.0 aplicaciones. Sin embargo, los nuevos controles de origen de datos de ASP.NET 2.0 ofrecen una manera declarativa para trabajar con datos. Uso de estos controles se pueden enlazar datos recuperados de BLL creado en el [tutorial anterior](../introduction/creating-a-business-logic-layer-cs.md) sin tener que escribir una línea de código.

ASP.NET 2.0 se suministra con cinco controles de origen de datos integrado [SqlDataSource](https://msdn.microsoft.com/library/dz12d98w(vs.80).aspx), [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx), [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a(en-US,VS.80).aspx), y [SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x(en-US,VS.80).aspx) aunque puede crear sus propios [controles de origen de datos personalizado](https://msdn.microsoft.com/library/default.asp?url=/library/dnvs05/html/DataSourceCon1.asp), si es necesario. Puesto que hemos desarrollado una arquitectura para la aplicación del tutorial, usaremos el ObjectDataSource con nuestras clases BLL.


![ASP.NET 2.0 incluye cinco controles de origen de datos integrados](displaying-data-with-the-objectdatasource-cs/_static/image1.png)

**Figura 1**: ASP.NET 2.0 incluye cinco controles de origen de datos integrados


ObjectDataSource actúa como un proxy para trabajar con otro objeto. Para configurar el ObjectDataSource especificamos este objeto y cómo se asignan lo ObjectDataSource sus métodos subyacentes `Select`, `Insert`, `Update`, y `Delete` métodos. Una vez que se ha especificado este objeto subyacente y sus métodos se asignan a la ObjectDataSource, a continuación, podemos enlazar el ObjectDataSource a un control Web de datos. ASP.NET incluye muchos datos Web controles, incluyendo la GridView, DetailsView, RadioButtonList y DropDownList, entre otros. Durante el ciclo de vida de la página, el control Web de datos que necesite tener acceso a los datos que está enlazado, que llevará a cabo mediante la invocación de su ObjectDataSource `Select` método; si los datos de control Web permiten insertar, actualizar, o eliminar, se pueden realizar llamadas a su De ObjectDataSource `Insert`, `Update`, o `Delete` métodos. Estas llamadas se enrutan por ObjectDataSource a los métodos del objeto subyacente adecuada como se muestra en el diagrama siguiente.


[![ObjectDataSource actúa como un Proxy](displaying-data-with-the-objectdatasource-cs/_static/image3.png)](displaying-data-with-the-objectdatasource-cs/_static/image2.png)

**Figura 2**: The ObjectDataSource actúa como un Proxy ([haga clic aquí para ver la imagen a tamaño completo](displaying-data-with-the-objectdatasource-cs/_static/image4.png))


Si bien ObjectDataSource puede usarse para invocar métodos para insertar, actualizar o eliminar datos, vamos a solo se centran en la devolución de datos; tutoriales futuros explorará mediante el ObjectDataSource y los datos de los controles Web que modifican datos.

## <a name="step-1-adding-and-configuring-the-objectdatasource-control"></a>Paso 1: Agregar y configurar el Control ObjectDataSource

Comience abriendo la `SimpleDisplay.aspx` página en el `BasicReporting` carpeta, cambie a la vista de diseño y, a continuación, arrastre un control ObjectDataSource desde el cuadro de herramientas a la superficie de diseño de la página. ObjectDataSource aparece como un cuadro gris en la superficie de diseño porque no produce ningún marcado; simplemente tiene acceso a datos mediante la invocación de un método de un objeto especificado. Los datos devueltos por un ObjectDataSource se pueden mostrar mediante un control Web, como GridView, DetailsView, FormView y así sucesivamente de datos.

> [!NOTE]
> También primero de puede que agregue los datos de control Web a la página y, a continuación, en la etiqueta inteligente, elija la &lt;nuevo origen de datos&gt; opción en la lista desplegable.


Para especificar el ObjectDataSource objeto subyacente y cómo se asignan los métodos del objeto para el ObjectDataSource, haga clic en el vínculo Configurar origen de datos de etiqueta inteligente de ObjectDataSource.


[![Haga clic en el vínculo a origen de datos de la etiqueta inteligente de configurar](displaying-data-with-the-objectdatasource-cs/_static/image6.png)](displaying-data-with-the-objectdatasource-cs/_static/image5.png)

**Figura 3**: haga clic en el vínculo Configurar de un origen de datos de la etiqueta inteligente ([haga clic aquí para ver la imagen a tamaño completo](displaying-data-with-the-objectdatasource-cs/_static/image7.png))


Se abrirá el Asistente para configurar orígenes de datos. En primer lugar, se debe especificar el objeto con que ObjectDataSource es trabajar. Si se activa la casilla "Mostrar sólo los componentes de datos", la lista desplegable en esta pantalla muestra solo los objetos que han sido decorados con el `DataObject` atributo. Actualmente la lista incluye los TableAdapters en el conjunto de datos con tipo y las clases BLL que se creó en el tutorial anterior. Si olvidó agregar la `DataObject` atributos a las clases de la capa de lógica empresarial no aparecen en esta lista. En ese caso, desactive la casilla "Mostrar sólo los componentes de datos" para ver todos los objetos, que deben incluir las clases BLL (junto con las demás clases en el conjunto de datos con tipo DataTables, filas de datos y así sucesivamente).

En esta primera pantalla, elija la `ProductsBLL` clase en la lista desplegable y haga clic en siguiente.


[![Especificar el objeto que se utilizan con el Control ObjectDataSource](displaying-data-with-the-objectdatasource-cs/_static/image9.png)](displaying-data-with-the-objectdatasource-cs/_static/image8.png)

**Figura 4**: especificar el objeto que se utilizan con el ObjectDataSource Control ([haga clic aquí para ver la imagen a tamaño completo](displaying-data-with-the-objectdatasource-cs/_static/image10.png))


La siguiente pantalla del asistente le solicita que seleccione qué método debe invocar el ObjectDataSource. La lista desplegable muestra los métodos que devuelven datos en el objeto seleccionado en la pantalla anterior. Aquí vemos `GetProductByProductID`, `GetProducts`, `GetProductsByCategoryID`, y `GetProductsBySupplierID`. Seleccione el `GetProducts` método desde la lista desplegable y haga clic en Finalizar (si ha agregado el `DataObjectMethodAttribute` a la `ProductBLL`de métodos tal y como se muestra en el tutorial anterior, esta opción se seleccionará de forma predeterminada).


[![Elija el método para devolver datos de la ficha Seleccionar](displaying-data-with-the-objectdatasource-cs/_static/image12.png)](displaying-data-with-the-objectdatasource-cs/_static/image11.png)

**Figura 5**: elija el método para devolver datos de la ficha SELECT ([haga clic aquí para ver la imagen a tamaño completo](displaying-data-with-the-objectdatasource-cs/_static/image13.png))


## <a name="configure-the-objectdatasource-manually"></a>Configurar manualmente el ObjectDataSource

Asistente para configurar origen de datos de ObjectDataSource ofrece una manera rápida para especificar el objeto que se usa y para asociar se invocan los métodos del objeto. Sin embargo, puede configurar el ObjectDataSource a través de sus propiedades, mediante la ventana Propiedades o directamente en el marcado declarativo. Basta con establecer la `TypeName` propiedad al tipo del objeto subyacente para usarse y el `SelectMethod` para el método que se invoca cuando se recuperan datos.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample1.aspx)]

Incluso si prefiere que el Asistente para configurar origen de datos puede darse cuando necesite configurar manualmente el ObjectDataSource, como el asistente sólo muestra clases creadas por el desarrollador. Si desea enlazar el ObjectDataSource a una clase de .NET Framework como el [clase de pertenencia](https://msdn.microsoft.com/library/system.web.security.membership.aspx)para tener acceso a información de la cuenta de usuario, o la [clase Directory](https://msdn.microsoft.com/library/system.io.directory.aspx) para trabajar con información del sistema de archivos debe establecer manualmente las propiedades de ObjectDataSource.

## <a name="step-2-adding-a-data-web-control-and-binding-it-to-the-objectdatasource"></a>Paso 2: Agregar un Control Web de datos y se enlaza a ObjectDataSource

Una vez que se ha agregado a la página y configurado el ObjectDataSource, estamos listos agregar controles Web de datos a la página para mostrar los datos devueltos por el ObjectDataSource `Select` método. Cualquier control Web de datos se pueden enlazar a un origen ObjectDataSource; Echemos un vistazo a mostrar datos de ObjectDataSource en GridView, DetailsView y FormView.

## <a name="binding-a-gridview-to-the-objectdatasource"></a>Enlazar un control GridView a ObjectDataSource

Agregar un control GridView desde el cuadro de herramientas para `SimpleDisplay.aspx`de superficie de diseño. En las etiquetas inteligentes del control GridView, elija el control ObjectDataSource que hemos agregado en el paso 1. Esto creará automáticamente un BoundField en GridView para cada propiedad devuelta por los datos desde el ObjectDataSource `Select` método (es decir, las propiedades definidas por la tabla de datos de productos).


[![Se ha agregado un control GridView a la página y enlazar a ObjectDataSource](displaying-data-with-the-objectdatasource-cs/_static/image15.png)](displaying-data-with-the-objectdatasource-cs/_static/image14.png)

**Figura 6**: A GridView se ha agregado a la página y se enlaza a ObjectDataSource ([haga clic aquí para ver la imagen a tamaño completo](displaying-data-with-the-objectdatasource-cs/_static/image16.png))


A continuación, puede personalizar, reorganizar o quitar BoundFields del GridView haciendo clic en la opción Editar columnas de la etiqueta inteligente.


[![Administrar BoundFields de GridView mediante el cuadro de diálogo Editar columnas](displaying-data-with-the-objectdatasource-cs/_static/image18.png)](displaying-data-with-the-objectdatasource-cs/_static/image17.png)

**Figura 7**: administrar BoundFields a través de las columnas de cuadro de GridView de diálogo Editar ([haga clic aquí para ver la imagen a tamaño completo](displaying-data-with-the-objectdatasource-cs/_static/image19.png))


Tómese un momento para modificar BoundFields de GridView, quitar la `ProductID`, `SupplierID`, `CategoryID`, `QuantityPerUnit`, `UnitsInStock`, `UnitsOnOrder`, y `ReorderLevel` BoundFields. Simplemente seleccione el BoundField en la lista en la parte inferior izquierda y haga clic en el botón Eliminar (la X roja) para quitarlos. A continuación, volver a organizar el BoundFields para que la `CategoryName` y `SupplierName` BoundFields preceden a la `UnitPrice` BoundField seleccionando estos BoundFields y haga clic en la flecha hacia arriba. Establecer el `HeaderText` propiedades de las restantes BoundFields a `Products`, `Category`, `Supplier`, y `Price`, respectivamente. A continuación, pida a los `Price` BoundField da formato como una moneda estableciendo el BoundField `HtmlEncode` propiedad en False y su `DataFormatString` propiedad `{0:c}`. Por último, Alinear horizontalmente el `Price` a la derecha y la `Discontinued` casilla de verificación en el centro a través de la `ItemStyle` / `HorizontalAlign` propiedad.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample2.aspx)]


[![Se ha personalizado BoundFields de GridView](displaying-data-with-the-objectdatasource-cs/_static/image21.png)](displaying-data-with-the-objectdatasource-cs/_static/image20.png)

**Figura 8**: se ha personalizado el GridView BoundFields ([haga clic aquí para ver la imagen a tamaño completo](displaying-data-with-the-objectdatasource-cs/_static/image22.png))


## <a name="using-themes-for-a-consistent-look"></a>Uso de los temas para una apariencia coherente

Estos tutoriales se esfuerzan por quitar cualquier configuración de estilo de nivel de control, en lugar de utilizar hojas de estilos en cascada definido en un archivo externo siempre que sea posible. El `Styles.css` archivo contiene `DataWebControlStyle`, `HeaderStyle`, `RowStyle`, y `AlternatingRowStyle` clases CSS que se deben usar para dictar la apariencia de los datos Web controles usados en estos tutoriales. Para ello, podríamos establecemos la GridView `CssClass` propiedad `DataWebControlStyle`y su `HeaderStyle`, `RowStyle`, y `AlternatingRowStyle` propiedades `CssClass` propiedades según corresponda.

Si se establece estos `CssClass` propiedades en el control de Web se necesita recordar establecer explícitamente estos valores de propiedad para los datos de cada control agregado a nuestros tutoriales de Web. Un enfoque más fácil de administrar consiste en definir las propiedades de CSS predeterminado del control GridView, DetailsView, y FormView se controla mediante un tema. Un tema es una colección de valores de las propiedades de nivel de control, imágenes y clases CSS que se pueden aplicar a las páginas a través de un sitio para aplicar una apariencia común.

Nuestro tema no incluye las imágenes o archivos CSS (dejaremos la hoja de estilos `Styles.css` como-se, definido en la carpeta raíz de la aplicación web), pero incluye las dos máscaras. Una máscara es un archivo que define las propiedades predeterminadas de un control Web. En concreto, tendremos un archivo de máscara para los controles GridView y DetailsView, que indica el valor predeterminado `CssClass`-propiedades relacionadas.

Empiece agregando un nuevo archivo de máscara al proyecto denominado `GridView.skin` , haga doble clic en el nombre del proyecto en el Explorador de soluciones y elija Agregar nuevo elemento.


[![Agregue un archivo de máscara denominado GridView.skin](displaying-data-with-the-objectdatasource-cs/_static/image24.png)](displaying-data-with-the-objectdatasource-cs/_static/image23.png)

**Figura 9**: agregar una máscara de archivo con el nombre `GridView.skin` ([haga clic aquí para ver la imagen a tamaño completo](displaying-data-with-the-objectdatasource-cs/_static/image25.png))


Los archivos de máscaras deben ubicarse en un tema, que se encuentran en el `App_Themes` carpeta. Puesto que aún no tenemos dicha carpeta, Visual Studio le ofrece, instálelo crear uno para que podamos al agregar la primera máscara. Haga clic en Sí para crear el `App_Theme` carpeta y coloque el nuevo `GridView.skin` archivo no existe.


[![Dejar que Visual Studio cree la carpeta App_Theme](displaying-data-with-the-objectdatasource-cs/_static/image27.png)](displaying-data-with-the-objectdatasource-cs/_static/image26.png)

**Figura 10**: permitir que Visual Studio crea el `App_Theme` carpeta ([haga clic aquí para ver la imagen a tamaño completo](displaying-data-with-the-objectdatasource-cs/_static/image28.png))


Esto creará un nuevo tema en el `App_Themes` carpeta denominada GridView con el archivo de máscara `GridView.skin`.


![El tema de GridView tiene ha agregado a la carpeta App_Theme](displaying-data-with-the-objectdatasource-cs/_static/image29.png)

**Figura 11**: el tema de GridView se ha agregado a la `App_Theme` carpeta


Cambie el nombre del tema de GridView a DataWebControls (haga doble clic en la carpeta de GridView en la `App_Theme` carpeta y seleccione Cambiar nombre). A continuación, escriba el siguiente marcado en el `GridView.skin` archivo:


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample3.aspx)]

Esto define las propiedades predeterminadas para el `CssClass`-relacionados con propiedades de cualquier GridView en cualquier página que utiliza el tema DataWebControls. Vamos a agregar otra máscara para DetailsView, un control Web que se va a usar en breve de datos. Agregar un nuevo aspecto al tema DataWebControls denominado `DetailsView.skin` y agregue el siguiente marcado:


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample4.aspx)]

Con nuestro tema definido, el último paso es aplicar el tema a nuestra página de ASP.NET. Puede aplicar un tema según una página a página o para todas las páginas en un sitio Web. Vamos a usar este tema para todas las páginas en el sitio Web. Para ello, agregue el siguiente marcado al `Web.config`del `<system.web>` sección:


[!code-xml[Main](displaying-data-with-the-objectdatasource-cs/samples/sample5.xml)]

Eso es todo lo! El `styleSheetTheme` valor indica que las propiedades especificadas en el tema deben *no* invalidar las propiedades especificadas en el nivel de control. Para especificar que los valores de tema deben cambiar predeterminada configuración de control, use la `theme` atributo en lugar de `styleSheetTheme`; Desafortunadamente, los valores de tema especificados a través de la `theme` atributo no aparecen en la vista de diseño de Visual Studio. Hacer referencia a [información general de máscaras y temas de ASP.NET](https://msdn.microsoft.com/library/ykzx33wh.aspx) y [servidor estilos Using Themes](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx) para obtener más información sobre los temas y máscaras; vea [How To: Apply ASP.NET Themes](https://msdn.microsoft.com/library/0yy5hxdk(VS.80).aspx) para obtener más información sobre Cómo configurar una página para que utilice un tema.


[![GridView muestra el nombre del producto, categoría, proveedor, precio e información no incluida](displaying-data-with-the-objectdatasource-cs/_static/image31.png)](displaying-data-with-the-objectdatasource-cs/_static/image30.png)

**Figura 12**: GridView muestra el nombre, categoría, proveedor, precio y no incluye información del producto ([haga clic aquí para ver la imagen a tamaño completo](displaying-data-with-the-objectdatasource-cs/_static/image32.png))


## <a name="displaying-one-record-at-a-time-in-the-detailsview"></a>Mostrar un registro a la vez en DetailsView

GridView muestra una fila por cada registro devuelto por el control de origen de datos al que está enlazado. Hay ocasiones, sin embargo, al que podríamos desear mostrar un registro único o un solo registro a la vez. El [control DetailsView](https://msdn.microsoft.com/library/s3w1w7t4.aspx) ofrece esta funcionalidad, como un elemento HTML de representación `<table>` con dos columnas y una fila por cada columna o propiedad enlazada al control. DetailsView se puede considerar como un control GridView con un único registro gira 90 grados.

Empiece agregando un control DetailsView *anteriormente* GridView en `SimpleDisplay.aspx`. A continuación, enlazarlo al mismo control ObjectDataSource como GridView. Al igual que con el control GridView, BoundField se agregará a la DetailsView para cada propiedad en el objeto devuelto por la ObjectDataSource `Select` método. La única diferencia es que BoundFields de DetailsView están dispuestos horizontalmente en lugar de verticalmente.


[![Agregue un DetailsView a la página y enlazarla a ObjectDataSource](displaying-data-with-the-objectdatasource-cs/_static/image34.png)](displaying-data-with-the-objectdatasource-cs/_static/image33.png)

**Figura 13**: agregar un DetailsView a la página y enlazarla a ObjectDataSource ([haga clic aquí para ver la imagen a tamaño completo](displaying-data-with-the-objectdatasource-cs/_static/image35.png))


Al igual que el control GridView, BoundFields de DetailsView originales se pueden modificar para proporcionar una presentación más personalizada de los datos devueltos por el ObjectDataSource. Figura 14 muestra DetailsView después de su BoundFields y `CssClass` propiedades se han configurado para que su aspecto similar al ejemplo de GridView.


[![DetailsView muestra un único registro](displaying-data-with-the-objectdatasource-cs/_static/image37.png)](displaying-data-with-the-objectdatasource-cs/_static/image36.png)

**Figura 14**: DetailsView muestra un único registro ([haga clic aquí para ver la imagen a tamaño completo](displaying-data-with-the-objectdatasource-cs/_static/image38.png))


Tenga en cuenta que la DetailsView muestra solo el primer registro devuelto por su origen de datos. Para permitir al usuario recorrer todos los registros, uno en uno, debemos habilitar la paginación de DetailsView. Para ello, vuelva a Visual Studio y Active la casilla Habilitar paginación en la etiqueta inteligente de DetailsView.


[![Habilitar la paginación en el Control DetailsView](displaying-data-with-the-objectdatasource-cs/_static/image40.png)](displaying-data-with-the-objectdatasource-cs/_static/image39.png)

**Figura 15**: habilitar la paginación en el DetailsView Control ([haga clic aquí para ver la imagen a tamaño completo](displaying-data-with-the-objectdatasource-cs/_static/image41.png))


[![Con la paginación habilitada, DetailsView permite al usuario ver cualquiera de los productos](displaying-data-with-the-objectdatasource-cs/_static/image43.png)](displaying-data-with-the-objectdatasource-cs/_static/image42.png)

**Figura 16**: con paginación habilitado, DetailsView permite al usuario ver cualquiera de los productos ([haga clic aquí para ver la imagen a tamaño completo](displaying-data-with-the-objectdatasource-cs/_static/image44.png))


Hablaremos más acerca de la paginación en el futuro tutoriales.

## <a name="a-more-flexible-layout-for-showing-one-record-at-a-time"></a>Un diseño más Flexible para mostrar un único registro a la vez

DetailsView es bastante rígida en cómo se muestra cada registro devuelto desde el origen ObjectDataSource. Puede que convenga una vista más flexible de los datos. Por ejemplo, en lugar de mostrar el nombre del producto, categoría, proveedor, precio y discontinuo información en una fila independiente, podemos queremos mostrar el nombre del producto y el precio en un `<h4>` encabezado con la información de categoría y proveedores que aparecen a continuación el nombre y el precio en un tamaño de fuente. Y no nos podemos importa mostrar los nombres de propiedad (producto, categoría etc.) junto a los valores.

El [control FormView](https://msdn.microsoft.com/library/fyf1dk77.aspx) proporciona este nivel de personalización. En lugar de usar campos (como la GridView y DetailsView no), FormView utiliza plantillas, que permiten una mezcla de controles Web, HTML estático, y [sintaxis de enlace de datos](http://www.15seconds.com/issue/040630.htm). Si está familiarizado con el control de repetidor de ASP.NET 1.x, se puede considerar el FormView como repetidor para mostrar un único registro.

Agregar un control FormView a la `SimpleDisplay.aspx` superficie de diseño de la página. Inicialmente la FormView muestra como un bloque de color gris, nos que informa que es necesario proporcionar, como mínimo, el control `ItemTemplate`.


[![FormView deben incluir un ItemTemplate](displaying-data-with-the-objectdatasource-cs/_static/image46.png)](displaying-data-with-the-objectdatasource-cs/_static/image45.png)

**Figura 17**: The FormView debe incluir un `ItemTemplate` ([haga clic aquí para ver la imagen a tamaño completo](displaying-data-with-the-objectdatasource-cs/_static/image47.png))


Puede enlazar FormView directamente a un control de origen de datos a través de la etiqueta inteligente de FormView, lo cual creará una predeterminada `ItemTemplate` automáticamente (junto con un `EditItemTemplate` y `InsertItemTemplate`, si el control de ObjectDatatSource `InsertMethod` y `UpdateMethod` se establecen propiedades). Sin embargo, en este ejemplo vamos a enlazar los datos a FormView y especificar su `ItemTemplate` manualmente. Empezar configurando la FormView `DataSourceID` propiedad a la `ID` del control ObjectDataSource, `ObjectDataSource1`. A continuación, cree el `ItemTemplate` para que se muestre el nombre del producto y el precio en un `<h4>` elemento y los nombres de categoría y el remitente por debajo de ese con un tamaño de fuente.


[!code-aspx[Main](displaying-data-with-the-objectdatasource-cs/samples/sample6.aspx)]


[![El primer producto (Chai) se muestra en un formato personalizado](displaying-data-with-the-objectdatasource-cs/_static/image49.png)](displaying-data-with-the-objectdatasource-cs/_static/image48.png)

**Figura 18**: el primer producto (Chai) se muestra en un formato personalizado ([haga clic aquí para ver la imagen a tamaño completo](displaying-data-with-the-objectdatasource-cs/_static/image50.png))


El `<%# Eval(propertyName) %>` se muestra la sintaxis de enlace de datos. El `Eval` método devuelve el valor de la propiedad especificada para el objeto actual que se va a enlazar al control FormView. Consulte el artículo de Alex Homero [simplificado y extendidos sintaxis enlace de datos en ASP.NET 2.0](http://www.15seconds.com/issue/040630.htm) para obtener más información sobre las ventajas y desventajas de enlace de datos.

Como el DetailsView FormView solo muestra el primer registro devuelto de ObjectDataSource. Puede habilitar la paginación en FormView para permitir que los visitantes de paso a través de los productos de uno en uno.

## <a name="summary"></a>Resumen

Obtener acceso y mostrar datos de una capa de lógica empresarial pueden realizarse sin escribir una línea de código gracias al control ObjectDataSource de ASP.NET 2.0. ObjectDataSource invoca un método especificado de una clase y devuelve los resultados. Estos resultados se pueden mostrar en un control Web que está enlazado a ObjectDataSource de datos. En este tutorial, observamos enlazar los controles GridView, DetailsView y FormView a ObjectDataSource.

Hasta ahora sólo hemos visto cómo utilizar el ObjectDataSource para invocar un método sin parámetros, pero ¿qué ocurre si desea invocar un método que espera parámetros de entrada, como el `ProductBLL` la clase `GetProductsByCategoryID(categoryID)`? Para poder llamar a un método que espera uno o más parámetros debemos configuramos el ObjectDataSource para especificar los valores para estos parámetros. Veremos cómo hacerlo en nuestro [siguiente tutorial](declarative-parameters-cs.md).

Feliz programación.

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Crear sus propios controles de origen de datos](https://msdn.microsoft.com/library/ms364049.aspx)
- [Ejemplos de GridView para ASP.NET 2.0](https://msdn.microsoft.com/library/aa479339.aspx)
- [Simplificado y datos de enlace de sintaxis en ASP.NET 2.0 extendidos](http://www.15seconds.com/issue/040630.htm)
- [Temas en ASP.NET 2.0](http://www.odetocode.com/Articles/423.aspx)
- [Estilos de servidor con temas](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx)
- [Cómo: Aplicar temas de ASP.NET mediante programación](https://msdn.microsoft.com/library/tx35bd89.aspx)

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Revisor inicial para este tutorial era Hilton Giesenow. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Siguiente](declarative-parameters-cs.md)
