---
title: Artículos basados en los proyectos de ASP.NET Core creados con cuentas de usuario individuales
author: rick-anderson
description: Detectar artículos basados en los proyectos de ASP.NET Core creados con cuentas de usuario individuales.
manager: wpickett
ms.author: riande
ms.date: 11/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/individual
ms.openlocfilehash: 699def0133f53b922477ac294f70db41998945ef
ms.sourcegitcommit: a0b6319c36f41cdce76ea334372f6e14fc66507e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/02/2018
ms.locfileid: "34729555"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a>Artículos basados en los proyectos de ASP.NET Core creados con cuentas de usuario individuales

Identidad de ASP.NET Core se incluye en las plantillas de proyecto en Visual Studio con la opción de "Cuentas de usuario individuales".

Las plantillas de autenticación están disponibles en .NET Core CLI con `-au Individual`:

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new webapp -au Individual
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

::: moniker-end

Los artículos siguientes muestran cómo usar el código generado en las plantillas de ASP.NET Core que utilizan cuentas de usuario individuales:

* [Autenticación en dos fases con SMS](xref:security/authentication/2fa)
* [Confirmación de las cuentas y recuperación de contraseñas en ASP.NET Core](xref:security/authentication/accconfirm)
* [Crear una aplicación de ASP.NET Core con datos de usuario protegidos por autorización](xref:security/authorization/secure-data)
