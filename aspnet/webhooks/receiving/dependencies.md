---
uid: webhooks/receiving/dependencies
title: Dependencias del receptor de ASP.NET WebHooks | Microsoft Docs
author: rick-anderson
description: Dependencias del receptor y la inserción de dependencias en ASP.NET WebHooks.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.openlocfilehash: c44cfe3ed310aa728a989b108c410e8786e4f514
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828347"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a><span data-ttu-id="823dd-103">Dependencias del receptor de WebHooks de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="823dd-103">ASP.NET WebHooks receiver dependencies</span></span>

<span data-ttu-id="823dd-104">Microsoft ASP.NET WebHooks está diseñado con la inserción de dependencias en mente.</span><span class="sxs-lookup"><span data-stu-id="823dd-104">Microsoft ASP.NET WebHooks is designed with dependency injection in mind.</span></span> <span data-ttu-id="823dd-105">La mayoría de las dependencias en el sistema se pueden reemplazar con implementaciones alternativas mediante un motor de inyección de dependencia.</span><span class="sxs-lookup"><span data-stu-id="823dd-105">Most dependencies in the system can be replaced with alternative implementations using a dependency injection engine.</span></span>

<span data-ttu-id="823dd-106">Consulte [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) para obtener una lista de dependencias del receptor.</span><span class="sxs-lookup"><span data-stu-id="823dd-106">Please see [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) for a list of receiver dependencies.</span></span> <span data-ttu-id="823dd-107">Si no se ha registrado ninguna dependencia, se usa una implementación predeterminada.</span><span class="sxs-lookup"><span data-stu-id="823dd-107">If no dependency has been registered, a default implementation is used.</span></span> <span data-ttu-id="823dd-108">Consulte [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) para obtener una lista de las implementaciones predeterminadas.</span><span class="sxs-lookup"><span data-stu-id="823dd-108">Please see [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) for a list of default implementations.</span></span>
