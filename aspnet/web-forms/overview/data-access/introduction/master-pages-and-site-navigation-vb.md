---
uid: web-forms/overview/data-access/introduction/master-pages-and-site-navigation-vb
title: Páginas maestras y navegación del sitio (VB) | Microsoft Docs
author: rick-anderson
description: Una característica común de sitios Web fácil de usar es que tienen un esquema de diseño y la navegación de página coherente en todo el sitio. Este tutorial examina cómo y...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 022801d8-a327-4d0c-8780-6094c9cee00d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/introduction/master-pages-and-site-navigation-vb
msc.type: authoredcontent
ms.openlocfilehash: c823421a6b934951dd08656b48801c3d26247c5c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372049"
---
<a name="master-pages-and-site-navigation-vb"></a>Páginas maestras y navegación del sitio (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargue la aplicación de ejemplo](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_3_VB.exe) o [descargar PDF](master-pages-and-site-navigation-vb/_static/datatutorial03vb1.pdf)

> Una característica común de sitios Web fácil de usar es que tienen un esquema de diseño y la navegación de página coherente en todo el sitio. Este tutorial examina cómo puede crear una apariencia coherente en todas las páginas que fácilmente se pueden actualizar.


## <a name="introduction"></a>Introducción

Una característica común de sitios Web fácil de usar es que tienen un esquema de diseño y la navegación de página coherente en todo el sitio. ASP.NET 2.0 introduce dos nuevas características que simplifican considerablemente la implementación de ambos un esquema de diseño y la navegación de página de todo el sitio: páginas maestras y navegación del sitio. Las páginas principales permiten a los desarrolladores crear una plantilla de todo el sitio con regiones modificables designadas. Esta plantilla, a continuación, se puede aplicar a las páginas ASP.NET en el sitio. Estas páginas ASP.NET sólo necesitan proporcionar el contenido de la página maestra especificado del regiones modificables las demás marcas de la página maestra es idéntico en todas las páginas ASP.NET que utilicen la página principal. Este modelo permite a los desarrolladores definir y unificar un diseño de página de todo el sitio, por lo que sea más fácil crear una apariencia coherente en todas las páginas que fácilmente se pueden actualizar.

El [sistema de navegación del sitio](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx) proporciona tanto un mecanismo para que los desarrolladores de páginas definir un mapa del sitio y una API para esa asignación de sitio para consultarse mediante programación. Los nuevos controles de navegación Web que Menu, TreeView y SiteMapPath facilitan representan todo o parte del mapa del sitio en un elemento de interfaz de usuario de navegación comunes. Vamos a usar el proveedor de navegación del sitio predeterminado, lo que significa que nuestro mapa del sitio se definirá en un archivo con formato XML.

Para ilustrar estos conceptos y hacer más fácil de usar nuestro sitio Web de tutoriales, dediquemos en esta lección, definir un diseño de página de todo el sitio, la implementación de un mapa del sitio y agregar la interfaz de usuario de navegación. Al final de este tutorial, tendrá un diseño de sitio Web perfeccionado para construir nuestras páginas web del tutorial.


[![El resultado final de este Tutorial](master-pages-and-site-navigation-vb/_static/image2.png)](master-pages-and-site-navigation-vb/_static/image1.png)

**Figura 1**: resultado de finalización el para este Tutorial ([haga clic aquí para ver imagen en tamaño completo](master-pages-and-site-navigation-vb/_static/image3.png))


## <a name="step-1-creating-the-master-page"></a>Paso 1: Creación de la página maestra

El primer paso es crear la página principal del sitio. En este momento nuestro sitio Web consta de sólo el conjunto de datos con tipo (`Northwind.xsd`, en el `App_Code` carpeta), las clases BLL (`ProductsBLL.vb`, `CategoriesBLL.vb`, etc., todo en el `App_Code` carpeta), la base de datos (`NORTHWND.MDF`, en el `App_Data` carpeta), el archivo de configuración (`Web.config`) y un archivo de hoja de estilos CSS (`Styles.css`). Vacié las páginas y archivos que mostraban el uso de DAL y BLL desde los dos primeros tutoriales porque analizaremos esos ejemplos con más detalle en próximos tutoriales.


