---
title: La dirección IP de cliente de la ASP.NET Core
author: damienbod
description: Aprenda a escribir filtros de middleware o de acción para validar direcciones IP remotas en una lista de direcciones IP aprobadas.
ms.author: riande
ms.custom: mvc
ms.date: 08/31/2018
uid: security/ip-safelist
ms.openlocfilehash: 02e44135ab1742d44691cfda8c4167f21d6efa4e
ms.sourcegitcommit: 8835b6777682da6fb3becf9f9121c03f89dc7614
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2019
ms.locfileid: "69975647"
---
# <a name="client-ip-safelist-for-aspnet-core"></a><span data-ttu-id="d38cb-103">La dirección IP de cliente de la ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d38cb-103">Client IP safelist for ASP.NET Core</span></span>

<span data-ttu-id="d38cb-104">Por [Damien Bowden](https://twitter.com/damien_bod) y [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="d38cb-104">By [Damien Bowden](https://twitter.com/damien_bod) and [Tom Dykstra](https://github.com/tdykstra)</span></span>
 
<span data-ttu-id="d38cb-105">En este artículo se muestran tres maneras de implementar una lista de permitidos de IP (también conocida como lista blanca) en una aplicación ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d38cb-105">This article shows three ways to implement an IP safelist (also known as a whitelist) in an ASP.NET Core app.</span></span> <span data-ttu-id="d38cb-106">Puede usar:</span><span class="sxs-lookup"><span data-stu-id="d38cb-106">You can use:</span></span>

* <span data-ttu-id="d38cb-107">Middleware para comprobar la dirección IP remota de cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="d38cb-107">Middleware to check the remote IP address of every request.</span></span>
* <span data-ttu-id="d38cb-108">Filtros de acción para comprobar la dirección IP remota de las solicitudes de controladores o métodos de acción específicos.</span><span class="sxs-lookup"><span data-stu-id="d38cb-108">Action filters to check the remote IP address of requests for specific controllers or action methods.</span></span>
* <span data-ttu-id="d38cb-109">Razor Pages filtros para comprobar la dirección IP remota de las solicitudes de páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="d38cb-109">Razor Pages filters to check the remote IP address of requests for Razor pages.</span></span>

<span data-ttu-id="d38cb-110">En cada caso, una cadena que contiene direcciones IP de cliente aprobadas se almacena en una configuración de aplicación.</span><span class="sxs-lookup"><span data-stu-id="d38cb-110">In each case, a string containing approved client IP addresses is stored in an app setting.</span></span> <span data-ttu-id="d38cb-111">El middleware o filtro analiza la cadena en una lista y comprueba si la dirección IP remota está en la lista.</span><span class="sxs-lookup"><span data-stu-id="d38cb-111">The middleware or filter parses the string into a list and checks if the remote IP is in the list.</span></span> <span data-ttu-id="d38cb-112">Si no es así, se devuelve un código de Estado HTTP 403 prohibido.</span><span class="sxs-lookup"><span data-stu-id="d38cb-112">If not, an HTTP 403 Forbidden status code is returned.</span></span>

<span data-ttu-id="d38cb-113">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d38cb-113">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="the-safelist"></a><span data-ttu-id="d38cb-114">La de la</span><span class="sxs-lookup"><span data-stu-id="d38cb-114">The safelist</span></span>

<span data-ttu-id="d38cb-115">La lista se configura en el archivo *appSettings. JSON* .</span><span class="sxs-lookup"><span data-stu-id="d38cb-115">The list is configured in the *appsettings.json* file.</span></span> <span data-ttu-id="d38cb-116">Se trata de una lista delimitada por signos de punto y coma y puede contener direcciones IPv4 e IPv6.</span><span class="sxs-lookup"><span data-stu-id="d38cb-116">It's a semicolon-delimited list and can contain IPv4 and IPv6 addresses.</span></span>

[!code-json[](ip-safelist/samples/2.x/ClientIpAspNetCore/appsettings.json?highlight=2)]

## <a name="middleware"></a><span data-ttu-id="d38cb-117">Software intermedio</span><span class="sxs-lookup"><span data-stu-id="d38cb-117">Middleware</span></span>

<span data-ttu-id="d38cb-118">El `Configure` método agrega el middleware y le pasa la cadena de la misma en un parámetro de constructor.</span><span class="sxs-lookup"><span data-stu-id="d38cb-118">The `Configure` method adds the middleware and passes the safelist string to it in a constructor parameter.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_Configure&highlight=10)]

