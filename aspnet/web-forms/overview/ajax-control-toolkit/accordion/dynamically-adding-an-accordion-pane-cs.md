---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
title: Agregar dinámicamente un panel Accordion (C#) | Microsoft Docs
author: wenz
description: El control Accordion de AJAX Control Toolkit ofrece varios paneles y permite al usuario mostrar uno de ellos a la vez. Los paneles se declaran normalmente w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 66d88cfa-f26f-46b1-ad52-1c9e03c04a48
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
msc.type: authoredcontent
ms.openlocfilehash: 555a8a5cdd701bba3d41c3ff3313fe119a350e53
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37381537"
---
<a name="dynamically-adding-an-accordion-pane-c"></a>Agregar dinámicamente un panel Accordion (C#)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.cs.zip) o [descargar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2CS.pdf)

> El control Accordion de AJAX Control Toolkit ofrece varios paneles y permite al usuario mostrar uno de ellos a la vez. Paneles normalmente se declaran dentro de la propia página, pero el código del lado servidor puede usarse para lograr el mismo resultado.


## <a name="overview"></a>Información general

El control Accordion de AJAX Control Toolkit ofrece varios paneles y permite al usuario mostrar uno de ellos a la vez. Paneles normalmente se declaran dentro de la propia página, pero el código del lado servidor puede usarse para lograr el mismo resultado.

## <a name="steps"></a>Pasos

El control Accordion expone todas las propiedades importantes de código del lado servidor. Entre otras cosas, la `Panes` propiedad concede acceso a la colección de paneles que componen el Accordion. Cada panel hay de tipo `AccordionPane`. Por lo tanto, es muy fácil crear un panel de este tipo:

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample1.cs)]

El `HeaderContainer` propiedad de `AccordionPane` proporciona acceso a los controles de ASP.NET dentro de la sección de encabezado del panel; la `ContentContainer` propiedad de `AccordionPane` hace lo mismo para la sección contenido del panel. Esto permite que el código ASP.NET agregar contenido a los paneles:

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample2.cs)]

Por último, el panel o paneles deben agregarse a la `Panes` colección de lo Accordion:

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample3.cs)]

Este es un código de servidor completando que agrega dos paneles a un control Accordion:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample4.aspx)]

El único elemento que falta es el propio, acordeón que depende de la presencia de ASP.NET `ScriptManager` control:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample5.aspx)]

Para finalizar el ejemplo, las dos clases CSS que se hace referencia en el control Accordion proporcionan información de estilo para el explorador:

[!code-css[Main](dynamically-adding-an-accordion-pane-cs/samples/sample6.css)]


[![Los datos en el accordion dinámicamente se agregan mediante código de servidor](dynamically-adding-an-accordion-pane-cs/_static/image2.png)](dynamically-adding-an-accordion-pane-cs/_static/image1.png)

Los datos en el accordion dinámicamente se agregan mediante código de servidor ([haga clic aquí para ver imagen en tamaño completo](dynamically-adding-an-accordion-pane-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](databinding-to-an-accordion-cs.md)
> [Siguiente](databinding-to-an-accordion-vb.md)
