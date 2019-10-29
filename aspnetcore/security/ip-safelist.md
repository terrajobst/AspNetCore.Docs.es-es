---
title: La dirección IP de cliente de la ASP.NET Core
author: damienbod
description: Aprenda a escribir filtros de middleware o de acción para validar direcciones IP remotas en una lista de direcciones IP aprobadas.
ms.author: riande
ms.custom: mvc
ms.date: 08/31/2018
uid: security/ip-safelist
ms.openlocfilehash: ca5b0f8088773027f7403120247cbeca8900bcf5
ms.sourcegitcommit: 16cf016035f0c9acf3ff0ad874c56f82e013d415
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2019
ms.locfileid: "73034344"
---
# <a name="client-ip-safelist-for-aspnet-core"></a>La dirección IP de cliente de la ASP.NET Core

Por [Damien Bowden](https://twitter.com/damien_bod) y [Tom Dykstra](https://github.com/tdykstra)
 
En este artículo se muestran tres maneras de implementar una lista de permitidos de IP (también conocida como lista blanca) en una aplicación ASP.NET Core. Puede usar:

* Middleware para comprobar la dirección IP remota de cada solicitud.
* Filtros de acción para comprobar la dirección IP remota de las solicitudes de controladores o métodos de acción específicos.
* Razor Pages filtros para comprobar la dirección IP remota de las solicitudes de páginas de Razor.

En cada caso, una cadena que contiene direcciones IP de cliente aprobadas se almacena en una configuración de aplicación. El middleware o filtro analiza la cadena en una lista y comprueba si la dirección IP remota está en la lista. Si no es así, se devuelve un código de Estado HTTP 403 prohibido.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/ip-safelist/samples/2.x/ClientIpAspNetCore) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="the-safelist"></a>La de la

La lista se configura en el archivo *appSettings. JSON* . Se trata de una lista delimitada por signos de punto y coma y puede contener direcciones IPv4 e IPv6.

[!code-json[](ip-safelist/samples/2.x/ClientIpAspNetCore/appsettings.json?highlight=2)]

## <a name="middleware"></a>Software intermedio

El método `Configure` agrega el middleware y le pasa la cadena de la misma en un parámetro de constructor.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_Configure&highlight=10)]

El middleware analiza la cadena en una matriz y busca la dirección IP remota en la matriz. Si no se encuentra la dirección IP remota, el middleware devuelve el HTTP 401 prohibido. Este proceso de validación se omite para las solicitudes HTTP GET.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/AdminSafeListMiddleware.cs?name=snippet_ClassOnly)]

## <a name="action-filter"></a>Filtro de acción

Si desea una sola de la información solo para controladores o métodos de acción específicos, use un filtro de acción. Por ejemplo: 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIpCheckFilter.cs)]

El filtro de acción se agrega al contenedor de servicios.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

A continuación, el filtro se puede usar en un método de acción o controlador.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Controllers/ValuesController.cs?name=snippet_Filter&highlight=1)]

En la aplicación de ejemplo, el filtro se aplica al método `Get`. Por lo tanto, al probar la aplicación mediante el envío de una solicitud de API de `Get`, el atributo está validando la dirección IP del cliente. Al realizar la prueba mediante una llamada a la API con cualquier otro método HTTP, el middleware está validando la IP del cliente.

## <a name="razor-pages-filter"></a>Razor Pages filtro 

Si desea una aplicación de la Razor Pages, use un filtro de Razor Pages. Por ejemplo: 

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Filters/ClientIpCheckPageFilter.cs)]

Este filtro se habilita agregándolo a la colección de filtros de MVC.

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServices&highlight=7-9)]

Al ejecutar la aplicación y solicitar una página de Razor, el filtro de Razor Pages está validando la dirección IP del cliente.

## <a name="next-steps"></a>Pasos siguientes

[Más información acerca de ASP.net Core middleware](xref:fundamentals/middleware/index).
