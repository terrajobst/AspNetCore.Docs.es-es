---
uid: webhooks/receiving/dependencies
title: Dependencias del receptor de ASP.NET WebHooks | Microsoft Docs
author: rick-anderson
description: Dependencias del receptor y la inserción de dependencias en ASP.NET WebHooks.
ms.author: aspnetcontent
ms.date: 01/17/2012
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.openlocfilehash: 05dcfac121e7974fd83c5b3736616479574944a1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817842"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a>Dependencias del receptor de WebHooks de ASP.NET

Microsoft ASP.NET WebHooks está diseñado con la inserción de dependencias en mente. La mayoría de las dependencias en el sistema se pueden reemplazar con implementaciones alternativas mediante un motor de inyección de dependencia.

Consulte [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) para obtener una lista de dependencias del receptor. Si no se ha registrado ninguna dependencia, se usa una implementación predeterminada. Consulte [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) para obtener una lista de las implementaciones predeterminadas.
