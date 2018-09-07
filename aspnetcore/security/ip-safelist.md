---
title: Lista segura IP de cliente para ASP.NET Core
author: damienbod
description: Obtenga información sobre cómo escribir filtros de acción o Middleware para validar las direcciones IP remotas con una lista de direcciones IP aprobadas.
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: security/ip-safelist
ms.openlocfilehash: 40fe7b67359efd1692490099c3fb529ba4a6148f
ms.sourcegitcommit: 08bf41d4b3e696ab512b044970e8304816f8cc56
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/06/2018
ms.locfileid: "44040118"
---
# <a name="client-ip-safelist-for-aspnet-core"></a><span data-ttu-id="5ceb1-103">Lista segura IP de cliente para ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5ceb1-103">Client IP safelist for ASP.NET Core</span></span>

<span data-ttu-id="5ceb1-104">Por [Damien Bowden](https://twitter.com/damien_bod) y [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="5ceb1-104">By [Damien Bowden](https://twitter.com/damien_bod) and [Tom Dykstra](https://github.com/tdykstra)</span></span>
 
<span data-ttu-id="5ceb1-105">En este artículo se muestra dos maneras de implementar un safelist IP (también conocido como una lista blanca):</span><span class="sxs-lookup"><span data-stu-id="5ceb1-105">This article shows two ways to implement an IP safelist (also known as a whitelist):</span></span>

* <span data-ttu-id="5ceb1-106">Mediante el uso de middleware de ASP.NET Core para comprobar la dirección IP remota de todas las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="5ceb1-106">By using ASP.NET Core middleware to check the remote IP address of every request.</span></span>
* <span data-ttu-id="5ceb1-107">Mediante el uso de filtros de acción de ASP.NET Core para comprobar la dirección IP remota de las solicitudes de métodos de acción específica.</span><span class="sxs-lookup"><span data-stu-id="5ceb1-107">By using ASP.NET Core action filters to check the remote IP address of requests for specific action methods.</span></span>

<span data-ttu-id="5ceb1-108">La aplicación de ejemplo muestra ambos enfoques.</span><span class="sxs-lookup"><span data-stu-id="5ceb1-108">The sample app illustrates both approaches.</span></span> <span data-ttu-id="5ceb1-109">En cada caso, una cadena que contiene las direcciones IP de cliente aprobadas se almacena en una configuración de aplicación.</span><span class="sxs-lookup"><span data-stu-id="5ceb1-109">In each case, a string containing approved client IP addresses is stored in an app setting.</span></span> <span data-ttu-id="5ceb1-110">El software intermedio o el filtro analiza la cadena en una lista y comprueba si la dirección IP remota está en la lista.</span><span class="sxs-lookup"><span data-stu-id="5ceb1-110">The middleware or filter parses the string into a list and  checks if the remote IP is in the list.</span></span> <span data-ttu-id="5ceb1-111">Si no es así, se devuelve un código de estado HTTP 403 Prohibido.</span><span class="sxs-lookup"><span data-stu-id="5ceb1-111">If not, an HTTP 403 Forbidden status code is returned.</span></span>

<span data-ttu-id="5ceb1-112">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5ceb1-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-safelist"></a><span data-ttu-id="5ceb1-113">La lista segura</span><span class="sxs-lookup"><span data-stu-id="5ceb1-113">The safelist</span></span>

<span data-ttu-id="5ceb1-114">La lista está configurada en el *appsettings.json* archivo.</span><span class="sxs-lookup"><span data-stu-id="5ceb1-114">The list is configured in the *appsettings.json* file.</span></span> <span data-ttu-id="5ceb1-115">Es una lista delimitada por punto y coma y puede contener direcciones IPv4 e IPv6.</span><span class="sxs-lookup"><span data-stu-id="5ceb1-115">It's a semicolon-delimited list and can contain IPv4 and IPv6 addresses.</span></span>

[!code-json[](ip-safelist/samples/2.x/ClientIpAspNetCore/appsettings.json?highlight=2)]

## <a name="middleware"></a><span data-ttu-id="5ceb1-116">Software intermedio</span><span class="sxs-lookup"><span data-stu-id="5ceb1-116">Middleware</span></span>

<span data-ttu-id="5ceb1-117">El `Configure` método agrega el software intermedio y pasa la cadena de safelist a él en un parámetro de constructor.</span><span class="sxs-lookup"><span data-stu-id="5ceb1-117">The `Configure` method adds the middleware and passes the safelist string to it in a constructor parameter.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_Configure&highlight=7)]

