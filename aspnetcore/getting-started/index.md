---
title: Introducción a ASP.NET Core
author: rick-anderson
description: Tutorial rápido que crea y ejecuta una aplicación Hola mundo sencilla mediante ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 22e9c982921cc03d89506e18ff99bf481027dda6
ms.sourcegitcommit: 931b6a2d7eb28a0f1295e8a95690b8c4c5f58477
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2018
ms.locfileid: "37077665"
---
# <a name="get-started-with-aspnet-core"></a>Introducción a ASP.NET Core

::: moniker range=">= aspnetcore-2.1"

1. Instale el [!INCLUDE [](~/includes/2.1-SDK.md)].

2. Cree un proyecto de ASP.NET Core. Abra un shell de comandos y escriba el siguiente comando:

    ```console
    dotnet new webapp -o aspnetcoreapp
    ```

    [!INCLUDE [](~/includes/webapp-alias-notice.md) [](~/includes/webapp-alias-notice.md)]

3. Confíe en el certificado de desarrollo HTTPS:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following dialog:

    ![Security warning dialog](_static/cert.png)

    Select **Yes** if you agree to trust the development certificate.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following message:

    *Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:*
    `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`
    *This command might prompt you for your password to install the certificate on the system keychain.
    Password:*

    Enter your password if you agree to trust the development certificate.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

    See the documentation for your Linux distribution on how to trust the HTTPS development certificate
---

4. Ejecute la aplicación:

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

5. Vaya a [http://localhost:5001](http://localhost:5001).  Haga clic en **Aceptar** para aceptar la política de privacidad y de cookies. Esta aplicación no conserva información de carácter personal.

6. Abra *Pages/About.cshtml* y modifique la página con el siguiente marcado resaltado:

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

7. Vaya a [http://localhost:5001/About](http://localhost:5001/About) y confirme que los cambios aparecen reflejados.

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. Instale el [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].

2. Cree un proyecto de ASP.NET Core.

   Abra un shell de comandos. Escriba el comando siguiente:

    ```console
    dotnet new razor -o aspnetcoreapp
    ```

3. Ejecute la aplicación con los siguientes comandos:

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

4. Vaya a [http://localhost:5000](http://localhost:5000).

5. Abra *Pages/About.cshtml* y modifique la página para que muestre el mensaje "¡Hola, mundo!". La hora del servidor es @DateTime.Now" :

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. Vaya a [http://localhost:5000/About](http://localhost:5000/About) y compruebe los cambios.

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. Instale el **instalador del SDK** de .NET Core para SDK 1.0.4 desde la [página de descargas de .NET Core](https://www.microsoft.com/net/download/all).

2. Cree una carpeta para un nuevo proyecto de ASP .NET Core.

   Abra un shell de comandos. Escriba los siguientes comandos:

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. Si ha instalado una versión posterior del SDK en el equipo, cree un archivo *global.json* para seleccionar la versión 1.0.4 del SDK.

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. Cree un proyecto de ASP.NET Core.

   ```console
   dotnet new web
   ```

5. Restaure los paquetes.

    ```console
    dotnet restore
    ```

6. Ejecute la aplicación.

   ```console
   dotnet run
   ```

   El comando [dotnet run](/dotnet/core/tools/dotnet-run) compila primero la aplicación en caso de que sea necesario.

7. Vaya a `http://localhost:5000`.

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end
