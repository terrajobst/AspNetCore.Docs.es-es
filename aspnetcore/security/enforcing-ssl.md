---
title: Exigir HTTPS en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo requerir HTTPS/TLS en una aplicación web ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/11/2018
uid: security/enforcing-ssl
ms.openlocfilehash: b4c058d3b4f00276043d9520d756e62ed8cac5d9
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325606"
---
# <a name="enforce-https-in-aspnet-core"></a>Exigir HTTPS en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Este documento se muestra cómo:

* Requerir HTTPS para todas las solicitudes.
* Redirigir todas las solicitudes HTTP a HTTPS.

Ninguna API puede evitar que a un cliente envía información confidencial en la primera solicitud.

> [!WARNING]
> Hacer **no** usar [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) en las API Web que recibe información confidencial. `RequireHttpsAttribute` usa códigos de estado HTTP para redirigir los exploradores de HTTP a HTTPS. Los clientes de API no pueden entender o siguen las redirecciones de HTTP a HTTPS. Estos clientes pueden enviar información a través de HTTP. Las API Web deben:
>
> * No escucha en HTTP.
> * Cierre la conexión con el código de estado 400 (solicitud incorrecta) y no atender la solicitud.

<a name="require"></a>

## <a name="require-https"></a>Requerir HTTPS

::: moniker range=">= aspnetcore-2.1"

Se recomienda toda la producción de ASP.NET Core llamada de aplicaciones web:

* El Middleware de redireccionamiento de HTTPS ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) para redirigir todas las solicitudes HTTP a HTTPS.
* [UseHsts](#hsts), protocolo de seguridad de transporte estricto HTTP (HSTS).

### <a name="usehttpsredirection"></a>UseHttpsRedirection

El código siguiente llama `UseHttpsRedirection` en el `Startup` clase:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

El código resaltado anterior:

* Usa el valor predeterminado [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).
* Usa el valor predeterminado [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null), a menos que se reemplaza por la `ASPNETCORE_HTTPS_PORT` variable de entorno o [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).

Se recomienda usar redirecciones temporales en lugar de redirecciones permanentes, como el almacenamiento en caché de vínculo puede provocar un comportamiento inestable en entornos de desarrollo. Se recomienda usar [HSTS](#hsts) para indicar a los clientes que solo proteger el recurso se deben enviar solicitudes a la aplicación (solo en producción).

> [!WARNING]
> Un puerto debe estar disponible para el middleware redirigir a HTTPS. Si el puerto no está disponible, no se produce la redirección a HTTPS. Se puede especificar el puerto HTTPS mediante cualquiera de los métodos siguientes:
>
> * Establecer `HttpsRedirectionOptions.HttpsPort`.
> * Establecer la variable de entorno `ASPNETCORE_HTTPS_PORT`.
> * En el desarrollo, establezca una dirección URL HTTPS *launchsettings.json*.
> * Configurar un punto de conexión de dirección URL HTTPS para [Kestrel](xref:fundamentals/servers/kestrel) o [HTTP.sys](xref:fundamentals/servers/httpsys).
>
> Cuando se usa Kestrel o HTTP.sys como un servidor perimetral de acceso público, Kestrel o HTTP.sys debe configurarse para que escuche en ambos:
>
> * El puerto seguro donde se redirige el cliente (normalmente, 443 en 5001 en desarrollo y producción).
> * El puerto no seguro (normalmente, 80 en producción) y 5000 en el desarrollo.
>
> El puerto no seguro debe ser accesible por el cliente en el orden de la aplicación para recibir una solicitud poco segura y redirigirlo a un puerto seguro.
>
> Ningún firewall entre el cliente y el servidor también debe tener los puertos abierto para el tráfico.
>
> Para obtener más información, consulte [configuración de punto de conexión Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) o <xref:fundamentals/servers/httpsys>.

Los siguientes resaltan las llamadas de código [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) para configurar las opciones de middleware:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

Una llamada a `AddHttpsRedirection` sólo es necesario cambiar los valores de `HttpsPort` o `RedirectStatusCode`.

El código resaltado anterior:

* Conjuntos de [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) a [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect), que es el valor predeterminado. Utilice los campos de la [StatusCodes](/dotnet/api/microsoft.aspnetcore.http.statuscodes) clase para las asignaciones a `RedirectStatusCode`.
* Establece el puerto HTTPS a 5001. El valor predeterminado es 443.

Los mecanismos siguientes establecen automáticamente el puerto:

* El middleware puede detectar los puertos a través de [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) cuando se aplican las condiciones siguientes:

  * Kestrel o HTTP.sys se usa directamente con los puntos de conexión HTTPS (también se aplica a la ejecución de la aplicación con el depurador de Visual Studio Code).
  * Solo **un puerto HTTPS** utilizado por la aplicación.

* Se usa Visual Studio:

  * IIS Express tiene habilitadas para HTTPS.
  * *launchSettings.json* establece el `sslPort` para IIS Express.

> [!NOTE]
> Cuando se ejecuta una aplicación detrás de un proxy inverso (por ejemplo, IIS, IIS Express), `IServerAddressesFeature` no está disponible. El puerto debe configurarse manualmente. Cuando no se configura el puerto, no se redirigen las solicitudes.

El puerto puede configurarse estableciendo el [https_port de configuración de Host Web](xref:fundamentals/host/web-host#https-port):

**Clave**: https_port  
**Tipo**: *cadena*  
**Valor predeterminado**: no se establece un valor predeterminado.  
**Establecer mediante**: `UseSetting`  
**Variable de entorno**: `<PREFIX_>HTTPS_PORT` (el prefijo es `ASPNETCORE_` cuando se usa el Host de Web.)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

> [!NOTE]
> Se puede configurar el puerto indirectamente estableciendo la dirección URL con el `ASPNETCORE_URLS` variable de entorno. La variable de entorno configura el servidor y, a continuación, el middleware detecta indirectamente el puerto HTTPS a través de `IServerAddressesFeature`.

Si no se establece ningún puerto:

* No se redirigen las solicitudes.
* El middleware registra la advertencia "No se pudo determinar el puerto https para la redirección."

> [!NOTE]
> Una alternativa al uso de Middleware de redireccionamiento de HTTPS (`UseHttpsRedirection`) consiste en usar el Middleware de reescritura de dirección URL (`AddRedirectToHttps`). `AddRedirectToHttps` También se puede establecer el código de estado y el puerto cuando se ejecuta la redirección. Para obtener más información, consulte [Middleware de reescritura de URL](xref:fundamentals/url-rewriting).
>
> Al redirigir a HTTPS sin necesidad de reglas de redirección adicionales, se recomienda usar software intermedio de redireccionamiento de HTTPS (`UseHttpsRedirection`) se describe en este tema.

::: moniker-end

::: moniker range="< aspnetcore-2.1"

El [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) se usa para requerir HTTPS. `[RequireHttpsAttribute]` puede decorar los controladores o métodos, o se pueden aplicar globalmente. Para aplicar el atributo global, agregue el código siguiente al `ConfigureServices` en `Startup`:

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

El código resaltado anterior requiere que todas las solicitudes usan `HTTPS`; por lo tanto, se omiten las solicitudes HTTP. El código resaltado siguiente redirige todas las solicitudes HTTP a HTTPS:

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

Para obtener más información, consulte [Middleware de reescritura de URL](xref:fundamentals/url-rewriting). El software intermedio también permite a la aplicación para establecer el código de estado o el código de estado y el puerto cuando se ejecuta la redirección.

Requerir HTTPS globalmente (`options.Filters.Add(new RequireHttpsAttribute());`) es una práctica recomendada de seguridad. Aplicar el `[RequireHttps]` atributo a todas las páginas de Razor/controladores no se considera tan seguro como requerir HTTPS globalmente. No puede garantizar la `[RequireHttps]` atributo se aplica cuando se agregan nuevos controladores y las páginas de Razor.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>

## <a name="http-strict-transport-security-protocol-hsts"></a>Protocolo de seguridad de transporte estricto HTTP (HSTS)

Por [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [seguridad de transporte estricto (HSTS) de HTTP](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) es una mejora de seguridad opcional que se especifica mediante una aplicación web mediante el uso de un encabezado de respuesta. Cuando un [explorador que admita HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) recibe este encabezado:

* El explorador almacena la configuración para el dominio que impide el envío de cualquier comunicación a través de HTTP. El explorador obliga a todas las comunicaciones a través de HTTPS.
* El explorador impide que el usuario mediante certificados de confianza o no es válidos. El explorador deshabilita avisos para que un usuario que confíe en temporalmente dicho certificado.

Porque se aplica HSTS por el cliente tiene algunas limitaciones:

* El cliente debe admitir HSTS.
* HSTS requiere al menos una solicitud HTTPS correcta para establecer la directiva HSTS.
* La aplicación debe comprobar todas las solicitudes HTTP y redirigir o rechazar la solicitud HTTP.

ASP.NET Core 2.1 o posterior implementa HSTS con el `UseHsts` método de extensión. El código siguiente llama `UseHsts` cuando la aplicación no se encuentra en [modo de desarrollo](xref:fundamentals/environments):

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

`UseHsts` no se recomienda en el desarrollo porque la configuración de HSTS sea altamente almacenable en caché por los exploradores. De forma predeterminada, `UseHsts` excluye la dirección de bucle invertido local.

Para entornos de producción implementar HTTPS por primera vez, establezca el valor HSTS inicial en un valor pequeño. Establezca el valor de horas en no más de un solo día en caso de que necesite revertir la infraestructura HTTPS a HTTP. Una vez que esté seguro en la sostenibilidad de la configuración de HTTPS, aumente el valor de max-age HSTS; un valor frecuente es un año. 

El código siguiente:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* Establece el parámetro precarga del encabezado Strict-Transport-Security. Precarga no forma parte de la [especificación RFC HSTS](https://tools.ietf.org/html/rfc6797), pero es compatible con los exploradores web para cargar previamente sitios HSTS en una instalación nueva. Para más información, vea [https://hstspreload.org/](https://hstspreload.org/).
* Permite [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), que aplica la directiva HSTS para hospedar subdominios.
* Establece explícitamente el parámetro de antigüedad máxima del encabezado Strict-Transport-Security en 60 días. Si no se establece, el valor predeterminado es 30 días. Consulte la [max-age directiva](https://tools.ietf.org/html/rfc6797#section-6.1.1) para obtener más información.
* Agrega `example.com` a la lista de hosts para excluir.

`UseHsts` excluye los hosts de bucle invertido siguientes:

* `localhost` : La dirección de bucle invertido de IPv4.
* `127.0.0.1` : La dirección de bucle invertido de IPv4.
* `[::1]` : La dirección de bucle invertido de IPv6.

El ejemplo anterior muestra cómo agregar hosts adicionales.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>

## <a name="opt-out-of-httpshsts-on-project-creation"></a>Desactivación de HTTPS/HSTS crea un proyecto

En algunos escenarios de servicio de back-end donde se controla la seguridad de conexión en el perímetro orientados al público de la red, la configuración de seguridad de conexión en cada nodo no es necesario. Web apps generados a partir de las plantillas en Visual Studio o desde la [dotnet nuevo](/dotnet/core/tools/dotnet-new) Habilitar comando [redirección HTTPS](#require) y [HSTS](#hsts). Para las implementaciones que no requieren estos escenarios, puede desactivar de HTTPS/HSTS cuando se crea la aplicación de la plantilla.

Para participar en HTTPS/HSTS:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Desactive el **configurar HTTPS** casilla de verificación.

![Diagrama de entidades](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli) 

Use la opción `--no-https`. Por ejemplo

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a>Cómo configurar un certificado de desarrollador para Docker

Consulte [este problema de GitHub](https://github.com/aspnet/Docs/issues/6199).

::: moniker-end

## <a name="additional-information"></a>Información adicional

* [Compatibilidad con exploradores de OWASP HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
