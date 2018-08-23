---
uid: webhooks/source
title: Código fuente de ASP.NET WebHooks y paquetes de NuGet | Microsoft Docs
author: rick-anderson
description: Vínculos a código fuente de ASP.NET WebHooks y paquetes de NuGet
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.openlocfilehash: ff716b476f7dc69b6071d3febd5b5871e4f02689
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836952"
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a><span data-ttu-id="cc337-103">Código fuente de ASP.NET WebHooks y paquetes de NuGet</span><span class="sxs-lookup"><span data-stu-id="cc337-103">ASP.NET WebHooks source code and NuGet packages</span></span>

<span data-ttu-id="cc337-104">WebHooks de ASP.NET de Microsoft forma parte de la familia Microsoft ASP.NET de módulos y está hospedado como un [Open Source Project en GitHub](https://github.com/aspnet/WebHooks).</span><span class="sxs-lookup"><span data-stu-id="cc337-104">Microsoft ASP.NET WebHooks is part of the Microsoft ASP.NET family of modules and is hosted as an [Open Source Project on GitHub](https://github.com/aspnet/WebHooks).</span></span> <span data-ttu-id="cc337-105">Esto significa que se aceptan contribuciones, pero eche un vistazo a la [directrices de contribución](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) antes de enviar una solicitud de incorporación de cambios.</span><span class="sxs-lookup"><span data-stu-id="cc337-105">This means that we accept contributions, but please look at the [Contribution Guidelines](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) before submitting a pull request.</span></span>

<span data-ttu-id="cc337-106">Esta documentación en línea que se está leyendo ahora también está hospedada como [código abierto en GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) y también acepta contribuciones.</span><span class="sxs-lookup"><span data-stu-id="cc337-106">This online documentation which you are reading now is also hosted as [Open Source on GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) and also accepts contributions.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="cc337-107">Paquetes NuGet</span><span class="sxs-lookup"><span data-stu-id="cc337-107">NuGet packages</span></span>

<span data-ttu-id="cc337-108">El [paquetes de NuGet](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) se dividen en tres partes:</span><span class="sxs-lookup"><span data-stu-id="cc337-108">The [NuGet packages](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) are divided into three parts:</span></span>

* <span data-ttu-id="cc337-109">[Common](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): un paquete común que se comparte entre remitentes y receptores.</span><span class="sxs-lookup"><span data-stu-id="cc337-109">[Common](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): A common package that is shared between senders and receivers.</span></span>

* <span data-ttu-id="cc337-110">[Remitente](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): un conjunto de paquetes que admiten enviar sus propios WebHooks a otros usuarios.</span><span class="sxs-lookup"><span data-stu-id="cc337-110">[Sender](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): A set of packages supporting sending your own WebHooks to others.</span></span> <span data-ttu-id="cc337-111">La funcionalidad para enviar los WebHooks se describe con más detalle en [WebHooks enviar](sending/index.md).</span><span class="sxs-lookup"><span data-stu-id="cc337-111">The functionality for sending WebHooks is described in more detail in [Sending WebHooks](sending/index.md).</span></span>

* <span data-ttu-id="cc337-112">[Receptores](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): un conjunto de paquetes que admiten reciban WebHooks de otros usuarios.</span><span class="sxs-lookup"><span data-stu-id="cc337-112">[Receivers](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): A set of packages supporting receiving WebHooks from others.</span></span> <span data-ttu-id="cc337-113">La funcionalidad para recibir los WebHooks se describe con más detalle en [WebHooks recibir](receiving/index.md).</span><span class="sxs-lookup"><span data-stu-id="cc337-113">The functionality for receiving WebHooks is described in more detail in [Receiving WebHooks](receiving/index.md).</span></span>
