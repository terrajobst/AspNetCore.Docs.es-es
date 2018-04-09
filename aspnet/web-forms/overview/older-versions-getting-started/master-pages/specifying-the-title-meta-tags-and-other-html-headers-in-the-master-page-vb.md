---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb
title: Especificar el título, etiquetas Meta y otros encabezados HTML en la página maestra (VB) | Documentos de Microsoft
author: rick-anderson
description: Examina las técnicas diferentes para definir ordenadas &lt;head&gt; elementos en la página principal de la página de contenido.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2008
ms.topic: article
ms.assetid: ea8196f5-039d-43ec-8447-8997ad4d3900
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb
msc.type: authoredcontent
ms.openlocfilehash: b8bf9d32eee3e35ffc84521f7f82f7beecc99a0c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb"></a>Especificar el título, etiquetas Meta y otros encabezados HTML en la página maestra (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_03_VB.zip) o [descarga de PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_03_VB.pdf)

> Examina las técnicas diferentes para definir ordenadas &lt;head&gt; elementos en la página principal de la página de contenido.


## <a name="introduction"></a>Introducción

Dispone de nuevas páginas maestras creadas en Visual Studio 2008, de forma predeterminada, dos controles ContentPlaceHolder: uno denominado `head`, situado en la `<head>` elemento; y otro denominado `ContentPlaceHolder1`, ubicado en el formulario Web Forms. El propósito de `ContentPlaceHolder1` consiste en definir una región en el formulario Web Forms que se pueden personalizar según una página a página. El `head` ContentPlaceHolder permite páginas Agregar contenido personalizado para el `<head>` sección. (Por supuesto, se pueden modificar o quitar estos dos ContentPlaceHolders y ContentPlaceHolder adicional puede agregarse a la página maestra. La página maestra, `Site.master`, actualmente tiene cuatro controles ContentPlaceHolder.)

El código HTML `<head>` elemento actúa como repositorio para obtener información sobre el documento de página web que no forma parte del propio documento. Esto incluye información como el título de la página web, metainformación utiliza los motores de búsqueda o rastreadores internos y vínculos a recursos externos, como las fuentes RSS, JavaScript y CSS archivos. Parte de esta información puede ser relativa a todas las páginas en el sitio Web. Por ejemplo, puede importar globalmente las mismas reglas CSS y archivos de JavaScript para cada página ASP.NET. Sin embargo, hay algunas partes de la `<head>` elemento que son específicos de la página. El título de página es un buen ejemplo.

En este tutorial se examina cómo definir global y específica de la página `<head>` marcado de sección en la página maestra y en las páginas de contenido.

## <a name="examining-the-master-pagesheadsection"></a>Examen de la página maestra`<head>`sección

El archivo de página maestra predeterminado creado por Visual Studio 2008 contiene el marcado siguiente en su `<head>` sección:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample1.aspx)]

Tenga en cuenta que la `<head>` elemento contiene un `runat="server"` atributo, que indica que es un control de servidor (en lugar de HTML estático). Todas las páginas ASP.NET se derivan de la [ `Page` clase](https://msdn.microsoft.com/library/system.web.ui.page.aspx), que se encuentra en la `System.Web.UI` espacio de nombres. Esta clase contiene un [ `Header` propiedad](https://msdn.microsoft.com/library/system.web.ui.page.header.aspx) que proporciona acceso a la página `<head>` región. Mediante el `Header` propiedad podemos establecer el título de la página ASP.NET o agregar marcas adicionales a la representada `<head>` sección. A continuación, es posible personalizar una página de contenido `<head>` elemento escribiendo un poco de código en la página `Page_Load` controlador de eventos. Examinaremos cómo establecer mediante programación el título de la página en el paso 1.

El marcado que se muestra en el `<head>` elemento anterior también incluye un control ContentPlaceHolder denominado `head`. Este control ContentPlaceHolder no es necesario, como páginas de contenido pueden agregar contenido personalizado para el `<head>` elemento mediante programación. Resulta útil, sin embargo, en situaciones donde debe agregar marcado estático en una página de contenido la `<head>` elemento como el marcado estático se puede agregar mediante declaración para el control de contenido correspondiente en lugar de mediante programación.

Además el `<title>` elemento y `head` del ContentPlaceHolder, la página principal `<head>` elemento debe tener ninguna `<head>`-nivel marcado que es común a todas las páginas. En nuestro sitio Web, todas las páginas utilizan las reglas CSS definidas en el `Styles.css` archivo. Por lo tanto, se han actualizado los `<head>` elemento en el [ *crear un diseño de todo el sitio con las páginas maestras* ](creating-a-site-wide-layout-using-master-pages-vb.md) tutorial para incluir correspondiente `<link>` elemento. Nuestro `Site.master` actual de la página maestra `<head>` marcado se muestra a continuación.


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample2.aspx)]

