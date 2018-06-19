---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
title: Agregar dinámicamente un panel acordeón (VB) | Documentos de Microsoft
author: wenz
description: El control Accordion en el Kit de herramientas de Control de AJAX proporciona varios paneles y permite al usuario mostrar uno de ellos al mismo tiempo. Paneles normalmente se declaran w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: fae968c9-1902-487d-b053-86a46dd52c3f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
msc.type: authoredcontent
ms.openlocfilehash: 68c60ba6d4be5eb6709f7558d6be4165f8232a4f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868729"
---
<a name="dynamically-adding-an-accordion-pane-vb"></a>Agregar dinámicamente un panel acordeón (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) o [descarga de PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)

> El control Accordion en el Kit de herramientas de Control de AJAX proporciona varios paneles y permite al usuario mostrar uno de ellos al mismo tiempo. Paneles normalmente se declaran dentro de la propia página, pero el código del lado servidor puede utilizarse para lograr el mismo resultado.


## <a name="overview"></a>Información general

El control Accordion en el Kit de herramientas de Control de AJAX proporciona varios paneles y permite al usuario mostrar uno de ellos al mismo tiempo. Paneles normalmente se declaran dentro de la propia página, pero el código del lado servidor puede utilizarse para lograr el mismo resultado.

## <a name="steps"></a>Pasos

El control Accordion expone todas las propiedades importantes para el código del lado servidor. Entre otras cosas, la `Panes` propiedad concede acceso a la colección de paneles que forman el Accordion. Cada panel hay de tipo `AccordionPane`. Por lo tanto, es muy fácil de crear un panel de este tipo:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample1.vb)]

El `HeaderContainer` propiedad de `AccordionPane` proporciona acceso a los controles ASP.NET en la sección de encabezado del panel; la `ContentContainer` propiedad de `AccordionPane` hace lo mismo para la sección de contenido del panel. Esto permite que el código ASP.NET agregar contenido a los paneles:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample2.vb)]

Por último, se deben agregar el panel o paneles a la `Panes` colección de la Accordion:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample3.vb)]

Este es un código de servidor completo que agrega dos paneles a un control Accordion:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample4.aspx)]

El único elemento que falta es la Accordion, que depende de la presencia de ASP.NET `ScriptManager` control:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample5.aspx)]

Para terminar el ejemplo, las dos clases CSS que se hace referencia en el control Accordion proporcionan información de estilo para el explorador:

[!code-css[Main](dynamically-adding-an-accordion-pane-vb/samples/sample6.css)]


[![Los datos de la accordion dinámicamente se agregan mediante código del lado servidor](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)

Los datos de la accordion dinámicamente se agregan mediante código del lado servidor ([haga clic aquí para ver la imagen a tamaño completo](dynamically-adding-an-accordion-pane-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](databinding-to-an-accordion-vb.md)
