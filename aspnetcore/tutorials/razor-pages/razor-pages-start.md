---
title: "Introducción a las páginas de Razor en ASP.NET Core"
author: rick-anderson
description: "Introducción a las páginas de Razor en ASP.NET Core"
keywords: "ASP.NET Core, páginas de Razor, Razor, MVC"
ms.author: riande
manager: wpickett
ms.date: 12/22/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 4643665d48ca07ff43ce52064291fc106bd5c8ac
ms.sourcegitcommit: df2157ae9aeea0075772719c29784425c783e82a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/10/2018
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a>Introducción a las páginas de Razor en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

En este tutorial se enseñan los conceptos básicos de la compilación de una aplicación web de páginas de Razor de ASP.NET Core. Las páginas de Razor son el método recomendado para crear la interfaz de usuario de aplicaciones web en ASP.NET Core.

Hay tres versiones de este tutorial:

* Windows: este tutorial
* MacOS: [Introducción a las páginas de Razor en ASP.NET Core con Visual Studio para Mac](xref:tutorials/razor-pages-mac/razor-pages-start)
* macOS, Linux y Windows: [Introducción a las páginas de Razor en ASP.NET Core con Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="create-a-razor-web-app"></a>Creación de una aplicación web de Razor

* En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.
* Cree una aplicación web de ASP.NET Core. Asigne al proyecto el nombre **RazorPagesMovie**. Es importante asignarle el nombre *RazorPagesMovie* para que los espacios de nombres coincidan al copiar y pegar el código.
  ![Nueva aplicación web de ASP.NET Core](../../mvc/razor-pages/index/_static/np.png)
* Seleccione **ASP.NET Core 2.0** en la lista desplegable y, luego, seleccione **Aplicación web**.

  [!INCLUDE[install 2.0](../../includes/dotnetcore-on-dotnetfx-vs.md)]

La plantilla de Visual Studio crea un proyecto de inicio:

![Explorador de soluciones](razor-pages-start/_static/se.png)

Presione **F5** para ejecutar la aplicación en modo de depuración o **Ctrl-F5** para que se ejecute sin adjuntar el depurador.

![Página Inicio o Índice](razor-pages-start/_static/home.png)

* Visual Studio inicia [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación. En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`). Esto es así porque `localhost` es el nombre de host estándar del equipo local. Localhost solo sirve las solicitudes web del equipo local. Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web. En la imagen anterior, el número de puerto es 5000. Al ejecutar la aplicación verá otro puerto distinto.
* Iniciar la aplicación con **CTRL+F5** (modo de no depuración) le permite efectuar cambios en el código, guardar el archivo, actualizar el explorador y ver los cambios de código. Muchos desarrolladores prefieren usar el modo de no depuración para iniciar la aplicación rápidamente y ver los cambios.

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

>[!div class="step-by-step"]
[Siguiente: Adición de un modelo](xref:tutorials/razor-pages/model)

>[!div class="step-by-step"]
[Siguiente: Adición de un modelo](xref:tutorials/razor-pages/model)
