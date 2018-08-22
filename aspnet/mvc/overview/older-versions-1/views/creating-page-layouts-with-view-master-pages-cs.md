---
uid: mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-cs
title: Crear diseños de página con páginas maestras de vista (C#) | Microsoft Docs
author: microsoft
description: En este tutorial, aprenderá a crear un diseño de página común para varias páginas en la aplicación aprovechando las ventajas de vista de las páginas maestras. Puede usar un...
ms.author: riande
ms.date: 10/16/2008
ms.assetid: dff54fcb-68b1-4488-89a2-ca97532d6a4c
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: acf304d0f1cf349f7e9c30acb9271e602a47d3d9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835735"
---
<a name="creating-page-layouts-with-view-master-pages-c"></a>Crear diseños de página con páginas maestras de vista (C#)
====================
por [Microsoft](https://github.com/microsoft)

[Descargar PDF](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_12_CS.pdf)

> En este tutorial, aprenderá a crear un diseño de página común para varias páginas en la aplicación aprovechando las ventajas de vista de las páginas maestras. Puede usar una página maestra de la vista, por ejemplo, para definir un diseño de página de dos columnas y usar el diseño de dos columnas para todas las páginas en la aplicación web.


## <a name="creating-page-layouts-with-view-master-pages"></a>Crear diseños de página con páginas maestras de vista

En este tutorial, aprenderá a crear un diseño de página común para varias páginas en la aplicación aprovechando las ventajas de vista de las páginas maestras. Puede usar una página maestra de la vista, por ejemplo, para definir un diseño de página de dos columnas y usar el diseño de dos columnas para todas las páginas en la aplicación web.

También puede aprovechar las ventajas de vista de páginas maestras para compartir contenido común en varias páginas en la aplicación. Por ejemplo, puede colocar el logotipo del sitio Web, vínculos de navegación y anuncios de pancarta en una página maestra de la vista. De este modo, todas las páginas en la aplicación podrían mostrar este contenido automáticamente.

En este tutorial, aprenderá a crear una nueva página principal de la vista y crear una nueva página de contenido de vista basada en la página maestra.

### <a name="creating-a-view-master-page"></a>Creación de una página de vista maestra

Comencemos por crear una página maestra de la vista que define un diseño de dos columnas. Agregar una nueva página principal de la vista a un proyecto MVC hace doble clic en la carpeta Views\Shared, seleccionando la opción de menú **agregar, nuevo elemento**y seleccionando la **página maestra de la vista de MVC** plantilla (consulte la figura 1).


[![Adición de una página maestra de la vista](creating-page-layouts-with-view-master-pages-cs/_static/image2.png)](creating-page-layouts-with-view-master-pages-cs/_static/image1.png)

**Figura 01**: adición de una página de vista maestra ([haga clic aquí para ver imagen en tamaño completo](creating-page-layouts-with-view-master-pages-cs/_static/image3.png))


Puede crear más de una página maestra de la vista en una aplicación. Cada página principal de la vista puede definir un diseño de página diferentes. Por ejemplo, puede que desee ciertas páginas que tienen un diseño de dos columnas y otras páginas que tienen un diseño de tres columnas.

Una página maestra de la vista parece mucho a una vista de MVC de ASP.NET estándar. Sin embargo, a diferencia de una vista normal, una página maestra de la vista contiene uno o varios `<asp:ContentPlaceHolder>` etiquetas. El `<contentplaceholder>` se usan etiquetas para marcar las áreas de la página maestra que se pueden invalidar en una página de contenido individual.

Por ejemplo, la página principal de la vista en el listado 1 define un diseño de dos columnas. Contiene dos `<contentplaceholder>` etiquetas. Un `<ContentPlaceHolder>` para cada columna.

**Listado 1: `Views\Shared\Site.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample1.aspx)]

El cuerpo de la vista de página maestra en el listado 1 contiene dos `<div>` etiquetas que corresponden a las dos columnas. La clase de columna de la hoja de estilos en cascada se aplica a ambos `<div>` etiquetas. Esta clase se define en la hoja de estilos que se declaran en la parte superior de la página maestra. Puede ver cómo se presentará la página principal de la vista al cambiar a vista de diseño. Haga clic en la pestaña diseño en la parte inferior izquierda del editor de código fuente (consulte la figura 2).


[![Vista previa de una página maestra en el diseñador](creating-page-layouts-with-view-master-pages-cs/_static/image5.png)](creating-page-layouts-with-view-master-pages-cs/_static/image4.png)

**Figura 02**: vista previa de una página maestra en el diseñador ([haga clic aquí para ver imagen en tamaño completo](creating-page-layouts-with-view-master-pages-cs/_static/image6.png))


### <a name="creating-a-view-content-page"></a>Creación de una página de contenido de vista

Después de crear una página maestra de la vista, puede crear vista de una o varias páginas de contenido basadas en la página principal de la vista. Por ejemplo, puede crear una página de contenido de la vista de índice para el controlador Home hace doble clic en la carpeta Views\Home, seleccionar **agregar, nuevo elemento**, seleccionando la **página de contenido de vista de MVC** plantilla, escriba el nombre Index.aspx y haga clic en el **agregar** (consulte la figura 3).


[![Agregar una página de contenido de vista](creating-page-layouts-with-view-master-pages-cs/_static/image8.png)](creating-page-layouts-with-view-master-pages-cs/_static/image7.png)

**Figura 03**: adición de una página de contenido de vista ([haga clic aquí para ver imagen en tamaño completo](creating-page-layouts-with-view-master-pages-cs/_static/image9.png))


Tras hacer clic en el botón Agregar, aparece un diálogo nuevo que le permite seleccionar una página maestra de la vista para asociar a la página de vista de contenido (consulte la figura 4). Puede navegar a la página maestra vista Site.master que creamos en la sección anterior.


[![Seleccionar una página maestra](creating-page-layouts-with-view-master-pages-cs/_static/image11.png)](creating-page-layouts-with-view-master-pages-cs/_static/image10.png)

**Figura 04**: seleccionar una página maestra ([haga clic aquí para ver imagen en tamaño completo](creating-page-layouts-with-view-master-pages-cs/_static/image12.png))


Después de crear una nueva página de contenido de vista basada en la página maestra Site.master, obtenga el archivo en el listado 2.

**Listado 2: `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample2.aspx)]

Tenga en cuenta que esta vista contiene un `<asp:Content>` etiqueta que corresponde a cada uno de los `<asp:ContentPlaceHolder>` etiquetas en la página principal de la vista. Cada `<asp:Content>` etiqueta incluye un atributo ContentPlaceHolderID que apunta a la instancia concreta `<asp:ContentPlaceHolder>` que reemplaza.

Además, tenga en cuenta que la página de vista de contenido en el listado 2 no contiene cualquiera de las etiquetas HTML de cierre y apertura normal. Por ejemplo, no contienen la apertura y cierre `<html>` o `<head>` etiquetas. Todas las etiquetas de cierre y apertura normal se encuentran en la página principal de la vista.

Cualquier contenido que desea mostrar en una página de contenido de la vista debe colocarse en un `<asp:Content>` etiqueta. Si coloca el código HTML u otro contenido fuera de estas etiquetas, obtendrá un error al intentar ver la página.

No es necesario invalidar cada `<asp:ContentPlaceHolder>` etiqueta de una página maestra en una página de vista de contenido. Solo necesita invalidar un `<asp:ContentPlaceHolder>` etiqueta cuando desea reemplazar la etiqueta con un contenido en particular.

Por ejemplo, la vista de índice modificada en el listado 3 contiene sólo dos `<asp:Content>` etiquetas. Cada uno de los `<asp:Content>` etiquetas incluye algún texto.

**Listado 3: `Views\Home\Index.aspx (modified)`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample3.aspx)]

