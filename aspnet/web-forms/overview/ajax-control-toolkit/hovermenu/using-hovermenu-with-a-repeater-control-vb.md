---
uid: web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-vb
title: Usar HoverMenu con un Control de repetidor (VB) | Documentos de Microsoft
author: wenz
description: "El control HoverMenu en el Kit de herramientas de Control de AJAX proporciona un efecto emergente simple: cuando el puntero del mouse se desplace sobre un elemento, aparece un mensaje emergente en una específicamente..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 7f07c112-cd4f-4427-9699-57cfab2791fd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 77759afe00f341e15ee8e0f52a469d3b7c08ea89
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="using-hovermenu-with-a-repeater-control-vb"></a><span data-ttu-id="f4395-103">Usar HoverMenu con un Control de repetidor (VB)</span><span class="sxs-lookup"><span data-stu-id="f4395-103">Using HoverMenu with a Repeater Control (VB)</span></span>
====================
<span data-ttu-id="f4395-104">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f4395-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f4395-105">[Descargar código](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.vb.zip) o [descarga de PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="f4395-105">[Download Code](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1VB.pdf)</span></span>

> <span data-ttu-id="f4395-106">El control HoverMenu en el Kit de herramientas de Control de AJAX proporciona un efecto emergente simple: cuando el puntero del mouse se desplace sobre un elemento, aparece un mensaje emergente en una posición especificada.</span><span class="sxs-lookup"><span data-stu-id="f4395-106">The HoverMenu control in the AJAX Control Toolkit provides a simple popup effect: When the mouse pointer hovers over an element, a popup appears at a specified position.</span></span> <span data-ttu-id="f4395-107">También es posible utilizar este control dentro de un repetidor.</span><span class="sxs-lookup"><span data-stu-id="f4395-107">It is also possible to use this control within a repeater.</span></span>


## <a name="overview"></a><span data-ttu-id="f4395-108">Información general</span><span class="sxs-lookup"><span data-stu-id="f4395-108">Overview</span></span>

<span data-ttu-id="f4395-109">El `HoverMenu` control en el Kit de herramientas de Control de AJAX proporciona un efecto emergente simple: cuando el puntero del mouse se desplace sobre un elemento, aparece un mensaje emergente en una posición especificada.</span><span class="sxs-lookup"><span data-stu-id="f4395-109">The `HoverMenu` control in the AJAX Control Toolkit provides a simple popup effect: When the mouse pointer hovers over an element, a popup appears at a specified position.</span></span> <span data-ttu-id="f4395-110">También es posible utilizar este control dentro de un repetidor.</span><span class="sxs-lookup"><span data-stu-id="f4395-110">It is also possible to use this control within a repeater.</span></span>

## <a name="steps"></a><span data-ttu-id="f4395-111">Pasos</span><span class="sxs-lookup"><span data-stu-id="f4395-111">Steps</span></span>

<span data-ttu-id="f4395-112">En primer lugar, es necesario un origen de datos.</span><span class="sxs-lookup"><span data-stu-id="f4395-112">First of all, a data source is required.</span></span> <span data-ttu-id="f4395-113">Este ejemplo utiliza la base de datos de AdventureWorks y Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="f4395-113">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="f4395-114">La base de datos es una parte opcional de una instalación de Visual Studio (incluida la edición express) y también está disponible como una descarga independiente en [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="f4395-114">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="f4395-115">La base de datos de AdventureWorks es parte de los ejemplos de SQL Server 2005 y bases de datos de ejemplo (descargue en [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang = es](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="f4395-115">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="f4395-116">La manera más fácil de configurar la base de datos es usar Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx? FamilyID = c243a5ae 4bd1 4e3d 94b8 5a0f62bf7796&amp;DisplayLang = es](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) y adjuntar la `AdventureWorks.mdf` archivo de base de datos.</span><span class="sxs-lookup"><span data-stu-id="f4395-116">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="f4395-117">En este ejemplo, se supone que se llama a la instancia de SQL Server 2005 Express Edition `SQLEXPRESS` y reside en el mismo equipo que el servidor web; también es la configuración predeterminada.</span><span class="sxs-lookup"><span data-stu-id="f4395-117">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="f4395-118">Si el programa de instalación diferente, tendrá que adaptar la información de conexión para la base de datos.</span><span class="sxs-lookup"><span data-stu-id="f4395-118">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="f4395-119">Para activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager` control se debe colocar en cualquier sitio en la página (pero en el `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="f4395-119">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample1.aspx)]

