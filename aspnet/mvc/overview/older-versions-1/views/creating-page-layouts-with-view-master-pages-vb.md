---
uid: mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-vb
title: Crear diseños de página con las páginas maestras de vista (VB) | Documentos de Microsoft
author: microsoft
description: En este tutorial, aprenderá a crear un diseño de página común para varias páginas en la aplicación aprovechando las ventajas de la vista de páginas maestras. Puede usar un...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/16/2008
ms.topic: article
ms.assetid: d34f90a1-6de3-482a-a326-f87fdcbaaaff
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 5208cedd8d24a290a0227bdcbaa84ae6210cd969
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="creating-page-layouts-with-view-master-pages-vb"></a>Crear diseños de página con las páginas maestras de vista (VB)
====================
por [Microsoft](https://github.com/microsoft)

[Descarga de PDF](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_12_VB.pdf)

> En este tutorial, aprenderá a crear un diseño de página común para varias páginas en la aplicación aprovechando las ventajas de la vista de páginas maestras. Puede utilizar una página maestra de la vista, por ejemplo, para definir un diseño de página de dos columnas y use el diseño de dos columnas para todas las páginas en la aplicación web.


## <a name="creating-page-layouts-with-view-master-pages"></a>Crear diseños de página con las páginas maestras de vista

En este tutorial, aprenderá a crear un diseño de página común para varias páginas en la aplicación aprovechando las ventajas de la vista de páginas maestras. Puede utilizar una página maestra de la vista, por ejemplo, para definir un diseño de página de dos columnas y use el diseño de dos columnas para todas las páginas en la aplicación web.

También puede beneficiarse de la vista de páginas maestras para compartir contenido común a través de varias páginas en la aplicación. Por ejemplo, puede colocar el logotipo del sitio Web, los vínculos de navegación y los anuncios de pancarta en una página maestra de la vista. De este modo, todas las páginas de la aplicación deberían mostrar este contenido automáticamente.

En este tutorial, aprenderá a crear una nueva página de vista maestra y crear una nueva página de contenido de vista basada en la página maestra.

### <a name="creating-a-view-master-page"></a>Crear una página maestra de vista

Empecemos creando una página maestra de vista que define un diseño de dos columnas. Agregar una nueva página maestra de la vista a un proyecto MVC haciendo clic en la carpeta Views\Shared, al seleccionar la opción de menú **agregar, nuevo elemento**y seleccione la plantilla de página maestra de la vista de MVC (consulte la figura 1).


[![Agregar una página maestra de vista](creating-page-layouts-with-view-master-pages-vb/_static/image2.png)](creating-page-layouts-with-view-master-pages-vb/_static/image1.png)

**Figura 01**: agregar una página maestra de vista ([haga clic aquí para ver la imagen a tamaño completo](creating-page-layouts-with-view-master-pages-vb/_static/image3.png))


Puede crear más de una página de vista maestra en una aplicación. Cada página maestra de la vista puede definir un diseño de página diferentes. Por ejemplo, puede que desee determinadas páginas tienen un diseño de dos columnas y otras páginas que tienen un diseño de tres columnas.

Una página maestra de vista parece mucho a una vista de ASP.NET MVC estándar. Sin embargo, a diferencia de una vista normal, una página maestra de la vista contiene uno o varios `<asp:ContentPlaceHolder>` etiquetas. El `<contentplaceholder>` se usan etiquetas para marcar las áreas de la página maestra que se pueden invalidar en una página de contenido individual.

Por ejemplo, la página maestra de la vista en la lista 1 define un diseño de dos columnas. Contiene dos `<contentplaceholder>` etiquetas. Un `<ContentPlaceHolder>` para cada columna.

**Lista 1: `Views\Shared\Site.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample1.aspx)]

El cuerpo de la vista de página maestra en el listado 1 contiene dos `<div>` etiquetas que corresponden a las dos columnas. La clase de columna de la hoja de estilos en cascada se aplica a ambos `<div>` etiquetas. Esta clase se define en la hoja de estilos que se declaran en la parte superior de la página maestra. Puede ver cómo se presentará la página maestra de la vista al cambiar a la vista Diseño. Haga clic en la pestaña diseño en la parte inferior izquierda del editor de código fuente (consulte la figura 2).


[![Obtener una vista previa una página maestra en el diseñador](creating-page-layouts-with-view-master-pages-vb/_static/image5.png)](creating-page-layouts-with-view-master-pages-vb/_static/image4.png)

**Figura 02**: obtener una vista previa una página maestra en el diseñador ([haga clic aquí para ver la imagen a tamaño completo](creating-page-layouts-with-view-master-pages-vb/_static/image6.png))


### <a name="creating-a-view-content-page"></a>Crear una página de vista de contenido

Después de crear una página maestra de la vista, puede crear vista de una o varias páginas de contenido basadas en la página maestra de la vista. Por ejemplo, puede crear una página de contenido de la vista de índice para el controlador Home haciendo clic en la carpeta Views\Home, seleccionar **agregar, nuevo elemento**, seleccione la **página de contenido de vista de MVC** plantilla, escriba el nombre Index.aspx y haga clic en Agregar botón (consulte la figura 3).


[![Agregar una página de vista de contenido](creating-page-layouts-with-view-master-pages-vb/_static/image8.png)](creating-page-layouts-with-view-master-pages-vb/_static/image7.png)

**Figura 03**: agregar una página de vista de contenido ([haga clic aquí para ver la imagen a tamaño completo](creating-page-layouts-with-view-master-pages-vb/_static/image9.png))


Tras hacer clic en el botón Agregar, aparece un diálogo nuevo que le permite seleccionar una página maestra de la vista para asociar a la página de vista de contenido (consulte la figura 4). Puede navegar a la página de principal de la vista Site.master que hemos creado en la sección anterior.


[![Al seleccionar una página maestra](creating-page-layouts-with-view-master-pages-vb/_static/image11.png)](creating-page-layouts-with-view-master-pages-vb/_static/image10.png)

**Figura 04**: al seleccionar una página maestra ([haga clic aquí para ver la imagen a tamaño completo](creating-page-layouts-with-view-master-pages-vb/_static/image12.png))


Después de crear una nueva página de contenido de vista basada en la página maestra Site.master, se crea el archivo en el listado 2.

**La lista 2: `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample2.aspx)]