## <a name="step-1-setting-a-content-pages-title"></a>Paso 1: Establecer el título de la página de contenido

Título de la página web se especifica mediante el `<title>` elemento. Es importante establecer el título de cada página en un valor adecuado. Al visitar una página, el título se muestra en la barra de título del explorador. Además, al marcar una página, los exploradores utilizan el título de la página como nombre sugerido para el marcador. Además, muchos motores de búsqueda mostrar el título de la página para mostrar los resultados de la búsqueda.

> [!NOTE]
> De forma predeterminada, Visual Studio establece la `<title>` elemento en la página maestra a "Página" sin título". De forma similar, las páginas ASP.NET nuevo tienen sus `<title>` establecido en "Página sin título" demasiado. Dado que puede ser fácil olvidarse de establecer el título de la página en un valor apropiado, hay muchas páginas en Internet con el título "Página sin título". La búsqueda de Google para páginas web con este título devuelve aproximadamente 2,460,000 resultados. Ni siquiera Microsoft es susceptible a publicar páginas web con el título "Página sin título". En el momento de redactar este artículo, una búsqueda de Google había notificado 236 dichas páginas web en el dominio Microsoft.com.


Una página ASP.NET puede especificar el título de una de las maneras siguientes:

- Si coloca el valor directamente en el `<title>` elemento
- Mediante el `Title` de atributo en la `<%@ Page %>` (directiva)
- Establecer mediante programación la página `Title` propiedad con un código como `Page.Title="title"` o `Page.Header.Title="title"`.

Contenido de páginas no tienen un `<title>` elemento, tal y como se define en la página maestra. Por lo tanto, para establecer el título de la página de contenido, puede utilizar el `<%@ Page %>` la directiva `Title` de atributo o establecerlo mediante programación.

### <a name="setting-the-pages-title-declaratively"></a>Establecer el título de la página de forma declarativa

Título de la página de contenido se puede establecer mediante declaración a través del `Title` atributo de la [ `<%@ Page %>` directiva](https://msdn.microsoft.com/library/ydy4x04a.aspx). Esta propiedad se puede establecer modificando directamente el `<%@ Page %>` directiva o a través de la ventana Propiedades. Echemos un vistazo a ambos enfoques.

En la vista código fuente, busque la `<%@ Page %>` directiva, que se encuentra en la parte superior del marcado declarativo de la página. El `<%@ Page %>` directivas para `Default.aspx` sigue:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample3.aspx)]

El `<%@ Page %>` directiva especifica atributos específicos de página utilizados por el motor ASP.NET al analizar y compilar la página. Esto incluye su archivo de página maestra, la ubicación de su archivo de código y su título, entre otros datos.

De forma predeterminada, al crear una nueva página de contenido, Visual Studio establece la `Title` atributo a "Página" sin título". Cambio `Default.aspx`del `Title` de atributo de "Página sin título" a "Tutoriales de página maestra" y, a continuación, ver la página a través de un explorador. La figura 1 muestra la barra de título del explorador, que refleja el nuevo título de página.


![Ahora se muestra la barra de título del explorador &quot;tutoriales de página maestra&quot; en lugar de &quot;página sin título&quot;](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image1.png)

