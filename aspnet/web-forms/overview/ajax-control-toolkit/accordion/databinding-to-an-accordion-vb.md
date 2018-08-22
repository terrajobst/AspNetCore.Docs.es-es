---
uid: web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-vb
title: Enlace de datos a Accordion (VB) | Microsoft Docs
author: wenz
description: El control Accordion de AJAX Control Toolkit ofrece varios paneles y permite al usuario mostrar uno de ellos a la vez. Los paneles se declaran normalmente w...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: b19f0875-7d3e-4ecf-baa1-a0c693c765b3
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-vb
msc.type: authoredcontent
ms.openlocfilehash: 948741e3b8618a642c440a527fddef825faf595e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829723"
---
<a name="databinding-to-an-accordion-vb"></a>Enlace de datos a Accordion (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.vb.zip) o [descargar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1VB.pdf)

> El control Accordion de AJAX Control Toolkit ofrece varios paneles y permite al usuario mostrar uno de ellos a la vez. Paneles normalmente se declaran dentro de la propia página, pero el enlace a un origen de datos ofrece más flexibilidad.


## <a name="overview"></a>Información general

El control Accordion de AJAX Control Toolkit ofrece varios paneles y permite al usuario mostrar uno de ellos a la vez. Paneles normalmente se declaran dentro de la propia página, pero el enlace a un origen de datos ofrece más flexibilidad.

## <a name="steps"></a>Pasos

En primer lugar, es necesario un origen de datos. Este ejemplo utiliza la base de datos AdventureWorks y Microsoft SQL Server 2005 Express Edition. La base de datos es una parte opcional de una instalación de Visual Studio (incluida la edición express) y también está disponible como una descarga independiente en [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). La base de datos de AdventureWorks es parte de los ejemplos de SQL Server 2005 y las bases de datos de ejemplo (descargue en [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). La manera más fácil de configurar la base de datos es usar Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) y adjuntar el `AdventureWorks.mdf` el archivo de base de datos.

Para este ejemplo, se supone que se llama a la instancia de SQL Server 2005 Express Edition `SQLEXPRESS` y reside en el mismo equipo que el servidor web; también trata la configuración predeterminada. Si el programa de instalación diferente, deberá adaptar la información de conexión para la base de datos.

Para activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager` control debe colocarse en cualquier lugar en la página (pero dentro del `<form>` elemento):

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample1.aspx)]

A continuación, agregue un origen de datos a la página. Para poder usar una cantidad limitada de datos, se selecciona solo las cinco primeras entradas en la tabla de proveedor de la base de datos AdventureWorks. Si usas el Asistente de Visual Studio para crear el origen de datos, recuerde que un error en la versión actual no prefijo de nombre de la tabla (`Vendor`) con `Purchasing`. El marcado siguiente muestra la sintaxis correcta:

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample2.aspx)]

Recuerde el nombre (identificador) del origen de datos. A continuación, se debe usar esta identificación muy en el `DataSourceID` propiedad del control Accordion:

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample3.aspx)]

En el control Accordion, puede proporcionar plantillas para las distintas partes del control, incluido el encabezado (`<HeaderTemplate>`) y el contenido (`<ContentTemplate>`). Dentro de estos elementos, mandar el resultado de los datos de los datos de origen, utilizando el `DataBinder.Eval()` método:

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample4.aspx)]

Cuando se carga la página, el origen de datos debe estar enlazado a la accordion con este código de servidor:

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample5.aspx)]

Para concluir este ejemplo, debe definir las dos clases CSS que se hace referencia en el control Accordion (en sus propiedades `HeaderCssClass` y `ContentCssClass`). Coloque el siguiente marcado en el `<head>` sección de la página:

[!code-css[Main](databinding-to-an-accordion-vb/samples/sample6.css)]


[![Los datos de la accordion proceden directamente del origen de datos](databinding-to-an-accordion-vb/_static/image2.png)](databinding-to-an-accordion-vb/_static/image1.png)

Los datos de la accordion proceden directamente del origen de datos ([haga clic aquí para ver imagen en tamaño completo](databinding-to-an-accordion-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](dynamically-adding-an-accordion-pane-cs.md)
> [Siguiente](dynamically-adding-an-accordion-pane-vb.md)