Tenga en cuenta que esta vista contiene un `<asp:Content>` etiqueta que corresponde a cada uno de los `<asp:ContentPlaceHolder>` etiquetas en la página maestra de la vista. Cada `<asp:Content>` etiqueta incluye un atributo ContentPlaceHolderID que apunta a ese `<asp:ContentPlaceHolder>` que reemplaza.

Además, tenga en cuenta que la página de vista de contenido en el listado 2 no contiene cualquiera de las etiquetas de cierre HTML y normales de apertura. Por ejemplo, no contiene la apertura y cierre `<html>` o `<head>` etiquetas. Todas las etiquetas de cierre y apertura normal se encuentran en la página maestra de la vista.

Cualquier contenido que desea mostrar en una página de contenido de la vista debe colocarse dentro de un `<asp:Content>` etiqueta. Si coloca el código HTML u otro contenido fuera de estas etiquetas, obtendrá un error al intentar ver la página.

No es necesario invalidar cada `<asp:ContentPlaceHolder>` etiqueta desde una página maestra en una página de vista de contenido. Solo necesita invalidar un `<asp:ContentPlaceHolder>` etiqueta cuando desea reemplazar la etiqueta con un contenido en particular.

Por ejemplo, la vista de índice modificada en el listado 3 contiene solo dos `<asp:Content>` etiquetas. Cada uno de los `<asp:Content>` etiquetas incluye algún texto.

**Enumerar 3: `Views\Home\Index.aspx (modified)`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample3.aspx)]

