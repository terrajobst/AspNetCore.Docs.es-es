---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs
title: Especificar el título, etiquetas Meta y otros encabezados HTML en la página maestra (C#) | Microsoft Docs
author: rick-anderson
description: Examina las distintas técnicas para definir ordenadas &lt;head&gt; elementos en la página maestra desde la página de contenido.
ms.author: aspnetcontent
ms.date: 05/21/2008
ms.assetid: 0aa1c84f-c9e2-4699-b009-0e28643ecbc6
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 773d8aee55a3e685c9759f9a9e0b571564f8cd31
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841016"
---
<a name="specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-c"></a>Especificar el título, etiquetas Meta y otros encabezados HTML en la página maestra (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_03_CS.zip) o [descargar PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_03_CS.pdf)

> Examina las distintas técnicas para definir ordenadas &lt;head&gt; elementos en la página maestra desde la página de contenido.


## <a name="introduction"></a>Introducción

Tienen nuevas páginas maestras creadas en Visual Studio 2008, de forma predeterminada, dos controles ContentPlaceHolder: uno denominado head y se encuentra en la `<head>` ; y un elemento denominado `ContentPlaceHolder1`, situado dentro del formulario Web Forms. El propósito de `ContentPlaceHolder1` consiste en definir una región en el formulario Web Forms que se pueden personalizar según una página por página. El `head` ContentPlaceHolder permite páginas Agregar contenido personalizado para el `<head>` sección. (Por supuesto, puede ser modificados o quitados estos dos ContentPlaceHolders y ContentPlaceHolder adicional se puede agregar a la página maestra. Esta página principal, `Site.master`, actualmente tiene cuatro controles ContentPlaceHolder.)

El código HTML `<head>` elemento actúa como repositorio para obtener información sobre el documento de página web que no forma parte del propio documento. Esto incluye información como el título de la página web, información de metadatos utilizado por los motores de búsqueda o rastreadores internos y vínculos a recursos externos, como las fuentes RSS, JavaScript y CSS archivos. Parte de esta información puede ser relativa a todas las páginas en el sitio Web. Por ejemplo, es posible que desea importar globalmente las mismas reglas CSS y archivos de JavaScript para todas las páginas ASP.NET. Sin embargo, hay algunas partes de la `<head>` elemento que son específicos de la página. El título de página es un buen ejemplo.

En este tutorial se examina cómo definir globales y específicas de la página `<head>` marcado de la sección en la página maestra y en las páginas de contenido.

## <a name="examining-the-master-pagesheadsection"></a>Examen de la página maestra`<head>`sección

El archivo de página maestra predeterminada creado por Visual Studio 2008 contiene el marcado siguiente en su `<head>` sección:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample1.aspx)]

Tenga en cuenta que el `<head>` elemento contiene un `runat="server"` atributo, que indica que es un control de servidor (en lugar de HTML estático). Todas las páginas ASP.NET se derivan de la [ `Page` clase](https://msdn.microsoft.com/library/system.web.ui.page.aspx), que se encuentra en la `System.Web.UI` espacio de nombres. Esta clase contiene un `Header` propiedad que proporciona acceso a la página `<head>` región. Mediante el [ `Header` propiedad](https://msdn.microsoft.com/library/system.web.ui.page.header.aspx) podemos establecer el título de la página ASP.NET o agregar marcado adicional a la representada `<head>` sección. A continuación, es posible personalizar una página de contenido `<head>` elemento escribiendo un fragmento de código en la página `Page_Load` controlador de eventos. Examinamos cómo establecer mediante programación el título de la página en el paso 1.

El marcado que se muestra en el `<head>` elemento anterior también incluye un control ContentPlaceHolder denominado head. Este control ContentPlaceHolder no es necesario, como las páginas de contenido pueden agregar contenidos personalizados a la `<head>` elemento mediante programación. Es útil, sin embargo, en situaciones donde debe agregar el marcado estático a una página de contenido la `<head>` elemento como el marcado estático se puede agregar mediante declaración para el control de contenido correspondiente en lugar de mediante programación.

Además el `<title>` del elemento y head ContentPlaceHolder, la página principal `<head>` elemento debe tener ninguna `<head>`-nivel marcado que es común a todas las páginas. En nuestro sitio Web, todas las páginas utilizan las reglas CSS definidas en el `Styles.css` archivo. Por lo tanto, hemos actualizado la `<head>` elemento en el [ *crear un diseño de todo el sitio con páginas maestras* ](creating-a-site-wide-layout-using-master-pages-cs.md) tutorial para incluir la correspondiente `<link>` elemento. Nuestro `Site.master` actual de la página maestra `<head>` marcado se muestra a continuación.


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample2.aspx)]

