---
uid: webhooks/receiving/dependencies
title: Dependencias del receptor de ASP.NET WebHooks | Microsoft Docs
author: rick-anderson
description: Dependencias del receptor y la inserción de dependencias en ASP.NET WebHooks.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.technology: ''
ms.openlocfilehash: cf45e3d2e45e4b7882b42d9aa0931e18c08f76b5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37401193"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a><span data-ttu-id="7d0f6-103">Dependencias del receptor de WebHooks de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="7d0f6-103">ASP.NET WebHooks receiver dependencies</span></span>

<span data-ttu-id="7d0f6-104">Microsoft ASP.NET WebHooks está diseñado con la inserción de dependencias en mente.</span><span class="sxs-lookup"><span data-stu-id="7d0f6-104">Microsoft ASP.NET WebHooks is designed with dependency injection in mind.</span></span> <span data-ttu-id="7d0f6-105">La mayoría de las dependencias en el sistema se pueden reemplazar con implementaciones alternativas mediante un motor de inyección de dependencia.</span><span class="sxs-lookup"><span data-stu-id="7d0f6-105">Most dependencies in the system can be replaced with alternative implementations using a dependency injection engine.</span></span>

<span data-ttu-id="7d0f6-106">Consulte [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) para obtener una lista de dependencias del receptor.</span><span class="sxs-lookup"><span data-stu-id="7d0f6-106">Please see [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) for a list of receiver dependencies.</span></span> <span data-ttu-id="7d0f6-107">Si no se ha registrado ninguna dependencia, se usa una implementación predeterminada.</span><span class="sxs-lookup"><span data-stu-id="7d0f6-107">If no dependency has been registered, a default implementation is used.</span></span> <span data-ttu-id="7d0f6-108">Consulte [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) para obtener una lista de las implementaciones predeterminadas.</span><span class="sxs-lookup"><span data-stu-id="7d0f6-108">Please see [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) for a list of default implementations.</span></span>
