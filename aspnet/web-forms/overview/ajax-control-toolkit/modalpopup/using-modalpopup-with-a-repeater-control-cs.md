---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-cs
title: Usar ModalPopup con un Control Repeater (C#) | Microsoft Docs
author: wenz
description: El control ModalPopup de AJAX Control Toolkit ofrece una manera sencilla de crear un elemento emergente modal mediante medios de lado cliente. También es posible usar este contr...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: d686d84a-1c58-492e-8a77-3eb5a0cfe918
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: c2ffa22fb8d9d985d99d9f8d2298d317e6caf469
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829426"
---
<a name="using-modalpopup-with-a-repeater-control-c"></a>Usar ModalPopup con un Control Repeater (C#)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup2.cs.zip) o [descargar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup2CS.pdf)

> El control ModalPopup de AJAX Control Toolkit ofrece una manera sencilla de crear un elemento emergente modal mediante medios de lado cliente. También es posible usar este control dentro de un control repeater.


## <a name="overview"></a>Información general

El control ModalPopup de AJAX Control Toolkit ofrece una manera sencilla de crear un elemento emergente modal mediante medios de lado cliente. También es posible usar este control dentro de un control repeater.

## <a name="steps"></a>Pasos

En primer lugar, es necesario un origen de datos. Este ejemplo utiliza la base de datos AdventureWorks y Microsoft SQL Server 2005 Express Edition. La base de datos es una parte opcional de una instalación de Visual Studio (incluida la edición express) y también está disponible como una descarga independiente en [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). La base de datos de AdventureWorks es parte de los ejemplos de SQL Server 2005 y las bases de datos de ejemplo (descargue en [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). La manera más fácil de configurar la base de datos es usar Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) y adjuntar el `AdventureWorks.mdf` el archivo de base de datos. Para este ejemplo, se supone que se llama a la instancia de SQL Server 2005 Express Edition `SQLEXPRESS` y reside en el mismo equipo que el servidor web; también trata la configuración predeterminada. Si el programa de instalación diferente, deberá adaptar la información de conexión para la base de datos. Para activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager` control debe colocarse en cualquier lugar en la página (pero dentro del `<form>` elemento):

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample1.aspx)]

A continuación, agregue un origen de datos a la página. Para poder usar una cantidad limitada de datos, se selecciona solo las cinco primeras entradas en la tabla de proveedor de la base de datos AdventureWorks. Si usas el Asistente de Visual Studio para crear el origen de datos, recuerde que un error en la versión actual no prefijo de nombre de la tabla (`Vendor`) con `Purchasing`. El marcado siguiente muestra la sintaxis correcta:

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample2.aspx)]

A continuación, agregue un panel que actúa como el elemento emergente modal. Contiene un `Button` control para cerrar la ventana emergente nuevo:

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample3.aspx)]

Con el fin de que el elemento emergente funcione del control Repeater, el `ModalPopupExtender` control debe colocarse dentro de la `<ItemTemplate>` sección del repetidor. Por lo que el panel está fuera del control repeater, pero el extensor está dentro. Aquí está el marcado para el control repeater:

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample4.aspx)]

A continuación, se muestra cada elemento del origen de datos con un botón junto a él que desencadena el elemento emergente modal.


[![El elemento emergente modal puede activarse para cada entrada del origen de datos](using-modalpopup-with-a-repeater-control-cs/_static/image2.png)](using-modalpopup-with-a-repeater-control-cs/_static/image1.png)

El elemento emergente modal puede activarse para cada entrada del origen de datos ([haga clic aquí para ver imagen en tamaño completo](using-modalpopup-with-a-repeater-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](launching-a-modal-popup-window-from-server-code-cs.md)
> [Siguiente](handling-postbacks-from-a-modalpopup-cs.md)