## <a name="step-1-setting-a-content-pages-title"></a>Paso 1: Establecer el título de la página de contenido

Título de la página web se especifica a través de la `<title>` elemento. Es importante establecer el título de cada página en un valor adecuado. Cuando se visita una página, el título se muestra en la barra de título del explorador. Además, al marcar una página, los exploradores utilizan el título de la página como el nombre sugerido para el marcador. Además, muchos motores de búsqueda Mostrar título de la página al mostrar los resultados de búsqueda.

> [!NOTE]
> De forma predeterminada, Visual Studio establece la `<title>` elemento en la página maestra para "Página sin título". De forma similar, las páginas ASP.NET tienen sus `<title>` establecido en "Página sin título" demasiado. Dado que puede ser fácil olvidarse de establecer el título de la página en un valor adecuado, hay muchas páginas en Internet con el título "Página sin título". La búsqueda de Google para páginas web con este título devuelve aproximadamente 2,460,000 resultados. Incluso Microsoft es susceptible a publicar páginas web con el título "Página sin título". En el momento de redactar este artículo, una búsqueda de Google había notificado 236 dichas páginas web en el dominio Microsoft.com.


Una página ASP.NET puede especificar el título de una de las maneras siguientes:

- Al colocar el valor directamente dentro de la `<title>` elemento
- Mediante el `Title` atributo el `<%@ Page %>` directiva
- Establecer mediante programación la página `Title` propiedad mediante código similar al siguiente `Page.Title="title"` o `Page.Header.Title="title"`.

Contenido de páginas no tienen un `<title>` elemento, tal y como se define en la página maestra. Por lo tanto, para establecer el título de la página de contenido, puede usar el `<%@ Page %>` la directiva `Title` de atributo o establecerlo mediante programación.

### <a name="setting-the-pages-title-declaratively"></a>Establecer título de la página de forma declarativa

Título de la página de contenido se puede establecer mediante declaración a través del `Title` atributo de la [ `<%@ Page %>` directiva](https://msdn.microsoft.com/library/ydy4x04a.aspx). Esta propiedad puede establecerse si se modifica directamente la `<%@ Page %>` la directiva o a través de la ventana Propiedades. Echemos un vistazo a ambos enfoques.

En la vista del origen, localice el `<%@ Page %>` directiva, que es la parte superior de marcado declarativo de la página. El `<%@ Page %>` directiva `Default.aspx` sigue:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample3.aspx)]

El `<%@ Page %>` directiva especifica los atributos específicos de página utilizados por el motor ASP.NET al analizar y compilar la página. Esto incluye su archivo de página maestra, la ubicación de su archivo de código y su título, entre otros datos.

De forma predeterminada, al crear una nueva página de contenido Visual Studio establece la `Title` atributo a una página sin título. Cambio `Default.aspx`del `Title` de atributo de "Página sin título" a "Tutoriales de página maestra" y, a continuación, ver la página mediante un explorador. Figura 1 muestra la barra de título del explorador, lo que refleja el nuevo título de página.


![Ahora se muestra la barra de título del explorador &quot;tutoriales de página maestra&quot; en lugar de &quot;página sin título&quot;](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image1.png)

**Figura 01**: barra de título del explorador ahora muestra "Tutoriales de página maestra" en lugar de "Página sin título"


