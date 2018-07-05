---
uid: web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-vb
title: Usar un ConfirmButton en un control Repeater (VB) | Microsoft Docs
author: wenz
description: El extensor ConfirmButton de AJAX Control Toolkit crea un Sí o ningún elemento emergente cuando el usuario hace clic en un botón (incluido el control LinkButton). Solo si sí es...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 18c31709-3f9d-4d93-8b01-f1356bf610b4
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-vb
msc.type: authoredcontent
ms.openlocfilehash: ce403e84766f586eca36ef6bc513d9fbf7bd1d40
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396424"
---
<a name="using-a-confirmbutton-in-a-repeater-vb"></a>Usar un ConfirmButton en un control Repeater (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.vb.zip) o [descargar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1VB.pdf)

> El extensor ConfirmButton de AJAX Control Toolkit crea un Sí o ningún elemento emergente cuando el usuario hace clic en un botón (incluido el control LinkButton). Solo si se hace clic en Sí, se ejecuta la acción del botón, en caso contrario, se ha cancelado. Esto también es posible en un control repeater.


## <a name="overview"></a>Información general

El extensor ConfirmButton de AJAX Control Toolkit crea un Sí o ningún elemento emergente cuando el usuario hace clic en un botón (incluido el control LinkButton). Solo si se hace clic en Sí, se ejecuta la acción del botón, en caso contrario, se ha cancelado. Esto también es posible en un control repeater.

## <a name="steps"></a>Pasos

En primer lugar, es necesario un origen de datos. Este ejemplo utiliza la base de datos AdventureWorks y Microsoft SQL Server 2005 Express Edition. La base de datos es una parte opcional de una instalación de Visual Studio (incluida la edición express) y también está disponible como una descarga independiente en [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). La base de datos de AdventureWorks es parte de los ejemplos de SQL Server 2005 y las bases de datos de ejemplo (descargue en [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). La manera más fácil de configurar la base de datos es usar Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) y adjuntar el `AdventureWorks.mdf` el archivo de base de datos.

Para este ejemplo, se supone que se llama a la instancia de SQL Server 2005 Express Edition `SQLEXPRESS` y reside en el mismo equipo que el servidor web; también trata la configuración predeterminada. Si el programa de instalación diferente, deberá adaptar la información de conexión para la base de datos.

Para activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager` control debe colocarse en cualquier lugar en la página (pero dentro del `<form>` elemento):

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample1.aspx)]

A continuación, se requiere un origen de datos. Por simplicidad, se recuperan solo las primeras cinco entradas de tabla de proveedores de AdventureWorks. Tenga en cuenta que al usar el Asistente de Visual Studio para crear el origen de datos, el nombre de tabla (`Vendors`) es actualmente no incluyen el prefijo correctamente con `Purchasing`. El siguiente marcado es la correcta:

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample2.aspx)]

A continuación, se puede usar este origen de datos dentro de un control repeater. Como es habitual, el `DataBinder.Eval()` método recupera datos del origen de datos. El `ConfirmButtonExtender` control debe colocarse dentro de la `<ItemTemplate>` sección del repetidor para que aparezca para cada entrada en el origen de datos.

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample3.aspx)]


[![El botón Confirmar aparece junto a cada entrada del origen de datos](using-a-confirmbutton-in-a-repeater-vb/_static/image2.png)](using-a-confirmbutton-in-a-repeater-vb/_static/image1.png)

El botón Confirmar aparece junto a cada entrada del origen de datos ([haga clic aquí para ver imagen en tamaño completo](using-a-confirmbutton-in-a-repeater-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](using-a-confirmbutton-in-a-repeater-cs.md)