**Figura 01**: barra de título del explorador ahora muestra "Tutoriales de página maestra" en lugar de "Página sin título"


Título de la página también puede establecerse desde la ventana Propiedades. En la ventana Propiedades, seleccione el documento en la lista desplegable a carga el nivel de página de propiedades, que incluye el `Title` propiedad. La figura 2 muestra la ventana Propiedades después de `Title` se ha establecido en "Tutoriales de página maestra".


![Puede configurar el título de la ventana Propiedades, demasiado](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image2.png)

**Figura 02**: puede configurar el título de la ventana Propiedades, demasiado


### <a name="setting-the-pages-title-programmatically"></a>Establecer el título de la página mediante programación

La página maestra `<head runat="server">` marcado se traduce en un [ `HtmlHead` clase](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlhead.aspx) instancia cuando la página se representa por el motor ASP.NET. El `HtmlHead` clase tiene un [ `Title` propiedad](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlhead.title.aspx) cuyo valor se refleja en el representado `<title>` elemento. Esta propiedad es accesible desde la clase de código subyacente de la página ASP.NET a través de `Page.Header.Title`; este mismo propiedad también puede obtenerse a través de `Page.Title`.

Para establecer el título de la página mediante programación de procedimientos, navegue a la `About.aspx` código subyacente de la página de clase y crear un controlador de eventos de la página `Load` eventos. A continuación, establezca el título de la página en "tutoriales de página maestra:: sobre:: *fecha*", donde *fecha* es la fecha actual. Después de agregar este código su `Page_Load` controlador de eventos debe ser similar al siguiente:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample4.vb)]

La figura 3 muestra la barra de título del explorador al visitar la `About.aspx` página.


![Título de la página se establece mediante programación e incluye la fecha actual](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image3.png)

**Figura 03**: título de la página es establecer mediante programación e incluye la fecha actual


## <a name="step-2-automatically-assigning-a-page-title"></a>Paso 2: Asignar automáticamente un título de página

Como vimos en el paso 1, se puede establecer el título de la página mediante declaración o mediante programación. Si se olvida de cambiar explícitamente el título a algo más descriptivo, sin embargo, la página tendrá el título predeterminado, "Página sin título". Idealmente, título de la página se ajustará automáticamente para que podamos en caso de que se no se especifica explícitamente su valor. Por ejemplo, si en tiempo de ejecución título de la página "Página sin título", podría desear tener el título que se actualiza automáticamente para coincidir con el nombre de archivo de la página ASP.NET. La ventaja es que con un poco de trabajo por adelantado que es posible tener el título asignado automáticamente.

Todas las páginas web ASP.NET que se derivan de la `Page` clase en el espacio de nombres System.Web.UI. El `Page` clase define la funcionalidad mínima necesaria para una página ASP.NET y expone las propiedades esenciales como `IsPostBack`, `IsValid`, `Request`, y `Response`, entre otros muchos. A menudo, todas las páginas en una aplicación web requiere características adicionales o funcionalidad. Una manera común de ofrecer esto consiste en crear una clase de página base personalizada. Una clase de página base personalizada es crear una clase que deriva de la `Page` clase e incluye una funcionalidad adicional. Una vez creada esta clase base, puede tener las páginas ASP.NET derivan de él (en lugar de la `Page` clase), lo que ofrece la funcionalidad extendida para las páginas ASP.NET.

En este paso se creará una página de base que establece automáticamente el título de la página al nombre de archivo de la página ASP.NET si no se ha establecido en caso contrario de explícitamente el título. Paso 3 se examina establecer el título de la página basado en el mapa del sitio.

> [!NOTE]
> Un examen exhaustivo de creación y uso de las clases de la página base personalizada queda fuera del ámbito de esta serie de tutoriales. Para obtener más información, lea [con una clase de Base personalizada para las clases de código subyacente de sus páginas ASP.NET](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx).


### <a name="creating-the-base-page-class"></a>Crear la clase de página Base

