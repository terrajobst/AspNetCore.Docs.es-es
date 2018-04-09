---
uid: web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-vb
title: Usar un ConfirmButton en un repetidor (VB) | Documentos de Microsoft
author: wenz
description: El extensor de ConfirmButton en el Kit de herramientas de Control de AJAX crea Yes/No emergente cuando el usuario hace clic en un botón (incluido control LinkButton). Solo si sí es...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 18c31709-3f9d-4d93-8b01-f1356bf610b4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-vb
msc.type: authoredcontent
ms.openlocfilehash: 89f412c242a3a5bcd10b72b7f0cbfb23705edb51
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="using-a-confirmbutton-in-a-repeater-vb"></a><span data-ttu-id="171ba-104">Usar un ConfirmButton en un repetidor (VB)</span><span class="sxs-lookup"><span data-stu-id="171ba-104">Using a ConfirmButton In a Repeater (VB)</span></span>
====================
<span data-ttu-id="171ba-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="171ba-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="171ba-106">[Descargar código](http://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.vb.zip) o [descarga de PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="171ba-106">[Download Code](http://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1VB.pdf)</span></span>

> <span data-ttu-id="171ba-107">El extensor de ConfirmButton en el Kit de herramientas de Control de AJAX crea Yes/No emergente cuando el usuario hace clic en un botón (incluido control LinkButton).</span><span class="sxs-lookup"><span data-stu-id="171ba-107">The ConfirmButton extender in the AJAX Control Toolkit creates a Yes/No popup when the user clicks on a button (including LinkButton control).</span></span> <span data-ttu-id="171ba-108">Solo si se hace clic en Sí, se ejecuta la acción del botón, en caso contrario, se cancela.</span><span class="sxs-lookup"><span data-stu-id="171ba-108">Only if Yes is clicked, the button's action is executed, otherwise cancelled.</span></span> <span data-ttu-id="171ba-109">Esto también es posible en un repetidor.</span><span class="sxs-lookup"><span data-stu-id="171ba-109">This is also possible in a repeater.</span></span>


## <a name="overview"></a><span data-ttu-id="171ba-110">Información general</span><span class="sxs-lookup"><span data-stu-id="171ba-110">Overview</span></span>

<span data-ttu-id="171ba-111">El extensor de ConfirmButton en el Kit de herramientas de Control de AJAX crea Yes/No emergente cuando el usuario hace clic en un botón (incluido control LinkButton).</span><span class="sxs-lookup"><span data-stu-id="171ba-111">The ConfirmButton extender in the AJAX Control Toolkit creates a Yes/No popup when the user clicks on a button (including LinkButton control).</span></span> <span data-ttu-id="171ba-112">Solo si se hace clic en Sí, se ejecuta la acción del botón, en caso contrario, se cancela.</span><span class="sxs-lookup"><span data-stu-id="171ba-112">Only if Yes is clicked, the button's action is executed, otherwise cancelled.</span></span> <span data-ttu-id="171ba-113">Esto también es posible en un repetidor.</span><span class="sxs-lookup"><span data-stu-id="171ba-113">This is also possible in a repeater.</span></span>

## <a name="steps"></a><span data-ttu-id="171ba-114">Pasos</span><span class="sxs-lookup"><span data-stu-id="171ba-114">Steps</span></span>

<span data-ttu-id="171ba-115">En primer lugar, es necesario un origen de datos.</span><span class="sxs-lookup"><span data-stu-id="171ba-115">First of all, a data source is required.</span></span> <span data-ttu-id="171ba-116">Este ejemplo utiliza la base de datos de AdventureWorks y Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="171ba-116">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="171ba-117">La base de datos es una parte opcional de una instalación de Visual Studio (incluida la edición express) y también está disponible como una descarga independiente en [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="171ba-117">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="171ba-118">La base de datos de AdventureWorks es parte de los ejemplos de SQL Server 2005 y bases de datos de ejemplo (descargue en [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = es](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="171ba-118">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="171ba-119">La manera más fácil de configurar la base de datos es usar Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = es](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) y adjuntar la `AdventureWorks.mdf` archivo de base de datos.</span><span class="sxs-lookup"><span data-stu-id="171ba-119">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="171ba-120">En este ejemplo, se supone que se llama a la instancia de SQL Server 2005 Express Edition `SQLEXPRESS` y reside en el mismo equipo que el servidor web; también es la configuración predeterminada.</span><span class="sxs-lookup"><span data-stu-id="171ba-120">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="171ba-121">Si el programa de instalación diferente, tendrá que adaptar la información de conexión para la base de datos.</span><span class="sxs-lookup"><span data-stu-id="171ba-121">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="171ba-122">Para activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager` control se debe colocar en cualquier sitio en la página (pero en el `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="171ba-122">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample1.aspx)]

<span data-ttu-id="171ba-123">A continuación, es necesario un origen de datos.</span><span class="sxs-lookup"><span data-stu-id="171ba-123">Then, a data source is required.</span></span> <span data-ttu-id="171ba-124">Por simplicidad, se recuperan sólo las primeras cinco entradas de tabla de proveedores de AdventureWorks.</span><span class="sxs-lookup"><span data-stu-id="171ba-124">For the sake of simplicity, only the first five entries in AdventureWorks' Vendors table are retrieved.</span></span> <span data-ttu-id="171ba-125">Tenga en cuenta que, cuando utilice el Asistente de Visual Studio para crear el origen de datos, el nombre de tabla (`Vendors`) está actualmente sin correctamente el prefijo `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="171ba-125">Note that when using the Visual Studio wizard to create the data source, the table name (`Vendors`) is currently not correctly prefixed with `Purchasing`.</span></span> <span data-ttu-id="171ba-126">El siguiente marcado es la correcta:</span><span class="sxs-lookup"><span data-stu-id="171ba-126">The following markup is the correct one:</span></span>

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample2.aspx)]