Cuando se solicita la vista en el listado 3, representa la página en la figura 5. Tenga en cuenta que la vista presenta una página con dos columnas. Además, tenga en cuenta que el contenido de la página de contenido de la vista se combina con el contenido de la página principal de la vista


[![La página de contenido de la vista de índice](creating-page-layouts-with-view-master-pages-cs/_static/image14.png)](creating-page-layouts-with-view-master-pages-cs/_static/image13.png)

**Figura 05**: página de índice de la vista contenido ([haga clic aquí para ver imagen en tamaño completo](creating-page-layouts-with-view-master-pages-cs/_static/image15.png))


### <a name="modifying-view-master-page-content"></a>Modificar el contenido de la página de vista maestra

Un problema que se encuentra casi inmediatamente cuando se trabaja con páginas maestras de vista es el problema de modificar el contenido de la página principal de la vista cuando se solicitan las páginas de contenido de vista diferente. Por ejemplo, cada página que desee en la aplicación web para tener un título único. Sin embargo, el título se declara en la página principal de la vista y no en la página de contenido de la vista. Por lo tanto, ¿cómo personalizar el título de página para cada página de contenido de vista?

Hay dos maneras en que puede modificar el título mostrado por una página de contenido de la vista. En primer lugar, puede asignar un título de página con el atributo de título de la `<%@ page %>` directiva declarado en la parte superior de una página de contenido de la vista. Por ejemplo, si desea asignar un título de la página "Sitio Web excelente de Super" a la vista de índice, puede incluir la siguiente directiva en la parte superior de la vista de índice:

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample4.aspx)]

