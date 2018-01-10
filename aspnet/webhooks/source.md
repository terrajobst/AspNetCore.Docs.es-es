---
uid: webhooks/source
title: "Paquetes de NuGet y código fuente de ASP.NET WebHooks | Documentos de Microsoft"
author: rick-anderson
description: "Vínculos a código fuente de ASP.NET WebHooks y paquetes de NuGet"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 733a0839c77bcfc96214bdf235ce8fe22ee2d3cf
ms.sourcegitcommit: 2d23ea501e0213bbacf65298acf1c8bd17209540
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/09/2018
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a>Código de origen de WebHooks de ASP.NET y los paquetes de NuGet

Microsoft ASP.NET WebHooks forma parte de la familia de Microsoft ASP.NET de módulos y se hospeda como una [Open Source Project en GitHub](https://github.com/aspnet/WebHooks). Esto significa que se aceptan las contribuciones, pero mire el [directrices de contribución](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) antes de enviar una solicitud de extracción.

Esta documentación en línea que está leyendo ahora también se hospeda como [código abierto en GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) y también acepta las contribuciones.

## <a name="nuget-packages"></a>Paquetes NuGet

El [paquetes de NuGet](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) se dividen en tres partes:

* [Common](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): un paquete común que se comparte entre remitentes y receptores.

* [Remitente](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): un conjunto de paquetes que admite enviar sus propios WebHooks a otros usuarios. La funcionalidad para el envío de Webhook se describe con más detalle en [WebHooks enviar](sending/index.md).

* [Receptores](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): un conjunto de paquetes admitir reciba WebHooks de otros equipos. La funcionalidad para la recepción de Webhook se describe con más detalle en [WebHooks recibir](receiving/index.md).
