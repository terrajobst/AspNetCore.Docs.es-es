---
title: "Artículos basados en los proyectos creados con cuentas de usuario individuales"
author: rick-anderson
description: "Este documento enumeran artículos basados en los proyectos creados con cuentas de usuario individuales."
keywords: "Núcleo de ASP.NET, autorización, IAuthorizationService"
ms.author: riande
manager: wpickett
ms.date: 11/30/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/individual
ms.openlocfilehash: 1864625e0ad6b4ec6fc2ada3fa7d93edec91b633
ms.sourcegitcommit: 037d3900f739dbaa2ba14158e3d7dc81478952ad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2017
---
# <a name="articles-based-on-projects-created-with-individual-user-accounts"></a><span data-ttu-id="3540f-104">Artículos basados en los proyectos creados con cuentas de usuario individuales</span><span class="sxs-lookup"><span data-stu-id="3540f-104">Articles based on projects created with individual user accounts</span></span>

<span data-ttu-id="3540f-105">Identidad de ASP.NET Core se incluye en las plantillas de proyecto en Visual Studio con la opción de "Cuentas de usuario individuales".</span><span class="sxs-lookup"><span data-stu-id="3540f-105">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="3540f-106">Las plantillas de autenticación están disponibles en .NET Core CLI con `-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="3540f-106">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

<span data-ttu-id="3540f-107">Los artículos siguientes muestran cómo usar el código generado en las plantillas de ASP.NET Core que utilizan cuentas de usuario individuales:</span><span class="sxs-lookup"><span data-stu-id="3540f-107">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="3540f-108">Autenticación en dos fases con SMS</span><span class="sxs-lookup"><span data-stu-id="3540f-108">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="3540f-109">Confirmación de las cuentas y recuperación de contraseñas en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3540f-109">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="3540f-110">Crear una aplicación de ASP.NET Core con datos de usuario protegidos por autorización</span><span class="sxs-lookup"><span data-stu-id="3540f-110">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)