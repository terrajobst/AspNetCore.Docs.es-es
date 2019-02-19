---
title: Introducción a ASP.NET Core
author: rick-anderson
description: Tutorial rápido que crea y ejecuta una aplicación Hola mundo sencilla mediante ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/15/2019
uid: getting-started
ms.openlocfilehash: eee927f4306fa7757f3f361e6c6f367658512897
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341815"
---
# <a name="tutorial-get-started-with-aspnet-core"></a><span data-ttu-id="95dba-103">Tutorial: Introducción a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="95dba-103">Tutorial: Get started with ASP.NET Core</span></span>

<span data-ttu-id="95dba-104">En este tutorial se muestra cómo usar la interfaz de la línea de comandos de .NET Core para crear una aplicación web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="95dba-104">This tutorial shows how to use the .NET Core command-line interface to create an ASP.NET Core web app.</span></span>

<span data-ttu-id="95dba-105">Aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="95dba-105">You'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="95dba-106">Crear un proyecto de aplicación web.</span><span class="sxs-lookup"><span data-stu-id="95dba-106">Create a web app project.</span></span>
> * <span data-ttu-id="95dba-107">Habilitar HTTPS local.</span><span class="sxs-lookup"><span data-stu-id="95dba-107">Enable local HTTPS.</span></span>
> * <span data-ttu-id="95dba-108">Ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="95dba-108">Run the app.</span></span>
> * <span data-ttu-id="95dba-109">Editar una página de Razor.</span><span class="sxs-lookup"><span data-stu-id="95dba-109">Edit a Razor page.</span></span>

<span data-ttu-id="95dba-110">Al final, tendrá una aplicación web en funcionamiento ejecutándose en el equipo local.</span><span class="sxs-lookup"><span data-stu-id="95dba-110">At the end, you'll have a working web app running on your local machine.</span></span>

![Página principal de aplicación web](_static/home-page.png)

## <a name="prerequisites"></a><span data-ttu-id="95dba-112">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="95dba-112">Prerequisites</span></span>

