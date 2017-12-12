---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-vb
title: Usar TextBoxWatermark en un FormView (VB) | Documentos de Microsoft
author: wenz
description: El control TextBoxWatermark en el Kit de herramientas de Control de AJAX extiende un cuadro de texto para que se muestre un texto en el cuadro. Cuando un usuario hace clic en el cuadro, lo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 41497361-7fba-4825-b36c-f58d79522a88
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-vb
msc.type: authoredcontent
ms.openlocfilehash: ad75d9729c068f7d512cf076b2ae156291fe76ec
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="using-textboxwatermark-in-a-formview-vb"></a>Usar TextBoxWatermark en un FormView (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark1.vb.zip) o [descarga de PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark1VB.pdf)

> El control TextBoxWatermark en el Kit de herramientas de Control de AJAX extiende un cuadro de texto para que se muestre un texto en el cuadro. Cuando un usuario hace clic en el cuadro, se vacía. Si el usuario deja la casilla sin escribir texto, vuelve a aparecer el texto rellenado previamente. Esto también es posible dentro de un control de FormView.


## <a name="overview"></a>Información general

El `TextBoxWatermark` control en el Kit de herramientas de Control de AJAX extiende un cuadro de texto para que se muestre un texto en el cuadro. Cuando un usuario hace clic en el cuadro, se vacía. Si el usuario deja la casilla sin escribir texto, vuelve a aparecer el texto rellenado previamente. Esto también es posible dentro de un `FormView` control.

## <a name="steps"></a>Pasos

En primer lugar, es necesario un origen de datos. Este ejemplo utiliza la base de datos de AdventureWorks y Microsoft SQL Server 2005 Express Edition. La base de datos es una parte opcional de una instalación de Visual Studio (incluida la edición express) y también está disponible como una descarga independiente en [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). La base de datos de AdventureWorks es parte de los ejemplos de SQL Server 2005 y bases de datos de ejemplo (descargue en [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang = es](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). La manera más fácil de configurar la base de datos es usar Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx? FamilyID = c243a5ae 4bd1 4e3d 94b8 5a0f62bf7796&amp;DisplayLang = es](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) y adjuntar la `AdventureWorks.mdf` archivo de base de datos.

En este ejemplo, se supone que se llama a la instancia de SQL Server 2005 Express Edition `SQLEXPRESS` y reside en el mismo equipo que el servidor web; también es la configuración predeterminada. Si el programa de instalación diferente, tendrá que adaptar la información de conexión para la base de datos.

Para activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager` control se debe colocar en cualquier sitio en la página (pero en el `<form>` elemento):

[!code-aspx[Main](using-textboxwatermark-in-a-formview-vb/samples/sample1.aspx)]

A continuación, agregue un origen de datos a la página que admite la `DELETE`, `INSERT` y `UPDATE` instrucciones SQL. Si está utilizando el Asistente de Visual Studio para crear el origen de datos, recuerde que un error en la versión actual no utiliza ningún prefijo el nombre de tabla (`Vendor`) con `Purchasing`. El marcado siguiente muestra la sintaxis correcta:

[!code-aspx[Main](using-textboxwatermark-in-a-formview-vb/samples/sample2.aspx)]

Recordar el nombre (`ID`) del origen de datos, ya que se usarán en el `DataSourceID` propiedad de la `FormView` control. El `<InsertItemTemplate>` sección de la `FormView` contiene un cuadro de texto que se extiende por la `TextBoxWatermarkExtender` control. Asegúrese de que el dispositivo extender reside dentro de la plantilla y no fuera de él.

[!code-aspx[Main](using-textboxwatermark-in-a-formview-vb/samples/sample3.aspx)]

Ahora cuando el usuario cambia en el modo de inserción de la `FormView` controlar, el campo de texto para el nuevo proveedor se rellenarán automáticamente gracias a la `TextBoxWatermarkExtender` control. Hacer clic en el cuadro de texto permite el texto de relleno desaparecen.


[![Incluye la marca de agua en el campo desde el dispositivo extender](using-textboxwatermark-in-a-formview-vb/_static/image2.png)](using-textboxwatermark-in-a-formview-vb/_static/image1.png)

Incluye la marca de agua en el campo desde el dispositivo extender ([haga clic aquí para ver la imagen a tamaño completo](using-textboxwatermark-in-a-formview-vb/_static/image3.png))

>[!div class="step-by-step"]
[Anterior](using-textboxwatermark-with-validation-controls-cs.md)
[Siguiente](using-textboxwatermark-with-validation-controls-vb.md)
