---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
title: Arrastrar y colocar mediante ReorderList (VB) | Microsoft Docs
author: wenz
description: /Data-Access/tutorials/Master-Detail-Using-a-bulleted-List-of-master-Records-with-a-Details-DataList-vb
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 848e6bcf-4c3f-4d14-974d-e45b9444ab79
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 12c5aa60aa6bb71c99e267a1a71b40ed718df8cf
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837951"
---
<a name="drag-and-drop-via-reorderlist-vb"></a>Arrastrar y colocar mediante ReorderList (VB)
====================
por [Christian Wenz](https://github.com/wenz)

[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip) o [descargar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)

> El control ReorderList en AJAX Control Toolkit proporciona una lista que se puede reordenar el usuario mediante arrastrar y colocar. El orden actual de la lista deberá conservarse en el servidor.


## <a name="overview"></a>Información general

El `ReorderList` control de AJAX Control Toolkit proporciona una lista que se puede reordenar el usuario mediante arrastrar y colocar. El orden actual de la lista deberá conservarse en el servidor.

## <a name="steps"></a>Pasos

El `ReorderList` control admite el enlace de datos de una base de datos a la lista. Lo mejor de todo, también admite cambios de escritura al orden del elemento de lista en el almacén de datos.

Este ejemplo utiliza Microsoft SQL Server 2005 Express Edition como almacén de datos. La base de datos es una parte opcional (y gratuita) de una instalación de Visual Studio, incluida la edición express. También está disponible como descarga independiente en [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Para este ejemplo, se supone que se llama a la instancia de SQL Server 2005 Express Edition `SQLEXPRESS` y reside en el mismo equipo que el servidor web; también trata la configuración predeterminada. Si el programa de instalación diferente, deberá adaptar la información de conexión para la base de datos.

La manera más fácil de configurar la base de datos es usar Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ). Conectarse al servidor, haga doble clic en `Databases` y crear una nueva base de datos (haga clic en y elija `New Database`) llamado `Tutorials`.

En esta base de datos, cree una nueva tabla denominada `AJAX` con las cuatro columnas siguientes:

- `id` (entero de clave, principal, identidad, no es NULL)
- `char` (char (1), NULL)
- `description` (varchar (50), NULL)
- `position` (int, NULL)


[![El diseño de la tabla de AJAX](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)

El diseño de la tabla de AJAX ([haga clic aquí para ver imagen en tamaño completo](drag-and-drop-via-reorderlist-vb/_static/image3.png))


A continuación, rellene la tabla con un par de valores. Tenga en cuenta que la `position` columna contiene el criterio de ordenación de los elementos.


[![Los datos iniciales en la tabla de AJAX](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)

Los datos iniciales en la tabla de AJAX ([haga clic aquí para ver imagen en tamaño completo](drag-and-drop-via-reorderlist-vb/_static/image6.png))


El siguiente paso se requiere para generar un `SqlDataSource` control para comunicarse con la nueva base de datos y su tabla. El origen de datos debe admitir la `SELECT` y `UPDATE` comandos SQL. Cuando posteriormente se cambia el orden de los elementos de lista, el `ReorderList` control envía automáticamente los dos valores para el origen de datos `Update` comando: la nueva posición y el identificador del elemento. Por lo tanto, las necesidades del origen de datos un `<UpdateParameters>` sección para estos dos valores:

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample1.aspx)]

El `ReorderList` control necesita establecer los atributos siguientes:

- `AllowReorder`: Determina si se pueden reorganizar los elementos de lista
- `DataSourceID`: El identificador del origen de datos
- `DataKeyField`: El nombre de la columna de clave principal en el origen de datos
- `SortOrderField`: La columna de origen de datos que proporciona el criterio de ordenación para los elementos de lista

En el `<DragHandleTemplate>` y `<ItemTemplate>` secciones, el diseño de la lista puede ajustarse. Además, el enlace de datos es posible mediante la `Eval()` método, tal como se muestra aquí:

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample2.aspx)]

Información de estilo de CSS siguientes (que se hace referencia en el `<DragHandleTemplate>` sección de la `ReorderList` control) garantiza que el puntero del mouse cambia correctamente cuando se mantiene el mouse sobre el controlador de arrastre:

[!code-css[Main](drag-and-drop-via-reorderlist-vb/samples/sample3.css)]

Por último, un `ScriptManager` control inicializa AJAX de ASP.NET para la página:

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample4.aspx)]

Ejecutar este ejemplo en el explorador y reorganizar los elementos de lista un poco. A continuación, vuelva a cargar la página o eche un vistazo a la base de datos. Las posiciones modificadas se haya mantenido y también se reflejan en los valores de la `position` columna en la base de datos y todo ello sin ningún código, simplemente mediante el uso de marcado.


[![Los datos de los cambios de la base de datos según el orden de elemento de lista nuevo](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)

Elemento de datos en la base de datos cambia según la nueva lista de orden ([haga clic aquí para ver imagen en tamaño completo](drag-and-drop-via-reorderlist-vb/_static/image9.png))

> [!div class="step-by-step"]
> [Anterior](using-postbacks-with-reorderlist-vb.md)
