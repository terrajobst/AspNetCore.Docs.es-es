---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: "Representación ASP.NET Web Pages (Razor) sitios para dispositivos móviles | Documentos de Microsoft"
author: tfitzmac
description: "En este artículo se describe cómo crear páginas en un sitio de ASP.NET Web Pages (Razor) que se representará correctamente en los dispositivos móviles. Lo que aprenderá: cómo se..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: 899bbdef82d689be81cd77ea6805e0484fb614aa
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/15/2018
---
<a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a><span data-ttu-id="467f6-104">Representación de sitios de ASP.NET Web Pages (Razor) para dispositivos móviles</span><span class="sxs-lookup"><span data-stu-id="467f6-104">Rendering ASP.NET Web Pages (Razor) Sites for Mobile Devices</span></span>
====================
<span data-ttu-id="467f6-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="467f6-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="467f6-106">En este artículo se describe cómo crear páginas en un sitio de ASP.NET Web Pages (Razor) que se representará correctamente en los dispositivos móviles.</span><span class="sxs-lookup"><span data-stu-id="467f6-106">This article describes how to create pages in an ASP.NET Web Pages (Razor) site that will render appropriately on mobile devices.</span></span>
> 
> <span data-ttu-id="467f6-107">Lo que aprenderá:</span><span class="sxs-lookup"><span data-stu-id="467f6-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="467f6-108">Cómo usar una convención de nomenclatura para especificar que una página está diseñado específicamente para dispositivos móviles.</span><span class="sxs-lookup"><span data-stu-id="467f6-108">How to use a naming convention to specify that a page is designed specifically for mobile devices.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="467f6-109">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="467f6-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="467f6-110">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="467f6-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="467f6-111">Este tutorial también funciona con ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="467f6-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="467f6-112">ASP.NET Web Pages le permite crear pantallas personalizadas para representar el contenido en el teléfono móvil o de otros dispositivos.</span><span class="sxs-lookup"><span data-stu-id="467f6-112">ASP.NET Web Pages lets you create custom displays for rendering content on mobile or other devices.</span></span>

<span data-ttu-id="467f6-113">La manera más sencilla de crear página específico del dispositivo en un sitio de ASP.NET Web Pages está utilizando un modelo de nomenclatura de archivos similar al siguiente: *nombre de archivo. **Mobile**.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="467f6-113">The simplest way to create device-specific page in an ASP.NET Web Pages site is by using a file-naming pattern like this: *FileName.**Mobile**.cshtml*.</span></span> <span data-ttu-id="467f6-114">Puede crear dos versiones de una página (por ejemplo, uno denominado *MyFile.cshtml* y otro denominado *MyFile.Mobile.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="467f6-114">You can create two versions of a page (for example, one named *MyFile.cshtml* and one named *MyFile.Mobile.cshtml*).</span></span> <span data-ttu-id="467f6-115">Durante la ejecución, cuando se solicita un dispositivo móvil *MyFile.cshtml*, ASP.NET representa el contenido de *MyFile.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="467f6-115">At run time, when a mobile device requests *MyFile.cshtml*, ASP.NET renders the content from *MyFile.Mobile.cshtml*.</span></span> <span data-ttu-id="467f6-116">En caso contrario, *MyFile.cshtml* se representa.</span><span class="sxs-lookup"><span data-stu-id="467f6-116">Otherwise, *MyFile.cshtml* is rendered.</span></span>

<span data-ttu-id="467f6-117">En el ejemplo siguiente se muestra cómo habilitar la representación de dispositivos móvil mediante la adición de una página de contenido para dispositivos móviles.</span><span class="sxs-lookup"><span data-stu-id="467f6-117">The following example shows how to enable mobile rendering by adding a content page for mobile devices.</span></span> <span data-ttu-id="467f6-118">*Page1.cshtml* contiene contenido más de una barra lateral de navegación.</span><span class="sxs-lookup"><span data-stu-id="467f6-118">*Page1.cshtml* contains content plus a navigation sidebar.</span></span> <span data-ttu-id="467f6-119">*Page1.Mobile.cshtml* contiene el mismo contenido, pero omite la barra lateral.</span><span class="sxs-lookup"><span data-stu-id="467f6-119">*Page1.Mobile.cshtml* contains the same content, but omits the sidebar.</span></span>

1. <span data-ttu-id="467f6-120">En un sitio de ASP.NET Web Pages, cree un archivo denominado *Page1.cshtml* y reemplace el contenido actual por el siguiente código de marcado.</span><span class="sxs-lookup"><span data-stu-id="467f6-120">In an ASP.NET Web Pages site, create a file named *Page1.cshtml* and replace the current content with following markup.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. <span data-ttu-id="467f6-121">Cree un archivo denominado *Page1.Mobile.cshtml* y reemplace el contenido existente por el siguiente marcado.</span><span class="sxs-lookup"><span data-stu-id="467f6-121">Create a file named *Page1.Mobile.cshtml* and replace the existing content with the following markup.</span></span> <span data-ttu-id="467f6-122">Tenga en cuenta que la versión móvil de la página omite la sección de exploración para representar mejor en una pantalla más pequeña.</span><span class="sxs-lookup"><span data-stu-id="467f6-122">Notice that the mobile version of the page omits the navigation section for better rendering on a smaller screen.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. <span data-ttu-id="467f6-123">Ejecutar un explorador de escritorio y vaya a *Page1.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="467f6-123">Run a desktop browser and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="467f6-124">![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="467f6-124">![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span></span>
4. <span data-ttu-id="467f6-125">Ejecutar un explorador móvil (o un emulador de dispositivos móviles) y vaya a *Page1.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="467f6-125">Run a mobile browser (or a mobile device emulator) and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="467f6-126">(Tenga en cuenta que no incluye *.mobile.*</span><span class="sxs-lookup"><span data-stu-id="467f6-126">(Notice that you do not include *.mobile.*</span></span> <span data-ttu-id="467f6-127">como parte de la dirección URL). Aunque la solicitud es a *Page1.cshtml*, ASP.NET presenta *Page1.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="467f6-127">as part of the URL.) Even though the request is to *Page1.cshtml*, ASP.NET renders *Page1.Mobile.cshtml*.</span></span>

    ![mobilesites-2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="467f6-129">Para probar las páginas móviles, puede usar un simulador de dispositivos móviles que se ejecuta en un equipo de escritorio.</span><span class="sxs-lookup"><span data-stu-id="467f6-129">To test mobile pages, you can use a mobile device simulator that runs on a desktop computer.</span></span> <span data-ttu-id="467f6-130">Esta herramienta le permite probar las páginas web tal como aparecen en los dispositivos móviles (es decir, normalmente con mucho menor Mostrar área).</span><span class="sxs-lookup"><span data-stu-id="467f6-130">This tool lets you test web pages as they would look on mobile devices (that is, typically with a much smaller display area).</span></span> <span data-ttu-id="467f6-131">Un ejemplo de un simulador es el [complemento de selector de agente de usuario](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) para Mozilla Firefox, que le permite emular distintos exploradores móviles desde una versión de escritorio de Firefox.</span><span class="sxs-lookup"><span data-stu-id="467f6-131">One example of a simulator is the [User Agent Switcher add-on](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) for Mozilla Firefox, which lets you emulate various mobile browsers from a desktop version of Firefox.</span></span>


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="467f6-132">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="467f6-132">Additional Resources</span></span>


<span data-ttu-id="467f6-133">[Emulador de Windows Phone](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)</span><span class="sxs-lookup"><span data-stu-id="467f6-133">[Windows Phone Emulator](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)</span></span>
