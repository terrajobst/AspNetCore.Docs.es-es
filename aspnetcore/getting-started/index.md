---
title: Introducción a ASP.NET Core
author: rick-anderson
description: Tutorial breve que crea y ejecuta una aplicación Hola mundo básica mediante ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 09/22/2019
uid: getting-started
ms.openlocfilehash: c9cd5e05f52c8bdefa931adc654087dac91e2f05
ms.sourcegitcommit: e644258c95dd50a82284f107b9bf3becbc43b2b2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2019
ms.locfileid: "71317764"
---
# <a name="tutorial-get-started-with-aspnet-core"></a><span data-ttu-id="b5b16-103">Tutorial: Introducción a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b5b16-103">Tutorial: Get started with ASP.NET Core</span></span>

<span data-ttu-id="b5b16-104">En este tutorial se muestra cómo usar la interfaz de la línea de comandos de .NET Core para crear y ejecutar una aplicación web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b5b16-104">This tutorial shows how to use the .NET Core command-line interface to create and run an ASP.NET Core web app.</span></span>

<span data-ttu-id="b5b16-105">Aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="b5b16-105">You'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b5b16-106">Crear un proyecto de aplicación web.</span><span class="sxs-lookup"><span data-stu-id="b5b16-106">Create a web app project.</span></span>
> * <span data-ttu-id="b5b16-107">Confíe en el certificado de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="b5b16-107">Trust the development certificate.</span></span>
> * <span data-ttu-id="b5b16-108">Ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b5b16-108">Run the app.</span></span>
> * <span data-ttu-id="b5b16-109">Editar una página de Razor.</span><span class="sxs-lookup"><span data-stu-id="b5b16-109">Edit a Razor page.</span></span>

<span data-ttu-id="b5b16-110">Al final, tendrá una aplicación web en funcionamiento ejecutándose en el equipo local.</span><span class="sxs-lookup"><span data-stu-id="b5b16-110">At the end, you'll have a working web app running on your local machine.</span></span>

![Página principal de aplicación web](_static/home-page.png)

## <a name="prerequisites"></a><span data-ttu-id="b5b16-112">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="b5b16-112">Prerequisites</span></span>

[!INCLUDE[](~/includes/3.0-SDK.md)]

## <a name="create-a-web-app-project"></a><span data-ttu-id="b5b16-113">Crear un proyecto de aplicación web</span><span class="sxs-lookup"><span data-stu-id="b5b16-113">Create a web app project</span></span>

<span data-ttu-id="b5b16-114">Abra un shell de comandos y escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="b5b16-114">Open a command shell, and enter the following command:</span></span>

```dotnetcli
dotnet new webapp -o aspnetcoreapp
```

<span data-ttu-id="b5b16-115">El comando anterior:</span><span class="sxs-lookup"><span data-stu-id="b5b16-115">The preceding command:</span></span>

* <span data-ttu-id="b5b16-116">Crea una nueva aplicación web.</span><span class="sxs-lookup"><span data-stu-id="b5b16-116">Creates a new web app.</span></span>  
* <span data-ttu-id="b5b16-117">El parámetro `-o` crea un directorio llamado *aspnetcoreapp* con los archivos de código fuente de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b5b16-117">The `-o` parameter creates a directory named *aspnetcoreapp* with the source files for the app.</span></span>

### <a name="trust-the-development-certificate"></a><span data-ttu-id="b5b16-118">Confíe en el certificado de desarrollo</span><span class="sxs-lookup"><span data-stu-id="b5b16-118">Trust the development certificate</span></span>

