---
title: Introducción a ASP.NET Core
author: rick-anderson
description: Tutorial rápido que crea y ejecuta una aplicación Hola mundo sencilla mediante ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 4a5a0cc5a5dab2171ab8ef43818185a4ee91af0e
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912571"
---
# <a name="tutorial-get-started-with-aspnet-core"></a><span data-ttu-id="9a8c9-103">Tutorial: Introducción a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9a8c9-103">Tutorial: Get started with ASP.NET Core</span></span>

<span data-ttu-id="9a8c9-104">En este tutorial se muestra cómo usar la interfaz de la línea de comandos de .NET Core para crear una aplicación web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9a8c9-104">This tutorial shows how to use the .NET Core command-line interface to create an ASP.NET Core web app.</span></span> <span data-ttu-id="9a8c9-105">Aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="9a8c9-105">You'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9a8c9-106">Crear un proyecto de aplicación web.</span><span class="sxs-lookup"><span data-stu-id="9a8c9-106">Create a web app project.</span></span>
> * <span data-ttu-id="9a8c9-107">Habilitar HTTPS local.</span><span class="sxs-lookup"><span data-stu-id="9a8c9-107">Enable local HTTPS.</span></span>
> * <span data-ttu-id="9a8c9-108">Ejecute la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9a8c9-108">Run the app.</span></span>
> * <span data-ttu-id="9a8c9-109">Editar una página de Razor.</span><span class="sxs-lookup"><span data-stu-id="9a8c9-109">Edit a Razor page.</span></span>

<span data-ttu-id="9a8c9-110">Al final, tendrá una aplicación web en funcionamiento ejecutándose en el equipo local.</span><span class="sxs-lookup"><span data-stu-id="9a8c9-110">At the end, you'll have a working web app running on your local machine.</span></span>

![Página principal de aplicación web](_static/home-page.png)


## <a name="prerequisites"></a><span data-ttu-id="9a8c9-112">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="9a8c9-112">Prerequisites</span></span>

* <span data-ttu-id="9a8c9-113">Instale el [!INCLUDE [](~/includes/2.1-SDK.md)].</span><span class="sxs-lookup"><span data-stu-id="9a8c9-113">Install the [!INCLUDE [](~/includes/2.1-SDK.md)].</span></span>

## <a name="create-a-web-app-project"></a><span data-ttu-id="9a8c9-114">Crear un proyecto de aplicación web</span><span class="sxs-lookup"><span data-stu-id="9a8c9-114">Create a web app project</span></span>

* <span data-ttu-id="9a8c9-115">Abra un shell de comandos y escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="9a8c9-115">Open a command shell, and enter the following command:</span></span>

   ```console
   dotnet new webapp -o aspnetcoreapp
   ```

## <a name="enable-local-https"></a><span data-ttu-id="9a8c9-116">Habilitar HTTPS local</span><span class="sxs-lookup"><span data-stu-id="9a8c9-116">Enable local HTTPS</span></span>

