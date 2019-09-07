---
title: Aplicación de HTTPS en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo requerir HTTPS/TLS en una aplicación Web de ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 09/06/2019
uid: security/enforcing-ssl
ms.openlocfilehash: 654b083a0dade2fc8df5cccf9fa434f30627794b
ms.sourcegitcommit: f65d8765e4b7c894481db9b37aa6969abc625a48
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/06/2019
ms.locfileid: "70774010"
---
# <a name="enforce-https-in-aspnet-core"></a>Aplicación de HTTPS en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

En este documento se muestra cómo:

* Requiere HTTPS para todas las solicitudes.
* Redirija todas las solicitudes HTTP a HTTPS.

Ninguna API puede impedir que un cliente envíe datos confidenciales en la primera solicitud.

::: moniker range=">= aspnetcore-3.0"

> [!WARNING]
> ## <a name="api-projects"></a>Proyectos de API
>
> **No** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) en las API Web que reciben información confidencial. `RequireHttpsAttribute`utiliza códigos de Estado HTTP para redirigir exploradores de HTTP a HTTPS. Es posible que los clientes de API no conozcan o cumplan las redirecciones de HTTP a HTTPS. Estos clientes pueden enviar información a través de HTTP. Las API Web deben:
>
> * No escuche en HTTP.
> * Cierre la conexión con el código de estado 400 (solicitud incorrecta) y no atienda la solicitud.
>
> ## <a name="hsts-and-api-projects"></a>Proyectos de HSTS y API
>
> Los proyectos de API predeterminados no incluyen [HSTS](#hsts) porque HSTS suele ser una instrucción solo del explorador. Otros llamadores, como las aplicaciones de escritorio o de teléfono, **no** obedecen a la instrucción. Incluso dentro de los exploradores, una única llamada autenticada a una API sobre HTTP tiene riesgos en las redes no seguras. El enfoque seguro consiste en configurar proyectos de API para que solo escuchen y respondan a través de HTTPS.

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

> [!WARNING]
> ## <a name="api-projects"></a>Proyectos de API
>
> **No** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) en las API Web que reciben información confidencial. `RequireHttpsAttribute`utiliza códigos de Estado HTTP para redirigir exploradores de HTTP a HTTPS. Es posible que los clientes de API no conozcan o cumplan las redirecciones de HTTP a HTTPS. Estos clientes pueden enviar información a través de HTTP. Las API Web deben:
>
> * No escuche en HTTP.
> * Cierre la conexión con el código de estado 400 (solicitud incorrecta) y no atienda la solicitud.

::: moniker-end

## <a name="require-https"></a>Requisito de HTTPS

Se recomienda que las aplicaciones Web de producción ASP.NET Core usen:

* Middleware de redirección de<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>https () para redirigir las solicitudes HTTP a https.
* Middleware de HSTS ([UseHsts](#http-strict-transport-security-protocol-hsts)) para enviar encabezados del Protocolo de seguridad de transporte estricto (HSTS) http a los clientes.

> [!NOTE]
> Las aplicaciones implementadas en una configuración de proxy inverso permiten que el proxy controle la seguridad de conexión (HTTPS). Si el proxy también controla el redireccionamiento de HTTPS, no es necesario usar el middleware de redirección de HTTPS. Si el servidor proxy también controla la escritura de encabezados HSTS (por ejemplo, la [compatibilidad con HSTS nativa en IIS 10,0 (1709) o posterior](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support)), la aplicación no requiere el middleware HSTS. Para obtener más información, consulte no [participar en https/HSTS en la creación de proyectos](#opt-out-of-httpshsts-on-project-creation).

### <a name="usehttpsredirection"></a>UseHttpsRedirection

El código siguiente llama `UseHttpsRedirection` a en `Startup` la clase:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](enforcing-ssl/sample-snapshot/3.x/Startup.cs?name=snippet1&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](enforcing-ssl/sample-snapshot/2.x/Startup.cs?name=snippet1&highlight=13)]

::: moniker-end

Código resaltado anterior:

* Usa el valor predeterminado [HttpsRedirectionOptions. RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).
* Usa el valor predeterminado de [HttpsRedirectionOptions. HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (NULL) a menos que `ASPNETCORE_HTTPS_PORT` se reemplace por la variable de entorno o [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).

Se recomienda el uso de redirecciones temporales en lugar de redireccionamientos permanentes. El almacenamiento en caché de vínculos puede producir un comportamiento inestable en entornos de desarrollo. Si prefiere enviar un código de estado de redirección permanente cuando la aplicación se encuentra en un entorno que no es de desarrollo, consulte la sección [configuración de redirecciones permanentes en producción](#configure-permanent-redirects-in-production) . Se recomienda usar [HSTS](#http-strict-transport-security-protocol-hsts) para indicar a los clientes que solo se deben enviar solicitudes de recursos seguros a la aplicación (solo en producción).

### <a name="port-configuration"></a>Configuración de Puerto

Un puerto debe estar disponible para que el middleware redirija una solicitud no segura a HTTPS. Si no hay ningún puerto disponible:

* No se produce la redirección a HTTPS.
* El middleware registra la advertencia "no se pudo determinar el puerto https para la redirección".

Especifique el Puerto HTTPS mediante cualquiera de los métodos siguientes:

* Establezca [HttpsRedirectionOptions. HttpsPort](#options).

::: moniker range=">= aspnetcore-3.0"

* Establezca la `https_port` [configuración de host](/aspnet/core/fundamentals/host/generic-host?view=aspnetcore-3.0#https_port):

  * En configuración de host.
  * Estableciendo la variable `ASPNETCORE_HTTPS_PORT` de entorno.
  * Agregando una entrada de nivel superior en *appSettings. JSON*:

    [!code-json[](enforcing-ssl/sample-snapshot/3.x/appsettings.json?highlight=2)]

* Indique un puerto con el esquema seguro mediante la [variable de entorno ASPNETCORE_URLS](/aspnet/core/fundamentals/host/generic-host?view=aspnetcore-3.0#urls). La variable de entorno configura el servidor. El middleware detecta indirectamente el Puerto HTTPS a través <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>de. Este enfoque no funciona en las implementaciones de proxy inverso.

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

* Establezca la `https_port` [configuración de host](xref:fundamentals/host/web-host#https-port):

  * En configuración de host.
  * Estableciendo la variable `ASPNETCORE_HTTPS_PORT` de entorno.
  * Agregando una entrada de nivel superior en *appSettings. JSON*:

    [!code-json[](enforcing-ssl/sample-snapshot/2.x/appsettings.json?highlight=2)]

* Indique un puerto con el esquema seguro mediante la [variable de entorno ASPNETCORE_URLS](xref:fundamentals/host/web-host#server-urls). La variable de entorno configura el servidor. El middleware detecta indirectamente el Puerto HTTPS a través <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>de. Este enfoque no funciona en las implementaciones de proxy inverso.

::: moniker-end

* En desarrollo, establezca una dirección URL HTTPS en *launchsettings. JSON*. Habilite HTTPS cuando se use IIS Express.

* Configure un punto de conexión de dirección URL HTTPS para una implementación perimetral de acceso público del servidor [Kestrel](xref:fundamentals/servers/kestrel) o [http. sys](xref:fundamentals/servers/httpsys) . La aplicación solo usa **un puerto https** . El middleware detecta el puerto a través <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>de.

> [!NOTE]
> Cuando una aplicación se ejecuta en una configuración de proxy inverso, <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature> no está disponible. Establezca el puerto con uno de los otros enfoques descritos en esta sección.

### <a name="edge-deployments"></a>Implementaciones perimetrales 

Cuando Kestrel o HTTP. sys se usa como un servidor perimetral de acceso público, se debe configurar Kestrel o HTTP. sys para que escuche en ambos:

* El puerto seguro al que se redirige el cliente (normalmente, 443 en producción y 5001 en desarrollo).
* El puerto no seguro (normalmente, 80 en producción y 5000 en desarrollo).

El cliente debe tener acceso al puerto inseguro para que la aplicación reciba una solicitud no segura y redirija el cliente al puerto seguro.

Para obtener más información, vea configuración del punto <xref:fundamentals/servers/httpsys>de conexión de [Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) o.

### <a name="deployment-scenarios"></a>Escenarios de implementación

Cualquier firewall entre el cliente y el servidor también debe tener puertos de comunicación abiertos para el tráfico.

Si las solicitudes se reenvían en una configuración de proxy inverso, use el [middleware de encabezados reenviados](xref:host-and-deploy/proxy-load-balancer) antes de llamar al middleware de redireccionamiento de HTTPS. El middleware de encabezados reenviados `Request.Scheme`actualiza el, `X-Forwarded-Proto` mediante el encabezado. El middleware permite que los URI de redirección y otras directivas de seguridad funcionen correctamente. Cuando no se usa middleware de encabezados reenviados, es posible que la aplicación de back-end no reciba el esquema correcto y acabe en un bucle de redirección. Un mensaje de error común del usuario final es que se han producido demasiadas redirecciones.

Al implementar en Azure App Service, siga las instrucciones del [Tutorial: Enlazar un certificado SSL personalizado existente a Azure Web Apps](/azure/app-service/app-service-web-tutorial-custom-ssl).

### <a name="options"></a>Opciones

El siguiente código resaltado llama a [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) para configurar las opciones de middleware:


::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](enforcing-ssl/sample-snapshot/3.x/Startup.cs?name=snippet2&highlight=14-18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](enforcing-ssl/sample-snapshot/2.x/Startup.cs?name=snippet2&highlight=14-18)]

::: moniker-end


Llamar `AddHttpsRedirection` a solo es necesario para cambiar los valores `HttpsPort` de `RedirectStatusCode`o.

Código resaltado anterior:

* Establece [HttpsRedirectionOptions. RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*) en <xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>, que es el valor predeterminado. Utilice los campos de la <xref:Microsoft.AspNetCore.Http.StatusCodes> clase para las asignaciones a. `RedirectStatusCode`
* Establece el Puerto HTTPS en 5001. El valor predeterminado es 443.

#### <a name="configure-permanent-redirects-in-production"></a>Configuración de redirecciones permanentes en producción

De forma predeterminada, el middleware envía un [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) con todos los redireccionamientos. Si prefiere enviar un código de estado de redirección permanente cuando la aplicación se encuentra en un entorno que no es de desarrollo, ajuste la configuración de las opciones de middleware en una comprobación condicional para un entorno que no sea de desarrollo.

::: moniker range=">= aspnetcore-3.0"

Al configurar servicios en *Startup.CS*:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // IWebHostEnvironment (stored in _env) is injected into the Startup class.
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

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

Al configurar servicios en *Startup.CS*:

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

::: moniker-end


## <a name="https-redirection-middleware-alternative-approach"></a>Enfoque alternativo de middleware de redirección de HTTPS

Una alternativa al uso del middleware de redirección`UseHttpsRedirection`de HTTPS () es usar el middleware de`AddRedirectToHttps`reescritura de direcciones URL (). `AddRedirectToHttps`también puede establecer el código de estado y el puerto cuando se ejecuta la redirección. Para obtener más información, vea [middleware de reescritura de direcciones URL](xref:fundamentals/url-rewriting).

Al redirigir a https sin necesidad de reglas de redirección adicionales, se recomienda usar el middleware de redireccionamiento`UseHttpsRedirection`de HTTPS () que se describe en este tema.

<a name="hsts"></a>

## <a name="http-strict-transport-security-protocol-hsts"></a>Protocolo de seguridad de transporte estricto HTTP (HSTS)

Por [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), la [seguridad de transporte estricta http (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) es una mejora de seguridad opcional que se especifica mediante una aplicación Web mediante el uso de un encabezado de respuesta. Cuando un [explorador que admite HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) recibe este encabezado:

* El explorador almacena la configuración del dominio que evita el envío de cualquier comunicación a través de HTTP. El explorador fuerza toda la comunicación a través de HTTPS.
* El explorador impide que el usuario Use certificados no confiables o no válidos. El explorador deshabilita los mensajes que permiten a un usuario confiar temporalmente en este tipo de certificado.

Dado que el cliente exige HSTS, tiene algunas limitaciones:

* El cliente debe admitir HSTS.
* HSTS requiere al menos una solicitud HTTPS correcta para establecer la Directiva HSTS.
* La aplicación debe comprobar cada solicitud HTTP y redirigir o rechazar la solicitud HTTP.

ASP.net Core 2,1 y posteriores implementa HSTS con el método `UseHsts` de extensión. El código siguiente llama `UseHsts` a cuando la aplicación no está en [modo de desarrollo](xref:fundamentals/environments):

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](enforcing-ssl/sample-snapshot/3.x/Startup.cs?name=snippet1&highlight=11)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](enforcing-ssl/sample-snapshot/2.x/Startup.cs?name=snippet1&highlight=10)]

::: moniker-end

`UseHsts`no se recomienda en el desarrollo porque los exploradores pueden almacenar en memoria caché los valores de HSTS. De forma predeterminada `UseHsts` , excluye la dirección de bucle invertido local.

En el caso de los entornos de producción que implementan https por primera vez, establezca [HstsOptions. MaxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*) inicial en un valor pequeño utilizando <xref:System.TimeSpan> uno de los métodos. Establezca el valor de horas en no más de un día único en caso de que necesite revertir la infraestructura HTTPS a HTTP. Después de estar seguro de la sostenibilidad de la configuración de HTTPS, aumente el valor de HSTS Max-Age. un valor utilizado comúnmente es un año.

El código siguiente:


::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](enforcing-ssl/sample-snapshot/3.x/Startup.cs?name=snippet2&highlight=5-12)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](enforcing-ssl/sample-snapshot/2.x/Startup.cs?name=snippet2&highlight=5-12)]

::: moniker-end


* Establece el parámetro preload del encabezado STRICT-Transport-Security. La precarga no forma parte de la [especificación RFC HSTS](https://tools.ietf.org/html/rfc6797), pero es compatible con los exploradores Web para precargar sitios de HSTS en la instalación nueva. Para más información, vea [https://hstspreload.org/](https://hstspreload.org/).
* Habilita [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), que aplica la Directiva HSTS para hospedar subdominios.
* Establece explícitamente el parámetro Max-Age del encabezado STRICT-Transport-Security en 60 días. Si no se establece, el valor predeterminado es 30 días. Para obtener más información, vea la [Directiva Max-Age](https://tools.ietf.org/html/rfc6797#section-6.1.1) .
* Agrega `example.com` a la lista de hosts que se van a excluir.

`UseHsts`excluye los siguientes hosts de bucle invertido:

* `localhost`: Dirección de bucle invertido IPv4.
* `127.0.0.1`: Dirección de bucle invertido IPv4.
* `[::1]`: Dirección de bucle invertido IPv6.

## <a name="opt-out-of-httpshsts-on-project-creation"></a>No participar en HTTPS/HSTS en la creación de proyectos

En algunos escenarios de servicio de back-end en los que la seguridad de la conexión se controla en el perímetro de acceso público de la red, no es necesario configurar la seguridad de conexión en cada nodo. Las aplicaciones web que se generan a partir de las plantillas en Visual Studio o desde el comando [dotnet New](/dotnet/core/tools/dotnet-new) habilitan la [redirección de https](#require-https) y [HSTS](#http-strict-transport-security-protocol-hsts). En el caso de las implementaciones que no requieren estos escenarios, puede optar por HTTPS/HSTS cuando la aplicación se crea a partir de la plantilla.

Para no participar en HTTPS/HSTS:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Desactive la casilla **configurar para https** .

::: moniker range=">= aspnetcore-3.0"

![Cuadro de diálogo Nueva ASP.NET Core aplicación web que muestra la casilla de verificación configurar para HTTPS no seleccionada.](enforcing-ssl/_static/out-vs2019.png)

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

![Cuadro de diálogo Nueva ASP.NET Core aplicación web que muestra la casilla de verificación configurar para HTTPS no seleccionada.](enforcing-ssl/_static/out.png)

::: moniker-end


# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli) 

Use la opción `--no-https`. Por ejemplo

```console
dotnet new webapp --no-https
```

---

<a name="trust"></a>

## <a name="trust-the-aspnet-core-https-development-certificate-on-windows-and-macos"></a>Confíe en el certificado de desarrollo de ASP.NET Core HTTPS en Windows y macOS

El SDK de .NET Core incluye un certificado de desarrollo de HTTPS. El certificado se instala como parte de la experiencia de primera ejecución. Por ejemplo, `dotnet --info` genera una salida similar a la siguiente:

```text
ASP.NET Core
------------
Successfully installed the ASP.NET Core HTTPS Development Certificate.
To trust the certificate run 'dotnet dev-certs https --trust' (Windows and macOS only).
For establishing trust on other platforms refer to the platform specific documentation.
For more information on configuring HTTPS see https://go.microsoft.com/fwlink/?linkid=848054.
```

Al instalar el SDK de .NET Core se instala el certificado de desarrollo HTTPS de ASP.NET Core en el almacén de certificados local del usuario. El certificado se ha instalado, pero no es de confianza. Para confiar en el certificado, realice el paso de una vez para ejecutar `dev-certs` la herramienta dotnet:

```console
dotnet dev-certs https --trust
```

El siguiente comando proporciona ayuda sobre la herramienta `dev-certs`:

```console
dotnet dev-certs https --help
```

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a>Cómo configurar un certificado de desarrollador para Docker

Consulte [este problema de GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/6199).

<a name="wsl"></a>

## <a name="trust-https-certificate-from-windows-subsystem-for-linux"></a>Confiar en el certificado HTTPS del subsistema de Windows para Linux

El subsistema de Windows para Linux (WSL) genera un certificado autofirmado HTTPS. Para configurar el almacén de certificados de Windows para que confíe en el certificado WSL:

* Ejecute el siguiente comando para exportar el certificado generado por WSL:`dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p <cryptic-password>`
* En una ventana de WSL, ejecute el siguiente comando:`ASPNETCORE_Kestrel__Certificates__Default__Password="<cryptic-password>" ASPNETCORE_Kestrel__Certificates__Default__Path=/mnt/c/Users/user-name/.aspnet/https/aspnetapp.pfx dotnet watch run`

  El comando anterior establece las variables de entorno, por lo que Linux usa el certificado de confianza de Windows.

## <a name="additional-information"></a>Información adicional

* <xref:host-and-deploy/proxy-load-balancer>
* [ASP.NET Core host en Linux con Apache: Configuración de HTTPS](xref:host-and-deploy/linux-apache#https-configuration)
* [ASP.NET Core de host en Linux con NGINX: Configuración de HTTPS](xref:host-and-deploy/linux-nginx#https-configuration)
* [Procedimientos para configurar SSL en IIS](/iis/manage/configuring-security/how-to-set-up-ssl-on-iis)
* [Compatibilidad con el explorador de OWASP HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
