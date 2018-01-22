---
title: "Introducción a las páginas de Razor en ASP.NET Core con Visual Studio Code"
author: rick-anderson
description: "Introducción a las páginas de Razor en ASP.NET Core con Visual Studio Code"
ms.author: riande
manager: wpickett
ms.date: 08/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: 727c3d5c8bed0aef3094816b7e5f0a940aea654c
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
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

![Página Inicio o Índice](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a>Abrir el proyecto

Presione CTRL+C para cerrar la aplicación.

En Visual Studio Code (VS Code), seleccione **Archivo > Abrir carpeta** y elija la carpeta *RazorPagesMovie*.

- Seleccione **Sí** en el mensaje de **advertencia** "Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?" (Faltan los activos necesarios para compilar y depurar en 'RazorPagesMovie'. Add them?" (Faltan los activos necesarios para compilar y depurar en 'TodoApi'. ¿Quiere agregarlos?).
- Seleccione **Restaurar** en el mensaje de **información** "Hay dependencias no resueltas".

### <a name="launch-the-app"></a>Iniciar la aplicación

Presione Ctrl+F5 para iniciar la aplicación sin depurar. Si lo prefiere, puede ir al menú **Depurar** y seleccionar **Iniciar sin depurar**.

En el tutorial siguiente, agregaremos un modelo al proyecto. 

>[!div class="step-by-step"]
[Siguiente: Adición de un modelo](xref:tutorials/razor-pages-vsc/model)  