![Los archivos en el proyecto](master-pages-and-site-navigation-vb/_static/image4.png)

**Figura 2**: los archivos en el proyecto


Para crear una página principal, haga doble clic en el nombre del proyecto en el Explorador de soluciones y seleccione Agregar nuevo elemento. A continuación, seleccione el tipo de página maestra en la lista de plantillas y asígnele el nombre `Site.master`.


[![Agregar una página principal nueva al sitio Web](master-pages-and-site-navigation-vb/_static/image6.png)](master-pages-and-site-navigation-vb/_static/image5.png)

**Figura 3**: agregar una página principal nueva al sitio Web ([haga clic aquí para ver imagen en tamaño completo](master-pages-and-site-navigation-vb/_static/image7.png))


Definir el diseño de página de todo el sitio aquí en la página maestra. Puede usar la vista de diseño y agregar los controles Web o de diseño que necesita, o puede agregar manualmente el marcado de forma manual en la vista del origen. En mi página principal uso [hojas de estilos en cascada](http://www.w3schools.com/css/default.asp) para colocar y estilos con configuración CSS definidos en el archivo externo `Style.css`. Mientras que no se puede deducirlo del marcado que se muestra a continuación, se definen las reglas de CSS que el panel de navegación `<div>`del absolutamente se coloca el contenido por lo que aparece a la izquierda y tiene un ancho fijo de 200 píxeles.

Site.master


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample1.aspx)]

Una página principal define el diseño de página estática y las regiones que se pueden editar mediante las páginas ASP.NET que utilicen la página principal. Estas regiones contenidas editables se indican mediante el control ContentPlaceHolder, que se puede ver dentro del contenido `<div>`. Esta página principal tiene sólo un ContentPlaceHolder (`MainContent`), pero la página principal puede tener varios ContentPlaceHolders.

Con el marcado anterior, cambiar a la vista Diseño, muestra el diseño de la página maestra. Todas las páginas ASP.NET que usan esta página principal tendrán este diseño uniforme y con capacidad para especificar el marcado para el `MainContent` región.


[![La página maestra, cuando se ven a través de la vista de diseño](master-pages-and-site-navigation-vb/_static/image9.png)](master-pages-and-site-navigation-vb/_static/image8.png)

**Figura 4**: la página maestra, al ver a través de la vista de diseño ([haga clic aquí para ver imagen en tamaño completo](master-pages-and-site-navigation-vb/_static/image10.png))


## <a name="step-2-adding-a-homepage-to-the-website"></a>Paso 2: Agregar una página de inicio al sitio Web

Con la página principal definida, ya estamos listos para agregar las páginas ASP.NET para el sitio Web. Comencemos por agregar `Default.aspx`, página principal de nuestro sitio Web. Haga doble clic en el nombre del proyecto en el Explorador de soluciones y seleccione Agregar nuevo elemento. Elija la opción formulario Web de la lista de plantillas y el nombre del archivo `Default.aspx`. Compruebe también la casilla de verificación "Seleccionar la página principal".


[![Agregue un nuevo formulario Web, comprobación de la casilla de verificación Seleccionar la página maestra](master-pages-and-site-navigation-vb/_static/image12.png)](master-pages-and-site-navigation-vb/_static/image11.png)

**Figura 5**: agregar un nuevo formulario Web, comprobación de la casilla de verificación Seleccionar la página maestra ([haga clic aquí para ver imagen en tamaño completo](master-pages-and-site-navigation-vb/_static/image13.png))


Después de hacer clic en el botón Aceptar, se nos pide que seleccionemos la página principal que debe usar esta nueva página ASP.NET. Aunque es posible tener varias páginas principales en el proyecto, tengamos sólo una.


[![Elija la página maestra que se debe usar esta página de ASP.NET](master-pages-and-site-navigation-vb/_static/image15.png)](master-pages-and-site-navigation-vb/_static/image14.png)

