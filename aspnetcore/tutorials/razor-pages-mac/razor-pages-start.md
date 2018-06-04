---
title: Introducción a las páginas de Razor en ASP.NET Core en macOS con Visual Studio para Mac
author: rick-anderson
description: Obtenga información sobre cómo empezar a usar las páginas de Razor de ASP.NET Core con Visual Studio para Mac.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 07/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-mac/razor-pages-start
ms.openlocfilehash: 8da34f0a59976032747edcaf482f75c087ca8d8d
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2018
ms.locfileid: "34688275"
---
# <a name="get-started-with-razor-pages-in-aspnet-core-on-macos-with-visual-studio-for-mac"></a>Introducción a las páginas de Razor en ASP.NET Core en macOS con Visual Studio para Mac

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

En este tutorial se enseñan los conceptos básicos de la compilación de una aplicación web de páginas de Razor de ASP.NET Core. Se recomienda leer [Introduction to Razor Pages](xref:mvc/razor-pages/index) (Introducción a las páginas de Razor) antes de empezar este tutorial. Las páginas de Razor son el método recomendado para crear la interfaz de usuario de aplicaciones web en ASP.NET Core.

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [](~/includes/net-core-prereqs-macos.md)]

## <a name="create-a-razor-web-app"></a>Creación de una aplicación web de Razor

Desde un terminal, ejecute estos comandos:

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

Los comandos anteriores usan el [CLI de .NET Core](https://docs.microsoft.com/dotnet/core/tools/dotnet) para crear y ejecutar un proyecto de páginas de Razor. Abra http://localhost:5000 en un explorador para ver la aplicación.

![Página Inicio o Índice](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a>Abrir el proyecto

Presione CTRL+C para cerrar la aplicación.

En Visual Studio, seleccione **Archivo > Abrir** y elija el archivo *RazorPagesMovie.csproj*.

### <a name="launch-the-app"></a>Iniciar la aplicación

En Visual Studio, seleccione **Ejecutar > Iniciar sin depurar** para iniciar la aplicación. Visual Studio iniciará [Kestrel](xref:fundamentals/servers/kestrel) y un explorador, y se desplazará a `http://localhost:5000`.

En el tutorial siguiente, agregaremos un modelo al proyecto.

> [!div class="step-by-step"]
> [Siguiente: Adición de un modelo](xref:tutorials/razor-pages-mac/model)
