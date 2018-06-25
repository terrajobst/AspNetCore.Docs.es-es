---
title: Introducción a ASP.NET Core MVC en macOS, Linux o Windows
author: rick-anderson
description: Obtenga información sobre cómo empezar a usar ASP.NET Core MVC y Visual Studio Code en macOS, Linux y Windows.
ms.author: riande
ms.date: 07/07/2017
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: b25ee29541e345a3bf6700b67d992409c02b183a
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275280"
---
# <a name="introduction-to-aspnet-core-mvc-on-macos-linux-or-windows"></a>Introducción a ASP.NET Core MVC en macOS, Linux o Windows

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

En este tutorial aprenderá los aspectos básicos de la creación de una aplicación web de ASP.NET Core MVC con [Visual Studio Code](https://code.visualstudio.com) (VS Code). Para realizar el tutorial debe estar familiarizado con VS Code. Para más información, vea [Getting started with VS Code](https://code.visualstudio.com/docs) (Introducción a VS Code) y [Visual Studio Code help](#visual-studio-code-help) (Ayuda de Visual Studio Code). 

[!INCLUDE [consider RP](../../includes/razor.md)]

Hay tres versiones de este tutorial:

* macOS: [Creación de una aplicación de ASP.NET Core MVC con Visual Studio para Mac](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows: [Creación de una aplicación de ASP.NET Core MVC con Visual Studio](xref:tutorials/first-mvc-app/start-mvc)
* macOS, Linux y Windows: [Creación de una aplicación de ASP.NET Core MVC con Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc) 

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-web-app-with-dotnet"></a>Creación de una aplicación web con dotnet

Desde un terminal, ejecute estos comandos:

```console
mkdir MvcMovie
cd MvcMovie
dotnet new mvc
```

Abra la carpeta *MvcMovie* en Visual Studio Code (VS Code) y seleccione el archivo *Startup.cs*.

- Seleccione **Sí** en el mensaje de **advertencia** "Required assets to build and debug are missing from 'MvcMovie'. Add them?" (Faltan los activos necesarios para compilar y depurar en 'MvcMovie'. ¿Desea agregarlos?).
- Seleccione **Restaurar** en el mensaje de **información** "There are unresolved dependencies" (Hay dependencias no resueltas).

![VS Code con la advertencia "Required assets to build and debug are missing from 'MvcMovie'. Add them?" (Faltan los activos necesarios para compilar y depurar en 'MvcMovie'. ¿Desea agregarlos?). Don't ask Again (No volver a preguntar), Not Now (Ahora no), Yes (Sí) y también Info (Información) - There are unresolved dependencies (Hay dependencias sin resolver) - Restore (Restaurar) - Close (Cerrar)](../web-api-vsc/_static/vsc_restore.png)

Presione **Depurar** (F5) para compilar y ejecutar el programa.

![aplicación en ejecución](../first-mvc-app/start-mvc/_static/1.png)

VS Code inicia el servidor web [Kestrel](xref:fundamentals/servers/kestrel) y ejecuta la aplicación. Tenga en cuenta que la barra de direcciones muestra `localhost:5000` (y no algo como `example.com`). Esto es así porque `localhost` es el nombre de host estándar del equipo local.

La plantilla predeterminada proporciona los vínculos **Inicio, Acerca de** y **Contacto**, totalmente funcionales. En la imagen del explorador anterior no se muestran estos vínculos. Según el tamaño del explorador, tendrá que hacer clic en el icono de navegación para que se muestren.

![icono de navegación en la esquina superior derecha](../first-mvc-app/start-mvc/_static/2.png)

En la siguiente sección de este tutorial obtendrá información sobre MVC y empezará a escribir código.

## <a name="visual-studio-code-help"></a>Ayuda de Visual Studio Code

- [Introducción](https://code.visualstudio.com/docs)
- [Depuración](https://code.visualstudio.com/docs/editor/debugging)
- [Terminal integrado](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [Métodos abreviados de teclado](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [Funciones rápidas de teclado de macOS](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [Métodos abreviados de teclado de Linux](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [Windows keyboard shortcuts](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf) (Métodos abreviados de teclado de Windows)

> [!div class="step-by-step"]
> [Next - Add a controller](adding-controller.md) (Siguiente - Agregar un controlador)