**Figura 6**: elija la página principal de esta página de ASP.NET debe utilice ([haga clic aquí para ver imagen en tamaño completo](master-pages-and-site-navigation-vb/_static/image16.png))


Después de elegir la página principal, las páginas ASP.NET nuevas contendrá el siguiente marcado:

Default.aspx


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample2.aspx)]

En el `@Page` directiva es una referencia al archivo de página principal usado (`MasterPageFile="~/Site.master"`), y el marcado de la página ASP.NET contiene un control de contenido para cada uno de los controles ContentPlaceHolder definidos en la página principal, con el control `ContentPlaceHolderID` asignar el contenido de control para un ContentPlaceHolder específico. El control Content es donde se coloca el marcado que desee que aparezca en el ContentPlaceHolder correspondiente. Establecer el `@Page` la directiva `Title` atributo a la página principal y agregar algún contenido de bienvenida al control de contenido:

Default.aspx


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample3.aspx)]

El `Title` atributo en el `@Page` directiva nos permite establecer título de la página en la página ASP.NET, aunque el `<title>` elemento está definido en la página maestra. También podemos establecer el título mediante programación, utilizando `Page.Title`. Tenga en cuenta también que las referencias de la página principal a las hojas de estilo (como `Style.css`) se actualizan automáticamente para que funcionen en cualquier página ASP.NET, independientemente del directorio de la página ASP.NET está en relativa a la página maestra.

Cambiar a la vista Diseño, que podemos ver cómo se vería nuestra página en un explorador. Tenga en cuenta que en el diseño de la vista de la página ASP.NET que son editables solo las regiones contenidas editables la marca non-ContentPlaceHolder definida en la página maestra está atenuado.


[![La vista de diseño de la página ASP.NET muestra las regiones modificables y no modificables](master-pages-and-site-navigation-vb/_static/image18.png)](master-pages-and-site-navigation-vb/_static/image17.png)

**Figura 7**: la vista de diseño para el ASP.NET página muestra tanto el modificable y regiones no modificable ([haga clic aquí para ver imagen en tamaño completo](master-pages-and-site-navigation-vb/_static/image19.png))


Cuando el `Default.aspx` un explorador visita una página, el motor ASP.NET combina automáticamente el contenido de la página principal de la página y el ASP. NET del contenido y representa el contenido combinado en el formato HTML final que se envía al explorador solicitante. Cuando se actualiza el contenido de la página maestra, todas las páginas ASP.NET que usan esta página principal tendrán a combinar con la nueva página maestra contenida la próxima vez que se solicitan su contenido. En resumen, permite que el modelo de página maestra para una sola página plantilla de diseño definido (página principal) cuyos cambios se reflejan inmediatamente en todo el sitio.

## <a name="adding-additional-aspnet-pages-to-the-website"></a>Adición de páginas ASP.NET adicionales al sitio Web

Dedique un momento para agregar códigos auxiliares de páginas ASP.NET adicionales al sitio que contendrá finalmente las diferentes demostraciones de informes. Habrá más de 35 demostraciones en total, por lo que en lugar de crear todas las páginas de código auxiliar, vamos a simplemente crear las primeras. Puesto que también habrá muchas categorías de demostraciones, para administrar mejor las demostraciones de agregar una carpeta para las categorías. Por ahora, agregue las tres carpetas siguientes:

- `BasicReporting`
- `Filtering`
- `CustomFormatting`

Finalmente, agregue archivos nuevos como se muestra en el Explorador de soluciones en la figura 8. Al agregar cada archivo, recuerde comprobar la casilla de verificación "Seleccionar la página principal".


![Agregue los siguientes archivos](master-pages-and-site-navigation-vb/_static/image20.png)

**Figura 8**: agregue los siguientes archivos


## <a name="step-2-creating-a-site-map"></a>Paso 2: Crear un mapa del sitio

