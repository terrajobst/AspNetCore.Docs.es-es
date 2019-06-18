---
title: Exigir HTTPS en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo requerir HTTPS/TLS en una aplicación web ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/01/2018
uid: security/enforcing-ssl
ms.openlocfilehash: 08ce50775d1b5348cb0528a1724cec2e5c72dae2
ms.sourcegitcommit: 4ef0362ef8b6e5426fc5af18f22734158fe587e1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/17/2019
ms.locfileid: "67152898"
---
# <a name="enforce-https-in-aspnet-core"></a>Exigir HTTPS en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Este documento se muestra cómo:

* Requerir HTTPS para todas las solicitudes.
* Redirigir todas las solicitudes HTTP a HTTPS.

Ninguna API puede evitar que a un cliente envía información confidencial en la primera solicitud.

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> ## <a name="api-projects"></a>Proyectos de API
>
> Hacer **no** usar [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) en las API Web que recibe información confidencial. `RequireHttpsAttribute` usa códigos de estado HTTP para redirigir los exploradores de HTTP a HTTPS. Los clientes de API no pueden entender o siguen las redirecciones de HTTP a HTTPS. Estos clientes pueden enviar información a través de HTTP. Las API Web deben:
>
> * No escucha en HTTP.
> * Cierre la conexión con el código de estado 400 (solicitud incorrecta) y no atender la solicitud.
::: moniker-end

::: moniker range=">= aspnetcore-3.0"

