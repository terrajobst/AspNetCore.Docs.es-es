---
title: Configurar la autenticación de Windows en ASP.NET Core
author: scottaddie
description: Obtenga información sobre cómo configurar la autenticación de Windows en ASP.NET Core para IIS y HTTP.sys.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 07/01/2019
uid: security/authentication/windowsauth
ms.openlocfilehash: 30f1f554a29412ed6b84115d457d2da1aba91c17
ms.sourcegitcommit: eb3e51d58dd713eefc242148f45bd9486be3a78a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2019
ms.locfileid: "67500504"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a>Configurar la autenticación de Windows en ASP.NET Core

Por [Scott Addie](https://twitter.com/Scott_Addie) y [Luke Latham](https://github.com/guardrex)

::: moniker range=">= aspnetcore-3.0"

Se puede configurar la autenticación de Windows (también conocida como autenticación Negotiate, Kerberos o NTLM) para las aplicaciones de ASP.NET Core hospedadas con [IIS](xref:host-and-deploy/iis/index), [Kestrel](xref:fundamentals/servers/kestrel), o [HTTP.sys](xref:fundamentals/servers/httpsys) .

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Se puede configurar la autenticación de Windows (también conocida como autenticación Negotiate, Kerberos o NTLM) para las aplicaciones de ASP.NET Core hospedadas con [IIS](xref:host-and-deploy/iis/index) o [HTTP.sys](xref:fundamentals/servers/httpsys).

::: moniker-end

Autenticación de Windows se basa en el sistema operativo para autenticar usuarios de aplicaciones de ASP.NET Core. Puede usar la autenticación de Windows cuando el servidor se ejecuta en una red corporativa mediante identidades de dominio de Active Directory o cuentas de Windows para identificar a los usuarios. Autenticación de Windows se adapta mejor a los entornos de intranet donde los usuarios, las aplicaciones cliente y servidores web pertenecen al mismo dominio de Windows.

> [!NOTE]
> No se admite la autenticación de Windows con HTTP/2. Desafíos de autenticación se pueden enviar en las respuestas HTTP/2, pero el cliente debe cambiar a HTTP/1.1 antes de autenticar.

## <a name="iisiis-express"></a>IIS/IIS Express

Agregar servicios de autenticación mediante la invocación <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.IISIntegration?displayProperty=fullName> espacio de nombres) en `Startup.ConfigureServices`:

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

### <a name="launch-settings-debugger"></a>Iniciar configuración (depurador)