También se puede establecer el título de la página desde la ventana Propiedades. En la ventana Propiedades, seleccione el documento en la lista desplegable a carga el nivel de página de propiedades, que incluye el `Title` propiedad. La figura 2 muestra la ventana Propiedades después `Title` se ha establecido en "Tutoriales de página maestra".


![Puede configurar el título de la ventana Propiedades, también](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image2.png)

**Figura 02**: puede configurar el título de la ventana Propiedades, también


### <a name="setting-the-pages-title-programmatically"></a>Establecer título de la página mediante programación

La página maestra `<head runat="server">` marcado se traduce en un [ `HtmlHead` clase](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlhead.aspx) instancia cuando la página se representa mediante el motor de ASP.NET. El `HtmlHead` clase tiene un [ `Title` propiedad](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlhead.title.aspx) cuyo valor se refleja en el texto representado `<title>` elemento. Esta propiedad es accesible desde la clase de código subyacente de una página ASP.NET a través de `Page.Header.Title`; esta misma propiedad también se puede acceder mediante `Page.Title`.

Para practicar Establecer título de la página mediante programación, vaya a la `About.aspx` código subyacente de la página de clase y crear un controlador de eventos de la página `Load` eventos. A continuación, establezca su título "tutoriales de página maestra:: sobre:: *fecha*", donde *fecha* es la fecha actual. Después de agregar este código su `Page_Load` controlador de eventos debe ser similar al siguiente:


[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample4.cs)]

Figura 3 muestra la barra de título del explorador cuando se visita el `About.aspx` página.


![Título de la página se establece mediante programación e incluye la fecha actual](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image3.png)

**Figura 03**: título de la página se establece mediante programación e incluye la fecha actual


## <a name="step-2-automatically-assigning-a-page-title"></a>Paso 2: Asignar automáticamente un título de página

Como hemos visto en el paso 1, se puede establecer el título de la página mediante declaración o mediante programación. Si se olvida de cambiar explícitamente el título a algo más descriptivo, sin embargo, la página tendrá el título predeterminado, "Página sin título". Idealmente, título de la página se establecería automáticamente para nosotros en caso de que se no se especifica explícitamente su valor. Por ejemplo, si en tiempo de ejecución título de la página "Página sin título", que podría desear tienen el título que se actualiza automáticamente para coincidir con el nombre de archivo de la página ASP.NET. La buena noticia es con un poco de trabajo inicial es posible tener el título asignado automáticamente.

Todas las páginas web ASP.NET que se derivan de la `Page` clase en el `System.Web.UI` espacio de nombres. El `Page` clase define la funcionalidad mínima necesaria para una página ASP.NET y expone propiedades esenciales como `IsPostBack`, `IsValid`, `Request`, y `Response`, entre muchas otras. A menudo, todas las páginas en una aplicación web requiere características adicionales o funcionalidad. Una manera común de proporcionar esto es crear una clase de página base personalizada. Una clase de página base personalizada es crear una clase que derive de la `Page` clase e incluye funcionalidad adicional. Una vez creada esta clase base, puede tener las páginas ASP.NET derivan de ella (en lugar de `Page` clase), lo que ofrece la funcionalidad ampliada a las páginas ASP.NET.

En este paso se creará una página de base que establece automáticamente el título de la página al nombre de archivo de la página ASP.NET si el título no si no se estableció explícitamente. Paso 3 se examina Establecer título de la página según el mapa del sitio.

> [!NOTE]
> Un examen exhaustivo de creación y uso de las clases de página base personalizada está fuera del ámbito de esta serie de tutoriales. Para obtener más información, lea [mediante una clase de Base personalizada para las clases de código subyacente de sus páginas de ASP.NET](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx).


### <a name="creating-the-base-page-class"></a>Creación de la clase Base

