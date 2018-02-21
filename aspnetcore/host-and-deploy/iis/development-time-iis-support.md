---
title: Compatibilidad de IIS de tiempo de desarrollo en Visual Studio para ASP.NET Core
author: shirhatti
description: "Descubrir compatibilidad para depurar aplicaciones de ASP.NET Core cuando se ejecuta detrás de IIS en Windows Server."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 09/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/iis/development-time-iis-support
ms.openlocfilehash: a8bdf4c0c0399c62666e6e61e70c0298a42c2c12
ms.sourcegitcommit: 9f758b1550fcae88ab1eb284798a89e6320548a5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2018
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a>Compatibilidad de IIS de tiempo de desarrollo en Visual Studio para ASP.NET Core

De: [Sourabh Shirhatti](https://twitter.com/sshirhatti)

Este artículo se describen [Visual Studio](https://www.visualstudio.com/vs/) la compatibilidad para depurar aplicaciones de ASP.NET Core que se ejecutan detrás de IIS en Windows Server. Este tema le guía a través de habilitar esta característica y configurar un proyecto.

## <a name="prerequisites"></a>Requisitos previos

* Visual Studio (2017, versión 15.3 o una posterior)
* Carga de trabajo de desarrollo de ASP.NET y web *O BIEN* la carga de trabajo de desarrollo multiplataforma de .NET Core

## <a name="enable-iis"></a>Habilitar IIS

Habilitar IIS. Vaya a **Panel de control** > **Programas** > **Programas y características** > **Activar o desactivar las características de Windows** (lado izquierdo de la pantalla). Seleccione la casilla **Internet Information Services**.

![Características de Windows en las que se muestra la casilla Internet Information Services marcada con un cuadrado negro (no una marca de verificación) que indica que algunas de las características de IIS están habilitadas](development-time-iis-support/_static/enable_iis.png)

Si la instalación de IIS requiere un reinicio, reinicie el sistema.

## <a name="enable-development-time-iis-support"></a>Habilitar compatibilidad con IIS en tiempo de desarrollo

Inicie al instalador de Visual Studio. Seleccione el **IIS compatible con el tiempo de desarrollo** componente. El componente se enumera como opcionales en la **resumen** panel para la **ASP.NET y desarrollo web** carga de trabajo. Esto instala los [módulo principal de ASP.NET](xref:fundamentals/servers/aspnet-core-module), que es un módulo nativo de IIS necesario para ejecutar aplicaciones de ASP.NET Core.

![Modificación de las características de Visual Studio: la pestaña Cargas de trabajo está seleccionada. En la sección Web and Cloud (Web y nube), el panel Desarrollo de ASP.NET y web está seleccionado. A la derecha en el área opcional del panel de resumen, hay una casilla de verificación para IIS compatible con el tiempo de desarrollo.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a>Configuración del proyecto

Cree un nuevo perfil de inicio para agregar la compatibilidad con IIS en tiempo de desarrollo. En el **Explorador de soluciones** de Visual Studio, haga clic con el botón derecho en el proyecto y seleccione **Propiedades**. Seleccione la pestaña **Depurar**. Seleccione **IIS** en el menú desplegable **Iniciar**. Confirme que la característica **Iniciar explorador** esté habilitada con la URL correcta.

![Ventana Propiedades del proyecto con la pestaña Depurar seleccionada. Los valores Perfil e Inicio están definidos en IIS. La característica Iniciar explorador está habilitada con una dirección de http://localhost/WebApplication2. La misma dirección también se proporciona en el campo Dirección URL de la aplicación de la sección Configuración del servidor web con la opción Habilitar autenticación anónima habilitada.](development-time-iis-support/_static/project_properties.png)

O bien, agregar manualmente un perfil de inicio para la [launchSettings.json](http://json.schemastore.org/launchsettings) archivo en la aplicación:

```json
{
    "iisSettings": {
        "windowsAuthentication": false,
        "anonymousAuthentication": true,
        "iis": {
            "applicationUrl": "http://localhost/WebApplication2",
            "sslPort": 0
        }
    },
    "profiles": {
        "IIS": {
            "commandName": "IIS",
            "launchBrowser": "true",
            "launchUrl": "http://localhost/WebApplication2",
            "environmentVariables": {
                "ASPNETCORE_ENVIRONMENT": "Development"
            }
        }
    }
}
```

Visual Studio puede solicitar un reinicio si no se ejecuta como administrador. Si es así, reinicie Visual Studio.

## <a name="additional-resources"></a>Recursos adicionales

* [Hospedaje de ASP.NET Core en Windows con IIS](xref:host-and-deploy/iis/index)
* [Introducción al módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)
* [Referencia de configuración del módulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module)
