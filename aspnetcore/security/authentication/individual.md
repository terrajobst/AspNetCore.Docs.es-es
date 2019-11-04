---
title: Artículos basados en proyectos de ASP.NET Core creados con cuentas de usuario individuales
author: rick-anderson
description: Descubra artículos basados en ASP.NET Core proyectos creados con cuentas de usuario individuales.
ms.author: riande
ms.date: 11/30/2017
uid: security/authentication/individual
ms.openlocfilehash: 91c5665dc50124b3ba09bdcfbf3ba501f684c604
ms.sourcegitcommit: 9e85c2562df5e108d7933635c830297f484bb775
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73463037"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a><span data-ttu-id="27772-103">Artículos basados en proyectos de ASP.NET Core creados con cuentas de usuario individuales</span><span class="sxs-lookup"><span data-stu-id="27772-103">Articles based on ASP.NET Core projects created with individual user accounts</span></span>

<span data-ttu-id="27772-104">ASP.NET Core identidad se incluye en las plantillas de proyecto de Visual Studio con la opción "cuentas de usuario individuales".</span><span class="sxs-lookup"><span data-stu-id="27772-104">ASP.NET Core Identity is included in project templates in Visual Studio with the "Individual User Accounts" option.</span></span>

<span data-ttu-id="27772-105">Las plantillas de autenticación están disponibles en CLI de .NET Core con `-au Individual`:</span><span class="sxs-lookup"><span data-stu-id="27772-105">The authentication templates are available in .NET Core CLI with `-au Individual`:</span></span>

::: moniker range=">= aspnetcore-2.1"

```dotnetcli
dotnet new mvc -au Individual
dotnet new webapp -au Individual
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```dotnetcli
dotnet new mvc -au Individual
dotnet new razor -au Individual
```

::: moniker-end

<span data-ttu-id="27772-106">Consulte [este problema de github para la](https://github.com/aspnet/AspNetCore/issues/5833) autenticación de API Web.</span><span class="sxs-lookup"><span data-stu-id="27772-106">See [this GitHub issue](https://github.com/aspnet/AspNetCore/issues/5833) for web API authentication.</span></span>

<a name="no"></a>

## <a name="no-authentication"></a><span data-ttu-id="27772-107">Sin autenticación</span><span class="sxs-lookup"><span data-stu-id="27772-107">No Authentication</span></span>

<span data-ttu-id="27772-108">La autenticación se especifica en el CLI de .NET Core con la opción `-au`.</span><span class="sxs-lookup"><span data-stu-id="27772-108">Authentication is specified in the .NET Core CLI with the `-au` option.</span></span> <span data-ttu-id="27772-109">En Visual Studio, el cuadro de diálogo **cambiar autenticación** está disponible para las nuevas aplicaciones Web.</span><span class="sxs-lookup"><span data-stu-id="27772-109">In Visual Studio, the **Change Authentication** dialog is available for new web applications.</span></span> <span data-ttu-id="27772-110">El valor predeterminado para las nuevas aplicaciones web en Visual Studio **no es autenticación**.</span><span class="sxs-lookup"><span data-stu-id="27772-110">The default for new web apps in Visual Studio is **No Authentication**.</span></span>

<span data-ttu-id="27772-111">Proyectos creados sin autenticación:</span><span class="sxs-lookup"><span data-stu-id="27772-111">Projects created with no authentication:</span></span>

* <span data-ttu-id="27772-112">No contenga páginas web y la interfaz de usuario para iniciar sesión y cerrar sesión.</span><span class="sxs-lookup"><span data-stu-id="27772-112">Don't contain web pages and UI to sign in and sign out.</span></span>
* <span data-ttu-id="27772-113">No contenga código de autenticación.</span><span class="sxs-lookup"><span data-stu-id="27772-113">Don't contain authentication code.</span></span>

<a name="win"></a>

## <a name="windows-authentication"></a><span data-ttu-id="27772-114">Autenticación de Windows</span><span class="sxs-lookup"><span data-stu-id="27772-114">Windows Authentication</span></span>

<span data-ttu-id="27772-115">La autenticación de Windows se especifica para las nuevas aplicaciones web en el CLI de .NET Core con la opción `-au Windows`.</span><span class="sxs-lookup"><span data-stu-id="27772-115">Windows Authentication is specified for new web apps in the .NET Core CLI with the `-au Windows` option.</span></span> <span data-ttu-id="27772-116">En Visual Studio, el cuadro de diálogo **cambiar autenticación** proporciona las opciones de **autenticación de Windows** .</span><span class="sxs-lookup"><span data-stu-id="27772-116">In Visual Studio, the **Change Authentication** dialog provides the **Windows Authentication** options.</span></span>

<span data-ttu-id="27772-117">Si se selecciona autenticación de Windows, la aplicación se configura para usar el [módulo IIS de autenticación de Windows](xref:host-and-deploy/iis/modules).</span><span class="sxs-lookup"><span data-stu-id="27772-117">If Windows Authentication is selected, the app is configured to use the [Windows Authentication IIS module](xref:host-and-deploy/iis/modules).</span></span> <span data-ttu-id="27772-118">La autenticación de Windows está pensada para sitios web de la intranet.</span><span class="sxs-lookup"><span data-stu-id="27772-118">Windows Authentication is intended for Intranet web sites.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="27772-119">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="27772-119">Additional resources</span></span>

<span data-ttu-id="27772-120">En los artículos siguientes se muestra cómo usar el código generado en ASP.NET Core plantillas que utilizan cuentas de usuario individuales:</span><span class="sxs-lookup"><span data-stu-id="27772-120">The following articles show how to use the code generated in ASP.NET Core templates that use individual user accounts:</span></span>

* [<span data-ttu-id="27772-121">Confirmación de las cuentas y recuperación de contraseñas en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="27772-121">Account confirmation and password recovery in ASP.NET Core</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="27772-122">Creación de una aplicación ASP.NET Core con datos de usuario protegidos por autorización</span><span class="sxs-lookup"><span data-stu-id="27772-122">Create an ASP.NET Core app with user data protected by authorization</span></span>](xref:security/authorization/secure-data)
