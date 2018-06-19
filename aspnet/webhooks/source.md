---
uid: webhooks/source
title: Paquetes de NuGet y código fuente de ASP.NET WebHooks | Documentos de Microsoft
author: rick-anderson
description: Vínculos a código fuente de ASP.NET WebHooks y paquetes de NuGet
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.technology: ''
ms.prod: .net-framework
ms.openlocfilehash: 733a0839c77bcfc96214bdf235ce8fe22ee2d3cf
ms.sourcegitcommit: 2d23ea501e0213bbacf65298acf1c8bd17209540
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/09/2018
ms.locfileid: "27709975"
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a><span data-ttu-id="97ab0-103">Código de origen de WebHooks de ASP.NET y los paquetes de NuGet</span><span class="sxs-lookup"><span data-stu-id="97ab0-103">ASP.NET WebHooks source code and NuGet packages</span></span>

<span data-ttu-id="97ab0-104">Microsoft ASP.NET WebHooks forma parte de la familia de Microsoft ASP.NET de módulos y se hospeda como una [Open Source Project en GitHub](https://github.com/aspnet/WebHooks).</span><span class="sxs-lookup"><span data-stu-id="97ab0-104">Microsoft ASP.NET WebHooks is part of the Microsoft ASP.NET family of modules and is hosted as an [Open Source Project on GitHub](https://github.com/aspnet/WebHooks).</span></span> <span data-ttu-id="97ab0-105">Esto significa que se aceptan las contribuciones, pero mire el [directrices de contribución](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) antes de enviar una solicitud de extracción.</span><span class="sxs-lookup"><span data-stu-id="97ab0-105">This means that we accept contributions, but please look at the [Contribution Guidelines](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) before submitting a pull request.</span></span>

<span data-ttu-id="97ab0-106">Esta documentación en línea que está leyendo ahora también se hospeda como [código abierto en GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) y también acepta las contribuciones.</span><span class="sxs-lookup"><span data-stu-id="97ab0-106">This online documentation which you are reading now is also hosted as [Open Source on GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) and also accepts contributions.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="97ab0-107">Paquetes NuGet</span><span class="sxs-lookup"><span data-stu-id="97ab0-107">NuGet packages</span></span>

<span data-ttu-id="97ab0-108">El [paquetes de NuGet](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) se dividen en tres partes:</span><span class="sxs-lookup"><span data-stu-id="97ab0-108">The [NuGet packages](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) are divided into three parts:</span></span>

* <span data-ttu-id="97ab0-109">[Common](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): un paquete común que se comparte entre remitentes y receptores.</span><span class="sxs-lookup"><span data-stu-id="97ab0-109">[Common](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): A common package that is shared between senders and receivers.</span></span>

* <span data-ttu-id="97ab0-110">[Remitente](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): un conjunto de paquetes que admite enviar sus propios WebHooks a otros usuarios.</span><span class="sxs-lookup"><span data-stu-id="97ab0-110">[Sender](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): A set of packages supporting sending your own WebHooks to others.</span></span> <span data-ttu-id="97ab0-111">La funcionalidad para el envío de Webhook se describe con más detalle en [WebHooks enviar](sending/index.md).</span><span class="sxs-lookup"><span data-stu-id="97ab0-111">The functionality for sending WebHooks is described in more detail in [Sending WebHooks](sending/index.md).</span></span>

* <span data-ttu-id="97ab0-112">[Receptores](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): un conjunto de paquetes admitir reciba WebHooks de otros equipos.</span><span class="sxs-lookup"><span data-stu-id="97ab0-112">[Receivers](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): A set of packages supporting receiving WebHooks from others.</span></span> <span data-ttu-id="97ab0-113">La funcionalidad para la recepción de Webhook se describe con más detalle en [WebHooks recibir](receiving/index.md).</span><span class="sxs-lookup"><span data-stu-id="97ab0-113">The functionality for receiving WebHooks is described in more detail in [Receiving WebHooks](receiving/index.md).</span></span>
