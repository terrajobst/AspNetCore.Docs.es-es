---
title: Introducción a ASP.NET Core
author: rick-anderson
description: Tutorial rápido que crea y ejecuta una aplicación Hola mundo sencilla mediante ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: c2f18c69901a5a6503314d508a776e6985872681
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-aspnet-core"></a>Introducción a ASP.NET Core

> [!NOTE]
> Estas instrucciones corresponden a la versión más reciente de ASP.NET Core. Vea [Introducción a ASP.NET Core 1.1](xref:getting-started-1.1) para obtener este documento, pero sobre la versión 1.1.

1. Instale el [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].

2. Cree un nuevo proyecto de .NET Core.

   En macOS y Linux, abra una ventana de terminal. En Windows, abra un símbolo del sistema. Escriba el comando siguiente:

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
3. Ejecute la aplicación.

    Use los comandos siguientes para ejecutar la aplicación:

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. Vaya a [http://localhost:5000](http://localhost:5000).

5. Abra <em>Pages/About.cshtml</em> y modifique la página para que muestre el mensaje "¡Hola, mundo!". La hora del servidor es "@DateTime.Now":

    [!code-html[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. Vaya a [http://localhost:5000/About](http://localhost:5000/About) y compruebe los cambios.

### <a name="next-steps"></a>Pasos siguientes

Para obtener tutoriales de introducción, vea [Tutoriales de ASP.NET Core](tutorials/index.md)

Para obtener una introducción a la arquitectura y los conceptos de ASP.NET Core, vea [ASP.NET Core Introduction (Introducción a ASP.NET Core)](index.md) y [ASP.NET Core Fundamentals (Conceptos básicos de ASP.NET Core)](fundamentals/index.md).

Una aplicación de ASP.NET Core puede usar la biblioteca de clases base y el runtime de .NET Core o .NET Framework. Para más información, vea [Selección entre .NET Core y .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).
