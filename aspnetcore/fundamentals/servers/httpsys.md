---
title: Implementación del servidor web HTTP.sys en ASP.NET Core
author: guardrex
description: Obtenga información sobre HTTP.sys, un servidor web para ASP.NET Core en Windows. Basado en el controlador de modo kernel de HTTP.sys, HTTP.sys es una alternativa a Kestrel que se puede usar para conectarse directamente a Internet sin IIS.
monikerRange: '>= aspnetcore-2.0'
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/03/2019
uid: fundamentals/servers/httpsys
ms.openlocfilehash: 46538d256ae2c5f3b7e6c725fa8f29092759f69f
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098859"
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a>Implementación del servidor web HTTP.sys en ASP.NET Core

Por [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher) y [Stephen Halter](https://github.com/guardrex)

[HTTP.sys](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture#hypertext-transfer-protocol-stack-httpsys) es un [servidor web de ASP.NET Core](xref:fundamentals/servers/index) que solo se ejecuta en Windows. HTTP.sys supone una alternativa al servidor [Kestrel](xref:fundamentals/servers/kestrel) y ofrece algunas características que este no facilita.

> [!IMPORTANT]
> HTTP.sys no es compatible con el [módulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module) y no se puede usar con IIS o IIS Express.

HTTP.sys admite las siguientes características:

* [Autenticación de Windows](xref:security/authentication/windowsauth)
* Uso compartido de puertos
* HTTPS con SNI
* HTTP/2 a través de TLS (Windows 10 o posterior)
* Transmisión directa de archivos
* Almacenamiento en caché de respuestas
* WebSockets (Windows 8 o posterior)

Versiones de Windows compatibles:

* Windows 7 o posterior
* Windows Server 2008 R2 o posterior

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="when-to-use-httpsys"></a>Cuándo usar HTTP.sys

HTTP.sys resulta útil para implementaciones en las que:

* Es necesario exponer el servidor directamente a Internet sin usar IIS.

  ![HTTP.sys se comunica directamente con Internet](httpsys/_static/httpsys-to-internet.png)

* Una implementación interna requiere una característica que no está disponible en Kestrel, como [Autenticación de Windows](xref:security/authentication/windowsauth).

  ![HTTP.sys se comunica directamente con la red interna](httpsys/_static/httpsys-to-internal.png)

HTTP.sys es una tecnología consolidada que protege contra muchos tipos de ataques y que proporciona la solidez, la seguridad y la escalabilidad de un servidor web con todas las características. El propio IIS se ejecuta como agente de escucha de HTTP sobre HTTP.sys.

## <a name="http2-support"></a>Compatibilidad con HTTP/2

[HTTP/2](https://httpwg.org/specs/rfc7540.html) está habilitado para las aplicaciones de ASP.NET Core si se cumplen los siguientes requisitos básicos:

* Windows Server 2016/Windows 10 o posterior
* Conexión con [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3)
* Conexión con TLS 1.2 o una versión posterior

::: moniker range=">= aspnetcore-2.2"

Si se establece una conexión HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) notifica `HTTP/2`.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Si se establece una conexión HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) notifica `HTTP/1.1`.

::: moniker-end

HTTP/2 está habilitado de forma predeterminada. Si no se establece una conexión HTTP/2, la conexión vuelve a HTTP/1.1. En una futura versión de Windows, las marcas de configuración de HTTP/2 estarán disponibles, incluida la capacidad para deshabilitar HTTP/2 con HTTP.sys.

## <a name="kernel-mode-authentication-with-kerberos"></a>Autenticación de modo kernel con Kerberos

HTTP.sys delega en la autenticación de modo kernel con el protocolo de autenticación de Kerberos. La autenticación de modo usuario no se admite con Kerberos y HTTP.sys. Se debe usar la cuenta de equipo para descifrar el token o el vale de Kerberos que se obtiene de Active Directory y que el cliente reenvía al servidor para autenticar al usuario. Registre el nombre de entidad de seguridad de servicio (SPN) para el host, no el usuario de la aplicación.

## <a name="how-to-use-httpsys"></a>Cómo usar HTTP.sys

### <a name="configure-the-aspnet-core-app-to-use-httpsys"></a>Configuración de la aplicación de ASP.NET Core para usar HTTP.sys

1. No se requiere una referencia de paquete en el archivo de proyecto cuando se usa el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) ([nuget.org](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)) (ASP.NET Core 2.1 o posterior). Si no usa el metapaquete `Microsoft.AspNetCore.App`, agregue una referencia de paquete a [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/).