Solo afecta la configuración para la configuración de inicio la *Properties/launchSettings.json* de archivos para IIS Express y no configura IIS para la autenticación de Windows. Configuración del servidor se explica en el [IIS](#iis) sección.

El **aplicación Web** plantilla disponible a través de Visual Studio o la CLI de .NET Core puede configurarse para admitir la autenticación de Windows, que actualiza el *Properties/launchSettings.json* archivo de forma automática.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

**nuevo proyecto**

1. Cree un nuevo proyecto.
1. Seleccione **Aplicación web de ASP.NET Core**. Seleccione **Siguiente**.
1. Proporcione un nombre en el **nombre del proyecto** campo. Confirme la **ubicación** entrada es correcta o proporcionar una ubicación para el proyecto. Seleccione **Crear**.
1. Seleccione **cambio** en **autenticación**.
1. En el **Cambiar autenticación** ventana, seleccione **Windows autenticación**. Seleccione **Aceptar**.
1. Seleccione **Aplicación web**.
1. Seleccione **Crear**.

Ejecutar la aplicación. El nombre de usuario aparece en la interfaz de usuario de la aplicación representada.

**Proyecto existente**

Las propiedades del proyecto habilitar la autenticación de Windows y deshabilitar la autenticación anónima:

1. Haga clic con el botón derecho en el proyecto en el **Explorador de soluciones** y seleccione **Propiedades**.
1. Seleccione la pestaña **Depurar**.
1. Desactive la casilla de verificación **habilitar la autenticación anónima**.
1. Seleccione la casilla de verificación **habilitar la autenticación de Windows**.
1. Guarde y cierre la página de propiedades.

Como alternativa, se pueden configurar las propiedades en el `iisSettings` nodo de la *launchSettings.json* archivo:

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[Visual Studio Code y CLI de .NET Core](#tab/visual-studio-code+netcore-cli)

**nuevo proyecto**

Ejecute el [dotnet nuevo](/dotnet/core/tools/dotnet-new) comando con el `webapp` argumento (aplicación Web de ASP.NET Core) y `--auth Windows` cambiar:

```console
dotnet new webapp --auth Windows
```

**Proyecto existente**

Actualización de la `iisSettings` nodo de la *launchSettings.json* archivo:

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

---

Cuando se modifica un proyecto existente, compruebe que el archivo de proyecto incluye una referencia de paquete para el [Microsoft.AspNetCore.App metapaquete](xref:fundamentals/metapackage-app) **o** el [ Microsoft.AspNetCore.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication/) paquete NuGet.

### <a name="iis"></a>IIS

IIS usa el [módulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module) para hospedar aplicaciones ASP.NET Core. Autenticación de Windows se configura para IIS a través de la *web.config* archivo. Las siguientes secciones muestran cómo:

* Proporcionar una variable local *web.config* archivo que activa la autenticación de Windows en el servidor cuando se implementa la aplicación.
* Use el Administrador de IIS para configurar el *web.config* archivos de una aplicación de ASP.NET Core que ya se ha implementado en el servidor.

Si aún no lo ha hecho, habilite IIS para hospedar aplicaciones ASP.NET Core. Para obtener más información, consulta <xref:host-and-deploy/iis/index>.

Habilite el servicio de rol IIS para la autenticación de Windows. Para obtener más información, consulte [Habilitar autenticación de Windows en servicios de rol de IIS (consulte el paso 2)](xref:host-and-deploy/iis/index#iis-configuration).

[Middleware de IIS Integration](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) está configurado para autenticar automáticamente las solicitudes de forma predeterminada. Para obtener más información, consulte [Host ASP.NET Core en Windows con IIS: Las opciones de IIS (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).

El módulo ASP.NET Core está configurado para reenviar el token de autenticación de Windows para la aplicación de forma predeterminada. Para obtener más información, consulte [referencia de configuración del módulo ASP.NET Core: Atributos del elemento aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

Use **cualquier** de los métodos siguientes:

* **Antes de publicar e implementar el proyecto,** agregue las siguientes *web.config* archivo a la raíz del proyecto:

  [!code-xml[](windowsauth/sample_snapshot/web_2.config)]

  Cuando se publica el proyecto mediante el SDK de .NET Core (sin el `<IsTransformWebConfigDisabled>` propiedad establecida en `true` en el archivo de proyecto), publicado *web.config* archivo incluye la `<location><system.webServer><security><authentication>` sección. Para obtener más información sobre la `<IsTransformWebConfigDisabled>` propiedad, vea <xref:host-and-deploy/iis/index#webconfig-file>.

* **Después de publicar e implementar el proyecto,** realizar la configuración de servidor con el Administrador de IIS:

  1. En el Administrador de IIS, seleccione el sitio IIS en el **sitios** nodo de la **conexiones** barra lateral.
  1. Haga doble clic en **autenticación** en el **IIS** área.
  1. Seleccione **autenticación anónima**. Seleccione **deshabilitar** en el **acciones** barra lateral.
  1. Seleccione **Windows autenticación**. Seleccione **habilitar** en el **acciones** barra lateral.

  Cuando se realizan estas acciones, el Administrador de IIS modifica la aplicación *web.config* archivo. Un `<system.webServer><security><authentication>` se agrega el nodo con la configuración actualizada para `anonymousAuthentication` y `windowsAuthentication`:

  [!code-xml[](windowsauth/sample_snapshot/web_1.config?highlight=4-5)]

  El `<system.webServer>` sección agregada a la *web.config* archivo por el Administrador de IIS está fuera de la aplicación `<location>` sección agregada por el SDK de .NET Core cuando se publique la aplicación. Dado que se agrega la sección fuera de la `<location>` nodo, la configuración se hereda por cualquier [las aplicaciones secundarias](xref:host-and-deploy/iis/index#sub-applications) a la aplicación actual. Para impedir la herencia, mover agregado `<security>` sección dentro de la `<location><system.webServer>` sección suministradas por el SDK de .NET Core.

  Cuando se usa el Administrador de IIS para agregar la configuración de IIS, solo afecta a la aplicación *web.config* archivo en el servidor. Una implementación posterior de la aplicación podría sobrescribir la configuración en el servidor si la copia del servidor de *web.config* se sustituye por el proyecto *web.config* archivo. Use **cualquier** de los métodos siguientes para administrar la configuración:

  * Use el Administrador de IIS para restablecer la configuración en el *web.config* archivo después de que el archivo se sobrescribe en la implementación.
  * Agregar un *archivo web.config* a la aplicación localmente con la configuración.

::: moniker range=">= aspnetcore-3.0"

## <a name="kestrel"></a>Kestrel

 El [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) se puede usar el paquete NuGet con [Kestrel](xref:fundamentals/servers/kestrel) para admitir la autenticación de Windows mediante Negotiate, Kerberos y NTLM en Windows, Linux y macOS.

> [!WARNING]
> Las credenciales pueden conservarse entre las solicitudes en una conexión. *Negociar la autenticación no debe usarse con servidores proxy a menos que el proxy mantiene una afinidad de conexión de 1:1 (una conexión persistente) con Kestrel.*

> [!NOTE]
> El controlador Negotiate detecta si el servidor subyacente admite autenticación de Windows de forma nativa y si está habilitado. Si el servidor admite la autenticación de Windows, pero está deshabilitado, se produce un error que le pide que habilite la implementación del servidor. Cuando se habilita la autenticación de Windows en el servidor, el controlador Negotiate reenvía de manera transparente a él.

 Agregar servicios de autenticación mediante la invocación <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (`Microsoft.AspNetCore.Authentication.Negotiate` espacio de nombres) y `AddNegotitate` (`Microsoft.AspNetCore.Authentication.Negotiate` espacio de nombres) en `Startup.ConfigureServices`:

 ```csharp
services.AddAuthentication(NegotiateDefaults.AuthenticationScheme)
    .AddNegotiate();
```

Agregue el Middleware de autenticación mediante una llamada a <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*> en `Startup.Configure`:

 ```csharp
app.UseAuthentication();

app.UseMvc();
```

Para obtener más información sobre el software intermedio, consulte <xref:fundamentals/middleware/index>.

Se permiten las solicitudes anónimas. Use [autorización de ASP.NET Core](xref:security/authorization/introduction) a las solicitudes anónimas para la autenticación de desafío.

### <a name="windows-environment-configuration"></a>Configuración del entorno de Windows

El [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) componente realiza la autenticación de modo de usuario. Nombres de entidad de servicio (SPN) debe agregarse a la cuenta de usuario que ejecuta el servicio, no la cuenta de equipo. Ejecutar `setspn -S HTTP/mysrevername.mydomain.com myuser` en un shell de comandos administrativas.

### <a name="linux-and-macos-environment-configuration"></a>Configuración del entorno de Linux y macOS

Las instrucciones para unir una máquina Linux o macOS a un dominio de Windows están disponibles en el [conectar Studio datos de Azure a SQL Server mediante la autenticación de Windows - Kerberos](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller) artículo. Las instrucciones de creación una cuenta de equipo para la máquina de Linux en el dominio. Los SPN deben agregarse a esa cuenta de equipo.

> [!NOTE]
> Al seguir las instrucciones de la [conectar Studio datos de Azure a SQL Server mediante la autenticación de Windows - Kerberos](/sql/azure-data-studio/enable-kerberos?view=sql-server-2017#join-your-os-to-the-active-directory-domain-controller) artículo, reemplace `python-software-properties` con `python3-software-properties` si es necesario.

Una vez que la máquina Linux o macOS está unida al dominio, se requieren pasos adicionales para proporcionar un [archivo keytab](https://blogs.technet.microsoft.com/pie/2018/01/03/all-you-need-to-know-about-keytab-files/) con los SPN:

* En el controlador de dominio, agregue SPN del servicio web nuevo a la cuenta de equipo:
  * `setspn -S HTTP/mywebservice.mydomain.com mymachine`
  * `setspn -S HTTP/mywebservice@MYDOMAIN.COM mymachine`
* Use [ktpass](/windows-server/administration/windows-commands/ktpass) para generar un archivo keytab:
  * `ktpass -princ HTTP/mywebservice.mydomain.com@MYDOMAIN.COM -pass myKeyTabFilePassword -mapuser MYDOMAIN\mymachine$ -pType KRB5_NT_PRINCIPAL -out c:\temp\mymachine.HTTP.keytab -crypto AES256-SHA1`
  * Algunos campos deben especificarse en mayúsculas como se indica.
* Copie el archivo keytab en el equipo Linux o macOS.
* Seleccione el archivo keytab a través de una variable de entorno: `export KRB5_KTNAME=/tmp/mymachine.HTTP.keytab`
* Invocar `klist` para mostrar los SPN disponibles actualmente para su uso.

> [!NOTE]
> Un archivo keytab contiene las credenciales de acceso de dominio y debe protegerse en consecuencia.

::: moniker-end

## <a name="httpsys"></a>HTTP.sys

[HTTP.sys](xref:fundamentals/servers/httpsys) admite la autenticación de Windows de modo Kernel con Negotiate, NTLM o autenticación básica.

Agregar servicios de autenticación mediante la invocación <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> (<xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> espacio de nombres) en `Startup.ConfigureServices`:

```csharp
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

Configurar el host de la aplicación web para usar HTTP.sys con autenticación de Windows (*Program.cs*). <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderHttpSysExtensions.UseHttpSys*> en el <xref:Microsoft.AspNetCore.Server.HttpSys?displayProperty=fullName> espacio de nombres.

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_GenericHost.cs?highlight=13-19)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](windowsauth/sample_snapshot/Program_WebHost.cs?highlight=9-15)]

::: moniker-end

> [!NOTE]
> HTTP.sys delega en la autenticación de modo kernel con el protocolo de autenticación de Kerberos. La autenticación de modo usuario no se admite con Kerberos y HTTP.sys. Se debe usar la cuenta de equipo para descifrar el token o el vale de Kerberos que se obtiene de Active Directory y que el cliente reenvía al servidor para autenticar al usuario. Registre el nombre de entidad de seguridad de servicio (SPN) para el host, no el usuario de la aplicación.

> [!NOTE]
> HTTP.sys no se admite en Nano Server versión 1709 o posterior. Para usar autenticación de Windows y HTTP.sys con Nano Server, use un [contenedor de Server Core (microsoft/windowsservercore)](https://hub.docker.com/r/microsoft/windowsservercore/). Para obtener más información sobre Server Core, vea [¿qué es la opción de instalación Server Core en Windows Server?](/windows-server/administration/server-core/what-is-server-core).

## <a name="authorize-users"></a>Autorizar a los usuarios

Estado de configuración del acceso anónimo determina la manera en que el `[Authorize]` y `[AllowAnonymous]` atributos se utilizan en la aplicación. Las dos secciones siguientes explican cómo controlar los Estados de configuración de permitidos y no permitidos del acceso anónimo.

### <a name="disallow-anonymous-access"></a>No permitir el acceso anónimo

Cuando está habilitada la autenticación de Windows y acceso anónimo está deshabilitado, el `[Authorize]` y `[AllowAnonymous]` atributos no tienen ningún efecto. Si un sitio de IIS está configurado para no permitir el acceso anónimo, la solicitud nunca llega a la aplicación. Por este motivo, la `[AllowAnonymous]` atributo no es aplicable.

### <a name="allow-anonymous-access"></a>Permitir el acceso anónimo

Cuando se habilitan tanto la autenticación de Windows como el acceso anónimo, utilice el `[Authorize]` y `[AllowAnonymous]` atributos. El `[Authorize]` atributo permite proteger los puntos de conexión de la aplicación que requieren autenticación. El `[AllowAnonymous]` atributo invalida la `[Authorize]` atributo en las aplicaciones que permiten el acceso anónimo. Para obtener detalles de uso de atributo, vea <xref:security/authorization/simple>.

> [!NOTE]
> De forma predeterminada, se presentan a los usuarios que no tienen autorización para acceder a una página con una respuesta HTTP 403 vacía. El [StatusCodePages Middleware](xref:fundamentals/error-handling#usestatuscodepages) puede configurarse para proporcionar a los usuarios una mejor experiencia de "Acceso denegado".

## <a name="impersonation"></a>Suplantación

ASP.NET Core no implementa la suplantación. Las aplicaciones se ejecutan con la identidad de la aplicación para todas las solicitudes, utilizando la identidad de proceso o grupo de servidores de aplicación. Si la aplicación debe realizar una acción en nombre de un usuario, use [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) en un [software intermedio alineado terminal](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) en `Startup.Configure`. Ejecutar una sola acción en este contexto y, a continuación, cierre el contexto.

[!code-csharp[](windowsauth/sample_snapshot/Startup.cs?highlight=10-19)]

`RunImpersonated` no es compatible con operaciones asincrónicas y no puede utilizarse para escenarios complejos. Por ejemplo, el ajuste de las solicitudes de todas o las cadenas de middleware no es compatible ni recomendable.

::: moniker range=">= aspnetcore-3.0"

Mientras el [Microsoft.AspNetCore.Authentication.Negotiate](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Negotiate) paquete habilita la autenticación de Windows, Linux y macOS, suplantación solo se admite en Windows.

::: moniker-end

## <a name="claims-transformations"></a>Transformaciones de notificaciones

::: moniker range=">= aspnetcore-3.0"

Al hospedarse con IIS, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> no se llama internamente para inicializar un usuario. Por tanto, se usa una implementación de <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> para transformar las notificaciones después de que cada autenticación no se active de forma predeterminada. Para obtener más información y un ejemplo de código que activa las transformaciones de notificaciones, consulte <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Al hospedar con el modo en proceso IIS, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> no se llama internamente para inicializar un usuario. Por tanto, se usa una implementación de <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> para transformar las notificaciones después de que cada autenticación no se active de forma predeterminada. Para obtener más información y un ejemplo de código que activa las transformaciones de notificaciones cuando se hospedan en proceso, consulte <xref:host-and-deploy/aspnet-core-module#in-process-hosting-model>.

::: moniker-end

## <a name="additional-resources"></a>Recursos adicionales

* [dotnet publish](/dotnet/core/tools/dotnet-publish)
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/visual-studio-publish-profiles>
