---
uid: whitepapers/side-by-side-with-10
title: La ejecución de .NET Framework 1.0 y 1.1 de ASP.NET Side-by-Side | Microsoft Docs
author: rick-anderson
description: Estas notas del producto describen cómo instalar .NET 1.0 y 1.1 de .NET en el equipo, lo que permite a una aplicación Web ASP.NET para ejecutarse en cualquier versión de los fotogramas...
ms.author: aspnetcontent
ms.date: 02/10/2010
ms.assetid: bdea2003-e964-4db5-9092-d56cc7560616
msc.legacyurl: /whitepapers/side-by-side-with-10
msc.type: content
ms.openlocfilehash: 1018845e3d2c6603732bfbbde78f4a9183e49d5d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808400"
---
<a name="aspnet-side-by-side-execution-of-net-framework-10-and-11"></a><span data-ttu-id="8aea5-103">Ejecución de .NET Framework 1.0 y 1.1 de ASP.NET Side-by-Side</span><span class="sxs-lookup"><span data-stu-id="8aea5-103">ASP.NET Side-by-Side Execution of .NET Framework 1.0 and 1.1</span></span>
====================
> <span data-ttu-id="8aea5-104">Estas notas del producto describen cómo instalar .NET 1.0 y 1.1 de .NET en el equipo, lo que permite a una aplicación Web ASP.NET para ejecutarse en cualquier versión de framework.</span><span class="sxs-lookup"><span data-stu-id="8aea5-104">This whitepaper describes how to install both .NET 1.0 and .NET 1.1 on your machine, allowing an ASP.NET Web application to run on either version of the framework.</span></span>
> 
> <span data-ttu-id="8aea5-105">Se aplica a ASP.NET 1.0 y 1.1 de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8aea5-105">Applies to ASP.NET 1.0 and ASP.NET 1.1.</span></span>


<span data-ttu-id="8aea5-106">En ASP.NET, se dice que las aplicaciones se ejecutan en paralelo cuando se instalan en el mismo equipo, pero usar diferentes versiones de .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="8aea5-106">In ASP.NET, applications are said to be running side by side when they are installed on the same computer, but use different versions of the .NET Framework.</span></span> <span data-ttu-id="8aea5-107">El siguiente tema describe cómo configurar las aplicaciones de ASP.NET para la ejecución en paralelo y proporciona pasos detallados para:</span><span class="sxs-lookup"><span data-stu-id="8aea5-107">The following topic describes how to configure ASP.NET applications for side-by-side execution and provides detailed steps to:</span></span>

