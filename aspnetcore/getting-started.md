---
title: "Introducción a ASP.NET Core 2.0"
author: rick-anderson
description: "Tutorial rápido que crea y ejecuta una aplicación Hola mundo sencilla mediante ASP.NET Core."
keywords: "ASP.NET Core,tutorial,introducción"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.assetid: 73543e9d-d9d5-47d6-9664-17a9beea6cd3
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started
ms.openlocfilehash: 3399df3958093da9117b013736b1cb370fd6beb2
ms.sourcegitcommit: 297ee5d2f3b3b24eb8a2c4a25195c9e2973cb91b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/14/2017
---
# <a name="getting-started-with-aspnet-core"></a>Introducción a ASP.NET Core

> [!NOTE]
> Estas instrucciones corresponden a la versión más reciente de ASP.NET Core. Si busca la introducción de una versión anterior, vea [la versión 1.1 de este tutorial](xref:getting-started-1.1).

1. Instale [.NET Core](https://microsoft.com/net/core/).

2. Cree un nuevo proyecto de .NET Core.

   En macOS y Linux, abra una ventana de terminal. En Windows, abra un símbolo del sistema.

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   dotnet new web
   ```
    
4. Ejecute la aplicación.

   El comando `dotnet run` primero compila la aplicación en caso necesario.

   ```terminal
   dotnet run
   ```

5. Vaya a `http://localhost:5000`

### <a name="next-steps"></a>Pasos siguientes

Para obtener tutoriales de introducción, vea [Tutoriales de ASP.NET Core](tutorials/index.md)

Para obtener una introducción a la arquitectura y los conceptos de ASP.NET Core, vea [ASP.NET Core Introduction (Introducción a ASP.NET Core)](index.md) y [ASP.NET Core Fundamentals (Conceptos básicos de ASP.NET Core)](fundamentals/index.md).

Una aplicación de ASP.NET Core puede usar la biblioteca de clases base y el runtime de .NET Core o .NET Framework. Para más información, vea [Selección entre .NET Core y .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).
