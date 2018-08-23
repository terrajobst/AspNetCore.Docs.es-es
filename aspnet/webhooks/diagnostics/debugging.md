---
uid: webhooks/diagnostics/debugging
title: WebHooks de ASP.NET depuración | Microsoft Docs
author: rick-anderson
description: Cómo depurar WebHooks de ASP.NET.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.openlocfilehash: 517d282fc22703b5861b748aea51023fa0a12a26
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828589"
---
# <a name="aspnet-webhooks-debugging"></a><span data-ttu-id="587dc-103">Depuración de WebHooks de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="587dc-103">ASP.NET WebHooks debugging</span></span>  

## <a name="debugging-in-azure"></a><span data-ttu-id="587dc-104">Depuración en Azure</span><span class="sxs-lookup"><span data-stu-id="587dc-104">Debugging in Azure</span></span>

<span data-ttu-id="587dc-105">Para depurar la aplicación Web mientras se ejecuta en Azure, consulte el tutorial [solucionar problemas de una aplicación web en Azure App Service mediante Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span><span class="sxs-lookup"><span data-stu-id="587dc-105">To debug your Web Application while running in Azure, please see the tutorial [Troubleshoot a web app in Azure App Service using Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span></span>

## <a name="debugging-with-source-and-symbols"></a><span data-ttu-id="587dc-106">Depuración con símbolos y código fuente</span><span class="sxs-lookup"><span data-stu-id="587dc-106">Debugging with Source and Symbols</span></span>

<span data-ttu-id="587dc-107">Además de su propio código de depuración, es posible depurar directamente en Microsoft ASP.NET WebHooks y, en realidad todo de. NET.</span><span class="sxs-lookup"><span data-stu-id="587dc-107">In addition to debugging your own code, it is possible to debug directly into Microsoft ASP.NET WebHooks, and in fact all of .NET.</span></span> <span data-ttu-id="587dc-108">Esto funciona independientemente de si se depura localmente o remotamente.</span><span class="sxs-lookup"><span data-stu-id="587dc-108">This works regardless of whether you debug locally or remotely.</span></span> <span data-ttu-id="587dc-109">En primer lugar, configurar Visual Studio para ver el código fuente y símbolos, vaya a **depurar** y, a continuación, **opciones y configuración**.</span><span class="sxs-lookup"><span data-stu-id="587dc-109">First, configure Visual Studio to find the source and symbols by going to **Debug** and then **Options and Settings**.</span></span> <span data-ttu-id="587dc-110">Establecer las opciones similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="587dc-110">Set the options like this:</span></span>

![Opciones y configuración](_static/SourceSymbols.png)

<span data-ttu-id="587dc-112">A continuación, agregue un vínculo a [symbolsource.org](http://symbolsource.org) para descargar el código fuente y símbolos.</span><span class="sxs-lookup"><span data-stu-id="587dc-112">Then add a link to [symbolsource.org](http://symbolsource.org) for downloading the source and symbols.</span></span> <span data-ttu-id="587dc-113">Vaya a la **símbolos** ficha del menú anterior y agregue lo siguiente como una ubicación de símbolos:</span><span class="sxs-lookup"><span data-stu-id="587dc-113">Go to the **Symbols** tab of the menu above and add the following as a symbol location:</span></span>

```
http://srv.symbolsource.org/pdb/Public
```

<span data-ttu-id="587dc-114">Además, asegúrese de que el directorio de caché tiene un nombre corto; en caso contrario, los nombres de archivo pueden obtener demasiados lo que hará que los símbolos que no se cargue.</span><span class="sxs-lookup"><span data-stu-id="587dc-114">In addition, make sure that the cache directory has a short name; otherwise the file names can get too long which will cause the symbols to not load.</span></span> <span data-ttu-id="587dc-115">Es una ruta de acceso de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="587dc-115">A sample path is:</span></span>

```
C:\SymCache
```

<span data-ttu-id="587dc-116">La configuración debe ser similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="587dc-116">The settings should look similar to this:</span></span>

![Ejemplo de ubicación de archivo de símbolos de opciones](_static/SymSource.png)