- [<span data-ttu-id="8aea5-108">Mantener la asignación de la aplicación Web a la versión 1.0 de .NET Framework durante la instalación</span><span class="sxs-lookup"><span data-stu-id="8aea5-108">Maintain your Web application's mapping to .NET Framework version 1.0 during installation</span></span>](#1)
- [<span data-ttu-id="8aea5-109">Asignar una aplicación Web a una versión específica de .NET Framework</span><span class="sxs-lookup"><span data-stu-id="8aea5-109">Map a Web application to a specific version of the .NET Framework</span></span>](#2)
- [<span data-ttu-id="8aea5-110">Busque la versión de .NET Framework que está utilizando un sitio Web</span><span class="sxs-lookup"><span data-stu-id="8aea5-110">Find the version of the .NET Framework that a Web site is using</span></span>](#3)

<span data-ttu-id="8aea5-111">Tradicionalmente, cuando se actualiza un componente o aplicación en un equipo, la versión anterior está eliminada y reemplazada por la versión más reciente.</span><span class="sxs-lookup"><span data-stu-id="8aea5-111">Traditionally, when a component or application is updated on a computer, the older version is removed and replaced with the newer version.</span></span> <span data-ttu-id="8aea5-112">Si la nueva versión no es compatible con la versión anterior, esto suele afectar a otras aplicaciones que usan el componente o aplicación.</span><span class="sxs-lookup"><span data-stu-id="8aea5-112">If the new version is not compatible with the previous version, this usually breaks other applications that use the component or application.</span></span> <span data-ttu-id="8aea5-113">.NET Framework proporciona compatibilidad para la ejecución en paralelo, lo que permite varias versiones de un ensamblado o la aplicación se instale en el mismo equipo al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="8aea5-113">The .NET Framework provides support for side-by-side execution, which allows multiple versions of an assembly or application to be installed on the same computer at the same time.</span></span> <span data-ttu-id="8aea5-114">Dado que varias versiones pueden instalarse al mismo tiempo, las aplicaciones administradas pueden seleccionar qué versión debe utilizar sin que afecte a las aplicaciones que usan una versión diferente.</span><span class="sxs-lookup"><span data-stu-id="8aea5-114">Because multiple versions can be installed simultaneously, managed applications can select which version to use without affecting applications that use a different version.</span></span>

<span data-ttu-id="8aea5-115">De forma predeterminada, durante la instalación de .NET Framework versión 1.1, todas las aplicaciones ASP.NET existentes se vuelven a configurar automáticamente para utilizar la versión más reciente de .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="8aea5-115">By default, during the installation of the .NET Framework version 1.1, all existing ASP.NET applications are automatically reconfigured to use the latest version of the .NET Framework.</span></span> <span data-ttu-id="8aea5-116">Si no desea que sus aplicaciones ASP.NET para .NET Framework 1.1 de forma predeterminada, haga clic en [aquí](#1) para saber cómo evitarlo durante la instalación.</span><span class="sxs-lookup"><span data-stu-id="8aea5-116">If you do not want your ASP.NET applications to default to .NET Framework 1.1, click [here](#1) to learn how to prevent this during installation.</span></span>

<span data-ttu-id="8aea5-117">Si actualiza su servidor Web a .NET Framework 1.1 y desea que una o varias aplicaciones Web para ejecutar .NET Framework 1.0, deberá actualizar la asignación de Script de Internet Information Services (IIS).</span><span class="sxs-lookup"><span data-stu-id="8aea5-117">If you update your Web server to .NET Framework 1.1 and want one or more Web applications to run .NET Framework 1.0, you need to update the Internet Information Services (IIS) Script Map.</span></span> <span data-ttu-id="8aea5-118">La asignación de script es el mecanismo para asignar la extensión de archivo .aspx para una aplicación Web específica a una versión de .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="8aea5-118">The script mapping is the mechanism to map the .aspx file extension for a specific Web application to a version of the .NET Framework.</span></span> <span data-ttu-id="8aea5-119">Haga clic en [aquí](#2) para obtener información sobre cómo asignar una aplicación Web a una versión específica de .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="8aea5-119">Click [here](#2) to learn how to map a Web application to a specific version of the .NET Framework.</span></span>

<span data-ttu-id="8aea5-120">Puede usar el Administrador de información de Internet o la herramienta de registro de IIS de ASP.NET (Aspnet\_regiis.exe) para buscar la versión de .NET Framework se está ejecutando una aplicación Web concreta.</span><span class="sxs-lookup"><span data-stu-id="8aea5-120">You can use the Internet Information Manger or the ASP.NET IIS Registration Tool (Aspnet\_regiis.exe) to find which .NET Framework version is running a particular Web application.</span></span> <span data-ttu-id="8aea5-121">Haga clic en [aquí](#3) para obtener información sobre cómo buscar la versión de .NET Framework que está utilizando un sitio Web.</span><span class="sxs-lookup"><span data-stu-id="8aea5-121">Click [here](#3) to learn how to find the version of the .NET Framework that a Web site is using.</span></span>

<span data-ttu-id="8aea5-122">Una consideración de importación al migrar a .NET Framework 1.1 es que cada versión de .NET Framework usa su propio archivo Machine.config.</span><span class="sxs-lookup"><span data-stu-id="8aea5-122">One import consideration when migrating to .NET Framework 1.1 is that each version of the .NET Framework uses its own Machine.config file.</span></span> <span data-ttu-id="8aea5-123">Como resultado, si un administrador Web ha realizado cambios en el archivo Machine.config, esos cambios se deben migrar al archivo Machine.config de .NET Framework 1.1.</span><span class="sxs-lookup"><span data-stu-id="8aea5-123">As a result, if a Web administrator has made changes to the Machine.config file, those changes must be migrated to the .NET Framework 1.1 Machine.config file.</span></span>

<a id="1"></a>

## <a name="maintaining-your-web-applications-mapping-to-net-framework-10-during-installation"></a><span data-ttu-id="8aea5-124">Mantener la asignación de la aplicación Web a .NET Framework 1.0 durante la instalación</span><span class="sxs-lookup"><span data-stu-id="8aea5-124">Maintaining your Web application's mapping to .NET Framework 1.0 during installation</span></span>

<span data-ttu-id="8aea5-125">De forma predeterminada, todas las aplicaciones ASP.NET existentes se vuelven a configurar automáticamente durante la instalación para usar la versión más reciente de .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="8aea5-125">By default, all existing ASP.NET applications are automatically reconfigured during installation to use the newer version of the .NET Framework.</span></span> <span data-ttu-id="8aea5-126">Con la versión más reciente de .NET Framework, las aplicaciones pueden aprovechar al máximo las mejoras y nuevas características incluidas en la nueva versión.</span><span class="sxs-lookup"><span data-stu-id="8aea5-126">Using the newer version of the .NET Framework, applications can take full advantage of improvements and new features included in the new release.</span></span> <span data-ttu-id="8aea5-127">Al mismo tiempo, el Administrador de Web, que es posible que desee un control granular sobre qué aplicaciones se actualizan, puede impedir la reasignación automática de todas las aplicaciones ASP.NET existentes durante la instalación de .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="8aea5-127">At the same time, the Web administrator, who might want granular control over which applications are updated, can prevent the automatic remapping of all existing ASP.NET applications during installation of the .NET Framework.</span></span>

<span data-ttu-id="8aea5-128">Para evitar la reasignación automática de toda la aplicación ASP.NET a la versión más reciente de .NET Framework, el administrador Web puede usar la opción de línea de comandos /noaspupgrade con el programa de instalación Dotnetfx.exe.</span><span class="sxs-lookup"><span data-stu-id="8aea5-128">To prevent the automatic remapping of the entire ASP.NET application to the newer version of the .NET Framework, the Web administrator can use the /noaspupgrade command-line option with the Dotnetfx.exe setup program.</span></span>

<span data-ttu-id="8aea5-129">**Para evitar la reasignación del total de la aplicación ASP.NET a la versión más reciente**</span><span class="sxs-lookup"><span data-stu-id="8aea5-129">**To prevent total remapping of ASP.NET application to newer version**</span></span>

1. <span data-ttu-id="8aea5-130">Vaya a **iniciar**.</span><span class="sxs-lookup"><span data-stu-id="8aea5-130">Go to **Start**.</span></span>
2. <span data-ttu-id="8aea5-131">Haga clic en **ejecutar**.</span><span class="sxs-lookup"><span data-stu-id="8aea5-131">Click on **run**.</span></span>
3. <span data-ttu-id="8aea5-132">Escriba **cmd**.</span><span class="sxs-lookup"><span data-stu-id="8aea5-132">Type **cmd**.</span></span>
4. <span data-ttu-id="8aea5-133">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="8aea5-133">Click **OK**.</span></span>  
  
    ![](side-by-side-with-10/_static/image1.gif)
5. <span data-ttu-id="8aea5-134">Desde el símbolo del sistema, escriba la línea siguiente para iniciar la instalación de .NET Framework: **/c: Dotnetfx.exe "instalar /noaspupgrade?**.</span><span class="sxs-lookup"><span data-stu-id="8aea5-134">From the command prompt, type the following line to start the installation of the .NET Framework: **Dotnetfx.exe /c:"install /noaspupgrade?**.</span></span>  
  
    ![](side-by-side-with-10/_static/image2.gif)
6. <span data-ttu-id="8aea5-135">Haga clic en **Sí** en el programa de instalación de Microsoft .NET Framework 1.1.</span><span class="sxs-lookup"><span data-stu-id="8aea5-135">Click **Yes** in the Microsoft .NET Framework 1.1 Setup.</span></span> <span data-ttu-id="8aea5-136">Esta acción iniciará el proceso de instalación de .NET Framework 1.1.</span><span class="sxs-lookup"><span data-stu-id="8aea5-136">This will start the setup process of the .NET Framework 1.1.</span></span>  
  
    ![](side-by-side-with-10/_static/image3.gif)

<a id="2"></a>

## <a name="map-a-web-application-to-a-specific-version-of-the-net-framework"></a><span data-ttu-id="8aea5-137">Asignar una aplicación Web a una versión específica de .NET Framework</span><span class="sxs-lookup"><span data-stu-id="8aea5-137">Map a Web application to a specific version of the .NET Framework</span></span>

<span data-ttu-id="8aea5-138">Cada versión de .NET Framework incluye una versión de la herramienta de registro de IIS de ASP.NET (Aspnet\_regiis.exe).</span><span class="sxs-lookup"><span data-stu-id="8aea5-138">Each version of the .NET Framework includes a version of the ASP.NET IIS Registration Tool (Aspnet\_regiis.exe).</span></span> <span data-ttu-id="8aea5-139">Esta herramienta permite a los administradores especificar que una aplicación Web se ejecute en una versión concreta de .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="8aea5-139">This tool enables administrators to specify that a Web application be run under a particular version of the .NET Framework.</span></span> <span data-ttu-id="8aea5-140">Esto se conoce como asignación de una aplicación Web a una versión de .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="8aea5-140">This is referred to as mapping a Web application to a version of the .NET Framework.</span></span> <span data-ttu-id="8aea5-141">Los administradores deben seleccionar Aspnet\_regiis.exe que corresponde a la versión de .NET Framework que se asociará con la aplicación Web.</span><span class="sxs-lookup"><span data-stu-id="8aea5-141">Administrators must select the Aspnet\_regiis.exe that corresponds to the version of the .NET Framework that will be associated with the Web application.</span></span> <span data-ttu-id="8aea5-142">Por ejemplo, un administrador que desea especificar que un sitio Web usa .NET Framework 1.1 debe usar Aspnet\_regiis.exe que viene con .NET Framework 1.1.</span><span class="sxs-lookup"><span data-stu-id="8aea5-142">For example, an administrator who wants to specify that a Web site use .NET Framework 1.1 must use the Aspnet\_regiis.exe that comes with .NET Framework 1.1.</span></span>

<span data-ttu-id="8aea5-143">Aspnet\_regiis.exe para la versión 1.0 se encuentra en:</span><span class="sxs-lookup"><span data-stu-id="8aea5-143">The Aspnet\_regiis.exe for version 1.0 is located at:</span></span>

- <span data-ttu-id="8aea5-144">C:\Windows\Microsoft.NET\Framework\**v1.0.3705**\aspnet\_regiis</span><span class="sxs-lookup"><span data-stu-id="8aea5-144">C:\WINDOWS\Microsoft.NET\Framework\**v1.0.3705**\aspnet\_regiis</span></span>

<span data-ttu-id="8aea5-145">Aspnet\_regiis.exe versión 1,1 se encuentra en:</span><span class="sxs-lookup"><span data-stu-id="8aea5-145">The Aspnet\_regiis.exe for version 1,1 is located at:</span></span>

- <span data-ttu-id="8aea5-146">C:\Windows\Microsoft.NET\Framework\**v1.1.4322**\aspnet\_regiis</span><span class="sxs-lookup"><span data-stu-id="8aea5-146">C:\WINDOWS\Microsoft.NET\Framework\**v1.1.4322**\aspnet\_regiis</span></span>

<span data-ttu-id="8aea5-147">Aspnet\_regiis.exe proporciona dos opciones para la asignación de una aplicación Web de la secuencia de comandos:</span><span class="sxs-lookup"><span data-stu-id="8aea5-147">The Aspnet\_regiis.exe provides two options for script mapping a Web application:</span></span>

- <span data-ttu-id="8aea5-148">**-s** establece la asignación de script en la ruta de acceso y en su elemento secundario directorios.</span><span class="sxs-lookup"><span data-stu-id="8aea5-148">**-s** sets the script map in the path and in its child directories.</span></span>
- <span data-ttu-id="8aea5-149">**-sn** establece la asignación de script en la ruta de acceso solo.</span><span class="sxs-lookup"><span data-stu-id="8aea5-149">**-sn** sets the script map in the path only.</span></span>

<span data-ttu-id="8aea5-150">La ruta de acceso define la ruta de los metadatos de Web application IIS, que se define en el formulario de W3SVC/ROOT / {WebSiteNumber} / {aplicación\_nombre}.</span><span class="sxs-lookup"><span data-stu-id="8aea5-150">The path defines the Web application IIS metadata path, which is defined in the form of W3SVC/ROOT/{WebSiteNumber}/{Application\_Name}.</span></span> <span data-ttu-id="8aea5-151">Por ejemplo, para una aplicación Web denominada Portal ubicado en el sitio Web predeterminado, la ruta de acceso de metabase es W3SVC/1/ROOT/Portal.</span><span class="sxs-lookup"><span data-stu-id="8aea5-151">For example, for a Web application called Portal located under the default Web site, the metabase path is W3SVC/1/ROOT/Portal.</span></span>

![](side-by-side-with-10/_static/image4.gif)

<span data-ttu-id="8aea5-152">Tenga en cuenta que también puede usar una herramienta denominada Editor de la Metabase para obtener la ruta de acceso de metabase.</span><span class="sxs-lookup"><span data-stu-id="8aea5-152">Note You can also use a tool called the Metabase Editor to get the metabase path.</span></span> <span data-ttu-id="8aea5-153">Esta herramienta se puede descargar desde el sitio de Microsoft Support en [ https://support.microsoft.com/default.aspx?scid=kb; en-us; 232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)</span><span class="sxs-lookup"><span data-stu-id="8aea5-153">You can download this tool from the Microsoft Support site at [https://support.microsoft.com/default.aspx?scid=kb;en-us;232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)</span></span>

- <span data-ttu-id="8aea5-154">Ejecute Aspnet\_mapa y su subapplication de secuencias de comandos regiis.exe -s W3SVC/1/ROOT/el Portal para actualizar el portal de IIS.</span><span class="sxs-lookup"><span data-stu-id="8aea5-154">Run Aspnet\_regiis.exe -s W3SVC/1/ROOT/Portal to update the portal IIS script map and its subapplication.</span></span>  
  
    ![](side-by-side-with-10/_static/image5.gif)

- <span data-ttu-id="8aea5-155">Ejecute Aspnet\_regiis.exe -sn W3SVC/1/ROOT/el Portal para actualizar la secuencia de comandos IIS portal se asignan, sin que afecte a las aplicaciones en el portal? subdirectorios s.</span><span class="sxs-lookup"><span data-stu-id="8aea5-155">Run Aspnet\_regiis.exe -sn W3SVC/1/ROOT/Portal to update the portal IIS script map, without affecting applications in the portal?s subdirectories.</span></span>  
  
    ![](side-by-side-with-10/_static/image6.gif)

<a id="3"></a>

## <a name="find-the-net-framework-version-that-a-web-application-is-using"></a><span data-ttu-id="8aea5-156">Busque la versión de .NET Framework que está utilizando una aplicación Web</span><span class="sxs-lookup"><span data-stu-id="8aea5-156">Find the .NET Framework version that a Web application is using</span></span>

<span data-ttu-id="8aea5-157">Un administrador puede usar el Administrador de servicios de Internet para encontrar la versión de .NET Framework ejecuta un sitio Web.</span><span class="sxs-lookup"><span data-stu-id="8aea5-157">An administrator can use the Internet Service Manager to find which version of the .NET Framework runs a Web site.</span></span> <span data-ttu-id="8aea5-158">Versiones diferentes del sistema operativo inicie el Administrador de servicios de Internet de forma diferente.</span><span class="sxs-lookup"><span data-stu-id="8aea5-158">Different operating system versions launch the Internet Service Manager differently.</span></span> <span data-ttu-id="8aea5-159">Para iniciar el Administrador de servicios, siga los pasos indicados a continuación.</span><span class="sxs-lookup"><span data-stu-id="8aea5-159">To start the service manager, follow the steps listed below.</span></span>

<span data-ttu-id="8aea5-160">**Para iniciar el Administrador de servicios Internet**</span><span class="sxs-lookup"><span data-stu-id="8aea5-160">**To start Internet Service Manager**</span></span>

1. <span data-ttu-id="8aea5-161">Vaya a **iniciar**.</span><span class="sxs-lookup"><span data-stu-id="8aea5-161">Go to **Start**.</span></span>
2. <span data-ttu-id="8aea5-162">Haga clic en **ejecutar**.</span><span class="sxs-lookup"><span data-stu-id="8aea5-162">Click on **run**.</span></span>
3. <span data-ttu-id="8aea5-163">Tipo **inetmgr**.</span><span class="sxs-lookup"><span data-stu-id="8aea5-163">Type **inetmgr**.</span></span>  
  
    ![](side-by-side-with-10/_static/image7.gif)
4. <span data-ttu-id="8aea5-164">Desde el Administrador de servicios de Internet, seleccione la aplicación Web cuya versión de .NET Framework que desea saber.</span><span class="sxs-lookup"><span data-stu-id="8aea5-164">From the Internet Service Manager, select the Web application whose version of the .NET Framework you want to know.</span></span>  
  
    ![](side-by-side-with-10/_static/image8.gif)
5. <span data-ttu-id="8aea5-165">Haga doble clic en la aplicación Web y haga clic en **propiedades.**</span><span class="sxs-lookup"><span data-stu-id="8aea5-165">Right-click on the Web application, and click on **Properties.**</span></span>  
  
    ![](side-by-side-with-10/_static/image9.gif)
6. <span data-ttu-id="8aea5-166">En la ventana de propiedades, seleccione **configuración.**</span><span class="sxs-lookup"><span data-stu-id="8aea5-166">From the Property window, select **Configuration.**</span></span>  
  
    ![](side-by-side-with-10/_static/image10.gif)
7. <span data-ttu-id="8aea5-167">En la tabla de asignación de aplicaciones, seleccione **.aspx**y haga clic en **editar**.</span><span class="sxs-lookup"><span data-stu-id="8aea5-167">From the application mapping table, select **.aspx**, and click **Edit**.</span></span>  
  
    ![](side-by-side-with-10/_static/image11.gif)
8. <span data-ttu-id="8aea5-168">Desde el **ejecutable** cuadro de texto, examine el directorio de versión mediante el desplazamiento.</span><span class="sxs-lookup"><span data-stu-id="8aea5-168">From the **Executable** text box, look at the version directory by scrolling.</span></span> <span data-ttu-id="8aea5-169">Si el directorio de versión es v.1.1.4322, la aplicación se asigna a la versión 1.1 de .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="8aea5-169">If the version directory is v.1.1.4322, the application is mapped to .NET Framework 1.1.</span></span> <span data-ttu-id="8aea5-170">Por el contrario, si el directorio de versión es v1.0.3705, la aplicación se asigna a .NET Framework 1.0.</span><span class="sxs-lookup"><span data-stu-id="8aea5-170">Conversely, if the version directory is v1.0.3705, the application is mapped to .NET Framework 1.0.</span></span>  
  
    ![](side-by-side-with-10/_static/image12.gif)
