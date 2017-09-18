---
title: "Introducción a las páginas de Razor en ASP.NET Core con Visual Studio Code"
author: rick-anderson
description: "Introducción a las páginas de Razor en ASP.NET Core con Visual Studio Code"
keywords: "ASP.NET Core,páginas de Razor,scaffolding,Entity Framework Core,EF,EF Core,base de datos,mac,macOS, Visual Studio Code,Code"
ms.author: riande
manager: wpickett
ms.date: 8/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: 096a60dae171ac5dbfa935be4c16e0903d8f5bb3
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2017
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-code"></a>Introducción a las páginas de Razor en ASP.NET Core con Visual Studio Code

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

En este tutorial se enseñan los conceptos básicos de la compilación de una aplicación web de páginas de Razor de ASP.NET Core. Se recomienda leer [Introduction to Razor Pages](xref:mvc/razor-pages/index) (Introducción a las páginas de Razor) antes de empezar este tutorial. Las páginas de Razor son el método recomendado para crear la interfaz de usuario de aplicaciones web en ASP.NET Core.

## <a name="prerequisites"></a>Requisitos previos

Instale el software siguiente:

* [SDK de .NET Core 2.0.0](https://www.microsoft.com/net/core) o versiones posteriores
* [Visual Studio Code](https://code.visualstudio.com)
* [Extensión de C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) de VS Code 

## <a name="create-a-razor-web-app"></a>Creación de una aplicación web de Razor

Desde un terminal, ejecute estos comandos:

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

Los comandos anteriores usan el [CLI de .NET Core](https://docs.microsoft.com/dotnet/core/tools/dotnet) para crear y ejecutar un proyecto de páginas de Razor. Abra http://localhost:5000 en un explorador para ver la aplicación.

![Página principal o de índice](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a>Abrir el proyecto

Presione CTRL+C para cerrar la aplicación.

En Visual Studio Code (VS Code), seleccione **Archivo > Abrir carpeta** y elija la carpeta *RazorPagesMovie*.

- Seleccione **Sí** en el mensaje de **advertencia** "Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?" (Faltan los activos necesarios para compilar y depurar en 'RazorPagesMovie'. ¿Desea agregarlos?).
- Seleccione **Restaurar** en el mensaje de **información** "Hay dependencias no resueltas".

### <a name="launch-the-app"></a>Iniciar la aplicación

Presione Ctrl+F5 para iniciar la aplicación sin depurar. Si lo prefiere, puede ir al menú **Depurar** y seleccionar **Iniciar sin depurar**.

En el tutorial siguiente, agregaremos un modelo al proyecto. 

>[!div class="step-by-step"]
[Siguiente: Adición de un modelo](xref:tutorials/razor-pages-vsc/model)  
