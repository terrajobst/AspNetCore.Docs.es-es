---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-vb
title: Usar ModalPopup con un Control de repetidor (VB) | Documentos de Microsoft
author: wenz
description: "El control ModalPopup en el Kit de herramientas de Control de AJAX ofrece una manera sencilla de crear un elemento emergente modal mediante medios de lado cliente. También es posible utilizar esta contrat...."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0c8e74f1-b3ba-4ca9-a1c5-f5c4831a359a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: a1e49fdebfb3ad62667ffd5a979d366730a097bd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="using-modalpopup-with-a-repeater-control-vb"></a><span data-ttu-id="b478c-104">Usar ModalPopup con un Control de repetidor (VB)</span><span class="sxs-lookup"><span data-stu-id="b478c-104">Using ModalPopup with a Repeater Control (VB)</span></span>
====================
<span data-ttu-id="b478c-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="b478c-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="b478c-106">[Descargar código](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup2.vb.zip) o [descarga de PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="b478c-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup2VB.pdf)</span></span>

> <span data-ttu-id="b478c-107">El control ModalPopup en el Kit de herramientas de Control de AJAX ofrece una manera sencilla de crear un elemento emergente modal mediante medios de lado cliente.</span><span class="sxs-lookup"><span data-stu-id="b478c-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="b478c-108">También es posible utilizar este control dentro de un repetidor.</span><span class="sxs-lookup"><span data-stu-id="b478c-108">It is also possible to use this control within a repeater.</span></span>


## <a name="overview"></a><span data-ttu-id="b478c-109">Información general</span><span class="sxs-lookup"><span data-stu-id="b478c-109">Overview</span></span>

<span data-ttu-id="b478c-110">El control ModalPopup en el Kit de herramientas de Control de AJAX ofrece una manera sencilla de crear un elemento emergente modal mediante medios de lado cliente.</span><span class="sxs-lookup"><span data-stu-id="b478c-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="b478c-111">También es posible utilizar este control dentro de un repetidor.</span><span class="sxs-lookup"><span data-stu-id="b478c-111">It is also possible to use this control within a repeater.</span></span>

## <a name="steps"></a><span data-ttu-id="b478c-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="b478c-112">Steps</span></span>

<span data-ttu-id="b478c-113">En primer lugar, es necesario un origen de datos.</span><span class="sxs-lookup"><span data-stu-id="b478c-113">First of all, a data source is required.</span></span> <span data-ttu-id="b478c-114">Este ejemplo utiliza la base de datos de AdventureWorks y Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="b478c-114">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="b478c-115">La base de datos es una parte opcional de una instalación de Visual Studio (incluida la edición express) y también está disponible como una descarga independiente en [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="b478c-115">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="b478c-116">La base de datos de AdventureWorks es parte de los ejemplos de SQL Server 2005 y bases de datos de ejemplo (descargue en [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang = es](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="b478c-116">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="b478c-117">La manera más fácil de configurar la base de datos es usar Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx? FamilyID = c243a5ae 4bd1 4e3d 94b8 5a0f62bf7796&amp;DisplayLang = es](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) y adjuntar la `AdventureWorks.mdf` archivo de base de datos.</span><span class="sxs-lookup"><span data-stu-id="b478c-117">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span> <span data-ttu-id="b478c-118">En este ejemplo, se supone que se llama a la instancia de SQL Server 2005 Express Edition `SQLEXPRESS` y reside en el mismo equipo que el servidor web; también es la configuración predeterminada.</span><span class="sxs-lookup"><span data-stu-id="b478c-118">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="b478c-119">Si el programa de instalación diferente, tendrá que adaptar la información de conexión para la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b478c-119">If your setup differs, you have to adapt the connection information for the database.</span></span> <span data-ttu-id="b478c-120">Para activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager` control se debe colocar en cualquier sitio en la página (pero en el `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="b478c-120">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample1.aspx)]

<span data-ttu-id="b478c-121">A continuación, agregue un origen de datos a la página.</span><span class="sxs-lookup"><span data-stu-id="b478c-121">Then, add a data source to the page.</span></span> <span data-ttu-id="b478c-122">Para poder usar una cantidad limitada de datos, se selecciona solo las primeras cinco entradas en la tabla de proveedor de la base de datos de AdventureWorks.</span><span class="sxs-lookup"><span data-stu-id="b478c-122">In order to use a limited amount of data, we only select the first five entries in the Vendor table of the AdventureWorks database.</span></span> <span data-ttu-id="b478c-123">Si está utilizando el Asistente de Visual Studio para crear el origen de datos, recuerde que un error en la versión actual no utiliza ningún prefijo el nombre de tabla (`Vendor`) con `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="b478c-123">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="b478c-124">El marcado siguiente muestra la sintaxis correcta:</span><span class="sxs-lookup"><span data-stu-id="b478c-124">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample2.aspx)]

<span data-ttu-id="b478c-125">A continuación, agregue un panel que actúa como el elemento emergente modal.</span><span class="sxs-lookup"><span data-stu-id="b478c-125">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="b478c-126">Contiene un `Button` control para cerrar la ventana emergente de nuevo:</span><span class="sxs-lookup"><span data-stu-id="b478c-126">It contains a `Button` control to close the popup again:</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample3.aspx)]

<span data-ttu-id="b478c-127">Con el fin de que la ventana emergente funcione dentro del repetidor el `ModalPopupExtender` control debe situarse dentro de la `<ItemTemplate>` sección del repetidor.</span><span class="sxs-lookup"><span data-stu-id="b478c-127">In order to make the popup work within the repeater, the `ModalPopupExtender` control must be put within the `<ItemTemplate>` section of the repeater.</span></span> <span data-ttu-id="b478c-128">Por lo que el panel está fuera del repetidor, pero el dispositivo extender está dentro.</span><span class="sxs-lookup"><span data-stu-id="b478c-128">So the panel is outside the repeater, but the extender is inside.</span></span> <span data-ttu-id="b478c-129">Este es el marcado de repetidor:</span><span class="sxs-lookup"><span data-stu-id="b478c-129">Here is the markup for the repeater:</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample4.aspx)]

<span data-ttu-id="b478c-130">Finalmente, se muestran todos los elementos del origen de datos con un botón junto a él que desencadena la ventana emergente modal.</span><span class="sxs-lookup"><span data-stu-id="b478c-130">Then, every item in the data source is displayed with a button next to it that triggers the modal popup.</span></span>


<span data-ttu-id="b478c-131">[![Se puede activar la ventana emergente modal para cada entrada del origen de datos](using-modalpopup-with-a-repeater-control-vb/_static/image2.png)](using-modalpopup-with-a-repeater-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b478c-131">[![The modal popup can be triggered for every data source entry](using-modalpopup-with-a-repeater-control-vb/_static/image2.png)](using-modalpopup-with-a-repeater-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="b478c-132">Se puede activar la ventana emergente modal para cada entrada del origen de datos ([haga clic aquí para ver la imagen a tamaño completo](using-modalpopup-with-a-repeater-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="b478c-132">The modal popup can be triggered for every data source entry ([Click to view full-size image](using-modalpopup-with-a-repeater-control-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="b478c-133">[Anterior](launching-a-modal-popup-window-from-server-code-vb.md)
[Siguiente](handling-postbacks-from-a-modalpopup-vb.md)</span><span class="sxs-lookup"><span data-stu-id="b478c-133">[Previous](launching-a-modal-popup-window-from-server-code-vb.md)
[Next](handling-postbacks-from-a-modalpopup-vb.md)</span></span>
