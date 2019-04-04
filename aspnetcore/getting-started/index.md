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
# <a name="tutorial-get-started-with-aspnet-core"></a><span data-ttu-id="b9c87-103">Tutorial: Introducción a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b9c87-103">Tutorial: Get started with ASP.NET Core</span></span>

<span data-ttu-id="b9c87-104">En este tutorial se muestra cómo usar la interfaz de la línea de comandos de .NET Core para crear una aplicación web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b9c87-104">This tutorial shows how to use the .NET Core command-line interface to create an ASP.NET Core web app.</span></span>

<span data-ttu-id="b9c87-105">Aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="b9c87-105">You'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b9c87-106">Crear un proyecto de aplicación web.</span><span class="sxs-lookup"><span data-stu-id="b9c87-106">Create a web app project.</span></span>
> * <span data-ttu-id="b9c87-107">Habilitar HTTPS local.</span><span class="sxs-lookup"><span data-stu-id="b9c87-107">Enable local HTTPS.</span></span>
> * <span data-ttu-id="b9c87-108">Ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b9c87-108">Run the app.</span></span>
> * <span data-ttu-id="b9c87-109">Editar una página de Razor.</span><span class="sxs-lookup"><span data-stu-id="b9c87-109">Edit a Razor page.</span></span>

<span data-ttu-id="b9c87-110">Al final, tendrá una aplicación web en funcionamiento ejecutándose en el equipo local.</span><span class="sxs-lookup"><span data-stu-id="b9c87-110">At the end, you'll have a working web app running on your local machine.</span></span>

![Página principal de aplicación web](_static/home-page.png)

## <a name="prerequisites"></a><span data-ttu-id="b9c87-112">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="b9c87-112">Prerequisites</span></span>

