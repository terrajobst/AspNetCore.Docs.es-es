---
title: Exigir HTTPS en un núcleo de ASP.NET
author: rick-anderson
description: Muestra cómo requerir HTTPS/TLS en un núcleo de ASP.NET de aplicación web.
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: 2ebb975e1ea17698cee13ca00d3f5df4a5135e38
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2018
---
# <a name="enforce-https-in-an-aspnet-core"></a><span data-ttu-id="94b65-103">Exigir HTTPS en un núcleo de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="94b65-103">Enforce HTTPS in an ASP.NET Core</span></span>

<span data-ttu-id="94b65-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="94b65-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="94b65-105">Este documento se muestra cómo:</span><span class="sxs-lookup"><span data-stu-id="94b65-105">This document shows how to:</span></span>

- <span data-ttu-id="94b65-106">Requerir HTTPS para todas las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="94b65-106">Require HTTPS for all requests.</span></span>
- <span data-ttu-id="94b65-107">Redirigir todas las solicitudes HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="94b65-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="94b65-108">Hacer **no** usar `RequireHttpsAttribute` en las API Web que reciben información confidencial.</span><span class="sxs-lookup"><span data-stu-id="94b65-108">Do **not** use `RequireHttpsAttribute` on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="94b65-109">`RequireHttpsAttribute` usa códigos de estado HTTP para redirigir exploradores de HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="94b65-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="94b65-110">Los clientes de API no pueden entender o siguen las redirecciones de HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="94b65-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="94b65-111">Estos clientes pueden enviar información a través de HTTP.</span><span class="sxs-lookup"><span data-stu-id="94b65-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="94b65-112">Las API Web deben realizar las tareas:</span><span class="sxs-lookup"><span data-stu-id="94b65-112">Web APIs should either:</span></span>
>
>* <span data-ttu-id="94b65-113">No escuchar en HTTP.</span><span class="sxs-lookup"><span data-stu-id="94b65-113">Not listen on HTTP.</span></span>
>* <span data-ttu-id="94b65-114">Cierre la conexión con el código de estado 400 (solicitud incorrecta) y no atender la solicitud.</span><span class="sxs-lookup"><span data-stu-id="94b65-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

## <a name="require-https"></a><span data-ttu-id="94b65-115">Requerir HTTPS</span><span class="sxs-lookup"><span data-stu-id="94b65-115">Require HTTPS</span></span>

<span data-ttu-id="94b65-116">El [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) se usa para requerir HTTPS.</span><span class="sxs-lookup"><span data-stu-id="94b65-116">The [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) is used to require HTTPS.</span></span> <span data-ttu-id="94b65-117">`[RequireHttpsAttribute]` puede decorar controladores o métodos, o se pueden aplicar globalmente.</span><span class="sxs-lookup"><span data-stu-id="94b65-117">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="94b65-118">Para aplicar el atributo global, agregue el código siguiente a `ConfigureServices` en `Startup`:</span><span class="sxs-lookup"><span data-stu-id="94b65-118">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="94b65-119">El código resaltado anterior requiere que todas las solicitudes usar `HTTPS`; por lo tanto, se omiten las solicitudes HTTP.</span><span class="sxs-lookup"><span data-stu-id="94b65-119">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="94b65-120">El código resaltado siguiente redirige todas las solicitudes HTTP a HTTPS:</span><span class="sxs-lookup"><span data-stu-id="94b65-120">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="94b65-121">Para obtener más información, consulte [Middleware de reescritura de dirección URL](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="94b65-121">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="94b65-122">Requerir HTTPS globalmente (`options.Filters.Add(new RequireHttpsAttribute());`) es una práctica recomendada de seguridad.</span><span class="sxs-lookup"><span data-stu-id="94b65-122">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="94b65-123">Aplicar el `[RequireHttps]` atributo a todas las páginas de Razor/controladores no considera tan seguro como requerir HTTPS globalmente.</span><span class="sxs-lookup"><span data-stu-id="94b65-123">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="94b65-124">No puede garantizar la `[RequireHttps]` atributo se aplica cuando se agregan nuevos controladores y las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="94b65-124">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>
