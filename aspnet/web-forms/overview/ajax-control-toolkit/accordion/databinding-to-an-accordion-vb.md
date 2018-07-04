---
uid: web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-vb
title: Enlace de datos a Accordion (VB) | Microsoft Docs
author: wenz
description: El control Accordion de AJAX Control Toolkit ofrece varios paneles y permite al usuario mostrar uno de ellos a la vez. Los paneles se declaran normalmente w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b19f0875-7d3e-4ecf-baa1-a0c693c765b3
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-vb
msc.type: authoredcontent
ms.openlocfilehash: bc11aa96577919c2ca5dfca4ffc6301da67645ea
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377433"
---
<a name="databinding-to-an-accordion-vb"></a><span data-ttu-id="2dc66-104">Enlace de datos a Accordion (VB)</span><span class="sxs-lookup"><span data-stu-id="2dc66-104">Databinding to an Accordion (VB)</span></span>
====================
<span data-ttu-id="2dc66-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="2dc66-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="2dc66-106">[Descargar código](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.vb.zip) o [descargar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="2dc66-106">[Download Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1VB.pdf)</span></span>

> <span data-ttu-id="2dc66-107">El control Accordion de AJAX Control Toolkit ofrece varios paneles y permite al usuario mostrar uno de ellos a la vez.</span><span class="sxs-lookup"><span data-stu-id="2dc66-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="2dc66-108">Paneles normalmente se declaran dentro de la propia página, pero el enlace a un origen de datos ofrece más flexibilidad.</span><span class="sxs-lookup"><span data-stu-id="2dc66-108">Panels are usually declared within the page itself, but binding to a data source offers more flexibility.</span></span>


## <a name="overview"></a><span data-ttu-id="2dc66-109">Información general</span><span class="sxs-lookup"><span data-stu-id="2dc66-109">Overview</span></span>

<span data-ttu-id="2dc66-110">El control Accordion de AJAX Control Toolkit ofrece varios paneles y permite al usuario mostrar uno de ellos a la vez.</span><span class="sxs-lookup"><span data-stu-id="2dc66-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="2dc66-111">Paneles normalmente se declaran dentro de la propia página, pero el enlace a un origen de datos ofrece más flexibilidad.</span><span class="sxs-lookup"><span data-stu-id="2dc66-111">Panels are usually declared within the page itself, but binding to a data source offers more flexibility.</span></span>

## <a name="steps"></a><span data-ttu-id="2dc66-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="2dc66-112">Steps</span></span>

<span data-ttu-id="2dc66-113">En primer lugar, es necesario un origen de datos.</span><span class="sxs-lookup"><span data-stu-id="2dc66-113">First of all, a data source is required.</span></span> <span data-ttu-id="2dc66-114">Este ejemplo utiliza la base de datos AdventureWorks y Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="2dc66-114">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="2dc66-115">La base de datos es una parte opcional de una instalación de Visual Studio (incluida la edición express) y también está disponible como una descarga independiente en [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="2dc66-115">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="2dc66-116">La base de datos de AdventureWorks es parte de los ejemplos de SQL Server 2005 y las bases de datos de ejemplo (descargue en [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="2dc66-116">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="2dc66-117">La manera más fácil de configurar la base de datos es usar Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) y adjuntar el `AdventureWorks.mdf` el archivo de base de datos.</span><span class="sxs-lookup"><span data-stu-id="2dc66-117">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="2dc66-118">Para este ejemplo, se supone que se llama a la instancia de SQL Server 2005 Express Edition `SQLEXPRESS` y reside en el mismo equipo que el servidor web; también trata la configuración predeterminada.</span><span class="sxs-lookup"><span data-stu-id="2dc66-118">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="2dc66-119">Si el programa de instalación diferente, deberá adaptar la información de conexión para la base de datos.</span><span class="sxs-lookup"><span data-stu-id="2dc66-119">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="2dc66-120">Para activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager` control debe colocarse en cualquier lugar en la página (pero dentro del `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="2dc66-120">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample1.aspx)]

<span data-ttu-id="2dc66-121">A continuación, agregue un origen de datos a la página.</span><span class="sxs-lookup"><span data-stu-id="2dc66-121">Then, add a data source to the page.</span></span> <span data-ttu-id="2dc66-122">Para poder usar una cantidad limitada de datos, se selecciona solo las cinco primeras entradas en la tabla de proveedor de la base de datos AdventureWorks.</span><span class="sxs-lookup"><span data-stu-id="2dc66-122">In order to use a limited amount of data, we only select the first five entries in the Vendor table of the AdventureWorks database.</span></span> <span data-ttu-id="2dc66-123">Si usas el Asistente de Visual Studio para crear el origen de datos, recuerde que un error en la versión actual no prefijo de nombre de la tabla (`Vendor`) con `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="2dc66-123">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="2dc66-124">El marcado siguiente muestra la sintaxis correcta:</span><span class="sxs-lookup"><span data-stu-id="2dc66-124">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample2.aspx)]

<span data-ttu-id="2dc66-125">Recuerde el nombre (identificador) del origen de datos.</span><span class="sxs-lookup"><span data-stu-id="2dc66-125">Remember the name (ID) of the data source.</span></span> <span data-ttu-id="2dc66-126">A continuación, se debe usar esta identificación muy en el `DataSourceID` propiedad del control Accordion:</span><span class="sxs-lookup"><span data-stu-id="2dc66-126">This very identification must then be used in the `DataSourceID` property of the Accordion control:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample3.aspx)]

<span data-ttu-id="2dc66-127">En el control Accordion, puede proporcionar plantillas para las distintas partes del control, incluido el encabezado (`<HeaderTemplate>`) y el contenido (`<ContentTemplate>`).</span><span class="sxs-lookup"><span data-stu-id="2dc66-127">Within the Accordion control, you can provide templates for various parts of the control, including the header (`<HeaderTemplate>`) and the content (`<ContentTemplate>`).</span></span> <span data-ttu-id="2dc66-128">Dentro de estos elementos, mandar el resultado de los datos de los datos de origen, utilizando el `DataBinder.Eval()` método:</span><span class="sxs-lookup"><span data-stu-id="2dc66-128">Within these elements, just output the data from the data source, using the `DataBinder.Eval()` method:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample4.aspx)]

<span data-ttu-id="2dc66-129">Cuando se carga la página, el origen de datos debe estar enlazado a la accordion con este código de servidor:</span><span class="sxs-lookup"><span data-stu-id="2dc66-129">When the page is loaded, the data source must be bound to the accordion with this server-side code:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample5.aspx)]

<span data-ttu-id="2dc66-130">Para concluir este ejemplo, debe definir las dos clases CSS que se hace referencia en el control Accordion (en sus propiedades `HeaderCssClass` y `ContentCssClass`).</span><span class="sxs-lookup"><span data-stu-id="2dc66-130">To conclude this sample, you need to define the two CSS classes that are referenced in the Accordion control (in its properties `HeaderCssClass` and `ContentCssClass`).</span></span> <span data-ttu-id="2dc66-131">Coloque el siguiente marcado en el `<head>` sección de la página:</span><span class="sxs-lookup"><span data-stu-id="2dc66-131">Put the following markup in the `<head>` section of the page:</span></span>

[!code-css[Main](databinding-to-an-accordion-vb/samples/sample6.css)]


<span data-ttu-id="2dc66-132">[![Los datos de la accordion proceden directamente del origen de datos](databinding-to-an-accordion-vb/_static/image2.png)](databinding-to-an-accordion-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2dc66-132">[![The data in the accordion comes directly from the data source](databinding-to-an-accordion-vb/_static/image2.png)](databinding-to-an-accordion-vb/_static/image1.png)</span></span>

<span data-ttu-id="2dc66-133">Los datos de la accordion proceden directamente del origen de datos ([haga clic aquí para ver imagen en tamaño completo](databinding-to-an-accordion-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="2dc66-133">The data in the accordion comes directly from the data source ([Click to view full-size image](databinding-to-an-accordion-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2dc66-134">[Anterior](dynamically-adding-an-accordion-pane-cs.md)
> [Siguiente](dynamically-adding-an-accordion-pane-vb.md)</span><span class="sxs-lookup"><span data-stu-id="2dc66-134">[Previous](dynamically-adding-an-accordion-pane-cs.md)
[Next](dynamically-adding-an-accordion-pane-vb.md)</span></span>
