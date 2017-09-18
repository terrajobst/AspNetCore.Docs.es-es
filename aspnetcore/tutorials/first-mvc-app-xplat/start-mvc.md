---
title: "Introducción a ASP.NET Core MVC en Mac, Linux o Windows"
author: rick-anderson
description: "Introducción a ASP.NET Core MVC y Visual Studio Code en Mac, Linux y Windows"
keywords: ASP.NET Core, MVC, VS Code, Visual Studio Code, Mac, Linux, Windows
ms.author: riande
manager: wpickett
ms.date: 07/07/2017
ms.topic: get-started-article
ms.assetid: 1d18b589-1638-4dc6-1638-fb0f41998d78
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-xplat/start-mvc
ms.openlocfilehash: 82ce5bbc695b190759ac2c05cdceebb5f7854eb7
ms.sourcegitcommit: e6a8f171f26fab1b2195a2d7f14e7d258a2e690e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/23/2017
---
# <a name="getting-started-with-aspnet-core-mvc--on-mac-linux-or-windows"></a>Introducción a ASP.NET Core MVC en Mac, Linux o Windows

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

En este tutorial aprenderá los aspectos básicos de la creación de una aplicación web de ASP.NET Core MVC con [Visual Studio Code](https://code.visualstudio.com) (VS Code). Para realizar el tutorial debe estar familiarizado con VS Code. Para más información, vea [Getting started with VS Code](https://code.visualstudio.com/docs) (Introducción a VS Code) y [Visual Studio Code help](#visual-studio-code-help) (Ayuda de Visual Studio Code). [!INCLUDE[consider RP](../../includes/razor.md)]

Hay tres versiones de este tutorial:

* macOS: [Creación de una aplicación de ASP.NET Core MVC con Visual Studio para Mac](xref:tutorials/first-mvc-app-mac/start-mvc)
* Windows: [Creación de una aplicación de ASP.NET Core MVC con Visual Studio](xref:tutorials/first-mvc-app/start-mvc)
* macOS, Linux y Windows: [Creación de una aplicación de ASP.NET Core MVC con Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc) 

## <a name="install-vs-code-and-net-core"></a>Instalación de VS Code y .NET Core

Para realizar este tutorial se necesita el [SDK de .NET Core 2.0.0](https://dot.net/core) o una versión posterior. Vea [este PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-mvc-app-mac/start-mvc/8-23-17.pdf) para la versión 1.1 de ASP.NET Core.

Instale el software siguiente:

* [SDK de .NET Core 2.0.0](https://dot.net/core) o versiones posteriores
* [Visual Studio Code](https://code.visualstudio.com)
* [Extensión de C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) de VS Code 

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
- [Integrated Terminal](https://code.visualstudio.com/docs/editor/integrated-terminal) (Terminal integrado)
- [Keyboard shortcuts](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference) (Métodos abreviados de teclado)

  - [Mac keyboard shortcuts](https://go.microsoft.com/fwlink/?linkid=832143) (Métodos abreviados de teclado de Mac)
  - [Linux keyboard shortcuts](https://go.microsoft.com/fwlink/?linkid=832144) (Métodos abreviados de teclado de Linux)
  - [Windows keyboard shortcuts](https://go.microsoft.com/fwlink/?linkid=832145) (Métodos abreviados de teclado de Windows)

>[!div class="step-by-step"]
[Next - Add a controller](adding-controller.md) (Siguiente - Agregar un controlador)