Uno de los desafíos de la administración de un sitio Web que se compone de más de unas cuantas páginas es proporcionar un método sencillo para los visitantes a navegar por el sitio. Para empezar, se debe definir la estructura de navegación del sitio. A continuación, esta estructura debe traducirse a elementos de interfaz de usuario navegable, como menús o elementos tipo breadcrumb. Por último, todo este proceso debe mantener y actualizar cuando se agregan nuevas páginas para el sitio y la eliminación de otras existentes. Antes de ASP.NET 2.0, los desarrolladores se encontraban en sus propios para crear estructura de navegación del sitio, su mantenimiento y traducirlo a elementos de la interfaz de usuario navegable. Con ASP.NET 2.0, sin embargo, los desarrolladores pueden usar muy flexible creado en el sistema de navegación del sitio.

El sistema de navegación del sitio de ASP.NET 2.0 proporciona un medio para que un desarrollador definir un mapa del sitio y, a continuación, obtener acceso a esta información a través de una API de programación. ASP.NET incluye un proveedor del mapa del sitio que se espera que los datos del mapa del sitio se almacenen en un archivo XML con formato de una manera determinada. Pero, puesto que el sistema de navegación del sitio se basa el [modelo de proveedor](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx) puede ampliarse para admitir las formas alternativas para serializar la información de mapa del sitio. Artículo de Prosise, [The SQL Site Map proveedor se ve Been Waiting For](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx) muestra cómo crear un proveedor del mapa del sitio que almacena el mapa del sitio en una base de datos de SQL Server; otra opción consiste en crear [según un proveedor del mapa del sitio la estructura del sistema de archivos](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx).

Para este tutorial, sin embargo, vamos a usar el proveedor del mapa de sitio predeterminado que se incluye con ASP.NET 2.0. Para crear el mapa del sitio, simplemente haga doble clic en el nombre del proyecto en el Explorador de soluciones, seleccione Agregar nuevo elemento y elija la opción de mapa del sitio. Deje el nombre como `Web.sitemap` y haga clic en el botón Agregar.


[![Agregar un mapa del sitio al proyecto](master-pages-and-site-navigation-vb/_static/image22.png)](master-pages-and-site-navigation-vb/_static/image21.png)

**Figura 9**: agregar un mapa del sitio para el proyecto ([haga clic aquí para ver imagen en tamaño completo](master-pages-and-site-navigation-vb/_static/image23.png))


El archivo de mapa del sitio es un archivo XML. Tenga en cuenta que Visual Studio proporciona IntelliSense para la estructura de mapa del sitio. El archivo de asignación de sitio debe tener la `<siteMap>` nodo como nodo raíz, que debe contener exactamente un `<siteMapNode>` elemento secundario. Primero `<siteMapNode>` elemento, a continuación, puede contener un número arbitrario de descendientes `<siteMapNode>` elementos.

Definir el mapa del sitio para imitar la estructura del sistema de archivos. Es decir, agregue un `<siteMapNode>` (elemento) para cada una de las tres carpetas y secundarios `<siteMapNode>` elementos para cada una de las páginas ASP.NET en esas carpetas, así:

Web.SiteMap


[!code-xml[Main](master-pages-and-site-navigation-vb/samples/sample4.xml)]

El mapa del sitio define la estructura de navegación del sitio Web, que es una jerarquía que se describe las diversas secciones del sitio. Cada `<siteMapNode>` elemento `Web.sitemap` representa una sección de la estructura de navegación del sitio.


[![El mapa del sitio representa una estructura de navegación jerárquica](master-pages-and-site-navigation-vb/_static/image25.png)](master-pages-and-site-navigation-vb/_static/image24.png)

**Figura 10**: el mapa del sitio representa una estructura de navegación jerárquica ([haga clic aquí para ver imagen en tamaño completo](master-pages-and-site-navigation-vb/_static/image26.png))


