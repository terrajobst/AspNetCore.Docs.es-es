---
title: Introducción a ASP.NET Core
author: rick-anderson
description: Tutorial breve que crea y ejecuta una aplicación Hola mundo básica mediante ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 05/15/2019
uid: getting-started
ms.openlocfilehash: d1edf91f1b37ba2b69732471dc6c1f306ac5ad24
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2019
ms.locfileid: "71081123"
---
# <a name="tutorial-get-started-with-aspnet-core"></a><span data-ttu-id="3aa24-103">Tutorial: Introducción a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3aa24-103">Tutorial: Get started with ASP.NET Core</span></span>

<span data-ttu-id="3aa24-104">En este tutorial se muestra cómo usar la interfaz de la línea de comandos de .NET Core para crear y ejecutar una aplicación web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3aa24-104">This tutorial shows how to use the .NET Core command-line interface to create and run an ASP.NET Core web app.</span></span>

<span data-ttu-id="3aa24-105">Aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="3aa24-105">You'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="3aa24-106">Crear un proyecto de aplicación web.</span><span class="sxs-lookup"><span data-stu-id="3aa24-106">Create a web app project.</span></span>
> * <span data-ttu-id="3aa24-107">Confíe en el certificado de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="3aa24-107">Trust the development certificate.</span></span>
> * <span data-ttu-id="3aa24-108">Ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3aa24-108">Run the app.</span></span>
> * <span data-ttu-id="3aa24-109">Editar una página de Razor.</span><span class="sxs-lookup"><span data-stu-id="3aa24-109">Edit a Razor page.</span></span>

<span data-ttu-id="3aa24-110">Al final, tendrá una aplicación web en funcionamiento ejecutándose en el equipo local.</span><span class="sxs-lookup"><span data-stu-id="3aa24-110">At the end, you'll have a working web app running on your local machine.</span></span>

![Página principal de aplicación web](_static/home-page.png)

## <a name="prerequisites"></a><span data-ttu-id="3aa24-112">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="3aa24-112">Prerequisites</span></span>

