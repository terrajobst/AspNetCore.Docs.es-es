---
title: "Artículos basados en los proyectos creados con cuentas de usuario individuales"
author: rick-anderson
description: "Este documento enumeran artículos basados en los proyectos creados con cuentas de usuario individuales."
ms.author: riande
manager: wpickett
ms.date: 11/30/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/individual
ms.openlocfilehash: 844514f2970b761ec65589765eb21421cd1962a1
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="articles-based-on-projects-created-with-individual-user-accounts"></a>Artículos basados en los proyectos creados con cuentas de usuario individuales

Identidad de ASP.NET Core se incluye en las plantillas de proyecto en Visual Studio con la opción de "Cuentas de usuario individuales".

Las plantillas de autenticación están disponibles en .NET Core CLI con `-au Individual`:

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

Los artículos siguientes muestran cómo usar el código generado en las plantillas de ASP.NET Core que utilizan cuentas de usuario individuales:

* [Autenticación en dos fases con SMS](xref:security/authentication/2fa)
* [Confirmación de las cuentas y recuperación de contraseñas en ASP.NET Core](xref:security/authentication/accconfirm)
* [Crear una aplicación de ASP.NET Core con datos de usuario protegidos por autorización](xref:security/authorization/secure-data)