ASP.NET expone la estructura del mapa del sitio a través de .NET Framework [clase SiteMap class](https://msdn.microsoft.com/library/system.web.sitemap.aspx). Esta clase tiene un `CurrentNode` propiedad, que devuelve información acerca de la sección que el usuario está visitando; la `RootNode` propiedad devuelve la raíz del mapa del sitio (inicio en nuestro mapa del sitio). Tanto el `CurrentNode` y `RootNode` devuelven propiedades [SiteMapNode](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) las instancias que tienen propiedades como `ParentNode`, `ChildNodes`, `NextSibling`, `PreviousSibling`, y así sucesivamente, que permiten que el mapa del sitio jerarquía que se desplazará.

## <a name="step-3-displaying-a-menu-based-on-the-site-map"></a>Paso 3: Mostrar un menú según el mapa del sitio

Acceso a datos en ASP.NET 2.0 pueden llevarlo a cabo mediante programación, como en ASP.NET 1.x, o mediante declaración a través del nuevo [controles de origen de datos](https://msdn.microsoft.com/library/ms227679.aspx). Hay varios controles de origen de datos integrado como el control SqlDataSource para tener acceso a datos de la base de datos relacional, el control ObjectDataSource, para tener acceso a datos de las clases y otros. Incluso puede crear sus propios [controles de origen de datos personalizados](https://msdn.microsoft.com/asp.net/reference/data/default.aspx?pull=/library/dnvs05/html/DataSourceCon1.asp).

Los controles de origen de datos actúan como un proxy entre su página ASP.NET y los datos subyacentes. Con el fin de mostrar los datos recuperados del control de origen de datos, normalmente se le agrega otro control Web a la página y enlácelo al control de origen de datos. Para enlazar un control Web a un control de origen de datos, basta con establecer el control Web `DataSourceID` propiedad en el valor del control de origen de datos `ID` propiedad.

Para ayudar a trabajar con datos del mapa del sitio, ASP.NET incluye el control SiteMapDataSource, que nos permite enlazar un control Web en el mapa del sitio de nuestro sitio Web. Dos controles Web TreeView y Menu normalmente se usan para proporcionar una interfaz de usuario de navegación. Para enlazar los datos del mapa de sitio a uno de estos controles, simplemente agregue un SiteMapDataSource a la página junto con un control TreeView o control de menú cuya `DataSourceID` propiedad se establece en consecuencia. Por ejemplo, podríamos agregar un control de menú a la página maestra mediante el siguiente marcado:


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample5.aspx)]

Para un mayor grado de control sobre la HTML emitida, podemos enlazar el control SiteMapDataSource al control Repeater, así:


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample6.aspx)]

El control SiteMapDataSource devuelve el nivel de una jerarquía del mapa de sitio a la vez, empezando por el nodo raíz del mapa (inicio en nuestro mapa del sitio), a continuación, el siguiente nivel (Basic Reporting, Filtering Reports y Customized Formatting) y así sucesivamente. Cuando se enlaza SiteMapDataSource a un Repeater, enumera el primer nivel devuelto y crea una instancia de la `ItemTemplate` para cada `SiteMapNode` instancia en el primer nivel. Para obtener acceso a una propiedad concreta de la `SiteMapNode`, podemos usar `Eval(propertyName)`, que es cómo obtenemos cada `SiteMapNode`del `Url` y `Title` propiedades para el control de hipervínculo.

El ejemplo de Repeater anterior representará el marcado siguiente:


[!code-html[Main](master-pages-and-site-navigation-vb/samples/sample7.html)]

Estos nodos de mapa del sitio (Basic Reporting, Filtering Reports y Customized Formatting) forman el *segundo* nivel del mapa del sitio que se va a representar, no en la primera. Esto es porque el SiteMapDataSource `ShowStartingNode` propiedad está establecida en False, provocando el SiteMapDataSource omita el nodo raíz del mapa y comenzar en su lugar, devuelve el segundo nivel en la jerarquía del mapa del sitio.

Para mostrar los elementos secundarios de la Basic Reporting, Filtering Reports y Customized Formatting `SiteMapNode` s, podemos agregar otro Repeater del Repeater inicial `ItemTemplate`. Este segundo Repeater se enlaza a la `SiteMapNode` la instancia `ChildNodes` propiedad, así:


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample8.aspx)]

Estos dos Repeater generan el siguiente marcado (algunas marcas se han omitido por razones de brevedad):


[!code-html[Main](master-pages-and-site-navigation-vb/samples/sample9.html)]

