---
title: Introducción a ASP.NET Core
author: rick-anderson
description: Tutorial rápido que crea y ejecuta una aplicación Hola mundo sencilla mediante ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 4c899ff3c087f1b569521c6e2e8458fca01d6061
ms.sourcegitcommit: bdfba5e7575b2a786ef27c0edf688c7dbd09ee95
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/22/2018
ms.locfileid: "52288647"
---
# <a name="tutorial-get-started-with-aspnet-core"></a>Tutorial: Introducción a ASP.NET Core

En este tutorial se muestra cómo usar la interfaz de la línea de comandos de .NET Core para crear una aplicación web ASP.NET Core. Aprenderá a:

> [!div class="checklist"]
> * Crear un proyecto de aplicación web.
> * Habilitar HTTPS local.
> * Ejecute la aplicación.
> * Editar una página de Razor.

Al final, tendrá una aplicación web en funcionamiento ejecutándose en el equipo local.

![Página principal de aplicación web](_static/home-page.png)


## <a name="prerequisites"></a>Requisitos previos

* Instale el [!INCLUDE [](~/includes/2.1-SDK.md)].

## <a name="create-a-web-app-project"></a>Crear un proyecto de aplicación web

* Abra un shell de comandos y escriba el siguiente comando:

   ```console
   dotnet new webapp -o aspnetcoreapp
   ```

## <a name="enable-local-https"></a>Habilitar HTTPS local

* Confíe en el certificado de desarrollo HTTPS:

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

## <a name="run-the-app"></a>Ejecutar la aplicación

* Ejecute los comandos siguientes:

   ```console
   cd aspnetcoreapp
   dotnet run
   ```

* Vaya a [https://localhost:5001](https://localhost:5001). Haga clic en **Aceptar** para aceptar la política de privacidad y de cookies. Esta aplicación no conserva información de carácter personal.

## <a name="edit-a-razor-page"></a>Editar una página de Razor

* Abra *Pages/About.cshtml* y modifique la página con el siguiente marcado resaltado:

   [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

* Vaya a [https://localhost:5001/About](https://localhost:5001/About) y confirme que los cambios aparecen reflejados.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha aprendido a:

> [!div class="checklist"]
> * Crear un proyecto de aplicación web.
> * Habilitar HTTPS local.
> * Ejecute el proyecto.
> * Realizar un cambio.

Para obtener más información sobre ASP.NET Core, vea la introducción:

> [!div class="nextstepaction"]
> <xref:index>



> [!NOTE]
> Vamos a probar la facilidad de uso de una nueva estructura propuesta para la tabla de contenido de ASP.NET Core.  Si tiene unos minutos para realizar un ejercicio de búsqueda de 7 temas diferentes en la tabla actual o propuesta de contenido, [haga clic aquí para participar en el estudio](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).