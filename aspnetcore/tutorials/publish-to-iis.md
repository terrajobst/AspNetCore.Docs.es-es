---
title: Publicación de una aplicación ASP.NET Core en IIS
author: guardrex
description: Obtenga información sobre cómo hospedar una aplicación ASP.NET Core en un servidor IIS.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/08/2019
uid: tutorials/publish-to-iis
ms.openlocfilehash: c31beae16f46153daac188ab1638e5530584ac88
ms.sourcegitcommit: 776367717e990bdd600cb3c9148ffb905d56862d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2019
ms.locfileid: "68927095"
---
# <a name="publish-an-aspnet-core-app-to-iis"></a><span data-ttu-id="c826e-103">Publicación de una aplicación ASP.NET Core en IIS</span><span class="sxs-lookup"><span data-stu-id="c826e-103">Publish an ASP.NET Core app to IIS</span></span>

<span data-ttu-id="c826e-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="c826e-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="c826e-105">En este tutorial se muestra cómo hospedar una aplicación ASP.NET Core en un servidor IIS.</span><span class="sxs-lookup"><span data-stu-id="c826e-105">This tutorial shows how to host an ASP.NET Core app on an IIS server.</span></span>

<span data-ttu-id="c826e-106">En este tutorial se describen los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="c826e-106">This tutorial covers the following subjects:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c826e-107">Instalación del conjunto de hospedaje de .NET Core en Windows Server</span><span class="sxs-lookup"><span data-stu-id="c826e-107">Install the .NET Core Hosting Bundle on Windows Server.</span></span>
> * <span data-ttu-id="c826e-108">Creación de un sitio de IIS en el Administrador de IIS</span><span class="sxs-lookup"><span data-stu-id="c826e-108">Create an IIS site in IIS Manager.</span></span>
> * <span data-ttu-id="c826e-109">Implementación de una aplicación ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c826e-109">Deploy an ASP.NET Core app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c826e-110">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="c826e-110">Prerequisites</span></span>

* <span data-ttu-id="c826e-111">El [SDK de .NET Core](/dotnet/core/sdk) instalado en el equipo de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="c826e-111">[.NET Core SDK](/dotnet/core/sdk) installed on the development machine.</span></span>
* <span data-ttu-id="c826e-112">Windows Server configurado con el rol de servidor **Servidor web (IIS)** .</span><span class="sxs-lookup"><span data-stu-id="c826e-112">Windows Server configured with the **Web Server (IIS)** server role.</span></span> <span data-ttu-id="c826e-113">Si el servidor no está configurado para hospedar sitios web con IIS, siga las instrucciones de la sección *Configuración de IIS* del artículo <xref:host-and-deploy/iis/index#iis-configuration> y, después, vuelva a este tutorial.</span><span class="sxs-lookup"><span data-stu-id="c826e-113">If your server isn't configured to host websites with IIS, follow the guidance in the *IIS configuration* section of the <xref:host-and-deploy/iis/index#iis-configuration> article and then return to this tutorial.</span></span>