2. Llame al método de extensión [UseHttpSys](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderhttpsysextensions.usehttpsys) extensión al compilar el host de web y especifique las [opciones de HTTP.sys](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions) necesarias:

   [!code-csharp[](httpsys/sample/Program.cs?name=snippet1&highlight=4-12)]

   La configuración adicional de HTTP.sys se controla a través de [Configuración del Registro](https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows).

   **Opciones de HTTP.sys**

   | Propiedad. | Descripción | Default |
   | -------- | ----------- | :-----: |
   | [AllowSynchronousIO](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.allowsynchronousio) | Controlar si se permite la entrada/salida sincrónica de los objetos `HttpContext.Request.Body` y `HttpContext.Response.Body`. | `true` |
   | [Authentication.AllowAnonymous](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationmanager.allowanonymous) | Permitir solicitudes anónimas. | `true` |
   | [Authentication.Schemes](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationmanager.schemes) | Especificar los esquemas de autenticación permitidos. Puede modificarse en cualquier momento antes de eliminar el agente de escucha. Los valores se proporcionan con la [enumeración AuthenticationSchemes](/dotnet/api/microsoft.aspnetcore.server.httpsys.authenticationschemes): `Basic`, `Kerberos`, `Negotiate`, `None` y `NTLM`. | `None` |
   | [EnableResponseCaching](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.enableresponsecaching) | Intentar el almacenamiento en memoria caché en [modo kernel](/windows-hardware/drivers/gettingstarted/user-mode-and-kernel-mode) de las respuestas con encabezados elegibles. Es posible que la respuesta no incluya encabezados `Set-Cookie`, `Vary` o `Pragma`. Debe incluir un encabezado `Cache-Control` que sea `public` y un valor `shared-max-age` o `max-age`, o un encabezado `Expires`. | `true` |
   | [MaxAccepts](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxaccepts) | Número máximo de aceptaciones simultáneas. | Cinco &times; [entorno.<br> ProcessorCount](/dotnet/api/system.environment.processorcount) |
   | [MaxConnections](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxconnections) | Establecer el número máximo de conexiones simultáneas que se aceptan. Use `-1` para infinito. Use `null` para usar la configuración de la máquina del Registro. | `null`<br>(ilimitado) |
   | [MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxrequestbodysize) | Vea la sección <a href="#maxrequestbodysize">MaxRequestBodySize</a>. | 30 000 000 bytes<br>(~28,6 MB) |
   | [RequestQueueLimit](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.requestqueuelimit) | Número máximo de solicitudes que se pueden poner en cola. | 1000 |
   | [ThrowWriteExceptions](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.throwwriteexceptions) | Indicar si las escrituras del cuerpo de respuesta que no se producen debido a desconexiones del cliente deben iniciar excepciones o finalizar con normalidad. | `false`<br>(finalizar con normalidad) |
   | [Timeouts](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.timeouts) | Exponer la configuración de [TimeoutManager](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager) de HTTP.sys, que también puede configurarse en el Registro. Siga los vínculos de API para obtener más información sobre cada configuración, incluidos los valores predeterminados:<ul><li>[TimeoutManager.DrainEntityBody](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.drainentitybody) &ndash; Tiempo permitido para que la API HTTP Server purgue el cuerpo de la entidad en una conexión persistente.</li><li>[TimeoutManager.EntityBody](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.entitybody) &ndash; Tiempo permitido para que llegue el cuerpo de la entidad de solicitud.</li><li>[TimeoutManager.HeaderWait](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.headerwait) &ndash; Tiempo permitido para que la API HTTP Server analice el encabezado de solicitud.</li><li>[TimeoutManager.IdleConnection](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.idleconnection) &ndash; Tiempo permitido para una conexión inactiva.</li><li>[TimeoutManager.MinSendBytesPerSecond](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.minsendbytespersecond) &ndash; Tasa mínima de envío de la respuesta.</li><li>[TimeoutManager.RequestQueue](/dotnet/api/microsoft.aspnetcore.server.httpsys.timeoutmanager.requestqueue) &ndash; Tiempo permitido para que la solicitud permanezca en la cola de solicitudes antes de que la aplicación la recoja.</li></ul> |  |
   | [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes) | Especifique el objeto [UrlPrefixCollection](/dotnet/api/microsoft.aspnetcore.server.httpsys.urlprefixcollection) que se registra con HTTP.sys. El más útil es [UrlPrefixCollection.Add](/dotnet/api/microsoft.aspnetcore.server.httpsys.urlprefixcollection.add), que se usa para agregar un prefijo a la colección. Pueden modificarse en cualquier momento antes de eliminar el agente de escucha. |  |

   <a name="maxrequestbodysize"></a>
   **MaxRequestBodySize**

   Tamaño máximo permitido de cualquier cuerpo de solicitud en bytes. Cuando se establece en `null`, el tamaño máximo del cuerpo de solicitud es ilimitado. Este límite no tiene ningún efecto en las conexiones actualizadas, que siempre son ilimitadas.

   El método recomendado para reemplazar el límite de una aplicación de ASP.NET Core MVC para un solo objeto `IActionResult` consiste en usar el atributo [RequestSizeLimitAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) en un método de acción:

   ```csharp
   [RequestSizeLimit(100000000)]
   public IActionResult MyActionMethod()
   ```

   Se inicia una excepción si la aplicación intenta configurar el límite de una solicitud después de que la aplicación haya empezado a leer la solicitud. Puede usarse una propiedad `IsReadOnly` que indique si la propiedad `MaxRequestBodySize` tiene el estado de solo lectura, lo que significa que es demasiado tarde para configurar el límite.

   Si la aplicación debe reemplazar [MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.maxrequestbodysize) por solicitud, use la interfaz [IHttpMaxRequestBodySizeFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpmaxrequestbodysizefeature):

   [!code-csharp[](httpsys/sample/Startup.cs?name=snippet1&highlight=6-7)]