Mediante los estilos CSS elegidos del [Rachel Andrew](http://www.rachelandrew.co.uk/)del libro [The CSS Anthology: 101 Essential Tips, Tricks, &amp; Hacks](https://www.amazon.com/gp/product/0957921888/qid=1137565739/sr=8-1/ref=pd_bbs_1/103-0562306-3386214?n=507846&amp;s=books&amp;v=glance), el `<ul>` y `<li>` se aplica el estilo de elementos que el marcado genera el siguiente resultado visual:


![Menú formado por dos Repeater y varios estilos CSS](master-pages-and-site-navigation-vb/_static/image27.png)

**Figura 11**: menú formado por dos Repeater y varios estilos CSS


Este menú está en la página maestra y enlazado al mapa del sitio definido en `Web.sitemap`, lo que significa que cualquier cambio en el mapa del sitio se reflejará inmediatamente en todas las páginas que usen el `Site.master` página maestra.

## <a name="disabling-viewstate"></a>Deshabilitación de ViewState

Todos los controles ASP.NET pueden mantener opcionalmente su estado en el [estado de vista](https://msdn.microsoft.com/msdnmag/issues/03/02/CuttingEdge/), que se serializa como un campo de formulario oculto en el HTML representado. Estado de vista se usa por los controles a recordar su estado cambiado mediante programación a través de devoluciones de datos, como los datos enlazados a un control Web de datos. Mientras el estado de vista permite recordar postbacks la información, aumenta el tamaño del marcado que se debe enviar al cliente y puede provocar un sobredimensionamiento graves de página si no se controla. Los controles Web de datos especial el control GridView son particularmente adecuados para agregar cantidades adicionales de kilobytes al marcado para una página. Mientras que dicho incremento puede ser insignificante para los usuarios de banda ancha o una intranet, el estado de vista puede sumar varios segundos en la ida y vuelta para que los usuarios de acceso telefónico.

Para ver el impacto del estado de vista, visite una página en un explorador y, a continuación, ver el código fuente emitido por la página web (en Internet Explorer, vaya al menú Ver y elija la opción de origen). También puede activar [seguimiento de página](https://msdn.microsoft.com/library/sfbfw58f.aspx) para ver la asignación del estado de vista utilizada por cada uno de los controles en la página. La información de estado de vista se serializa en un campo de formulario oculto denominado `__VIEWSTATE`, que se encuentra en un `<div>` elemento inmediatamente después de la apertura `<form>` etiqueta. Solo se conserva el estado de vista cuando hay un formulario Web que se va a utilizar; Si su página ASP.NET no incluye un `<form runat="server">` en su sintaxis declarativa no habrá una `__VIEWSTATE` campo de formulario oculto en el marcado representado.

El `__VIEWSTATE` campo de formulario generado por la página maestra agrega alrededor de 1.800 bytes al marcado generado de la página. Esta recarga adicional vence principalmente para el control Repeater, como el contenido del control SiteMapDataSource se mantiene en estado de vista. Mientras que 1.800 bytes puede no parecer mucho a entusiasmarse respecto, al usar un GridView con muchos campos y registros, el estado de vista se puede multiplicar fácilmente por un factor de 10 o más.

Estado de vista se puede deshabilitar en el nivel de página o control estableciendo el `EnableViewState` propiedad `False`, lo que reduce el tamaño del marcado representado. Desde el estado de vista para una Web control mantiene los datos enlazados a los datos de control Web en las devoluciones de datos, al deshabilitar el estado de vista de datos de un control Web se deben enlazar los datos en cada devolución de datos. En la versión ASP.NET 1.x, esto se encarga en las arraigadas, el desarrollador de páginas; con ASP.NET 2.0, sin embargo, los controles Web de datos se enlazará de nuevo a su control de origen de datos en cada devolución de datos si es necesario.

Para reducir el estado de vista vamos a establece el control Repeater `EnableViewState` propiedad `False`. Esto puede hacerse a través de la ventana de propiedades en el diseñador o mediante declaración en la vista del origen. Después de realizar este cambio debe ser marcado declarativo de Repeater similar:


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample10.aspx)]

Después de representa el este cambio, la página vista de tamaño de estado se ha reducido a tan sólo 52 bytes, un ahorro del 97% en la vista estado tamaño! En los tutoriales de esta serie se deberá deshabilitar el estado de vista de los datos de los controles Web de forma predeterminada con el fin de reducir el tamaño del marcado representado. En la mayoría de los ejemplos del `EnableViewState` propiedad se establecerá en `False` y sin necesidad de mención. La única vez hablaremos del estado de vista se encuentra en escenarios donde debe habilitarse en el orden de los datos de control Web para proporcionar su funcionalidad esperada.

## <a name="step-4-adding-breadcrumb-navigation"></a>Paso 4: Agregar la ruta de navegación

Para completar la página maestra, vamos a agregar un elemento de interfaz de usuario de navegación de ruta de navegación para cada página. El elemento breadcrumb muestra rápidamente a los usuarios en su posición actual dentro de la jerarquía de sitios. Agregar elementos breadcrumb en ASP.NET 2.0 es fácil, simplemente agregue un control SiteMapPath a la página; no se necesita ningún código.

Para nuestro sitio, agrega este control para el encabezado `<div>`:


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample11.aspx)]

