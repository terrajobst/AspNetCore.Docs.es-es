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
# <a name="client-ip-safelist-for-aspnet-core"></a>Lista segura IP de cliente para ASP.NET Core

Por [Damien Bowden](https://twitter.com/damien_bod) y [Tom Dykstra](https://github.com/tdykstra)
 
En este artículo se muestra tres maneras de implementar un safelist IP (también conocido como una lista blanca) en una aplicación ASP.NET Core. Puede usar:

* Software intermedio para comprobar la dirección IP remota de todas las solicitudes.
* Filtros de acción para comprobar la dirección IP remota de las solicitudes de los métodos de acción o controladores concretos.
* Filtros de páginas de Razor para comprobar la dirección IP remota de solicitudes de páginas de Razor.

La aplicación de ejemplo muestra ambos enfoques. En cada caso, una cadena que contiene las direcciones IP de cliente aprobadas se almacena en una configuración de aplicación. El software intermedio o el filtro analiza la cadena en una lista y comprueba si la dirección IP remota está en la lista. Si no es así, se devuelve un código de estado HTTP 403 Prohibido.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="the-safelist"></a>La lista segura

La lista está configurada en el *appsettings.json* archivo. Es una lista delimitada por punto y coma y puede contener direcciones IPv4 e IPv6.

[!code-json[](ip-safelist/samples/2.x/ClientIpAspNetCore/appsettings.json?highlight=2)]

## <a name="middleware"></a>Software intermedio

El `Configure` método agrega el software intermedio y pasa la cadena de safelist a él en un parámetro de constructor.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_Configure&highlight=7)]

El middleware analiza la cadena en una matriz y busca la dirección IP remota de la matriz. Si no se encuentra la dirección IP remota, el middleware devuelve HTTP 401 prohibido. Este proceso de validación se omite para las solicitudes HTTP Get.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/AdminSafeListMiddleware.cs?name=snippet_ClassOnly)]

## <a name="action-filter"></a>Filtro de acción

Si desea una lista segura solo para los métodos de acción o controladores concretos, use un filtro de acción. Por ejemplo: 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckFilter.cs)]

El filtro de acción se agrega al contenedor de servicios.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

El filtro, a continuación, puede usarse en un método de acción o controlador.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Controllers/ValuesController.cs?name=snippet_Filter&highlight=1)]

En la aplicación de ejemplo, el filtro se aplica a la `Get` método. Por lo que al probar la aplicación mediante el envío de un `Get` solicitud de API, el atributo está validando la dirección IP del cliente. Cuando se prueba mediante una llamada a la API con cualquier otro método HTTP, el middleware está validando la IP del cliente.

## <a name="razor-pages-filter"></a>Filtrar las páginas de Razor 

Si desea una lista segura para una aplicación de páginas de Razor, use un filtro de las páginas de Razor. Por ejemplo: 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIdCheckPageFilter.cs)]

Este filtro está habilitado, éste se agrega a la colección de filtros de MVC.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

Cuando ejecute la aplicación y solicite una página de Razor, el filtro de las páginas de Razor está validando la IP del cliente.

## <a name="next-steps"></a>Pasos siguientes

[Más información sobre el Middleware de ASP.NET Core](xref:fundamentals/middleware/index).
