---
title: Lista segura de IP de cliente para ASP.NET Core
author: damienbod
description: Aprenda a escribir middleware o filtros de acción para validar direcciones IP remotas con una lista de direcciones IP aprobadas.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/12/2020
uid: security/ip-safelist
ms.openlocfilehash: 2db879a6918245cbacff8b1a5dc15786ffab6a34
ms.sourcegitcommit: 196e4a36df5be5b04fedcff484a4261f8046ec57
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/31/2020
ms.locfileid: "80471797"
---
# <a name="client-ip-safelist-for-aspnet-core"></a>Lista segura de IP de cliente para ASP.NET Core

Por [Damien Bowden](https://twitter.com/damien_bod) y [Tom Dykstra](https://github.com/tdykstra)
 
En este artículo se muestran tres formas de implementar una lista segura de direcciones IP (también conocida como lista de permitidos) en una aplicación ASP.NET Core. Una aplicación de ejemplo adjunta muestra los tres enfoques. Puede usar:

* Middleware para comprobar la dirección IP remota de cada solicitud.
* Filtros de acción MVC para comprobar la dirección IP remota de las solicitudes de controladores específicos o métodos de acción.
* Razor Pages filtra para comprobar la dirección IP remota de las solicitudes de las páginas de Razor.

En cada caso, una cadena que contiene direcciones IP de cliente aprobadas se almacena en una configuración de aplicación. El middleware o filtro:

* Analiza la cadena en una matriz. 
* Comprueba si la dirección IP remota existe en la matriz.

Se permite el acceso si la matriz contiene la dirección IP. De lo contrario, se devuelve un código de estado HTTP 403 Prohibido.

[Ver o descargar código de ejemplo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/ip-safelist/samples) ( cómo[descargar](xref:index#how-to-download-a-sample))

## <a name="ip-address-safelist"></a>Lista segura de direcciones IP

En la aplicación de ejemplo, la lista segura de direcciones IP es:

* Definido por `AdminSafeList` la propiedad en el archivo *appsettings.json.*
* Cadena delimitada por punto y coma que puede contener direcciones de protocolo de [Internet versión 4 (IPv4)](https://wikipedia.org/wiki/IPv4) y protocolo de [Internet versión 6 (IPv6).](https://wikipedia.org/wiki/IPv6)

[!code-json[](ip-safelist/samples/3.x/ClientIpAspNetCore/appsettings.json?range=1-3&highlight=2)]

En el ejemplo anterior, se permiten `127.0.0.1` `192.168.1.5` las direcciones IPv4 `::1` de y `0:0:0:0:0:0:0:1`la dirección de bucle de iPv6 de (formato comprimido para ).

## <a name="middleware"></a>Software intermedio

El `Startup.Configure` método agrega `AdminSafeListMiddleware` el tipo de middleware personalizado a la canalización de solicitudes de la aplicación. La lista segura se recupera con el proveedor de configuración de .NET Core y se pasa como un parámetro de constructor.

[!code-csharp[](ip-safelist/samples/3.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureAddMiddleware)]

El middleware analiza la cadena en una matriz y busca la dirección IP remota en la matriz. Si no se encuentra la dirección IP remota, el middleware devuelve HTTP 403 Prohibido. Este proceso de validación se omite para las solicitudes HTTP GET.

[!code-csharp[](ip-safelist/samples/Shared/ClientIpSafelistComponents/Middlewares/AdminSafeListMiddleware.cs?name=snippet_ClassOnly)]

## <a name="action-filter"></a>Filtro de acción

Si desea un control de acceso controlado por lista segura para controladores MVC específicos o métodos de acción, use un filtro de acción. Por ejemplo:

[!code-csharp[](ip-safelist/samples/Shared/ClientIpSafelistComponents/Filters/ClientIpCheckActionFilter.cs?name=snippet_ClassOnly)]

En `Startup.ConfigureServices`, agregue el filtro de acción a la colección de filtros MVC. En el ejemplo `ClientIpCheckActionFilter` siguiente, se agrega un filtro de acción. Una lista segura y una instancia de registrador de consola se pasan como parámetros de constructor.

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](ip-safelist/samples/3.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServicesActionFilter)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServicesActionFilter)]

::: moniker-end

A continuación, el filtro de acción se puede aplicar a un controlador o método de acción con el atributo [[ServiceFilter]:](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute)

[!code-csharp[](ip-safelist/samples/3.x/ClientIpAspNetCore/Controllers/ValuesController.cs?name=snippet_ActionFilter&highlight=1)]

En la aplicación de ejemplo, el filtro `Get` de acción se aplica al método de acción del controlador. Al probar la aplicación mediante el envío:

* Una solicitud HTTP `[ServiceFilter]` GET, el atributo valida la dirección IP del cliente. Si se permite `Get` el acceso al método de acción, el filtro de acción y el método de acción producen una variación de la siguiente salida de la consola:

    ```
    dbug: ClientIpSafelistComponents.Filters.ClientIpCheckActionFilter[0]
          Remote IpAddress: ::1
    dbug: ClientIpAspNetCore.Controllers.ValuesController[0]
          successful HTTP GET    
    ```

* Un verbo de solicitud HTTP `AdminSafeListMiddleware` distinto de GET, el middleware valida la dirección IP del cliente.

## <a name="razor-pages-filter"></a>Filtro Razor Pages

Si quieres un control de acceso controlado por listas seguras para una aplicación Razor Pages, usa un filtro Razor Pages. Por ejemplo:

[!code-csharp[](ip-safelist/samples/Shared/ClientIpSafelistComponents/Filters/ClientIpCheckPageFilter.cs?name=snippet_ClassOnly)]

En `Startup.ConfigureServices`, habilite el filtro Razor Pages agregándolo a la colección de filtros MVC. En el ejemplo `ClientIpCheckPageFilter` siguiente, se agrega un filtro Razor Pages. Una lista segura y una instancia de registrador de consola se pasan como parámetros de constructor.

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](ip-safelist/samples/3.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServicesPageFilter)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](ip-safelist/samples/2.x/ClientIpAspNetCore/Startup.cs?name=snippet_ConfigureServicesPageFilter)]

::: moniker-end

Cuando se solicita la página *Index* Razor de la aplicación de ejemplo, el filtro Razor Pages valida la dirección IP del cliente. El filtro produce una variación de la siguiente salida de consola:

```
dbug: ClientIpSafelistComponents.Filters.ClientIpCheckPageFilter[0]
      Remote IpAddress: ::1
```

## <a name="additional-resources"></a>Recursos adicionales

* <xref:fundamentals/middleware/index>
* [Filtros de acciones](xref:mvc/controllers/filters#action-filters)
* <xref:razor-pages/filter>
