---
title: Introducción a ASP.NET Core
author: rick-anderson
description: Tutorial rápido que crea y ejecuta una aplicación Hola mundo sencilla mediante ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: e814277663ff5a964171a71ebb6e0f094e0ddc60
ms.sourcegitcommit: 3d071fabaf90e32906df97b08a8d00e602db25c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/10/2018
---
# <a name="get-started-with-aspnet-core"></a>Introducción a ASP.NET Core

::: moniker range=">= aspnetcore-2.0"

1. Instale el [!INCLUDE[](~/includes/net-core-sdk-download-link.md)].

2. Cree un nuevo proyecto de .NET Core.

   En macOS y Linux, abra una ventana de terminal. En Windows, abra un símbolo del sistema. Escriba el comando siguiente:

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```

3. Ejecute la aplicación con los siguientes comandos:

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. Vaya a [http://localhost:5000](http://localhost:5000).

5. Abra *Pages/About.cshtml* y modifique la página para que muestre el mensaje "¡Hola, mundo!". La hora del servidor es @DateTime.Now" :

    [!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. Vaya a [http://localhost:5000/About](http://localhost:5000/About) y compruebe los cambios.

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. Instale el **instalador del SDK** de .NET Core para SDK 1.0.4 desde la [página de descargas de .NET Core](https://www.microsoft.com/net/download/all).

2. Cree una carpeta para un nuevo proyecto de .NET Core.

   En macOS y Linux, abra una ventana de terminal. En Windows, abra un símbolo del sistema.

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. Si ha instalado una versión posterior del SDK en el equipo, cree un archivo *global.json* para seleccionar la versión 1.0.4 del SDK.

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. Cree un nuevo proyecto de .NET Core.

   ```terminal
   dotnet new web
   ```

5. Restaure los paquetes.

    ```terminal
    dotnet restore
    ```

6. Ejecute la aplicación.

   ```terminal
   dotnet run
   ```

   El comando [dotnet run](/dotnet/core/tools/dotnet-run) compila primero la aplicación en caso de que sea necesario.

7. Vaya a `http://localhost:5000`.

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end