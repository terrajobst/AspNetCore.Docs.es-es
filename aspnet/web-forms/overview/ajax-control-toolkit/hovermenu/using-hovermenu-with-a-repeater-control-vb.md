---
uid: web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-vb
title: Usar HoverMenu con un Control de repetidor (VB) | Documentos de Microsoft
author: wenz
description: 'El control HoverMenu en el Kit de herramientas de Control de AJAX proporciona un efecto emergente simple: cuando el puntero del mouse se desplace sobre un elemento, aparece un mensaje emergente en una específicamente...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 7f07c112-cd4f-4427-9699-57cfab2791fd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: aa107a7483683d965f3a510e6a43f174a386da0c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868404"
---
<a name="using-hovermenu-with-a-repeater-control-vb"></a>Usar HoverMenu con un Control de repetidor (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.vb.zip) o [descarga de PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1VB.pdf)

> El control HoverMenu en el Kit de herramientas de Control de AJAX proporciona un efecto emergente simple: cuando el puntero del mouse se desplace sobre un elemento, aparece un mensaje emergente en una posición especificada. También es posible utilizar este control dentro de un repetidor.


## <a name="overview"></a>Información general

El `HoverMenu` control en el Kit de herramientas de Control de AJAX proporciona un efecto emergente simple: cuando el puntero del mouse se desplace sobre un elemento, aparece un mensaje emergente en una posición especificada. También es posible utilizar este control dentro de un repetidor.

## <a name="steps"></a>Pasos

En primer lugar, es necesario un origen de datos. Este ejemplo utiliza la base de datos de AdventureWorks y Microsoft SQL Server 2005 Express Edition. La base de datos es una parte opcional de una instalación de Visual Studio (incluida la edición express) y también está disponible como una descarga independiente en [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). La base de datos de AdventureWorks es parte de los ejemplos de SQL Server 2005 y bases de datos de ejemplo (descargue en [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = es](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). La manera más fácil de configurar la base de datos es usar Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = es](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) y adjuntar la `AdventureWorks.mdf` archivo de base de datos.

En este ejemplo, se supone que se llama a la instancia de SQL Server 2005 Express Edition `SQLEXPRESS` y reside en el mismo equipo que el servidor web; también es la configuración predeterminada. Si el programa de instalación diferente, tendrá que adaptar la información de conexión para la base de datos.

Para activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager` control se debe colocar en cualquier sitio en la página (pero en el `<form>` elemento):

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample1.aspx)]

A continuación, agregue un origen de datos a la página. Para poder usar una cantidad limitada de datos, se selecciona solo las primeras cinco entradas en la tabla de proveedor de la base de datos de AdventureWorks. Si está utilizando el Asistente de Visual Studio para crear el origen de datos, recuerde que un error en la versión actual no utiliza ningún prefijo el nombre de tabla (`Vendor`) con `Purchasing`. El marcado siguiente muestra la sintaxis correcta:

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample2.aspx)]

A continuación, agregue un panel que actúa como el elemento emergente modal:

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample3.aspx)]

Ahora, la `HoverMenuExtender` entra en juego. Para que todos los elementos del origen de datos obtiene su propia ventana emergente, el dispositivo extender debe situarse dentro del repetidor `<ItemTemplate>` sección. Aquí está el marcado:

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample4.aspx)]

Ahora, todos los elementos del origen de datos muestran un menú emergente a la derecha (`PopupPosition` atributo) después de un retraso de 50 milisegundos (`PopDelay` atributo).


[![El menú de desplazamiento que aparece junto a cada elemento del repetidor](using-hovermenu-with-a-repeater-control-vb/_static/image2.png)](using-hovermenu-with-a-repeater-control-vb/_static/image1.png)

El menú de desplazamiento que aparece junto a cada elemento del Repetidor ([haga clic aquí para ver la imagen a tamaño completo](using-hovermenu-with-a-repeater-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](using-hovermenu-with-a-repeater-control-cs.md)
