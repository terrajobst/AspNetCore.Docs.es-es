---
title: Lista segura IP de cliente para ASP.NET Core
author: damienbod
description: Obtenga información sobre cómo escribir filtros de acción o Middleware para validar las direcciones IP remotas con una lista de direcciones IP aprobadas.
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: security/ip-safelist
ms.openlocfilehash: cfbb50ea33ae3af577f13b00bccc75fe0be57f79
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/27/2019
ms.locfileid: "64898152"
---
# <a name="client-ip-safelist-for-aspnet-core"></a><span data-ttu-id="92da0-103">Lista segura IP de cliente para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="92da0-103">Client IP safelist for ASP.NET Core</span></span>

<span data-ttu-id="92da0-104">Por [Damien Bowden](https://twitter.com/damien_bod) y [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="92da0-104">By [Damien Bowden](https://twitter.com/damien_bod) and [Tom Dykstra](https://github.com/tdykstra)</span></span>
 
<span data-ttu-id="92da0-105">En este artículo se muestra tres maneras de implementar un safelist IP (también conocido como una lista blanca) en una aplicación ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="92da0-105">This article shows three ways to implement an IP safelist (also known as a whitelist) in an ASP.NET Core app.</span></span> <span data-ttu-id="92da0-106">Puede usar:</span><span class="sxs-lookup"><span data-stu-id="92da0-106">You can use:</span></span>

* <span data-ttu-id="92da0-107">Software intermedio para comprobar la dirección IP remota de todas las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="92da0-107">Middleware to check the remote IP address of every request.</span></span>
* <span data-ttu-id="92da0-108">Filtros de acción para comprobar la dirección IP remota de las solicitudes de los métodos de acción o controladores concretos.</span><span class="sxs-lookup"><span data-stu-id="92da0-108">Action filters to check the remote IP address of requests for specific controllers or action methods.</span></span>
* <span data-ttu-id="92da0-109">Filtros de páginas de Razor para comprobar la dirección IP remota de solicitudes de páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="92da0-109">Razor Pages filters to check the remote IP address of requests for Razor pages.</span></span>

<span data-ttu-id="92da0-110">La aplicación de ejemplo muestra ambos enfoques.</span><span class="sxs-lookup"><span data-stu-id="92da0-110">The sample app illustrates both approaches.</span></span> <span data-ttu-id="92da0-111">En cada caso, una cadena que contiene las direcciones IP de cliente aprobadas se almacena en una configuración de aplicación.</span><span class="sxs-lookup"><span data-stu-id="92da0-111">In each case, a string containing approved client IP addresses is stored in an app setting.</span></span> <span data-ttu-id="92da0-112">El software intermedio o el filtro analiza la cadena en una lista y comprueba si la dirección IP remota está en la lista.</span><span class="sxs-lookup"><span data-stu-id="92da0-112">The middleware or filter parses the string into a list and  checks if the remote IP is in the list.</span></span> <span data-ttu-id="92da0-113">Si no es así, se devuelve un código de estado HTTP 403 Prohibido.</span><span class="sxs-lookup"><span data-stu-id="92da0-113">If not, an HTTP 403 Forbidden status code is returned.</span></span>

<span data-ttu-id="92da0-114">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="92da0-114">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="the-safelist"></a><span data-ttu-id="92da0-115">La lista segura</span><span class="sxs-lookup"><span data-stu-id="92da0-115">The safelist</span></span>

<span data-ttu-id="92da0-116">La lista está configurada en el *appsettings.json* archivo.</span><span class="sxs-lookup"><span data-stu-id="92da0-116">The list is configured in the *appsettings.json* file.</span></span> <span data-ttu-id="92da0-117">Es una lista delimitada por punto y coma y puede contener direcciones IPv4 e IPv6.</span><span class="sxs-lookup"><span data-stu-id="92da0-117">It's a semicolon-delimited list and can contain IPv4 and IPv6 addresses.</span></span>

[!code-json[](ip-safelist/samples/2.x/ClientIpAspNetCore/appsettings.json?highlight=2)]

## <a name="middleware"></a><span data-ttu-id="92da0-118">Software intermedio</span><span class="sxs-lookup"><span data-stu-id="92da0-118">Middleware</span></span>

<span data-ttu-id="92da0-119">El `Configure` método agrega el software intermedio y pasa la cadena de safelist a él en un parámetro de constructor.</span><span class="sxs-lookup"><span data-stu-id="92da0-119">The `Configure` method adds the middleware and passes the safelist string to it in a constructor parameter.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_Configure&highlight=7)]

