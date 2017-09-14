---
title: "Introducción a las páginas de Razor en ASP.NET Core en Mac"
author: rick-anderson
description: "Introducción a las páginas de Razor en ASP.NET Core con Visual Studio para Mac"
keywords: "ASP.NET Core,páginas Razor,scaffolding,Entity Framework Core,EF,EF Core,base de datos,mac,macOS,Visual Studio para Mac"
ms.author: riande
manager: wpickett
ms.date: 7/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-mac/razor-pages-start
ms.openlocfilehash: caadc3fcb3bb71abe0773aed4f6ff60a043e3a02
ms.sourcegitcommit: d9ec19e5452af83648074db5d96c0a0f4f9e7f9a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2017
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-for-mac"></a>Introducción a las páginas de Razor en ASP.NET Core con Visual Studio para Mac

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

En este tutorial se enseñan los conceptos básicos de la creación de una aplicación web de las páginas Razor de ASP.NET Core. Se recomienda completar [Introducción a las páginas Razor](xref:mvc/razor-pages/index) antes de iniciar este tutorial. Las páginas de Razor son el método recomendado para crear la interfaz de usuario de aplicaciones web en ASP.NET Core.

## <a name="prerequisites"></a>Requisitos previos

Instale los elementos siguientes:

* [SDK de .NET Core 2.0.0](https://dot.net/core) o versiones posteriores
* [Visual Studio para Mac](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-a-razor-web-app"></a>Crear una aplicación web de Razor

Desde un terminal, ejecute los comandos siguientes:

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

En Visual Studio, seleccione **Archivo > Abrir** y elija el archivo *RazorPagesMovie.csproj*.

### <a name="launch-the-app"></a>Iniciar la aplicación

En Visual Studio, seleccione **Ejecutar > Iniciar sin depurar** para iniciar la aplicación. Visual Studio iniciará [IIS Express](http://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) y un explorador y se desplazará a `http://localhost:5000`.

En el tutorial siguiente, agregaremos un modelo al proyecto.

>[!div class="step-by-step"]
[Siguiente: Agregar un modelo](xref:tutorials/razor-pages-mac/model)
