---
title: "Exigir HTTPS en una aplicación ASP.NET básica"
author: rick-anderson
description: "Muestra cómo requerir HTTPS/TLS en un núcleo de ASP.NET de aplicación web."
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: dc320faf0048200412f131ea816f33f29ac023e1
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2018
---
# <a name="enforcing-https-in-an-aspnet-core-app"></a>Exigir HTTPS en una aplicación ASP.NET básica

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Este documento se muestra cómo:

- Requerir HTTPS para todas las solicitudes.
- Redirigir todas las solicitudes HTTP a HTTPS.

> [!WARNING]
> Hacer **no** usar `RequireHttpsAttribute` en las API Web que reciben información confidencial. `RequireHttpsAttribute` usa códigos de estado HTTP para redirigir exploradores de HTTP a HTTPS. Los clientes de API no pueden entender o siguen las redirecciones de HTTP a HTTPS. Estos clientes pueden enviar información a través de HTTP. Las API Web deben realizar las tareas:
>
>* No escuchar en HTTP.
>* Cierre la conexión con el código de estado 400 (solicitud incorrecta) y no atender la solicitud.

## <a name="require-https"></a>Requerir HTTPS

El [RequireHttpsAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.RequireHttpsAttribute) se usa para requerir HTTPS. `[RequireHttpsAttribute]` puede decorar controladores o métodos, o se pueden aplicar globalmente. Para aplicar el atributo global, agregue el código siguiente a `ConfigureServices` en `Startup`:

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

El código resaltado anterior requiere que todas las solicitudes usar `HTTPS`; por lo tanto, se omiten las solicitudes HTTP. El código resaltado siguiente redirige todas las solicitudes HTTP a HTTPS:

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

Para obtener más información, consulte [Middleware de reescritura de dirección URL](xref:fundamentals/url-rewriting).

Requerir HTTPS globalmente (`options.Filters.Add(new RequireHttpsAttribute());`) es una práctica recomendada de seguridad. Aplicar el `[RequireHttps]` atributo a todas las páginas de Razor/controladores no considera tan seguro como requerir HTTPS globalmente. No puede garantizar la `[RequireHttps]` atributo se aplica cuando se agregan nuevos controladores y las páginas de Razor.