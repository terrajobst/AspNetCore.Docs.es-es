---
uid: webhooks/receiving/dependencies
title: Dependencias de receptor de ASP.NET WebHooks | Documentos de Microsoft
author: rick-anderson
description: "Dependencias de receptor e inyección de dependencia en ASP.NET Webhook."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: f9726c746c8934594e26f2871f9b867c192374bb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-receiver-dependencies"></a><span data-ttu-id="eabe2-103">Dependencias de receptor de Webhook de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="eabe2-103">ASP.NET WebHooks receiver dependencies</span></span>

<span data-ttu-id="eabe2-104">Microsoft ASP.NET WebHooks está diseñado con la inserción de dependencias en cuenta.</span><span class="sxs-lookup"><span data-stu-id="eabe2-104">Microsoft ASP.NET WebHooks is designed with dependency injection in mind.</span></span> <span data-ttu-id="eabe2-105">La mayoría de las dependencias en el sistema pueden reemplazarse con implementaciones alternativas con un motor de inyección de dependencia.</span><span class="sxs-lookup"><span data-stu-id="eabe2-105">Most dependencies in the system can be replaced with alternative implementations using a dependency injection engine.</span></span>

<span data-ttu-id="eabe2-106">Vea [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) para obtener una lista de dependencias de receptor.</span><span class="sxs-lookup"><span data-stu-id="eabe2-106">Please see [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) for a list of receiver dependencies.</span></span> <span data-ttu-id="eabe2-107">Si no se ha registrado ninguna dependencia, se utiliza una implementación predeterminada.</span><span class="sxs-lookup"><span data-stu-id="eabe2-107">If no dependency has been registered, a default implementation is used.</span></span> <span data-ttu-id="eabe2-108">Vea [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) para obtener una lista de las implementaciones predeterminadas.</span><span class="sxs-lookup"><span data-stu-id="eabe2-108">Please see [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) for a list of default implementations.</span></span>