Cuando se representa la vista de índice en el explorador, el título deseado aparece en la barra de título del explorador:


[![Barra de título del explorador](creating-page-layouts-with-view-master-pages-cs/_static/image17.png)](creating-page-layouts-with-view-master-pages-cs/_static/image16.png)


Hay un requisito importante que debe satisfacer una página de vista maestra en orden para el atributo de título para que funcione. La página principal de la vista debe contener un `<head runat="server">` etiqueta en lugar de una normal `<head>` etiqueta para el encabezado. Si el `<head>` etiqueta no incluye el runat = atributo "server", a continuación, el título no aparecerá. La vista predeterminada incluye la página maestra `<head runat="server">` etiqueta.

Es un enfoque alternativo a la modificación de contenido de la página principal de una página de contenido de la vista individuales ajustar la región que desea modificar en un `<asp:ContentPlaceHolder>` etiqueta. Por ejemplo, imagine que desea cambiar no solo el título, sino también las etiquetas meta, representadas por una página de vista maestra. La página de vista maestra en el listado 4 contiene un `<asp:ContentPlaceHolder>` etiqueta dentro de su `<head>` etiqueta.

**Listado 4: `Views\Shared\Site2.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample5.aspx)]

Tenga en cuenta que la `<asp:ContentPlaceHolder>` etiqueta en el listado 4 incluye contenido predeterminado: un título predeterminado y las etiquetas meta de forma predeterminada. Si no se invalidan este `<asp:ContentPlaceHolder>` de etiquetas en una página de contenido de la vista individuales, a continuación, se mostrará el contenido predeterminado.

La página de vista de contenido en el listado 5 invalida la `<asp:ContentPlaceHolder>` etiqueta con el fin de mostrar un título personalizado y etiquetas meta personalizadas.

**Listado 5: `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample6.aspx)]

### <a name="summary"></a>Resumen

Este tutorial proporciona una introducción básica para ver las páginas maestras y las páginas de contenido. Ha aprendido a crear nueva vista de páginas maestras y crear páginas de contenido de vista basadas en ellas. También examinamos cómo se puede modificar el contenido de una página principal de la vista de una página de contenido de la vista.

> [!div class="step-by-step"]
> [Anterior](using-the-tagbuilder-class-to-build-html-helpers-cs.md)
> [Siguiente](passing-data-to-view-master-pages-cs.md)
