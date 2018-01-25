---
uid: webhooks/diagnostics/debugging
title: "WebHooks ASP.NET depuración | Documentos de Microsoft"
author: rick-anderson
description: "Cómo depurar WebHooks de ASP.NET."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 524cdf0246eda9ef213414923cd23a92a01f211e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
# <a name="aspnet-webhooks-debugging"></a><span data-ttu-id="2659a-103">Depuración de Webhook de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="2659a-103">ASP.NET WebHooks debugging</span></span>  

## <a name="debugging-in-azure"></a><span data-ttu-id="2659a-104">Depuración de Azure</span><span class="sxs-lookup"><span data-stu-id="2659a-104">Debugging in Azure</span></span>

<span data-ttu-id="2659a-105">Para depurar la aplicación Web mientras se ejecuta en Azure, vea el tutorial [solucionar problemas de una aplicación web en el servicio de aplicaciones de Azure con Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span><span class="sxs-lookup"><span data-stu-id="2659a-105">To debug your Web Application while running in Azure, please see the tutorial [Troubleshoot a web app in Azure App Service using Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span></span>

## <a name="debugging-with-source-and-symbols"></a><span data-ttu-id="2659a-106">Depuración con símbolos y código fuente</span><span class="sxs-lookup"><span data-stu-id="2659a-106">Debugging with Source and Symbols</span></span>

<span data-ttu-id="2659a-107">Además de su propio código de depuración, es posible depurar directamente en Microsoft ASP.NET WebHooks y, de hecho todos de. NET.</span><span class="sxs-lookup"><span data-stu-id="2659a-107">In addition to debugging your own code, it is possible to debug directly into Microsoft ASP.NET WebHooks, and in fact all of .NET.</span></span> <span data-ttu-id="2659a-108">Esto funciona independientemente de si se depura de forma local o remota.</span><span class="sxs-lookup"><span data-stu-id="2659a-108">This works regardless of whether you debug locally or remotely.</span></span> <span data-ttu-id="2659a-109">En primer lugar, configurar Visual Studio para ver el código fuente y símbolos, vaya a **depurar** y, a continuación, **opciones y configuración**.</span><span class="sxs-lookup"><span data-stu-id="2659a-109">First, configure Visual Studio to find the source and symbols by going to **Debug** and then **Options and Settings**.</span></span> <span data-ttu-id="2659a-110">Establecer las opciones similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="2659a-110">Set the options like this:</span></span>

![Opciones y configuración](_static/SourceSymbols.png)

<span data-ttu-id="2659a-112">A continuación, agregue un vínculo a [symbolsource.org del asociado](http://symbolsource.org) para descargar el código fuente y símbolos.</span><span class="sxs-lookup"><span data-stu-id="2659a-112">Then add a link to [symbolsource.org](http://symbolsource.org) for downloading the source and symbols.</span></span> <span data-ttu-id="2659a-113">Vaya a la **símbolos** ficha del menú anterior y agregue lo siguiente como una ubicación de los símbolos:</span><span class="sxs-lookup"><span data-stu-id="2659a-113">Go to the **Symbols** tab of the menu above and add the following as a symbol location:</span></span>

```
http://srv.symbolsource.org/pdb/Public
```

<span data-ttu-id="2659a-114">Además, asegúrese de que el directorio de caché tiene un nombre corto; en caso contrario, los nombres de archivo pueden acaban siendo demasiado largos que hará que los símbolos que no se cargue.</span><span class="sxs-lookup"><span data-stu-id="2659a-114">In addition, make sure that the cache directory has a short name; otherwise the file names can get too long which will cause the symbols to not load.</span></span> <span data-ttu-id="2659a-115">Es una ruta de acceso de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="2659a-115">A sample path is:</span></span>

```
C:\SymCache
```

<span data-ttu-id="2659a-116">La configuración debe ser similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="2659a-116">The settings should look similar to this:</span></span>

![Ejemplo de ubicación de archivo de símbolos de opciones](_static/SymSource.png)