* <span data-ttu-id="9a8c9-117">Confíe en el certificado de desarrollo HTTPS:</span><span class="sxs-lookup"><span data-stu-id="9a8c9-117">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="9a8c9-118">Windows</span><span class="sxs-lookup"><span data-stu-id="9a8c9-118">Windows</span></span>](#tab/windows)

  ```console
  dotnet dev-certs https --trust
  ```

  <span data-ttu-id="9a8c9-119">El comando anterior muestra el siguiente cuadro de diálogo:</span><span class="sxs-lookup"><span data-stu-id="9a8c9-119">The preceding command displays the following dialog:</span></span>

  ![Cuadro de diálogo de advertencia de seguridad](_static/cert.png)

  <span data-ttu-id="9a8c9-121">Si acepta confiar en el certificado de desarrollo, seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="9a8c9-121">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="9a8c9-122">macOS</span><span class="sxs-lookup"><span data-stu-id="9a8c9-122">macOS</span></span>](#tab/macos)

  ```console
  dotnet dev-certs https --trust
  ```

  <span data-ttu-id="9a8c9-123">El comando anterior muestra el siguiente mensaje:</span><span class="sxs-lookup"><span data-stu-id="9a8c9-123">The preceding command displays the following message:</span></span>

  <span data-ttu-id="9a8c9-124">*Se ha solicitado que confíe en el certificado de desarrollo HTTPS. Si el certificado todavía no es de confianza, no se ejecutará el comando siguiente:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span><span class="sxs-lookup"><span data-stu-id="9a8c9-124">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span></span>  
  <span data-ttu-id="9a8c9-125">\* Este comando podría solicitarle la contraseña para instalar el certificado en la cadena de claves del sistema.</span><span class="sxs-lookup"><span data-stu-id="9a8c9-125">\*This command might prompt you for your password to install the certificate on the system keychain.</span></span>
  
  <span data-ttu-id="9a8c9-126">Contraseña:\*</span><span class="sxs-lookup"><span data-stu-id="9a8c9-126">Password:\*</span></span>

  <span data-ttu-id="9a8c9-127">Si acepta confiar en el certificado de desarrollo, escriba la contraseña.</span><span class="sxs-lookup"><span data-stu-id="9a8c9-127">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="9a8c9-128">Linux</span><span class="sxs-lookup"><span data-stu-id="9a8c9-128">Linux</span></span>](#tab/linux)

  <span data-ttu-id="9a8c9-129">Consulte la documentación correspondiente a su distribución Linux sobre cómo confiar en el certificado de desarrollo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9a8c9-129">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>
   
---

## <a name="run-the-app"></a><span data-ttu-id="9a8c9-130">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="9a8c9-130">Run the app</span></span>

* <span data-ttu-id="9a8c9-131">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="9a8c9-131">Run the following commands:</span></span>

   ```console
   cd aspnetcoreapp
   dotnet run
   ```

* <span data-ttu-id="9a8c9-132">Vaya a [https://localhost:5001](https://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="9a8c9-132">Browse to [https://localhost:5001](https://localhost:5001).</span></span> <span data-ttu-id="9a8c9-133">Haga clic en **Aceptar** para aceptar la política de privacidad y de cookies.</span><span class="sxs-lookup"><span data-stu-id="9a8c9-133">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="9a8c9-134">Esta aplicación no conserva información de carácter personal.</span><span class="sxs-lookup"><span data-stu-id="9a8c9-134">This app doesn't keep personal information.</span></span>

## <a name="edit-a-razor-page"></a><span data-ttu-id="9a8c9-135">Editar una página de Razor</span><span class="sxs-lookup"><span data-stu-id="9a8c9-135">Edit a Razor page</span></span>

* <span data-ttu-id="9a8c9-136">Abra *Pages/About.cshtml* y modifique la página con el siguiente marcado resaltado:</span><span class="sxs-lookup"><span data-stu-id="9a8c9-136">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

   [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

* <span data-ttu-id="9a8c9-137">Vaya a [https://localhost:5001/About](https://localhost:5001/About) y confirme que los cambios aparecen reflejados.</span><span class="sxs-lookup"><span data-stu-id="9a8c9-137">Browse to [https://localhost:5001/About](https://localhost:5001/About) and verify the changes are displayed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9a8c9-138">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="9a8c9-138">Next steps</span></span>

<span data-ttu-id="9a8c9-139">En este tutorial ha aprendido a:</span><span class="sxs-lookup"><span data-stu-id="9a8c9-139">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9a8c9-140">Crear un proyecto de aplicación web.</span><span class="sxs-lookup"><span data-stu-id="9a8c9-140">Create a web app project.</span></span>
> * <span data-ttu-id="9a8c9-141">Habilitar HTTPS local.</span><span class="sxs-lookup"><span data-stu-id="9a8c9-141">Enable local HTTPS.</span></span>
> * <span data-ttu-id="9a8c9-142">Ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="9a8c9-142">Run the project.</span></span>
> * <span data-ttu-id="9a8c9-143">Realizar un cambio.</span><span class="sxs-lookup"><span data-stu-id="9a8c9-143">Make a change.</span></span>

<span data-ttu-id="9a8c9-144">Para obtener más información sobre ASP.NET Core, vea la introducción:</span><span class="sxs-lookup"><span data-stu-id="9a8c9-144">To learn more about ASP.NET Core, see the introduction:</span></span>

> [!div class="nextstepaction"]
> <xref:index>