<span data-ttu-id="92da0-120">El middleware analiza la cadena en una matriz y busca la dirección IP remota de la matriz.</span><span class="sxs-lookup"><span data-stu-id="92da0-120">The middleware parses the string into an array and looks for the remote IP address in the array.</span></span> <span data-ttu-id="92da0-121">Si no se encuentra la dirección IP remota, el middleware devuelve HTTP 401 prohibido.</span><span class="sxs-lookup"><span data-stu-id="92da0-121">If the remote IP address is not found, the middleware returns HTTP 401 Forbidden.</span></span> <span data-ttu-id="92da0-122">Este proceso de validación se omite para las solicitudes HTTP Get.</span><span class="sxs-lookup"><span data-stu-id="92da0-122">This validation process is bypassed for HTTP Get requests.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/AdminSafeListMiddleware.cs?name=snippet_ClassOnly)]

## <a name="action-filter"></a><span data-ttu-id="92da0-123">Filtro de acción</span><span class="sxs-lookup"><span data-stu-id="92da0-123">Action filter</span></span>

<span data-ttu-id="92da0-124">Si desea una lista segura solo para los métodos de acción o controladores concretos, use un filtro de acción.</span><span class="sxs-lookup"><span data-stu-id="92da0-124">If you want a safelist only for specific controllers or action methods, use an action filter.</span></span> <span data-ttu-id="92da0-125">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="92da0-125">Here's an example:</span></span> 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckFilter.cs)]

<span data-ttu-id="92da0-126">El filtro de acción se agrega al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="92da0-126">The action filter is added to the services container.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

<span data-ttu-id="92da0-127">El filtro, a continuación, puede usarse en un método de acción o controlador.</span><span class="sxs-lookup"><span data-stu-id="92da0-127">The filter can then be used on a controller or action method.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Controllers/ValuesController.cs?name=snippet_Filter&highlight=1)]

<span data-ttu-id="92da0-128">En la aplicación de ejemplo, el filtro se aplica a la `Get` método.</span><span class="sxs-lookup"><span data-stu-id="92da0-128">In the sample app, the filter is applied to the `Get` method.</span></span> <span data-ttu-id="92da0-129">Por lo que al probar la aplicación mediante el envío de un `Get` solicitud de API, el atributo está validando la dirección IP del cliente.</span><span class="sxs-lookup"><span data-stu-id="92da0-129">So when you test the app by sending a `Get` API request, the attribute is validating the client IP address.</span></span> <span data-ttu-id="92da0-130">Cuando se prueba mediante una llamada a la API con cualquier otro método HTTP, el middleware está validando la IP del cliente.</span><span class="sxs-lookup"><span data-stu-id="92da0-130">When you test by calling the API with any other HTTP method, the middleware is validating the client IP.</span></span>

## <a name="razor-pages-filter"></a><span data-ttu-id="92da0-131">Filtrar las páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="92da0-131">Razor Pages filter</span></span> 

<span data-ttu-id="92da0-132">Si desea una lista segura para una aplicación de páginas de Razor, use un filtro de las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="92da0-132">If you want a safelist for a Razor Pages app, use a Razor Pages filter.</span></span> <span data-ttu-id="92da0-133">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="92da0-133">Here's an example:</span></span> 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckPageFilter.cs)]

<span data-ttu-id="92da0-134">Este filtro está habilitado, éste se agrega a la colección de filtros de MVC.</span><span class="sxs-lookup"><span data-stu-id="92da0-134">This filter is enabled by adding it to the MVC Filters collection.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

<span data-ttu-id="92da0-135">Cuando ejecute la aplicación y solicite una página de Razor, el filtro de las páginas de Razor está validando la IP del cliente.</span><span class="sxs-lookup"><span data-stu-id="92da0-135">When you run the app and request a Razor page, the Razor Pages filter is validating the client IP.</span></span>

## <a name="next-steps"></a><span data-ttu-id="92da0-136">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="92da0-136">Next steps</span></span>

<span data-ttu-id="92da0-137">[Más información sobre el Middleware de ASP.NET Core](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="92da0-137">[Learn more about ASP.NET Core Middleware](xref:fundamentals/middleware/index).</span></span>