La primera tarea consiste en crear una clase de página base, que es una clase que extiende la `Page` clase. Empiece agregando un `App_Code` carpeta al proyecto con el botón secundario en el nombre del proyecto en el Explorador de soluciones, elija Agregar carpeta ASP.NET y, a continuación, seleccione `App_Code`. A continuación, haga doble clic en el `App_Code` carpeta y agregue una nueva clase denominada `BasePage.vb`. La figura 4 muestra el Explorador de soluciones después de la `App_Code` carpeta y `BasePage.vb` se han agregado la clase.


![Agregar una carpeta App_Code y una clase denominada BasePage](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image4.png)

**Figura 04**: agregar un `App_Code` carpeta y una clase denominada `BasePage`


> [!NOTE]
> Visual Studio admite dos modos de administración de proyectos: proyectos de sitios Web y proyectos de aplicación Web. El `App_Code` carpeta está diseñada para usarse con el modelo de proyecto de sitio Web. Si está utilizando el modelo de proyecto de aplicación Web, coloque el `BasePage.vb` clase en una carpeta denominada algo distinto de `App_Code`, como `Classes`. Para obtener más información acerca de este tema, consulte [migrando un proyecto de sitio Web a un proyecto de aplicación Web](http://webproject.scottgu.com/VisualBasic/Migration2/Migration2.aspx).


Puesto que la página base personalizada actúa como la clase base para las clases de código subyacente de páginas ASP.NET, debe ampliar la `Page` clase.


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample5.vb)]

Cuando se solicita una página ASP.NET pasa a través de una serie de fases, hasta la preinstalación en la página solicitada que se va a representar en HTML. Podemos aprovechar una fase invalidando el `Page` la clase `OnEvent` método. Para nuestra base de la página Vamos a establecer automáticamente el título si no se ha especificado explícitamente por el `LoadComplete` fase (que, como habrá adivinado, se produce después de la `Load` fase).

Para ello, invalide el `OnLoadComplete` (método) y escriba el código siguiente:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample6.vb)]

El `OnLoadComplete` método comienza mediante la determinación de si la `Title` propiedad no se estableció explícitamente. Si el `Title` propiedad es `Nothing`, una cadena vacía, o tiene el valor "Página sin título", se le asigna el nombre de archivo de la página ASP.NET solicitada. La ruta de acceso física a la página ASP.NET solicitada - `C:\MySites\Tutorial03\Login.aspx`, por ejemplo: es accesible a través de la `Request.PhysicalPath` propiedad. El `Path.GetFileNameWithoutExtension` método se usa para extraer solo la parte del nombre de archivo y, a continuación, se asigna este nombre de archivo a la `Page.Title` propiedad.

> [!NOTE]
> Se puede invitar a mejorar esta lógica para mejorar el formato del título. Por ejemplo, si el nombre de archivo de la página es `Company-Products.aspx`, el código anterior generará el título "Productos de la empresa", pero lo ideal es que se reemplazaría el guión con un espacio, como en "Productos de la empresa". Además, considere la posibilidad de agregar un espacio cada vez que hay un cambio de mayúsculas. Es decir, considere la posibilidad de agregar el código que transforma el nombre de archivo `OurBusinessHours.aspx` a un título de "Nuestro horario comercial".


### <a name="having-the-content-pages-inherit-the-base-page-class"></a>Con las páginas de contenido heredan la clase de página Base

Ahora necesitamos actualizar las páginas ASP.NET en nuestro sitio de la página base personalizada se deriva (`BasePage`) en lugar de la `Page` clase. Para lograr esto vaya a cada clase de código subyacente y cambie la declaración de clase de:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample7.vb)]

A:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample8.vb)]

Una vez hecho esto, visite el sitio a través de un explorador. Si visita una página cuyo título se establece explícitamente, como `Default.aspx` o `About.aspx`, se utiliza el título especificado explícitamente. Si, sin embargo, se visita una página cuyo título no ha cambiado el valor predeterminado ("página sin título"), la clase de página base establece el título al nombre de archivo de la página.

