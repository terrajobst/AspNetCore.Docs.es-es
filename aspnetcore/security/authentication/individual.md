---
title: Artículos basados en los proyectos de ASP.NET Core creados con cuentas de usuario individuales
author: rick-anderson
description: Descubra artículos basados en los proyectos de ASP.NET Core creados con cuentas de usuario individuales.
ms.author: riande
ms.date: 11/30/2017
uid: security/authentication/individual
ms.openlocfilehash: ac843342ffc73632fbf9f6359c6c1a5878dcef0d
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523069"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a><span data-ttu-id="41d0c-103">Artículos basados en los proyectos de ASP.NET Core creados con cuentas de usuario individuales</span><span class="sxs-lookup"><span data-stu-id="41d0c-103">Articles based on ASP.NET Core projects created with individual user accounts</span></span>

<span data-ttu-id="41d0c-104">ASP.NET Core Identity se incluye en las plantillas de proyecto en Visual Studio con la opción "Cuentas de usuario individuales".</span><span class="sxs-lookup"><span data-stu-id="41d0c-104">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="41d0c-105">Las plantillas de autenticación están disponibles en la CLI de .NET Core con `-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="41d0c-105">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

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

<a name="no"></a>
## <a name="no-authentication"></a><span data-ttu-id="41d0c-106">Sin autenticación</span><span class="sxs-lookup"><span data-stu-id="41d0c-106">No Authentication</span></span>

<span data-ttu-id="41d0c-107">La autenticación se especifica en la CLI de .NET Core con el `-au` opción.</span><span class="sxs-lookup"><span data-stu-id="41d0c-107">Authentication is specified in the .NET Core CLI with the `-au` option.</span></span> <span data-ttu-id="41d0c-108">En Visual Studio, el **Cambiar autenticación** cuadro de diálogo está disponible para nuevas aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="41d0c-108">In Visual Studio, the **Change Authentication** dialog is available for new web applications.</span></span> <span data-ttu-id="41d0c-109">El valor predeterminado para nuevas aplicaciones web en Visual Studio es **sin autenticación**.</span><span class="sxs-lookup"><span data-stu-id="41d0c-109">The default for new web apps in Visual Studio is **No Authentication**.</span></span>

<span data-ttu-id="41d0c-110">Proyectos creados con ninguna autenticación:</span><span class="sxs-lookup"><span data-stu-id="41d0c-110">Projects created with no authentication:</span></span>

* <span data-ttu-id="41d0c-111">No contienen páginas web y la interfaz de usuario para iniciar sesión y cierre la sesión.</span><span class="sxs-lookup"><span data-stu-id="41d0c-111">Don't contain web pages and UI to sign in and sign out.</span></span>
* <span data-ttu-id="41d0c-112">No contienen código de autenticación.</span><span class="sxs-lookup"><span data-stu-id="41d0c-112">Don't contain authentication code.</span></span>

<a name="win"></a>
## <a name="windows-authentication"></a><span data-ttu-id="41d0c-113">Autenticación de Windows</span><span class="sxs-lookup"><span data-stu-id="41d0c-113">Windows Authentication</span></span>

<span data-ttu-id="41d0c-114">Se especifica la autenticación de Windows para las nuevas aplicaciones web en la CLI de .NET Core con el `-au Windows` opción.</span><span class="sxs-lookup"><span data-stu-id="41d0c-114">Windows Authentication is specified for new web apps in the .NET Core CLI with the `-au Windows` option.</span></span> <span data-ttu-id="41d0c-115">En Visual Studio, el **Cambiar autenticación** cuadro de diálogo proporciona el **Windows autenticación** opciones.</span><span class="sxs-lookup"><span data-stu-id="41d0c-115">In Visual Studio, the **Change Authentication** dialog provides the **Windows Authentication** options.</span></span>

<span data-ttu-id="41d0c-116">Si selecciona la autenticación de Windows, la aplicación está configurada para usar el [módulo de autenticación de Windows IIS](xref:host-and-deploy/iis/modules).</span><span class="sxs-lookup"><span data-stu-id="41d0c-116">If Windows Authentication is selected, the app is configured to use the [Windows Authentication IIS module](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="41d0c-117">Autenticación de Windows está diseñada para sitios web de Intranet.</span><span class="sxs-lookup"><span data-stu-id="41d0c-117">Windows Authentication is intended for Intranet web sites.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="41d0c-118">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="41d0c-118">Additional resources</span></span>

<span data-ttu-id="41d0c-119">Los artículos siguientes muestran cómo usar el código generado en las plantillas de ASP.NET Core que usan cuentas de usuario individuales:</span><span class="sxs-lookup"><span data-stu-id="41d0c-119">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="41d0c-120">Autenticación en dos fases con SMS</span><span class="sxs-lookup"><span data-stu-id="41d0c-120">Two-factor authentication with SMS</span></span>](xref:security/authentication/2fa)
* [<span data-ttu-id="41d0c-121">Confirmación de las cuentas y recuperación de contraseñas en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="41d0c-121">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="41d0c-122">Crear una aplicación ASP.NET Core con datos de usuario protegidos por autorización</span><span class="sxs-lookup"><span data-stu-id="41d0c-122">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
