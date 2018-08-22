---
uid: web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-cs
title: Usar HoverMenu con un Control Repeater (C#) | Microsoft Docs
author: wenz
description: 'El control de HoverMenu en AJAX Control Toolkit proporciona un efecto emergente simple: cuando el puntero del mouse se sitúa sobre un elemento, aparece un mensaje emergente en un específicamente...'
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e7700e7b-edc3-4183-a713-70e507cc7490
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 33f65a132ea9f0a939ab70ac249b397ea883ac8c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829903"
---
<a name="using-hovermenu-with-a-repeater-control-c"></a>Usar HoverMenu con un Control Repeater (C#)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.cs.zip) o [descargar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1CS.pdf)

> El control de HoverMenu en AJAX Control Toolkit proporciona un efecto emergente simple: cuando el puntero del mouse se sitúa sobre un elemento, aparece un mensaje emergente en una posición especificada. También es posible usar este control dentro de un control repeater.


## <a name="overview"></a>Información general

El `HoverMenu` control de AJAX Control Toolkit proporciona un efecto emergente simple: cuando el puntero del mouse se sitúa sobre un elemento, aparece un mensaje emergente en una posición especificada. También es posible usar este control dentro de un control repeater.

## <a name="steps"></a>Pasos

En primer lugar, es necesario un origen de datos. Este ejemplo utiliza la base de datos AdventureWorks y Microsoft SQL Server 2005 Express Edition. La base de datos es una parte opcional de una instalación de Visual Studio (incluida la edición express) y también está disponible como una descarga independiente en [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). La base de datos de AdventureWorks es parte de los ejemplos de SQL Server 2005 y las bases de datos de ejemplo (descargue en [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). La manera más fácil de configurar la base de datos es usar Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) y adjuntar el `AdventureWorks.mdf` el archivo de base de datos.

Para este ejemplo, se supone que se llama a la instancia de SQL Server 2005 Express Edition `SQLEXPRESS` y reside en el mismo equipo que el servidor web; también trata la configuración predeterminada. Si el programa de instalación diferente, deberá adaptar la información de conexión para la base de datos.

Para activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager` control debe colocarse en cualquier lugar en la página (pero dentro del `<form>` elemento):

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample1.aspx)]

A continuación, agregue un origen de datos a la página. Para poder usar una cantidad limitada de datos, se selecciona solo las cinco primeras entradas en la tabla de proveedor de la base de datos AdventureWorks. Si usas el Asistente de Visual Studio para crear el origen de datos, recuerde que un error en la versión actual no prefijo de nombre de la tabla (`Vendor`) con `Purchasing`. El marcado siguiente muestra la sintaxis correcta:

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample2.aspx)]

A continuación, agregue un panel que actúa como el elemento emergente modal:

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample3.aspx)]

Ahora, el `HoverMenuExtender` entra en juego. Para que todos los elementos del origen de datos obtiene su propia ventana emergente, se debe poner el extensor del control Repeater del `<ItemTemplate>` sección. Aquí está el marcado:

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample4.aspx)]

Ahora, todos los elementos del origen de datos muestran un menú emergente a la derecha (`PopupPosition` atributo) después de un retraso de 50 milisegundos (`PopDelay` atributo).


[![El menú de desplazamiento que aparece junto a cada elemento del control de repetidor](using-hovermenu-with-a-repeater-control-cs/_static/image2.png)](using-hovermenu-with-a-repeater-control-cs/_static/image1.png)

El menú de desplazamiento que aparece junto a cada elemento en el control repeater ([haga clic aquí para ver imagen en tamaño completo](using-hovermenu-with-a-repeater-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Siguiente](using-hovermenu-with-a-repeater-control-vb.md)
