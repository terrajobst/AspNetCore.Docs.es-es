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
ms.openlocfilehash: 40715debb48c0a7121ce84d7843b8517b0973e74
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2018
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a><span data-ttu-id="e6cca-103">Artículos basados en los proyectos de ASP.NET Core creados con cuentas de usuario individuales</span><span class="sxs-lookup"><span data-stu-id="e6cca-103">Articles based on ASP.NET Core projects created with individual user accounts</span></span>

<span data-ttu-id="e6cca-104">Identidad de ASP.NET Core se incluye en las plantillas de proyecto en Visual Studio con la opción de "Cuentas de usuario individuales".</span><span class="sxs-lookup"><span data-stu-id="e6cca-104">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="e6cca-105">Las plantillas de autenticación están disponibles en .NET Core CLI con `-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="e6cca-105">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

<span data-ttu-id="e6cca-106">Los artículos siguientes muestran cómo usar el código generado en las plantillas de ASP.NET Core que utilizan cuentas de usuario individuales:</span><span class="sxs-lookup"><span data-stu-id="e6cca-106">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="e6cca-107">Autenticación en dos fases con SMS</span><span class="sxs-lookup"><span data-stu-id="e6cca-107">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="e6cca-108">Confirmación de las cuentas y recuperación de contraseñas en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e6cca-108">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="e6cca-109">Crear una aplicación de ASP.NET Core con datos de usuario protegidos por autorización</span><span class="sxs-lookup"><span data-stu-id="e6cca-109">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