<span data-ttu-id="d38cb-119">El middleware analiza la cadena en una matriz y busca la dirección IP remota en la matriz.</span><span class="sxs-lookup"><span data-stu-id="d38cb-119">The middleware parses the string into an array and looks for the remote IP address in the array.</span></span> <span data-ttu-id="d38cb-120">Si no se encuentra la dirección IP remota, el middleware devuelve el HTTP 401 prohibido.</span><span class="sxs-lookup"><span data-stu-id="d38cb-120">If the remote IP address is not found, the middleware returns HTTP 401 Forbidden.</span></span> <span data-ttu-id="d38cb-121">Este proceso de validación se omite para las solicitudes HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="d38cb-121">This validation process is bypassed for HTTP Get requests.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/AdminSafeListMiddleware.cs?name=snippet_ClassOnly)]

## <a name="action-filter"></a><span data-ttu-id="d38cb-122">Filtro de acción</span><span class="sxs-lookup"><span data-stu-id="d38cb-122">Action filter</span></span>

<span data-ttu-id="d38cb-123">Si desea una sola de la información solo para controladores o métodos de acción específicos, use un filtro de acción.</span><span class="sxs-lookup"><span data-stu-id="d38cb-123">If you want a safelist only for specific controllers or action methods, use an action filter.</span></span> <span data-ttu-id="d38cb-124">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="d38cb-124">Here's an example:</span></span> 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckFilter.cs)]

<span data-ttu-id="d38cb-125">El filtro de acción se agrega al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="d38cb-125">The action filter is added to the services container.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

<span data-ttu-id="d38cb-126">A continuación, el filtro se puede usar en un método de acción o controlador.</span><span class="sxs-lookup"><span data-stu-id="d38cb-126">The filter can then be used on a controller or action method.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Controllers/ValuesController.cs?name=snippet_Filter&highlight=1)]

<span data-ttu-id="d38cb-127">En la aplicación de ejemplo, el filtro se aplica al `Get` método.</span><span class="sxs-lookup"><span data-stu-id="d38cb-127">In the sample app, the filter is applied to the `Get` method.</span></span> <span data-ttu-id="d38cb-128">Por lo tanto, al probar la aplicación mediante `Get` el envío de una solicitud de API, el atributo está validando la dirección IP del cliente.</span><span class="sxs-lookup"><span data-stu-id="d38cb-128">So when you test the app by sending a `Get` API request, the attribute is validating the client IP address.</span></span> <span data-ttu-id="d38cb-129">Al realizar la prueba mediante una llamada a la API con cualquier otro método HTTP, el middleware está validando la IP del cliente.</span><span class="sxs-lookup"><span data-stu-id="d38cb-129">When you test by calling the API with any other HTTP method, the middleware is validating the client IP.</span></span>

## <a name="razor-pages-filter"></a><span data-ttu-id="d38cb-130">Razor Pages filtro</span><span class="sxs-lookup"><span data-stu-id="d38cb-130">Razor Pages filter</span></span> 

<span data-ttu-id="d38cb-131">Si desea una aplicación de la Razor Pages, use un filtro de Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="d38cb-131">If you want a safelist for a Razor Pages app, use a Razor Pages filter.</span></span> <span data-ttu-id="d38cb-132">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="d38cb-132">Here's an example:</span></span> 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckPageFilter.cs)]

<span data-ttu-id="d38cb-133">Este filtro se habilita agregándolo a la colección de filtros de MVC.</span><span class="sxs-lookup"><span data-stu-id="d38cb-133">This filter is enabled by adding it to the MVC Filters collection.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

<span data-ttu-id="d38cb-134">Al ejecutar la aplicación y solicitar una página de Razor, el filtro de Razor Pages está validando la dirección IP del cliente.</span><span class="sxs-lookup"><span data-stu-id="d38cb-134">When you run the app and request a Razor page, the Razor Pages filter is validating the client IP.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d38cb-135">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="d38cb-135">Next steps</span></span>

<span data-ttu-id="d38cb-136">[Más información acerca de ASP.net Core middleware](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="d38cb-136">[Learn more about ASP.NET Core Middleware](xref:fundamentals/middleware/index).</span></span>
