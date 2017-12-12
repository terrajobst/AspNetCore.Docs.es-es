---
title: "Exigir SSL en una aplicación ASP.NET básica"
author: rick-anderson
description: "Aplicación web se muestra cómo requerir SSL en un núcleo de ASP.NET"
keywords: "Núcleo de ASP.NET, SSL, HTTPS, RequireHttpsAttribute, IIS Express"
ms.author: riande
manager: wpickett
ms.date: 07/19/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/enforcing-ssl
ms.openlocfilehash: 6f2755a606000717ca8a57f045b1ef613c7f14f6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a><span data-ttu-id="8a538-104">Exigir SSL en una aplicación ASP.NET básica</span><span class="sxs-lookup"><span data-stu-id="8a538-104">Enforcing SSL in an ASP.NET Core app</span></span>

<span data-ttu-id="8a538-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8a538-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8a538-106">Este documento se muestra cómo:</span><span class="sxs-lookup"><span data-stu-id="8a538-106">This document shows how to:</span></span>

- <span data-ttu-id="8a538-107">Requerir SSL para todas las solicitudes (sólo solicitudes HTTPS).</span><span class="sxs-lookup"><span data-stu-id="8a538-107">Require SSL for all requests (HTTPS requests only).</span></span>
- <span data-ttu-id="8a538-108">Redirigir todas las solicitudes HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="8a538-108">Redirect all HTTP requests to HTTPS.</span></span>

## <a name="require-ssl"></a><span data-ttu-id="8a538-109">Requerir SSL</span><span class="sxs-lookup"><span data-stu-id="8a538-109">Require SSL</span></span>

<span data-ttu-id="8a538-110">El [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) se usa para requerir SSL.</span><span class="sxs-lookup"><span data-stu-id="8a538-110">The [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require SSL.</span></span> <span data-ttu-id="8a538-111">Puede decorar controladores o métodos con este atributo o se puede aplicar a todo el mundo tal y como se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="8a538-111">You can decorate controllers or methods with this attribute or you can apply it globally as shown below:</span></span>

<span data-ttu-id="8a538-112">Agregue el código siguiente a `ConfigureServices` en `Startup`:</span><span class="sxs-lookup"><span data-stu-id="8a538-112">Add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-)]

<span data-ttu-id="8a538-113">El código resaltado anterior requiere que todas las solicitudes usar `HTTPS`, por lo tanto, se omiten las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="8a538-113">The highlighted code above requires all requests use `HTTPS`, therefore HTTP requests are ignored.</span></span> <span data-ttu-id="8a538-114">El código resaltado siguiente redirige todas las solicitudes HTTP a HTTPS:</span><span class="sxs-lookup"><span data-stu-id="8a538-114">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-)]

<span data-ttu-id="8a538-115">Vea [Middleware de reescritura de dirección URL](xref:fundamentals/url-rewriting) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="8a538-115">See [URL Rewriting Middleware](xref:fundamentals/url-rewriting) for more information.</span></span>

<span data-ttu-id="8a538-116">Requerir HTTPS globalmente (`options.Filters.Add(new RequireHttpsAttribute());`) es una práctica recomendada de seguridad.</span><span class="sxs-lookup"><span data-stu-id="8a538-116">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="8a538-117">Aplicar el `[RequireHttps]` atributo para todos los controladores no se considera tan seguro como requerir HTTPS globalmente.</span><span class="sxs-lookup"><span data-stu-id="8a538-117">Applying the `[RequireHttps]` attribute to all controller is not considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="8a538-118">No puede garantizar nuevos controladores que se agregan a la aplicación le recordará que se aplicará la `[RequireHttps]` atributo.</span><span class="sxs-lookup"><span data-stu-id="8a538-118">You can't guarantee new controllers added to your app will remember to apply the `[RequireHttps]` attribute.</span></span>