Nuestra primera tarea consiste en crear una clase de página base, que es una clase que extiende el `Page` clase. Empiece agregando un `App_Code` carpeta al proyecto, con el botón secundario en el nombre del proyecto en el Explorador de soluciones, elija Agregar carpeta ASP.NET y, a continuación, seleccione `App_Code`. A continuación, haga doble clic en el `App_Code` carpeta y agregue una nueva clase denominada `BasePage.cs`. Figura 4 se muestra el Explorador de soluciones después de la `App_Code` carpeta y `BasePage.cs` clase se han agregado.


![Agregar una carpeta App_Code y una clase denominada BasePage](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image4.png)

**Figura 04**: agregar un `App_Code` carpeta y una clase denominada `BasePage`


> [!NOTE]
> Visual Studio admite dos modos de administración de proyectos: proyectos de sitios Web y proyectos de aplicación Web. El `App_Code` carpeta está diseñada para usarse con el modelo de proyecto de sitio Web. Si usa el modelo de proyecto de aplicación Web, coloque el `BasePage.cs` clase en una carpeta denominada algo distinto `App_Code`, tales como `Classes`. Para obtener más información sobre este tema, consulte [migrar un proyecto de sitio Web a un proyecto de aplicación Web](http://webproject.scottgu.com/CSharp/Migration2/Migration2.aspx).


Puesto que la página base personalizada actúa como clase base para las clases de código subyacente de las páginas ASP.NET, debe extender el `Page` clase.


[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample5.cs)]

Cada vez que se solicita una página ASP.NET continúa a través de una serie de fases, y se culmina en la página solicitada que se va a representar en HTML. Podemos aprovechar una fase invalidando el `Page` la clase `OnEvent` método. Para nuestra base de la página Vamos a establecer automáticamente el título si no se ha especificado explícitamente por el `LoadComplete` fase (que, como habrá adivinado, se produce después de la `Load` fase).

Para ello, invalide el `OnLoadComplete` método y escriba el código siguiente:


[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample6.cs)]

El `OnLoadComplete` método empieza por determinar si la `Title` no se ha establecido aún de explícitamente propiedad. Si el `Title` propiedad es `null`, una cadena vacía, o tiene el valor "Página sin título", se asigna al nombre de archivo de la página ASP.NET solicitada. La ruta de acceso física a la página ASP.NET solicitada - `C:\MySites\Tutorial03\Login.aspx`, por ejemplo, es accesible a través de la `Request.PhysicalPath` propiedad. El `Path.GetFileNameWithoutExtension` método se usa para extraer únicamente la parte del nombre de archivo y, a continuación, se asigna este nombre de archivo a la `Page.Title` propiedad.

> [!NOTE]
> Le invito a mejorar esta lógica para mejorar el formato del título. Por ejemplo, si el nombre de archivo de la página es `Company-Products.aspx`, el código anterior genera el título "Productos de la empresa", pero lo ideal es que el guión se reemplazaría por un espacio, como en "Productos de empresa". Además, puede agregar un espacio cuando hay un cambio de mayúsculas. Es decir, considere la posibilidad de agregar código que transforma el nombre de archivo `OurBusinessHours.aspx` a un título de "Nuestro horario comercial".


### <a name="having-the-content-pages-inherit-the-base-page-class"></a>Tener las páginas de contenido heredan la clase Base

Ahora es necesario actualizar las páginas de ASP.NET en nuestro sitio para que se derivan de la página base personalizada (`BasePage`) en lugar de la `Page` clase. Para lograr esta vaya a cada clase de código subyacente y cambiar la declaración de clase desde:


[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample7.cs)]

A:


[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample8.cs)]

Una vez hecho esto, visite el sitio mediante un explorador. Si visita una página cuyo título se establece explícitamente, como `Default.aspx` o `About.aspx`, se usa el título especificado explícitamente. Si, sin embargo, visita una página cuyo título no ha cambiado el valor predeterminado ("página sin título"), la clase base de página establece el título para el nombre de archivo de la página.

La figura 5 muestra el `MultipleContentPlaceHolders.aspx` página cuando se ve mediante un explorador. Tenga en cuenta que el título es precisamente filename de la página (menos la extensión), "MultipleContentPlaceHolders".


