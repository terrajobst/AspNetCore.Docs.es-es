---
uid: webhooks/receiving/dependencies
title: Dependencias de receptor de ASP.NET WebHooks | Documentos de Microsoft
author: rick-anderson
description: Dependencias de receptor e inyección de dependencia en ASP.NET Webhook.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.technology: ''
ms.prod: .net-framework
ms.openlocfilehash: f9726c746c8934594e26f2871f9b867c192374bb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
ms.locfileid: "26529914"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a>Dependencias de receptor de Webhook de ASP.NET

Microsoft ASP.NET WebHooks está diseñado con la inserción de dependencias en cuenta. La mayoría de las dependencias en el sistema pueden reemplazarse con implementaciones alternativas con un motor de inyección de dependencia.

Vea [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) para obtener una lista de dependencias de receptor. Si no se ha registrado ninguna dependencia, se utiliza una implementación predeterminada. Vea [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) para obtener una lista de las implementaciones predeterminadas.