<span data-ttu-id="f4395-120">A continuación, agregue un origen de datos a la página.</span><span class="sxs-lookup"><span data-stu-id="f4395-120">Then, add a data source to the page.</span></span> <span data-ttu-id="f4395-121">Para poder usar una cantidad limitada de datos, se selecciona solo las primeras cinco entradas en la tabla de proveedor de la base de datos de AdventureWorks.</span><span class="sxs-lookup"><span data-stu-id="f4395-121">In order to use a limited amount of data, we only select the first five entries in the Vendor table of the AdventureWorks database.</span></span> <span data-ttu-id="f4395-122">Si está utilizando el Asistente de Visual Studio para crear el origen de datos, recuerde que un error en la versión actual no utiliza ningún prefijo el nombre de tabla (`Vendor`) con `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="f4395-122">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="f4395-123">El marcado siguiente muestra la sintaxis correcta:</span><span class="sxs-lookup"><span data-stu-id="f4395-123">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample2.aspx)]

<span data-ttu-id="f4395-124">A continuación, agregue un panel que actúa como el elemento emergente modal:</span><span class="sxs-lookup"><span data-stu-id="f4395-124">Next, add a panel which serves as the modal popup:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample3.aspx)]

<span data-ttu-id="f4395-125">Ahora, la `HoverMenuExtender` entra en juego.</span><span class="sxs-lookup"><span data-stu-id="f4395-125">Now, the `HoverMenuExtender` comes into play.</span></span> <span data-ttu-id="f4395-126">Para que todos los elementos del origen de datos obtiene su propia ventana emergente, el dispositivo extender debe situarse dentro del repetidor `<ItemTemplate>` sección.</span><span class="sxs-lookup"><span data-stu-id="f4395-126">So that every element in the data source gets its own popup, the extender must be put within the repeater's `<ItemTemplate>` section.</span></span> <span data-ttu-id="f4395-127">Aquí está el marcado:</span><span class="sxs-lookup"><span data-stu-id="f4395-127">Here is the markup:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample4.aspx)]

<span data-ttu-id="f4395-128">Ahora, todos los elementos del origen de datos muestran un menú emergente a la derecha (`PopupPosition` atributo) después de un retraso de 50 milisegundos (`PopDelay` atributo).</span><span class="sxs-lookup"><span data-stu-id="f4395-128">Now every item in the data source displays a popup to the right (`PopupPosition` attribute) after a delay of 50 milliseconds (`PopDelay` attribute).</span></span>


<span data-ttu-id="f4395-129">[![El menú de desplazamiento que aparece junto a cada elemento del repetidor](using-hovermenu-with-a-repeater-control-vb/_static/image2.png)](using-hovermenu-with-a-repeater-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f4395-129">[![The hover menu appears next to each item in the repeater](using-hovermenu-with-a-repeater-control-vb/_static/image2.png)](using-hovermenu-with-a-repeater-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="f4395-130">El menú de desplazamiento que aparece junto a cada elemento del Repetidor ([haga clic aquí para ver la imagen a tamaño completo](using-hovermenu-with-a-repeater-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f4395-130">The hover menu appears next to each item in the repeater ([Click to view full-size image](using-hovermenu-with-a-repeater-control-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="f4395-131">Anterior</span><span class="sxs-lookup"><span data-stu-id="f4395-131">Previous</span></span>](using-hovermenu-with-a-repeater-control-cs.md)