[![Si un título no es especifica explícitamente, el nombre de archivo de la página es usa automáticamente](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image6.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image5.png)

**Figura 05**: si un título no es especifica explícitamente, el nombre de archivo de la página es usa automáticamente ([haga clic aquí para ver imagen en tamaño completo](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image7.png))


## <a name="step-3-basing-the-page-title-on-the-site-map"></a>Paso 3: Basar el título de página en el mapa del sitio

ASP.NET ofrece un marco de asignación de sitio robusto que permite a los desarrolladores de páginas definir un mapa jerárquico del sitio en un recurso externo (por ejemplo, una tabla de base de datos o archivo XML) junto con los controles Web para mostrar información sobre el mapa del sitio (por ejemplo, el SiteMapPath, Menús y controles TreeView).

La estructura de mapa del sitio también se puede acceder mediante programación desde la clase de código subyacente de una página ASP.NET. De esta manera podemos establecer automáticamente título de una página en el título de nodo correspondiente en el mapa del sitio. Vamos a mejorar la `BasePage` clase creada en el paso 2, por lo que ofrece esta funcionalidad. Pero primero necesitamos crear un mapa del sitio de nuestro sitio.

> [!NOTE]
> En este tutorial se da por supuesto que el lector ya está familiarizado con ASP. Características de mapa del sitio de la red. Para obtener más información sobre el uso de mapa del sitio, consulte mi serie de artículos de varias partes, [examen de ASP. Navegación del sitio de la red](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx).


### <a name="creating-the-site-map"></a>Crear el mapa del sitio

El sistema de sitio se basa en el [modelo de proveedor](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), que desacopla el mapa del sitio API de la lógica que se serializa la información de asignación de sitio entre la memoria y un almacén persistente. .NET Framework se incluye con el [ `XmlSiteMapProvider` clase](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx), que es el proveedor del mapa del sitio predeterminado. Como su nombre implica, `XmlSiteMapProvider` utiliza un archivo XML como su almacén de mapa del sitio. Vamos a usar este proveedor para definir nuestro mapa del sitio.

Empiece por crear un archivo de mapa del sitio en la carpeta de raíz del sitio Web denominada `Web.sitemap`. Para ello, haga doble clic en el nombre del sitio Web en el Explorador de soluciones, seleccione Agregar nuevo elemento y seleccione la plantilla de mapa del sitio. Asegúrese de que el archivo se denomina `Web.sitemap` y haga clic en Agregar.


[![Agregue un archivo denominado Web.sitemap a carpeta de raíz del sitio Web](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image9.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image8.png)

**Figura 06**: agregar un archivo denominado `Web.sitemap` a carpeta de raíz del sitio Web ([haga clic aquí para ver imagen en tamaño completo](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image10.png))


Agregue el siguiente código XML para el `Web.sitemap` archivo:


[!code-xml[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample9.xml)]

Este XML define la estructura jerárquica que se muestra en la figura 7.


![El mapa del sitio es actualmente consta de tres nodos mapa del sitio](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image11.png)

**Figura 07**: el mapa del sitio es actualmente consta de tres nodos mapa del sitio


Actualizaremos la estructura de mapa del sitio en próximos tutoriales como agregamos nuevos ejemplos.

### <a name="updating-the-master-page-to-include-navigation-web-controls"></a>Actualizar la página maestra para incluir controles de navegación Web

Ahora que tenemos un mapa del sitio definido, vamos a actualizar la página maestra para incluir controles de navegación Web. En concreto, vamos a agregar un control ListView a la columna izquierda en la sección de lecciones que presenta una lista desordenada con un elemento de lista para cada nodo que se define en el mapa del sitio.

> [!NOTE]
> El control ListView es nuevo en la versión 3.5 de ASP.NET. Si utiliza una versión anterior de ASP.NET, utilice el control Repeater en su lugar. Para obtener más información sobre el control ListView, consulte [utilizando ASP.NET 3.5 ListView y controles DataPager](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx).


Empiece quitando el marcado existente de la lista sin ordenar de la sección lecciones. A continuación, arrastre un control ListView desde el cuadro de herramientas y colóquelo debajo de las lecciones encabezado. El ListView se encuentra en la sección de datos de cuadro de herramientas, junto con los otros controles de vista: el control GridView, DetailsView y FormView. Establecer propiedad de identificador del ListView en `LessonsList`.

Desde el Asistente para configuración de orígenes de datos que elija para enlazar el ListView con un nuevo control SiteMapDataSource denominado `LessonsDataSource`. El control SiteMapDataSource devuelve la estructura jerárquica del sistema de mapa del sitio.


[![Enlazar un Control SiteMapDataSource al Control ListView LessonsList](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image13.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image12.png)

**Figura 08**: enlazar un SiteMapDataSource Control a la `LessonsList` ListView Control ([haga clic aquí para ver imagen en tamaño completo](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image14.png))


Después de crear el control SiteMapDataSource, necesitamos definir plantillas de ListView de manera que represente una lista desordenada con un elemento de lista para cada nodo devuelto por el control SiteMapDataSource. Esto puede lograrse mediante el siguiente marcado de plantilla:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample10.aspx)]

El `LayoutTemplate` genera el marcado para una lista desordenada (`<ul>...</ul>`) mientras el `ItemTemplate` procesa cada elemento devuelto por el SiteMapDataSource como un elemento de lista (`<li>`) que contenga un vínculo a la lección determinado.

Después de configurar las plantillas de ListView, visite el sitio Web. Como se muestra en la figura 9, la sección lecciones contiene un solo elemento con viñetas, principal. ¿Dónde están About y el uso de las lecciones de controles ContentPlaceHolder varios? El SiteMapDataSource está diseñado para devolver un conjunto jerárquico de datos, pero el control ListView solo puede mostrar un solo nivel de la jerarquía. Por lo tanto, se muestra solo el primer nivel de nodos del mapa del sitio devuelto por el SiteMapDataSource.


[![La sección lecciones contiene un único elemento de lista](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image16.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image15.png)

**Figura 09**: la sección lecciones contiene un único elemento de lista ([haga clic aquí para ver imagen en tamaño completo](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image17.png))


Para mostrar varios niveles podríamos anidar varias ListView dentro de la `ItemTemplate`. Esta técnica se examinó en el [ *páginas maestras y navegación del sitio* tutorial](../../data-access/introduction/master-pages-and-site-navigation-cs.md) de mi [trabajar con la serie de tutoriales de datos](../../data-access/index.md). Sin embargo, para esta serie de tutoriales nuestro mapa del sitio contiene sólo un dos niveles: principal (el nivel superior); y cada lección como elemento secundario de casa. En lugar de diseñar un ListView anidado, nos podemos indicar en su lugar SiteMapDataSource para no devolver el nodo inicial estableciendo su [ `ShowStartingNode` propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx) a `false`. El efecto neto es que el SiteMapDataSource comienza devolviendo el segundo nivel de nodos del mapa del sitio.

Con este cambio, el ListView muestra los elementos de viñeta de que está en About y utilizando varios controles ContentPlaceHolder lecciones, pero se omite un elemento de viñeta para el hogar. Para solucionar esto, podemos agregar explícitamente un elemento de viñeta de página de inicio de la `LayoutTemplate`:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample11.aspx)]

