---
title: Esquemas de directivas en ASP.NET Core
author: rick-anderson
description: Esquemas de autenticación de directiva que sea más fácil tener un esquema de autenticación lógico único
ms.author: riande
ms.date: 2/28/2019
uid: security/authentication/policyschemes
ms.openlocfilehash: c310b61e14df2b7846e32a602bb75914a5850aff
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/27/2019
ms.locfileid: "64895202"
---
# <a name="policy-schemes-in-aspnet-core"></a><span data-ttu-id="2546e-103">Esquemas de directivas en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2546e-103">Policy schemes in ASP.NET Core</span></span>

<span data-ttu-id="2546e-104">Esquemas de la directiva de autenticación facilitan tiene un esquema de autenticación lógico único potencialmente usar varios enfoques.</span><span class="sxs-lookup"><span data-stu-id="2546e-104">Authentication policy schemes make it easier to have a single logical authentication scheme potentially use multiple approaches.</span></span> <span data-ttu-id="2546e-105">Por ejemplo, un esquema de la directiva podría usar autenticación de Google para enfrentar los desafíos y autenticación de cookies para todo lo demás.</span><span class="sxs-lookup"><span data-stu-id="2546e-105">For example, a policy scheme might use Google authentication for challenges, and cookie authentication for everything else.</span></span> <span data-ttu-id="2546e-106">Esquemas de autenticación de directiva hacen que sea:</span><span class="sxs-lookup"><span data-stu-id="2546e-106">Authentication policy schemes make it:</span></span>

* <span data-ttu-id="2546e-107">Fácil reenviar cualquier acción de autenticación a otro esquema.</span><span class="sxs-lookup"><span data-stu-id="2546e-107">Easy to forward any authentication action to another scheme.</span></span>
* <span data-ttu-id="2546e-108">Avance de manera dinámica basándose en la solicitud.</span><span class="sxs-lookup"><span data-stu-id="2546e-108">Forward dynamically based on the request.</span></span>

<span data-ttu-id="2546e-109">Todos los esquemas de autenticación que utilice derivados <xref:Microsoft.AspNetCore.Authentication.AuthenticationSchemeOptions> y asociado [ `AuthenticationHandler<TOptions>` ](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1):</span><span class="sxs-lookup"><span data-stu-id="2546e-109">All authentication schemes that use derived <xref:Microsoft.AspNetCore.Authentication.AuthenticationSchemeOptions> and the associated [`AuthenticationHandler<TOptions>`](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1):</span></span>

* <span data-ttu-id="2546e-110">Son automáticamente combinaciones de directivas en ASP.NET Core 2.1 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="2546e-110">Are automatically policy schemes in ASP.NET Core 2.1 and later.</span></span>
* <span data-ttu-id="2546e-111">Puede habilitarse a través de configuración de las opciones del esquema.</span><span class="sxs-lookup"><span data-stu-id="2546e-111">Can be enabled via configuring the scheme's options.</span></span>

[!code-csharp[sample](policyschemes/samples/AuthenticationSchemeOptions.cs?name=snippet)]

## <a name="examples"></a><span data-ttu-id="2546e-112">Ejemplos</span><span class="sxs-lookup"><span data-stu-id="2546e-112">Examples</span></span>

<span data-ttu-id="2546e-113">El ejemplo siguiente muestra un esquema de nivel superior que combina los esquemas de nivel inferior.</span><span class="sxs-lookup"><span data-stu-id="2546e-113">The following example shows a higher level scheme that combines lower level schemes.</span></span> <span data-ttu-id="2546e-114">Se utiliza la autenticación de Google para enfrentar los desafíos y se usa la autenticación de cookies para todo lo demás:</span><span class="sxs-lookup"><span data-stu-id="2546e-114">Google authentication is used for challenges, and cookie authentication is used for everything else:</span></span>

[!code-csharp[sample](policyschemes/samples/Startup.cs?name=snippet1)]

<span data-ttu-id="2546e-115">El ejemplo siguiente habilita la selección dinámica de esquemas en función de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="2546e-115">The following example enables dynamic selection of schemes on a per request basis.</span></span> <span data-ttu-id="2546e-116">Es decir, cómo se combinan las cookies y la autenticación de API.</span><span class="sxs-lookup"><span data-stu-id="2546e-116">That is, how to mix cookies and API authentication.</span></span>

 <!-- REVIEW, missing If set in public Func<HttpContext, string> ForwardDefaultSelector -->

[!code-csharp[sample](policyschemes/samples/Startup.cs?name=snippet2)]