* [<span data-ttu-id="95dba-113">SDK de .NET Core 2.2</span><span class="sxs-lookup"><span data-stu-id="95dba-113">.NET Core 2.2 SDK</span></span>](https://www.microsoft.com/net/download/all)

## <a name="create-a-web-app-project"></a><span data-ttu-id="95dba-114">Crear un proyecto de aplicación web</span><span class="sxs-lookup"><span data-stu-id="95dba-114">Create a web app project</span></span>

<span data-ttu-id="95dba-115">Abra un shell de comandos y escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="95dba-115">Open a command shell, and enter the following command:</span></span>

```console
dotnet new webapp -o aspnetcoreapp
```

## <a name="enable-local-https"></a><span data-ttu-id="95dba-116">Habilitar HTTPS local</span><span class="sxs-lookup"><span data-stu-id="95dba-116">Enable local HTTPS</span></span>

<span data-ttu-id="95dba-117">Confíe en el certificado de desarrollo HTTPS:</span><span class="sxs-lookup"><span data-stu-id="95dba-117">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="95dba-118">Windows</span><span class="sxs-lookup"><span data-stu-id="95dba-118">Windows</span></span>](#tab/windows)

```console
dotnet dev-certs https --trust
```

<span data-ttu-id="95dba-119">El comando anterior muestra el siguiente cuadro de diálogo:</span><span class="sxs-lookup"><span data-stu-id="95dba-119">The preceding command displays the following dialog:</span></span>

![Cuadro de diálogo de advertencia de seguridad](_static/cert.png)

<span data-ttu-id="95dba-121">Si acepta confiar en el certificado de desarrollo, seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="95dba-121">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="95dba-122">macOS</span><span class="sxs-lookup"><span data-stu-id="95dba-122">macOS</span></span>](#tab/macos)

```console
dotnet dev-certs https --trust
```

<span data-ttu-id="95dba-123">El comando anterior muestra el siguiente mensaje:</span><span class="sxs-lookup"><span data-stu-id="95dba-123">The preceding command displays the following message:</span></span>

<span data-ttu-id="95dba-124">*Se ha solicitado que confíe en el certificado de desarrollo HTTPS. Si el certificado todavía no es de confianza, no se ejecutará el comando siguiente:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span><span class="sxs-lookup"><span data-stu-id="95dba-124">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span></span>  
<span data-ttu-id="95dba-125">\* Este comando podría solicitarle la contraseña para instalar el certificado en la cadena de claves del sistema.</span><span class="sxs-lookup"><span data-stu-id="95dba-125">\*This command might prompt you for your password to install the certificate on the system keychain.</span></span>

<span data-ttu-id="95dba-126">Contraseña:\*</span><span class="sxs-lookup"><span data-stu-id="95dba-126">Password:\*</span></span>

<span data-ttu-id="95dba-127">Si acepta confiar en el certificado de desarrollo, escriba la contraseña.</span><span class="sxs-lookup"><span data-stu-id="95dba-127">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="95dba-128">Linux</span><span class="sxs-lookup"><span data-stu-id="95dba-128">Linux</span></span>](#tab/linux)

<span data-ttu-id="95dba-129">Consulte la documentación correspondiente a su distribución Linux sobre cómo confiar en el certificado de desarrollo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="95dba-129">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>

---

## <a name="run-the-app"></a><span data-ttu-id="95dba-130">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="95dba-130">Run the app</span></span>

<span data-ttu-id="95dba-131">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="95dba-131">Run the following commands:</span></span>

```console
cd aspnetcoreapp
dotnet run
```

<span data-ttu-id="95dba-132">Después de que el shell de comandos indique que se ha iniciado la aplicación, vaya a [https://localhost:5001](https://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="95dba-132">After the command shell indicates that the app has started, browse to [https://localhost:5001](https://localhost:5001).</span></span> <span data-ttu-id="95dba-133">Haga clic en **Aceptar** para aceptar la política de privacidad y de cookies.</span><span class="sxs-lookup"><span data-stu-id="95dba-133">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="95dba-134">Esta aplicación no conserva información de carácter personal.</span><span class="sxs-lookup"><span data-stu-id="95dba-134">This app doesn't keep personal information.</span></span>

## <a name="edit-a-razor-page"></a><span data-ttu-id="95dba-135">Editar una página de Razor</span><span class="sxs-lookup"><span data-stu-id="95dba-135">Edit a Razor page</span></span>

<span data-ttu-id="95dba-136">Abra *Pages/Index.cshtml* y modifique la página con el siguiente marcado resaltado:</span><span class="sxs-lookup"><span data-stu-id="95dba-136">Open *Pages/Index.cshtml* and modify the page with the following highlighted markup:</span></span>

[!code-cshtml[](sample/index.cshtml?highlight=9)]

<span data-ttu-id="95dba-137">Vaya a [https://localhost:5001](https://localhost:5001) y confirme que los cambios aparecen reflejados.</span><span class="sxs-lookup"><span data-stu-id="95dba-137">Browse to [https://localhost:5001](https://localhost:5001), and verify the changes are displayed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="95dba-138">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="95dba-138">Next steps</span></span>

<span data-ttu-id="95dba-139">En este tutorial ha aprendido a:</span><span class="sxs-lookup"><span data-stu-id="95dba-139">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="95dba-140">Crear un proyecto de aplicación web.</span><span class="sxs-lookup"><span data-stu-id="95dba-140">Create a web app project.</span></span>
> * <span data-ttu-id="95dba-141">Habilitar HTTPS local.</span><span class="sxs-lookup"><span data-stu-id="95dba-141">Enable local HTTPS.</span></span>
> * <span data-ttu-id="95dba-142">Ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="95dba-142">Run the project.</span></span>
> * <span data-ttu-id="95dba-143">Realizar un cambio.</span><span class="sxs-lookup"><span data-stu-id="95dba-143">Make a change.</span></span>

<span data-ttu-id="95dba-144">Para obtener más información sobre ASP.NET Core, vea la introducción:</span><span class="sxs-lookup"><span data-stu-id="95dba-144">To learn more about ASP.NET Core, see the introduction:</span></span>

> [!div class="nextstepaction"]
> <xref:index>
