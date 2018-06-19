---
uid: web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
title: Agrupar y Minificar activos en un ASP.NET Web Pages (Razor) sitio | Documentos de Microsoft
author: microsoft
description: Unión y minificación son formas para agilizar su sitio. Cómo agrupar permite combinar varios archivos de JavaScript (.js) o varios hoja en cascada de estilos (...)
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/21/2012
ms.topic: article
ms.assetid: 8906f1e9-4b66-4a03-8e8a-9e9debf8ed91
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
msc.type: authoredcontent
ms.openlocfilehash: df00b5c9e741e429011143d04121df97c1930602
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
ms.locfileid: "26528554"
---
<a name="bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="e1d12-104">Agrupar y Minificar activos en un sitio ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="e1d12-104">Bundling and Minifying Assets in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="e1d12-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="e1d12-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="e1d12-106">Unión y minificación son formas para agilizar su sitio.</span><span class="sxs-lookup"><span data-stu-id="e1d12-106">Bundling and minification are ways to make your site faster.</span></span> <span data-ttu-id="e1d12-107">Cómo agrupar le permite combinar varios JavaScript (*.js*) archivos o varias hojas de estilos en cascada (*.css*) archivos para que se puede descargar como una unidad, en lugar de uno en uno.</span><span class="sxs-lookup"><span data-stu-id="e1d12-107">Bundling lets you combine multiple JavaScript (*.js*) files or multiple cascading style sheet (*.css*) files so that they can be downloaded as a unit, rather than one at a time.</span></span> <span data-ttu-id="e1d12-108">Reducción encoge espera un espacio en blanco y realiza otros tipos de compresión que se va a realizar una posible de los archivos descargados como pequeño.</span><span class="sxs-lookup"><span data-stu-id="e1d12-108">Minification squeezes out whitespace and performs other types of compression to make the downloaded files as small a possible.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="e1d12-109">La versión RC de ASP.NET Web Pages 2 no admite la agrupación y minificación porque el paquete que contiene los elementos necesarios que aún no está disponible en Microsoft WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="e1d12-109">The RC release of ASP.NET Web Pages 2 does not support bundling and minification because the package that contains the required elements is not yet available in Microsoft WebMatrix.</span></span> <span data-ttu-id="e1d12-110">Lamentamos las molestias.</span><span class="sxs-lookup"><span data-stu-id="e1d12-110">We apologize for this inconvenience.</span></span> <span data-ttu-id="e1d12-111">El paquete se espera que esté disponible en la versión final de ASP.NET Web Pages 2 y WebMatrix 2.</span><span class="sxs-lookup"><span data-stu-id="e1d12-111">The package is expected to be available in the final release of ASP.NET Web Pages 2 and WebMatrix 2.</span></span>
