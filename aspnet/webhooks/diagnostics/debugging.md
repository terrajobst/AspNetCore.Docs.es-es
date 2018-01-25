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
# <a name="aspnet-webhooks-debugging"></a>Depuración de Webhook de ASP.NET  

## <a name="debugging-in-azure"></a>Depuración de Azure

Para depurar la aplicación Web mientras se ejecuta en Azure, vea el tutorial [solucionar problemas de una aplicación web en el servicio de aplicaciones de Azure con Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="debugging-with-source-and-symbols"></a>Depuración con símbolos y código fuente

Además de su propio código de depuración, es posible depurar directamente en Microsoft ASP.NET WebHooks y, de hecho todos de. NET. Esto funciona independientemente de si se depura de forma local o remota. En primer lugar, configurar Visual Studio para ver el código fuente y símbolos, vaya a **depurar** y, a continuación, **opciones y configuración**. Establecer las opciones similar al siguiente:

![Opciones y configuración](_static/SourceSymbols.png)

A continuación, agregue un vínculo a [symbolsource.org del asociado](http://symbolsource.org) para descargar el código fuente y símbolos. Vaya a la **símbolos** ficha del menú anterior y agregue lo siguiente como una ubicación de los símbolos:

```
http://srv.symbolsource.org/pdb/Public
```

Además, asegúrese de que el directorio de caché tiene un nombre corto; en caso contrario, los nombres de archivo pueden acaban siendo demasiado largos que hará que los símbolos que no se cargue. Es una ruta de acceso de ejemplo:

```
C:\SymCache
```

La configuración debe ser similar al siguiente:

![Ejemplo de ubicación de archivo de símbolos de opciones](_static/SymSource.png)
