---
uid: whitepapers/side-by-side-with-10
title: Ejecución de ASP.NET en paralelo de .NET Framework 1.0 y 1.1 | Documentos de Microsoft
author: rick-anderson
description: Estas notas del producto describen cómo instalar .NET 1.0 y 1.1 de .NET en su equipo, lo que permite una aplicación Web ASP.NET para ejecutarse en cualquiera de las versiones de los fotogramas...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: bdea2003-e964-4db5-9092-d56cc7560616
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/side-by-side-with-10
msc.type: content
ms.openlocfilehash: 939b04d440955b184bf5f4c40a2ef8175641edfa
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
ms.locfileid: "26530184"
---
<a name="aspnet-side-by-side-execution-of-net-framework-10-and-11"></a><span data-ttu-id="104cf-103">Ejecución de ASP.NET en paralelo de .NET Framework 1.0 y 1.1</span><span class="sxs-lookup"><span data-stu-id="104cf-103">ASP.NET Side-by-Side Execution of .NET Framework 1.0 and 1.1</span></span>
====================
> <span data-ttu-id="104cf-104">Estas notas del producto describen cómo instalar .NET 1.0 y 1.1 de .NET en su equipo, lo que permite una aplicación Web ASP.NET para ejecutarse en cualquiera de las versiones de framework.</span><span class="sxs-lookup"><span data-stu-id="104cf-104">This whitepaper describes how to install both .NET 1.0 and .NET 1.1 on your machine, allowing an ASP.NET Web application to run on either version of the framework.</span></span>
> 
> <span data-ttu-id="104cf-105">Se aplica a ASP.NET 1.0 y 1.1 de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="104cf-105">Applies to ASP.NET 1.0 and ASP.NET 1.1.</span></span>


<span data-ttu-id="104cf-106">En ASP.NET, las aplicaciones se dice que se ejecutan en paralelo cuando se instalan en el mismo equipo, pero utilizan distintas versiones de .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="104cf-106">In ASP.NET, applications are said to be running side by side when they are installed on the same computer, but use different versions of the .NET Framework.</span></span> <span data-ttu-id="104cf-107">El siguiente tema describe cómo configurar aplicaciones ASP.NET para la ejecución en paralelo y proporciona los pasos detallados para:</span><span class="sxs-lookup"><span data-stu-id="104cf-107">The following topic describes how to configure ASP.NET applications for side-by-side execution and provides detailed steps to:</span></span>