Figura 5 se muestra la `MultipleContentPlaceHolders.aspx` página cuando se ve mediante un explorador. Tenga en cuenta que el título es precisamente nombre de archivo de la página (menos la extensión), "MultipleContentPlaceHolders".


[![Si un título no es especifica explícitamente, el nombre de archivo de la página es utiliza automáticamente](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image6.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image5.png)

**Figura 05**: si un título no es especifica explícitamente, el nombre de archivo de la página es utiliza automáticamente ([haga clic aquí para ver la imagen a tamaño completo](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image7.png))


## <a name="step-3-basing-the-page-title-on-the-site-map"></a>Paso 3: Basar el título de página en el mapa del sitio

ASP.NET proporciona un marco de trabajo de asignación de sitio sólida que permite a los desarrolladores de página Definir un mapa del sitio jerárquica en un recurso externo (por ejemplo, una tabla de base de datos o archivo XML) junto con los controles Web para mostrar información sobre la asignación de sitio (por ejemplo, SiteMapPath, Menús y controles de vista de árbol).

La estructura de asignación de sitio también puede tener acceso mediante programación de la clase de código subyacente de una página ASP.NET. De esta manera podemos establecer automáticamente título de una página en el título de su nodo correspondiente en el mapa del sitio. Vamos a mejorar la `BasePage` clase creada en el paso 2 para que ofrece esta funcionalidad. Pero primero se debe crear un mapa del sitio para el sitio.

> [!NOTE]
> Este tutorial se da por supuesto que el lector ya está familiarizado con ASP. Características de mapa del sitio de red. Para obtener más información sobre cómo utilizar el mapa del sitio, consulte mi serie de artículos de varias partes, [examen de ASP. Navegación por el sitio de red](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx).


### <a name="creating-the-site-map"></a>Crear el mapa del sitio

El sistema de sitio se compila sobre la [modelo de proveedor](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), que separa el mapa del sitio API de la lógica que se serializa la información de asignación de sitio entre la memoria y un almacén persistente. .NET Framework se suministra con la [ `XmlSiteMapProvider` clase](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx), que es el proveedor de mapas de sitio predeterminado. Como su nombre implica, `XmlSiteMapProvider` utiliza un archivo XML como su almacén de asignación de sitio. Vamos a usar este proveedor para definir el mapa del sitio.

Empiece por crear un archivo de asignación de sitio en la carpeta de raíz del sitio Web denominada `Web.sitemap`. Para ello, haga doble clic en el nombre del sitio Web en el Explorador de soluciones, elija Agregar nuevo elemento y seleccione la plantilla de mapa del sitio. Asegúrese de que el archivo se denomina `Web.sitemap` y haga clic en Agregar.


[![Agregue un archivo denominado Web.sitemap a carpeta de raíz del sitio Web](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image9.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image8.png)

**Figura 06**: agregue un archivo denominado `Web.sitemap` a carpeta de raíz del sitio Web ([haga clic aquí para ver la imagen a tamaño completo](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image10.png))


Agregue el siguiente código XML para el `Web.sitemap` archivo:


[!code-xml[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample9.xml)]

Este código XML define la estructura jerárquica que se muestra en la figura 7.


![El mapa del sitio es actualmente compuesto de tres nodos mapa del sitio](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image11.png)

**Figura 07**: el mapa del sitio es actualmente compuesto de tres nodos mapa del sitio


En tutoriales futuros, se actualizará la estructura de mapa del sitio cuando se agregan nuevos ejemplos.

### <a name="updating-the-master-page-to-include-navigation-web-controls"></a>Actualizar la página maestra para incluir controles de navegación en Web

Ahora que tenemos un mapa del sitio definido, vamos a actualizar la página maestra para incluir controles de navegación Web. En concreto, vamos a agregar un control ListView a la columna izquierda en la sección de lecciones que presenta una lista desordenada con un elemento de lista para cada nodo que se definen en el mapa del sitio.

> [!NOTE]
> ListView (control) es nuevo en ASP.NET versión 3.5. Si está utilizando una versión anterior de ASP.NET, utilice en su lugar el control de repetidor. Para obtener más información sobre el control ListView, consulte [utilizando ASP.NET 3.5 controles ListView y DataPager](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx).


Iniciar quitando el marcado existente de la lista sin ordenar de la sección lecciones. A continuación, arrastre un control ListView desde el cuadro de herramientas y colóquelo debajo de las lecciones encabezado. La vista de lista se encuentra en la sección de datos del cuadro de herramientas, junto con los demás controles de vista: la GridView, DetailsView y FormView. Establecer la vista de lista `ID` propiedad `LessonsList`.

Desde el Asistente para configuración de orígenes de datos que elija para enlazar el control ListView a un nuevo control SiteMapDataSource denominado `LessonsDataSource`. El control SiteMapDataSource devuelve la estructura jerárquica del sistema de mapa del sitio.


[![Enlazar un Control SiteMapDataSource al Control LessonsList ListView](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image13.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image12.png)

**Figura 08**: enlazar un SiteMapDataSource Control al ListView Control LessonsList ([haga clic aquí para ver la imagen a tamaño completo](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image14.png))


Después de crear el control SiteMapDataSource, es necesario definir plantillas de ListView para que presenta una lista desordenada con un elemento de lista para cada nodo devuelto por el control SiteMapDataSource. Esto puede realizarse mediante el siguiente marcado de plantilla:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample10.aspx)]

El `LayoutTemplate` genera el marcado para una lista desordenada (`<ul>...</ul>`) mientras el `ItemTemplate` procesa cada elemento devuelto por el SiteMapDataSource como un elemento de lista (`<li>`) que contenga un vínculo a la lección determinada.

Después de configurar plantillas de ListView, visite el sitio Web. Como se muestra en la figura 9, la sección lecciones contiene un solo elemento con viñetas, principal. ¿Dónde están About y el uso de lecciones ContentPlaceHolder varios controles? SiteMapDataSource está diseñado para devolver un conjunto jerárquico de datos, pero el control ListView solo puede mostrar un solo nivel de la jerarquía. Por lo tanto, se muestra sólo el primer nivel de nodos de mapa del sitio devolviendo SiteMapDataSource.


[![La sección lecciones contiene un único elemento de lista](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image16.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image15.png)

**Figura 09**: la sección lecciones contiene un único elemento de lista ([haga clic aquí para ver la imagen a tamaño completo](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image17.png))


Para mostrar varios niveles se puede anidar varias ListView dentro de la `ItemTemplate`. Esta técnica se examinó en el [ *páginas maestras y navegación del sitio* tutorial](../../data-access/introduction/master-pages-and-site-navigation-vb.md) de mi [trabajar con la serie de tutoriales de datos](../../data-access/index.md). Sin embargo, para esta serie de tutoriales nuestro mapa del sitio contiene solo un dos niveles: principal (el nivel superior); y cada lección como un elemento secundario del principal. En lugar de diseñar un control ListView anidado, en su lugar se podemos indicar SiteMapDataSource no devuelva ningún valor del nodo de inicio estableciendo su [ `ShowStartingNode` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx) a `False`. El efecto neto es que se inicia el SiteMapDataSource devolviendo el segundo nivel de nodos del mapa del sitio.

Con este cambio, la vista de lista muestra los elementos de viñeta para About y utilizando varios controles ContentPlaceHolder lecciones, pero omite un elemento de viñeta de inicio de. Para solucionar esto, podemos agregar explícitamente un elemento de viñeta de página de inicio de la `LayoutTemplate`:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample11.aspx)]

