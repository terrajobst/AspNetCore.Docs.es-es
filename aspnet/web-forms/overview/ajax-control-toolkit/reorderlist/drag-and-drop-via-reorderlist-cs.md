---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-cs
title: Arrastrar y colocar mediante ReorderList (C#) | Microsoft Docs
author: wenz
description: El control ReorderList en AJAX Control Toolkit proporciona una lista que se puede reordenar el usuario mediante arrastrar y colocar. Será el orden actual de la lista...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 6350ee8e-11d6-4aff-b51c-942878014835
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-cs
msc.type: authoredcontent
ms.openlocfilehash: 59f88787e7794e9c7bd391ae3e8cea538b446f7d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389116"
---
<a name="drag-and-drop-via-reorderlist-c"></a><span data-ttu-id="d1a9b-104">Arrastrar y colocar mediante ReorderList (C#)</span><span class="sxs-lookup"><span data-stu-id="d1a9b-104">Drag and Drop via ReorderList (C#)</span></span>
====================
<span data-ttu-id="d1a9b-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="d1a9b-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="d1a9b-106">[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.cs.zip) o [descargar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="d1a9b-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5CS.pdf)</span></span>

> <span data-ttu-id="d1a9b-107">El control ReorderList en AJAX Control Toolkit proporciona una lista que se puede reordenar el usuario mediante arrastrar y colocar.</span><span class="sxs-lookup"><span data-stu-id="d1a9b-107">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="d1a9b-108">El orden actual de la lista deberá conservarse en el servidor.</span><span class="sxs-lookup"><span data-stu-id="d1a9b-108">The current order of the list shall be persisted on the server.</span></span>


## <a name="overview"></a><span data-ttu-id="d1a9b-109">Información general</span><span class="sxs-lookup"><span data-stu-id="d1a9b-109">Overview</span></span>

<span data-ttu-id="d1a9b-110">El `ReorderList` control de AJAX Control Toolkit proporciona una lista que se puede reordenar el usuario mediante arrastrar y colocar.</span><span class="sxs-lookup"><span data-stu-id="d1a9b-110">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="d1a9b-111">El orden actual de la lista deberá conservarse en el servidor.</span><span class="sxs-lookup"><span data-stu-id="d1a9b-111">The current order of the list shall be persisted on the server.</span></span>

## <a name="steps"></a><span data-ttu-id="d1a9b-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="d1a9b-112">Steps</span></span>

<span data-ttu-id="d1a9b-113">El `ReorderList` control admite el enlace de datos de una base de datos a la lista.</span><span class="sxs-lookup"><span data-stu-id="d1a9b-113">The `ReorderList` control supports binding data from a database to the list.</span></span> <span data-ttu-id="d1a9b-114">Lo mejor de todo, también admite cambios de escritura al orden del elemento de lista en el almacén de datos.</span><span class="sxs-lookup"><span data-stu-id="d1a9b-114">Best of all, it also supports writing changes to the order of the list element back to the data store.</span></span>

<span data-ttu-id="d1a9b-115">Este ejemplo utiliza Microsoft SQL Server 2005 Express Edition como almacén de datos.</span><span class="sxs-lookup"><span data-stu-id="d1a9b-115">This sample uses Microsoft SQL Server 2005 Express Edition as the data store.</span></span> <span data-ttu-id="d1a9b-116">La base de datos es una parte opcional (y gratuita) de una instalación de Visual Studio, incluida la edición express.</span><span class="sxs-lookup"><span data-stu-id="d1a9b-116">The database is an optional (and free) part of a Visual Studio installation, including express edition.</span></span> <span data-ttu-id="d1a9b-117">También está disponible como descarga independiente en [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="d1a9b-117">It is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="d1a9b-118">Para este ejemplo, se supone que se llama a la instancia de SQL Server 2005 Express Edition `SQLEXPRESS` y reside en el mismo equipo que el servidor web; también trata la configuración predeterminada.</span><span class="sxs-lookup"><span data-stu-id="d1a9b-118">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="d1a9b-119">Si el programa de instalación diferente, deberá adaptar la información de conexión para la base de datos.</span><span class="sxs-lookup"><span data-stu-id="d1a9b-119">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="d1a9b-120">La manera más fácil de configurar la base de datos es usar Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span><span class="sxs-lookup"><span data-stu-id="d1a9b-120">The easiest way to set up the database is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span></span> <span data-ttu-id="d1a9b-121">Conectarse al servidor, haga doble clic en `Databases` y crear una nueva base de datos (haga clic en y elija `New Database`) llamado `Tutorials`.</span><span class="sxs-lookup"><span data-stu-id="d1a9b-121">Connect to the server, double-click on `Databases` and create a new database (right-click and choose `New Database`) called `Tutorials`.</span></span>

<span data-ttu-id="d1a9b-122">En esta base de datos, cree una nueva tabla denominada `AJAX` con las cuatro columnas siguientes:</span><span class="sxs-lookup"><span data-stu-id="d1a9b-122">In this database, create a new table called `AJAX` with the following four columns:</span></span>

- <span data-ttu-id="d1a9b-123">`id` (entero de clave, principal, identidad, no es NULL)</span><span class="sxs-lookup"><span data-stu-id="d1a9b-123">`id` (primary key, integer, identity, not NULL)</span></span>
- <span data-ttu-id="d1a9b-124">`char` (char (1), NULL)</span><span class="sxs-lookup"><span data-stu-id="d1a9b-124">`char` (char(1), NULL)</span></span>
- <span data-ttu-id="d1a9b-125">`description` (varchar (50), NULL)</span><span class="sxs-lookup"><span data-stu-id="d1a9b-125">`description` (varchar(50), NULL)</span></span>
- <span data-ttu-id="d1a9b-126">`position` (int, NULL)</span><span class="sxs-lookup"><span data-stu-id="d1a9b-126">`position` (int, NULL)</span></span>


<span data-ttu-id="d1a9b-127">[![El diseño de la tabla de AJAX](drag-and-drop-via-reorderlist-cs/_static/image2.png)](drag-and-drop-via-reorderlist-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d1a9b-127">[![The layout of the AJAX table](drag-and-drop-via-reorderlist-cs/_static/image2.png)](drag-and-drop-via-reorderlist-cs/_static/image1.png)</span></span>

<span data-ttu-id="d1a9b-128">El diseño de la tabla de AJAX ([haga clic aquí para ver imagen en tamaño completo](drag-and-drop-via-reorderlist-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d1a9b-128">The layout of the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-cs/_static/image3.png))</span></span>


<span data-ttu-id="d1a9b-129">A continuación, rellene la tabla con un par de valores.</span><span class="sxs-lookup"><span data-stu-id="d1a9b-129">Next, fill the table with a couple of values.</span></span> <span data-ttu-id="d1a9b-130">Tenga en cuenta que la `position` columna contiene el criterio de ordenación de los elementos.</span><span class="sxs-lookup"><span data-stu-id="d1a9b-130">Note that the `position` column holds the sort order of the elements.</span></span>


<span data-ttu-id="d1a9b-131">[![Los datos iniciales en la tabla de AJAX](drag-and-drop-via-reorderlist-cs/_static/image5.png)](drag-and-drop-via-reorderlist-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="d1a9b-131">[![The initial data in the AJAX table](drag-and-drop-via-reorderlist-cs/_static/image5.png)](drag-and-drop-via-reorderlist-cs/_static/image4.png)</span></span>

<span data-ttu-id="d1a9b-132">Los datos iniciales en la tabla de AJAX ([haga clic aquí para ver imagen en tamaño completo](drag-and-drop-via-reorderlist-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="d1a9b-132">The initial data in the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-cs/_static/image6.png))</span></span>


<span data-ttu-id="d1a9b-133">El siguiente paso se requiere para generar un `SqlDataSource` control para comunicarse con la nueva base de datos y su tabla.</span><span class="sxs-lookup"><span data-stu-id="d1a9b-133">The next step requires to generate an `SqlDataSource` control to communicate with the new database and its table.</span></span> <span data-ttu-id="d1a9b-134">El origen de datos debe admitir la `SELECT` y `UPDATE` comandos SQL.</span><span class="sxs-lookup"><span data-stu-id="d1a9b-134">The data source must support the `SELECT` and `UPDATE` SQL commands.</span></span> <span data-ttu-id="d1a9b-135">Cuando posteriormente se cambia el orden de los elementos de lista, el `ReorderList` control envía automáticamente los dos valores para el origen de datos `Update` comando: la nueva posición y el identificador del elemento.</span><span class="sxs-lookup"><span data-stu-id="d1a9b-135">When the order of the list elements is later changed, the `ReorderList` control automatically submits two values to the data source's `Update` command: the new position and the ID of the element.</span></span> <span data-ttu-id="d1a9b-136">Por lo tanto, las necesidades del origen de datos un `<UpdateParameters>` sección para estos dos valores:</span><span class="sxs-lookup"><span data-stu-id="d1a9b-136">Therefore, the data source needs an `<UpdateParameters>` section for these two values:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample1.aspx)]

<span data-ttu-id="d1a9b-137">El `ReorderList` control necesita establecer los atributos siguientes:</span><span class="sxs-lookup"><span data-stu-id="d1a9b-137">The `ReorderList` control needs to set the following attributes:</span></span>

- <span data-ttu-id="d1a9b-138">`AllowReorder`: Determina si se pueden reorganizar los elementos de lista</span><span class="sxs-lookup"><span data-stu-id="d1a9b-138">`AllowReorder`: Whether the list items may be rearranged</span></span>
- <span data-ttu-id="d1a9b-139">`DataSourceID`: El identificador del origen de datos</span><span class="sxs-lookup"><span data-stu-id="d1a9b-139">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="d1a9b-140">`DataKeyField`: El nombre de la columna de clave principal en el origen de datos</span><span class="sxs-lookup"><span data-stu-id="d1a9b-140">`DataKeyField`: The name of the primary key column in the data source</span></span>
- <span data-ttu-id="d1a9b-141">`SortOrderField`: La columna de origen de datos que proporciona el criterio de ordenación para los elementos de lista</span><span class="sxs-lookup"><span data-stu-id="d1a9b-141">`SortOrderField`: The data source column that provides the sort order for the list items</span></span>

<span data-ttu-id="d1a9b-142">En el `<DragHandleTemplate>` y `<ItemTemplate>` secciones, el diseño de la lista puede ajustarse.</span><span class="sxs-lookup"><span data-stu-id="d1a9b-142">In the `<DragHandleTemplate>` and `<ItemTemplate>` sections, the layout of the list can be fine-tuned.</span></span> <span data-ttu-id="d1a9b-143">Además, el enlace de datos es posible mediante la `Eval()` método, tal como se muestra aquí:</span><span class="sxs-lookup"><span data-stu-id="d1a9b-143">Also, databinding is possible using the `Eval()` method, as seen here:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample2.aspx)]

<span data-ttu-id="d1a9b-144">Información de estilo de CSS siguientes (que se hace referencia en el `<DragHandleTemplate>` sección de la `ReorderList` control) garantiza que el puntero del mouse cambia correctamente cuando se mantiene el mouse sobre el controlador de arrastre:</span><span class="sxs-lookup"><span data-stu-id="d1a9b-144">The following CSS style information (referenced in the `<DragHandleTemplate>` section of the `ReorderList` control) makes sure that the mouse pointer changes appropriately when it hovers over the drag handle:</span></span>

[!code-css[Main](drag-and-drop-via-reorderlist-cs/samples/sample3.css)]

<span data-ttu-id="d1a9b-145">Por último, un `ScriptManager` control inicializa AJAX de ASP.NET para la página:</span><span class="sxs-lookup"><span data-stu-id="d1a9b-145">Finally, a `ScriptManager` control initializes ASP.NET AJAX for the page:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample4.aspx)]

<span data-ttu-id="d1a9b-146">Ejecutar este ejemplo en el explorador y reorganizar los elementos de lista un poco.</span><span class="sxs-lookup"><span data-stu-id="d1a9b-146">Run this example in the browser and rearrange the list items a bit.</span></span> <span data-ttu-id="d1a9b-147">A continuación, vuelva a cargar la página o eche un vistazo a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="d1a9b-147">Then, reload the page and/or have a look at the database.</span></span> <span data-ttu-id="d1a9b-148">Las posiciones modificadas se haya mantenido y también se reflejan en los valores de la `position` columna en la base de datos y todo ello sin ningún código, simplemente mediante el uso de marcado.</span><span class="sxs-lookup"><span data-stu-id="d1a9b-148">The altered positions have been maintained and are also reflected by the values in the `position` column in the database and that all without any code, just by using markup.</span></span>


<span data-ttu-id="d1a9b-149">[![Los datos de los cambios de la base de datos según el orden de elemento de lista nuevo](drag-and-drop-via-reorderlist-cs/_static/image8.png)](drag-and-drop-via-reorderlist-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="d1a9b-149">[![The data in the database changes according to the new list item order](drag-and-drop-via-reorderlist-cs/_static/image8.png)](drag-and-drop-via-reorderlist-cs/_static/image7.png)</span></span>

<span data-ttu-id="d1a9b-150">Elemento de datos en la base de datos cambia según la nueva lista de orden ([haga clic aquí para ver imagen en tamaño completo](drag-and-drop-via-reorderlist-cs/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="d1a9b-150">The data in the database changes according to the new list item order ([Click to view full-size image](drag-and-drop-via-reorderlist-cs/_static/image9.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d1a9b-151">[Anterior](using-postbacks-with-reorderlist-cs.md)
> [Siguiente](using-postbacks-with-reorderlist-vb.md)</span><span class="sxs-lookup"><span data-stu-id="d1a9b-151">[Previous](using-postbacks-with-reorderlist-cs.md)
[Next](using-postbacks-with-reorderlist-vb.md)</span></span>
