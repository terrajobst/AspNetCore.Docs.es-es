---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
title: Empezar a trabajar con el Kit de herramientas de Control de AJAX (C#) | Documentos de Microsoft
author: microsoft
description: Obtenga información acerca de todo lo que necesita saber para empezar a usar el Kit de herramientas de Control de AJAX.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 16dc5c11-65be-4eae-a818-9fad7f8259c6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
msc.type: authoredcontent
ms.openlocfilehash: e6a7a8d45f32a33eaacf3c42b52a02d2ada1aab6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="get-started-with-the-ajax-control-toolkit-c"></a><span data-ttu-id="61b56-103">Empezar a trabajar con el Kit de herramientas de Control de AJAX (C#)</span><span class="sxs-lookup"><span data-stu-id="61b56-103">Get Started with the AJAX Control Toolkit (C#)</span></span>
====================
<span data-ttu-id="61b56-104">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="61b56-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="61b56-105">Obtenga información acerca de todo lo que necesita saber para empezar a usar el Kit de herramientas de Control de AJAX.</span><span class="sxs-lookup"><span data-stu-id="61b56-105">Learn all you need to know to get started using the AJAX Control Toolkit.</span></span>


<span data-ttu-id="61b56-106">El Kit de herramientas de Control de AJAX contiene más de 30 controles libres que puede usar en las aplicaciones ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="61b56-106">The AJAX Control Toolkit contains more than 30 free controls that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="61b56-107">En este tutorial, aprenderá a descargar el Kit de herramientas de Control de AJAX y agregar los controles del Kit de herramientas a su cuadro de herramientas de Visual Studio o Visual Web Developer Express.</span><span class="sxs-lookup"><span data-stu-id="61b56-107">In this tutorial, you learn how to download the AJAX Control Toolkit and add the toolkit controls to your Visual Studio/Visual Web Developer Express toolbox.</span></span>

## <a name="downloading-the-ajax-control-toolkit"></a><span data-ttu-id="61b56-108">Descargar el Kit de herramientas de Control de AJAX</span><span class="sxs-lookup"><span data-stu-id="61b56-108">Downloading the AJAX Control Toolkit</span></span>

<span data-ttu-id="61b56-109">El [AJAX Control Toolkit](http://devexpress.com/act) es un proyecto de código abierto desarrollado por los miembros de la Comunidad de ASP.NET y el equipo ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="61b56-109">The [AJAX Control Toolkit](http://devexpress.com/act) is an open source project developed by the members of the ASP.NET community and the ASP.NET team.</span></span> 


<span data-ttu-id="61b56-110">[![Descargar el Kit de herramientas de Control de AJAX](get-started-with-the-ajax-control-toolkit-cs/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="61b56-110">[![Downloading the AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image1.png)</span></span>

<span data-ttu-id="61b56-111">**Figura 01**: descargar el Kit de herramientas de Control de AJAX ([haga clic aquí para ver la imagen a tamaño completo](get-started-with-the-ajax-control-toolkit-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="61b56-111">**Figure 01**: Downloading the AJAX Control Toolkit([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image2.png))</span></span>


<span data-ttu-id="61b56-112">Después de descargar el archivo, debe desbloquear el archivo.</span><span class="sxs-lookup"><span data-stu-id="61b56-112">After you download the file, you need to unblock the file.</span></span> <span data-ttu-id="61b56-113">Haga clic en el archivo, seleccione Propiedades y haga clic en el **Unblock** botón (consulte la figura 2).</span><span class="sxs-lookup"><span data-stu-id="61b56-113">Right-click the file, select Properties, and click the **Unblock** button (see Figure 2).</span></span>


<span data-ttu-id="61b56-114">[![Desbloquear el archivo ZIP de Kit de herramientas de Control de AJAX](get-started-with-the-ajax-control-toolkit-cs/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="61b56-114">[![Unblocking the AJAX Control Toolkit ZIP file](get-started-with-the-ajax-control-toolkit-cs/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image3.png)</span></span>

<span data-ttu-id="61b56-115">**Figura 02**: desbloquear el archivo ZIP de Kit de herramientas de Control de AJAX ([haga clic aquí para ver la imagen a tamaño completo](get-started-with-the-ajax-control-toolkit-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="61b56-115">**Figure 02**: Unblocking the AJAX Control Toolkit ZIP file([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image4.png))</span></span>


<span data-ttu-id="61b56-116">Tras desbloquear el archivo, puede descomprimir el archivo: haga clic en el archivo y seleccione la **extraer todo** opción de menú.</span><span class="sxs-lookup"><span data-stu-id="61b56-116">After you unblock the file, you can unzip the file: Right-click the file and select the **Extract All** menu option.</span></span> <span data-ttu-id="61b56-117">Ahora, estamos preparados para agregar el Kit de herramientas al cuadro de herramientas de Visual Studio o Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="61b56-117">Now, we are ready to add the toolkit to the Visual Studio/Visual Web Developer toolbox.</span></span>

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a><span data-ttu-id="61b56-118">Agregar el Kit de herramientas de Control de AJAX al cuadro de herramientas</span><span class="sxs-lookup"><span data-stu-id="61b56-118">Adding the AJAX Control Toolkit to the Toolbox</span></span>

<span data-ttu-id="61b56-119">La manera más fácil de usar el Kit de herramientas de Control de AJAX consiste en agregar el Kit de herramientas en el cuadro de herramientas de Visual Studio o Visual Web Developer (consulte la figura 3).</span><span class="sxs-lookup"><span data-stu-id="61b56-119">The easiest way to use the AJAX Control Toolkit is to add the toolkit to your Visual Studio/Visual Web Developer toolbox (see Figure 3).</span></span> <span data-ttu-id="61b56-120">De este modo, puede simplemente arrastrar un control del Kit de herramientas en una página si desea usarlo.</span><span class="sxs-lookup"><span data-stu-id="61b56-120">That way, you can simply drag a toolkit control onto a page when you want to use it.</span></span>


<span data-ttu-id="61b56-121">[![Kit de herramientas de Control de AJAX aparece en el cuadro de herramientas](get-started-with-the-ajax-control-toolkit-cs/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="61b56-121">[![AJAX Control Toolkit appears in toolbox](get-started-with-the-ajax-control-toolkit-cs/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image5.png)</span></span>

<span data-ttu-id="61b56-122">**Figura 03**: Kit de herramientas de Control de AJAX aparece en el cuadro de herramientas ([haga clic aquí para ver la imagen a tamaño completo](get-started-with-the-ajax-control-toolkit-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="61b56-122">**Figure 03**: AJAX Control Toolkit appears in toolbox([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image6.png))</span></span>


<span data-ttu-id="61b56-123">En primer lugar, debe agregar una ficha de AJAX Control Toolkit al cuadro de herramientas.</span><span class="sxs-lookup"><span data-stu-id="61b56-123">First, you need to add an AJAX Control Toolkit tab to the toolbox.</span></span> <span data-ttu-id="61b56-124">Siga estos pasos.</span><span class="sxs-lookup"><span data-stu-id="61b56-124">Follow these steps.</span></span>

1. <span data-ttu-id="61b56-125">Crear un nuevo sitio Web de ASP.NET, seleccione la opción de menú archivo, nuevo sitio Web.</span><span class="sxs-lookup"><span data-stu-id="61b56-125">Create a new ASP.NET Website by selecting the menu option File, New Website.</span></span> <span data-ttu-id="61b56-126">Haga doble clic en Default.aspx en la ventana Explorador de soluciones para abrir el archivo en el editor.</span><span class="sxs-lookup"><span data-stu-id="61b56-126">Double-click the Default.aspx in the Solution Explorer window to open the file in the editor.</span></span>
2. <span data-ttu-id="61b56-127">Haga clic en el cuadro de herramientas debajo de la ficha General y seleccione la opción de menú **Agregar pestaña** (consulte la figura 4).</span><span class="sxs-lookup"><span data-stu-id="61b56-127">Right-click the Toolbox beneath the General Tab and select the menu option **Add Tab** (see Figure 4).</span></span>
3. <span data-ttu-id="61b56-128">Escriba una nueva ficha denominada Kit de herramientas de Control de AJAX.</span><span class="sxs-lookup"><span data-stu-id="61b56-128">Enter a new tab named AJAX Control Toolkit.</span></span>


<span data-ttu-id="61b56-129">[![Agregar una nueva pestaña](get-started-with-the-ajax-control-toolkit-cs/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="61b56-129">[![Adding a new tab](get-started-with-the-ajax-control-toolkit-cs/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image7.png)</span></span>

<span data-ttu-id="61b56-130">**Figura 04**: agregar una nueva pestaña ([haga clic aquí para ver la imagen a tamaño completo](get-started-with-the-ajax-control-toolkit-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="61b56-130">**Figure 04**: Adding a new tab([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image8.png))</span></span>


<span data-ttu-id="61b56-131">A continuación, debe agregar los controles de AJAX Control Toolkit para la nueva pestaña. Siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="61b56-131">Next, you need to add the AJAX Control Toolkit controls to the new tab. Follow these steps:</span></span>

- <span data-ttu-id="61b56-132">Haga clic en debajo de la ficha del Kit de herramientas de Control de AJAX y seleccione la opción de menú **elegir elementos (Véase la figura 5)**.</span><span class="sxs-lookup"><span data-stu-id="61b56-132">Right-click beneath the AJAX Control Toolkit tab and select the menu option **Choose Items (see Figure 5)**.</span></span>
- <span data-ttu-id="61b56-133">Vaya a la ubicación donde descomprimió el Kit de herramientas de Control de AJAX y seleccione el ensamblado AjaxControlToolkit.dll.</span><span class="sxs-lookup"><span data-stu-id="61b56-133">Browse to the location where you unzipped the AJAX Control Toolkit and select the AjaxControlToolkit.dll assembly.</span></span>


<span data-ttu-id="61b56-134">[![Elija los elementos que desea agregar al cuadro de herramientas](get-started-with-the-ajax-control-toolkit-cs/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="61b56-134">[![Choose items to add to the toolbox](get-started-with-the-ajax-control-toolkit-cs/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image9.png)</span></span>

<span data-ttu-id="61b56-135">**Figura 05**: elija los elementos que desea agregar al cuadro de herramientas ([haga clic aquí para ver la imagen a tamaño completo](get-started-with-the-ajax-control-toolkit-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="61b56-135">**Figure 05**: Choose items to add to the toolbox([Click to view full-size image](get-started-with-the-ajax-control-toolkit-cs/_static/image10.png))</span></span>


<span data-ttu-id="61b56-136">Después de completar estos pasos, todos los controles del Kit de herramientas aparecerán en el cuadro de herramientas.</span><span class="sxs-lookup"><span data-stu-id="61b56-136">After you complete these steps, all of the toolkit controls will appear in your toolbox.</span></span>

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a><span data-ttu-id="61b56-137">Actualizar a una nueva versión del Kit de herramientas</span><span class="sxs-lookup"><span data-stu-id="61b56-137">Upgrading to a New Version of the Toolkit</span></span>

<span data-ttu-id="61b56-138">Si usaba una versión anterior del Kit de herramientas y ahora tiene que mover a una versión posterior aquí son los pasos recomendados:</span><span class="sxs-lookup"><span data-stu-id="61b56-138">If you were using an older release of the Toolkit and now need to move to a later version here are the recommended steps:</span></span>

- <span data-ttu-id="61b56-139">Los archivos binarios - eliminar la versión anterior del ensamblado AjaxControlToolkit.dll desde la carpeta Bin del sitio Web.</span><span class="sxs-lookup"><span data-stu-id="61b56-139">Binaries - Delete the old version of the AjaxControlToolkit.dll assembly from your website Bin folder.</span></span>
- <span data-ttu-id="61b56-140">Elementos de cuadro de herramientas - eliminar la ficha del Kit de herramientas de Control de AJAX y siga los pasos anteriores para volver a crear la pestaña con la nueva versión del ensamblado AjaxControlToolkit.dll.</span><span class="sxs-lookup"><span data-stu-id="61b56-140">Toolbox Items - Delete the AJAX Control Toolkit tab and follow the steps above to re-create the tab with the new version of the AjaxControlToolkit.dll assembly.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="61b56-141">Siguiente</span><span class="sxs-lookup"><span data-stu-id="61b56-141">Next</span></span>](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
