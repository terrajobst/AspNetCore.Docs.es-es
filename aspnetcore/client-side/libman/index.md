---
title: Adquisición de bibliotecas del lado cliente en ASP.NET Core con LibMan
author: scottaddie
description: Obtenga información sobre cómo instalar los recursos de bibliotecas del lado cliente en un proyecto de ASP.NET Core mediante Library Manager (LibMan).
ms.author: scaddie
ms.custom: mvc
ms.date: 08/14/2018
uid: client-side/libman/index
ms.openlocfilehash: b21ab0dbeda043dceda5376bcd95ccd334707c5a
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2018
ms.locfileid: "41909920"
---
# <a name="client-side-library-acquisition-in-aspnet-core-with-libman"></a><span data-ttu-id="a86c1-103">Adquisición de bibliotecas del lado cliente en ASP.NET Core con LibMan</span><span class="sxs-lookup"><span data-stu-id="a86c1-103">Client-side library acquisition in ASP.NET Core with LibMan</span></span>

<span data-ttu-id="a86c1-104">Por [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="a86c1-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="a86c1-105">Library Manager (LibMan) es una herramienta ligera para la adquisición de bibliotecas del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="a86c1-105">Library Manager (LibMan) is a lightweight, client-side library acquisition tool.</span></span> <span data-ttu-id="a86c1-106">Asimismo, LibMan permite descargar bibliotecas y marcos populares de sistemas de archivos o [redes de entrega de contenido (CDN)](https://wikipedia.org/wiki/Content_delivery_network).</span><span class="sxs-lookup"><span data-stu-id="a86c1-106">LibMan downloads popular libraries and frameworks from the file system or from a [content delivery network (CDN)](https://wikipedia.org/wiki/Content_delivery_network).</span></span> <span data-ttu-id="a86c1-107">Entre las CDN admitidas se incluyen [CDNJS](https://cdnjs.com/) y [unpkg](https://unpkg.com/#/).</span><span class="sxs-lookup"><span data-stu-id="a86c1-107">The supported CDNs include [CDNJS](https://cdnjs.com/) and [unpkg](https://unpkg.com/#/).</span></span> <span data-ttu-id="a86c1-108">Los archivos de biblioteca seleccionados se capturan y se colocan en la ubicación adecuada dentro del proyecto de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a86c1-108">The selected library files are fetched and placed in the appropriate location within the ASP.NET Core project.</span></span>

## <a name="libman-use-cases"></a><span data-ttu-id="a86c1-109">Casos de uso de LibMan</span><span class="sxs-lookup"><span data-stu-id="a86c1-109">LibMan use cases</span></span>

<span data-ttu-id="a86c1-110">LibMan ofrece las ventajas siguientes:</span><span class="sxs-lookup"><span data-stu-id="a86c1-110">LibMan offers the following benefits:</span></span>

* <span data-ttu-id="a86c1-111">Solo se descargan los archivos de biblioteca necesarios.</span><span class="sxs-lookup"><span data-stu-id="a86c1-111">Only the library files you need are downloaded.</span></span>
* <span data-ttu-id="a86c1-112">No es necesario usar otras herramientas como [Node.js](https://nodejs.org), [npm](https://www.npmjs.com) o [WebPack](https://webpack.js.org) para adquirir un subconjunto de archivos de una biblioteca.</span><span class="sxs-lookup"><span data-stu-id="a86c1-112">Additional tooling, such as [Node.js](https://nodejs.org), [npm](https://www.npmjs.com), and [WebPack](https://webpack.js.org), isn't necessary to acquire a subset of files in a library.</span></span>
* <span data-ttu-id="a86c1-113">Los archivos se pueden colocar en una ubicación específica sin tener que usar tareas de compilación ni copiar archivos manualmente.</span><span class="sxs-lookup"><span data-stu-id="a86c1-113">Files can be placed in a specific location without resorting to build tasks or manual file copying.</span></span>

<span data-ttu-id="a86c1-114">Para obtener más información sobre las ventajas del LibMan, vea [Modern front-end web development in Visual Studio 2017: LibMan segment](https://channel9.msdn.com/Events/Build/2017/B8073#time=43m34s) (Desarrollo web front-end moderno en Visual Studio 2017: segmento LibMan).</span><span class="sxs-lookup"><span data-stu-id="a86c1-114">For more information about LibMan's benefits, watch [Modern front-end web development in Visual Studio 2017: LibMan segment](https://channel9.msdn.com/Events/Build/2017/B8073#time=43m34s).</span></span>

<span data-ttu-id="a86c1-115">LibMan no es un sistema de administración de paquetes.</span><span class="sxs-lookup"><span data-stu-id="a86c1-115">LibMan isn't a package management system.</span></span> <span data-ttu-id="a86c1-116">Si ya usa un administrador de paquetes, por ejemplo, npm o [yarn](https://yarnpkg.com), le recomendamos que sigua haciéndolo.</span><span class="sxs-lookup"><span data-stu-id="a86c1-116">If you're already using a package manager, such as npm or [yarn](https://yarnpkg.com), continue doing so.</span></span> <span data-ttu-id="a86c1-117">LibMan no se ha desarrollado para reemplazar dichas herramientas.</span><span class="sxs-lookup"><span data-stu-id="a86c1-117">LibMan wasn't developed to replace those tools.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a86c1-118">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="a86c1-118">Additional resources</span></span>

* <xref:client-side/libman/libman-vs>
* [<span data-ttu-id="a86c1-119">Repositorio de LibMan en GitHub</span><span class="sxs-lookup"><span data-stu-id="a86c1-119">LibMan GitHub repository</span></span>](https://github.com/aspnet/LibraryManager)
