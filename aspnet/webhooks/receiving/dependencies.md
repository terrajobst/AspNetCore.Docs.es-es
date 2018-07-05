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
# <a name="aspnet-webhooks-receiver-dependencies"></a>Dependencias del receptor de WebHooks de ASP.NET

Microsoft ASP.NET WebHooks está diseñado con la inserción de dependencias en mente. La mayoría de las dependencias en el sistema se pueden reemplazar con implementaciones alternativas mediante un motor de inyección de dependencia.

Consulte [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) para obtener una lista de dependencias del receptor. Si no se ha registrado ninguna dependencia, se usa una implementación predeterminada. Consulte [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) para obtener una lista de las implementaciones predeterminadas.