<span data-ttu-id="171ba-127">Este origen de datos, a continuación, puede usarse en un repetidor.</span><span class="sxs-lookup"><span data-stu-id="171ba-127">This data source can then be used within a repeater.</span></span> <span data-ttu-id="171ba-128">Como es habitual, el `DataBinder.Eval()` método recupera datos del origen de datos.</span><span class="sxs-lookup"><span data-stu-id="171ba-128">As usual, the `DataBinder.Eval()` method retrieves data from the data source.</span></span> <span data-ttu-id="171ba-129">El `ConfirmButtonExtender` , a continuación, se debe colocar el control dentro de la `<ItemTemplate>` sección del repetidor para que aparezca para cada entrada del origen de datos.</span><span class="sxs-lookup"><span data-stu-id="171ba-129">The `ConfirmButtonExtender` control must then be placed within the `<ItemTemplate>` section of the repeater so that it appears for every entry in the data source.</span></span>

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample3.aspx)]


<span data-ttu-id="171ba-130">[![El botón Confirmar aparece junto a cada entrada del origen de datos](using-a-confirmbutton-in-a-repeater-vb/_static/image2.png)](using-a-confirmbutton-in-a-repeater-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="171ba-130">[![The confirm button appears next to each entry from the data source](using-a-confirmbutton-in-a-repeater-vb/_static/image2.png)](using-a-confirmbutton-in-a-repeater-vb/_static/image1.png)</span></span>

<span data-ttu-id="171ba-131">El botón Confirmar aparece junto a cada entrada del origen de datos ([haga clic aquí para ver la imagen a tamaño completo](using-a-confirmbutton-in-a-repeater-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="171ba-131">The confirm button appears next to each entry from the data source ([Click to view full-size image](using-a-confirmbutton-in-a-repeater-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="171ba-132">Anterior</span><span class="sxs-lookup"><span data-stu-id="171ba-132">Previous</span></span>](using-a-confirmbutton-in-a-repeater-cs.md)
