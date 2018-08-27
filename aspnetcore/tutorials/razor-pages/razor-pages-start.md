---
title: Introducción a las páginas de Razor en ASP.NET Core
author: rick-anderson
description: Conozca los conceptos básicos de la compilación de una aplicación web de páginas de Razor de ASP.NET Core. Se recomienda usar páginas de Razor en las cargas de trabajo web en ASP.NET Core.
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: bc18ec3ad3bb7e3afe38030a34b2e748ce9e341b
ms.sourcegitcommit: 74c09caec8992635825b45b7f065f871d33c077a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2018
ms.locfileid: "42634982"
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a>Introducción a las páginas de Razor en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range="= aspnetcore-2.0"

Se recomienda seguir la versión de ASP.NET Core 2.1 de este tutorial, ya que es **mucho** más fácil de seguir y abarca más características. Seleccione **ASP.NET Core 2.1** en el selector de versión.

![Selector de versión en la TDC](razor-pages-start/_static/v21.png)

::: moniker-end

En este tutorial se enseñan los conceptos básicos de la compilación de una aplicación web de páginas de Razor de ASP.NET Core. Las páginas de Razor son el método recomendado para crear la interfaz de usuario de aplicaciones web en ASP.NET Core.

Hay tres versiones de este tutorial:

* Windows: este tutorial
* MacOS: [Introducción a las páginas de Razor en ASP.NET Core con Visual Studio para Mac](xref:tutorials/razor-pages-mac/razor-pages-start)
* macOS, Linux y Windows: [Introducción a las páginas de Razor en ASP.NET Core con Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

::: moniker range=">= aspnetcore-2.1"

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [Prerequisites](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a>Creación de una aplicación web de Razor

* En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.
* Cree una aplicación web de ASP.NET Core. Asigne al proyecto el nombre **RazorPagesMovie**. Es importante asignarle el nombre *RazorPagesMovie* para que los espacios de nombres coincidan al copiar y pegar el código.
 ![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/np_2.1.png)
* Seleccione **ASP.NET Core 2.1** en la lista desplegable y, luego, **Aplicación web**.

 ![Nueva aplicación web de ASP.NET Core](razor-pages-start/_static/np_2_2.1.png)

La plantilla de Visual Studio crea un proyecto de inicio:

![Explorador de soluciones](razor-pages-start/_static/se2.1.png)

Presione **F5** para ejecutar la aplicación en modo de depuración o **Ctrl+F5** para que se ejecute sin adjuntar el depurador. Seleccione **Aceptar** para dar su consentimiento al seguimiento. Esta aplicación no lleva un seguimiento de la información personal. El código generado con plantilla incluye activos que sirven para cumplir el [Reglamento general de protección de datos (RGPD)](xref:security/gdpr).

![Página Inicio o Índice](razor-pages-start/_static/homeGDPR.png)

En la siguiente imagen se muestra la aplicación tras haber aceptado el seguimiento:

![Página Inicio o Índice](razor-pages-start/_static/home2.1.png)

* Visual Studio inicia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación. En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`). Esto es así porque `localhost` es el nombre de host estándar del equipo local. Localhost solo sirve las solicitudes web del equipo local. Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web. En la imagen anterior, el número de puerto es 5000. Al ejecutar la aplicación verá otro puerto distinto.
* Iniciar la aplicación con **CTRL+F5** (modo de no depuración) le permite efectuar cambios en el código, guardar el archivo, actualizar el explorador y ver los cambios de código. Muchos desarrolladores prefieren usar el modo de no depuración para iniciar la aplicación rápidamente y ver los cambios.

[!INCLUDE [razor-pages-start](~/includes/RP/2.1/razor-pages-start.md)]

[!INCLUDE [F7](~/includes/RP/F7.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [Prerequisites](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a>Creación de una aplicación web de Razor

* En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.
* Cree una aplicación web de ASP.NET Core. Asigne al proyecto el nombre **RazorPagesMovie**. Es importante asignarle el nombre *RazorPagesMovie* para que los espacios de nombres coincidan al copiar y pegar el código.
  ![Nueva aplicación web de ASP.NET Core](../../razor-pages/index/_static/np.png)
* Seleccione **ASP.NET Core 2.0** en la lista desplegable y, luego, seleccione **Aplicación web**.

  [!INCLUDE [install 2.0](~/includes/dotnetcore-on-dotnetfx-vs.md)]

La plantilla de Visual Studio crea un proyecto de inicio:

![Explorador de soluciones](razor-pages-start/_static/se.png)

Presione **F5** para ejecutar la aplicación en modo de depuración o **Ctrl-F5** para que se ejecute sin adjuntar el depurador.

![Página Inicio o Índice](razor-pages-start/_static/home.png)

* Visual Studio inicia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación. En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`). Esto es así porque `localhost` es el nombre de host estándar del equipo local. Localhost solo sirve las solicitudes web del equipo local. Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web. En la imagen anterior, el número de puerto es 5000. Al ejecutar la aplicación verá otro puerto distinto.
* Iniciar la aplicación con **CTRL+F5** (modo de no depuración) le permite efectuar cambios en el código, guardar el archivo, actualizar el explorador y ver los cambios de código. Muchos desarrolladores prefieren usar el modo de no depuración para iniciar la aplicación rápidamente y ver los cambios.

[!INCLUDE [razor-pages-start](~/includes/RP/razor-pages-start.md)]

::: moniker-end

> [!div class="step-by-step"]
> [Siguiente: Adición de un modelo](xref:tutorials/razor-pages/model)
