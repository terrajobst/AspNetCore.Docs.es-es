---
uid: web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
title: Con distintas versiones de ASP.NET Web Pages (Razor) en paralelo | Microsoft Docs
author: tfitzmac
description: Este artículo explica cómo ejecutar sitios Web de ASP.NET Web Pages (Razor) en el mismo servidor o equipo cuando los sitios Web están configurados para usar diferentes versiones...
ms.author: riande
ms.date: 02/10/2014
ms.assetid: a861409b-4ae6-4868-9e09-87edfac3535f
msc.legacyurl: /web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
msc.type: authoredcontent
ms.openlocfilehash: 9021f9b7a68b8b20f7f2fbcd5649cc7226401a1b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837511"
---
<a name="running-different-versions-of-aspnet-web-pages-razor-side-by-side"></a><span data-ttu-id="7c2d3-103">Ejecución en paralelo diferentes versiones de ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="7c2d3-103">Running Different Versions of ASP.NET Web Pages (Razor) Side by Side</span></span>
====================
<span data-ttu-id="7c2d3-104">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="7c2d3-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="7c2d3-105">En este artículo se explica cómo ejecutar sitios Web de ASP.NET Web Pages (Razor) en el mismo servidor o equipo cuando los sitios Web están configurados para usar diferentes versiones de ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-105">This article explains how to run ASP.NET Web Pages (Razor) websites on the same computer or server when the websites are configured to use different versions of ASP.NET Web Pages.</span></span>
> 
> <span data-ttu-id="7c2d3-106">Lo que aprenderá:</span><span class="sxs-lookup"><span data-stu-id="7c2d3-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="7c2d3-107">Lo que el comportamiento predeterminado es de ASP.NET cuando tenga sitios creados con ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-107">What the default behavior is in ASP.NET when you have sites built with ASP.NET Web Pages.</span></span>
> - <span data-ttu-id="7c2d3-108">Cómo configurar un nuevo sitio se ejecute con una versión anterior de ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-108">How to configure a new site to run with an older version of ASP.NET Web Pages.</span></span>
>   
> 
> <span data-ttu-id="7c2d3-109">Esta es la característica ASP.NET que se introdujo en el artículo:</span><span class="sxs-lookup"><span data-stu-id="7c2d3-109">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="7c2d3-110">El `webPages:Version` opción de configuración.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-110">The `webPages:Version` configuration setting.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="7c2d3-111">Versiones de software</span><span class="sxs-lookup"><span data-stu-id="7c2d3-111">Software versions</span></span>
> 
> 
> - <span data-ttu-id="7c2d3-112">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="7c2d3-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="7c2d3-113">Este tutorial también funciona con ASP.NET Web Pages 2 y las páginas Web de ASP.NET 1.0.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-113">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0.</span></span>


<span data-ttu-id="7c2d3-114">ASP.NET Web Pages admite la capacidad para ejecutar sitios Web en paralelo.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-114">ASP.NET Web Pages supports the ability to run websites side by side.</span></span> <span data-ttu-id="7c2d3-115">Esto le permite continuar para ejecutar sus aplicaciones más antiguas de ASP.NET Web Pages, crear nuevas aplicaciones de ASP.NET Web Pages y ejecutar todas ellas en el mismo equipo.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-115">This lets you continue to run your older ASP.NET Web Pages applications, build new ASP.NET Web Pages applications, and run all of them on the same computer.</span></span>

<span data-ttu-id="7c2d3-116">Estas son algunas cosas que recordar cuando se instalación las páginas Web con WebMatrix:</span><span class="sxs-lookup"><span data-stu-id="7c2d3-116">Here are some things to remember when you install the Web Pages with WebMatrix:</span></span>

- <span data-ttu-id="7c2d3-117">De forma predeterminada, las aplicaciones existentes de las páginas Web se ejecutarán como la versión más reciente en el equipo.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-117">By default, existing Web Pages applications will run as the latest version on your computer.</span></span> <span data-ttu-id="7c2d3-118">(Los ensamblados se instalan en la caché global de ensamblados (GAC) y se usan automáticamente).</span><span class="sxs-lookup"><span data-stu-id="7c2d3-118">(The assemblies are installed in the global assembly cache (GAC) and are used automatically.)</span></span>
- <span data-ttu-id="7c2d3-119">Si desea ejecutar un sitio con una versión diferente de ASP.NET Web Pages, puede configurar el sitio para hacerlo.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-119">If you want to run a site using a different version of ASP.NET Web Pages, you can configure the site to do that.</span></span> <span data-ttu-id="7c2d3-120">Si el sitio ya no tiene un *web.config* de archivos en la raíz del sitio, cree uno y copie el siguiente código XML en él, sobrescribiendo el contenido existente.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-120">If your site doesn't already have a *web.config* file in the root of the site, create a new one and copy the following XML into it, overwriting the existing content.</span></span> <span data-ttu-id="7c2d3-121">Si el sitio ya contiene un *web.config* , agregue un `<appSettings>` elemento como el siguiente a la `<configuration>` sección.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-121">If the site already contains a *web.config* file, add an `<appSettings>` element like the following one to the `<configuration>` section.</span></span>

    [!code-xml[Main](running-v1-and-v2-sites-side-by-side/samples/sample1.xml)]
  <span data-ttu-id="7c2d3-122">'-Si no especifica ninguna versión en el *web.config* archivo, un sitio se implementa como la versión más reciente.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-122">\`- If you do not specify a version in the *web.config* file, a site is deployed as the latest version.</span></span> <span data-ttu-id="7c2d3-123">(Los ensamblados se copian en el *bin* carpeta en el sitio implementado.)</span><span class="sxs-lookup"><span data-stu-id="7c2d3-123">(The assemblies are copied to the *bin* folder in the deployed site.)</span></span>
- <span data-ttu-id="7c2d3-124">Las aplicaciones nuevas que cree mediante las plantillas de sitio en WebMatrix incluyen los ensamblados de la versión de las páginas Web en el sitio *bin* carpeta.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-124">New applications that you create using the site templates in Web Matrix include the Web Pages version assemblies in the site's *bin* folder.</span></span>

<span data-ttu-id="7c2d3-125">En general, siempre puede controlar qué versión de las páginas Web para usar con su sitio mediante el uso de NuGet para instalar los ensamblados correspondientes en la carpeta del sitio *bin* carpeta.</span><span class="sxs-lookup"><span data-stu-id="7c2d3-125">In general, you can always control which version of Web Pages to use with your site by using NuGet to install the appropriate assemblies into the site's *bin* folder.</span></span> <span data-ttu-id="7c2d3-126">Para buscar paquetes, visite [NuGet.org](http://NuGet.org).</span><span class="sxs-lookup"><span data-stu-id="7c2d3-126">To find packages, visit [NuGet.org](http://NuGet.org).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7c2d3-127">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="7c2d3-127">Additional Resources</span></span>

[<span data-ttu-id="7c2d3-128">Las principales características de ASP.NET Web Pages 2</span><span class="sxs-lookup"><span data-stu-id="7c2d3-128">The Top Features in ASP.NET Web Pages 2</span></span>](top-features-in-web-pages-2.md)