Configurando el SiteMapDataSource para omitir el nodo de inicio y agregar explícitamente un elemento de viñeta de página principal, la sección lecciones ahora muestra el resultado deseado.


[![La sección lecciones contiene un elemento de viñeta de principal y cada nodo secundario](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image19.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image18.png)

**Figura 10**: la sección lecciones contiene un elemento de viñeta de principal y todos los nodos secundarios ([haga clic aquí para ver imagen en tamaño completo](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image20.png))


### <a name="setting-the-title-based-on-the-site-map"></a>Establecer el título según el mapa del sitio

Con el mapa del sitio en su lugar, podemos actualizar nuestro `BasePage` clase con el título especificado en el mapa del sitio. Como hicimos en el paso 2, solo queremos usar el título del nodo mapa del sitio si el título de la página no se ha establecido explícitamente por el desarrollador de páginas. Si la página solicitada no tiene establecido explícitamente un título de página y no se encuentra en el mapa del sitio, a continuación, se deberá recurrir al uso filename de la página solicitada (menos la extensión), como hicimos en el paso 2. Figura 11 se ilustra este proceso de toma de decisiones.


![En ausencia de una manera explícita Establecer título de página, se utiliza el título del nodo correspondiente de asignación de sitio](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image21.png)

**Figura 11**: en ausencia de una manera explícita Establecer título de página, se utiliza el título del nodo correspondiente de asignación de sitio


Actualización de la `BasePage` la clase `OnLoadComplete` método para incluir el código siguiente:


[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample12.cs)]