Configurando el SiteMapDataSource para omitir el nodo de inicio y agregar explícitamente un elemento de viñeta de inicio, la sección lecciones ahora muestra el resultado previsto.


[![La sección de lecciones contiene un elemento de viñeta de principal y cada nodo secundario](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image19.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image18.png)

**Figura 10**: la sección de lecciones contiene un elemento de viñeta de principal y todos los nodos secundarios ([haga clic aquí para ver la imagen a tamaño completo](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image20.png))


### <a name="setting-the-title-based-on-the-site-map"></a>Establecer el título basado en el mapa del sitio

Con el mapa del sitio en su lugar, podemos actualizar nuestro `BasePage` clase que se usa el título especificado en el mapa del sitio. Como hicimos en el paso 2, solamente queremos usar título del nodo mapa del sitio si el título de la página no se ha establecido explícitamente por el desarrollador de la página. Si la página solicitada no tiene establecido explícitamente un título de página y no se encuentra en el mapa del sitio, a continuación, se deberá recurrir al uso filename de la página solicitada (menos la extensión), como hicimos en el paso 2. La figura 11 ilustra este proceso de toma de decisiones.


![En ausencia de una manera explícita Establecer título de página, se utiliza el título del nodo correspondiente de asignación de sitio](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image21.png)

**Figura 11**: en la ausencia de una manera explícita Establecer título de página, se utiliza el título del nodo correspondiente de asignación de sitio


Actualización de la `BasePage` la clase `OnLoadComplete` método para incluir el código siguiente:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample12.vb)]