El elemento breadcrumb muestra la página actual, el usuario está visitando en la jerarquía del mapa del sitio, así como a ese sitio "antecesores," del nodo del mapa hasta la raíz (inicio en nuestro mapa del sitio).


![El elemento Breadcrumb muestra la página actual y sus antecesores en el sitio de la jerarquía del mapa](master-pages-and-site-navigation-vb/_static/image28.png)

**Figura 12**: el elemento Breadcrumb muestra la página actual y sus antecesores en el sitio de la jerarquía del mapa


## <a name="step-5-adding-the-default-page-for-each-section"></a>Paso 5: Adición de la página predeterminada para cada sección

Los tutoriales en nuestro sitio están divididos en categorías diferentes Basic Reporting, Filtering, Custom Formatting, etc. con una carpeta para cada categoría y los tutoriales correspondientes como páginas ASP.NET dentro de esa carpeta. Además, cada carpeta contiene un `Default.aspx` página. Para esta página de forma predeterminada, vamos a mostrar todos los tutoriales de la sección actual. Es decir, para la `Default.aspx` en el `BasicReporting` carpeta, tendríamos vínculos a `SimpleDisplay.aspx`, `DeclarativeParams.aspx`, y `ProgrammaticParams.aspx`. En este caso, una vez más, podemos usar el `SiteMap` clase y un control Web para mostrar esta información según el mapa del sitio de datos definen en `Web.sitemap`.

