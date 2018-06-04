---
title: Configurar la autenticación de Windows en ASP.NET Core
author: ardalis
description: En este artículo se describe cómo configurar la autenticación de Windows en ASP.NET Core, mediante IIS Express, IIS, HTTP.sys y WebListener.
manager: wpickett
ms.author: riande
ms.date: 10/24/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/windowsauth
ms.openlocfilehash: dbcef095561fe656bdd28c4fa6560c6b269a2db0
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2018
ms.locfileid: "34689014"
---
# <a name="configure-windows-authentication-in-aspnet-core"></a>Configurar la autenticación de Windows en ASP.NET Core

Por [Steve Smith](https://ardalis.com) y [Scott Addie](https://twitter.com/Scott_Addie)

Se puede configurar la autenticación de Windows para las aplicaciones ASP.NET Core hospedadas en IIS, [HTTP.sys](xref:fundamentals/servers/httpsys), o [WebListener](xref:fundamentals/servers/weblistener).

## <a name="what-is-windows-authentication"></a>¿Qué es la autenticación de Windows?

Autenticación de Windows se basa en el sistema operativo para autenticar a los usuarios de las aplicaciones de ASP.NET Core. Puede utilizar la autenticación de Windows cuando el servidor se ejecuta en una red corporativa utilizando las identidades del dominio de Active Directory o a otras cuentas de Windows para identificar a los usuarios. Autenticación de Windows es más adecuada para entornos de intranet en el que los usuarios, las aplicaciones cliente y los servidores web pertenecen al mismo dominio de Windows.

[Obtener más información sobre la autenticación de Windows e instalarla para IIS](/iis/configuration/system.webServer/security/authentication/windowsAuthentication/).

## <a name="enable-windows-authentication-in-an-aspnet-core-app"></a>Habilitar la autenticación de Windows en una aplicación de ASP.NET Core

La plantilla de aplicación Web de Visual Studio puede configurarse para admitir la autenticación de Windows.

### <a name="use-the-windows-authentication-app-template"></a>Usa la plantilla de aplicación de autenticación de Windows

En Visual Studio:
1. Cree una aplicación web de ASP.NET Core. 
1. En la lista de plantillas, seleccione aplicación Web.
1. Seleccione el **Cambiar autenticación** botón y seleccione **autenticación de Windows**. 

Ejecute la aplicación. El nombre de usuario aparece en la parte superior derecha de la aplicación.

![Captura de pantalla del explorador de la autenticación de Windows](windowsauth/_static/browser-screenshot.png)

Para el trabajo de desarrollo con IIS Express, la plantilla proporciona la configuración necesaria para utilizar la autenticación de Windows. La sección siguiente muestra cómo configurar manualmente una aplicación de ASP.NET Core para la autenticación de Windows.

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a>Configuración de Visual Studio para Windows y la autenticación anónima

El proyecto de Visual Studio **propiedades** la página **depurar** ficha proporciona casillas de verificación para la autenticación de Windows y la autenticación anónima.

![Captura de pantalla del explorador de la autenticación de Windows](windowsauth/_static/vs-auth-property-menu.png)

Como alternativa, estas dos propiedades pueden configurarse en el *launchSettings.json* archivo:

[!code-json[](windowsauth/sample/launchSettings.json?highlight=3-4)]

## <a name="enable-windows-authentication-with-iis"></a>Habilitar la autenticación de Windows con IIS

IIS usa el [módulo principal de ASP.NET](xref:fundamentals/servers/aspnet-core-module) a las aplicaciones ASP.NET Core de host. El módulo permite la autenticación de Windows para que fluya a IIS de forma predeterminada. Se configura la autenticación de Windows en IIS, no la aplicación. Las secciones siguientes muestran cómo usar el Administrador de IIS para configurar una aplicación ASP.NET Core para usar la autenticación de Windows.

### <a name="create-a-new-iis-site"></a>Crear un nuevo sitio IIS

Especifique un nombre y una carpeta y permitir que se cree un nuevo grupo de aplicaciones.

### <a name="customize-authentication"></a>Personalizar la autenticación

Abra el menú de autenticación para el sitio.

![Menú de autenticación de IIS](windowsauth/_static/iis-authentication-menu.png)

Deshabilite la autenticación anónima y habilitar la autenticación de Windows.

![Configuración de autenticación IIS](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a>Publicar el proyecto en la carpeta del sitio IIS

Con Visual Studio o la CLI de núcleo. NET, publique la aplicación en la carpeta de destino.

![Cuadro de diálogo de publicación de Visual Studio](windowsauth/_static/vs-publish-app.png)

Obtenga más información sobre [publicar en IIS](xref:host-and-deploy/iis/index).

Inicie la aplicación para comprobar que funciona la autenticación de Windows.

## <a name="enable-windows-authentication-with-httpsys-or-weblistener"></a>Habilitar la autenticación de Windows con HTTP.sys o WebListener

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

Aunque Kestrel no admite la autenticación de Windows, puede usar [HTTP.sys](xref:fundamentals/servers/httpsys) para admitir escenarios hospedados por sí mismo en Windows. En el ejemplo siguiente se configura el host de la aplicación web para usar HTTP.sys con autenticación de Windows:

[!code-csharp[](windowsauth/sample/Program2x.cs?highlight=9-14)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Aunque Kestrel no admite la autenticación de Windows, puede usar [WebListener](xref:fundamentals/servers/weblistener) para admitir escenarios hospedados por sí mismo en Windows. En el ejemplo siguiente se configura el host de la aplicación web para usar WebListener con la autenticación de Windows:

[!code-csharp[](windowsauth/sample/Program1x.cs?highlight=6-11)]

---

## <a name="work-with-windows-authentication"></a>Trabajar con la autenticación de Windows

El estado de configuración de acceso anónimo determina la manera en que la `[Authorize]` y `[AllowAnonymous]` atributos se utilizan en la aplicación. Las dos secciones siguientes explican cómo controlar los Estados de configuración permitidos y no permitidos de acceso anónimo.

### <a name="disallow-anonymous-access"></a>Denegar el acceso anónimo

Cuando se habilita la autenticación de Windows y acceso anónimo está deshabilitado, el `[Authorize]` y `[AllowAnonymous]` atributos no tienen ningún efecto. Si el sitio IIS (o servidor HTTP.sys o WebListener) está configurado para denegar el acceso anónimo, la solicitud nunca llega a la aplicación. Por este motivo, la `[AllowAnonymous]` atributo no se aplica.

### <a name="allow-anonymous-access"></a>Permitir el acceso anónimo

Cuando se habilitan la autenticación de Windows y el acceso anónimo, use la `[Authorize]` y `[AllowAnonymous]` atributos. El `[Authorize]` atributo permite proteger partes de la aplicación que realmente requieren autenticación de Windows. El `[AllowAnonymous]` atributo invalidaciones `[Authorize]` atributo uso dentro de las aplicaciones que permiten el acceso anónimo. Vea [autorización sencilla](xref:security/authorization/simple) para obtener detalles de uso de atributos.

En ASP.NET Core 2.x, el `[Authorize]` atributo requiere una configuración adicional de *Startup.cs* Desafíe solicitudes anónimas para la autenticación de Windows. La configuración recomendada varía ligeramente según el servidor web que se va a usar.

> [!NOTE]
> De forma predeterminada, los usuarios que no tienen autorización para acceder a una página se le presentará una respuesta HTTP 403 vacía. El [StatusCodePages middleware](xref:fundamentals/error-handling#configuring-status-code-pages) se puede configurar para proporcionar a los usuarios una mejor experiencia de "Acceso denegado".

#### <a name="iis"></a>IIS

Si usa IIS, agregue lo siguiente a la `ConfigureServices` método: 

```csharp
// IISDefaults requires the following import:
// using Microsoft.AspNetCore.Server.IISIntegration;
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

#### <a name="httpsys"></a>HTTP.sys

Si utiliza HTTP.sys, agregue lo siguiente a la `ConfigureServices` método:

```csharp
// HttpSysDefaults requires the following import:
// using Microsoft.AspNetCore.Server.HttpSys;
services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
```

### <a name="impersonation"></a>Suplantación

ASP.NET Core no implementa la suplantación. Las aplicaciones se ejecutan con la identidad de aplicación para todas las solicitudes, uso de la identidad de proceso o grupo de servidores de aplicación. Si necesita realizar una acción en nombre del usuario de forma explícita, use `WindowsIdentity.RunImpersonated`. Ejecutar una sola acción en este contexto y, a continuación, cierre el contexto.

[!code-csharp[](windowsauth/sample/Startup.cs?name=snippet_Impersonate&highlight=10-18)]

Tenga en cuenta que `RunImpersonated` no es compatible con operaciones asincrónicas y no deben usarse para escenarios complejos. Por ejemplo, el ajuste de las solicitudes de todas o cadenas de middleware no es compatible ni recomendable.

---
