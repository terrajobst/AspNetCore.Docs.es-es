---
uid: web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
title: Enlace de datos a Accordion (C#) | Microsoft Docs
author: wenz
description: El control Accordion de AJAX Control Toolkit ofrece varios paneles y permite al usuario mostrar uno de ellos a la vez. Los paneles se declaran normalmente w...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 9c8f0054-e319-46f8-80c0-35b606d2fbd4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
msc.type: authoredcontent
ms.openlocfilehash: 05adc7158725bd5a6ba276b81222de04158d3c64
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833545"
---
<a name="databinding-to-an-accordion-c"></a>Enlace de datos a Accordion (C#)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.cs.zip) o [descargar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1CS.pdf)

> El control Accordion de AJAX Control Toolkit ofrece varios paneles y permite al usuario mostrar uno de ellos a la vez. Paneles normalmente se declaran dentro de la propia página, pero el enlace a un origen de datos ofrece más flexibilidad.


## <a name="overview"></a>Información general

El control Accordion de AJAX Control Toolkit ofrece varios paneles y permite al usuario mostrar uno de ellos a la vez. Paneles normalmente se declaran dentro de la propia página, pero el enlace a un origen de datos ofrece más flexibilidad.

## <a name="steps"></a>Pasos

En primer lugar, es necesario un origen de datos. Este ejemplo utiliza la base de datos AdventureWorks y Microsoft SQL Server 2005 Express Edition. La base de datos es una parte opcional de una instalación de Visual Studio (incluida la edición express) y también está disponible como una descarga independiente en [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). La base de datos de AdventureWorks es parte de los ejemplos de SQL Server 2005 y las bases de datos de ejemplo (descargue en [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). La manera más fácil de configurar la base de datos es usar Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) y adjuntar el `AdventureWorks.mdf` el archivo de base de datos.

Para este ejemplo, se supone que se llama a la instancia de SQL Server 2005 Express Edition `SQLEXPRESS` y reside en el mismo equipo que el servidor web; también trata la configuración predeterminada. Si el programa de instalación diferente, deberá adaptar la información de conexión para la base de datos.

Para activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager` control debe colocarse en cualquier lugar en la página (pero dentro del `<form>` elemento):

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample1.aspx)]

A continuación, agregue un origen de datos a la página. Para poder usar una cantidad limitada de datos, se selecciona solo las cinco primeras entradas en la tabla de proveedor de la base de datos AdventureWorks. Si usas el Asistente de Visual Studio para crear el origen de datos, recuerde que un error en la versión actual no prefijo de nombre de la tabla (`Vendor`) con `Purchasing`. El marcado siguiente muestra la sintaxis correcta:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample2.aspx)]

Recuerde el nombre (identificador) del origen de datos. A continuación, se debe usar esta identificación muy en el `DataSourceID` propiedad del control Accordion:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample3.aspx)]

En el control Accordion, puede proporcionar plantillas para las distintas partes del control, incluido el encabezado (`<HeaderTemplate>`) y el contenido (`<ContentTemplate>`). Dentro de estos elementos, mandar el resultado de los datos de los datos de origen, utilizando el `DataBinder.Eval()` método:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample4.aspx)]

Cuando se carga la página, el origen de datos debe estar enlazado a la accordion con este código de servidor:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample5.aspx)]

Para concluir este ejemplo, debe definir las dos clases CSS que se hace referencia en el control Accordion (en sus propiedades `HeaderCssClass` y `ContentCssClass`). Coloque el siguiente marcado en el `<head>` sección de la página:

[!code-css[Main](databinding-to-an-accordion-cs/samples/sample6.css)]


[![Los datos de la accordion proceden directamente del origen de datos](databinding-to-an-accordion-cs/_static/image2.png)](databinding-to-an-accordion-cs/_static/image1.png)

Los datos de la accordion proceden directamente del origen de datos ([haga clic aquí para ver imagen en tamaño completo](databinding-to-an-accordion-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Siguiente](dynamically-adding-an-accordion-pane-cs.md)
