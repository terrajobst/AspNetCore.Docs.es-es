---
title: Introducción a ASP.NET Core
author: rick-anderson
description: Tutorial rápido que crea y ejecuta una aplicación Hola mundo sencilla mediante ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/15/2019
uid: getting-started
ms.openlocfilehash: 76728c484368a8b63130c259a9663473970846d3
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58209481"
---
# <a name="tutorial-get-started-with-aspnet-core"></a>Tutorial: Introducción a ASP.NET Core

En este tutorial se muestra cómo usar la interfaz de la línea de comandos de .NET Core para crear una aplicación web ASP.NET Core.

Aprenderá a:

> [!div class="checklist"]
> * Crear un proyecto de aplicación web.
> * Habilitar HTTPS local.
> * Ejecutar la aplicación.
> * Editar una página de Razor.

Al final, tendrá una aplicación web en funcionamiento ejecutándose en el equipo local.

![Página principal de aplicación web](_static/home-page.png)

## <a name="prerequisites"></a>Requisitos previos

* [SDK de .NET Core 2.2](https://www.microsoft.com/net/download/all)

## <a name="create-a-web-app-project"></a>Crear un proyecto de aplicación web

Abra un shell de comandos y escriba el siguiente comando:

```console
dotnet new webapp -o aspnetcoreapp
```

## <a name="enable-local-https"></a>Habilitar HTTPS local

Confíe en el certificado de desarrollo HTTPS:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```console
dotnet dev-certs https --trust
```

El comando anterior muestra el siguiente cuadro de diálogo:

![Cuadro de diálogo de advertencia de seguridad](~/getting-started/_static/cert.png)

Si acepta confiar en el certificado de desarrollo, seleccione **Sí**.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```console
dotnet dev-certs https --trust
```

El comando anterior muestra el siguiente mensaje:

*Se ha solicitado que confíe en el certificado de desarrollo HTTPS. Si el certificado todavía no es de confianza, no se ejecutará el comando siguiente:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.

Este comando podría solicitarle la contraseña para instalar el certificado en la cadena de claves del sistema. Si acepta confiar en el certificado de desarrollo, escriba la contraseña.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Consulte la documentación correspondiente a su distribución Linux sobre cómo confiar en el certificado de desarrollo HTTPS.

---

Para más información, consulte [Confiar en el certificado de desarrollo de ASP.NET Core HTTPS ](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)

## <a name="run-the-app"></a>Ejecutar la aplicación

Ejecute los comandos siguientes:

```console
cd aspnetcoreapp
dotnet run
```

Después de que el shell de comandos indique que se ha iniciado la aplicación, vaya a [https://localhost:5001](https://localhost:5001). Haga clic en **Aceptar** para aceptar la política de privacidad y de cookies. Esta aplicación no conserva información de carácter personal.

## <a name="edit-a-razor-page"></a>Editar una página de Razor

Abra *Pages/Index.cshtml* y modifique la página con el siguiente marcado resaltado:

[!code-cshtml[](sample/index.cshtml?highlight=9)]

Vaya a [https://localhost:5001](https://localhost:5001) y confirme que los cambios aparecen reflejados.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha aprendido a:

> [!div class="checklist"]
> * Crear un proyecto de aplicación web.
> * Habilitar HTTPS local.
> * Ejecute el proyecto.
> * Realizar un cambio.

Para obtener más información sobre ASP.NET Core, vea la ruta de aprendizaje recomendada en la introducción:

> [!div class="nextstepaction"]
> <xref:index#recommended-learning-path>