Como antes, el `OnLoadComplete` método empieza por determinar si el título de la página se ha establecido explícitamente. Si `Page.Title` es `null`, una cadena vacía, o se asigna el valor "Página sin título", a continuación, el código asigna automáticamente un valor a `Page.Title`.

Para determinar el título que se utilizará, el código comienza por hacer referencia a la [ `SiteMap` clase](https://msdn.microsoft.com/library/system.web.sitemap.aspx)del [ `CurrentNode` propiedad](https://msdn.microsoft.com/library/system.web.sitemap.currentnode.aspx). `CurrentNode` Devuelve el [ `SiteMapNode` ](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) instancia en el mapa del sitio que corresponde a la página solicitada actualmente. Suponiendo que la página solicitada actualmente se encuentra dentro del mapa del sitio, el `SiteMapNode`del `Title` propiedad está asignada al título de la página. Si la página actualmente solicitada no está en el mapa del sitio, `CurrentNode` devuelve `null` y nombre de archivo de la página solicitada se utiliza como título (como se hizo en el paso 2).

Figura 12 se muestra el `MultipleContentPlaceHolders.aspx` página cuando se ve mediante un explorador. Porque no se establece explícitamente el título de la página, el título de su correspondiente sitio del nodo del mapa se usa en su lugar.


![Título de la página MultipleContentPlaceHolders.aspx se extrae del mapa del sitio](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image22.png)

**Figura 12**: el `MultipleContentPlaceHolders.aspx` se extrae el título de la página del mapa del sitio


## <a name="step-4-adding-other-page-specific-markup-to-theheadsection"></a>Paso 4: Agregar otro elemento de marcado específicas de la página a la`<head>`sección

Los pasos 1, 2 y 3 examinado personalizar el `<title>` elemento según una página por página. Además `<title>`, `<head>` sección puede contener `<meta>` elementos y `<link>` elementos. Como se indicó anteriormente en este tutorial, `Site.master`del `<head>` sección incluye una `<link>` elemento `Styles.css`. Dado que esto `<link>` elemento está definido en la página maestra, se incluirá en el `<head>` sección para todas las páginas de contenido. Pero ¿cómo podríamos acerca de cómo agregar `<meta>` y `<link>` elementos según una página por página?

La manera más fácil de agregar contenido específico de la página a la `<head>` sección es crear un control ContentPlaceHolder en la página maestra. Ya tenemos tal un ContentPlaceHolder (denominado `head`). Por lo tanto, para agregar personalizado `<head>` marcado, creará un control en la página de contenido y colocar allí el marcado.

Para ilustrar personalizado agregando `<head>` marcado para una página, vamos a incluir un `<meta>` elemento description para nuestro conjunto actual de páginas de contenido. El `<meta>` description (elemento) proporciona una breve descripción acerca de la página web; la mayoría de los motores de búsqueda incorporan esta información en alguna forma al mostrar los resultados de búsqueda.

Un `<meta>` description (elemento) tiene el formato siguiente:


[!code-html[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample13.html)]

Para agregar este marcado para una página de contenido, agregue el texto anterior para el control de contenido que se asigna a la cabeza de la página maestra ContentPlaceHolder. Por ejemplo, para definir un `<meta>` elemento description para `Default.aspx`, agregue el siguiente marcado:


[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample14.aspx)]

