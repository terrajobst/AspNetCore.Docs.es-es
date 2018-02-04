---
title: "Exigir SSL en una aplicación ASP.NET básica"
author: rick-anderson
description: "Aplicación web se muestra cómo requerir SSL en un núcleo de ASP.NET"
manager: wpickett
ms.author: riande
ms.date: 07/19/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: 3b72cddb7a240ad6d6e1427796e9bb4f7003a3f7
ms.sourcegitcommit: 7a87d66cf1d01febe6635c7306f2f679434901d1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/03/2018
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a><span data-ttu-id="5a483-103">Exigir SSL en una aplicación ASP.NET básica</span><span class="sxs-lookup"><span data-stu-id="5a483-103">Enforcing SSL in an ASP.NET Core app</span></span>

<span data-ttu-id="5a483-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5a483-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5a483-105">Este documento se muestra cómo:</span><span class="sxs-lookup"><span data-stu-id="5a483-105">This document shows how to:</span></span>

- <span data-ttu-id="5a483-106">Requerir SSL para todas las solicitudes (sólo solicitudes HTTPS).</span><span class="sxs-lookup"><span data-stu-id="5a483-106">Require SSL for all requests (HTTPS requests only).</span></span>
- <span data-ttu-id="5a483-107">Redirigir todas las solicitudes HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="5a483-107">Redirect all HTTP requests to HTTPS.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="5a483-108">Requerir SSL</span><span class="sxs-lookup"><span data-stu-id="5a483-108">Require SSL</span></span>

<span data-ttu-id="5a483-109">El [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) se usa para requerir SSL.</span><span class="sxs-lookup"><span data-stu-id="5a483-109">The [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require SSL.</span></span> <span data-ttu-id="5a483-110">Puede decorar controladores o métodos con este atributo o se puede aplicar a todo el mundo tal y como se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="5a483-110">You can decorate controllers or methods with this attribute or you can apply it globally as shown below:</span></span>

<span data-ttu-id="5a483-111">Agregue el código siguiente a `ConfigureServices` en `Startup`:</span><span class="sxs-lookup"><span data-stu-id="5a483-111">Add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="5a483-112">El código resaltado anterior requiere que todas las solicitudes usar `HTTPS`, por lo tanto, se omiten las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="5a483-112">The highlighted code above requires all requests use `HTTPS`, therefore HTTP requests are ignored.</span></span> <span data-ttu-id="5a483-113">El código resaltado siguiente redirige todas las solicitudes HTTP a HTTPS:</span><span class="sxs-lookup"><span data-stu-id="5a483-113">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="5a483-114">Vea [Middleware de reescritura de dirección URL](xref:fundamentals/url-rewriting) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="5a483-114">See [URL Rewriting Middleware](xref:fundamentals/url-rewriting) for more information.</span></span>

<span data-ttu-id="5a483-115">Requerir HTTPS globalmente (`options.Filters.Add(new RequireHttpsAttribute());`) es una práctica recomendada de seguridad.</span><span class="sxs-lookup"><span data-stu-id="5a483-115">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="5a483-116">Aplicar el `[RequireHttps]` atributo para todos los controladores no considera tan seguro como requerir HTTPS globalmente.</span><span class="sxs-lookup"><span data-stu-id="5a483-116">Applying the `[RequireHttps]` attribute to all controller isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="5a483-117">No puede garantizar nuevos controladores que se agregan a la aplicación le recordará que se aplicará la `[RequireHttps]` atributo.</span><span class="sxs-lookup"><span data-stu-id="5a483-117">You can't guarantee new controllers added to your app will remember to apply the `[RequireHttps]` attribute.</span></span>
