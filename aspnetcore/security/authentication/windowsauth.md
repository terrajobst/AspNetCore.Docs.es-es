---
title: Configurar la autenticación de Windows en ASP.NET Core
author: ardalis
description: En este artículo se describe cómo configurar la autenticación de Windows en ASP.NET Core, usar IIS Express, IIS, HTTP.sys y WebListener.
ms.author: riande
ms.date: 08/18/2018
uid: security/authentication/windowsauth
ms.openlocfilehash: 93b1a1de74ef6554d48709b04870f7e23738846b
ms.sourcegitcommit: 15d7bd0b2c4e6fe9ac335d658bab71a45ca5bc72
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2018
ms.locfileid: "41828626"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a>Configurar la autenticación de Windows en ASP.NET Core

Por [Steve Smith](https://ardalis.com) y [Scott Addie](https://twitter.com/Scott_Addie)

Se puede configurar la autenticación de Windows para las aplicaciones de ASP.NET Core hospedadas en IIS, [HTTP.sys](xref:fundamentals/servers/httpsys), o [WebListener](xref:fundamentals/servers/weblistener).

## <a name="windows-authentication"></a>Autenticación de Windows

Autenticación de Windows se basa en el sistema operativo para autenticar usuarios de aplicaciones de ASP.NET Core. Puede usar la autenticación de Windows cuando el servidor se ejecuta en una red corporativa mediante identidades de dominio de Active Directory o a otras cuentas de Windows para identificar a los usuarios. Autenticación de Windows se adapta mejor a los entornos de intranet en el que los usuarios, las aplicaciones cliente y servidores web pertenecen al mismo dominio de Windows.

[Más información acerca de la autenticación de Windows e instalarla para IIS](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a>Habilitar la autenticación de Windows en una aplicación ASP.NET Core

La plantilla de aplicación Web de Visual Studio puede configurarse para admitir la autenticación de Windows.

### <a name="use-the-windows-authentication-app-template"></a>Usa la plantilla de aplicación de autenticación de Windows

En Visual Studio:

1. Cree una aplicación web de ASP.NET Core.
1. Seleccione la aplicación Web en la lista de plantillas.
1. Seleccione el **Cambiar autenticación** y seleccione **Windows autenticación**.

Ejecute la aplicación. El nombre de usuario aparece en la parte superior derecha de la aplicación.

![Captura de pantalla del explorador de la autenticación de Windows](windowsauth/_static/browser-screenshot.png)

Para el trabajo de desarrollo con IIS Express, la plantilla proporciona toda la configuración necesaria para utilizar la autenticación de Windows. En la sección siguiente se muestra cómo configurar manualmente una aplicación ASP.NET Core para la autenticación de Windows.

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a>Configuración de Visual Studio para Windows y la autenticación anónima

El proyecto de Visual Studio **propiedades** la página **depurar** ficha proporciona casillas de verificación para la autenticación de Windows y la autenticación anónima.

![Captura de pantalla del explorador de la autenticación de Windows](windowsauth/_static/vs-auth-property-menu.png)

Como alternativa, se pueden configurar estas dos propiedades en el *launchSettings.json* archivo:

[!code-json[](windowsauth/sample/launchSettings.json?highlight=3-4)]

## <a name="enable-windows-authentication-with-iis"></a>Habilitar la autenticación de Windows con IIS

IIS usa el [módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) para hospedar aplicaciones ASP.NET Core. Autenticación de Windows se configura en IIS, no la aplicación. Las secciones siguientes muestran cómo usar el Administrador de IIS para configurar una aplicación ASP.NET Core para usar la autenticación de Windows.

### <a name="iis-configuration"></a>Configuración de IIS

Habilite el servicio de rol IIS para la autenticación de Windows. Para obtener más información, consulte [Habilitar autenticación de Windows en servicios de rol de IIS (consulte el paso 2)](xref:host-and-deploy/iis/index#iis-configuration).

Middleware de integración de IIS está configurado para autenticar automáticamente las solicitudes de forma predeterminada. Para obtener más información, consulte [Host ASP.NET Core en Windows con IIS: las opciones de IIS (AutomaticAuthentication)](xref:host-and-deploy/iis/index#iis-options).

El módulo ASP.NET Core está configurado para reenviar el token de autenticación de Windows para la aplicación de forma predeterminada. Para obtener más información, consulte [referencia de configuración del módulo ASP.NET Core: los atributos del elemento aspNetCore](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).

### <a name="create-a-new-iis-site"></a>Crear un nuevo sitio IIS

Especifique un nombre y una carpeta y permitir que se cree un nuevo grupo de aplicaciones.

### <a name="customize-authentication"></a>Personalizar la autenticación

Abra las características de autenticación para el sitio.

![Menú de la autenticación de IIS](windowsauth/_static/iis-authentication-menu.png)

Deshabilitar la autenticación anónima y habilitar la autenticación de Windows.

![Configuración de autenticación IIS](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a>Publique el proyecto en la carpeta del sitio IIS

Con Visual Studio o la CLI de .NET Core, publicar la aplicación en la carpeta de destino.

![Cuadro de diálogo de publicación de Visual Studio](windowsauth/_static/vs-publish-app.png)

Obtenga más información sobre [publicar en IIS](xref:host-and-deploy/iis/index).

Inicie la aplicación para comprobar que funciona la autenticación de Windows.

::: moniker range=">= aspnetcore-2.0"

## <a name="enable-windows-authentication-with-httpsys"></a>Habilitar la autenticación de Windows con HTTP.sys

Aunque Kestrel no admite la autenticación de Windows, puede usar [HTTP.sys](xref:fundamentals/servers/httpsys) para admitir los escenarios Auto-hospedados en Windows. El ejemplo siguiente configura el host de la aplicación web para usar HTTP.sys con autenticación de Windows:

[!code-csharp[](windowsauth/sample/Program2x.cs?highlight=9-14)]

> [!NOTE]
> Delegados de HTTP.sys para la autenticación de modo kernel con el protocolo de autenticación Kerberos. No se admite la autenticación de modo usuario con Kerberos y HTTP.sys. La cuenta de equipo debe tener usada para descifrar el token/vale de Kerberos que se obtienen de Active Directory y reenviada por el cliente en el servidor para autenticar al usuario. Registrar el nombre Principal de servicio (SPN) para el host, no el usuario de la aplicación.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

## <a name="enable-windows-authentication-with-weblistener"></a>Habilitar la autenticación de Windows con WebListener

Aunque Kestrel no admite la autenticación de Windows, puede usar [WebListener](xref:fundamentals/servers/weblistener) para admitir los escenarios Auto-hospedados en Windows. El ejemplo siguiente configura el host de la aplicación web para el uso de WebListener con autenticación de Windows:

[!code-csharp[](windowsauth/sample/Program1x.cs?highlight=6-11)]

> [!NOTE]
> Delegados de WebListener para la autenticación de modo kernel con el protocolo de autenticación Kerberos. No se admite la autenticación de modo usuario con Kerberos y WebListener. La cuenta de equipo debe tener usada para descifrar el token/vale de Kerberos que se obtienen de Active Directory y reenviada por el cliente en el servidor para autenticar al usuario. Registrar el nombre Principal de servicio (SPN) para el host, no el usuario de la aplicación.

::: moniker-end

## <a name="work-with-windows-authentication"></a>Trabajar con la autenticación de Windows

Estado de configuración del acceso anónimo determina la manera en que el `[Authorize]` y `[AllowAnonymous]` atributos se utilizan en la aplicación. Las dos secciones siguientes explican cómo controlar los Estados de configuración de permitidos y no permitidos del acceso anónimo.

### <a name="disallow-anonymous-access"></a>No permitir el acceso anónimo

Cuando está habilitada la autenticación de Windows y acceso anónimo está deshabilitado, el `[Authorize]` y `[AllowAnonymous]` atributos no tienen ningún efecto. Si el sitio IIS (o servidor HTTP.sys o WebListener) está configurado para no permitir el acceso anónimo, la solicitud nunca llega a la aplicación. Por este motivo, la `[AllowAnonymous]` atributo no es aplicable.

### <a name="allow-anonymous-access"></a>Permitir el acceso anónimo

Cuando se habilitan tanto la autenticación de Windows como el acceso anónimo, utilice el `[Authorize]` y `[AllowAnonymous]` atributos. El `[Authorize]` atributo permite proteger partes de la aplicación que realmente requiera autenticación de Windows. El `[AllowAnonymous]` invalidaciones de atributo `[Authorize]` atributo uso dentro de las aplicaciones que permite el acceso anónimo. Consulte [autorización sencilla](xref:security/authorization/simple) para obtener detalles de uso del atributo.

En ASP.NET Core 2.x, el `[Authorize]` atributo requiere configuración adicional en *Startup.cs* Desafíe a las solicitudes anónimas para la autenticación de Windows. La configuración recomendada varía ligeramente según el servidor web que se va a usar.

> [!NOTE]
> De forma predeterminada, se presentan a los usuarios que no tienen autorización para acceder a una página con una respuesta HTTP 403 vacía. El [StatusCodePages middleware](xref:fundamentals/error-handling#configuring-status-code-pages) puede configurarse para proporcionar a los usuarios una mejor experiencia de "Acceso denegado".

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

ASP.NET Core no implementa la suplantación. Las aplicaciones se ejecutan con la identidad de aplicación para todas las solicitudes, utilizando la identidad de proceso o grupo de servidores de aplicación. Si necesita realizar explícitamente una acción en nombre de un usuario, use `WindowsIdentity.RunImpersonated`. Ejecutar una sola acción en este contexto y, a continuación, cierre el contexto.

[!code-csharp[](windowsauth/sample/Startup.cs?name=snippet_Impersonate&highlight=10-18)]

Tenga en cuenta que `RunImpersonated` no es compatible con operaciones asincrónicas y no puede utilizarse para escenarios complejos. Por ejemplo, el ajuste de las solicitudes de todas o las cadenas de middleware no es compatible ni recomendable.
