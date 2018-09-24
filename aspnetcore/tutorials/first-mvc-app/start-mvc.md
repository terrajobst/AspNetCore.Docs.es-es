---
title: Introducción a ASP.NET Core MVC y Visual Studio
author: rick-anderson
description: Obtenga información sobre cómo empezar a usar ASP.NET Core MVC y Visual Studio.
ms.author: riande
ms.date: 10/07/2017
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 41f986a06ec46dc025c4e8218745b4a513e8ee2a
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011711"
---
# <a name="get-started-with-aspnet-core-mvc-and-visual-studio"></a>Introducción a ASP.NET Core MVC y Visual Studio

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [consider RP](~/includes/razor.md)]

Hay tres versiones de este tutorial:

* macOS: [Creación de una aplicación de ASP.NET Core MVC con Visual Studio para Mac](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows: [Creación de una aplicación de ASP.NET Core MVC con Visual Studio](xref:tutorials/first-mvc-app/start-mvc)
* macOS, Linux y Windows: [Creación de una aplicación de ASP.NET Core MVC con Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)

## <a name="install-visual-studio-and-net-core"></a>Instalar Visual Studio y .NET Core

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-web-app"></a>Creación de una aplicación web

En Visual Studio, seleccione **Archivo > Nuevo > Proyecto**.

![Archivo > Nuevo > Proyecto](start-mvc/_static/alt_new_project.png)

Complete el cuadro de diálogo **Nuevo proyecto**:

* En el panel izquierdo, pulse **.NET Core**.
* En el panel central, pulse **Aplicación web ASP.NET Core (.NET Core)**.
* Asigne al proyecto el nombre "MvcMovie" (es importante asignarle este nombre para que, al copiar el código, coincida con el espacio de nombres).
* Pulse **Aceptar**.

![Cuadro de diálogo Nuevo proyecto, .NET CORE en el panel izquierdo, Aplicación web ASP.NET Core ](start-mvc/_static/new_project2-21.png)

Complete el cuadro de diálogo **Nueva aplicación web ASP.NET Core (.NET Core) - MvcMovie**:

* En el cuadro desplegable del selector de versión, seleccione **ASP.NET Core 2.1**.
* Seleccione **Aplicación web (controlador de vista de modelos)**.
* Pulse **Aceptar**.

![Cuadro de diálogo Nuevo proyecto, .NET CORE en el panel izquierdo, Aplicación web ASP.NET Core ](start-mvc/_static/new_project22-21.png)

Visual Studio ha usado una plantilla predeterminada para el proyecto de MVC que acaba de crear. Si escribe un nombre de proyecto y selecciona algunas opciones, dispondrá de inmediato de una aplicación operativa. Se trata de un proyecto introductorio básico, pero es un buen punto de partida.

Pulse **F5** para ejecutar la aplicación en modo de depuración o **Ctrl-F5** para ejecutarla en modo de no depuración.
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![aplicación en ejecución](start-mvc/_static/1.png)

* Visual Studio inicia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación. Tenga en cuenta que en la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`). Esto es así porque `localhost` es el nombre de host estándar del equipo local. Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web. En la imagen anterior, el número de puerto es el 5000. La dirección URL del explorador es `localhost:5000`. Al ejecutar la aplicación verá otro puerto distinto.
* Iniciar la aplicación con **CTRL+F5** (modo de no depuración) le permite efectuar cambios en el código, guardar el archivo, actualizar el explorador y ver los cambios de código. Muchos desarrolladores prefieren usar el modo de no depuración para iniciar la aplicación rápidamente y ver los cambios.
* Puede iniciar la aplicación en modo de depuración o en modo de no depuración desde el elemento de menú **Depurar**:

![Menú Depurar](start-mvc/_static/debug_menu.png)

* Puede depurar la aplicación pulsando el botón **IIS Express**.

![IIS Express](start-mvc/_static/iis_express.png)

La plantilla predeterminada proporciona los vínculos **Inicio, Acerca de** y **Contacto**, totalmente funcionales. En la imagen del explorador anterior no se muestran estos vínculos. Según el tamaño del explorador, tendrá que hacer clic en el icono de navegación para que se muestren.

![icono de navegación en la esquina superior derecha](start-mvc/_static/2.png)

Si iba a efectuar una ejecución en modo de depuración, pulse **MAYÚS-F5** para detener la depuración.

En la siguiente sección de este tutorial obtendrá información sobre MVC y empezará a escribir código.

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!INCLUDE [](~/includes/net-core-prereqs.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

Instale Visual Studio Community 2017. Seleccione la descarga de Community. Omita este paso si tiene instalado Visual Studio 2017.

* [Página principal del instalador de Visual Studio 2017](https://www.visualstudio.com/)

Ejecute el instalador y seleccione las siguientes cargas de trabajo:

* **Desarrollo de ASP.NET y web** (en **Web y nube**)
* **Desarrollo multiplataforma de .NET Core** (en **Otros conjuntos de herramientas**)

![**Desarrollo de ASP.NET y web** (en **Web y nube**)](start-mvc/_static/web_workload.png)

![**Desarrollo multiplataforma de .NET Core** (en **Otros conjuntos de herramientas**)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a>Crear una aplicación web

En Visual Studio, seleccione **Archivo > Nuevo > Proyecto**.

![Archivo > Nuevo > Proyecto](start-mvc/_static/alt_new_project.png)

Complete el cuadro de diálogo **Nuevo proyecto**:

* En el panel izquierdo, pulse **.NET Core**.
* En el panel central, pulse **Aplicación web ASP.NET Core (.NET Core)**.
* Asigne al proyecto el nombre "MvcMovie" (es importante asignarle este nombre para que, al copiar el código, coincida con el espacio de nombres).
* Pulse **Aceptar**.

![Cuadro de diálogo Nuevo proyecto, .NET CORE en el panel izquierdo, Aplicación web ASP.NET Core ](start-mvc/_static/new_project2.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Complete el cuadro de diálogo **Nueva aplicación web ASP.NET Core (.NET Core) - MvcMovie**:

* En el cuadro desplegable del selector de versión, seleccione **ASP.NET Core 2.-**.
* Seleccione **Aplicación web (controlador de vista de modelos)**.
* Pulse **Aceptar**.

![Cuadro de diálogo Nuevo proyecto, .NET CORE en el panel izquierdo, Aplicación web ASP.NET Core ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Complete el cuadro de diálogo **Nueva aplicación web ASP.NET Core (.NET Core) - MvcMovie**:

* En el cuadro desplegable del selector de versión, pulse **ASP.NET Core 1.1**.
* Pulse **Aplicación web**.
* Mantenga la opción predeterminado **Sin autenticación**.
* Pulse **Aceptar**.

![Nueva aplicación web de ASP.NET Core](start-mvc/_static/p3.png)

---

Visual Studio ha usado una plantilla predeterminada para el proyecto de MVC que acaba de crear. Si escribe un nombre de proyecto y selecciona algunas opciones, dispondrá de inmediato de una aplicación operativa. Se trata de un proyecto introductorio básico, pero es un buen punto de partida.

Pulse **F5** para ejecutar la aplicación en modo de depuración o **Ctrl-F5** para ejecutarla en modo de no depuración.
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![aplicación en ejecución](start-mvc/_static/1.png)

* Visual Studio inicia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación. Tenga en cuenta que en la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`). Esto es así porque `localhost` es el nombre de host estándar del equipo local. Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web. En la imagen anterior, el número de puerto es el 5000. La dirección URL del explorador es `localhost:5000`. Al ejecutar la aplicación verá otro puerto distinto.
* Iniciar la aplicación con **CTRL+F5** (modo de no depuración) le permite efectuar cambios en el código, guardar el archivo, actualizar el explorador y ver los cambios de código. Muchos desarrolladores prefieren usar el modo de no depuración para iniciar la aplicación rápidamente y ver los cambios.
* Puede iniciar la aplicación en modo de depuración o en modo de no depuración desde el elemento de menú **Depurar**:

![Menú Depurar](start-mvc/_static/debug_menu.png)

* Puede depurar la aplicación pulsando el botón **IIS Express**.

![IIS Express](start-mvc/_static/iis_express.png)

La plantilla predeterminada proporciona los vínculos **Inicio, Acerca de** y **Contacto**, totalmente funcionales. En la imagen del explorador anterior no se muestran estos vínculos. Según el tamaño del explorador, tendrá que hacer clic en el icono de navegación para que se muestren.

![icono de navegación en la esquina superior derecha](start-mvc/_static/2.png)

Si iba a efectuar una ejecución en modo de depuración, pulse **MAYÚS-F5** para detener la depuración.

En la siguiente sección de este tutorial obtendrá información sobre MVC y empezará a escribir código.

::: moniker-end

> [!div class="step-by-step"]
> [Siguiente](adding-controller.md)  
