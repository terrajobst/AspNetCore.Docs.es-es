---
title: Introducción a ASP.NET Core
author: rick-anderson
description: Tutorial breve que crea y ejecuta una aplicación Hola mundo básica mediante ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 09/22/2019
uid: getting-started
ms.openlocfilehash: 798f1ee87c05d886d8991e3f0230c8ebc6341ba8
ms.sourcegitcommit: 73e255e846e414821b8cc20ffa3aec946735cd4e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/03/2019
ms.locfileid: "71925099"
---
# <a name="tutorial-get-started-with-aspnet-core"></a><span data-ttu-id="4c2f2-103">Tutorial: Introducción a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4c2f2-103">Tutorial: Get started with ASP.NET Core</span></span>

<span data-ttu-id="4c2f2-104">En este tutorial se muestra cómo usar la interfaz de la línea de comandos de .NET Core para crear y ejecutar una aplicación web ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4c2f2-104">This tutorial shows how to use the .NET Core command-line interface to create and run an ASP.NET Core web app.</span></span>

<span data-ttu-id="4c2f2-105">Aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="4c2f2-105">You'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4c2f2-106">Crear un proyecto de aplicación web.</span><span class="sxs-lookup"><span data-stu-id="4c2f2-106">Create a web app project.</span></span>
> * <span data-ttu-id="4c2f2-107">Confíe en el certificado de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="4c2f2-107">Trust the development certificate.</span></span>
> * <span data-ttu-id="4c2f2-108">Ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4c2f2-108">Run the app.</span></span>
> * <span data-ttu-id="4c2f2-109">Editar una página de Razor.</span><span class="sxs-lookup"><span data-stu-id="4c2f2-109">Edit a Razor page.</span></span>

<span data-ttu-id="4c2f2-110">Al final, tendrá una aplicación web en funcionamiento ejecutándose en el equipo local.</span><span class="sxs-lookup"><span data-stu-id="4c2f2-110">At the end, you'll have a working web app running on your local machine.</span></span>

![Página principal de aplicación web](_static/home-page.png)

## <a name="prerequisites"></a><span data-ttu-id="4c2f2-112">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="4c2f2-112">Prerequisites</span></span>

[!INCLUDE[](~/includes/3.0-SDK.md)]

## <a name="create-a-web-app-project"></a><span data-ttu-id="4c2f2-113">Crear un proyecto de aplicación web</span><span class="sxs-lookup"><span data-stu-id="4c2f2-113">Create a web app project</span></span>

<span data-ttu-id="4c2f2-114">Abra un shell de comandos y escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="4c2f2-114">Open a command shell, and enter the following command:</span></span>

```dotnetcli
dotnet new webapp -o aspnetcoreapp
```

<span data-ttu-id="4c2f2-115">El comando anterior:</span><span class="sxs-lookup"><span data-stu-id="4c2f2-115">The preceding command:</span></span>

* <span data-ttu-id="4c2f2-116">Crea una nueva aplicación web.</span><span class="sxs-lookup"><span data-stu-id="4c2f2-116">Creates a new web app.</span></span>  
* <span data-ttu-id="4c2f2-117">El parámetro `-o aspnetcoreapp` crea un directorio llamado *aspnetcoreapp* con los archivos de código fuente de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4c2f2-117">The `-o aspnetcoreapp` parameter creates a directory named *aspnetcoreapp* with the source files for the app.</span></span>

### <a name="trust-the-development-certificate"></a><span data-ttu-id="4c2f2-118">Confíe en el certificado de desarrollo</span><span class="sxs-lookup"><span data-stu-id="4c2f2-118">Trust the development certificate</span></span>

