---
title: Exigir HTTPS en el núcleo de ASP.NET
author: rick-anderson
description: Muestra cómo requerir HTTPS/TLS en un núcleo de ASP.NET de aplicación web.
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: 69ce182855878e4d05bff95139fefb9e1312f3d5
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252079"
---
# <a name="enforce-https-in-aspnet-core"></a>Exigir HTTPS en el núcleo de ASP.NET

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Este documento se muestra cómo:

* Requerir HTTPS para todas las solicitudes.
* Redirigir todas las solicitudes HTTP a HTTPS.

> [!WARNING]
> Hacer **no** usar [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) en las API Web que reciben información confidencial. `RequireHttpsAttribute` usa códigos de estado HTTP para redirigir exploradores de HTTP a HTTPS. Los clientes de API no pueden entender o siguen las redirecciones de HTTP a HTTPS. Estos clientes pueden enviar información a través de HTTP. Las API Web deben realizar las tareas:
>
> * No escuchar en HTTP.
> * Cierre la conexión con el código de estado 400 (solicitud incorrecta) y no atender la solicitud.

<a name="require"></a>
## <a name="require-https"></a>Requerir HTTPS

::: moniker range=">= aspnetcore-2.1"

Se recomienda que todas las aplicaciones web de ASP.NET Core llamará Middleware de redirección de HTTPS ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) para redirigir todas las solicitudes HTTP a HTTPS.

El código siguiente llama `UseHttpsRedirection` en la `Startup` clase:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

El código siguiente llama [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) para configurar las opciones de middleware:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

El código resaltado anterior:

* Conjuntos de [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode).
* Establece el puerto HTTPS en 5001.

Los siguientes mecanismos establecen automáticamente el puerto:

* El software intermedio puede detectar los puertos a través de [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) cuando se aplican las condiciones siguientes:
  - Se utiliza kestrel o HTTP.sys directamente con los puntos de conexión HTTPS (también se aplica a la aplicación se ejecuta con el depurador de código de Visual Studio).
  - Solo **un puerto HTTPS** se utiliza la aplicación.
* Se utiliza Visual Studio:
  - IIS Express tiene habilitados para HTTPS.
  - *launchSettings.json* establece el `sslPort` de IIS Express.

> [!NOTE]
> Cuando una aplicación se ejecute detrás de un proxy inverso (por ejemplo, IIS, IIS Express), `IServerAddressesFeature` no está disponible. El puerto debe configurarse manualmente. Cuando el puerto no está configurado, no se redirigen las solicitudes.

El puerto se puede configurar estableciendo el:

* La variable de entorno `ASPNETCORE_HTTPS_PORT`.
* `http_port` clave de configuración de host (por ejemplo, a través de *hostsettings.json* o un argumento de línea de comandos).
* [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport). Vea el ejemplo anterior que se muestra cómo establecer el puerto a 5001.

> [!NOTE]
> El puerto puede configurarse indirectamente estableciendo la dirección URL con el `ASPNETCORE_URLS` variable de entorno. La variable de entorno configura el servidor y, a continuación, el middleware indirectamente detecta el puerto HTTPS a través de `IServerAddressesFeature`.

Si no se establece ningún puerto:

* No se redirigen las solicitudes.
* El middleware registra una advertencia.

::: moniker-end

::: moniker range="< aspnetcore-2.1"

El [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) se usa para requerir HTTPS. `[RequireHttpsAttribute]` puede decorar controladores o métodos, o se pueden aplicar globalmente. Para aplicar el atributo global, agregue el código siguiente a `ConfigureServices` en `Startup`:

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

El código resaltado anterior requiere que todas las solicitudes usar `HTTPS`; por lo tanto, se omiten las solicitudes HTTP. El código resaltado siguiente redirige todas las solicitudes HTTP a HTTPS:

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

Para obtener más información, consulte [Middleware de reescritura de dirección URL](xref:fundamentals/url-rewriting).

Requerir HTTPS globalmente (`options.Filters.Add(new RequireHttpsAttribute());`) es una práctica recomendada de seguridad. Aplicar el `[RequireHttps]` atributo a todas las páginas de Razor/controladores no considera tan seguro como requerir HTTPS globalmente. No puede garantizar la `[RequireHttps]` atributo se aplica cuando se agregan nuevos controladores y las páginas de Razor.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a>Protocolo de seguridad de transporte estrictos de HTTP (HSTS)

Por [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [seguridad de transporte estricta de HTTP (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) supone una mejora de seguridad opcional que se especifica mediante una aplicación web mediante el uso de un encabezado de respuesta especial. Una vez que un explorador compatible recibe este encabezado ese explorador impide que todas las comunicaciones se envíen a través de HTTP para el dominio especificado y en su lugar, le enviará todas las comunicaciones a través de HTTPS. También evita que haga clic en HTTPS a través de solicitudes en exploradores.

ASP.NET Core 2.1 o posterior implementa HSTS con el `UseHsts` método de extensión. El código siguiente llama `UseHsts` cuando la aplicación no se encuentra en [modo de desarrollo](xref:fundamentals/environments):

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

`UseHsts` no es recomendable en el desarrollo porque el encabezado HSTS es alta puede almacenar exploradores. De forma predeterminada, UseHsts excluye la dirección de bucle invertido local.

El código siguiente:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* Establece el parámetro precarga del encabezado de seguridad de transporte Strict. Precarga no es parte de la [especificación RFC HSTS](https://tools.ietf.org/html/rfc6797), pero es compatible con los exploradores web para cargar previamente sitios HSTS en instalación nueva. Para más información, vea [https://hstspreload.org/](https://hstspreload.org/).
* Permite [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), que aplica la directiva HSTS a subdominios de Host. 
* Establece explícitamente el parámetro de max-age del encabezado de seguridad de transporte Strict a 60 días. Si no se establece, el valor predeterminado es 30 días. Consulte la [max-age directiva](https://tools.ietf.org/html/rfc6797#section-6.1.1) para obtener más información.
* Agrega `example.com` a la lista de hosts que se van a excluir.

`UseHsts` excluye los siguientes hosts de bucle invertido:

* `localhost` : La dirección de bucle invertido de IPv4.
* `127.0.0.1` : La dirección de bucle invertido de IPv4.
* `[::1]` : La dirección de bucle invertido de IPv6.

En el ejemplo anterior se muestra cómo agregar hosts adicionales.
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a>Desactivación de HTTPS en la creación del proyecto

Habilitar las plantillas de aplicación de ASP.NET Core web 2.1 o posterior (en Visual Studio o la línea de comandos dotnet) [redirección HTTPS](#require) y [HSTS](#hsts). Para las implementaciones que no requieran HTTPS, puede cancelar voluntariamente la suscripción de HTTPS. Por ejemplo, algunos servicios back-end donde HTTPS se está controlando externamente en el perímetro, mediante HTTPS en cada nodo no es necesario.

A la cancelación de HTTPS:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Desactive el **configurar para HTTPS** casilla de verificación.

![Diagrama de entidades](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli) 

Use la opción `--no-https`. Por ejemplo

```console
dotnet new webapp --no-https
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-setup-a-developer-certificate-for-docker"></a>Cómo configurar un certificado de desarrollador para Docker

Vea [este problema de GitHub](https://github.com/aspnet/Docs/issues/6199).

::: moniker-end
