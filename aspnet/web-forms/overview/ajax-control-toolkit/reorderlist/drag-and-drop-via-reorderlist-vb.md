---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
title: Arrastrar y colocar a través de ReorderList (VB) | Documentos de Microsoft
author: wenz
description: /data-access/tutorials/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 848e6bcf-4c3f-4d14-974d-e45b9444ab79
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 99f47b969dc75efeec8485254d311c93dc0b5d35
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878794"
---
<a name="drag-and-drop-via-reorderlist-vb"></a><span data-ttu-id="16dd3-103">Arrastrar y colocar a través de ReorderList (VB)</span><span class="sxs-lookup"><span data-stu-id="16dd3-103">Drag and Drop via ReorderList (VB)</span></span>
====================
<span data-ttu-id="16dd3-104">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="16dd3-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="16dd3-105">[Descargar código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip) o [descarga de PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="16dd3-105">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)</span></span>

> <span data-ttu-id="16dd3-106">El control de ReorderList en el Kit de herramientas de Control de AJAX proporciona una lista que se puede ordenar por el usuario a través de arrastrar y colocar.</span><span class="sxs-lookup"><span data-stu-id="16dd3-106">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="16dd3-107">El orden actual de la lista debe conservarse en el servidor.</span><span class="sxs-lookup"><span data-stu-id="16dd3-107">The current order of the list shall be persisted on the server.</span></span>


## <a name="overview"></a><span data-ttu-id="16dd3-108">Información general</span><span class="sxs-lookup"><span data-stu-id="16dd3-108">Overview</span></span>

<span data-ttu-id="16dd3-109">El `ReorderList` control en el Kit de herramientas de Control de AJAX proporciona una lista que se puede ordenar por el usuario a través de arrastrar y colocar.</span><span class="sxs-lookup"><span data-stu-id="16dd3-109">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="16dd3-110">El orden actual de la lista debe conservarse en el servidor.</span><span class="sxs-lookup"><span data-stu-id="16dd3-110">The current order of the list shall be persisted on the server.</span></span>

## <a name="steps"></a><span data-ttu-id="16dd3-111">Pasos</span><span class="sxs-lookup"><span data-stu-id="16dd3-111">Steps</span></span>

<span data-ttu-id="16dd3-112">El `ReorderList` control admite el enlace de datos de una base de datos a la lista.</span><span class="sxs-lookup"><span data-stu-id="16dd3-112">The `ReorderList` control supports binding data from a database to the list.</span></span> <span data-ttu-id="16dd3-113">Lo mejor de todo, también admite la escritura de cambios con el orden de lo elemento de lista en el almacén de datos.</span><span class="sxs-lookup"><span data-stu-id="16dd3-113">Best of all, it also supports writing changes to the order of the list element back to the data store.</span></span>