* [<span data-ttu-id="b9c87-113">SDK de .NET Core 2.2</span><span class="sxs-lookup"><span data-stu-id="b9c87-113">.NET Core 2.2 SDK</span></span>](https://www.microsoft.com/net/download/all)

## <a name="create-a-web-app-project"></a><span data-ttu-id="b9c87-114">Crear un proyecto de aplicación web</span><span class="sxs-lookup"><span data-stu-id="b9c87-114">Create a web app project</span></span>

<span data-ttu-id="b9c87-115">Abra un shell de comandos y escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="b9c87-115">Open a command shell, and enter the following command:</span></span>

```console
dotnet new webapp -o aspnetcoreapp
```

## <a name="enable-local-https"></a><span data-ttu-id="b9c87-116">Habilitar HTTPS local</span><span class="sxs-lookup"><span data-stu-id="b9c87-116">Enable local HTTPS</span></span>

<span data-ttu-id="b9c87-117">Confíe en el certificado de desarrollo HTTPS:</span><span class="sxs-lookup"><span data-stu-id="b9c87-117">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="b9c87-118">Windows</span><span class="sxs-lookup"><span data-stu-id="b9c87-118">Windows</span></span>](#tab/windows)

```console
dotnet dev-certs https --trust
```

<span data-ttu-id="b9c87-119">El comando anterior muestra el siguiente cuadro de diálogo:</span><span class="sxs-lookup"><span data-stu-id="b9c87-119">The preceding command displays the following dialog:</span></span>

![Cuadro de diálogo de advertencia de seguridad](~/getting-started/_static/cert.png)

<span data-ttu-id="b9c87-121">Si acepta confiar en el certificado de desarrollo, seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="b9c87-121">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="b9c87-122">macOS</span><span class="sxs-lookup"><span data-stu-id="b9c87-122">macOS</span></span>](#tab/macos)

```console
dotnet dev-certs https --trust
```

<span data-ttu-id="b9c87-123">El comando anterior muestra el siguiente mensaje:</span><span class="sxs-lookup"><span data-stu-id="b9c87-123">The preceding command displays the following message:</span></span>

<span data-ttu-id="b9c87-124">*Se ha solicitado que confíe en el certificado de desarrollo HTTPS. Si el certificado todavía no es de confianza, no se ejecutará el comando siguiente:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span><span class="sxs-lookup"><span data-stu-id="b9c87-124">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`</span></span>

<span data-ttu-id="b9c87-125">Este comando podría solicitarle la contraseña para instalar el certificado en la cadena de claves del sistema.</span><span class="sxs-lookup"><span data-stu-id="b9c87-125">This command might prompt you for your password to install the certificate on the system keychain.</span></span> <span data-ttu-id="b9c87-126">Si acepta confiar en el certificado de desarrollo, escriba la contraseña.</span><span class="sxs-lookup"><span data-stu-id="b9c87-126">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="b9c87-127">Linux</span><span class="sxs-lookup"><span data-stu-id="b9c87-127">Linux</span></span>](#tab/linux)

<span data-ttu-id="b9c87-128">Consulte la documentación correspondiente a su distribución Linux sobre cómo confiar en el certificado de desarrollo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b9c87-128">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>

---

<span data-ttu-id="b9c87-129">Para más información, consulte [Confiar en el certificado de desarrollo de ASP.NET Core HTTPS ](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span><span class="sxs-lookup"><span data-stu-id="b9c87-129">For more information, see [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span></span>

## <a name="run-the-app"></a><span data-ttu-id="b9c87-130">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="b9c87-130">Run the app</span></span>

<span data-ttu-id="b9c87-131">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="b9c87-131">Run the following commands:</span></span>

```console
cd aspnetcoreapp
dotnet run
```

<span data-ttu-id="b9c87-132">Después de que el shell de comandos indique que se ha iniciado la aplicación, vaya a [https://localhost:5001](https://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="b9c87-132">After the command shell indicates that the app has started, browse to [https://localhost:5001](https://localhost:5001).</span></span> <span data-ttu-id="b9c87-133">Haga clic en **Aceptar** para aceptar la política de privacidad y de cookies.</span><span class="sxs-lookup"><span data-stu-id="b9c87-133">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="b9c87-134">Esta aplicación no conserva información de carácter personal.</span><span class="sxs-lookup"><span data-stu-id="b9c87-134">This app doesn't keep personal information.</span></span>

## <a name="edit-a-razor-page"></a><span data-ttu-id="b9c87-135">Editar una página de Razor</span><span class="sxs-lookup"><span data-stu-id="b9c87-135">Edit a Razor page</span></span>

<span data-ttu-id="b9c87-136">Abra *Pages/Index.cshtml* y modifique la página con el siguiente marcado resaltado:</span><span class="sxs-lookup"><span data-stu-id="b9c87-136">Open *Pages/Index.cshtml* and modify the page with the following highlighted markup:</span></span>

[!code-cshtml[](sample/index.cshtml?highlight=9)]

<span data-ttu-id="b9c87-137">Vaya a [https://localhost:5001](https://localhost:5001) y confirme que los cambios aparecen reflejados.</span><span class="sxs-lookup"><span data-stu-id="b9c87-137">Browse to [https://localhost:5001](https://localhost:5001), and verify the changes are displayed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b9c87-138">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="b9c87-138">Next steps</span></span>

<span data-ttu-id="b9c87-139">En este tutorial ha aprendido a:</span><span class="sxs-lookup"><span data-stu-id="b9c87-139">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b9c87-140">Crear un proyecto de aplicación web.</span><span class="sxs-lookup"><span data-stu-id="b9c87-140">Create a web app project.</span></span>
> * <span data-ttu-id="b9c87-141">Habilitar HTTPS local.</span><span class="sxs-lookup"><span data-stu-id="b9c87-141">Enable local HTTPS.</span></span>
> * <span data-ttu-id="b9c87-142">Ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="b9c87-142">Run the project.</span></span>
> * <span data-ttu-id="b9c87-143">Realizar un cambio.</span><span class="sxs-lookup"><span data-stu-id="b9c87-143">Make a change.</span></span>

<span data-ttu-id="b9c87-144">Para obtener más información sobre ASP.NET Core, vea la ruta de aprendizaje recomendada en la introducción:</span><span class="sxs-lookup"><span data-stu-id="b9c87-144">To learn more about ASP.NET Core, see the recommended learning path in the introduction:</span></span>

> [!div class="nextstepaction"]
> <xref:index#recommended-learning-path>
