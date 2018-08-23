---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-vb
title: Usar TextBoxWatermark en un FormView (VB) | Microsoft Docs
author: wenz
description: El control TextBoxWatermark de AJAX Control Toolkit amplía un cuadro de texto para que se muestra un texto dentro del cuadro. Cuando un usuario hace clic en el cuadro, lo...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 41497361-7fba-4825-b36c-f58d79522a88
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-vb
msc.type: authoredcontent
ms.openlocfilehash: b9a731be89d0582ebd411809a6bbcbee801fea2a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829076"
---
<a name="using-textboxwatermark-in-a-formview-vb"></a>Usar TextBoxWatermark en un FormView (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark1.vb.zip) o [descargar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark1VB.pdf)

> El control TextBoxWatermark de AJAX Control Toolkit amplía un cuadro de texto para que se muestra un texto dentro del cuadro. Cuando un usuario hace clic en el cuadro, se vacía. Si el usuario deja la casilla sin escribir texto, vuelve a aparecer el texto rellenado previamente. Esto también es posible dentro de un control FormView.


## <a name="overview"></a>Información general

El `TextBoxWatermark` control de AJAX Control Toolkit amplía un cuadro de texto para que se muestra un texto dentro del cuadro. Cuando un usuario hace clic en el cuadro, se vacía. Si el usuario deja la casilla sin escribir texto, vuelve a aparecer el texto rellenado previamente. Esto también es posible dentro de un `FormView` control.

## <a name="steps"></a>Pasos

En primer lugar, es necesario un origen de datos. Este ejemplo utiliza la base de datos AdventureWorks y Microsoft SQL Server 2005 Express Edition. La base de datos es una parte opcional de una instalación de Visual Studio (incluida la edición express) y también está disponible como una descarga independiente en [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). La base de datos de AdventureWorks es parte de los ejemplos de SQL Server 2005 y las bases de datos de ejemplo (descargue en [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). La manera más fácil de configurar la base de datos es usar Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) y adjuntar el `AdventureWorks.mdf` el archivo de base de datos.

Para este ejemplo, se supone que se llama a la instancia de SQL Server 2005 Express Edition `SQLEXPRESS` y reside en el mismo equipo que el servidor web; también trata la configuración predeterminada. Si el programa de instalación diferente, deberá adaptar la información de conexión para la base de datos.

Para activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager` control debe colocarse en cualquier lugar en la página (pero dentro del `<form>` elemento):

[!code-aspx[Main](using-textboxwatermark-in-a-formview-vb/samples/sample1.aspx)]

A continuación, agregue un origen de datos a la página que es compatible con la `DELETE`, `INSERT` y `UPDATE` instrucciones SQL. Si usas el Asistente de Visual Studio para crear el origen de datos, recuerde que un error en la versión actual no prefijo de nombre de la tabla (`Vendor`) con `Purchasing`. El marcado siguiente muestra la sintaxis correcta:

[!code-aspx[Main](using-textboxwatermark-in-a-formview-vb/samples/sample2.aspx)]

Recuerde el nombre (`ID`) del origen de datos, ya que se usará en el `DataSourceID` propiedad de la `FormView` control. El `<InsertItemTemplate>` sección de la `FormView` contiene un cuadro de texto que se extiende por la `TextBoxWatermarkExtender` control. Asegúrese de que el extensor reside dentro de la plantilla y no fuera de él.

[!code-aspx[Main](using-textboxwatermark-in-a-formview-vb/samples/sample3.aspx)]

Ahora cuando el usuario cambia al modo de inserción de la `FormView` controlar, el campo de texto para el nuevo proveedor se rellenarán automáticamente gracias a la `TextBoxWatermarkExtender` control. Un clic en el cuadro de texto permite que el texto de relleno desaparecen.


[![La marca de agua en el campo procede el extensor](using-textboxwatermark-in-a-formview-vb/_static/image2.png)](using-textboxwatermark-in-a-formview-vb/_static/image1.png)

La marca de agua en el campo procede el extensor ([haga clic aquí para ver imagen en tamaño completo](using-textboxwatermark-in-a-formview-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](using-textboxwatermark-with-validation-controls-cs.md)
> [Siguiente](using-textboxwatermark-with-validation-controls-vb.md)
