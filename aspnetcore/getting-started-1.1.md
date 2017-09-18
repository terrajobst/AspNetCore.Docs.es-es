---
title: "Introducción a ASP.NET Core 1.1"
author: rick-anderson
description: "Tutorial rápido que crea y ejecuta una aplicación Hola mundo sencilla mediante ASP.NET Core 1.1."
keywords: "ASP.NET Core,tutorial,introducción"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.assetid: 73543e9d-d9d5-47d6-9664-17a9beea6cd3
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started-1.1
ms.openlocfilehash: e8fd9ef60ebc1cff6ca0e03000ea50eebff0a9f9
ms.sourcegitcommit: 297ee5d2f3b3b24eb8a2c4a25195c9e2973cb91b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/14/2017
---
# <a name="getting-started-with-aspnet-core-11"></a>Introducción a ASP.NET Core 1.1

> [!NOTE]
> Estas instrucciones corresponden a ASP.NET Core 1.1. Si busca la versión más reciente, vea [la versión actual de este tutorial](xref:getting-started).

1. Instale el **instalador del SDK** de .NET Core para SDK 1.0.4 desde la [página de descargas de .NET Core 1.0.5 & 1.1.2 SDK 1.0.4](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.0.5-download.md).

2. Cree una carpeta para un nuevo proyecto de .NET Core.

   En macOS y Linux, abra una ventana de terminal. En Windows, abra un símbolo del sistema.

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

2. Si ha instalado una versión posterior del SDK en el equipo, cree un archivo *global.json* para seleccionar la versión 1.0.4 del SDK.

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

2. Cree un nuevo proyecto de .NET Core.

   ```terminal
   dotnet new web
   ```
   
3.  Restaure los paquetes.

    ```terminal
    dotnet restore
    ```

4. Ejecute la aplicación.

   El comando `dotnet run` primero compila la aplicación en caso necesario.

   ```terminal
   dotnet run
   ```

5. Vaya a `http://localhost:5000`

<!-- H3 to avoid a single-entry internal TOC -->
### <a name="next-steps"></a>Pasos siguientes

Para obtener tutoriales de introducción, vea [Tutoriales de ASP.NET Core](tutorials/index.md)

Para obtener una introducción a la arquitectura y los conceptos de ASP.NET Core, vea [ASP.NET Core Introduction (Introducción a ASP.NET Core)](index.md) y [ASP.NET Core Fundamentals (Conceptos básicos de ASP.NET Core)](fundamentals/index.md).

Una aplicación de ASP.NET Core puede usar la biblioteca de clases base y el runtime de .NET Core o .NET Framework. Para más información, vea [Selección entre .NET Core y .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).