<span data-ttu-id="4c2f2-119">Confíe en el certificado de desarrollo HTTPS:</span><span class="sxs-lookup"><span data-stu-id="4c2f2-119">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="4c2f2-120">Windows</span><span class="sxs-lookup"><span data-stu-id="4c2f2-120">Windows</span></span>](#tab/windows)

```dotnetcli
dotnet dev-certs https --trust
```

<span data-ttu-id="4c2f2-121">El comando anterior muestra el siguiente cuadro de diálogo:</span><span class="sxs-lookup"><span data-stu-id="4c2f2-121">The preceding command displays the following dialog:</span></span>

![Cuadro de diálogo de advertencia de seguridad](~/getting-started/_static/cert.png)

<span data-ttu-id="4c2f2-123">Si acepta confiar en el certificado de desarrollo, seleccione **Sí**.</span><span class="sxs-lookup"><span data-stu-id="4c2f2-123">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="4c2f2-124">macOS</span><span class="sxs-lookup"><span data-stu-id="4c2f2-124">macOS</span></span>](#tab/macos)

```dotnetcli
dotnet dev-certs https --trust
```

<span data-ttu-id="4c2f2-125">El comando anterior muestra el siguiente mensaje:</span><span class="sxs-lookup"><span data-stu-id="4c2f2-125">The preceding command displays the following message:</span></span>

<span data-ttu-id="4c2f2-126">*Se ha solicitado que confíe en el certificado de desarrollo HTTPS. Si el certificado todavía no es de confianza, no se ejecutará el comando siguiente:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span><span class="sxs-lookup"><span data-stu-id="4c2f2-126">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`</span></span>

<span data-ttu-id="4c2f2-127">Este comando podría solicitarle la contraseña para instalar el certificado en la cadena de claves del sistema.</span><span class="sxs-lookup"><span data-stu-id="4c2f2-127">This command might prompt you for your password to install the certificate on the system keychain.</span></span> <span data-ttu-id="4c2f2-128">Si acepta confiar en el certificado de desarrollo, escriba la contraseña.</span><span class="sxs-lookup"><span data-stu-id="4c2f2-128">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="4c2f2-129">Linux</span><span class="sxs-lookup"><span data-stu-id="4c2f2-129">Linux</span></span>](#tab/linux)

<span data-ttu-id="4c2f2-130">Para el subsistema Windows para Linux, consulte [Confiar en certificado HTTPS del subsistema de Windows para Linux](xref:security/enforcing-ssl#wsl).</span><span class="sxs-lookup"><span data-stu-id="4c2f2-130">For Windows Subsystem for Linux, see [Trust HTTPS certificate from Windows Subsystem for Linux](xref:security/enforcing-ssl#wsl).</span></span>

<span data-ttu-id="4c2f2-131">Consulte la documentación correspondiente a su distribución Linux sobre cómo confiar en el certificado de desarrollo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="4c2f2-131">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>

---

<span data-ttu-id="4c2f2-132">Para más información, consulte [Confiar en el certificado de desarrollo de ASP.NET Core HTTPS ](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span><span class="sxs-lookup"><span data-stu-id="4c2f2-132">For more information, see [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span></span>

## <a name="run-the-app"></a><span data-ttu-id="4c2f2-133">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="4c2f2-133">Run the app</span></span>

<span data-ttu-id="4c2f2-134">Ejecute los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="4c2f2-134">Run the following commands:</span></span>

```dotnetcli
cd aspnetcoreapp
dotnet watch run
```

<span data-ttu-id="4c2f2-135">Después de que el shell de comandos indique que se ha iniciado la aplicación, vaya a [https://localhost:5001](https://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="4c2f2-135">After the command shell indicates that the app has started, browse to [https://localhost:5001](https://localhost:5001).</span></span>

## <a name="edit-a-razor-page"></a><span data-ttu-id="4c2f2-136">Editar una página de Razor</span><span class="sxs-lookup"><span data-stu-id="4c2f2-136">Edit a Razor page</span></span>

<span data-ttu-id="4c2f2-137">Abra *Pages/Index.cshtml* y modifique y guarde la página con el siguiente marcado resaltado:</span><span class="sxs-lookup"><span data-stu-id="4c2f2-137">Open *Pages/Index.cshtml* and modify and save the page with the following highlighted markup:</span></span>

[!code-cshtml[](sample/index.cshtml?highlight=9)]

<span data-ttu-id="4c2f2-138">Vaya a [https://localhost:5001](https://localhost:5001), actualice la página y confirme que los cambios aparecen reflejados.</span><span class="sxs-lookup"><span data-stu-id="4c2f2-138">Browse to [https://localhost:5001](https://localhost:5001), refresh the page and verify the changes are displayed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4c2f2-139">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="4c2f2-139">Next steps</span></span>

<span data-ttu-id="4c2f2-140">En este tutorial ha aprendido a:</span><span class="sxs-lookup"><span data-stu-id="4c2f2-140">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="4c2f2-141">Crear un proyecto de aplicación web.</span><span class="sxs-lookup"><span data-stu-id="4c2f2-141">Create a web app project.</span></span>
> * <span data-ttu-id="4c2f2-142">Confíe en el certificado de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="4c2f2-142">Trust the development certificate.</span></span>
> * <span data-ttu-id="4c2f2-143">Ejecute el proyecto.</span><span class="sxs-lookup"><span data-stu-id="4c2f2-143">Run the project.</span></span>
> * <span data-ttu-id="4c2f2-144">Realizar un cambio.</span><span class="sxs-lookup"><span data-stu-id="4c2f2-144">Make a change.</span></span>

<span data-ttu-id="4c2f2-145">Para obtener más información sobre ASP.NET Core, vea la ruta de aprendizaje recomendada en la introducción:</span><span class="sxs-lookup"><span data-stu-id="4c2f2-145">To learn more about ASP.NET Core, see the recommended learning path in the introduction:</span></span>

> [!div class="nextstepaction"]
> <xref:index#recommended-learning-path>