* [<span data-ttu-id="3aa24-113">SDK de .NET Core 2.2</span><span class="sxs-lookup"><span data-stu-id="3aa24-113">.NET Core 2.2 SDK</span></span>](https://www.microsoft.com/net/download/all)

## <a name="create-a-web-app-project"></a><span data-ttu-id="3aa24-114">Crear un proyecto de aplicación web</span><span class="sxs-lookup"><span data-stu-id="3aa24-114">Create a web app project</span></span>

<span data-ttu-id="3aa24-115">Abra un shell de comandos y escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="3aa24-115">Open a command shell, and enter the following command:</span></span>

```dotnetcli
dotnet new webapp -o aspnetcoreapp
```

### <a name="trust-the-development-certificate"></a><span data-ttu-id="3aa24-116">Confíe en el certificado de desarrollo</span><span class="sxs-lookup"><span data-stu-id="3aa24-116">Trust the development certificate</span></span>

<span data-ttu-id="3aa24-117">Confíe en el certificado de desarrollo HTTPS:</span><span class="sxs-lookup"><span data-stu-id="3aa24-117">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="3aa24-118">Windows</span><span class="sxs-lookup"><span data-stu-id="3aa24-118">Windows</span></span>](#tab/windows)

```dotnetcli
dotnet dev-certs https --trust
```

<span data-ttu-id="3aa24-119">El comando anterior muestra el siguiente cuadro de diálogo:</span><span class="sxs-lookup"><span data-stu-id="3aa24-119">The preceding command displays the following dialog:</span></span>

![Cuadro de diálogo de advertencia de seguridad](~/getting-started/_static/cert.png)

<span data-ttu-id="3aa24-121">Si acepta confiar en el certificado de desarrollo, seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="3aa24-121">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="3aa24-122">macOS</span><span class="sxs-lookup"><span data-stu-id="3aa24-122">macOS</span></span>](#tab/macos)

```dotnetcli
dotnet dev-certs https --trust
```

<span data-ttu-id="3aa24-123">El comando anterior muestra el siguiente mensaje:</span><span class="sxs-lookup"><span data-stu-id="3aa24-123">The preceding command displays the following message:</span></span>

<span data-ttu-id="3aa24-124">*Se ha solicitado que confíe en el certificado de desarrollo HTTPS. Si el certificado todavía no es de confianza, no se ejecutará el comando siguiente:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span><span class="sxs-lookup"><span data-stu-id="3aa24-124">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`</span></span>

<span data-ttu-id="3aa24-125">Este comando podría solicitarle la contraseña para instalar el certificado en la cadena de claves del sistema.</span><span class="sxs-lookup"><span data-stu-id="3aa24-125">This command might prompt you for your password to install the certificate on the system keychain.</span></span> <span data-ttu-id="3aa24-126">Si acepta confiar en el certificado de desarrollo, escriba la contraseña.</span><span class="sxs-lookup"><span data-stu-id="3aa24-126">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="3aa24-127">Linux</span><span class="sxs-lookup"><span data-stu-id="3aa24-127">Linux</span></span>](#tab/linux)

<span data-ttu-id="3aa24-128">Para el subsistema Windows para Linux, consulte [Confiar en certificado HTTPS del subsistema de Windows para Linux](xref:security/enforcing-ssl#wsl).</span><span class="sxs-lookup"><span data-stu-id="3aa24-128">For Windows Subsystem for Linux, see [Trust HTTPS certificate from Windows Subsystem for Linux](xref:security/enforcing-ssl#wsl).</span></span>

<span data-ttu-id="3aa24-129">Consulte la documentación correspondiente a su distribución Linux sobre cómo confiar en el certificado de desarrollo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3aa24-129">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>

---

<span data-ttu-id="3aa24-130">Para más información, consulte [Confiar en el certificado de desarrollo de ASP.NET Core HTTPS ](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span><span class="sxs-lookup"><span data-stu-id="3aa24-130">For more information, see [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span></span>

## <a name="run-the-app"></a><span data-ttu-id="3aa24-131">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="3aa24-131">Run the app</span></span>

<span data-ttu-id="3aa24-132">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="3aa24-132">Run the following commands:</span></span>

```dotnetcli
cd aspnetcoreapp
dotnet run
```

<span data-ttu-id="3aa24-133">Después de que el shell de comandos indique que se ha iniciado la aplicación, vaya a [https://localhost:5001](https://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="3aa24-133">After the command shell indicates that the app has started, browse to [https://localhost:5001](https://localhost:5001).</span></span> <span data-ttu-id="3aa24-134">Haga clic en **Aceptar** para aceptar la política de privacidad y de cookies.</span><span class="sxs-lookup"><span data-stu-id="3aa24-134">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="3aa24-135">Esta aplicación no conserva información de carácter personal.</span><span class="sxs-lookup"><span data-stu-id="3aa24-135">This app doesn't keep personal information.</span></span>

## <a name="edit-a-razor-page"></a><span data-ttu-id="3aa24-136">Editar una página de Razor</span><span class="sxs-lookup"><span data-stu-id="3aa24-136">Edit a Razor page</span></span>

<span data-ttu-id="3aa24-137">Abra *Pages/Index.cshtml* y modifique la página con el siguiente marcado resaltado:</span><span class="sxs-lookup"><span data-stu-id="3aa24-137">Open *Pages/Index.cshtml* and modify the page with the following highlighted markup:</span></span>

[!code-cshtml[](sample/index.cshtml?highlight=9)]

<span data-ttu-id="3aa24-138">Vaya a [https://localhost:5001](https://localhost:5001) y confirme que los cambios aparecen reflejados.</span><span class="sxs-lookup"><span data-stu-id="3aa24-138">Browse to [https://localhost:5001](https://localhost:5001), and verify the changes are displayed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3aa24-139">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="3aa24-139">Next steps</span></span>

<span data-ttu-id="3aa24-140">En este tutorial ha aprendido a:</span><span class="sxs-lookup"><span data-stu-id="3aa24-140">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="3aa24-141">Crear un proyecto de aplicación web.</span><span class="sxs-lookup"><span data-stu-id="3aa24-141">Create a web app project.</span></span>
> * <span data-ttu-id="3aa24-142">Confíe en el certificado de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="3aa24-142">Trust the development certificate.</span></span>
> * <span data-ttu-id="3aa24-143">Ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="3aa24-143">Run the project.</span></span>
> * <span data-ttu-id="3aa24-144">Realizar un cambio.</span><span class="sxs-lookup"><span data-stu-id="3aa24-144">Make a change.</span></span>

<span data-ttu-id="3aa24-145">Para obtener más información sobre ASP.NET Core, vea la ruta de aprendizaje recomendada en la introducción:</span><span class="sxs-lookup"><span data-stu-id="3aa24-145">To learn more about ASP.NET Core, see the recommended learning path in the introduction:</span></span>

> [!div class="nextstepaction"]
> <xref:index#recommended-learning-path>
