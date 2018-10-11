---
title: Introducción a ASP.NET Core
author: rick-anderson
description: Tutorial rápido que crea y ejecuta una aplicación Hola mundo sencilla mediante ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: a6a5023594aec01370143e7d1f35fb45c109122a
ms.sourcegitcommit: 13940eb53c68664b11a2d685ee17c78faab1945d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2018
ms.locfileid: "47860945"
---
# <a name="get-started-with-aspnet-core"></a>Introducción a ASP.NET Core

Este documento proporciona los pasos necesarios para crear y ejecutar una aplicación ASP.NET Core.

::: moniker range=">= aspnetcore-2.1"

1. Instale el [!INCLUDE [](~/includes/2.1-SDK.md)].

2. Cree un proyecto de ASP.NET Core. Abra un shell de comandos y escriba el siguiente comando:

   ```console
   dotnet new webapp -o aspnetcoreapp
   ```

3. Confíe en el certificado de desarrollo HTTPS:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

  ```console
  dotnet dev-certs https --trust
  ```

  El comando anterior muestra el siguiente cuadro de diálogo:

  ![Cuadro de diálogo de advertencia de seguridad](_static/cert.png)

  Si acepta confiar en el certificado de desarrollo, seleccione **Sí**.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

  ```console
  dotnet dev-certs https --trust
  ```

  El comando anterior muestra el siguiente mensaje:

  *Se ha solicitado que confíe en el certificado de desarrollo HTTPS. Si el certificado todavía no es de confianza, no se ejecutará el comando siguiente:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.  
  * Este comando podría solicitarle la contraseña para instalar el certificado en la cadena de claves del sistema.
  
  Contraseña:*

  Si acepta confiar en el certificado de desarrollo, escriba la contraseña.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

  Consulte la documentación correspondiente a su distribución Linux sobre cómo confiar en el certificado de desarrollo HTTPS.
   
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