> [!WARNING]
> <span data-ttu-id="c826e-114">**La configuración de IIS y la seguridad de los sitios web implican conceptos que no se describen en este tutorial.**</span><span class="sxs-lookup"><span data-stu-id="c826e-114">**IIS configuration and website security involve concepts that aren't covered by this tutorial.**</span></span> <span data-ttu-id="c826e-115">Consulte las instrucciones de IIS en [la documentación de Microsoft IIS](https://www.iis.net/) y el [artículo de ASP.NET Core sobre hospedaje con IIS](xref:host-and-deploy/iis/index) antes de hospedar aplicaciones de producción en IIS.</span><span class="sxs-lookup"><span data-stu-id="c826e-115">Consult the IIS guidance in the [Microsoft IIS documentation](https://www.iis.net/) and the [ASP.NET Core article on hosting with IIS](xref:host-and-deploy/iis/index) before hosting production apps on IIS.</span></span>
>
> <span data-ttu-id="c826e-116">Entre los escenarios importantes para el hospedaje de IIS que no se describen en este tutorial se incluyen los siguientes:</span><span class="sxs-lookup"><span data-stu-id="c826e-116">Important scenarios for IIS hosting not covered by this tutorial include:</span></span>
>
> * [<span data-ttu-id="c826e-117">Creación de un subárbol del Registro para la protección de datos de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c826e-117">Creation of a registry hive for ASP.NET Core Data Protection</span></span>](xref:host-and-deploy/iis/index#data-protection)
> * [<span data-ttu-id="c826e-118">Configuración de la lista de control de acceso (ACL) del grupo de aplicaciones</span><span class="sxs-lookup"><span data-stu-id="c826e-118">Configuration of the app pool's Access Control List (ACL)</span></span>](xref:host-and-deploy/iis/index#application-pool-identity)
> * <span data-ttu-id="c826e-119">Para centrarse en los conceptos de implementación de IIS, en este tutorial se implementa una aplicación sin seguridad HTTPS configurada en IIS.</span><span class="sxs-lookup"><span data-stu-id="c826e-119">To focus on IIS deployment concepts, this tutorial deploys an app without HTTPS security configured in IIS.</span></span> <span data-ttu-id="c826e-120">Para obtener más información sobre cómo hospedar una aplicación habilitada para el protocolo HTTPS, vea los temas de seguridad en la sección [Recursos adicionales](#additional-resources) de este artículo.</span><span class="sxs-lookup"><span data-stu-id="c826e-120">For more information on hosting an app enabled for HTTPS protocol, see the security topics in the [Additional resources](#additional-resources) section of this article.</span></span> <span data-ttu-id="c826e-121">En el artículo <xref:host-and-deploy/iis/index> se proporciona más información sobre cómo hospedar aplicaciones ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c826e-121">Further guidance for hosting ASP.NET Core apps is provided in the <xref:host-and-deploy/iis/index> article.</span></span>

## <a name="install-the-net-core-hosting-bundle"></a><span data-ttu-id="c826e-122">Instalación del conjunto de hospedaje de .NET Core</span><span class="sxs-lookup"><span data-stu-id="c826e-122">Install the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="c826e-123">Instale el *conjunto de hospedaje de .NET Core* en el servidor IIS.</span><span class="sxs-lookup"><span data-stu-id="c826e-123">Install the *.NET Core Hosting Bundle* on the IIS server.</span></span> <span data-ttu-id="c826e-124">El lote instala .NET Core Runtime, .NET Core Library y el [módulo ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="c826e-124">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span> <span data-ttu-id="c826e-125">El módulo permite que las aplicaciones ASP.NET Core se ejecuten detrás de IIS.</span><span class="sxs-lookup"><span data-stu-id="c826e-125">The module allows ASP.NET Core apps to run behind IIS.</span></span> <span data-ttu-id="c826e-126">Si el sistema no tiene conexión a Internet, obtenga e instale [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) antes de instalar el conjunto de hospedaje de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="c826e-126">If the system doesn't have an Internet connection, obtain and install the [Microsoft Visual C++ 2015 Redistributable](https://www.microsoft.com/download/details.aspx?id=53840) before installing the .NET Core Hosting Bundle.</span></span>

<span data-ttu-id="c826e-127">Descargue al instalador mediante el vínculo siguiente:</span><span class="sxs-lookup"><span data-stu-id="c826e-127">Download the installer using the following link:</span></span>

[<span data-ttu-id="c826e-128">Instalador del conjunto de hospedaje de .NET Core actual (descarga directa)</span><span class="sxs-lookup"><span data-stu-id="c826e-128">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://www.microsoft.com/net/permalink/dotnetcore-current-windows-runtime-bundle-installer)

1. <span data-ttu-id="c826e-129">Ejecute el instalador en el servidor IIS.</span><span class="sxs-lookup"><span data-stu-id="c826e-129">Run the installer on the IIS server.</span></span>

1. <span data-ttu-id="c826e-130">Reinicie el servidor o ejecute **net stop was /y**, seguido de **net start w3svc** en un shell de comandos.</span><span class="sxs-lookup"><span data-stu-id="c826e-130">Restart the server or execute **net stop was /y** followed by **net start w3svc** in a command shell.</span></span>

## <a name="create-the-iis-site"></a><span data-ttu-id="c826e-131">Creación del sitio de IIS</span><span class="sxs-lookup"><span data-stu-id="c826e-131">Create the IIS site</span></span>

1. <span data-ttu-id="c826e-132">En el servidor IIS, cree una carpeta para que contenga los archivos y las carpetas publicados de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c826e-132">On the IIS server, create a folder to contain the app's published folders and files.</span></span> <span data-ttu-id="c826e-133">En un paso posterior, la ruta de acceso de la carpeta se proporciona a IIS como la ruta de acceso física a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c826e-133">In a following step, the folder's path is provided to IIS as the physical path to the app.</span></span>

1. <span data-ttu-id="c826e-134">En Administrador de IIS, abra el nodo del servidor en el panel **Conexiones**.</span><span class="sxs-lookup"><span data-stu-id="c826e-134">In IIS Manager, open the server's node in the **Connections** panel.</span></span> <span data-ttu-id="c826e-135">Haga clic con el botón derecho en la carpeta **Sitios**.</span><span class="sxs-lookup"><span data-stu-id="c826e-135">Right-click the **Sites** folder.</span></span> <span data-ttu-id="c826e-136">Haga clic en **Agregar sitio web** en el menú contextual.</span><span class="sxs-lookup"><span data-stu-id="c826e-136">Select **Add Website** from the contextual menu.</span></span>

1. <span data-ttu-id="c826e-137">Proporcione el **Nombre del sitio** y establezca la **Ruta de acceso física** a la carpeta de implementación de la aplicación que ha creado.</span><span class="sxs-lookup"><span data-stu-id="c826e-137">Provide a **Site name** and set the **Physical path** to the app's deployment folder that you created.</span></span> <span data-ttu-id="c826e-138">Proporcione la configuración de **Enlace** y seleccione **Aceptar** para crear el sitio web.</span><span class="sxs-lookup"><span data-stu-id="c826e-138">Provide the **Binding** configuration and create the website by selecting **OK**.</span></span>

## <a name="create-an-aspnet-core-razor-pages-app"></a><span data-ttu-id="c826e-139">Creación de una aplicación Razor Pages de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c826e-139">Create an ASP.NET Core Razor Pages app</span></span>

<span data-ttu-id="c826e-140">Siga el tutorial <xref:getting-started> para crear una aplicación Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="c826e-140">Follow the <xref:getting-started> tutorial to create a Razor Pages app.</span></span>

## <a name="publish-and-deploy-the-app"></a><span data-ttu-id="c826e-141">Publicar e implementar la aplicación</span><span class="sxs-lookup"><span data-stu-id="c826e-141">Publish and deploy the app</span></span>

<span data-ttu-id="c826e-142">*Publicar una aplicación* significa generar una aplicación compilada que se puede hospedar en un servidor.</span><span class="sxs-lookup"><span data-stu-id="c826e-142">*Publish an app* means to produce a compiled app that can be hosted by a server.</span></span> <span data-ttu-id="c826e-143">*Implementar una aplicación* significa trasladar la aplicación publicada a un sistema de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="c826e-143">*Deploy an app* means to move the published app to a hosting system.</span></span> <span data-ttu-id="c826e-144">El paso de publicación lo controla el [SDK de .NET Core](/dotnet/core/sdk), mientras que el paso de implementación se puede controlar mediante distintos enfoques.</span><span class="sxs-lookup"><span data-stu-id="c826e-144">The publish step is handled by the [.NET Core SDK](/dotnet/core/sdk), while the deployment step can be handled by a variety of approaches.</span></span> <span data-ttu-id="c826e-145">En este tutorial se adopta el enfoque de implementación de *carpetas*, donde:</span><span class="sxs-lookup"><span data-stu-id="c826e-145">This tutorial adopts the *folder* deployment approach, where:</span></span>

* <span data-ttu-id="c826e-146">La aplicación se publica en una carpeta.</span><span class="sxs-lookup"><span data-stu-id="c826e-146">The app is published to a folder.</span></span>
* <span data-ttu-id="c826e-147">El contenido de la carpeta se mueve a la carpeta del sitio de IIS (la **ruta de acceso física** al sitio en el Administrador de IIS).</span><span class="sxs-lookup"><span data-stu-id="c826e-147">The folder's contents are moved to the IIS site's folder (the **Physical path** to the site in IIS Manager).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c826e-148">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c826e-148">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="c826e-149">Desde el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="c826e-149">Right-click on the project in **Solution Explorer** and select **Publish**.</span></span>
1. <span data-ttu-id="c826e-150">En el cuadro de diálogo **Elegir un destino de publicación**, seleccione la opción de publicación **Carpeta**.</span><span class="sxs-lookup"><span data-stu-id="c826e-150">In the **Pick a publish target** dialog, select the **Folder** publish option.</span></span>
1. <span data-ttu-id="c826e-151">Establezca la ruta de acceso **Recurso compartido de archivos o carpeta**.</span><span class="sxs-lookup"><span data-stu-id="c826e-151">Set the **Folder or File Share** path.</span></span>
   * <span data-ttu-id="c826e-152">Si ha creado una carpeta para el sitio de IIS que está disponible en el equipo de desarrollo como un recurso compartido de red, proporcione la ruta de acceso al recurso compartido.</span><span class="sxs-lookup"><span data-stu-id="c826e-152">If you created a folder for the IIS site that's available on the development machine as a network share, provide the path to the share.</span></span> <span data-ttu-id="c826e-153">El usuario actual debe tener acceso de escritura para publicar en el recurso compartido.</span><span class="sxs-lookup"><span data-stu-id="c826e-153">The current user must have write access to publish to the share.</span></span>
   * <span data-ttu-id="c826e-154">Si no puede realizar la implementación directamente en la carpeta del sitio de IIS en el servidor IIS, publique en una carpeta de un medio extraíble y mueva físicamente la aplicación publicada a la carpeta del sitio de IIS en el servidor, que es la **ruta de acceso física** del sitio en el Administrador de IIS.</span><span class="sxs-lookup"><span data-stu-id="c826e-154">If you're unable to deploy directly to the IIS site folder on the IIS server, publish to a folder on removeable media and physically move the published app to the IIS site folder on the server, which is the site's **Physical path** in IIS Manager.</span></span> <span data-ttu-id="c826e-155">Mueva el contenido de la carpeta *bin/Release/{PLATAFORMA DE DESTINO}/publish* a la carpeta del sitio de IIS en el servidor, que es la **ruta de acceso física** del sitio en el Administrador de IIS.</span><span class="sxs-lookup"><span data-stu-id="c826e-155">Move the contents of the *bin/Release/{TARGET FRAMEWORK}/publish* folder to the IIS site folder on the server, which is the site's **Physical path** in IIS Manager.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c826e-156">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="c826e-156">.NET Core CLI</span></span>](#tab/netcore-cli)

1. <span data-ttu-id="c826e-157">En un shell de comandos, publique la aplicación en Configuración de versión con el comando [dotnet publish](/dotnet/core/tools/dotnet-publish):</span><span class="sxs-lookup"><span data-stu-id="c826e-157">In a command shell, publish the app in Release configuration with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command:</span></span>

   ```console
   dotnet publish --configuration Release
   ```

1. <span data-ttu-id="c826e-158">Mueva el contenido de la carpeta *bin/Release/{PLATAFORMA DE DESTINO}/publish* a la carpeta del sitio de IIS en el servidor, que es la **ruta de acceso física** del sitio en el Administrador de IIS.</span><span class="sxs-lookup"><span data-stu-id="c826e-158">Move the contents of the *bin/Release/{TARGET FRAMEWORK}/publish* folder to the IIS site folder on the server, which is the site's **Physical path** in IIS Manager.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c826e-159">Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="c826e-159">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="c826e-160">Haga clic con el botón derecho en el proyecto en **Solución** y seleccione **Publicar** > **Publicación en carpeta**.</span><span class="sxs-lookup"><span data-stu-id="c826e-160">Right-click on the project in **Solution** and select **Publish** > **Publish to Folder**.</span></span>
1. <span data-ttu-id="c826e-161">Establezca la ruta de acceso **Elegir una carpeta**.</span><span class="sxs-lookup"><span data-stu-id="c826e-161">Set the **Choose a folder** path.</span></span>
   * <span data-ttu-id="c826e-162">Si ha creado una carpeta para el sitio de IIS que está disponible en el equipo de desarrollo como un recurso compartido de red, proporcione la ruta de acceso al recurso compartido.</span><span class="sxs-lookup"><span data-stu-id="c826e-162">If you created a folder for the IIS site that's available on the development machine as a network share, provide the path to the share.</span></span> <span data-ttu-id="c826e-163">El usuario actual debe tener acceso de escritura para publicar en el recurso compartido.</span><span class="sxs-lookup"><span data-stu-id="c826e-163">The current user must have write access to publish to the share.</span></span>
   * <span data-ttu-id="c826e-164">Si no puede realizar la implementación directamente en la carpeta del sitio de IIS en el servidor IIS, publique en una carpeta de un medio extraíble y mueva físicamente la aplicación publicada a la carpeta del sitio de IIS en el servidor, que es la **ruta de acceso física** del sitio en el Administrador de IIS.</span><span class="sxs-lookup"><span data-stu-id="c826e-164">If you're unable to deploy directly to the IIS site folder on the IIS server, publish to a folder on removeable media and physically move the published app to the IIS site folder on the server, which is the site's **Physical path** in IIS Manager.</span></span> <span data-ttu-id="c826e-165">Mueva el contenido de la carpeta *bin/Release/{PLATAFORMA DE DESTINO}/publish* a la carpeta del sitio de IIS en el servidor, que es la **ruta de acceso física** del sitio en el Administrador de IIS.</span><span class="sxs-lookup"><span data-stu-id="c826e-165">Move the contents of the *bin/Release/{TARGET FRAMEWORK}/publish* folder to the IIS site folder on the server, which is the site's **Physical path** in IIS Manager.</span></span>

---

## <a name="browse-the-website"></a><span data-ttu-id="c826e-166">Examinar el sitio web</span><span class="sxs-lookup"><span data-stu-id="c826e-166">Browse the website</span></span>

<span data-ttu-id="c826e-167">Se puede acceder a la aplicación en un explorador después de que reciba la primera solicitud.</span><span class="sxs-lookup"><span data-stu-id="c826e-167">The app is accessible in a browser after it receives the first request.</span></span> <span data-ttu-id="c826e-168">Realice una solicitud a la aplicación en el enlace de punto de conexión que ha establecido en el Administrador de IIS para el sitio.</span><span class="sxs-lookup"><span data-stu-id="c826e-168">Make a request to the app at the endpoint binding that you established in IIS Manager for the site.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c826e-169">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="c826e-169">Next steps</span></span>

<span data-ttu-id="c826e-170">En este tutorial ha aprendido a:</span><span class="sxs-lookup"><span data-stu-id="c826e-170">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c826e-171">Instalación del conjunto de hospedaje de .NET Core en Windows Server</span><span class="sxs-lookup"><span data-stu-id="c826e-171">Install the .NET Core Hosting Bundle on Windows Server.</span></span>
> * <span data-ttu-id="c826e-172">Creación de un sitio de IIS en el Administrador de IIS</span><span class="sxs-lookup"><span data-stu-id="c826e-172">Create an IIS site in IIS Manager.</span></span>
> * <span data-ttu-id="c826e-173">Implementación de una aplicación ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c826e-173">Deploy an ASP.NET Core app.</span></span>

<span data-ttu-id="c826e-174">Para obtener más información sobre cómo hospedar aplicaciones ASP.NET Core en IIS, vea el artículo Información general de IIS:</span><span class="sxs-lookup"><span data-stu-id="c826e-174">To learn more about hosting ASP.NET Core apps on IIS, see the IIS Overview article:</span></span>

> [!div class="nextstepaction"]
> <xref:host-and-deploy/iis/index>

## <a name="additional-resources"></a><span data-ttu-id="c826e-175">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="c826e-175">Additional resources</span></span>

### <a name="articles-in-the-aspnet-core-documentation-set"></a><span data-ttu-id="c826e-176">Artículos en el conjunto de documentación de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c826e-176">Articles in the ASP.NET Core documentation set</span></span>

* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/directory-structure>
* <xref:test/troubleshoot-azure-iis>
* <xref:security/enforcing-ssl>

### <a name="articles-pertaining-to-aspnet-core-app-deployment"></a><span data-ttu-id="c826e-177">Artículos relacionados con la implementación de aplicaciones ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c826e-177">Articles pertaining to ASP.NET Core app deployment</span></span>

* <xref:tutorials/publish-to-azure-webapp-using-vs>
* <xref:tutorials/publish-to-azure-webapp-using-vscode>
* <xref:host-and-deploy/visual-studio-publish-profiles>
* [<span data-ttu-id="c826e-178">Publicación de una aplicación web en una carpeta mediante Visual Studio para Mac</span><span class="sxs-lookup"><span data-stu-id="c826e-178">Publish a Web app to a folder using Visual Studio for Mac</span></span>](/visualstudio/mac/publish-folder)

### <a name="articles-on-iis-https-configuration"></a><span data-ttu-id="c826e-179">Artículos sobre la configuración HTTPS de IIS</span><span class="sxs-lookup"><span data-stu-id="c826e-179">Articles on IIS HTTPS configuration</span></span>

* [<span data-ttu-id="c826e-180">Configuración de SSL en el Administrador de IIS</span><span class="sxs-lookup"><span data-stu-id="c826e-180">Configuring SSL in IIS Manager</span></span>](/iis/manage/configuring-security/configuring-ssl-in-iis-manager)
* [<span data-ttu-id="c826e-181">Procedimientos para configurar SSL en IIS</span><span class="sxs-lookup"><span data-stu-id="c826e-181">How to Set Up SSL on IIS</span></span>](/iis/manage/configuring-security/how-to-set-up-ssl-on-iis)

### <a name="articles-on-iis-and-windows-server"></a><span data-ttu-id="c826e-182">Artículos sobre IIS y Windows Server</span><span class="sxs-lookup"><span data-stu-id="c826e-182">Articles on IIS and Windows Server</span></span>

* [<span data-ttu-id="c826e-183">Sitio oficial de Microsoft IIS</span><span class="sxs-lookup"><span data-stu-id="c826e-183">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="c826e-184">Biblioteca de contenido técnico de Windows Server</span><span class="sxs-lookup"><span data-stu-id="c826e-184">Windows Server technical content library</span></span>](/windows-server/windows-server)
