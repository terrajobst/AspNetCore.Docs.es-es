---
title: Ejemplos de autenticación para ASP.NET Core
author: rick-anderson
description: Proporciona vínculos a los ejemplos de autenticación en el repositorio de ASP.NET Core.
ms.author: riande
ms.date: 01/31/2019
uid: security/authentication/samples
ms.openlocfilehash: d49aef198e926d88f1a6727f84b06f0861c8812d
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/23/2019
ms.locfileid: "71187299"
---
# <a name="authentication-samples-for-aspnet-core"></a><span data-ttu-id="50e6a-103">Ejemplos de autenticación para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="50e6a-103">Authentication samples for ASP.NET Core</span></span>

<span data-ttu-id="50e6a-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="50e6a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="50e6a-105">El [repositorio ASP.net Core](https://github.com/aspnet/AspNetCore) contiene los siguientes ejemplos de autenticación en la carpeta *AspNetCore/src/Security/samples* :</span><span class="sxs-lookup"><span data-stu-id="50e6a-105">The [ASP.NET Core repository](https://github.com/aspnet/AspNetCore) contains the following authentication samples in the *AspNetCore/src/Security/samples* folder:</span></span>

* [<span data-ttu-id="50e6a-106">Transformación de notificaciones</span><span class="sxs-lookup"><span data-stu-id="50e6a-106">Claims transformation</span></span>](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Security/samples/ClaimsTransformation)
* [<span data-ttu-id="50e6a-107">Autenticación de cookies</span><span class="sxs-lookup"><span data-stu-id="50e6a-107">Cookie authentication</span></span>](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Security/samples/Cookies)
* [<span data-ttu-id="50e6a-108">Proveedor de directivas personalizadas: IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="50e6a-108">Custom policy provider - IAuthorizationPolicyProvider</span></span>](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Security/samples/CustomPolicyProvider)
* [<span data-ttu-id="50e6a-109">Opciones y esquemas de autenticación dinámica</span><span class="sxs-lookup"><span data-stu-id="50e6a-109">Dynamic authentication schemes and options</span></span>](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Security/samples/DynamicSchemes)
* [<span data-ttu-id="50e6a-110">Notificaciones externas</span><span class="sxs-lookup"><span data-stu-id="50e6a-110">External claims</span></span>](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Security/samples/Identity.ExternalClaims)
* [<span data-ttu-id="50e6a-111">Selección entre cookies y otro esquema de autenticación en función de la solicitud</span><span class="sxs-lookup"><span data-stu-id="50e6a-111">Selecting between cookie and another authentication scheme based on the request</span></span>](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Security/samples/PathSchemeSelection)
* [<span data-ttu-id="50e6a-112">Restringe el acceso a los archivos estáticos</span><span class="sxs-lookup"><span data-stu-id="50e6a-112">Restricts access to static files</span></span>](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a><span data-ttu-id="50e6a-113">Ejecutar los ejemplos</span><span class="sxs-lookup"><span data-stu-id="50e6a-113">Run the samples</span></span>

* <span data-ttu-id="50e6a-114">Seleccione una [rama](https://github.com/aspnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="50e6a-114">Select a [branch](https://github.com/aspnet/AspNetCore).</span></span> <span data-ttu-id="50e6a-115">Por ejemplo, `Tag:v3.0.0`.</span><span class="sxs-lookup"><span data-stu-id="50e6a-115">For example, `Tag:v3.0.0`</span></span>
* <span data-ttu-id="50e6a-116">Clone o descargue el [repositorio de ASP.net Core](https://github.com/aspnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="50e6a-116">Clone or download the [ASP.NET Core repository](https://github.com/aspnet/AspNetCore).</span></span>
* <span data-ttu-id="50e6a-117">Compruebe que ha instalado la versión [SDK de .net Core](https://www.microsoft.com/net/download/all) que coincide con el clon del repositorio de ASP.net Core.</span><span class="sxs-lookup"><span data-stu-id="50e6a-117">Verify you have installed the [.NET Core SDK](https://www.microsoft.com/net/download/all) version matching the clone of the ASP.NET Core repository.</span></span>
* <span data-ttu-id="50e6a-118">Navegue a un ejemplo en *AspNetCore/src/Security/samples* y ejecute el ejemplo `dotnet run`con.</span><span class="sxs-lookup"><span data-stu-id="50e6a-118">Navigate to a sample in *AspNetCore/src/Security/samples* and run the sample with `dotnet run`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="50e6a-119">El [repositorio ASP.net Core](https://github.com/aspnet/AspNetCore) contiene los siguientes ejemplos de autenticación en la carpeta *AspNetCore/src/Security/samples* :</span><span class="sxs-lookup"><span data-stu-id="50e6a-119">The [ASP.NET Core repository](https://github.com/aspnet/AspNetCore) contains the following authentication samples in the *AspNetCore/src/Security/samples* folder:</span></span>

* [<span data-ttu-id="50e6a-120">Transformación de notificaciones</span><span class="sxs-lookup"><span data-stu-id="50e6a-120">Claims transformation</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/ClaimsTransformation)
* [<span data-ttu-id="50e6a-121">Autenticación de cookies</span><span class="sxs-lookup"><span data-stu-id="50e6a-121">Cookie authentication</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Cookies)
* [<span data-ttu-id="50e6a-122">Proveedor de directivas personalizadas: IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="50e6a-122">Custom policy provider - IAuthorizationPolicyProvider</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)
* [<span data-ttu-id="50e6a-123">Opciones y esquemas de autenticación dinámica</span><span class="sxs-lookup"><span data-stu-id="50e6a-123">Dynamic authentication schemes and options</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/DynamicSchemes)
* [<span data-ttu-id="50e6a-124">Notificaciones externas</span><span class="sxs-lookup"><span data-stu-id="50e6a-124">External claims</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Identity.ExternalClaims)
* [<span data-ttu-id="50e6a-125">Selección entre cookies y otro esquema de autenticación en función de la solicitud</span><span class="sxs-lookup"><span data-stu-id="50e6a-125">Selecting between cookie and another authentication scheme based on the request</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/PathSchemeSelection)
* [<span data-ttu-id="50e6a-126">Restringe el acceso a los archivos estáticos</span><span class="sxs-lookup"><span data-stu-id="50e6a-126">Restricts access to static files</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a><span data-ttu-id="50e6a-127">Ejecutar los ejemplos</span><span class="sxs-lookup"><span data-stu-id="50e6a-127">Run the samples</span></span>

* <span data-ttu-id="50e6a-128">Seleccione una [rama](https://github.com/aspnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="50e6a-128">Select a [branch](https://github.com/aspnet/AspNetCore).</span></span> <span data-ttu-id="50e6a-129">Por ejemplo, `release/2.2`.</span><span class="sxs-lookup"><span data-stu-id="50e6a-129">For example, `release/2.2`</span></span>
* <span data-ttu-id="50e6a-130">Clone o descargue el [repositorio de ASP.net Core](https://github.com/aspnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="50e6a-130">Clone or download the [ASP.NET Core repository](https://github.com/aspnet/AspNetCore).</span></span>
* <span data-ttu-id="50e6a-131">Compruebe que ha instalado la versión [SDK de .net Core](https://www.microsoft.com/net/download/all) que coincide con el clon del repositorio de ASP.net Core.</span><span class="sxs-lookup"><span data-stu-id="50e6a-131">Verify you have installed the [.NET Core SDK](https://www.microsoft.com/net/download/all) version matching the clone of the ASP.NET Core repository.</span></span>
* <span data-ttu-id="50e6a-132">Navegue a un ejemplo en *AspNetCore/src/Security/samples* y ejecute el ejemplo `dotnet run`con.</span><span class="sxs-lookup"><span data-stu-id="50e6a-132">Navigate to a sample in *AspNetCore/src/Security/samples* and run the sample with `dotnet run`.</span></span>

::: moniker-end
