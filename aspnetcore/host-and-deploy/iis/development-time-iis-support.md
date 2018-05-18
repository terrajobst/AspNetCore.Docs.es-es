---
title: Compatibilidad de IIS de tiempo de desarrollo en Visual Studio para ASP.NET Core
author: shirhatti
description: Descubrir compatibilidad para depurar aplicaciones de ASP.NET Core cuando se ejecuta detrás de IIS en Windows Server.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/14/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 0bf4585d44e61c5e7e5b89ce9d8dfdfa10d5460e
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/17/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a>Compatibilidad de IIS de tiempo de desarrollo en Visual Studio para ASP.NET Core

Por [Sourabh Shirhatti](https://twitter.com/sshirhatti) y [Luke Latham](https://github.com/guardrex)

Este artículo se describen [Visual Studio](https://www.visualstudio.com/vs/) la compatibilidad para depurar aplicaciones de ASP.NET Core que se ejecutan detrás de IIS en Windows Server. Este tema le guía a través de habilitar esta característica y configurar un proyecto.

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="enable-iis"></a>Habilitar IIS

1. Vaya a **Panel de control** > **Programas** > **Programas y características** > **Activar o desactivar las características de Windows** (lado izquierdo de la pantalla).
1. Seleccione el **Internet Information Services** casilla de verificación.

![Casilla de verificación de que muestra las características de Windows Internet Information Services activada como un cuadrado negro (no una marca de verificación) que indica que algunas de las características IIS están habilitados](development-time-iis-support/_static/enable_iis.png)

La instalación de IIS puede requerir un reinicio del sistema.

## <a name="configure-iis"></a>Configurar IIS

IIS debe tener un sitio Web configurado con lo siguiente:

* Nombre de host de un nombre de host que coincida con la dirección URL del perfil de inicio de la aplicación.
* Enlace para el puerto 443 con un certificado asignado.

Por ejemplo, el **nombre de Host** para un sitio Web de agregado se establece en "localhost" (el perfil de inicio también usarán "localhost" más adelante en este tema). El puerto se establece en "443" (HTTPS). El **certificado de IIS Express desarrollo** se asigna para el sitio Web, pero cualquier certificado válido funciona:

![Agregar ventana del sitio Web en IIS que muestra el conjunto de enlace para el host local en el puerto 443 con un certificado asignado.](development-time-iis-support/_static/add-website-window.png)

Si la instalación de IIS ya tiene un **sitio Web predeterminado** con un nombre de host que coincida con el nombre de host de dirección URL de perfil de inicio de la aplicación:

* Agregue un enlace de puerto para el puerto 443 (HTTPS).
* Asignar un certificado válido para el sitio Web.

## <a name="enable-development-time-iis-support-in-visual-studio"></a>Habilitar la compatibilidad IIS de tiempo de desarrollo en Visual Studio

1. Inicie al instalador de Visual Studio.
1. Seleccione el **IIS compatible con el tiempo de desarrollo** componente. El componente se enumera como opcionales en la **resumen** panel para la **ASP.NET y desarrollo web** carga de trabajo. El componente instala el [módulo principal de ASP.NET](xref:fundamentals/servers/aspnet-core-module), que es un módulo nativo de IIS necesario para ejecutar aplicaciones de IIS de ASP.NET Core en una configuración de proxy inverso.

![Modificación de las características de Visual Studio: la pestaña Cargas de trabajo está seleccionada. En la sección Web and Cloud (Web y nube), el panel Desarrollo de ASP.NET y web está seleccionado. A la derecha en el área opcional del panel de resumen, hay una casilla de verificación para IIS compatible con el tiempo de desarrollo.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a>Configuración del proyecto

### <a name="https-redirection"></a>Redirección de HTTPS

Para un nuevo proyecto, active la casilla de verificación para **configurar para HTTPS** en el **nueva aplicación de ASP.NET Core Web** ventana:

![Nueva ventana de aplicación Web de ASP.NET Core con la configuración para activada la casilla de verificación HTTPS.](development-time-iis-support/_static/new-app.png)

En un proyecto existente, use HTTPS redirección Middleware en `Startup.Configure` mediante una llamada a la [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection) método de extensión:

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Error");
        app.UseHsts();
    }

    app.UseHttpsRedirection();
    app.UseStaticFiles();
    app.UseCookiePolicy();

    app.UseMvc();
}
```

### <a name="iis-launch-profile"></a>Perfil de inicio de IIS

Crear un nuevo perfil de inicio para agregar compatibilidad en tiempo de desarrollo IIS:

1. Para **perfil**, seleccione la **New** botón. Nombre del perfil "IIS" en la ventana emergente. Seleccione **Aceptar** para crear el perfil.
1. Para el **iniciar** configuración, seleccione **IIS** en la lista.
1. Active la casilla de verificación para **Iniciar explorador** y proporcione la dirección URL del extremo. Use el protocolo HTTPS. Este ejemplo usa `https://localhost/WebApplication1`.
1. En el **variables de entorno** sección, seleccione la **agregar** botón. Proporcionar una variable de entorno con una clave de `ASPNETCORE_ENVIRONMENT` y un valor de `Development`.
1. En el **configuración del servidor Web** área, establecer la **URL de la aplicación**. Este ejemplo usa `https://localhost/WebApplication1`.
1. Guarde el perfil.

![Ventana Propiedades del proyecto con la pestaña Depurar seleccionada. Los valores Perfil e Inicio están definidos en IIS. La característica del explorador de inicio está habilitada con la dirección https://localhost/WebApplication1. También se proporciona la misma dirección en el campo de dirección URL de aplicación del área de configuración del servidor Web.](development-time-iis-support/_static/project_properties.png)

O bien, agregar manualmente un perfil de inicio para la [launchSettings.json](http://json.schemastore.org/launchsettings) archivo en la aplicación:

```json
{
  "iisSettings": {
    "windowsAuthentication": false,
    "anonymousAuthentication": true,
    "iis": {
      "applicationUrl": "https://localhost/WebApplication1",
      "sslPort": 0
    }
  },
  "profiles": {
    "IIS": {
      "commandName": "IIS",
      "launchBrowser": true,
      "launchUrl": "https://localhost/WebApplication1",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```

## <a name="run-the-project"></a>Ejecute el proyecto

En la interfaz de usuario de VS, establezca el botón ejecutar en el **IIS** genera un perfil y seleccione el botón para iniciar la aplicación:

![Botón de ejecución en la barra de herramientas de VS que se establece en el perfil "IIS".](development-time-iis-support/_static/toolbar.png)

Visual Studio puede solicitar un reinicio si no se ejecuta como administrador. Si es así, reinicie Visual Studio.

Si se utiliza un certificado de confianza de desarrollo, el explorador puede requerir crear una excepción para el certificado no es de confianza.

## <a name="additional-resources"></a>Recursos adicionales

* [Hospedaje de ASP.NET Core en Windows con IIS](xref:host-and-deploy/iis/index)
* [Introducción al módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)
* [Referencia de configuración del módulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Aplicación de HTTPS](xref:security/enforcing-ssl)