<span data-ttu-id="5ceb1-118">El middleware analiza la cadena en una matriz y busca la dirección IP remota de la matriz.</span><span class="sxs-lookup"><span data-stu-id="5ceb1-118">The middleware parses the string into an array and looks for the remote IP address in the array.</span></span> <span data-ttu-id="5ceb1-119">Si no se encuentra la dirección IP remota, el middleware devuelve HTTP 401 prohibido.</span><span class="sxs-lookup"><span data-stu-id="5ceb1-119">If the remote IP address is not found, the middleware returns HTTP 401 Forbidden.</span></span> <span data-ttu-id="5ceb1-120">Este proceso de validación se omite para las solicitudes HTTP Get.</span><span class="sxs-lookup"><span data-stu-id="5ceb1-120">This validation process is bypassed for HTTP Get requests.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/AdminSafeListMiddleware.cs?name=snippet_ClassOnly)]

## <a name="action-filter"></a><span data-ttu-id="5ceb1-121">Filtro de acción</span><span class="sxs-lookup"><span data-stu-id="5ceb1-121">Action filter</span></span>

<span data-ttu-id="5ceb1-122">Si desea una lista segura solo para los métodos de acción o controladores concretos, use un filtro de acción.</span><span class="sxs-lookup"><span data-stu-id="5ceb1-122">If you want a safelist only for specific controllers or action methods, use an action filter.</span></span> <span data-ttu-id="5ceb1-123">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="5ceb1-123">Here's an example:</span></span> 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckFilter.cs)]

<span data-ttu-id="5ceb1-124">El filtro de acción se agrega al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="5ceb1-124">The action filter is added to the services container.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

<span data-ttu-id="5ceb1-125">El filtro, a continuación, puede usarse en un método de acción o controlador.</span><span class="sxs-lookup"><span data-stu-id="5ceb1-125">The filter can then be used on a controller or action method.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Controllers/ValuesController.cs?name=snippet_Filter&highlight=1)]

<span data-ttu-id="5ceb1-126">En la aplicación de ejemplo, el filtro se aplica a la `Get` método.</span><span class="sxs-lookup"><span data-stu-id="5ceb1-126">In the sample app, the filter is applied to the `Get` method.</span></span> <span data-ttu-id="5ceb1-127">Por lo que al probar la aplicación mediante el envío de un `Get` solicitud de API, el atributo está validando la dirección IP del cliente.</span><span class="sxs-lookup"><span data-stu-id="5ceb1-127">So when you test the app by sending a `Get` API request, the attribute is validating the client IP address.</span></span> <span data-ttu-id="5ceb1-128">Cuando se prueba mediante una llamada a la API con cualquier otro método HTTP, el middleware está validando la IP del cliente.</span><span class="sxs-lookup"><span data-stu-id="5ceb1-128">When you test by calling the API with any other HTTP method, the middleware is validating the client IP.</span></span>

## <a name="razor-pages-filter"></a><span data-ttu-id="5ceb1-129">Filtrar las páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="5ceb1-129">Razor Pages filter</span></span> 

<span data-ttu-id="5ceb1-130">Si desea una lista segura para una aplicación de páginas de Razor, use un filtro de las páginas de Razor.</span><span class="sxs-lookup"><span data-stu-id="5ceb1-130">If you want a safelist for a Razor Pages app, use a Razor Pages filter.</span></span> <span data-ttu-id="5ceb1-131">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="5ceb1-131">Here's an example:</span></span> 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckPageFilter.cs)]

<span data-ttu-id="5ceb1-132">Este filtro está habilitado, éste se agrega a la colección de filtros de MVC.</span><span class="sxs-lookup"><span data-stu-id="5ceb1-132">This filter is enabled by adding it to the MVC Filters collection.</span></span>

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

<span data-ttu-id="5ceb1-133">Cuando ejecute la aplicación y solicite una página de Razor, el filtro de las páginas de Razor está validando la IP del cliente.</span><span class="sxs-lookup"><span data-stu-id="5ceb1-133">When you run the app and request a Razor page, the Razor Pages filter is validating the client IP.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5ceb1-134">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="5ceb1-134">Next steps</span></span>

<span data-ttu-id="5ceb1-135">[Más información sobre el Middleware de ASP.NET Core](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="5ceb1-135">[Learn more about ASP.NET Core Middleware](xref:fundamentals/middleware/index).</span></span>
