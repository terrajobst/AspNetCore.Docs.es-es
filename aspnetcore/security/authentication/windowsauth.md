---
title: Configurar la autenticación de Windows en ASP.NET Core
author: scottaddie
description: Obtenga información sobre cómo configurar la autenticación de Windows en ASP.NET Core, usar IIS Express, IIS y HTTP.sys.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 12/23/2018
uid: security/authentication/windowsauth
ms.openlocfilehash: 64178c8fce71445fc6a728a236d811484b21e3e0
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2019
ms.locfileid: "54099265"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a>Configurar la autenticación de Windows en ASP.NET Core

Por [Scott Addie](https://twitter.com/Scott_Addie) y [Luke Latham](https://github.com/guardrex)

[Autenticación de Windows](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/) puede configurarse para las aplicaciones de ASP.NET Core hospedadas con [IIS](xref:host-and-deploy/iis/index) o [HTTP.sys](xref:fundamentals/servers/httpsys).

Autenticación de Windows se basa en el sistema operativo para autenticar usuarios de aplicaciones de ASP.NET Core. Puede usar la autenticación de Windows cuando el servidor se ejecuta en una red corporativa mediante identidades de dominio de Active Directory o cuentas de Windows para identificar a los usuarios. Autenticación de Windows se adapta mejor a los entornos de intranet donde los usuarios, las aplicaciones cliente y servidores web pertenecen al mismo dominio de Windows.

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a>Habilitar la autenticación de Windows en una aplicación ASP.NET Core

El **aplicación Web** plantilla disponible a través de Visual Studio o la CLI de .NET Core puede configurarse para admitir la autenticación de Windows.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

### <a name="use-the-windows-authentication-app-template-for-a-new-project"></a>Usa la plantilla de aplicación de autenticación de Windows para un nuevo proyecto

En Visual Studio:

1. Cree un nuevo **aplicación Web ASP.NET Core**.
1. Seleccione **aplicación Web** en la lista de plantillas.
1. Seleccione el **Cambiar autenticación** y seleccione **Windows autenticación**.

Ejecute la aplicación. El nombre de usuario aparece en la interfaz de usuario de la aplicación representada.

### <a name="manual-configuration-for-an-existing-project"></a>Configuración manual de un proyecto existente

Las propiedades del proyecto le permiten habilitar la autenticación de Windows y deshabilitar la autenticación anónima:

1. Haga clic en el proyecto en Visual Studio **el Explorador de soluciones** y seleccione **propiedades**.
1. Seleccione la pestaña **Depurar**.
1. Desactive la casilla de verificación **habilitar la autenticación anónima**.
1. Seleccione la casilla de verificación **habilitar la autenticación de Windows**.

Como alternativa, se pueden configurar las propiedades en el `iisSettings` nodo de la *launchSettings.json* archivo:

[!code-json[](windowsauth/sample_snapshot/launchSettings.json?highlight=2-3)]

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

Use la **Windows autenticación** plantilla de aplicación.

Ejecute el [dotnet nuevo](/dotnet/core/tools/dotnet-new) comando con el `webapp` argumento (aplicación Web de ASP.NET Core) y `--auth Windows` cambiar:

```console
dotnet new webapp --auth Windows
```

---

## <a name="enable-windows-authentication-with-iis"></a>Habilitar la autenticación de Windows con IIS

IIS usa el [módulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module) para hospedar aplicaciones ASP.NET Core. Autenticación de Windows se configura para IIS a través de la *web.config* archivo. Las siguientes secciones muestran cómo:

* Proporcionar una variable local *web.config* archivo que activa la autenticación de Windows en el servidor cuando se implementa la aplicación.
* Use el Administrador de IIS para configurar el *web.config* archivos de una aplicación de ASP.NET Core que ya se ha implementado en el servidor.

### <a name="iis-configuration"></a>Configuración de IIS

Si aún no lo ha hecho, habilite IIS para hospedar aplicaciones ASP.NET Core. Para obtener más información, consulta <xref:host-and-deploy/iis/index>.

Habilite el servicio de rol IIS para la autenticación de Windows. Para obtener más información, consulte [Habilitar autenticación de Windows en servicios de rol de IIS (consulte el paso 2)](xref:host-and-deploy/iis/index#iis-configuration).

Middleware de integración de IIS está configurado para autenticar automáticamente las solicitudes de forma predeterminada. Para obtener más información, consulte [Host ASP.NET Core en Windows con IIS: Las opciones de IIS (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).

El módulo ASP.NET Core está configurado para reenviar el token de autenticación de Windows para la aplicación de forma predeterminada. Para obtener más información, consulte [referencia de configuración del módulo ASP.NET Core: Atributos del elemento aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

### <a name="create-a-new-iis-site"></a>Crear un nuevo sitio IIS

Especifique un nombre y una carpeta y permitir que se cree un nuevo grupo de aplicaciones.

### <a name="enable-windows-authentication-for-the-app-in-iis"></a>Habilitar la autenticación de Windows para la aplicación en IIS

Use **cualquier** de los métodos siguientes:

* [Configuración de desarrollo antes de publicar la aplicación](#development-side-configuration-with-a-local-webconfig-file) (*recomendado*)
* [Configuración del servidor después de publicar la aplicación](#server-side-configuration-with-the-iis-manager)

#### <a name="development-side-configuration-with-a-local-webconfig-file"></a>Configuración de desarrollo con un archivo local web.config

Realice los pasos siguientes **antes** [publicar e implementar el proyecto](#publish-and-deploy-your-project-to-the-iis-site-folder).

Agregue el siguiente *web.config* archivo a la raíz del proyecto:

[!code-xml[](windowsauth/sample_snapshot/web_2.config)]

Cuando se publica el proyecto mediante el SDK (sin el `<IsTransformWebConfigDisabled>` propiedad establecida en `true` en el archivo de proyecto), publicado *web.config* archivo incluye la `<location><system.webServer><security><authentication>` sección. Para obtener más información sobre la `<IsTransformWebConfigDisabled>` propiedad, vea <xref:host-and-deploy/iis/index#webconfig-file>.

#### <a name="server-side-configuration-with-the-iis-manager"></a>Configuración del servidor con el Administrador de IIS

Realice los pasos siguientes **después** [publicar e implementar el proyecto](#publish-and-deploy-your-project-to-the-iis-site-folder).

1. En el Administrador de IIS, seleccione el sitio IIS en el **sitios** nodo de la **conexiones** barra lateral.
1. Haga doble clic en **autenticación** en el **IIS** área.
1. Seleccione **autenticación anónima**. Seleccione **deshabilitar** en el **acciones** barra lateral.
1. Seleccione **Windows autenticación**. Seleccione **habilitar** en el **acciones** barra lateral.

Cuando se realizan estas acciones, el Administrador de IIS modifica la aplicación *web.config* archivo. Un `<system.webServer><security><authentication>` se agrega el nodo con la configuración actualizada para `anonymousAuthentication` y `windowsAuthentication`:

[!code-xml[](windowsauth/sample_snapshot/web_1.config?highlight=4-5)]

El `<system.webServer>` sección agregada a la *web.config* archivo por el Administrador de IIS está fuera de la aplicación `<location>` sección agregada por el SDK de .NET Core cuando se publique la aplicación. Dado que se agrega la sección fuera de la `<location>` nodo, la configuración se hereda por cualquier [las aplicaciones secundarias](xref:host-and-deploy/iis/index#sub-applications) a la aplicación actual. Para impedir la herencia, mover agregado `<security>` sección dentro de la `<location><system.webServer>` sección suministradas por el SDK.

Cuando se usa el Administrador de IIS para agregar la configuración de IIS, solo afecta a la aplicación *web.config* archivo en el servidor. Una implementación posterior de la aplicación podría sobrescribir la configuración en el servidor si la copia del servidor de *web.config* se sustituye por el proyecto *web.config* archivo. Use **cualquier** de los métodos siguientes para administrar la configuración:

* Use el Administrador de IIS para restablecer la configuración en el *web.config* archivo después de que el archivo se sobrescribe en la implementación.
* Agregar un *archivo web.config* a la aplicación localmente con la configuración. Para obtener más información, consulte el [configuración del desarrollo](#development-side-configuration-with-a-local-webconfig-file) sección.

### <a name="publish-and-deploy-your-project-to-the-iis-site-folder"></a>Publicar e implementar el proyecto a la carpeta del sitio IIS

Con Visual Studio o la CLI de .NET Core, publicar e implementar la aplicación en la carpeta de destino.

Para obtener más información sobre el hospedaje con IIS, publicación e implementación, vea los temas siguientes:

* [dotnet publish](/dotnet/core/tools/dotnet-publish)
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/visual-studio-publish-profiles>

Inicie la aplicación para comprobar que funciona la autenticación de Windows.

## <a name="enable-windows-authentication-with-httpsys"></a>Habilitar la autenticación de Windows con HTTP.sys

Aunque Kestrel no admite la autenticación de Windows, puede usar [HTTP.sys](xref:fundamentals/servers/httpsys) para admitir los escenarios Auto-hospedados en Windows. El ejemplo siguiente configura el host de la aplicación web para usar HTTP.sys con autenticación de Windows:

[!code-csharp[](windowsauth/sample_snapshot/Program.cs?highlight=9-14)]

> [!NOTE]
> HTTP.sys delega en la autenticación de modo kernel con el protocolo de autenticación de Kerberos. La autenticación de modo usuario no se admite con Kerberos y HTTP.sys. Se debe usar la cuenta de equipo para descifrar el token o el vale de Kerberos que se obtiene de Active Directory y que el cliente reenvía al servidor para autenticar al usuario. Registre el nombre de entidad de seguridad de servicio (SPN) para el host, no el usuario de la aplicación.

> [!NOTE]
> HTTP.sys no se admite en Nano Server versión 1709 o posterior. Para usar autenticación de Windows y HTTP.sys con Nano Server, use un [contenedor de Server Core (microsoft/windowsservercore)](https://hub.docker.com/r/microsoft/windowsservercore/). Para obtener más información sobre Server Core, vea [¿qué es la opción de instalación Server Core en Windows Server?](/windows-server/administration/server-core/what-is-server-core).

## <a name="work-with-windows-authentication"></a>Trabajar con la autenticación de Windows

Estado de configuración del acceso anónimo determina la manera en que el `[Authorize]` y `[AllowAnonymous]` atributos se utilizan en la aplicación. Las dos secciones siguientes explican cómo controlar los Estados de configuración de permitidos y no permitidos del acceso anónimo.

### <a name="disallow-anonymous-access"></a>No permitir el acceso anónimo

Cuando está habilitada la autenticación de Windows y acceso anónimo está deshabilitado, el `[Authorize]` y `[AllowAnonymous]` atributos no tienen ningún efecto. Si el sitio IIS (o HTTP.sys) está configurado para no permitir el acceso anónimo, la solicitud nunca llega a la aplicación. Por este motivo, la `[AllowAnonymous]` atributo no es aplicable.

### <a name="allow-anonymous-access"></a>Permitir el acceso anónimo

Cuando se habilitan tanto la autenticación de Windows como el acceso anónimo, utilice el `[Authorize]` y `[AllowAnonymous]` atributos. El `[Authorize]` atributo permite proteger partes de la aplicación que realmente requiera autenticación de Windows. El `[AllowAnonymous]` invalidaciones de atributo `[Authorize]` atributo uso dentro de las aplicaciones que permite el acceso anónimo. Consulte [autorización sencilla](xref:security/authorization/simple) para obtener detalles de uso del atributo.

En ASP.NET Core 2.x, el `[Authorize]` atributo requiere configuración adicional en *Startup.cs* Desafíe a las solicitudes anónimas para la autenticación de Windows. La configuración recomendada varía ligeramente según el servidor web que se va a usar.

> [!NOTE]
> De forma predeterminada, se presentan a los usuarios que no tienen autorización para acceder a una página con una respuesta HTTP 403 vacía. El [StatusCodePages middleware](xref:fundamentals/error-handling#configure-status-code-pages) puede configurarse para proporcionar a los usuarios una mejor experiencia de "Acceso denegado".

#### <a name="iis"></a>IIS

Si usa IIS, agregue lo siguiente a la `ConfigureServices` método:

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a>HTTP.sys

Si usa HTTP.sys, agregue lo siguiente a la `ConfigureServices` método:

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a>Suplantación

ASP.NET Core no implementa la suplantación. Las aplicaciones se ejecutan con la identidad de la aplicación para todas las solicitudes, utilizando la identidad de proceso o grupo de servidores de aplicación. Si necesita realizar explícitamente una acción en nombre de un usuario, use [WindowsIdentity.RunImpersonated](xref:System.Security.Principal.WindowsIdentity.RunImpersonated*) en un [software intermedio alineado terminal](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) en `Startup.Configure`. Ejecutar una sola acción en este contexto y, a continuación, cierre el contexto.

[!code-csharp[](windowsauth/sample_snapshot/Startup.cs?highlight=10-19)]

`RunImpersonated` no es compatible con operaciones asincrónicas y no puede utilizarse para escenarios complejos. Por ejemplo, el ajuste de las solicitudes de todas o las cadenas de middleware no es compatible ni recomendable.
