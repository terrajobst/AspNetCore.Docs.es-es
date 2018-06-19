---
uid: web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
title: Enlace de datos a Accordion (C#) | Documentos de Microsoft
author: wenz
description: El control Accordion en el Kit de herramientas de Control de AJAX proporciona varios paneles y permite al usuario mostrar uno de ellos al mismo tiempo. Paneles normalmente se declaran w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 9c8f0054-e319-46f8-80c0-35b606d2fbd4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
msc.type: authoredcontent
ms.openlocfilehash: a3ec242c4d5312026ddbc8282ef1b4c3142915a7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878937"
---
<a name="databinding-to-an-accordion-c"></a>Enlace de datos a Accordion (C#)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.cs.zip) o [descarga de PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1CS.pdf)

> El control Accordion en el Kit de herramientas de Control de AJAX proporciona varios paneles y permite al usuario mostrar uno de ellos al mismo tiempo. Paneles normalmente se declaran dentro de la propia página, pero el enlace a un origen de datos ofrece más flexibilidad.


## <a name="overview"></a>Información general

El control Accordion en el Kit de herramientas de Control de AJAX proporciona varios paneles y permite al usuario mostrar uno de ellos al mismo tiempo. Paneles normalmente se declaran dentro de la propia página, pero el enlace a un origen de datos ofrece más flexibilidad.

## <a name="steps"></a>Pasos

En primer lugar, es necesario un origen de datos. Este ejemplo utiliza la base de datos de AdventureWorks y Microsoft SQL Server 2005 Express Edition. La base de datos es una parte opcional de una instalación de Visual Studio (incluida la edición express) y también está disponible como una descarga independiente en [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). La base de datos de AdventureWorks es parte de los ejemplos de SQL Server 2005 y bases de datos de ejemplo (descargue en [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = es](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). La manera más fácil de configurar la base de datos es usar Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = es](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) y adjuntar la `AdventureWorks.mdf` archivo de base de datos.

En este ejemplo, se supone que se llama a la instancia de SQL Server 2005 Express Edition `SQLEXPRESS` y reside en el mismo equipo que el servidor web; también es la configuración predeterminada. Si el programa de instalación diferente, tendrá que adaptar la información de conexión para la base de datos.

Para activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager` control se debe colocar en cualquier sitio en la página (pero en el `<form>` elemento):

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample1.aspx)]

A continuación, agregue un origen de datos a la página. Para poder usar una cantidad limitada de datos, se selecciona solo las primeras cinco entradas en la tabla de proveedor de la base de datos de AdventureWorks. Si está utilizando el Asistente de Visual Studio para crear el origen de datos, recuerde que un error en la versión actual no utiliza ningún prefijo el nombre de tabla (`Vendor`) con `Purchasing`. El marcado siguiente muestra la sintaxis correcta:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample2.aspx)]

Recuerde el nombre (ID) del origen de datos. Esta identificación muy debe utilizarse, a continuación, en la `DataSourceID` propiedad del control Accordion:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample3.aspx)]

En el control Accordion, puede proporcionar plantillas para distintas partes del control, incluyendo el encabezado (`<HeaderTemplate>`) y el contenido (`<ContentTemplate>`). Dentro de estos elementos, simplemente extraer los datos de los datos de origen, con el `DataBinder.Eval()` método:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample4.aspx)]

Cuando se carga la página, el origen de datos debe estar enlazado a la accordion con este código de servidor:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample5.aspx)]

Para finalizar este ejemplo, debe definir las dos clases CSS que se hace referencia en el control Accordion (en sus propiedades `HeaderCssClass` y `ContentCssClass`). Coloque el siguiente marcado en la `<head>` sección de la página:

[!code-css[Main](databinding-to-an-accordion-cs/samples/sample6.css)]


[![Los datos de la accordion proceden directamente desde el origen de datos](databinding-to-an-accordion-cs/_static/image2.png)](databinding-to-an-accordion-cs/_static/image1.png)

Los datos de la accordion proceden directamente desde el origen de datos ([haga clic aquí para ver la imagen a tamaño completo](databinding-to-an-accordion-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Siguiente](dynamically-adding-an-accordion-pane-cs.md)