Vamos a mostrar una lista desordenada con un control Repeater, pero esta vez que veremos el título y la descripción de los tutoriales. Dado que el marcado y código para ello se deben repetir para cada `Default.aspx` página, podemos concretar esta lógica de la interfaz de usuario en un [Control de usuario](https://msdn.microsoft.com/library/y6wb1a0e.aspx). Cree una carpeta en el sitio Web denominado `UserControls` y agregar a la que un nuevo elemento de tipo de Control de usuario Web denominado `SectionLevelTutorialListing.ascx`y agregue el siguiente marcado:


[![Agregar un nuevo Control de usuario Web a la carpeta UserControls](master-pages-and-site-navigation-vb/_static/image30.png)](master-pages-and-site-navigation-vb/_static/image29.png)

**Figura 13**: agregar un nuevo Control de usuario Web para la `UserControls` carpeta ([haga clic aquí para ver imagen en tamaño completo](master-pages-and-site-navigation-vb/_static/image31.png))


SectionLevelTutorialListing.ascx


[!code-aspx[Main](master-pages-and-site-navigation-vb/samples/sample12.aspx)]

SectionLevelTutorialListing.ascx.vb


[!code-vb[Main](master-pages-and-site-navigation-vb/samples/sample13.vb)]

En el ejemplo de Repeater anterior, enlazamos el `SiteMap` datos a Repeater mediante declaración; el `SectionLevelTutorialListing` Control de usuario, sin embargo, lo hace mediante programación. En el `Page_Load` controlador de eventos, se realiza una comprobación para asegurarse de que esta página s dirección URL se asigna a un nodo del mapa del sitio. Si este Control de usuario se usa en una página que no tiene la correspondiente `<siteMapNode>` entrada, `SiteMap.CurrentNode` devolverá `Nothing` y ningún dato se enlazará con el control Repeater. Suponiendo que tenemos un `CurrentNode`, enlazamos su `ChildNodes` colección para el control Repeater. Puesto que nuestro mapa del sitio se ha configurado tal que la `Default.aspx` página en cada sección es el nodo primario de todos los tutoriales dentro de esa sección, este código mostrará vínculos y descripciones de todos los tutoriales de la sección, como se muestra en la siguiente captura de pantalla.

Una vez que se ha creado este Repeater, abra el `Default.aspx` las páginas de cada una de las carpetas, vaya a la vista Diseño y basta con arrastrar el Control de usuario desde el Explorador de soluciones en la superficie de diseño donde desee que aparezca la lista de tutoriales.


[![El Control de usuario tiene que se han agregado a Default.aspx](master-pages-and-site-navigation-vb/_static/image33.png)](master-pages-and-site-navigation-vb/_static/image32.png)

**Figura 14**: Control de usuario se ha agregado a `Default.aspx` ([haga clic aquí para ver imagen en tamaño completo](master-pages-and-site-navigation-vb/_static/image34.png))


[![Se enumeran los tutoriales de Reporting básico](master-pages-and-site-navigation-vb/_static/image36.png)](master-pages-and-site-navigation-vb/_static/image35.png)

**Figura 15**: se enumeran los tutoriales de Reporting básico ([haga clic aquí para ver imagen en tamaño completo](master-pages-and-site-navigation-vb/_static/image37.png))


## <a name="summary"></a>Resumen

Con el mapa del sitio definido y la página principal completa, ahora tenemos un esquema de diseño y la navegación de página coherente para los tutoriales relacionados con los datos. Independientemente de cuántas páginas que agreguemos al sitio, actualizar la información de navegación de página para todo el sitio de diseño o un sitio es un proceso rápido y sencillo debido a la centralización de la información. En concreto, la información de diseño de página se define en la página maestra `Site.master` y el mapa del sitio en `Web.sitemap`. No necesitamos escribir *cualquier* para lograr este mecanismo de diseño y la navegación de página de todo el sitio de código y se conservan WYSIWYG completa compatibilidad con el diseñador en Visual Studio.

Después de haber terminado la capa de acceso a datos y la capa de lógica empresarial y tener una página con diseño y exploración del sitio definido, estamos preparados para empezar a explorar los modelos de informe comunes. En los siguientes tres tutoriales se examinarán las tareas básicas de informes muestra los datos recuperados de BLL en los controles GridView, DetailsView y FormView.

Feliz programación.

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [ASP.NET Master Pages Overview](https://msdn.microsoft.com/library/wtxbf3hh.aspx)
- [Páginas maestras en ASP.NET 2.0](http://odetocode.com/Articles/419.aspx)
- [ASP.NET 2.0 Design Templates](https://msdn.microsoft.com/asp.net/reference/design/templates/default.aspx)
- [Información general sobre la navegación de sitio de ASP.NET](https://msdn.microsoft.com/library/e468hxky.aspx)
- [Examen de ASP.NET 2.0 de la navegación del sitio](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [ASP.NET 2.0 Site Navigation Features](https://weblogs.asp.net/scottgu/archive/2005/11/20/431019.aspx)
- [Understanding ASP.NET View State](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/viewstate.asp)
- [Cómo: habilitar el seguimiento de una página ASP.NET](https://msdn.microsoft.com/library/94c55d08%28VS.80%29.aspx)
- [Controles de usuario ASP.NET](https://msdn.microsoft.com/library/y6wb1a0e.aspx)

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Los revisores para este tutorial fueron Liz Shulok, Dennis Patterson y Hilton Giesenow. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](creating-a-business-logic-layer-vb.md)
