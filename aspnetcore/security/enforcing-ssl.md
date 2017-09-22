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
ms.openlocfilehash: e8e7d4a69fd681534fb313ff113805bfd6a6d44e
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2017
---
# <a name="enforcing-ssl-in-an-aspnet-core-app"></a>Exigir SSL en una aplicación ASP.NET básica

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Este documento se muestra cómo:

- Requerir SSL para todas las solicitudes (sólo solicitudes HTTPS).
- Redirigir todas las solicitudes HTTP a HTTPS.

## <a name="require-ssl"></a>Requerir SSL

El [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) se usa para requerir SSL. Puede decorar controladores o métodos con este atributo o se puede aplicar a todo el mundo tal y como se muestra a continuación:

Agregue el código siguiente a `ConfigureServices` en `Startup`:

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-)]

El código resaltado anterior requiere que todas las solicitudes usar `HTTPS`, por lo tanto, se omiten las solicitudes HTTP. El código resaltado siguiente redirige todas las solicitudes HTTP a HTTPS:

[!code-csharp[Main](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-)]

Vea [Middleware de reescritura de dirección URL](xref:fundamentals/url-rewriting) para obtener más información.

Requerir HTTPS globalmente (`options.Filters.Add(new RequireHttpsAttribute());`) es una práctica recomendada de seguridad. Aplicar el `[RequireHttps]` atributo para todos los controladores no se considera tan seguro como requerir HTTPS globalmente. No puede garantizar nuevos controladores que se agregan a la aplicación le recordará que se aplicará la `[RequireHttps]` atributo.

## <a name="set-up-iis-express-for-sslhttps"></a>Configurar IIS Express para HTTPS/SSL

Vea [configurar HTTPS para el desarrollo de ASP.NET Core](xref:security/https#iisxpress).