- [<span data-ttu-id="104cf-108">Mantener la asignación de la aplicación Web a la versión 1.0 de .NET Framework durante la instalación</span><span class="sxs-lookup"><span data-stu-id="104cf-108">Maintain your Web application's mapping to .NET Framework version 1.0 during installation</span></span>](#1)
- [<span data-ttu-id="104cf-109">Asignar una aplicación Web a una versión concreta de .NET Framework</span><span class="sxs-lookup"><span data-stu-id="104cf-109">Map a Web application to a specific version of the .NET Framework</span></span>](#2)
- [<span data-ttu-id="104cf-110">Buscar la versión de .NET Framework que está utilizando un sitio Web</span><span class="sxs-lookup"><span data-stu-id="104cf-110">Find the version of the .NET Framework that a Web site is using</span></span>](#3)

<span data-ttu-id="104cf-111">Tradicionalmente, cuando un componente o aplicación se actualiza en un equipo, la versión anterior es eliminada y reemplazada por la versión más reciente.</span><span class="sxs-lookup"><span data-stu-id="104cf-111">Traditionally, when a component or application is updated on a computer, the older version is removed and replaced with the newer version.</span></span> <span data-ttu-id="104cf-112">Si la nueva versión no es compatible con la versión anterior, esto normalmente interrumpe otras aplicaciones que utilizan el componente o aplicación.</span><span class="sxs-lookup"><span data-stu-id="104cf-112">If the new version is not compatible with the previous version, this usually breaks other applications that use the component or application.</span></span> <span data-ttu-id="104cf-113">.NET Framework proporciona compatibilidad para la ejecución en paralelo, lo que permite varias versiones de un ensamblado o la aplicación se instale en el mismo equipo al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="104cf-113">The .NET Framework provides support for side-by-side execution, which allows multiple versions of an assembly or application to be installed on the same computer at the same time.</span></span> <span data-ttu-id="104cf-114">Dado que se pueden instalar varias versiones simultáneamente, las aplicaciones administradas pueden seleccionar qué versión debe utilizar sin que ello afecte a las aplicaciones que utilizan una versión diferente.</span><span class="sxs-lookup"><span data-stu-id="104cf-114">Because multiple versions can be installed simultaneously, managed applications can select which version to use without affecting applications that use a different version.</span></span>

<span data-ttu-id="104cf-115">De forma predeterminada, durante la instalación de .NET Framework versión 1.1, todas las aplicaciones de ASP.NET existentes se vuelven a configurar automáticamente para utilizar la versión más reciente de .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="104cf-115">By default, during the installation of the .NET Framework version 1.1, all existing ASP.NET applications are automatically reconfigured to use the latest version of the .NET Framework.</span></span> <span data-ttu-id="104cf-116">Si no desea que las aplicaciones de ASP.NET en la configuración predeterminada para .NET Framework 1.1, haga clic en [aquí](#1) para obtener información sobre cómo evitar que esto durante la instalación.</span><span class="sxs-lookup"><span data-stu-id="104cf-116">If you do not want your ASP.NET applications to default to .NET Framework 1.1, click [here](#1) to learn how to prevent this during installation.</span></span>

<span data-ttu-id="104cf-117">Si actualiza el servidor Web a la versión 1.1 de .NET Framework y desea que una o varias aplicaciones Web para ejecutar .NET Framework 1.0, debe actualizar la asignación de Script de Internet Information Services (IIS).</span><span class="sxs-lookup"><span data-stu-id="104cf-117">If you update your Web server to .NET Framework 1.1 and want one or more Web applications to run .NET Framework 1.0, you need to update the Internet Information Services (IIS) Script Map.</span></span> <span data-ttu-id="104cf-118">La asignación de script es el mecanismo para asignar la extensión de archivo .aspx para una aplicación Web concreta a una versión de .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="104cf-118">The script mapping is the mechanism to map the .aspx file extension for a specific Web application to a version of the .NET Framework.</span></span> <span data-ttu-id="104cf-119">Haga clic en [aquí](#2) para obtener información sobre cómo asignar una aplicación Web a una versión concreta de .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="104cf-119">Click [here](#2) to learn how to map a Web application to a specific version of the .NET Framework.</span></span>

<span data-ttu-id="104cf-120">Puede usar el Administrador de información de Internet o la herramienta de registro de IIS de ASP.NET (Aspnet\_regiis.exe) para buscar qué versión de .NET Framework se ejecuta una aplicación Web concreta.</span><span class="sxs-lookup"><span data-stu-id="104cf-120">You can use the Internet Information Manger or the ASP.NET IIS Registration Tool (Aspnet\_regiis.exe) to find which .NET Framework version is running a particular Web application.</span></span> <span data-ttu-id="104cf-121">Haga clic en [aquí](#3) para obtener información sobre cómo buscar la versión de .NET Framework que está utilizando un sitio Web.</span><span class="sxs-lookup"><span data-stu-id="104cf-121">Click [here](#3) to learn how to find the version of the .NET Framework that a Web site is using.</span></span>

<span data-ttu-id="104cf-122">Una consideración de importación al migrar a .NET Framework 1.1 es que cada versión de .NET Framework usa su propio archivo Machine.config.</span><span class="sxs-lookup"><span data-stu-id="104cf-122">One import consideration when migrating to .NET Framework 1.1 is that each version of the .NET Framework uses its own Machine.config file.</span></span> <span data-ttu-id="104cf-123">Como resultado, si un administrador Web ha realizado cambios en el archivo Machine.config, esos cambios se deben migrar al archivo Machine.config de .NET Framework 1.1.</span><span class="sxs-lookup"><span data-stu-id="104cf-123">As a result, if a Web administrator has made changes to the Machine.config file, those changes must be migrated to the .NET Framework 1.1 Machine.config file.</span></span>

<a id="1"></a>

## <a name="maintaining-your-web-applications-mapping-to-net-framework-10-during-installation"></a><span data-ttu-id="104cf-124">Mantener la asignación de la aplicación Web para .NET Framework 1.0 durante la instalación</span><span class="sxs-lookup"><span data-stu-id="104cf-124">Maintaining your Web application's mapping to .NET Framework 1.0 during installation</span></span>

<span data-ttu-id="104cf-125">De forma predeterminada, todas las aplicaciones de ASP.NET existentes se reconfiguran automáticamente durante la instalación para usar la versión más reciente de .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="104cf-125">By default, all existing ASP.NET applications are automatically reconfigured during installation to use the newer version of the .NET Framework.</span></span> <span data-ttu-id="104cf-126">Con la versión más reciente de .NET Framework, las aplicaciones pueden aprovechar al máximo de mejoras y nuevas características incluidas en la nueva versión.</span><span class="sxs-lookup"><span data-stu-id="104cf-126">Using the newer version of the .NET Framework, applications can take full advantage of improvements and new features included in the new release.</span></span> <span data-ttu-id="104cf-127">Al mismo tiempo, el Administrador de Web, que podría desear el control granular sobre qué aplicaciones se actualizan, puede impedir la reasignación automática de todas las aplicaciones de ASP.NET existentes durante la instalación de .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="104cf-127">At the same time, the Web administrator, who might want granular control over which applications are updated, can prevent the automatic remapping of all existing ASP.NET applications during installation of the .NET Framework.</span></span>

<span data-ttu-id="104cf-128">Para impedir la reasignación automática de toda la aplicación ASP.NET a la versión más reciente de .NET Framework, el administrador Web puede usar la opción de línea de comandos /noaspupgrade con el programa de instalación Dotnetfx.exe.</span><span class="sxs-lookup"><span data-stu-id="104cf-128">To prevent the automatic remapping of the entire ASP.NET application to the newer version of the .NET Framework, the Web administrator can use the /noaspupgrade command-line option with the Dotnetfx.exe setup program.</span></span>

<span data-ttu-id="104cf-129">**Para evitar la reasignación del total de la aplicación ASP.NET a la versión más reciente**</span><span class="sxs-lookup"><span data-stu-id="104cf-129">**To prevent total remapping of ASP.NET application to newer version**</span></span>

1. <span data-ttu-id="104cf-130">Vaya a **iniciar**.</span><span class="sxs-lookup"><span data-stu-id="104cf-130">Go to **Start**.</span></span>
2. <span data-ttu-id="104cf-131">Haga clic en **ejecutar**.</span><span class="sxs-lookup"><span data-stu-id="104cf-131">Click on **run**.</span></span>
3. <span data-ttu-id="104cf-132">Escriba **cmd**.</span><span class="sxs-lookup"><span data-stu-id="104cf-132">Type **cmd**.</span></span>
4. <span data-ttu-id="104cf-133">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="104cf-133">Click **OK**.</span></span>  
  
    ![](side-by-side-with-10/_static/image1.gif)
5. <span data-ttu-id="104cf-134">Desde el símbolo del sistema, escriba la línea siguiente para iniciar la instalación de .NET Framework: **/c: Dotnetfx.exe "instalar /noaspupgrade?**.</span><span class="sxs-lookup"><span data-stu-id="104cf-134">From the command prompt, type the following line to start the installation of the .NET Framework: **Dotnetfx.exe /c:"install /noaspupgrade?**.</span></span>  
  
    ![](side-by-side-with-10/_static/image2.gif)
6. <span data-ttu-id="104cf-135">Haga clic en **Sí** en el programa de instalación de Microsoft .NET Framework 1.1.</span><span class="sxs-lookup"><span data-stu-id="104cf-135">Click **Yes** in the Microsoft .NET Framework 1.1 Setup.</span></span> <span data-ttu-id="104cf-136">Se iniciará el proceso de instalación de .NET Framework 1.1.</span><span class="sxs-lookup"><span data-stu-id="104cf-136">This will start the setup process of the .NET Framework 1.1.</span></span>  
  
    ![](side-by-side-with-10/_static/image3.gif)

<a id="2"></a>

## <a name="map-a-web-application-to-a-specific-version-of-the-net-framework"></a><span data-ttu-id="104cf-137">Asignar una aplicación Web a una versión concreta de .NET Framework</span><span class="sxs-lookup"><span data-stu-id="104cf-137">Map a Web application to a specific version of the .NET Framework</span></span>

<span data-ttu-id="104cf-138">Cada versión de .NET Framework incluye una versión de la herramienta de registro de IIS de ASP.NET (Aspnet\_regiis.exe).</span><span class="sxs-lookup"><span data-stu-id="104cf-138">Each version of the .NET Framework includes a version of the ASP.NET IIS Registration Tool (Aspnet\_regiis.exe).</span></span> <span data-ttu-id="104cf-139">Esta herramienta permite a los administradores especificar que una aplicación Web se ejecute en una versión concreta de .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="104cf-139">This tool enables administrators to specify that a Web application be run under a particular version of the .NET Framework.</span></span> <span data-ttu-id="104cf-140">Esto se conoce como asignación de una aplicación Web a una versión de .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="104cf-140">This is referred to as mapping a Web application to a version of the .NET Framework.</span></span> <span data-ttu-id="104cf-141">Los administradores deben seleccionar Aspnet\_regiis.exe que corresponde a la versión de .NET Framework que se asociará con la aplicación Web.</span><span class="sxs-lookup"><span data-stu-id="104cf-141">Administrators must select the Aspnet\_regiis.exe that corresponds to the version of the .NET Framework that will be associated with the Web application.</span></span> <span data-ttu-id="104cf-142">Por ejemplo, un administrador que desea especificar que un sitio Web use .NET Framework 1.1 debe utilizar Aspnet\_regiis.exe que se incluye con .NET Framework 1.1.</span><span class="sxs-lookup"><span data-stu-id="104cf-142">For example, an administrator who wants to specify that a Web site use .NET Framework 1.1 must use the Aspnet\_regiis.exe that comes with .NET Framework 1.1.</span></span>

<span data-ttu-id="104cf-143">Aspnet\_regiis.exe para la versión 1.0 se encuentra en:</span><span class="sxs-lookup"><span data-stu-id="104cf-143">The Aspnet\_regiis.exe for version 1.0 is located at:</span></span>

- <span data-ttu-id="104cf-144">C:\Windows\Microsoft.NET\Framework\**v1.0.3705** \aspnet\_regiis</span><span class="sxs-lookup"><span data-stu-id="104cf-144">C:\WINDOWS\Microsoft.NET\Framework\**v1.0.3705**\aspnet\_regiis</span></span>

<span data-ttu-id="104cf-145">Aspnet\_regiis.exe para la versión 1,1 se encuentra en:</span><span class="sxs-lookup"><span data-stu-id="104cf-145">The Aspnet\_regiis.exe for version 1,1 is located at:</span></span>

- <span data-ttu-id="104cf-146">C:\Windows\Microsoft.NET\Framework\**v1.1.4322** \aspnet\_regiis</span><span class="sxs-lookup"><span data-stu-id="104cf-146">C:\WINDOWS\Microsoft.NET\Framework\**v1.1.4322**\aspnet\_regiis</span></span>

<span data-ttu-id="104cf-147">Aspnet\_regiis.exe proporciona dos opciones para asignar una aplicación Web de la secuencia de comandos:</span><span class="sxs-lookup"><span data-stu-id="104cf-147">The Aspnet\_regiis.exe provides two options for script mapping a Web application:</span></span>

- <span data-ttu-id="104cf-148">**-s** establece la asignación de script en la ruta de acceso y su elemento secundario directorios.</span><span class="sxs-lookup"><span data-stu-id="104cf-148">**-s** sets the script map in the path and in its child directories.</span></span>
- <span data-ttu-id="104cf-149">**-sn** establece la asignación de script en la ruta de acceso solo.</span><span class="sxs-lookup"><span data-stu-id="104cf-149">**-sn** sets the script map in the path only.</span></span>

<span data-ttu-id="104cf-150">La ruta de acceso define la ruta de metadatos de Web aplicación IIS, que se define en el formulario de W3SVC/ROOT / {WebSiteNumber} / {aplicación\_nombre}.</span><span class="sxs-lookup"><span data-stu-id="104cf-150">The path defines the Web application IIS metadata path, which is defined in the form of W3SVC/ROOT/{WebSiteNumber}/{Application\_Name}.</span></span> <span data-ttu-id="104cf-151">Por ejemplo, para una aplicación Web que se le denominada Portal ubicado en el sitio Web predeterminado, la ruta de acceso de metabase es W3SVC/1/ROOT/Portal.</span><span class="sxs-lookup"><span data-stu-id="104cf-151">For example, for a Web application called Portal located under the default Web site, the metabase path is W3SVC/1/ROOT/Portal.</span></span>

![](side-by-side-with-10/_static/image4.gif)

<span data-ttu-id="104cf-152">Tenga en cuenta que también puede utilizar una herramienta denominada Editor de Metabase para obtener la ruta de acceso de metabase.</span><span class="sxs-lookup"><span data-stu-id="104cf-152">Note You can also use a tool called the Metabase Editor to get the metabase path.</span></span> <span data-ttu-id="104cf-153">Puede descargar esta herramienta desde el sitio de Microsoft Support en [https://support.microsoft.com/default.aspx?scid=kb;en-us;232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)</span><span class="sxs-lookup"><span data-stu-id="104cf-153">You can download this tool from the Microsoft Support site at [https://support.microsoft.com/default.aspx?scid=kb;en-us;232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)</span></span>

- <span data-ttu-id="104cf-154">Ejecutar Aspnet\_regiis.exe -s W3SVC/1/ROOT/Portal para actualizar el portal de IIS script mapa y su subapplication.</span><span class="sxs-lookup"><span data-stu-id="104cf-154">Run Aspnet\_regiis.exe -s W3SVC/1/ROOT/Portal to update the portal IIS script map and its subapplication.</span></span>  
  
    ![](side-by-side-with-10/_static/image5.gif)

- <span data-ttu-id="104cf-155">Ejecutar Aspnet\_regiis.exe -sn W3SVC/1/ROOT/Portal para actualizar la secuencia de comandos IIS portal se asignan, sin que ello afecte a las aplicaciones en el portal? subdirectorios s.</span><span class="sxs-lookup"><span data-stu-id="104cf-155">Run Aspnet\_regiis.exe -sn W3SVC/1/ROOT/Portal to update the portal IIS script map, without affecting applications in the portal?s subdirectories.</span></span>  
  
    ![](side-by-side-with-10/_static/image6.gif)

<a id="3"></a>

## <a name="find-the-net-framework-version-that-a-web-application-is-using"></a><span data-ttu-id="104cf-156">Buscar la versión de .NET Framework que está utilizando una aplicación Web</span><span class="sxs-lookup"><span data-stu-id="104cf-156">Find the .NET Framework version that a Web application is using</span></span>

<span data-ttu-id="104cf-157">Un administrador puede usar el Administrador de servicios de Internet para buscar la versión de .NET Framework ejecuta un sitio Web.</span><span class="sxs-lookup"><span data-stu-id="104cf-157">An administrator can use the Internet Service Manager to find which version of the .NET Framework runs a Web site.</span></span> <span data-ttu-id="104cf-158">Versiones diferentes del sistema operativo inicie el Administrador de servicios de Internet de manera diferente.</span><span class="sxs-lookup"><span data-stu-id="104cf-158">Different operating system versions launch the Internet Service Manager differently.</span></span> <span data-ttu-id="104cf-159">Para iniciar el Administrador de servicios, siga los pasos indicados a continuación.</span><span class="sxs-lookup"><span data-stu-id="104cf-159">To start the service manager, follow the steps listed below.</span></span>

<span data-ttu-id="104cf-160">**Para iniciar el Administrador de servicios de Internet**</span><span class="sxs-lookup"><span data-stu-id="104cf-160">**To start Internet Service Manager**</span></span>

1. <span data-ttu-id="104cf-161">Vaya a **iniciar**.</span><span class="sxs-lookup"><span data-stu-id="104cf-161">Go to **Start**.</span></span>
2. <span data-ttu-id="104cf-162">Haga clic en **ejecutar**.</span><span class="sxs-lookup"><span data-stu-id="104cf-162">Click on **run**.</span></span>
3. <span data-ttu-id="104cf-163">Tipo de **inetmgr**.</span><span class="sxs-lookup"><span data-stu-id="104cf-163">Type **inetmgr**.</span></span>  
  
    ![](side-by-side-with-10/_static/image7.gif)
4. <span data-ttu-id="104cf-164">Desde el Administrador de servicios de Internet, seleccione la aplicación Web cuya versión de .NET Framework que desea conocer.</span><span class="sxs-lookup"><span data-stu-id="104cf-164">From the Internet Service Manager, select the Web application whose version of the .NET Framework you want to know.</span></span>  
  
    ![](side-by-side-with-10/_static/image8.gif)
5. <span data-ttu-id="104cf-165">Haga doble clic en la aplicación Web y haga clic en **propiedades.**</span><span class="sxs-lookup"><span data-stu-id="104cf-165">Right-click on the Web application, and click on **Properties.**</span></span>  
  
    ![](side-by-side-with-10/_static/image9.gif)
6. <span data-ttu-id="104cf-166">En la ventana Propiedades, seleccione **configuración.**</span><span class="sxs-lookup"><span data-stu-id="104cf-166">From the Property window, select **Configuration.**</span></span>  
  
    ![](side-by-side-with-10/_static/image10.gif)
7. <span data-ttu-id="104cf-167">En la tabla de asignación de aplicación, seleccione **.aspx**y haga clic en **editar**.</span><span class="sxs-lookup"><span data-stu-id="104cf-167">From the application mapping table, select **.aspx**, and click **Edit**.</span></span>  
  
    ![](side-by-side-with-10/_static/image11.gif)
8. <span data-ttu-id="104cf-168">Desde el **ejecutable** cuadro de texto, busque en el directorio de la versión mediante el desplazamiento.</span><span class="sxs-lookup"><span data-stu-id="104cf-168">From the **Executable** text box, look at the version directory by scrolling.</span></span> <span data-ttu-id="104cf-169">Si el directorio de la versión es v.1.1.4322, la aplicación se asigna a la versión 1.1 de .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="104cf-169">If the version directory is v.1.1.4322, the application is mapped to .NET Framework 1.1.</span></span> <span data-ttu-id="104cf-170">Por el contrario, si el directorio de la versión es v1.0.3705, la aplicación se asigna a .NET Framework 1.0.</span><span class="sxs-lookup"><span data-stu-id="104cf-170">Conversely, if the version directory is v1.0.3705, the application is mapped to .NET Framework 1.0.</span></span>  
  
    ![](side-by-side-with-10/_static/image12.gif)