Dado que el encabezado ContentPlaceHolder no está dentro del cuerpo de la página HTML, el marcado agregado al control de contenido no se muestra en la vista Diseño. Para ver el `<meta>` Descripción elemento visita `Default.aspx` a través de un explorador. Después de haberse cargada la página, vea el código fuente y tenga en cuenta que el `<head>` sección incluye el marcado especificado en el control de contenido.

Dedique un momento para agregar `<meta>` elementos description para `About.aspx`, `MultipleContentPlaceHolders.aspx`, y `Login.aspx`.

### <a name="programmatically-adding-markup-to-theheadregion"></a>Agregar mediante programación el marcado para el`<head>`región

El encabezado ContentPlaceHolder nos permite agregar mediante declaración de marcado personalizada a la página maestra `<head>` región. También se puede agregar mediante programación el marcado personalizada. Recuerde que el `Page` la clase `Header` propiedad devuelve el `HtmlHead` instancia definida en la página maestra (el `<head runat="server">`).

Poder agregar mediante programación el contenido a la `<head>` región es útil cuando el contenido que se va a agregar es dinámico. Quizás se basa en el usuario visita la página. quizás es que se extrajo de una base de datos. Independientemente del motivo, puede agregar contenido a la `HtmlHead` agregando controles a su colección de controles de este modo:


[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample15.cs)]

El código anterior agrega el `<meta>` elemento palabras clave para el `<head>` región, que proporciona una lista delimitada por comas de palabras clave que describen la página. Tenga en cuenta que para agregar un `<meta>` etiqueta a la que se crea un [ `HtmlMeta` ](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlmeta.aspx) de instancia, establezca su `Name` y `Content` propiedades y, a continuación, agréguelo a la `Header`del `Controls` colección. De forma similar, para agregar mediante programación un `<link>` elemento, crear un [ `HtmlLink` ](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmllink.aspx) objeto, establecer sus propiedades y, a continuación, agregarlo a la `Header`del `Controls` colección.

> [!NOTE]
> Para agregar marcado arbitrario, cree un [ `LiteralControl` ](https://msdn.microsoft.com/library/system.web.ui.literalcontrol.aspx) de instancia, establezca su `Text` propiedad y, a continuación, agréguelo a la `Header`del `Controls` colección.


## <a name="summary"></a>Resumen

En este tutorial hemos visto una variedad de formas de agregar `<head>` marcado de la región en la página por página. Una página maestra debe incluir un `HtmlHead` instancia (`<head runat="server">`) con un ContentPlaceHolder. El `HtmlHead` instancia permite que las páginas de contenido para el acceso mediante programación el `<head>` región y establecer mediante declaración y mediante programación la página el título del; permite que el control ContentPlaceHolder marcado personalizada que se agregarán a la `<head>` sección mediante declaración a través de un control de contenido.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Establecer dinámicamente el título de la página en ASP.NET](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [Examen de ASP. Navegación del sitio de red](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Cómo usar etiquetas HTML Meta](http://searchenginewatch.com/showPage.html?page=2167931)
- [Páginas maestras en ASP.NET](http://www.odetocode.com/articles/419.aspx)
- [Con ASP.NET del 3.5 ListView y controles DataPager](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [Uso de una clase Base personalizada para las clases de código subyacente de las páginas de ASP.NET](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)

### <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de varios libros sobre ASP/ASP.NET y fundador de 4GuysFromRolla.com, ha estado trabajando con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [ *Sams Teach usted mismo ASP.NET 3.5 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) o a través de su blog en [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Los revisores para este tutorial fueron Zack Jones y Suchi Banerjee. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Anterior](multiple-contentplaceholders-and-default-content-cs.md)
> [Siguiente](urls-in-master-pages-cs.md)
