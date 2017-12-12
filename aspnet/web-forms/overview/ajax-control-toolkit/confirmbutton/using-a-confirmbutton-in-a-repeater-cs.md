---
uid: web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-cs
title: Usar un ConfirmButton en un repetidor (C#) | Documentos de Microsoft
author: wenz
description: "El extensor de ConfirmButton en el Kit de herramientas de Control de AJAX crea Yes/No emergente cuando el usuario hace clic en un botón (incluido control LinkButton). Solo si sí es..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a973ed3e-400c-4925-ace2-0b086b479301
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-cs
msc.type: authoredcontent
ms.openlocfilehash: 12fd3e4e58e6a73720ade38372caff496f7f2c97
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="using-a-confirmbutton-in-a-repeater-c"></a>Usar un ConfirmButton en un repetidor (C#)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.cs.zip) o [descarga de PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1CS.pdf)

> El extensor de ConfirmButton en el Kit de herramientas de Control de AJAX crea Yes/No emergente cuando el usuario hace clic en un botón (incluido control LinkButton). Solo si se hace clic en Sí, se ejecuta la acción del botón, en caso contrario, se cancela. Esto también es posible en un repetidor.


## <a name="overview"></a>Información general

El extensor de ConfirmButton en el Kit de herramientas de Control de AJAX crea Yes/No emergente cuando el usuario hace clic en un botón (incluido control LinkButton). Solo si se hace clic en Sí, se ejecuta la acción del botón, en caso contrario, se cancela. Esto también es posible en un repetidor.

## <a name="steps"></a>Pasos

En primer lugar, es necesario un origen de datos. Este ejemplo utiliza la base de datos de AdventureWorks y Microsoft SQL Server 2005 Express Edition. La base de datos es una parte opcional de una instalación de Visual Studio (incluida la edición express) y también está disponible como una descarga independiente en [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). La base de datos de AdventureWorks es parte de los ejemplos de SQL Server 2005 y bases de datos de ejemplo (descargue en [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang = es](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). La manera más fácil de configurar la base de datos es usar Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx? FamilyID = c243a5ae 4bd1 4e3d 94b8 5a0f62bf7796&amp;DisplayLang = es](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) y adjuntar la `AdventureWorks.mdf` archivo de base de datos.

En este ejemplo, se supone que se llama a la instancia de SQL Server 2005 Express Edition `SQLEXPRESS` y reside en el mismo equipo que el servidor web; también es la configuración predeterminada. Si el programa de instalación diferente, tendrá que adaptar la información de conexión para la base de datos.

Para activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager` control se debe colocar en cualquier sitio en la página (pero en el `<form>` elemento):

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample1.aspx)]

A continuación, es necesario un origen de datos. Por simplicidad, se recuperan sólo las primeras cinco entradas de tabla de proveedores de AdventureWorks. Tenga en cuenta que, cuando utilice el Asistente de Visual Studio para crear el origen de datos, el nombre de tabla (`Vendors`) está actualmente sin correctamente el prefijo `Purchasing`. El siguiente marcado es la correcta:

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample2.aspx)]

Este origen de datos, a continuación, puede usarse en un repetidor. Como es habitual, el `DataBinder.Eval()` método recupera datos del origen de datos. El `ConfirmButtonExtender` , a continuación, se debe colocar el control dentro de la `<ItemTemplate>` sección del repetidor para que aparezca para cada entrada del origen de datos.

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample3.aspx)]


[![El botón Confirmar aparece junto a cada entrada del origen de datos](using-a-confirmbutton-in-a-repeater-cs/_static/image2.png)](using-a-confirmbutton-in-a-repeater-cs/_static/image1.png)

El botón Confirmar aparece junto a cada entrada del origen de datos ([haga clic aquí para ver la imagen a tamaño completo](using-a-confirmbutton-in-a-repeater-cs/_static/image3.png))

>[!div class="step-by-step"]
[Siguiente](using-a-confirmbutton-in-a-repeater-vb.md)