> [!WARNING]
> ## <a name="api-projects"></a>Proyectos de API
>
> Hacer **no** usar [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) en las API Web que recibe información confidencial. `RequireHttpsAttribute` usa códigos de estado HTTP para redirigir los exploradores de HTTP a HTTPS. Los clientes de API no pueden entender o siguen las redirecciones de HTTP a HTTPS. Estos clientes pueden enviar información a través de HTTP. Las API Web deben:
>
> * No escucha en HTTP.
> * Cierre la conexión con el código de estado 400 (solicitud incorrecta) y no atender la solicitud.
>
> ## <a name="hsts-and-api-projects"></a>Proyectos HSTS y API
>
> No incluyen los proyectos de API predeterminado [HSTS](#hsts) porque HSTS suele ser una instrucción única del explorador. Otros autores de llamadas, como teléfono o aplicaciones de escritorio, hacer **no** obedecen a la instrucción. Incluso dentro de los exploradores, una sola llamada autenticada a una API a través de HTTP tiene riesgos en redes no seguras. El modo seguro es configurar los proyectos de API para que sólo escuche y responda a través de HTTPS.

::: moniker-end

## <a name="require-https"></a>Requisito de HTTPS

::: moniker range=">= aspnetcore-2.1"

Se recomienda que ASP.NET Core de producción llamada de aplicaciones web:

* Middleware de redireccionamiento de HTTPS (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) para redirigir las solicitudes HTTP a HTTPS.
* Middleware HSTS ([UseHsts](#http-strict-transport-security-protocol-hsts)) para enviar los encabezados de protocolo de seguridad de estricta transporte (HSTS) de HTTP a los clientes.

> [!NOTE]
> Las aplicaciones implementadas en una configuración de proxy inverso que el proxy controlar la seguridad de conexión (HTTPS). Si el servidor proxy también controla la redirección de HTTPS, no hay ninguna necesidad de usar Middleware de redireccionamiento de HTTPS. Si el servidor proxy también controla escribir encabezados HSTS (por ejemplo, [HSTS nativos se admite en IIS 10.0 (1709) o versiones posteriores](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support)), Middleware HSTS no es necesario por la aplicación. Para obtener más información, consulte [voluntaria de HTTPS/HSTS crea un proyecto](#opt-out-of-httpshsts-on-project-creation).

### <a name="usehttpsredirection"></a>UseHttpsRedirection

El código siguiente llama `UseHttpsRedirection` en el `Startup` clase:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

El código resaltado anterior:

* Usa el valor predeterminado [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).
* Usa el valor predeterminado [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null), a menos que se reemplaza por la `ASPNETCORE_HTTPS_PORT` variable de entorno o [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).

Se recomienda usar redirecciones temporales en lugar de redirecciones permanentes. Almacenamiento en caché de vínculo, puede provocar un comportamiento inestable en entornos de desarrollo. Si prefiere enviar un código de estado de redirección permanente cuando la aplicación está en un entorno de desarrollo no, consulte la [configurar redirecciones permanentes en producción](#configure-permanent-redirects-in-production) sección. Se recomienda usar [HSTS](#http-strict-transport-security-protocol-hsts) para indicar a los clientes que solo proteger el recurso se deben enviar solicitudes a la aplicación (solo en producción).

### <a name="port-configuration"></a>Configuración del puerto

Un puerto debe estar disponible para el middleware redirigir una solicitud poco segura a HTTPS. Si el puerto no está disponible:

* No se produce la redirección a HTTPS.
* El middleware registra la advertencia "No se pudo determinar el puerto https para la redirección."

Especifique el puerto HTTPS mediante cualquiera de los métodos siguientes:

* Establecer [HttpsRedirectionOptions.HttpsPort](#options).
* Establecer el `ASPNETCORE_HTTPS_PORT` variable de entorno o [https_port de configuración de Host Web](xref:fundamentals/host/web-host#https-port):

  **Clave**: `https_port`  
  **Tipo**: *cadena*  
  **Predeterminado**: no se ha establecido ningún valor predeterminado.  
  **Establecer mediante**: `UseSetting`  
  **Variable de entorno**: `<PREFIX_>HTTPS_PORT` (El prefijo es `ASPNETCORE_` cuando se usa el [Web Host](xref:fundamentals/host/web-host).)

  Al configurar un <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> en `Program`:

  [!code-csharp[](enforcing-ssl/sample-snapshot/Program.cs?name=snippet_Program&highlight=10)]
* Indique un puerto con el esquema seguro mediante el `ASPNETCORE_URLS` variable de entorno. La variable de entorno configura el servidor. El middleware detecta indirectamente el puerto HTTPS a través de <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>. Este enfoque no funciona en las implementaciones de proxy inverso.
* En el desarrollo, establezca una dirección URL HTTPS *launchsettings.json*. Habilitar HTTPS cuando se usa IIS Express.
* Configurar un punto de conexión de dirección URL HTTPS para una implementación de edge orientados al público de [Kestrel](xref:fundamentals/servers/kestrel) server o [HTTP.sys](xref:fundamentals/servers/httpsys) server. Solo **un puerto HTTPS** utilizado por la aplicación. El middleware detecta el puerto a través de <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.

> [!NOTE]
> Cuando una aplicación se ejecuta en una configuración de proxy inverso, <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature> no está disponible. Establezca el puerto mediante uno de los demás enfoques descritos en esta sección.

Cuando se usa Kestrel o HTTP.sys como un servidor perimetral de acceso público, Kestrel o HTTP.sys debe configurarse para que escuche en ambos:

* El puerto seguro donde se redirige el cliente (normalmente, 443 en 5001 en desarrollo y producción).
* El puerto no seguro (normalmente, 80 en producción) y 5000 en el desarrollo.

El puerto no seguro debe ser accesible por el cliente en el orden de la aplicación para recibir una solicitud poco segura y redirigir al cliente a un puerto seguro.

Para obtener más información, consulte [configuración de punto de conexión Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) o <xref:fundamentals/servers/httpsys>.

### <a name="deployment-scenarios"></a>Escenarios de implementación

Ningún firewall entre el cliente y el servidor también debe tener los puertos de comunicación abierto para el tráfico.

Si las solicitudes se reenvían en una configuración de proxy inverso, use [software intermedio de encabezados reenviados](xref:host-and-deploy/proxy-load-balancer) antes de llamar al Middleware de redireccionamiento de HTTPS. Reenvía las actualizaciones de software intermedio de encabezados el `Request.Scheme`, usando la `X-Forwarded-Proto` encabezado. Los middleware que permite redirección los identificadores URI y otras directivas de seguridad para que funcione correctamente. Cuando no se usa el software intermedio de encabezados reenviados, la aplicación de back-end no es posible que la combinación correcta de recepción y terminar en un bucle de redirección. Un mensaje de error de usuario final común es que se han producido demasiadas redirecciones.

Al implementar en Azure App Service, siga las instrucciones de [Tutorial: Enlazar un certificado SSL personalizado existente a Azure Web Apps](/azure/app-service/app-service-web-tutorial-custom-ssl).

### <a name="options"></a>Opciones

Los siguientes resaltan las llamadas de código [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) para configurar las opciones de middleware:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

Una llamada a `AddHttpsRedirection` sólo es necesario cambiar los valores de `HttpsPort` o `RedirectStatusCode`.

El código resaltado anterior:

* Conjuntos de [HttpsRedirectionOptions.RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*) a <xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>, que es el valor predeterminado. Utilice los campos de la <xref:Microsoft.AspNetCore.Http.StatusCodes> clase para las asignaciones a `RedirectStatusCode`.
* Establece el puerto HTTPS a 5001. El valor predeterminado es 443.

#### <a name="configure-permanent-redirects-in-production"></a>Configurar las redirecciones permanentes en producción

El valor predeterminado es el software intermedio para enviar un [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) con todos los redireccionamientos. Si prefiere enviar un código de estado de redirección permanente cuando la aplicación está en un entorno de desarrollo que no sean, ajuste la configuración de las opciones de middleware en una comprobación condicional para un entorno de desarrollo no.

Al configurar un `IWebHostBuilder` en *Startup.cs*:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // IHostingEnvironment (stored in _env) is injected into the Startup class.
    if (!_env.IsDevelopment())
    {
        services.AddHttpsRedirection(options =>
        {
            options.RedirectStatusCode = StatusCodes.Status308PermanentRedirect;
            options.HttpsPort = 443;
        });
    }
}
```

## <a name="https-redirection-middleware-alternative-approach"></a>Método alternativo de Middleware de redireccionamiento de HTTPS

Una alternativa al uso de Middleware de redireccionamiento de HTTPS (`UseHttpsRedirection`) consiste en usar el Middleware de reescritura de dirección URL (`AddRedirectToHttps`). `AddRedirectToHttps` También se puede establecer el código de estado y el puerto cuando se ejecuta la redirección. Para obtener más información, consulte [Middleware de reescritura de URL](xref:fundamentals/url-rewriting).

Al redirigir a HTTPS sin necesidad de reglas de redirección adicionales, se recomienda usar software intermedio de redireccionamiento de HTTPS (`UseHttpsRedirection`) se describe en este tema.

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

Para entornos de producción implementar HTTPS por primera vez, conjunto inicial [HstsOptions.MaxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*) en un valor pequeño mediante uno de los <xref:System.TimeSpan> métodos. Establezca el valor de horas en no más de un solo día en caso de que necesite revertir la infraestructura HTTPS a HTTP. Una vez que esté seguro en la sostenibilidad de la configuración de HTTPS, aumente el valor de max-age HSTS; un valor frecuente es un año.

El código siguiente:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* Establece el parámetro precarga del encabezado Strict-Transport-Security. Precarga no forma parte de la [especificación RFC HSTS](https://tools.ietf.org/html/rfc6797), pero es compatible con los exploradores web para cargar previamente sitios HSTS en una instalación nueva. Para más información, vea [https://hstspreload.org/](https://hstspreload.org/).
* Permite [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), que aplica la directiva HSTS para hospedar subdominios.
* Establece explícitamente el parámetro de antigüedad máxima del encabezado Strict-Transport-Security en 60 días. Si no se establece, el valor predeterminado es 30 días. Consulte la [max-age directiva](https://tools.ietf.org/html/rfc6797#section-6.1.1) para obtener más información.
* Agrega `example.com` a la lista de hosts para excluir.

`UseHsts` excluye los hosts de bucle invertido siguientes:

* `localhost`: La dirección de bucle invertido de IPv4.
* `127.0.0.1`: La dirección de bucle invertido de IPv4.
* `[::1]`: La dirección de bucle invertido de IPv6.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="opt-out-of-httpshsts-on-project-creation"></a>Desactivación de HTTPS/HSTS crea un proyecto

En algunos escenarios de servicio de back-end donde se controla la seguridad de conexión en el perímetro orientados al público de la red, la configuración de seguridad de conexión en cada nodo no es necesario. Web apps generados a partir de las plantillas en Visual Studio o desde la [dotnet nuevo](/dotnet/core/tools/dotnet-new) Habilitar comando [redirección HTTPS](#require-https) y [HSTS](#http-strict-transport-security-protocol-hsts). Para las implementaciones que no requieren estos escenarios, puede desactivar de HTTPS/HSTS cuando se crea la aplicación de la plantilla.

Para participar en HTTPS/HSTS:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Desactive el **configurar HTTPS** casilla de verificación.

![Nuevo cuadro de diálogo de aplicación Web ASP.NET Core que muestra la configuración de casilla de verificación HTTPS no está seleccionada.](enforcing-ssl/_static/out.png)

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli) 

Use la opción `--no-https`. Por ejemplo

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="trust"></a>

## <a name="trust-the-aspnet-core-https-development-certificate-on-windows-and-macos"></a>Confiar en el certificado de desarrollo de ASP.NET Core HTTPS en Windows y macOS

SDK de .NET core incluye un certificado de desarrollo de HTTPS. El certificado se instala como parte de la experiencia de primera ejecución. Por ejemplo, `dotnet --info` genera una salida similar al siguiente:

```text
ASP.NET Core
------------
Successfully installed the ASP.NET Core HTTPS Development Certificate.
To trust the certificate run 'dotnet dev-certs https --trust' (Windows and macOS only).
For establishing trust on other platforms refer to the platform specific documentation.
For more information on configuring HTTPS see https://go.microsoft.com/fwlink/?linkid=848054.
```

Al instalar el SDK de .NET Core se instala el certificado de desarrollo HTTPS de ASP.NET Core en el almacén de certificados local del usuario. Se ha instalado el certificado, pero no es de confianza. Confiar en el certificado de realizar el paso de un solo uso para ejecutar dotnet `dev-certs` herramienta:

```console
dotnet dev-certs https --trust
```

El siguiente comando proporciona ayuda sobre la herramienta `dev-certs`:

```console
dotnet dev-certs https --help
```

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a>Cómo configurar un certificado de desarrollador para Docker

Consulte [este problema de GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/6199).

::: moniker-end

<a name="wsl"></a>

## <a name="trust-https-certificate-from-windows-subsystem-for-linux"></a>Confiar en certificado HTTPS del subsistema de Windows para Linux

El subsistema de Windows para Linux (WSL) genera un certificado autofirmado de HTTPS. Para configurar el almacén de certificados de Windows para confiar en el certificado WSL:

* Ejecute el siguiente comando para exportar el certificado WSL generado: `dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p <cryptic-password>`
* En una ventana WSL, ejecute el siguiente comando: `ASPNETCORE_Kestrel__Certificates__Default__Password="<cryptic-password>" ASPNETCORE_Kestrel__Certificates__Default__Path=/mnt/c/Users/user-name/.aspnet/https/aspnetapp.pfx dotnet watch run`

  El comando anterior establece las variables de entorno para que Linux utiliza el certificado de confianza de Windows.

## <a name="additional-information"></a>Información adicional

* <xref:host-and-deploy/proxy-load-balancer>
* [Hospedaje de ASP.NET Core en Linux con Apache: Configuración de HTTPS](xref:host-and-deploy/linux-apache#https-configuration)
* [Hospedaje de ASP.NET Core en Linux con Nginx: Configuración de HTTPS](xref:host-and-deploy/linux-nginx#https-configuration)
* [Cómo configurar SSL en IIS](/iis/manage/configuring-security/how-to-set-up-ssl-on-iis)
* [Compatibilidad con exploradores de OWASP HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