Al igual que antes, el `OnLoadComplete` método comienza mediante la determinación de si el título de la página se ha establecido explícitamente. Si `Page.Title` es `Nothing`, una cadena vacía, o se asigna el valor "Página sin título", a continuación, el código asigna automáticamente un valor para `Page.Title`.

Para determinar el título que se utilizará, inicia el código haciendo referencia a la [ `SiteMap` clase](https://msdn.microsoft.com/library/system.web.sitemap.aspx)del [ `CurrentNode` propiedad](https://msdn.microsoft.com/library/system.web.sitemap.currentnode.aspx). `CurrentNode` Devuelve el [ `SiteMapNode` ](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) instancia en el mapa del sitio que corresponde a la página solicitada actualmente. Suponiendo que la página solicitada actualmente se encuentra en el mapa del sitio, el `SiteMapNode`del `Title` propiedad está asignada al título de la página. Si la página actualmente solicitada no está en el mapa del sitio, `CurrentNode` devuelve `Nothing` y nombre de archivo de la página solicitada se utiliza como título (tal como se hizo en el paso 2).

La figura 12 muestra la `MultipleContentPlaceHolders.aspx` página cuando se ve mediante un explorador. Porque no se establece explícitamente el título de la página, se utiliza en su lugar, el título de su correspondiente sitio del nodo del mapa.


![Título de la página MultipleContentPlaceHolders.aspx se extrae del mapa del sitio](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/_static/image22.png)

**Figura 12**: se extrae título de la página de MultipleContentPlaceHolders.aspx el del mapa del sitio


## <a name="step-4-adding-other-page-specific-markup-to-theheadsection"></a>Paso 4: Agregar otro marcado específica de la página a la`<head>`sección

Los pasos 1, 2 y 3 examinando personalizar el `<title>` elemento según una página a página. Además `<title>`, `<head>` sección puede contener `<meta>` elementos y `<link>` elementos. Como se indicó anteriormente en este tutorial, `Site.master`del `<head>` sección incluye una `<link>` elemento `Styles.css`. Dado que esto `<link>` elemento se define en la página maestra, se incluye en la `<head>` sección para todas las páginas de contenido. Pero ¿qué hacemos acerca de cómo agregar `<meta>` y `<link>` elementos según una página a página?

La manera más fácil de agregar contenido específico de la página a la `<head>` sección consiste en crear un control ContentPlaceHolder en la página maestra. Ya tenemos tal un ContentPlaceHolder (denominado `head`). Por lo tanto, para agregar personalizada `<head>` marcado, creará un control en la página de contenido y situar ahí el marcado.

Para ilustrar el hecho de agregar personalizada `<head>` marcado a una página, vamos a incluir un `<meta>` description (elemento) en nuestro conjunto de páginas de contenido actual. El `<meta>` description (elemento) proporciona una breve descripción de la página web, la mayoría de los motores de búsqueda incorporan esta información en alguna forma para mostrar los resultados de la búsqueda.

Un `<meta>` description (elemento) tiene el formato siguiente:


[!code-html[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample13.html)]

Para agregar este marcado a una página de contenido, agregue el texto anterior para el control de contenido que se asigna a la página maestra `head` ContentPlaceHolder. Por ejemplo, para definir un `<meta>` description (elemento) para `Default.aspx`, agregue el siguiente marcado:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample14.aspx)]