<span data-ttu-id="b5b16-119">Confíe en el certificado de desarrollo HTTPS:</span><span class="sxs-lookup"><span data-stu-id="b5b16-119">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="b5b16-120">Windows</span><span class="sxs-lookup"><span data-stu-id="b5b16-120">Windows</span></span>](#tab/windows)

```dotnetcli
dotnet dev-certs https --trust
```

<span data-ttu-id="b5b16-121">El comando anterior muestra el siguiente cuadro de diálogo:</span><span class="sxs-lookup"><span data-stu-id="b5b16-121">The preceding command displays the following dialog:</span></span>

![Cuadro de diálogo de advertencia de seguridad](~/getting-started/_static/cert.png)

<span data-ttu-id="b5b16-123">Si acepta confiar en el certificado de desarrollo, seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="b5b16-123">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="b5b16-124">macOS</span><span class="sxs-lookup"><span data-stu-id="b5b16-124">macOS</span></span>](#tab/macos)

```dotnetcli
dotnet dev-certs https --trust
```

<span data-ttu-id="b5b16-125">El comando anterior muestra el siguiente mensaje:</span><span class="sxs-lookup"><span data-stu-id="b5b16-125">The preceding command displays the following message:</span></span>

<span data-ttu-id="b5b16-126">*Se ha solicitado que confíe en el certificado de desarrollo HTTPS. Si el certificado todavía no es de confianza, no se ejecutará el comando siguiente:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span><span class="sxs-lookup"><span data-stu-id="b5b16-126">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`</span></span>

<span data-ttu-id="b5b16-127">Este comando podría solicitarle la contraseña para instalar el certificado en la cadena de claves del sistema.</span><span class="sxs-lookup"><span data-stu-id="b5b16-127">This command might prompt you for your password to install the certificate on the system keychain.</span></span> <span data-ttu-id="b5b16-128">Si acepta confiar en el certificado de desarrollo, escriba la contraseña.</span><span class="sxs-lookup"><span data-stu-id="b5b16-128">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="b5b16-129">Linux</span><span class="sxs-lookup"><span data-stu-id="b5b16-129">Linux</span></span>](#tab/linux)

<span data-ttu-id="b5b16-130">Para el subsistema Windows para Linux, consulte [Confiar en certificado HTTPS del subsistema de Windows para Linux](xref:security/enforcing-ssl#wsl).</span><span class="sxs-lookup"><span data-stu-id="b5b16-130">For Windows Subsystem for Linux, see [Trust HTTPS certificate from Windows Subsystem for Linux](xref:security/enforcing-ssl#wsl).</span></span>

<span data-ttu-id="b5b16-131">Consulte la documentación correspondiente a su distribución Linux sobre cómo confiar en el certificado de desarrollo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b5b16-131">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>

---

<span data-ttu-id="b5b16-132">Para más información, consulte [Confiar en el certificado de desarrollo de ASP.NET Core HTTPS ](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span><span class="sxs-lookup"><span data-stu-id="b5b16-132">For more information, see [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span></span>

## <a name="run-the-app"></a><span data-ttu-id="b5b16-133">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="b5b16-133">Run the app</span></span>

<span data-ttu-id="b5b16-134">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="b5b16-134">Run the following commands:</span></span>

```dotnetcli
cd aspnetcoreapp
dotnet watch run
```

<span data-ttu-id="b5b16-135">Después de que el shell de comandos indique que se ha iniciado la aplicación, vaya a [https://localhost:5001](https://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="b5b16-135">After the command shell indicates that the app has started, browse to [https://localhost:5001](https://localhost:5001).</span></span> <span data-ttu-id="b5b16-136">Haga clic en **Aceptar** para aceptar la política de privacidad y de cookies.</span><span class="sxs-lookup"><span data-stu-id="b5b16-136">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="b5b16-137">Esta aplicación no conserva información de carácter personal.</span><span class="sxs-lookup"><span data-stu-id="b5b16-137">This app doesn't keep personal information.</span></span>

## <a name="edit-a-razor-page"></a><span data-ttu-id="b5b16-138">Editar una página de Razor</span><span class="sxs-lookup"><span data-stu-id="b5b16-138">Edit a Razor page</span></span>

<span data-ttu-id="b5b16-139">Abra *Pages/Index.cshtml* y modifique y guarde la página con el siguiente marcado resaltado:</span><span class="sxs-lookup"><span data-stu-id="b5b16-139">Open *Pages/Index.cshtml* and modify and save the page with the following highlighted markup:</span></span>

[!code-cshtml[](sample/index.cshtml?highlight=9)]

<span data-ttu-id="b5b16-140">Vaya a [https://localhost:5001](https://localhost:5001), actualice la página y confirme que los cambios aparecen reflejados.</span><span class="sxs-lookup"><span data-stu-id="b5b16-140">Browse to [https://localhost:5001](https://localhost:5001), refresh the page and verify the changes are displayed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b5b16-141">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="b5b16-141">Next steps</span></span>

<span data-ttu-id="b5b16-142">En este tutorial ha aprendido a:</span><span class="sxs-lookup"><span data-stu-id="b5b16-142">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b5b16-143">Crear un proyecto de aplicación web.</span><span class="sxs-lookup"><span data-stu-id="b5b16-143">Create a web app project.</span></span>
> * <span data-ttu-id="b5b16-144">Confíe en el certificado de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="b5b16-144">Trust the development certificate.</span></span>
> * <span data-ttu-id="b5b16-145">Ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="b5b16-145">Run the project.</span></span>
> * <span data-ttu-id="b5b16-146">Realizar un cambio.</span><span class="sxs-lookup"><span data-stu-id="b5b16-146">Make a change.</span></span>

<span data-ttu-id="b5b16-147">Para obtener más información sobre ASP.NET Core, vea la ruta de aprendizaje recomendada en la introducción:</span><span class="sxs-lookup"><span data-stu-id="b5b16-147">To learn more about ASP.NET Core, see the recommended learning path in the introduction:</span></span>

> [!div class="nextstepaction"]
> <xref:index#recommended-learning-path>
