---
title: Introducción a ASP.NET Core 1.1
author: rick-anderson
description: Realice este tutorial rápido para crear y ejecutar una sencilla aplicación Hola mundo a través de ASP.NET Core 1.1.
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started-1.1
ms.openlocfilehash: c61a9a918e51bbd6c1f1142a04473393c8fc54ca
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2018
---
# <a name="get-started-with-aspnet-core-11"></a>Introducción a ASP.NET Core 1.1

> [!NOTE]
> Estas instrucciones corresponden a ASP.NET Core 1.1. Si busca la versión más reciente, vea [la versión actual de este tutorial](xref:getting-started).

1. Instale el **instalador del SDK** de .NET Core para SDK 1.0.4 desde la [página de descargas de .NET Core](https://www.microsoft.com/net/download/all).

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

   El comando [dotnet run](/dotnet/core/tools/dotnet-run) compila primero la aplicación en caso de que sea necesario.

   ```terminal
   dotnet run
   ```

5. Vaya a `http://localhost:5000`

<!-- H3 to avoid a single-entry internal TOC -->
### <a name="next-steps"></a>Pasos siguientes

Para obtener tutoriales de introducción, vea [Tutoriales de ASP.NET Core](tutorials/index.md)

Para obtener una introducción a la arquitectura y los conceptos de ASP.NET Core, vea [ASP.NET Core Introduction (Introducción a ASP.NET Core)](index.md) y [ASP.NET Core Fundamentals (Conceptos básicos de ASP.NET Core)](fundamentals/index.md).

Una aplicación de ASP.NET Core puede usar la biblioteca de clases base y el runtime de .NET Core o .NET Framework. Para más información, vea [Selección entre .NET Core y .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).
