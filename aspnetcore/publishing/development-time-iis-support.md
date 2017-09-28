---
title: Compatibilidad de IIS de tiempo de desarrollo en Visual Studio para ASP.NET Core
author: shirhatti
description: "Descubra la compatibilidad para depurar aplicaciones de ASP.NET Core al ejecutarlas detrás de IIS en Windows Server."
keywords: "ASP.NET Core,internet information services,iis,Windows Server,módulo asp.net core,depuración"
ms.author: riande
manager: wpickett
ms.date: 09/13/2017
ms.topic: article
ms.assetid: 83d98477-9d10-4a78-a54a-f325ad67d13b
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/development-time-iis-support
ms.openlocfilehash: a35a6fd9896c4c110d1b6680b6aaf718d29a18ab
ms.sourcegitcommit: 74a8ad9c1ba5c155d7c4303e67632a0922c38e86
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2017
---
# <a name="development-time-iis-support-in-visual-studio-for-aspnet-core"></a>Compatibilidad de IIS de tiempo de desarrollo en Visual Studio para ASP.NET Core

De: [Sourabh Shirhatti](https://twitter.com/sshirhatti)

En este artículo se describe la compatibilidad de [Visual Studio](https://www.visualstudio.com/vs/) para depurar aplicaciones de ASP.NET Core ejecutadas detrás de IIS en Windows Server. En este tema se explica cómo habilitar esta característica y configurar un proyecto.

## <a name="prerequisites"></a>Requisitos previos

* Visual Studio (2017, versión 15.3 o una posterior)
* Carga de trabajo de desarrollo de ASP.NET y web *O BIEN* la carga de trabajo de desarrollo multiplataforma de .NET Core

## <a name="enable-iis"></a>Habilitar IIS

Habilite IIS en su sistema. Vaya a **Panel de control** > **Programas** > **Programas y características** > **Activar o desactivar las características de Windows** (lado izquierdo de la pantalla). Seleccione la casilla **Internet Information Services**.

![Características de Windows en las que se muestra la casilla Internet Information Services marcada con un cuadrado negro (no una marca de verificación) que indica que algunas de las características de IIS están habilitadas](development-time-iis-support/_static/enable_iis.png)

Si la instalación de IIS lo requiere, reinicie el sistema.

## <a name="enable-development-time-iis-support"></a>Habilitar compatibilidad con IIS en tiempo de desarrollo

Una vez que haya instalado IIS, inicie el Instalador de Visual Studio para modificar la instalación de Visual Studio existente. En el instalador, seleccione el componente **Compatibilidad con IIS en tiempo de desarrollo**. Aparece como componente opcional en el panel **Resumen** de la carga de trabajo **Desarrollo de ASP.NET y web**. Se instalará el [módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module), que es un módulo de IIS nativo necesario para ejecutar aplicaciones de ASP.NET Core.

![Modificación de las características de Visual Studio: la pestaña Cargas de trabajo está seleccionada. En la sección Web and Cloud (Web y nube), el panel Desarrollo de ASP.NET y web está seleccionado. A la derecha, en la sección Opcional del panel Resumen, hay una casilla para la opción Compatibilidad con IIS en tiempo de desarrollo.](development-time-iis-support/_static/development_time_support.png)

## <a name="configure-the-project"></a>Configuración del proyecto

Cree un nuevo perfil de inicio para agregar la compatibilidad con IIS en tiempo de desarrollo. En el **Explorador de soluciones** de Visual Studio, haga clic con el botón derecho en el proyecto y seleccione **Propiedades**. Seleccione la pestaña **Depurar**. Seleccione **IIS** en el menú desplegable **Iniciar**. Confirme que la característica **Iniciar explorador** esté habilitada con la URL correcta.

![Ventana Propiedades del proyecto con la pestaña Depurar seleccionada. Los valores Perfil e Inicio están definidos en IIS. La característica Iniciar explorador está habilitada con una dirección de http://localhost/WebApplication2. La misma dirección también se proporciona en el campo Dirección URL de la aplicación de la sección Configuración del servidor web con la opción Habilitar autenticación anónima habilitada.](development-time-iis-support/_static/project_properties.png)

Como alternativa, puede agregar manualmente un perfil de inicio para su archivo [launchSettings.json](http://json.schemastore.org/launchsettings) en la aplicación:

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

Es posible que se le pida que reinicie Visual Studio si no lo estaba ejecutando como administrador. Si es así, reinicie Visual Studio.

¡Enhorabuena! En este momento, el proyecto está configurado para la compatibilidad con IIS en tiempo de desarrollo. 

## <a name="additional-resources"></a>Recursos adicionales

* [Hospedaje de ASP.NET Core en Windows con IIS](xref:publishing/iis)
* [Introducción al módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module)
* [Referencia de configuración del módulo ASP.NET Core](xref:hosting/aspnet-core-module)
