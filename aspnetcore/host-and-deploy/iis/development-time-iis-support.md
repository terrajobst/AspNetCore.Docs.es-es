---
title: Compatibilidad de IIS de tiempo de desarrollo en Visual Studio para ASP.NET Core
author: shirhatti
description: Descubra la compatibilidad con la depuración de aplicaciones ASP.NET Core cuando se ejecutan detrás de IIS en Windows Server.
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2018
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: 44570bb28451ce4c5fde12ec77e3856fb5bd3062
ms.sourcegitcommit: 816f39e852a8f453e8682081871a31bc66db153a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/19/2018
ms.locfileid: "53637669"
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a>Compatibilidad de IIS de tiempo de desarrollo en Visual Studio para ASP.NET Core

Por [Sourabh Shirhatti](https://twitter.com/sshirhatti) y [Luke Latham](https://github.com/guardrex)

En este artículo se describe la compatibilidad de [Visual Studio](https://www.visualstudio.com/vs/) con la depuración de aplicaciones ASP.NET Core que se ejecutan detrás de IIS en Windows Server. En este tema se explica cómo habilitar esta característica y configurar un proyecto.

## <a name="prerequisites"></a>Requisitos previos

* [Visual Studio para Windows](https://www.microsoft.com/net/download/windows).
* Carga de trabajo de **ASP.NET y desarrollo web**
* Carga de trabajo **Desarrollo multiplataforma de .NET Core**
* Certificado de seguridad X.509

## <a name="enable-iis"></a>Habilitar IIS

1. Vaya a **Panel de control** > **Programas** > **Programas y características** > **Activar o desactivar las características de Windows** (lado izquierdo de la pantalla).
1. Active la casilla **Internet Information Services**.

![Características de Windows que muestran la casilla de Internet Information Services activada como un cuadrado negro (no una marca de verificación) para indicar que alguna de las características de IIS está habilitada](development-time-iis-support/_static/enable_iis.png)

La instalación de IIS puede requerir un reinicio del sistema.

## <a name="configure-iis"></a>Configurar IIS

IIS debe tener un sitio web configurado con lo siguiente:

* Un nombre de host que coincida con el nombre de host de la dirección URL del perfil de inicio de la aplicación.
* Enlace al puerto 443 con un certificado asignado.

Por ejemplo, el **nombre de host** de un sitio web agregado se establece en "localhost" (el perfil de inicio también usará "localhost" más adelante en este tema). El puerto se establece en "443" (HTTPS). El **certificado de desarrollo de IIS Express** se asigna al sitio web, pero cualquier certificado válido sirve:

![Ventana Agregar sitio web de IIS que muestra el enlace establecido para localhost en el puerto 443 con un certificado asignado.](development-time-iis-support/_static/add-website-window.png)

Si la instalación de IIS ya tiene un **sitio web predeterminado** con un nombre de host que coincide con el nombre de host de la dirección URL del perfil de inicio de la aplicación:

* Agregue un enlace al puerto 443 (HTTPS).
* Asigne un certificado válido al sitio web.

## <a name="enable-development-time-iis-support-in-visual-studio"></a>Habilitación de la compatibilidad con IIS en tiempo de desarrollo en Visual Studio

1. Inicie el instalador de Visual Studio.
1. Seleccione el componente **Compatibilidad con IIS en tiempo de desarrollo**. El componente se muestra como opcional en el panel **Resumen** de la carga de trabajo **Desarrollo de ASP.NET y web**. El componente instala el [módulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module), que es un módulo de IIS nativo necesario para ejecutar aplicaciones de ASP.NET Core con IIS.

![Modificación de características de Visual Studio: la pestaña Cargas de trabajo está seleccionada. En la sección Web and Cloud (Web y nube), el panel Desarrollo de ASP.NET y web está seleccionado. A la derecha, en la sección Opcional del panel Resumen, hay una casilla para la opción Compatibilidad con IIS en tiempo de desarrollo.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a>Configuración del proyecto

### <a name="https-redirection"></a>Redireccionamiento de HTTPS

En un nuevo proyecto, active la casilla **Configure for HTTPS** (Configurar para HTTPS) en la ventana **Nueva aplicación web ASP.NET Core**:

![Ventana Nueva aplicación web ASP.NET Core con la casilla Configure for HTTPS (Configurar para HTTPS) activada](development-time-iis-support/_static/new-app.png)

En un proyecto existente, use el Middleware de redireccionamiento HTTPS en `Startup.Configure` mediante la llamada al método de extensión [UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection):

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

Cree un nuevo perfil de inicio para agregar la compatibilidad con IIS en tiempo de desarrollo:

1. En **Perfil**, seleccione el botón **Nuevo**. Asigne el perfil el nombre "IIS" en la ventana emergente. Seleccione **Aceptar** para crear el perfil.
1. En **Iniciar**, seleccione **IIS** en la lista.
1. Active la casilla **Iniciar explorador** y proporcione la dirección URL del punto de conexión. Use el protocolo HTTPS. Este ejemplo usa `https://localhost/WebApplication1`.
1. En la sección **Variables de entorno**, seleccione el botón **Agregar**. Proporcione una variable de entorno con una clave de `ASPNETCORE_ENVIRONMENT` y un valor de `Development`.
1. En el área **Configuración del servidor web**, establezca la **dirección URL de la aplicación**. Este ejemplo usa `https://localhost/WebApplication1`.
1. Guarde el perfil.

![Ventana Propiedades del proyecto con la pestaña Depurar seleccionada. Los valores Perfil e Inicio están definidos en IIS. La característica Iniciar explorador se habilita con la dirección https://localhost/WebApplication1. También se proporciona la misma dirección en el campo de dirección URL de aplicación del área de configuración del servidor web.](development-time-iis-support/_static/project_properties.png)

Como alternativa, puede agregar manualmente un perfil de inicio al archivo [launchSettings.json](http://json.schemastore.org/launchsettings) en la aplicación:

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

## <a name="run-the-project"></a>Ejecución del proyecto

En Visual Studio:

* Confirme que la lista desplegable de configuración de compilación está configurada como **Depurar**.
* Establezca el botón Ejecutar en el perfil **IIS** y seleccione el botón para iniciar la aplicación.

![El botón Ejecutar de la barra de herramientas de VS se establece en el perfil IIS con la lista desplegable de configuración de compilación establecida en Versión.](development-time-iis-support/_static/toolbar.png)

Visual Studio puede solicitar un reinicio si no se ejecuta como administrador. Si es así, reinicie Visual Studio.

Si se usa un certificado de desarrollo que no es de confianza, el explorador puede pedirle que cree una excepción para un certificado de esta clase.

> [!NOTE]
> La depuración de una configuración de compilación de versión con [Solo mi código](/visualstudio/debugger/just-my-code) y las optimizaciones del compilador degradan el rendimiento. Por ejemplo, no se alcanzan los puntos de interrupción.

## <a name="additional-resources"></a>Recursos adicionales

* [Hospedaje de ASP.NET Core en Windows con IIS](xref:host-and-deploy/iis/index)
* [Introducción al módulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Referencia de configuración del módulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
* [Aplicación de HTTPS](xref:security/enforcing-ssl)