<span data-ttu-id="16dd3-114">Este ejemplo utiliza Microsoft SQL Server 2005 Express Edition como almacén de datos.</span><span class="sxs-lookup"><span data-stu-id="16dd3-114">This sample uses Microsoft SQL Server 2005 Express Edition as the data store.</span></span> <span data-ttu-id="16dd3-115">La base de datos es una parte opcional (y free) de una instalación de Visual Studio, incluida la edición express.</span><span class="sxs-lookup"><span data-stu-id="16dd3-115">The database is an optional (and free) part of a Visual Studio installation, including express edition.</span></span> <span data-ttu-id="16dd3-116">También está disponible como una descarga independiente en [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="16dd3-116">It is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="16dd3-117">En este ejemplo, se supone que se llama a la instancia de SQL Server 2005 Express Edition `SQLEXPRESS` y reside en el mismo equipo que el servidor web; también es la configuración predeterminada.</span><span class="sxs-lookup"><span data-stu-id="16dd3-117">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="16dd3-118">Si el programa de instalación diferente, tendrá que adaptar la información de conexión para la base de datos.</span><span class="sxs-lookup"><span data-stu-id="16dd3-118">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="16dd3-119">La manera más fácil de configurar la base de datos es usar Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = es](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span><span class="sxs-lookup"><span data-stu-id="16dd3-119">The easiest way to set up the database is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span></span> <span data-ttu-id="16dd3-120">Conectarse al servidor, haga doble clic en `Databases` y crear una nueva base de datos (haga clic en y elija `New Database`) llama `Tutorials`.</span><span class="sxs-lookup"><span data-stu-id="16dd3-120">Connect to the server, double-click on `Databases` and create a new database (right-click and choose `New Database`) called `Tutorials`.</span></span>

<span data-ttu-id="16dd3-121">En esta base de datos, cree una nueva tabla denominada `AJAX` con las cuatro columnas siguientes:</span><span class="sxs-lookup"><span data-stu-id="16dd3-121">In this database, create a new table called `AJAX` with the following four columns:</span></span>

- <span data-ttu-id="16dd3-122">`id` (entero de clave, principal, identidad, no NULL)</span><span class="sxs-lookup"><span data-stu-id="16dd3-122">`id` (primary key, integer, identity, not NULL)</span></span>
- <span data-ttu-id="16dd3-123">`char` (char(1), NULL)</span><span class="sxs-lookup"><span data-stu-id="16dd3-123">`char` (char(1), NULL)</span></span>
- <span data-ttu-id="16dd3-124">`description` (varchar(50), NULL)</span><span class="sxs-lookup"><span data-stu-id="16dd3-124">`description` (varchar(50), NULL)</span></span>
- <span data-ttu-id="16dd3-125">`position` (int, NULL)</span><span class="sxs-lookup"><span data-stu-id="16dd3-125">`position` (int, NULL)</span></span>


<span data-ttu-id="16dd3-126">[![El diseño de la tabla de AJAX](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="16dd3-126">[![The layout of the AJAX table](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)</span></span>

<span data-ttu-id="16dd3-127">El diseño de la tabla de AJAX ([haga clic aquí para ver la imagen a tamaño completo](drag-and-drop-via-reorderlist-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="16dd3-127">The layout of the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image3.png))</span></span>


<span data-ttu-id="16dd3-128">A continuación, rellene la tabla con un par de valores.</span><span class="sxs-lookup"><span data-stu-id="16dd3-128">Next, fill the table with a couple of values.</span></span> <span data-ttu-id="16dd3-129">Tenga en cuenta que la `position` columna contiene el criterio de ordenación de los elementos.</span><span class="sxs-lookup"><span data-stu-id="16dd3-129">Note that the `position` column holds the sort order of the elements.</span></span>


<span data-ttu-id="16dd3-130">[![Los datos iniciales de la tabla de AJAX](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="16dd3-130">[![The initial data in the AJAX table](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)</span></span>

<span data-ttu-id="16dd3-131">Los datos iniciales de la tabla de AJAX ([haga clic aquí para ver la imagen a tamaño completo](drag-and-drop-via-reorderlist-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="16dd3-131">The initial data in the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image6.png))</span></span>


<span data-ttu-id="16dd3-132">El siguiente paso se requiere para generar un `SqlDataSource` control para comunicarse con la nueva base de datos y su tabla.</span><span class="sxs-lookup"><span data-stu-id="16dd3-132">The next step requires to generate an `SqlDataSource` control to communicate with the new database and its table.</span></span> <span data-ttu-id="16dd3-133">El origen de datos debe admitir la `SELECT` y `UPDATE` comandos SQL.</span><span class="sxs-lookup"><span data-stu-id="16dd3-133">The data source must support the `SELECT` and `UPDATE` SQL commands.</span></span> <span data-ttu-id="16dd3-134">Cuando posteriormente se cambia el orden de los elementos de lista, el `ReorderList` control envía automáticamente dos valores para el origen de datos `Update` comando: la nueva posición y el identificador del elemento.</span><span class="sxs-lookup"><span data-stu-id="16dd3-134">When the order of the list elements is later changed, the `ReorderList` control automatically submits two values to the data source's `Update` command: the new position and the ID of the element.</span></span> <span data-ttu-id="16dd3-135">Por lo tanto, las necesidades del origen de datos un `<UpdateParameters>` sección para estos dos valores:</span><span class="sxs-lookup"><span data-stu-id="16dd3-135">Therefore, the data source needs an `<UpdateParameters>` section for these two values:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample1.aspx)]

<span data-ttu-id="16dd3-136">El `ReorderList` control debe establecer los atributos siguientes:</span><span class="sxs-lookup"><span data-stu-id="16dd3-136">The `ReorderList` control needs to set the following attributes:</span></span>

- <span data-ttu-id="16dd3-137">`AllowReorder`: Determina si se pueden reorganizar los elementos de lista</span><span class="sxs-lookup"><span data-stu-id="16dd3-137">`AllowReorder`: Whether the list items may be rearranged</span></span>
- <span data-ttu-id="16dd3-138">`DataSourceID`: El identificador del origen de datos</span><span class="sxs-lookup"><span data-stu-id="16dd3-138">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="16dd3-139">`DataKeyField`: El nombre de la columna de clave principal en el origen de datos</span><span class="sxs-lookup"><span data-stu-id="16dd3-139">`DataKeyField`: The name of the primary key column in the data source</span></span>
- <span data-ttu-id="16dd3-140">`SortOrderField`: La columna de origen de datos que proporciona el criterio de ordenación para los elementos de lista</span><span class="sxs-lookup"><span data-stu-id="16dd3-140">`SortOrderField`: The data source column that provides the sort order for the list items</span></span>

<span data-ttu-id="16dd3-141">En el `<DragHandleTemplate>` y `<ItemTemplate>` secciones, el diseño de la lista puede ajustarse.</span><span class="sxs-lookup"><span data-stu-id="16dd3-141">In the `<DragHandleTemplate>` and `<ItemTemplate>` sections, the layout of the list can be fine-tuned.</span></span> <span data-ttu-id="16dd3-142">Además, el enlace de datos es posible mediante la `Eval()` método, tal como se muestra aquí:</span><span class="sxs-lookup"><span data-stu-id="16dd3-142">Also, databinding is possible using the `Eval()` method, as seen here:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample2.aspx)]

<span data-ttu-id="16dd3-143">El código CSS siguiente la información de estilo (que se hace referencia en el `<DragHandleTemplate>` sección de la `ReorderList` control) se asegura de que el puntero del mouse cambia correctamente cuando mantiene el mouse sobre el controlador de arrastre:</span><span class="sxs-lookup"><span data-stu-id="16dd3-143">The following CSS style information (referenced in the `<DragHandleTemplate>` section of the `ReorderList` control) makes sure that the mouse pointer changes appropriately when it hovers over the drag handle:</span></span>

[!code-css[Main](drag-and-drop-via-reorderlist-vb/samples/sample3.css)]

<span data-ttu-id="16dd3-144">Por último, un `ScriptManager` control inicializa AJAX de ASP.NET para la página:</span><span class="sxs-lookup"><span data-stu-id="16dd3-144">Finally, a `ScriptManager` control initializes ASP.NET AJAX for the page:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample4.aspx)]

<span data-ttu-id="16dd3-145">Ejecutar este ejemplo en el explorador y reorganizar los elementos de lista un poco.</span><span class="sxs-lookup"><span data-stu-id="16dd3-145">Run this example in the browser and rearrange the list items a bit.</span></span> <span data-ttu-id="16dd3-146">A continuación, vuelva a cargar la página o eche un vistazo a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="16dd3-146">Then, reload the page and/or have a look at the database.</span></span> <span data-ttu-id="16dd3-147">Las posiciones modificadas se haya mantenido y también se reflejan en los valores de la `position` columna en la base de datos y que todo ello sin ningún código, simplemente mediante el uso de marcado.</span><span class="sxs-lookup"><span data-stu-id="16dd3-147">The altered positions have been maintained and are also reflected by the values in the `position` column in the database and that all without any code, just by using markup.</span></span>


<span data-ttu-id="16dd3-148">[![Los datos de los cambios de base de datos según el orden de elemento de lista nuevo](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="16dd3-148">[![The data in the database changes according to the new list item order](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)</span></span>

<span data-ttu-id="16dd3-149">Orden de los datos de los cambios de base de datos según la nueva lista de elementos ([haga clic aquí para ver la imagen a tamaño completo](drag-and-drop-via-reorderlist-vb/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="16dd3-149">The data in the database changes according to the new list item order ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image9.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="16dd3-150">Anterior</span><span class="sxs-lookup"><span data-stu-id="16dd3-150">Previous</span></span>](using-postbacks-with-reorderlist-vb.md)