3. If usa Visual Studio, asegúrese de que la aplicación no está configurada para ejecutar IIS o IIS Express.

   En Visual Studio, el perfil de inicio predeterminado es para IIS Express. Para ejecutar el proyecto como aplicación de consola, cambie manualmente el perfil seleccionado, tal y como se muestra en la siguiente captura de pantalla:

   ![Seleccionar perfil de aplicación de consola](httpsys/_static/vs-choose-profile.png)

### <a name="configure-windows-server"></a>Configurar Windows Server

1. Determine los puertos que se abrirán para la aplicación y use el Firewall de Windows o los [cmdlet de PowerShell](https://technet.microsoft.com/library/jj554906) para abrir los puertos del firewall y permitir que el tráfico llegue a HTTP.sys. Cuando realice una implementación en una máquina virtual de Azure, abra los puertos del [grupo de seguridad de red](/azure/virtual-network/security-overview). En los comandos y la configuración de la aplicación siguientes, se usa el puerto 443.

1. Si es necesario, obtenga e instale los certificados X.509.

   En Windows, puede crear certificados autofirmados con el [cmdlet New-SelfSignedCertificate de PowerShell](/powershell/module/pkiclient/new-selfsignedcertificate). Para obtener un ejemplo no compatible, vea [UpdateIISExpressSSLForChrome.ps1](https://github.com/aspnet/Docs/tree/master/aspnetcore/includes/make-x509-cert/UpdateIISExpressSSLForChrome.ps1).

   Instale certificados autofirmados o firmados por CA en el almacén **personal del equipo** > **local** del servidor.

1. Si la aplicación es una [implementación dependiente del marco](/dotnet/core/deploying/#framework-dependent-deployments-fdd), instale .NET Core, .NET Framework o ambos (si se trata de una aplicación de .NET Core que tiene como destino .NET Framework).

   * **.NET Core**: si la aplicación requiere .NET Core, obtenga y ejecute el instalador del **entorno de ejecución de .NET Core** desde las [descargas de .NET](https://dotnet.microsoft.com/download). No instale el SDK completo en el servidor.
   * **.NET Framework**: si la aplicación requiere .NET Framework, consulte la [guía de instalación de .NET Framework.](/dotnet/framework/install/) Instale la versión necesaria de .NET Framework. El instalador de la versión más reciente de .NET Framework está disponible en la página de [descargas de .NET Core](https://dotnet.microsoft.com/download).

   Si la aplicación se basa en la [implementación autocontenida](/dotnet/core/deploying/#framework-dependent-deployments-scd), incluirá el entorno de ejecución en la implementación. No se requiere la instalación de ningún marco en el servidor.

1. Configure los puertos y las direcciones URL en la aplicación.

   ASP.NET Core se enlaza a `http://localhost:5000` de forma predeterminada. Para configurar los puertos y los prefijos de dirección URL, las opciones incluyen lo siguiente:

   * [UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)
   * El argumento de la línea de comandos `urls`
   * La variable de entorno `ASPNETCORE_URLS`
   * [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes)

   En el código siguiente, se muestra cómo usar [UrlPrefixes](/dotnet/api/microsoft.aspnetcore.server.httpsys.httpsysoptions.urlprefixes) con la dirección IP del servidor, `10.0.0.4`, en el puerto 443:

   [!code-csharp[](httpsys/sample_snapshot/Program.cs?name=snippet1&highlight=11)]

   Una ventaja de `UrlPrefixes` es que se genera inmediatamente un mensaje de error para prefijos con formato incorrecto.

   La configuración de `UrlPrefixes` invalida la configuración de `UseUrls`/`urls`/`ASPNETCORE_URLS`. Por lo tanto, la ventaja de `UseUrls`, `urls` y la variable de entorno `ASPNETCORE_URLS` es que resulta más fácil de cambiar entre Kestrel y HTTP.sys. Para obtener más información, vea <xref:fundamentals/host/web-host>.

   HTTP.sys usa los [formatos de cadena UrlPrefix de la API HTTP Server](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).

   > [!WARNING]
   > Los enlaces de carácter comodín de nivel superior (`http://*:80/` y `http://+:80`) **no** se deben usar. Los enlaces de carácter comodín de nivel superior generan vulnerabilidades de seguridad en la aplicación. Esto se aplica tanto a los caracteres comodín fuertes como a los débiles. Use nombres de host explícitos o direcciones IP en lugar de caracteres comodín. Los enlaces de carácter comodín de subdominio (por ejemplo, `*.mysub.com`) no suponen ningún riesgo de seguridad si se controla todo el dominio primario (a diferencia de `*.com`, que sí es vulnerable). Para obtener más información, vea [RFC 7230: Sección 5.4: Host](https://tools.ietf.org/html/rfc7230#section-5.4).

1. Registre previamente los prefijos de URL en el servidor.

   La herramienta integrada para configurar HTTP.sys es *netsh.exe*. *netsh.exe* se usa para reservar prefijos de dirección URL y asignar certificados X.509. Esta herramienta requiere privilegios de administrador.

   Use la herramienta *netsh.exe* para registrar URL de la aplicación:

   ```console
   netsh http add urlacl url=<URL> user=<USER>
   ```

   * `<URL>`: localizador uniforme de recursos (URL) completo especificado. No use ningún enlace de carácter comodín. Use un nombre de host válido o la dirección IP local. *La dirección URL debe incluir una barra diagonal final.*
   * `<USER>`: especifica el nombre de usuario o de grupo de usuarios.

   En el ejemplo siguiente, la dirección IP local del servidor es `10.0.0.4`:

   ```console
   netsh http add urlacl url=https://10.0.0.4:443/ user=Users
   ```

   Cuando se registra una dirección URL, la herramienta responde con `URL reservation successfully added`.

   Para eliminar una dirección URL registrada, use el comando `delete urlacl`:

   ```console
   netsh http delete urlacl url=<URL>
   ```

1. Registre certificados X.509 en el servidor.

   Use la herramienta *netsh.exe* para registrar certificados de la aplicación:

   ```console
   netsh http add sslcert ipport=<IP>:<PORT> certhash=<THUMBPRINT> appid="{<GUID>}"
   ```

   * `<IP>`: especifica la dirección IP local para el enlace. No use ningún enlace de carácter comodín. Use una dirección IP válida.
   * `<PORT>`: especifica el puerto para el enlace.
   * `<THUMBPRINT>`: huella digital del certificado X.509.
   * `<GUID>`: GUID generado por el desarrollador para representar la aplicación con fines informativos.

   A modo de referencia, almacene el GUID en la aplicación como etiqueta de paquete:

   * En Visual Studio:
     * Abra las propiedades del proyecto de la aplicación. Para ello, haga clic con el botón derecho en la aplicación, en el **Explorador de soluciones**, y seleccione **Propiedades**.
     * Seleccione la pestaña **Paquete**.
     * Escriba el GUID que creó en el campo **Etiquetas**.
   * Si no usa Visual Studio:
     * Abra el archivo de proyecto de la aplicación.
     * Agregue una propiedad `<PackageTags>` a un elemento `<PropertyGroup>` nuevo o existente con el GUID que creó:

       ```xml
       <PropertyGroup>
         <PackageTags>9412ee86-c21b-4eb8-bd89-f650fbf44931</PackageTags>
       </PropertyGroup>
       ```

   En el ejemplo siguiente:

   * La dirección IP local del servidor es `10.0.0.4`.
   * Un generador de GUID aleatorios en línea proporciona el valor `appid`.

   ```console
   netsh http add sslcert 
       ipport=10.0.0.4:443 
       certhash=b66ee04419d4ee37464ab8785ff02449980eae10 
       appid="{9412ee86-c21b-4eb8-bd89-f650fbf44931}"
   ```

   Cuando se registra un certificado, la herramienta responde con `SSL Certificate successfully added`.

   Para eliminar un registro de certificados, use el comando `delete sslcert`:

   ```console
   netsh http delete sslcert ipport=<IP>:<PORT>
   ```

   Documentación de referencia de *netsh.exe*:

   * [Comandos Netsh para protocolo de transferencia de hipertexto (HTTP)](https://technet.microsoft.com/library/cc725882.aspx)
   * [Cadenas de UrlPrefix](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

1. Ejecute la aplicación.

   No se necesitan privilegios de administrador para ejecutar la aplicación al enlazar a localhost mediante HTTP (no HTTPS) con un número de puerto mayor que 1024. Para otras configuraciones (por ejemplo, usar una dirección IP local o enlazar al puerto 443), ejecute la aplicación con privilegios de administrador.

   La aplicación responde a la dirección IP pública del servidor. En este ejemplo, el servidor está disponible en Internet mediante su dirección IP pública, `104.214.79.47`.

   En este ejemplo, se usa un certificado de desarrollo. La página se carga de forma segura tras omitir la advertencia de que el certificado no es de confianza para el explorador.

   ![Ventana del explorador en la que se muestra cargada la página de índice de la aplicación.](httpsys/_static/browser.png)

## <a name="proxy-server-and-load-balancer-scenarios"></a>Escenarios de servidor proxy y equilibrador de carga

En el caso de las aplicaciones hospedadas por HTTP.sys que interactúan con las solicitudes de Internet o de una red corporativa, puede que sea necesario configurar más elementos si esas aplicaciones se hospedan detrás de servidores proxy y equilibradores de carga. Para más información, vea [Configurar ASP.NET Core para trabajar con servidores proxy y equilibradores de carga](xref:host-and-deploy/proxy-load-balancer).

## <a name="additional-resources"></a>Recursos adicionales

* [Habilitación de la autenticación de Windows con HTTP.sys](xref:security/authentication/windowsauth#enable-windows-authentication-with-httpsys)
* [API HTTP Server](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx)
* [Repositorio aspnet/HttpSysServer de GitHub (código fuente)](https://github.com/aspnet/HttpSysServer/)
* <xref:fundamentals/host/index>
* <xref:test/troubleshoot>