Dado que el `head` ContentPlaceHolder no está dentro del cuerpo de la página HTML, el marcado que se agrega al control de contenido no se muestra en la vista Diseño. Para ver el `<meta>` Descripción elemento visita `Default.aspx` a través de un explorador. Después de haberse cargada la página, ver el código fuente y tenga en cuenta que la `<head>` sección incluye el marcado especificado en el control de contenido.

Tómese un momento para agregar `<meta>` elementos descripción `About.aspx`, `MultipleContentPlaceHolders.aspx`, y `Login.aspx`.

### <a name="programmatically-adding-markup-to-theheadregion"></a>Agregar mediante programación el marcado para la`<head>`región

El `head` ContentPlaceHolder nos permite agregar mediante declaración marcado personalizada a la página maestra `<head>` región. Marcado personalizado también puede agregarse mediante programación. Recuerde que el `Page` la clase `Header` propiedad devuelve el `HtmlHead` instancia definida en la página maestra (el `<head runat="server">`).

Posibilidad de agregar mediante programación el contenido a la `<head>` región es útil cuando el contenido para agregar es dinámico. Quizás se basa en el usuario visita la página; quizás se extrae de una base de datos. Independientemente del motivo, puede agregar contenido a la `HtmlHead` agregando controles a su `Controls` colección así:


[!code-vb[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb/samples/sample15.vb)]

El código anterior agrega la `<meta>` elemento palabras clave para el `<head>` región, que proporciona una lista delimitada por comas de las palabras clave que describen la página. Tenga en cuenta que para agregar un `<meta>` etiqueta se crea un [ `HtmlMeta` ](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlmeta.aspx) de la instancia, establecer su `Name` y `Content` propiedades y, a continuación, agréguelo a la `Header`del `Controls` colección. De forma similar, para agregar mediante programación un `<link>` elemento, crear una [ `HtmlLink` ](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmllink.aspx) objeto, establecer sus propiedades y, a continuación, agregarlo a la `Header`del `Controls` colección.

> [!NOTE]
> Para agregar un marcado arbitrario, cree un [ `LiteralControl` ](https://msdn.microsoft.com/library/system.web.ui.literalcontrol.aspx) de la instancia, establecer su `Text` propiedad y, a continuación, agréguelo a la `Header`del `Controls` colección.


## <a name="summary"></a>Resumen

En este tutorial se busca en una variedad de maneras de agregar `<head>` marcado región según una página a página. Una página maestra debe incluir un `HtmlHead` instancia (`<head runat="server">`) con un ContentPlaceHolder. El `HtmlHead` instancia permite que las páginas de contenido para obtener acceso mediante programación el `<head>` región y establecer mediante declaración y mediante programación la página del título; el control ContentPlaceHolder permite marcado personalizada para agregarse a la `<head>` sección de manera declarativa mediante un control de contenido.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Establecer el título de la página de forma dinámica en ASP.NET](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [Examen de ASP. Navegación por el sitio de red](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Cómo usar etiquetas HTML Meta](http://searchenginewatch.com/showPage.html?page=2167931)
- [Páginas maestras en ASP.NET](http://www.odetocode.com/articles/419.aspx)
- [Mediante ASP.NET 3.5 controles ListView y DataPager](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [Usar una clase Base personalizada para las clases de código subyacente de las páginas ASP.NET](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)

### <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de varios libros sobre ASP/ASP.NET y fundador de 4GuysFromRolla.com, ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [ *SAM enseñar a usted mismo ASP.NET 3.5 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) o a través de su blog en [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Los revisores iniciales para este tutorial eran Zack Jones y Suchi Banerjee. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Anterior](multiple-contentplaceholders-and-default-content-vb.md)
> [Siguiente](urls-in-master-pages-vb.md)