Cuando se solicita la vista en la lista 3, representa la página en la figura 5. Tenga en cuenta que la vista presenta una página con dos columnas. Además, tenga en cuenta que el contenido de la página de vista de contenido se combina con el contenido de la página maestra de la vista.


[![La página de contenido de la vista de índice](creating-page-layouts-with-view-master-pages-vb/_static/image14.png)](creating-page-layouts-with-view-master-pages-vb/_static/image13.png)

**Figura 05**: página de índice de la vista contenido ([haga clic aquí para ver la imagen a tamaño completo](creating-page-layouts-with-view-master-pages-vb/_static/image15.png))


### <a name="modifying-view-master-page-content"></a>Modificar el contenido de la página de vista maestra

Uno de los problemas que se encuentra casi inmediatamente cuando se trabaja con páginas maestras de vista es el problema consiste en modificar el contenido de la página principal de vista cuando se solicitan ver diferentes páginas de contenido. Por ejemplo, desea que cada página en la aplicación web para tener un título único. Sin embargo, el título se declara en la página maestra de la vista y no en la página de contenido de la vista. Por lo tanto, ¿cómo se personaliza el título de página para cada página de vista de contenido?

Hay dos formas que se puede modificar el título que se muestra mediante una página de contenido de la vista. En primer lugar, puede asignar un título de página con el atributo de título de la `<%@ page %>` directiva se declara en la parte superior de una página de contenido de la vista. Por ejemplo, si desea asignar el título de la página "Sitio Web excelente de Super" a la vista de índice, puede incluir la siguiente directiva en la parte superior de la vista de índice:

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample4.aspx)]

Cuando la vista de índice se representa en el explorador, el título deseado aparece en la barra de título del explorador:


[![Barra de título del explorador](creating-page-layouts-with-view-master-pages-vb/_static/image17.png)](creating-page-layouts-with-view-master-pages-vb/_static/image16.png)


Hay un requisito importante que debe cumplir una página de vista maestra en orden para el atributo de título para que funcione. La página maestra de la vista debe contener una `<head runat="server">` etiqueta en lugar de una normal `<head>` etiqueta para el encabezado. Si el `<head>` etiqueta no incluye el runat = atributo "server", a continuación, el título no aparecerá. La vista predeterminada incluye la página maestra `<head runat="server">` etiqueta.

Un enfoque alternativo para modificar el contenido de la página maestra de una página de contenido de la vista individuales consiste en encapsular la región que desea modificar en un `<asp:ContentPlaceHolder>` etiqueta. Por ejemplo, imagine que desea cambiar no solo el título, sino también las etiquetas meta, representadas por una página de vista maestra. La página de vista maestra en el listado 4 contiene un `<asp:ContentPlaceHolder>` etiquetar dentro de su `<head>` etiqueta.

**Enumerar 4: `Views\Shared\Site2.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample5.aspx)]

Tenga en cuenta que la `<asp:ContentPlaceHolder>` etiqueta en el listado 4 incluye contenido predeterminado: un título predeterminado y etiquetas meta de forma predeterminada. Si no se invalidan este `<asp:ContentPlaceHolder>` de etiquetas en una página de contenido de la vista individuales, se mostrará el contenido de forma predeterminada.

La página de vista de contenido en el listado 5 invalida la `<asp:ContentPlaceHolder>` etiqueta con el fin de mostrar un título personalizado y etiquetas meta personalizadas.

**Enumerar 5: `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample6.aspx)]

### <a name="summary"></a>Resumen

Este tutorial le proporcionará una introducción básica a ver páginas maestras y páginas de contenido. Aprendió a crear nueva vista páginas maestras y crear páginas de contenido de la vista basadas en ellas. También se examina cómo puede modificar el contenido de una página maestra de la vista de una página de contenido de la vista.

> [!div class="step-by-step"]
> [Anterior](using-the-tagbuilder-class-to-build-html-helpers-vb.md)
> [Siguiente](passing-data-to-view-master-pages-vb.md)
