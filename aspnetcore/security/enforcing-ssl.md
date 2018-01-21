---
title: "Exigir SSL en una aplicación ASP.NET básica"
author: rick-anderson
description: "Aplicación web se muestra cómo requerir SSL en un núcleo de ASP.NET"
ms.author: riande
manager: wpickett
ms.date: 07/19/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/enforcing-ssl
ms.openlocfilehash: 42d8767fda2d3f3545876f8ca18e0f6fbe6741b8
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
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
