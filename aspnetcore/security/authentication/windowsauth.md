---
title: "Configurar la autenticación de Windows en ASP.NET Core"
author: ardalis
description: "Cómo configurar la autenticación de Windows en ASP.NET Core"
keywords: "Núcleo de ASP.NET,"
ms.author: riande
manager: wpickett
ms.date: 7/5/2017
ms.topic: article
ms.assetid: cf119f21-1a2b-49a2-b052-548ccb66ee83
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/windowsauth
ms.openlocfilehash: 008a647295334e957c33c6db7f80687645b3b928
ms.sourcegitcommit: 69b3255f8b6f5db9e7d21f391420602d7ba9f4db
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2017
---
# <a name="configure-windows-authentication-in-aspnet-core"></a>Configurar la autenticación de Windows en ASP.NET Core

Por [Steve Smith](https://ardalis.com)

Autenticación de Windows puede configurarse para las aplicaciones ASP.NET Core hospedadas por IIS o WebListener.

## <a name="what-is-windows-authentication"></a>¿Qué es la autenticación de Windows

Autenticación de Windows se basa en el sistema operativo para autenticar a los usuarios de las aplicaciones de ASP.NET Core. Puede utilizar la autenticación de Windows cuando el servidor se ejecuta en una red corporativa utilizando las identidades del dominio de Active Directory o a otras cuentas de Windows para identificar a los usuarios. Autenticación de Windows es una forma segura de autenticación recomendada adecuada para entornos de intranet donde los usuarios, las aplicaciones cliente y los servidores web pertenecen al mismo dominio de Windows.

[Más información acerca de la autenticación de Windows e instalar para IIS](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).

## <a name="enabling-windows-authentication-in-an-aspnet-core-application"></a>Habilitar la autenticación de Windows en una aplicación de ASP.NET Core

La plantilla de aplicación Web de Visual Studio puede configurarse para admitir la autenticación de Windows.

### <a name="using-the-windows-authentication-app-template"></a>Con la plantilla de aplicación de autenticación de Windows

En Visual Studio:
* Cree una nueva aplicación Web de ASP.NET Core. 
* En la lista de plantillas, seleccione aplicación Web.
* Seleccione el botón de cambio de la autenticación y seleccione **autenticación de Windows**. 

Ejecutar la aplicación. El nombre de usuario aparece en la parte superior derecha de la aplicación.

![Captura de pantalla del explorador de la autenticación de Windows](windowsauth/_static/browser-screenshot.png)

Para el trabajo de desarrollo con IIS Express, la plantilla proporciona la configuración necesaria para utilizar la autenticación de Windows. La sección siguiente muestra cómo configurar una aplicación ASP.NET Core manualmente para la autenticación de Windows.

### <a name="visual-studio-settings-for-windows-and-anonymous-authentication"></a>Configuración de Visual Studio para Windows y la autenticación anónima

La página de propiedades de Visual Studio, pestaña depurar proporciona casillas de verificación para la autenticación de Windows y la autenticación anónima.

![Captura de pantalla del explorador de la autenticación de Windows](windowsauth/_static/vs-auth-property-menu.png)

También puede configurar estas propiedades en el `launchSettings.json` archivo:

```json
{
  "iisSettings": {
    "windowsAuthentication": true,
    "anonymousAuthentication": false,
    "iisExpress": {
      "applicationUrl": "http://localhost:52171/",
      "sslPort": 0
    }
  } // additional options trimmed
}
```

## <a name="enabling-windows-authentication-with-iis"></a>Habilitar la autenticación de Windows con IIS

IIS usa el [módulo principal de ASP.NET](xref:fundamentals/servers/aspnet-core-module) (ANCM) para hospedar aplicaciones ASP.NET Core. La ANCM flujos autenticación de Windows en IIS de forma predeterminada. Configuración de autenticación de Windows se realiza dentro de IIS, no en el proyecto de aplicación. Las secciones siguientes muestran cómo usar el Administrador de IIS para configurar una aplicación ASP.NET Core para usar la autenticación de Windows:

### <a name="create-a-new-iis-site"></a>Crear un nuevo sitio IIS

Especifique un nombre y una carpeta y permitir que se cree un nuevo grupo de aplicaciones.

### <a name="customize-authentication"></a>Personalizar la autenticación

Abra el menú de autenticación para el sitio.

![Menú de autenticación de IIS](windowsauth/_static/iis-authentication-menu.png)

Deshabilite la autenticación anónima y habilitar la autenticación de Windows.

![Configuración de autenticación IIS](windowsauth/_static/iis-auth-settings.png)

### <a name="publish-your-project-to-the-iis-site-folder"></a>Publicar el proyecto en la carpeta del sitio IIS

Con Visual Studio o la CLI de núcleo de .NET *publicar* la aplicación a la carpeta de destino.

![Cuadro de diálogo de publicación de Visual Studio](windowsauth/_static/vs-publish-app.png)

Obtenga más información sobre [publicar en IIS](https://docs.microsoft.com/aspnet/core/publishing/iis).

Inicie la aplicación para comprobar que funciona la autenticación de Windows.

## <a name="enabling-windows-authentication-with-weblistener"></a>Habilitar la autenticación de Windows con WebListener

Aunque Kestrel no admite la autenticación de Windows, puede usar [WebListener](xref:fundamentals/servers/weblistener) para admitir escenarios hospedados por sí mismo en Windows. En el ejemplo siguiente se configura el host de la aplicación web para usar WebListener con la autenticación de Windows:

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var host = new WebHostBuilder()
            .UseWebListener(options =>
            {
                options.ListenerSettings.Authentication.Schemes = 
                    AuthenticationSchemes.Negotiate | AuthenticationSchemes.NTLM;
                options.ListenerSettings.Authentication.AllowAnonymous = false;
            })
            .UseContentRoot(Directory.GetCurrentDirectory())
            .UseStartup<Startup>()
            .Build();

        host.Run();
    }
}
```

## <a name="working-with-windows-authentication"></a>Trabajar con autenticación de Windows

Si la aplicación utiliza la autenticación de Windows y el acceso anónimo, puede usar el ``[Authorize]`` y ``[AllowAnonymous]`` atributos. Aplicaciones que no tienen anónimo realice habilitado no requieren ``[Authorize]``; de la aplicación se considerará que requiera autenticación, se rechazan las solicitudes anónimas. Tenga en cuenta que si se configura el sitio de IIS **no** para permitir el acceso anónimo, el ``[AllowAnonymous]`` atributo hace **no** permitir solicitudes anónimas. El ``[AllowAnonymous]`` atributo invalidaciones ``[Authorize]`` atributo uso dentro de las aplicaciones que permiten el acceso anónimo.

### <a name="impersonation"></a>Suplantación

ASP.NET Core no lleva a cabo la suplantación. Las aplicaciones se ejecutan con la identidad de aplicación para todas las solicitudes, uso de la identidad de proceso o grupo de servidores de aplicación. Si necesita realizar una acción en nombre del usuario de forma explícita, use ``WindowsIdentity.RunImpersonated``. Ejecutar una sola acción en este contexto y, a continuación, cierre el contexto. Tenga en cuenta que ``RunImpersonated`` no es compatible con async y no debe usarse para escenarios complejos. Por ejemplo, las solicitudes de todas o cadenas de middleware de ajuste no es admiten o recomendado.
