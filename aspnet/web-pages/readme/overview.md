---
uid: web-pages/readme/overview
title: Archivo Léame de WebMatrix | Microsoft Docs
author: rick-anderson
description: WebMatrix y ASP.NET Web Pages (Razor) versión 1.0 Léame
ms.author: riande
ms.date: 01/06/2011
ms.assetid: 36c5beeb-45a7-48a0-9c30-f82cdf5c5f5f
msc.legacyurl: /web-pages/readme
msc.type: content
ms.openlocfilehash: aa852e7bbd93622154d59e0d0a13ffa680812df2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837671"
---
<a name="webmatrix-readme"></a><span data-ttu-id="e5e58-103">Archivo Léame de WebMatrix</span><span class="sxs-lookup"><span data-stu-id="e5e58-103">WebMatrix Readme</span></span>
====================
<span data-ttu-id="e5e58-104">13 de enero de 2011</span><span class="sxs-lookup"><span data-stu-id="e5e58-104">13 January 2011</span></span>

## <a name="contents"></a><span data-ttu-id="e5e58-105">Contenido</span><span class="sxs-lookup"><span data-stu-id="e5e58-105">Contents</span></span>

> [!NOTE]
> <span data-ttu-id="e5e58-106">Este archivo Léame se aplica a la 1.0 versión de WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="e5e58-106">This readme applies to the 1.0 release of WebMatrix.</span></span>


- [<span data-ttu-id="e5e58-107">Información general</span><span class="sxs-lookup"><span data-stu-id="e5e58-107">Overview</span></span>](#Overview)
- [<span data-ttu-id="e5e58-108">Instalación</span><span class="sxs-lookup"><span data-stu-id="e5e58-108">Installation</span></span>](#Installation_Notes)
- [<span data-ttu-id="e5e58-109">Cómo publicar aplicaciones</span><span class="sxs-lookup"><span data-stu-id="e5e58-109">How to Publish Applications</span></span>](#InstructionsForPublishingApplications)
- [<span data-ttu-id="e5e58-110">Los cambios y problemas</span><span class="sxs-lookup"><span data-stu-id="e5e58-110">Changes and Issues</span></span>](#ChangesAndIssues)

    - [<span data-ttu-id="e5e58-111">Instalación de WebMatrix 1.0</span><span class="sxs-lookup"><span data-stu-id="e5e58-111">WebMatrix 1.0 Installation</span></span>](#Known_Issues_Installation)
    - <span data-ttu-id="e5e58-112">[ASP.NET Web Pages](#Known_Issues_ASPNET) (Más información sobre páginas web de ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="e5e58-112">[ASP.NET Web Pages](#Known_Issues_ASPNET)</span></span>
    - [<span data-ttu-id="e5e58-113">WebMatrix</span><span class="sxs-lookup"><span data-stu-id="e5e58-113">WebMatrix</span></span>](#Known_Issues_WebMatrix)
    - [<span data-ttu-id="e5e58-114">IIS Express</span><span class="sxs-lookup"><span data-stu-id="e5e58-114">IIS Express</span></span>](#Known_Issues_IISExpress)
    - [<span data-ttu-id="e5e58-115">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="e5e58-115">SQL Server Compact</span></span>](#Known_Issues_SQLServerCompact)
    - [<span data-ttu-id="e5e58-116">Instalación de aplicaciones</span><span class="sxs-lookup"><span data-stu-id="e5e58-116">Installing Applications</span></span>](#Known_Issues_Installing_Applications)
    - [<span data-ttu-id="e5e58-117">Publicación de aplicaciones</span><span class="sxs-lookup"><span data-stu-id="e5e58-117">Publishing Applications</span></span>](#Known_Issues_Publishing_Applications)
- [<span data-ttu-id="e5e58-118">Para obtener más información</span><span class="sxs-lookup"><span data-stu-id="e5e58-118">For More Information</span></span>](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a><span data-ttu-id="e5e58-119">Información general</span><span class="sxs-lookup"><span data-stu-id="e5e58-119">Overview</span></span>

> <span data-ttu-id="e5e58-120">Microsoft WebMatrix 1.0 es una pila de desarrollo web gratuito que se instala en pocos minutos.</span><span class="sxs-lookup"><span data-stu-id="e5e58-120">Microsoft WebMatrix 1.0 is a free web development stack that installs in minutes.</span></span> <span data-ttu-id="e5e58-121">Se integra un servidor web con la base de datos y marcos para crear una única experiencia integrada de programación.</span><span class="sxs-lookup"><span data-stu-id="e5e58-121">It integrates a web server with database and programming frameworks to create a single, integrated experience.</span></span> <span data-ttu-id="e5e58-122">Puede usar WebMatrix para agilizar la manera de codificar, probar y publicar su propio sitio Web ASP.NET o PHP, o puede usar WebMatrix para iniciar un nuevo sitio Web con aplicaciones de código abierto populares como DotNetNuke, Umbraco, WordPress o Joomla.</span><span class="sxs-lookup"><span data-stu-id="e5e58-122">You can use WebMatrix to streamline the way you code, test, and publish your own ASP.NET or PHP website, or you can use WebMatrix to start a new website using popular open-source apps like DotNetNuke, Umbraco, WordPress, or Joomla.</span></span> <span data-ttu-id="e5e58-123">WebMatrix usa el mismo servidor web eficaz, el motor de base de datos y el entorno de marcos que se ejecutará el sitio Web en internet, lo que hace la transición de desarrollo a producción homogénea y sin problemas.</span><span class="sxs-lookup"><span data-stu-id="e5e58-123">WebMatrix uses the same powerful web server, database engine, and frameworks environment that will run your website on the internet, which makes the transition from development to production smooth and seamless.</span></span>


<a id="Installation_Notes"></a>

## <a name="installation"></a><span data-ttu-id="e5e58-124">Instalación</span><span class="sxs-lookup"><span data-stu-id="e5e58-124">Installation</span></span>

> <span data-ttu-id="e5e58-125">Para instalar WebMatrix 1.0, primero debe instalar el [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="e5e58-125">To install WebMatrix 1.0, you must first install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span> <span data-ttu-id="e5e58-126">Después de instalar al instalador de plataforma Web, puede usarlo para instalar WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="e5e58-126">After you've installed the Web Platform Installer, you can use it to install WebMatrix.</span></span>
> 
> <span data-ttu-id="e5e58-127">Si tiene problemas durante la instalación, consulte [solución de problemas con el instalador de plataforma Web de Microsoft](https://go.microsoft.com/fwlink/?LinkId=196212).</span><span class="sxs-lookup"><span data-stu-id="e5e58-127">If you have problems during installation, refer to [Troubleshooting Problems with Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span></span>


<a id="InstructionsForPublishingApplications"></a>
## <a name="how-to-publish-applications"></a><span data-ttu-id="e5e58-128">Cómo publicar aplicaciones</span><span class="sxs-lookup"><span data-stu-id="e5e58-128">How to Publish Applications</span></span>

> <span data-ttu-id="e5e58-129">Consulte [instrucciones paso a paso para publicar aplicaciones](https://go.microsoft.com/fwlink/?LinkID=196149)</span><span class="sxs-lookup"><span data-stu-id="e5e58-129">See [Step-by-Step Instructions for Publishing Applications](https://go.microsoft.com/fwlink/?LinkID=196149)</span></span>


<a id="ChangesAndIssues"></a>

## <a name="changes-and-issues"></a><span data-ttu-id="e5e58-130">Los cambios y problemas</span><span class="sxs-lookup"><span data-stu-id="e5e58-130">Changes and Issues</span></span>

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-10-installation-issues"></a><span data-ttu-id="e5e58-131">Problemas de instalación de 1.0 de WebMatrix</span><span class="sxs-lookup"><span data-stu-id="e5e58-131">WebMatrix 1.0 Installation Issues</span></span>

#### <a name="issue-webmatrix-10-is-available-only-on-platforms-that-support-microsoft-net-framework-4"></a><span data-ttu-id="e5e58-132">Problema: WebMatrix 1.0 solo está disponible en las plataformas que admiten Microsoft .NET Framework 4</span><span class="sxs-lookup"><span data-stu-id="e5e58-132">Issue: WebMatrix 1.0 is available only on platforms that support Microsoft .NET Framework 4</span></span>

> <span data-ttu-id="e5e58-133">La versión 4 de .NET Framework es necesaria para WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="e5e58-133">The .NET Framework version 4 is required for WebMatrix.</span></span> <span data-ttu-id="e5e58-134">En algunos casos, el programa de instalación de 1.0 WebMatrix le permitirá intenta instalar en una plataforma que no forma parte del conjunto de configuración admitidos.</span><span class="sxs-lookup"><span data-stu-id="e5e58-134">In certain cases, the WebMatrix 1.0 installer will let you try to install on a platform that is not part of the supported configuration set.</span></span> <span data-ttu-id="e5e58-135">En concreto, Windows Vista sin la actualización SP1 le permiten comenzar la instalación de WebMatrix, pero producirá un error en el componente de .NET Framework 4 y bloquear la instalación.</span><span class="sxs-lookup"><span data-stu-id="e5e58-135">In particular, Windows Vista without the SP1 update will let you begin the installation of WebMatrix, but the .NET Framework 4 component will fail and block your installation.</span></span>
> 
> <span data-ttu-id="e5e58-136">**Solución alternativa**</span><span class="sxs-lookup"><span data-stu-id="e5e58-136">**Workaround**</span></span>  
> <span data-ttu-id="e5e58-137">Instalar en una plataforma compatible, que incluye:</span><span class="sxs-lookup"><span data-stu-id="e5e58-137">Install on a supported platform, which includes:</span></span>
> 
> - <span data-ttu-id="e5e58-138">Windows 7</span><span class="sxs-lookup"><span data-stu-id="e5e58-138">Windows 7</span></span>
> - <span data-ttu-id="e5e58-139">Windows Server 2008</span><span class="sxs-lookup"><span data-stu-id="e5e58-139">Windows Server 2008</span></span>
> - <span data-ttu-id="e5e58-140">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="e5e58-140">Windows Server 2008 R2</span></span>
> - <span data-ttu-id="e5e58-141">Windows Vista SP1 o posterior</span><span class="sxs-lookup"><span data-stu-id="e5e58-141">Windows Vista SP1 or later</span></span>
> - <span data-ttu-id="e5e58-142">Windows XP SP3</span><span class="sxs-lookup"><span data-stu-id="e5e58-142">Windows XP SP3</span></span>
> - <span data-ttu-id="e5e58-143">Windows Server 2003 SP2</span><span class="sxs-lookup"><span data-stu-id="e5e58-143">Windows Server 2003 SP2</span></span>


#### <a name="issue-cannot-install-webmatrix-10-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a><span data-ttu-id="e5e58-144">Problema: No se puede instalar WebMatrix 1.0 si se instala Microsoft Visual Studio 2008 sin Microsoft Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="e5e58-144">Issue: Cannot install WebMatrix 1.0 if Microsoft Visual Studio 2008 is installed without Microsoft Visual Studio 2008 SP1</span></span>

> <span data-ttu-id="e5e58-145">**Solución alternativa**</span><span class="sxs-lookup"><span data-stu-id="e5e58-145">**Workaround**</span></span>  
> <span data-ttu-id="e5e58-146">Instalar [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) desde el centro de descarga de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="e5e58-146">Install [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) from the Microsoft Download Center.</span></span>


#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a><span data-ttu-id="e5e58-147">Problema: Algunos ensamblados de SQL Server Compact 4.0 no están instalados en la GAC</span><span class="sxs-lookup"><span data-stu-id="e5e58-147">Issue: Some assemblies for SQL Server Compact 4.0 are not installed in the GAC</span></span>

> <span data-ttu-id="e5e58-148">Los ensamblados administrados para SQL Server Compact 4.0 no se colocan en la caché de ensamblados global (GAC) al instalar SQL Server Compact 4.0 en un equipo de 64 bits y el equipo tiene solo .NET Framework 3.5 SP1 Client Profile instalado.</span><span class="sxs-lookup"><span data-stu-id="e5e58-148">The managed assemblies for SQL Server Compact 4.0 are not placed in the global assembly cache (GAC) when you install SQL Server Compact 4.0 on a 64-bit computer and the computer has only the .NET Framework 3.5 SP1 Client Profile installed.</span></span> <span data-ttu-id="e5e58-149">Son los ensamblados administrados que no están instalados en la GAC:</span><span class="sxs-lookup"><span data-stu-id="e5e58-149">The managed assemblies that are not installed in the GAC are:</span></span>
> 
> - <span data-ttu-id="e5e58-150">*System.Data.SqlServerCe.dll* (proveedor de ADO.NET)</span><span class="sxs-lookup"><span data-stu-id="e5e58-150">*System.Data.SqlServerCe.dll* (ADO.NET provider)</span></span>
> - <span data-ttu-id="e5e58-151">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework)</span><span class="sxs-lookup"><span data-stu-id="e5e58-151">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )</span></span>
> 
> <span data-ttu-id="e5e58-152">**Solución alternativa**</span><span class="sxs-lookup"><span data-stu-id="e5e58-152">**Workaround**</span></span>  
> <span data-ttu-id="e5e58-153">Desinstalar SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="e5e58-153">Uninstall SQL Server Compact 4.0.</span></span> <span data-ttu-id="e5e58-154">Descargue e instale la versión completa de .NET Framework 3.5 SP1 desde la ubicación siguiente:</span><span class="sxs-lookup"><span data-stu-id="e5e58-154">Download and install the full version of .NET Framework 3.5 SP1 from the following location:</span></span>  
>   
> [<span data-ttu-id="e5e58-155">Paquete de Microsoft .NET Framework 3.5 con Service 1 (paquete completo)</span><span class="sxs-lookup"><span data-stu-id="e5e58-155">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> <span data-ttu-id="e5e58-156">A continuación, vuelva a instalar SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="e5e58-156">Then reinstall SQL Server Compact 4.0.</span></span>


#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a><span data-ttu-id="e5e58-157">Problema: No se puede desinstalar SQL Server Compact usando la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="e5e58-157">Issue: Cannot uninstall SQL Server Compact using the command line</span></span>

> <span data-ttu-id="e5e58-158">Desinstalación de SQL Server Compact usando las opciones de línea de comandos no funciona en esta versión.</span><span class="sxs-lookup"><span data-stu-id="e5e58-158">Uninstallation of SQL Server Compact using command-line options does not work in this release.</span></span>
> 
> <span data-ttu-id="e5e58-159">**Solución alternativa**</span><span class="sxs-lookup"><span data-stu-id="e5e58-159">**Workaround**</span></span>  
> <span data-ttu-id="e5e58-160">Use *programas y características* en el Panel de Control de Windows para desinstalar Microsoft SQL Server Compact 4.0.</span><span class="sxs-lookup"><span data-stu-id="e5e58-160">Use *Programs and Features* in the Windows Control Panel to uninstall Microsoft SQL Server Compact 4.0.</span></span>


<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a><span data-ttu-id="e5e58-161">ASP.NET Web Pages</span><span class="sxs-lookup"><span data-stu-id="e5e58-161">ASP.NET Web Pages</span></span>

<span data-ttu-id="e5e58-162">Esta sección del documento describen las nuevas características, cambios y problemas conocidos con la 1.0 versión de ASP.NET Web Pages con sintaxis Razor.</span><span class="sxs-lookup"><span data-stu-id="e5e58-162">This section of the document describes new features, changes, and known issues with the 1.0 release of ASP.NET Web Pages with Razor syntax.</span></span>

- [<span data-ttu-id="e5e58-163">Características nuevas</span><span class="sxs-lookup"><span data-stu-id="e5e58-163">New features</span></span>](#NewFeatures)
- [<span data-ttu-id="e5e58-164">Cambios</span><span class="sxs-lookup"><span data-stu-id="e5e58-164">Changes</span></span>](#Changes)
- [<span data-ttu-id="e5e58-165">Problemas</span><span class="sxs-lookup"><span data-stu-id="e5e58-165">Issues</span></span>](#Issues)

#### <a id="NewFeatures"></a>  <span data-ttu-id="e5e58-166">Nuevas características</span><span class="sxs-lookup"><span data-stu-id="e5e58-166">New Features</span></span>

#### <a name="new-configuration-setting-added-to-disable-the-package-manager"></a><span data-ttu-id="e5e58-167">Nuevo: Agrega la opción de configuración para deshabilitar al administrador de paquetes</span><span class="sxs-lookup"><span data-stu-id="e5e58-167">New: Configuration setting added to disable the package manager</span></span>

> <span data-ttu-id="e5e58-168">Un nuevo `asp:AdminManagerEnabled` clave está disponible para el `<appSettings>` elemento en el *web.config* archivo, que le permite deshabilitar completamente el Administrador de paquetes.</span><span class="sxs-lookup"><span data-stu-id="e5e58-168">A new `asp:AdminManagerEnabled` key is available for the `<appSettings>` element in the *web.config* file, which lets you completely disable the package manager.</span></span> <span data-ttu-id="e5e58-169">El valor predeterminado para este elemento es true, lo que significa que si no se incluye en el *web.config* archivo, el Administrador de paquetes está habilitado.</span><span class="sxs-lookup"><span data-stu-id="e5e58-169">The default value for this element is true, meaning that if it is not included in the *web.config* file, the package manager is enabled.</span></span> <span data-ttu-id="e5e58-170">Para deshabilitar el Administrador de paquetes, agregue el siguiente elemento a la *web.config* archivo en la raíz del sitio Web:</span><span class="sxs-lookup"><span data-stu-id="e5e58-170">To disable the package manager, add the following element to the *web.config* file in the root of the website:</span></span>
> 
> [!code-xml[Main](overview/samples/sample1.xml)]


#### <a id="Changes"></a>  <span data-ttu-id="e5e58-171">Cambios</span><span class="sxs-lookup"><span data-stu-id="e5e58-171">Changes</span></span>

#### <a name="change-webpagesadminfoldervirtualpath-key-renamed-to-aspadminfoldervirtualpath"></a><span data-ttu-id="e5e58-172">Cambio: clave "webPages:AdminFolderVirtualPath" ha cambiado a "asp: AdminFolderVirtualPath"</span><span class="sxs-lookup"><span data-stu-id="e5e58-172">Change: "webPages:AdminFolderVirtualPath" key renamed to "asp:AdminFolderVirtualPath"</span></span>

> <span data-ttu-id="e5e58-173">El `webPages:AdminFolderVirtualPath` clave que se puede agregar a la *web.config* archivo para especificar la ubicación del Administrador de paquetes se cambió para usar el `asp:` espacio de nombres en lugar de la `webPages` espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="e5e58-173">The `webPages:AdminFolderVirtualPath` key that can be added to the *web.config* file to specify the location of the package manager has been renamed to use the `asp:` namespace instead of the `webPages` namespace.</span></span> <span data-ttu-id="e5e58-174">Si ha usado este elemento, debe cambiar el nombre del archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="e5e58-174">If you have used this element, you must rename it in the configuration file.</span></span>


#### <a id="Issues"></a>  <span data-ttu-id="e5e58-175">Problemas conocidos</span><span class="sxs-lookup"><span data-stu-id="e5e58-175">Known Issues</span></span>

#### <a name="issue-passwords-for-membership-users-no-longer-recognized"></a><span data-ttu-id="e5e58-176">Problema: Contraseñas para usuarios de pertenencia que ya no se reconoce</span><span class="sxs-lookup"><span data-stu-id="e5e58-176">Issue: Passwords for membership users no longer recognized</span></span>

> <span data-ttu-id="e5e58-177">El algoritmo para crear y almacenar las contraseñas de pertenencia (inicio de sesión) se ha cambiado para ser más seguro.</span><span class="sxs-lookup"><span data-stu-id="e5e58-177">The algorithm for creating and storing membership (login) passwords has been changed to be more secure.</span></span> <span data-ttu-id="e5e58-178">Como resultado, no se reconocerá las contraseñas almacenadas para los miembros (usuarios) creados en las versiones Beta de ASP.NET Razor.</span><span class="sxs-lookup"><span data-stu-id="e5e58-178">As a result, the passwords stored for members (users) created in Beta versions of ASP.NET Razor will not be recognized.</span></span> 
> 
> <span data-ttu-id="e5e58-179">**Solución alternativa** si el sitio aún no se haya puesto en producción, quitar los registros de usuario de la base de datos de pertenencia.</span><span class="sxs-lookup"><span data-stu-id="e5e58-179">**Workaround** If the site has not yet been put into production, remove the user records from the membership database.</span></span> <span data-ttu-id="e5e58-180">Si la base de datos está activa, volver a generar mediante programación las contraseñas existentes en la base de datos de pertenencia.</span><span class="sxs-lookup"><span data-stu-id="e5e58-180">If database is live, programmatically regenerate existing passwords in the membership database.</span></span>


#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a><span data-ttu-id="e5e58-181">Problema: Un comportamiento inesperado cuando se usa una tabla de usuario personalizada para la suscripción</span><span class="sxs-lookup"><span data-stu-id="e5e58-181">Issue: Unexpected behavior when using a custom user table for membership</span></span>

> <span data-ttu-id="e5e58-182">Para inicializar el proveedor de pertenencia para un sitio Web de ASP.NET Razor, llame a la `WebSecurity.InitializeDatabaseConnection` método.</span><span class="sxs-lookup"><span data-stu-id="e5e58-182">To initialize the membership provider for an ASP.NET Razor website, you call the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="e5e58-183">(En WebMatrix, la plantilla de sitio inicial incluye una llamada a este método en el  *\_AppStart.cshtml* archivo.) Si el `autoCreateTables` parámetro de este método está establecido en true (de forma predeterminada, se establece en true en la plantilla de sitio de inicio), y si un nombre de tabla no reconocida se pasa al método (el segundo parámetro), el método no produce un error.</span><span class="sxs-lookup"><span data-stu-id="e5e58-183">(In WebMatrix, the Starter Site template includes a call to this method in the *\_AppStart.cshtml* file.) If the `autoCreateTables` parameter of this method is set to true (by default, it is set to true in the Starter Site template), and if an unrecognized table name is passed to the method (the second parameter), the method does not throw an error.</span></span> <span data-ttu-id="e5e58-184">En su lugar, crea automáticamente la tabla.</span><span class="sxs-lookup"><span data-stu-id="e5e58-184">Instead, it automatically creates the table.</span></span>
> 
> <span data-ttu-id="e5e58-185">Esto puede ser un problema si va a usar una tabla de usuario personalizada para la suscripción pero pase el nombre de tabla incorrecto para el `WebSecurity.InitializeDatabaseConnection` método.</span><span class="sxs-lookup"><span data-stu-id="e5e58-185">This can be a problem if you intend to use a custom user table for membership but pass the wrong table name to the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="e5e58-186">Dado que el método no predeterminada genera un error si no existe la tabla que se especifique, y porque crea una nueva tabla en su lugar, la aplicación puede parecer a trabajar.</span><span class="sxs-lookup"><span data-stu-id="e5e58-186">Because the method does not by default raise an error if the table you specify does not exist, and because it instead creates a new table, the application can appear to be working.</span></span> <span data-ttu-id="e5e58-187">Sin embargo, código de aplicación que se basa en la tabla de usuario personalizada (y en los campos de él) puede producir un error con errores inesperados.</span><span class="sxs-lookup"><span data-stu-id="e5e58-187">However, application code that relies on your custom user table (and on fields in it) can eventually fail with unexpected errors.</span></span>
> 
> <span data-ttu-id="e5e58-188">**Solución alternativa**</span><span class="sxs-lookup"><span data-stu-id="e5e58-188">**Workaround**</span></span>  
> <span data-ttu-id="e5e58-189">Asegúrese de que pasa el nombre de la `InitializeDatabaseConnection` coincidencias de método, el perfil de usuario de la tabla en la base de datos de pertenencia o asegúrese de que el `autoCreateTables` parámetro se establece en false.</span><span class="sxs-lookup"><span data-stu-id="e5e58-189">Make sure that the name passed in the `InitializeDatabaseConnection` method matches the user profile table in the membership database, or make sure that the `autoCreateTables` parameter is set to false.</span></span>


#### <a name="issue-error-message-the-admin-module-requires-access-to-appdata"></a><span data-ttu-id="e5e58-190">Problema: Mensaje de Error "el módulo de administración requiere acceso a ~/App\_datos"</span><span class="sxs-lookup"><span data-stu-id="e5e58-190">Issue: Error message "The Admin Module requires access to ~/App\_Data"</span></span>

> <span data-ttu-id="e5e58-191">En algunas circunstancias, intentando crear usuarios o trabajar con el sistema de pertenencia ASP.NET puede hacer que la página Mostrar el error *el módulo de administración requiere acceso a ~/App\_datos*.</span><span class="sxs-lookup"><span data-stu-id="e5e58-191">Under some circumstances, trying to create users or otherwise work with the ASP.NET membership system can cause the page to display the error *The Admin Module requires access to ~/App\_Data*.</span></span> <span data-ttu-id="e5e58-192">Esto se produce si la cuenta que se está ejecutando en IIS o IIS Express no tiene permisos para crear y escribir en el *aplicación\_datos* carpeta bajo la raíz del sitio Web.</span><span class="sxs-lookup"><span data-stu-id="e5e58-192">This occurs if the account that IIS or IIS Express is running under does not have permissions to create and write to the *App\_Data* folder under the website root.</span></span> 
> 
> <span data-ttu-id="e5e58-193">**Solución alternativa** crear manualmente un *aplicación\_datos* carpeta para el sitio Web.</span><span class="sxs-lookup"><span data-stu-id="e5e58-193">**Workaround** Manually create an *App\_Data* folder for the website.</span></span> <span data-ttu-id="e5e58-194">Asegúrese de que la cuenta de Windows que se ejecuta la aplicación (normalmente, el servicio de red) tiene los permisos de lectura/escritura para las carpetas raíz de la aplicación y para las subcarpetas, como aplicación\_datos.</span><span class="sxs-lookup"><span data-stu-id="e5e58-194">Then make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as App\_Data.</span></span> <span data-ttu-id="e5e58-195">Información más detallada está disponible en el artículo de Knowledge Base [problemas con las instancias de usuario de SQL Server Express y proyectos de aplicación Web ASP.net](https://support.microsoft.com/kb/2002980).</span><span class="sxs-lookup"><span data-stu-id="e5e58-195">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>


#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a><span data-ttu-id="e5e58-196">Problema: "error no se pudo generar una instancia de usuario de SQL Server"</span><span class="sxs-lookup"><span data-stu-id="e5e58-196">Issue: "Failed to generate a user instance of SQL Server" error</span></span>

> <span data-ttu-id="e5e58-197">Si una aplicación WebMatrix Web usa SQL Server Express y se está ejecutando IIS 7.5 en Windows 7 o Windows Server 2008 R2, puede aparecer un error que indica que SQL Server no se puede recuperar la ruta de acceso de aplicación local del usuario en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="e5e58-197">If a WebMatrix Web application uses SQL Server Express and is running IIS 7.5 on Windows 7 or Windows Server 2008 R2, you might see an error that indicates that SQL Server cannot retrieve the user's local application path at run time.</span></span>
> 
> <span data-ttu-id="e5e58-198">**Solución alternativa** Asegúrese de que la cuenta de Windows que se ejecuta la aplicación (normalmente, el servicio de red) tiene los permisos de lectura/escritura para las carpetas raíz de la aplicación y para las subcarpetas como *aplicación\_datos*.</span><span class="sxs-lookup"><span data-stu-id="e5e58-198">**Workaround** Make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as *App\_Data*.</span></span> <span data-ttu-id="e5e58-199">Información más detallada está disponible en el artículo de Knowledge Base [problemas con las instancias de usuario de SQL Server Express y proyectos de aplicación Web ASP.net](https://support.microsoft.com/kb/2002980).</span><span class="sxs-lookup"><span data-stu-id="e5e58-199">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>


#### <a name="issue-files-that-contains-package-manager-resources-or-package-manager-passwords-are-servable-under-iis-60-and-earlier"></a><span data-ttu-id="e5e58-200">Problema: Los archivos que contiene los recursos del Administrador de paquetes o las contraseñas de administrador de paquetes son que se pueden proporcionar en IIS 6.0 y versiones anteriores</span><span class="sxs-lookup"><span data-stu-id="e5e58-200">Issue: Files that contains package-manager resources or package-manager passwords are servable under IIS 6.0 and earlier</span></span>

> <span data-ttu-id="e5e58-201">Si implementa una aplicación de ASP.NET Web Pages (Razor) que se generó con la versión RC2, y si la aplicación contiene un *password.txt* o *packagesources.txt* archivo */App\_ Administrador dedatos/*, IIS 6.0 servirá el archivo si se solicita, puede exponer las contraseñas para la instancia del Administrador de paquetes.</span><span class="sxs-lookup"><span data-stu-id="e5e58-201">If you deploy an ASP.NET Web Pages (Razor) application that was built using the RC2 release, and if the application contains a *password.txt* or *packagesources.txt* file under */App\_Data/admin*, IIS 6.0 will serve the file if requested, potentially exposing the passwords for your package manager instance.</span></span> 
> 
> <span data-ttu-id="e5e58-202">**Solución alternativa** cambiar el nombre de la *password.txt* o *packagesources.txt* archivo *password.config* o *packagesources.config*. De forma predeterminada, IIS 6.0 no suministrará los archivos que tienen la *.config* extensión.</span><span class="sxs-lookup"><span data-stu-id="e5e58-202">**Workaround** Rename the *password.txt* or *packagesources.txt* file to *password.config* or *packagesources.config*. By default, IIS 6.0 does not serve files that have the *.config* extension.</span></span> <span data-ttu-id="e5e58-203">(En IIS 7, no hay archivos en el *aplicación\_datos* se sirven carpeta, por lo que no es necesario cambiar el nombre de los archivos.)</span><span class="sxs-lookup"><span data-stu-id="e5e58-203">(In IIS 7, no files in the *App\_Data* folder are served, so you do not need to rename the files.)</span></span>


#### <a name="issue-uninstalling-packages-installed-using-the-beta-3-release-does-not-completely-remove-package-components"></a><span data-ttu-id="e5e58-204">Problema: Los paquetes instalados con la versión Beta 3 de desinstalación no quitar completamente los componentes del paquete</span><span class="sxs-lookup"><span data-stu-id="e5e58-204">Issue: Uninstalling packages installed using the Beta 3 release does not completely remove package components</span></span>

> <span data-ttu-id="e5e58-205">Si instaló un paquete mediante el Administrador de paquetes en la versión Beta 3 y, a continuación, intente desinstalar mediante la versión actual, el paquete no se desinstala completamente.</span><span class="sxs-lookup"><span data-stu-id="e5e58-205">If you installed a package using the package manager in the Beta 3 release and then try to uninstall it using the current release, the package is not completely uninstalled.</span></span> <span data-ttu-id="e5e58-206">Mediante el Administrador de paquetes **desinstalar** botón quita algunos componentes, pero deje el código de biblioteca del paquete y no actualiza el *package.config* archivo.</span><span class="sxs-lookup"><span data-stu-id="e5e58-206">Using the package manager's **Uninstall** button removes some components, but leaves the package's library code and does not update the *package.config* file.</span></span>
> 
> <span data-ttu-id="e5e58-207">**Solución alternativa** </span><span class="sxs-lookup"><span data-stu-id="e5e58-207">**Workaround** </span></span>  
> <span data-ttu-id="e5e58-208">Siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="e5e58-208">Perform these steps:</span></span>  
> 1. <span data-ttu-id="e5e58-209">Eliminar el *aplicación\_Data\packages* carpeta.</span><span class="sxs-lookup"><span data-stu-id="e5e58-209">Delete the *App\_Data\packages* folder.</span></span> <span data-ttu-id="e5e58-210">Esto quita todos los paquetes.</span><span class="sxs-lookup"><span data-stu-id="e5e58-210">This removes all packages.</span></span>   
> 2. <span data-ttu-id="e5e58-211">Eliminar el *packages.config* archivo en la raíz del sitio Web.</span><span class="sxs-lookup"><span data-stu-id="e5e58-211">Delete the *packages.config* file in the root of the website.</span></span>


#### <a name="issue-in-visual-studio-invoking-the-web-based-package-manager-takes-the-application-offline"></a><span data-ttu-id="e5e58-212">Problema: En Visual Studio, invocar el Administrador de paquetes basada en web toma la aplicación sin conexión</span><span class="sxs-lookup"><span data-stu-id="e5e58-212">Issue: In Visual Studio, invoking the web-based package manager takes the application offline</span></span>

> <span data-ttu-id="e5e58-213">Si está trabajando en Visual Studio (no WebMatrix) y use la  *\_admin* funcionalidad para iniciar el Administrador de paquetes, Visual Studio se desconecta de la aplicación y registra el *aplicación\_ offline.htm* en la raíz del sitio Web, se interrumpirá su capacidad para usar el Administrador de paquetes.</span><span class="sxs-lookup"><span data-stu-id="e5e58-213">If you are working in Visual Studio (not WebMatrix) and use the *\_admin* functionality to start the package manager, Visual Studio takes the application offline and posts the *app\_offline.htm* into the website root, which disrupts your ability to use the package manager.</span></span>
> 
> [!NOTE]
> <span data-ttu-id="e5e58-214">Aunque normalmente vería este comportamiento cuando se usa la interfaz del Administrador de paquetes basado en web, se produce el mismo comportamiento si agregar, quitar o modificar los archivos en el *aplicación\_datos* carpeta.</span><span class="sxs-lookup"><span data-stu-id="e5e58-214">Although you would most typically see this behavior when using the web-based package manager interface, the same behavior occurs if you add, remove, or modify any files in the *App\_Data* folder.</span></span>
> 
> <span data-ttu-id="e5e58-215">**Solución alternativa** </span><span class="sxs-lookup"><span data-stu-id="e5e58-215">**Workaround** </span></span>  
> <span data-ttu-id="e5e58-216">Para trabajar con paquetes en Visual Studio, utilice la extensión de NuGet en lugar del Administrador de paquetes basada en web.</span><span class="sxs-lookup"><span data-stu-id="e5e58-216">To work with packages in Visual Studio, use the NuGet extension instead of the web-based package manager.</span></span> <span data-ttu-id="e5e58-217">Para obtener información, consulte el [documentación de NuGet](https://docs.microsoft.com/nuget/).</span><span class="sxs-lookup"><span data-stu-id="e5e58-217">For information, see the [NuGet documentation](https://docs.microsoft.com/nuget/).</span></span> <span data-ttu-id="e5e58-218">Si está trabajando con otros archivos en el *aplicación\_datos* carpeta, considere la posibilidad de mantener los archivos en otra parte, para evitar este problema.</span><span class="sxs-lookup"><span data-stu-id="e5e58-218">If you are working with other files in the *App\_Data* folder, consider keeping the files elsewhere to avoid this issue.</span></span> <span data-ttu-id="e5e58-219">Si no es práctico, elimine el *aplicación\_offline.htm* archivo manualmente o espere a que el sitio vuelve a conectarse automáticamente (de forma predeterminada, después de 30 segundos).</span><span class="sxs-lookup"><span data-stu-id="e5e58-219">If that's not practical, delete the *app\_offline.htm* file manually or wait until the site comes back online automatically (by default, after 30 seconds).</span></span>


#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a><span data-ttu-id="e5e58-220">Problema: Visual Studio IntelliSense y plantillas de proyectos disponibles sólo en ASP.NET MVC versión 3</span><span class="sxs-lookup"><span data-stu-id="e5e58-220">Issue: Visual Studio IntelliSense and project templates available only in ASP.NET MVC version 3</span></span>

> <span data-ttu-id="e5e58-221">Instalar ASP.NET Web Pages no también instalar tools para Visual Studio, como IntelliSense y plantillas de proyectos para aplicaciones de ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="e5e58-221">Installing ASP.NET Web Pages does not also install tools for Visual Studio such as IntelliSense and project templates for ASP.NET Web Pages applications.</span></span>
> 
> <span data-ttu-id="e5e58-222">**Solución alternativa** para usar IntelliSense y plantillas de proyectos para aplicaciones de ASP.NET Web Pages en Visual Studio, instalar RC de ASP.NET MVC 3 mediante el instalador de plataforma Web o el [instalador independiente](https://go.microsoft.com/fwlink/?LinkID=191797).</span><span class="sxs-lookup"><span data-stu-id="e5e58-222">**Workaround** To use IntelliSense and project templates for ASP.NET Web Pages applications in Visual Studio, install ASP.NET MVC 3 RC either through the Web Platform Installer or the [stand-alone installer](https://go.microsoft.com/fwlink/?LinkID=191797).</span></span>


#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a><span data-ttu-id="e5e58-223">Problema: Leer fuentes u otros datos externos a través de un servidor proxy</span><span class="sxs-lookup"><span data-stu-id="e5e58-223">Issue: Reading feeds or other external data via a proxy server</span></span>

> <span data-ttu-id="e5e58-224">Si el servidor que ejecuta el sitio está detrás de un servidor proxy, deberá configurar la información de proxy en el *web.config* archivo para poder leer la información que proviene de fuera de su sitio.</span><span class="sxs-lookup"><span data-stu-id="e5e58-224">If the server running the site is behind a proxy server, you might need to configure proxy information in the *web.config* file in order to be able to read information that comes from outside your site.</span></span> <span data-ttu-id="e5e58-225">Por ejemplo, si usa el `ReCaptcha` auxiliar, la aplicación auxiliar se comunica con el servicio de reCAPTCHA, pero podría estar bloqueada por el servidor proxy.</span><span class="sxs-lookup"><span data-stu-id="e5e58-225">For example, if you use the `ReCaptcha` helper, the helper communicates with the reCAPTCHA service, but might be blocked by your proxy server.</span></span> <span data-ttu-id="e5e58-226">De forma similar, las fuentes que se usan en ASP.NET Web Pages, como la fuente utilizada por el Administrador de paquetes, pueden requerir la configuración de proxy.</span><span class="sxs-lookup"><span data-stu-id="e5e58-226">Similarly, feeds that are used in ASP.NET Web Pages, such as the feed used by the package manager, might require proxy configuration.</span></span>
> 
> <span data-ttu-id="e5e58-227">Si experimenta problemas en trabajar con un servicio externo o trabajar con la fuente del paquete, colocar los elementos siguientes en la raíz de la aplicación *web.config* archivo:</span><span class="sxs-lookup"><span data-stu-id="e5e58-227">If you experience problems in working with an external service or working with the package feed, put the following elements into your application's root *web.config* file:</span></span>
> 
> [!code-xml[Main](overview/samples/sample2.xml)]
> 
> <span data-ttu-id="e5e58-228">Para obtener más información acerca de cómo configurar un servidor proxy, consulte [ &lt;proxy&gt; (configuración de red) del elemento](https://msdn.microsoft.com/library/sa91de1e.aspx) en el sitio Web de MSDN.</span><span class="sxs-lookup"><span data-stu-id="e5e58-228">For more information about configuring a proxy server, see [&lt;proxy&gt; Element (Network Settings)](https://msdn.microsoft.com/library/sa91de1e.aspx) on the MSDN Web site.</span></span>


#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="e5e58-229">Problema: Al desinstalar la versión 4 de .NET Framework deshabilita ASP.NET Web Pages con sintaxis Razor</span><span class="sxs-lookup"><span data-stu-id="e5e58-229">Issue: Uninstalling the .NET Framework version 4 disables ASP.NET Web Pages with Razor Syntax</span></span>

> <span data-ttu-id="e5e58-230">Si desinstala la versión 4 de .NET Framework y, a continuación, vuelva a instalarlo, se deshabilita ASP.NET Web Pages con sintaxis Razor.</span><span class="sxs-lookup"><span data-stu-id="e5e58-230">If you uninstall the .NET Framework version 4 and then reinstall it, ASP.NET Web Pages with Razor syntax is disabled.</span></span> <span data-ttu-id="e5e58-231">Las páginas con la *.cshtml* extensión no se ejecutan correctamente.</span><span class="sxs-lookup"><span data-stu-id="e5e58-231">Pages with the *.cshtml* extension do not run correctly.</span></span> <span data-ttu-id="e5e58-232">Páginas Web de ASP.NET, se registra un ensamblado en la raíz de la máquina *web.config* archivo y quitar .NET Framework se elimina el archivo.</span><span class="sxs-lookup"><span data-stu-id="e5e58-232">ASP.NET Web Pages registers an assembly in the machine root *web.config* file, and removing the .NET Framework removes that file.</span></span> <span data-ttu-id="e5e58-233">Volver a instalar .NET Framework instala una nueva versión del archivo de configuración, pero no agrega la referencia del ensamblado de ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="e5e58-233">Reinstalling the .NET Framework installs a new version of the configuration file, but does not add the reference for the ASP.NET Web Pages assembly.</span></span>
> 
> <span data-ttu-id="e5e58-234">**Solución alternativa** después de volver a instalar .NET Framework, reinstale ASP.NET Web Pages con sintaxis Razor.</span><span class="sxs-lookup"><span data-stu-id="e5e58-234">**Workaround** After reinstalling the .NET Framework, reinstall ASP.NET Web Pages with Razor syntax.</span></span> <span data-ttu-id="e5e58-235">Esto agrega el siguiente elemento a la *web.config* archivo en la raíz de la máquina, que se encuentra normalmente en la siguiente ubicación:</span><span class="sxs-lookup"><span data-stu-id="e5e58-235">This adds the following element to the *web.config* file in the machine root, which is typically in the following location:</span></span>  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](overview/samples/sample3.xml)]


#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a><span data-ttu-id="e5e58-236">Problema: Direcciones URL sin extensión no encuentra los archivos de.cshtml/.vbhtml en IIS 7 o IIS 7.5</span><span class="sxs-lookup"><span data-stu-id="e5e58-236">Issue: Extensionless URLs do not find .cshtml/.vbhtml files on IIS 7 or IIS 7.5</span></span>

> <span data-ttu-id="e5e58-237">En IIS 7 o IIS 7.5, las solicitudes con una dirección URL similar a la siguiente no están posible encontrar las páginas que tienen la *.cshtml* o *.vbhtml* extensión:</span><span class="sxs-lookup"><span data-stu-id="e5e58-237">On IIS 7 or IIS 7.5, requests with a URL like the following are not able to find pages that have the *.cshtml* or *.vbhtml* extension:</span></span>  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> <span data-ttu-id="e5e58-238">El problema surge porque la reescritura de URL no está habilitada de forma predeterminada para IIS 7 o IIS 7.5.</span><span class="sxs-lookup"><span data-stu-id="e5e58-238">The issue arises because URL rewriting is not enabled by default for IIS 7 or IIS 7.5.</span></span> <span data-ttu-id="e5e58-239">El escenario más probable es que no ve el problema al realizar pruebas localmente mediante IIS Express, pero la experiencia al implementar su sitio Web en un sitio Web de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="e5e58-239">The likeliest scenario is that you do not see the problem when testing locally using IIS Express, but you experience it when you deploy your website to a hosting website.</span></span>
> 
> <span data-ttu-id="e5e58-240">**Solución alternativa**</span><span class="sxs-lookup"><span data-stu-id="e5e58-240">**Workaround**</span></span>
> 
> - <span data-ttu-id="e5e58-241">Si tiene control sobre el equipo del servidor, en el equipo servidor instalar la actualización que se describe en [hay una actualización disponible que habilita ciertos controladores de IIS 7.0 o IIS 7.5 para controlar las solicitudes cuyas direcciones URL no termina con un punto](https://support.microsoft.com/kb/980368).</span><span class="sxs-lookup"><span data-stu-id="e5e58-241">If you have control over the server computer, on the server computer install the update that is described in [A update is available that enables certain IIS 7.0 or IIS 7.5 handlers to handle requests whose URLs do not end with a period](https://support.microsoft.com/kb/980368).</span></span>
> - <span data-ttu-id="e5e58-242">Si no tiene control sobre el equipo del servidor (por ejemplo, va a implementar en un sitio de hospedaje Web), agregue lo siguiente del sitio Web *web.config* archivo:</span><span class="sxs-lookup"><span data-stu-id="e5e58-242">If you do not have control over the server computer (for example, you are deploying to a hosting website), add the following to the website's *web.config* file:</span></span> 
> 
>     [!code-xml[Main](overview/samples/sample4.xml)]


#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a><span data-ttu-id="e5e58-243">Problema: Implementar una aplicación en un equipo que no tiene SQL Server Compact instalado</span><span class="sxs-lookup"><span data-stu-id="e5e58-243">Issue: Deploying an application to a computer that does not have SQL Server Compact installed</span></span>

> <span data-ttu-id="e5e58-244">Las aplicaciones que incluyen bases de datos de SQL Server Compact pueden ejecutar en un equipo donde SQL Server Compact no está instalado.</span><span class="sxs-lookup"><span data-stu-id="e5e58-244">Applications that include SQL Server Compact databases can run on a computer where SQL Server Compact is not installed.</span></span> <span data-ttu-id="e5e58-245">Microsoft WebMatrix 1.0 automáticamente copia estos archivos binarios para usted y realiza adecuado *web.config* transformaciones de archivos.</span><span class="sxs-lookup"><span data-stu-id="e5e58-245">Microsoft WebMatrix 1.0 automatically copies these binaries for you and performs the appropriate *web.config* file transforms.</span></span>
> 
> <span data-ttu-id="e5e58-246">**Solución alternativa** si tiene que copiar estos archivos y realizar el *web.config* cambios en los archivos manualmente, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="e5e58-246">**Workaround** If you need to copy these files and make the *web.config* file changes manually, do the following:</span></span>
> 
> 1. <span data-ttu-id="e5e58-247">Copie los ensamblados del motor de base de datos a la *Bin* carpeta (y las subcarpetas) de la aplicación en el equipo de destino:</span><span class="sxs-lookup"><span data-stu-id="e5e58-247">Copy the database engine assemblies to the *Bin* folder (and subfolders) of the application on the target computer:</span></span>  
> 
>    - <span data-ttu-id="e5e58-248">Copia *C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* </span><span class="sxs-lookup"><span data-stu-id="e5e58-248">Copy *C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* </span></span>  
>        <span data-ttu-id="e5e58-249">**to** *\Bin*</span><span class="sxs-lookup"><span data-stu-id="e5e58-249">**to** *\Bin*</span></span>
>    - <span data-ttu-id="e5e58-250">Copia <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\</em><strong><em>a</em></strong>\Bin\x86\*</span><span class="sxs-lookup"><span data-stu-id="e5e58-250">Copy <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\</em><strong><em>to</em></strong>\Bin\x86\*</span></span>
>    - <span data-ttu-id="e5e58-251">Copia <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\</em>\* <strong>a</strong><em>\Bin\amd64</em></span><span class="sxs-lookup"><span data-stu-id="e5e58-251">Copy <em>C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\</em>\* <strong>to</strong><em>\Bin\amd64</em></span></span>
> 
> 2. <span data-ttu-id="e5e58-252">En la carpeta raíz del sitio Web, cree o abra un *web.config* archivo.</span><span class="sxs-lookup"><span data-stu-id="e5e58-252">In the root folder of the website, create or open a *web.config* file.</span></span> <span data-ttu-id="e5e58-253">(En WebMatrix 1.0, está disponible si hace clic en este tipo de archivo **todas** en el **elegir un tipo de archivo** cuadro de diálogo.)</span><span class="sxs-lookup"><span data-stu-id="e5e58-253">(In WebMatrix 1.0, this file type is available if you click **All** in the **Choose a File Type** dialog box.)</span></span>
> 3. <span data-ttu-id="e5e58-254">Agregue el siguiente elemento como elemento secundario de la `<configuration>` elemento (no está dentro de la `<system.web>` elemento):</span><span class="sxs-lookup"><span data-stu-id="e5e58-254">Add the following element as a child of the `<configuration>` element (not inside the `<system.web>` element):</span></span>
> 
>     [!code-xml[Main](overview/samples/sample5.xml)]


#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a><span data-ttu-id="e5e58-255">Problema: "Database" y "WebGrid" aplicaciones auxiliares no funcionan en el nivel de confianza medio en Visual Basic</span><span class="sxs-lookup"><span data-stu-id="e5e58-255">Issue: "Database" and "WebGrid" helpers do not work in Medium Trust in Visual Basic</span></span>

> <span data-ttu-id="e5e58-256">Si está utilizando Visual Basic (crear *.vbhtml* archivos), el `Database` y `WebGrid` aplicaciones auxiliares no funcionará si la aplicación está configurada para usar el nivel de confianza medio.</span><span class="sxs-lookup"><span data-stu-id="e5e58-256">If you are using Visual Basic (creating *.vbhtml* files), the `Database` and `WebGrid` helpers will not work if the application is set to use Medium Trust.</span></span>
> 
> <span data-ttu-id="e5e58-257">**Solución alternativa**</span><span class="sxs-lookup"><span data-stu-id="e5e58-257">**Workaround**</span></span>  
> <span data-ttu-id="e5e58-258">Si usa Visual Studio 2010, puede resolver este problema mediante la instalación de la versión del Service Pack 1.</span><span class="sxs-lookup"><span data-stu-id="e5e58-258">If you use Visual Studio 2010, you can resolve this problem by installing the Service Pack 1 release.</span></span> <span data-ttu-id="e5e58-259">Hasta que esté disponible la versión final de la versión SP1, puede descargar la versión Beta de SP1 desde el [Microsoft Visual Studio 2010 Service Pack 1 Beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) página en Microsoft Download Center.</span><span class="sxs-lookup"><span data-stu-id="e5e58-259">Until the final version of the SP1 release is available, you can download the Beta version of SP1 from the [Microsoft Visual Studio 2010 Service Pack 1 Beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) page on the Microsoft Download Center.</span></span>   
>   
> <span data-ttu-id="e5e58-260">Si esto no es práctico, o si no usa Visual Studio 2010, temporalmente puede establece la aplicación para usar la plena confianza.</span><span class="sxs-lookup"><span data-stu-id="e5e58-260">If this is not practical, or if you do not use Visual Studio 2010, you can temporarily set the application to use Full Trust.</span></span>


#### <a name="issue-applicationpart-resources-are-externally-accessible"></a><span data-ttu-id="e5e58-261">Problema: "ApplicationPart" recursos son accesibles desde el exterior</span><span class="sxs-lookup"><span data-stu-id="e5e58-261">Issue: "ApplicationPart" resources are externally accessible</span></span>

> <span data-ttu-id="e5e58-262">Si un ensamblado contiene objetos que se deriva el `ApplicationPart` clase, que se exponen los recursos del ensamblado por el `ResourceRouteHandler` clase.</span><span class="sxs-lookup"><span data-stu-id="e5e58-262">If an assembly contains objects that derives from the `ApplicationPart` class, that assembly's resources are exposed by the `ResourceRouteHandler` class.</span></span> <span data-ttu-id="e5e58-263">Pongamos como ejemplo esta URL:</span><span class="sxs-lookup"><span data-stu-id="e5e58-263">For example, consider the following URL:</span></span>  
>   
> `~/r.ashx/System.Web.WebPages.Administration/Resources/AdminResources.resources`  
>   
> <span data-ttu-id="e5e58-264">Esta solicitud de descarga todas las cadenas de recursos en el *System.Web.WebPages.Administration.dll* ensamblado.</span><span class="sxs-lookup"><span data-stu-id="e5e58-264">This request downloads all of the resource strings in the *System.Web.WebPages.Administration.dll* assembly.</span></span> <span data-ttu-id="e5e58-265">Se descargan todos los recursos incrustados (incluso aquellos que no están diseñados para ser servido como contenido estático).</span><span class="sxs-lookup"><span data-stu-id="e5e58-265">All of the embedded resources (even those that are not intended to be served as static content) are downloaded.</span></span> <span data-ttu-id="e5e58-266">Si los recursos incrustados contienen información confidencial, esto puede representar un riesgo de seguridad.</span><span class="sxs-lookup"><span data-stu-id="e5e58-266">If the embedded resources contain sensitive information, this can represent a security risk.</span></span> 
> 
> <span data-ttu-id="e5e58-267">**Solución alternativa** </span><span class="sxs-lookup"><span data-stu-id="e5e58-267">**Workaround** </span></span>  
> <span data-ttu-id="e5e58-268">Si creas un **ApplicationPart** , asegúrese de que los recursos incrustados asociada con la que **ApplicationPart** ensamblado del objeto no contienen información confidencial.</span><span class="sxs-lookup"><span data-stu-id="e5e58-268">If you create an **ApplicationPart** object, make sure that the embedded resources associated with that **ApplicationPart** object's assembly do not contain sensitive information.</span></span>


<a id="Known_Issues_WebMatrix"></a>

### <a name="webmatrix"></a><span data-ttu-id="e5e58-269">WebMatrix</span><span class="sxs-lookup"><span data-stu-id="e5e58-269">WebMatrix</span></span>

> [!NOTE]
> <span data-ttu-id="e5e58-270">Para obtener información acerca de problemas de instalación de WebMatrix, vea [problemas de instalación de WebMatrix](#Known_Issues_Installation) anteriormente en este documento.</span><span class="sxs-lookup"><span data-stu-id="e5e58-270">For information about installation issues for WebMatrix, see [WebMatrix Installation Issues](#Known_Issues_Installation) earlier in this document.</span></span>


<span data-ttu-id="e5e58-271">Esta sección del documento describen problemas conocidos para el entorno de desarrollo de WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="e5e58-271">This section of the document describes known issues for the WebMatrix development environment.</span></span>

#### <a name="issue-changes-in-the-username-or-password-of-a-database-connection-string-in-a-webconfig-file-are-not-reflected-in-the-databases-workspace"></a><span data-ttu-id="e5e58-272">Problema: No se reflejan los cambios en el nombre de usuario o contraseña de una cadena de conexión de base de datos en un archivo web.config en el área de trabajo de bases de datos</span><span class="sxs-lookup"><span data-stu-id="e5e58-272">Issue: Changes in the username or password of a database connection string in a web.config file are not reflected in the Databases workspace</span></span>

> <span data-ttu-id="e5e58-273">**Solución alternativa**</span><span class="sxs-lookup"><span data-stu-id="e5e58-273">**Workaround**</span></span>  
> 
> 1. <span data-ttu-id="e5e58-274">En el *web.config* de archivo, cambie el nombre de la base de datos en la cadena de conexión (por ejemplo, agregue "1" a él).</span><span class="sxs-lookup"><span data-stu-id="e5e58-274">In the *web.config* file, change the database name in the connection string (for example, add "1" to it).</span></span>
> 2. <span data-ttu-id="e5e58-275">Guardar el *web.config* archivo.</span><span class="sxs-lookup"><span data-stu-id="e5e58-275">Save the *web.config* file.</span></span>
> 3. <span data-ttu-id="e5e58-276">Haga clic en **bases de datos** y actualizar.</span><span class="sxs-lookup"><span data-stu-id="e5e58-276">Click **Databases** and refresh.</span></span>
> 4. <span data-ttu-id="e5e58-277">Cambiar el nombre de la base de datos en la cadena de conexión en el *web.config* archivo por el nombre de base de datos original.</span><span class="sxs-lookup"><span data-stu-id="e5e58-277">Change the database name in the connection string in the *web.config* file back to the original database name.</span></span>
> 5. <span data-ttu-id="e5e58-278">Guardar el *web.config* archivo.</span><span class="sxs-lookup"><span data-stu-id="e5e58-278">Save the *web.config* file.</span></span>
> 6. <span data-ttu-id="e5e58-279">Haga clic en **bases de datos** y actualizar.</span><span class="sxs-lookup"><span data-stu-id="e5e58-279">Click **Databases** and refresh.</span></span>


#### <a name="issue-folders-created-by-webmatrix-cannot-be-deleted"></a><span data-ttu-id="e5e58-280">Problema: No se puede eliminar las carpetas creadas mediante WebMatrix</span><span class="sxs-lookup"><span data-stu-id="e5e58-280">Issue: Folders created by WebMatrix cannot be deleted</span></span>

> <span data-ttu-id="e5e58-281">Si WebMatrix se ejecuta con permisos elevados (es decir, Introducción al uso de WebMatrix el **ejecutar como administrador** opción en Windows), no se puede eliminar carpetas que se crean mediante WebMatrix mediante el Explorador de Windows.</span><span class="sxs-lookup"><span data-stu-id="e5e58-281">If WebMatrix is running using elevated permissions (that is, you started WebMatrix using the **Run as Administrator** option in Windows), folders that are created by WebMatrix cannot be deleted using Windows Explorer.</span></span>
> 
> <span data-ttu-id="e5e58-282">**Solución alternativa**</span><span class="sxs-lookup"><span data-stu-id="e5e58-282">**Workaround**</span></span>  
> <span data-ttu-id="e5e58-283">Ejecute el Explorador de Windows con permisos elevados.</span><span class="sxs-lookup"><span data-stu-id="e5e58-283">Run Windows Explorer using elevated permissions.</span></span> <span data-ttu-id="e5e58-284">Siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="e5e58-284">Follow these steps:</span></span>  
> 
> 1. <span data-ttu-id="e5e58-285">En Windows, haga clic en **iniciar**.</span><span class="sxs-lookup"><span data-stu-id="e5e58-285">In Windows, click **Start**.</span></span>
> 2. <span data-ttu-id="e5e58-286">Escriba "Explorador de Windows" y haga clic en la entrada de **Windows Explorer**.</span><span class="sxs-lookup"><span data-stu-id="e5e58-286">Enter "Windows Explorer" and right-click the entry for **Windows Explorer**.</span></span>
> 3. <span data-ttu-id="e5e58-287">Haga clic en **ejecutar como administrador**.</span><span class="sxs-lookup"><span data-stu-id="e5e58-287">Click **Run as Administrator**.</span></span> <span data-ttu-id="e5e58-288">A continuación, puede eliminar las carpetas.</span><span class="sxs-lookup"><span data-stu-id="e5e58-288">You can then delete the folders.</span></span>


#### <a name="issue-webmatrix-10-is-unable-to-perform-certain-tasks-that-require-elevation"></a><span data-ttu-id="e5e58-289">Problema: WebMatrix 1.0 no puede realizar ciertas tareas que requieren la elevación</span><span class="sxs-lookup"><span data-stu-id="e5e58-289">Issue: WebMatrix 1.0 is unable to perform certain tasks that require elevation</span></span>

> <span data-ttu-id="e5e58-290">WebMatrix 1.0 no puede realizar ciertas tareas que requieren la elevación, como la instalación de componentes adicionales en las situaciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="e5e58-290">WebMatrix 1.0 is unable to perform certain tasks that require elevation, such as installing additional components in the following situations:</span></span>
> 
> - <span data-ttu-id="e5e58-291">En Windows Vista o Windows 7, que ha iniciado sesión con una cuenta que no tiene privilegios administrativos y Control de cuentas de usuario (UAC) está deshabilitada.</span><span class="sxs-lookup"><span data-stu-id="e5e58-291">On Windows Vista or Windows 7, you are logged in with an account that does not have administrative privileges and User Account Control (UAC) is disabled.</span></span>
> - <span data-ttu-id="e5e58-292">Está utilizando Microsoft Windows XP o Microsoft Windows Server 2003.</span><span class="sxs-lookup"><span data-stu-id="e5e58-292">You are using Microsoft Windows XP or Microsoft Windows Server 2003.</span></span>
> 
> <span data-ttu-id="e5e58-293">**Solución alternativa**</span><span class="sxs-lookup"><span data-stu-id="e5e58-293">**Workaround**</span></span>  
> <span data-ttu-id="e5e58-294">Mayoría de las tareas en WebMatrix 1.0 no requiere permisos administrativos.</span><span class="sxs-lookup"><span data-stu-id="e5e58-294">Most tasks in WebMatrix 1.0 do not require administrative permission.</span></span> <span data-ttu-id="e5e58-295">Para aquellos que lo hacen, puede realizar la operación porque un administrador o siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="e5e58-295">For those that do, you can perform the operation as an administrator, or follow these steps:</span></span>
> 
> - <span data-ttu-id="e5e58-296">En Windows Vista o Windows 7, habilitar UAC.</span><span class="sxs-lookup"><span data-stu-id="e5e58-296">On Windows Vista or Windows 7, enable UAC.</span></span>
> - <span data-ttu-id="e5e58-297">En Windows XP, agregue el usuario al grupo de seguridad Administradores.</span><span class="sxs-lookup"><span data-stu-id="e5e58-297">On Windows XP, add the user to the Administrators security group.</span></span>


#### <a name="issue-site-from-web-gallery-is-disabled"></a><span data-ttu-id="e5e58-298">Problema: "Sitio de la galería Web" está deshabilitada</span><span class="sxs-lookup"><span data-stu-id="e5e58-298">Issue: "Site from Web Gallery" is disabled</span></span>

> <span data-ttu-id="e5e58-299">El **sitio desde galería Web** opción está deshabilitada si el instalador de plataforma Web 3.0 no está instalado.</span><span class="sxs-lookup"><span data-stu-id="e5e58-299">The **Site from Web Gallery** option is disabled if the Web Platform Installer 3.0 is not installed.</span></span>
> 
> <span data-ttu-id="e5e58-300">**Solución alternativa**</span><span class="sxs-lookup"><span data-stu-id="e5e58-300">**Workaround**</span></span>  
> <span data-ttu-id="e5e58-301">Instalar el [instalador de plataforma Web de Microsoft 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="e5e58-301">Install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span>


#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a><span data-ttu-id="e5e58-302">Problema: Google Chrome no está disponible como una opción de ejecución</span><span class="sxs-lookup"><span data-stu-id="e5e58-302">Issue: Google Chrome is not available as a Run option</span></span>

> <span data-ttu-id="e5e58-303">Google Chrome no se muestra en la lista de exploradores en **ejecutar** en el **inicio** ficha.</span><span class="sxs-lookup"><span data-stu-id="e5e58-303">Google Chrome is not displayed in the list of browsers under **Run** on the **Home** tab.</span></span>
> 
> <span data-ttu-id="e5e58-304">**Solución alternativa**</span><span class="sxs-lookup"><span data-stu-id="e5e58-304">**Workaround**</span></span>  
> <span data-ttu-id="e5e58-305">Algunas versiones de Google Chrome no se registran correctamente con la característica de programas predeterminados en Windows.</span><span class="sxs-lookup"><span data-stu-id="e5e58-305">Some versions of Google Chrome do not register themselves correctly with the Default Programs feature in Windows.</span></span> <span data-ttu-id="e5e58-306">Como alternativa, inicie Google Chrome, haga clic en el *control Google Chrome y personalizar* menú, haga clic en *opciones*y, a continuación, haga clic en *Make Google Chrome mi explorador predeterminado*.</span><span class="sxs-lookup"><span data-stu-id="e5e58-306">As a workaround, start Google Chrome, click the *Customize and control Google Chrome* menu, click *Options*, and then click *Make Google Chrome my default browser*.</span></span>


#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a><span data-ttu-id="e5e58-307">Problema: El cuadro de diálogo "Foreign Key" no permite escribir una clave principal</span><span class="sxs-lookup"><span data-stu-id="e5e58-307">Issue: The "Foreign Key" dialog box doesn't allow entering a primary key</span></span>

> <span data-ttu-id="e5e58-308">El **Foreign Key** cuadro de diálogo no permite especificar el nombre de clave principal de la tabla de clave principal.</span><span class="sxs-lookup"><span data-stu-id="e5e58-308">The **Foreign Key** dialog box does not allow you to enter the primary key name from the primary key table.</span></span>
> 
> <span data-ttu-id="e5e58-309">**Solución alternativa**</span><span class="sxs-lookup"><span data-stu-id="e5e58-309">**Workaround**</span></span>  
> <span data-ttu-id="e5e58-310">Esto tiene su porqué.</span><span class="sxs-lookup"><span data-stu-id="e5e58-310">This is intentional.</span></span> <span data-ttu-id="e5e58-311">No es necesario que escriba el nombre de la clave principal de la tabla de clave principal.</span><span class="sxs-lookup"><span data-stu-id="e5e58-311">You do not need to enter the name of the primary key from the primary key table.</span></span>


#### <a name="issue-intellisense-is-not-available-in-webmatrix-for-razor-syntax-c-or-visual-basic"></a><span data-ttu-id="e5e58-312">Problema: IntelliSense no está disponible en WebMatrix para Razor sintaxis, C# o Visual Basic</span><span class="sxs-lookup"><span data-stu-id="e5e58-312">Issue: IntelliSense is not available in WebMatrix for Razor syntax, C#, or Visual Basic</span></span>

> <span data-ttu-id="e5e58-313">IntelliSense se admite en WebMatrix para HTML y CSS.</span><span class="sxs-lookup"><span data-stu-id="e5e58-313">IntelliSense is supported in WebMatrix for HTML and CSS.</span></span> <span data-ttu-id="e5e58-314">Sin embargo, no está disponible para otros lenguajes.</span><span class="sxs-lookup"><span data-stu-id="e5e58-314">However, it is not available for other languages.</span></span> 
> 
> <span data-ttu-id="e5e58-315">**Solución alternativa** </span><span class="sxs-lookup"><span data-stu-id="e5e58-315">**Workaround** </span></span>  
> <span data-ttu-id="e5e58-316">Ninguno.</span><span class="sxs-lookup"><span data-stu-id="e5e58-316">None.</span></span>


#### <a name="issue-intellisense-for-html-and-css-suggests-elements-that-are-not-contextually-appropriate"></a><span data-ttu-id="e5e58-317">Problema: IntelliSense para HTML y CSS sugiere los elementos que no son adecuados contextualmente</span><span class="sxs-lookup"><span data-stu-id="e5e58-317">Issue: IntelliSense for HTML and CSS suggests elements that are not contextually appropriate</span></span>

> <span data-ttu-id="e5e58-318">IntelliSense para el marcado en WebMatrix es compatible con HTML mediante la [esquema XHTML 1.0 Transitional](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) y el uso de CSS el [esquema CSS 2.1](http://www.w3.org/TR/CSS2/).</span><span class="sxs-lookup"><span data-stu-id="e5e58-318">IntelliSense for markup in WebMatrix supports HTML using the [XHTML 1.0 Transitional schema](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) and CSS using the [CSS 2.1 schema](http://www.w3.org/TR/CSS2/).</span></span> <span data-ttu-id="e5e58-319">Dado que IntelliSense se basa en estos esquemas específicos, determinadas etiquetas, atributos o propiedades podrían sugiere que no son adecuadas para la definición de estilo o la página actual.</span><span class="sxs-lookup"><span data-stu-id="e5e58-319">Because IntelliSense is based on these specific schemas, certain tags, attributes, or properties might be suggested that are not appropriate for the current page or style definition.</span></span> <span data-ttu-id="e5e58-320">Para HTML, también puede conducir a sugerencias inesperadas en el contenido que podría interpretarse como XHTML con formato incorrecto (por ejemplo, cuando no se cierran las etiquetas).</span><span class="sxs-lookup"><span data-stu-id="e5e58-320">For HTML, it can also lead to unexpected suggestions in content that might be interpreted as malformed XHTML (for example, when tags are not closed).</span></span> <span data-ttu-id="e5e58-321">Este problema podría ser más evidente si el punto de inserción está dentro de una etiqueta incompleta; en ese caso, IntelliSense puede sugerir nuevas etiquetas de apertura u ofrecen otras sugerencias incorrectos.</span><span class="sxs-lookup"><span data-stu-id="e5e58-321">This issue might be more noticeable if the insertion point is inside an incomplete tag; in that case, IntelliSense might suggest new opening tags or offer other incorrect suggestions.</span></span> 
> 
> <span data-ttu-id="e5e58-322">**Solución alternativa** </span><span class="sxs-lookup"><span data-stu-id="e5e58-322">**Workaround** </span></span>  
> <span data-ttu-id="e5e58-323">Para HTML, asegúrese de que está trabajando dentro de una página XHTML completa tiene el formato correcto.</span><span class="sxs-lookup"><span data-stu-id="e5e58-323">For HTML, make sure that you are working within a well-formed, complete XHTML page.</span></span> <span data-ttu-id="e5e58-324">No hay ninguna solución alternativa para CSS.</span><span class="sxs-lookup"><span data-stu-id="e5e58-324">For CSS, there is no workaround.</span></span>


#### <a name="issue-intellisense-is-not-invoked-while-you-type"></a><span data-ttu-id="e5e58-325">Problema: No se invoca IntelliSense mientras escribe</span><span class="sxs-lookup"><span data-stu-id="e5e58-325">Issue: IntelliSense is not invoked while you type</span></span>

> <span data-ttu-id="e5e58-326">En ocasiones, no se puede invocar IntelliSense como HTML o CSS se está entrando en el editor.</span><span class="sxs-lookup"><span data-stu-id="e5e58-326">At times, IntelliSense might not be invoked as HTML or CSS is being entered in the editor.</span></span> <span data-ttu-id="e5e58-327">En concreto, esto puede ocurrir cuando el punto de inserción está directamente al lado de otro elemento o al final de un archivo.</span><span class="sxs-lookup"><span data-stu-id="e5e58-327">In particular, this might happen when the insertion point is directly next to another element or at the end of a file.</span></span> 
> 
> <span data-ttu-id="e5e58-328">**Solución alternativa** </span><span class="sxs-lookup"><span data-stu-id="e5e58-328">**Workaround** </span></span>  
> <span data-ttu-id="e5e58-329">Asegúrese de que hay espacio en blanco alrededor del punto de inserción y que no es el punto de inserción al final de un archivo.</span><span class="sxs-lookup"><span data-stu-id="e5e58-329">Make sure that there is whitespace around the insertion point and that the insertion point is not at the end of a file.</span></span> <span data-ttu-id="e5e58-330">También puede invocar IntelliSense manualmente presionando CTRL+BARRA ESPACIADORA.</span><span class="sxs-lookup"><span data-stu-id="e5e58-330">You can also invoke IntelliSense manually by pressing Ctrl+Space.</span></span>


#### <a name="issue-no-ui-is-available-for-disabling-intellisense"></a><span data-ttu-id="e5e58-331">Problema: no hay ninguna interfaz de usuario para deshabilitar IntelliSense</span><span class="sxs-lookup"><span data-stu-id="e5e58-331">Issue: No UI is available for disabling IntelliSense</span></span>

> <span data-ttu-id="e5e58-332">WebMatrix 1.0 se proporciona ninguna interfaz de usuario o de gestos para deshabilitar IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="e5e58-332">WebMatrix 1.0 provides no UI or gesture for disabling IntelliSense.</span></span> 
> 
> <span data-ttu-id="e5e58-333">**Solución alternativa** </span><span class="sxs-lookup"><span data-stu-id="e5e58-333">**Workaround** </span></span>  
> <span data-ttu-id="e5e58-334">Inicie WebMatrix mediante el comando siguiente, que incluye un modificador que deshabilita IntelliSense:</span><span class="sxs-lookup"><span data-stu-id="e5e58-334">Start WebMatrix using the following command, which includes a switch that disables IntelliSense:</span></span>  
>   
> `WebMatrix.exe #ExecuteCommand# EditorIntelliSense off`


<a id="Known_Issues_IISExpress"></a>
### <a name="iis-express"></a><span data-ttu-id="e5e58-335">IIS Express</span><span class="sxs-lookup"><span data-stu-id="e5e58-335">IIS Express</span></span>

<span data-ttu-id="e5e58-336">IIS Express tiene su propio archivo Léame, que está disponible en la siguiente URL:</span><span class="sxs-lookup"><span data-stu-id="e5e58-336">IIS Express has its own readme file, which is available at the following URL:</span></span>

[<span data-ttu-id="e5e58-337">https://go.microsoft.com/fwlink/?LinkID=207675&amp; clcid = 0 x 409</span><span class="sxs-lookup"><span data-stu-id="e5e58-337">https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409</span></span>](https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409)

<a id="Known_Issues_SQLServerCompact"></a>

### <a name="sql-server-compact"></a><span data-ttu-id="e5e58-338">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="e5e58-338">SQL Server Compact</span></span>

<span data-ttu-id="e5e58-339">SQL Server Compact tiene su propio archivo Léame, que está disponible en la siguiente URL:</span><span class="sxs-lookup"><span data-stu-id="e5e58-339">SQL Server Compact has its own readme file, which is available at the following URL:</span></span>

[https://go.microsoft.com/fwlink/?LinkID=208545](https://go.microsoft.com/fwlink/?LinkID=208545&amp;clcid=0x409)

<span data-ttu-id="e5e58-340">Para obtener información acerca de los problemas que implican la instalación de SQL Server Compact como parte de WebMatrix, consulte [problemas de instalación de WebMatrix](#Known_Issues_Installation) anteriormente en este documento.</span><span class="sxs-lookup"><span data-stu-id="e5e58-340">For information about issues that involve installing SQL Server Compact as part of WebMatrix, see [WebMatrix Installation Issues](#Known_Issues_Installation) earlier in this document.</span></span>

### <a id="Known_Issues_Installing_Applications"></a>  <span data-ttu-id="e5e58-341">Instalación de aplicaciones</span><span class="sxs-lookup"><span data-stu-id="e5e58-341">Installing Applications</span></span>

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a><span data-ttu-id="e5e58-342">Problema: Instalar una aplicación puede tardar mucho tiempo si la carpeta Mis documentos del usuario se redirige a un recurso compartido de red</span><span class="sxs-lookup"><span data-stu-id="e5e58-342">Issue: Installing an application can take a long time if the user's My Documents folder is redirected to a network share</span></span>

> <span data-ttu-id="e5e58-343">**Solución alternativa**</span><span class="sxs-lookup"><span data-stu-id="e5e58-343">**Workaround**</span></span>  
> <span data-ttu-id="e5e58-344">Ninguno.</span><span class="sxs-lookup"><span data-stu-id="e5e58-344">None.</span></span> <span data-ttu-id="e5e58-345">La aplicación puede tardar un rato para instalar, pero se instalará correctamente.</span><span class="sxs-lookup"><span data-stu-id="e5e58-345">The application might take a while to install, but will install correctly.</span></span>


### <a id="Known_Issues_Publishing_Applications"></a>  <span data-ttu-id="e5e58-346">Publicación de aplicaciones</span><span class="sxs-lookup"><span data-stu-id="e5e58-346">Publishing Applications</span></span>

#### <a name="issue-required-permissions-cannot-be-acquired-error-when-publishing-a-sql-compact-database"></a><span data-ttu-id="e5e58-347">Problema: "requerido no se puede obtener permisos" error al publicar una base de datos SQL</span><span class="sxs-lookup"><span data-stu-id="e5e58-347">Issue: "Required permissions cannot be acquired" error when publishing a SQL Compact Database</span></span>

> <span data-ttu-id="e5e58-348">WebMatrix admite totalmente la implementación archivos binarios compatibles para SQL Server Compact a un servidor que se está ejecutando la versión 3.5 de .NET Framework con una configuración de nivel de confianza medio.</span><span class="sxs-lookup"><span data-stu-id="e5e58-348">WebMatrix does not fully support deploying supporting binaries for SQL Server Compact to a server that is running .NET Framework version 3.5 with a medium trust configuration.</span></span>
> 
> <span data-ttu-id="e5e58-349">**Solución alternativa**</span><span class="sxs-lookup"><span data-stu-id="e5e58-349">**Workaround**</span></span>  
> <span data-ttu-id="e5e58-350">Es la solución preferida instalar .NET Framework 4 en el servidor.</span><span class="sxs-lookup"><span data-stu-id="e5e58-350">The preferred workaround is to install the .NET Framework 4 on the server.</span></span> <span data-ttu-id="e5e58-351">Como alternativa, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="e5e58-351">Alternatively, do the following:</span></span>
> 
> 1. <span data-ttu-id="e5e58-352">Agregue los siguientes elementos para el `SecurityClasses` sección *Web\_MediumTrust.config* archivo:</span><span class="sxs-lookup"><span data-stu-id="e5e58-352">Add the following elements to the `SecurityClasses` section in *Web\_MediumTrust.config* file:</span></span>
> 
>     [!code-html[Main](overview/samples/sample6.html)]
> 2. <span data-ttu-id="e5e58-353">Crear un nuevo conjunto de permisos en el *Web\_MediumTrust.config* archivo con los siguientes permisos necesarios:</span><span class="sxs-lookup"><span data-stu-id="e5e58-353">Create a new permission set in the *Web\_MediumTrust.config* file with the following required permissions:</span></span>
> 
>     [!code-html[Main](overview/samples/sample7.html)]
> 3. <span data-ttu-id="e5e58-354">Se aplica el permiso establecido en SQL Server Compact colocando los siguientes elementos el *Web\_MediumTrust.config* archivo:</span><span class="sxs-lookup"><span data-stu-id="e5e58-354">Apply the permission set to SQL Server Compact by putting the following elements in the *Web\_MediumTrust.config* file:</span></span>
> 
>     [!code-html[Main](overview/samples/sample8.html)]


#### <a name="issue-gallery-and-phpbb-web-applications-display-a-service-is-unavailable-error-after-publishing"></a><span data-ttu-id="e5e58-355">Problema: Las aplicaciones web de galería y PhpBB muestran un error "Servicio no está disponible" después de la publicación</span><span class="sxs-lookup"><span data-stu-id="e5e58-355">Issue: Gallery and PhpBB web applications display a "Service is unavailable" error after publishing</span></span>

> <span data-ttu-id="e5e58-356">En algunas circunstancias, publicar una aplicación, produce un error "servicio no está disponible".</span><span class="sxs-lookup"><span data-stu-id="e5e58-356">Under some circumstances, publishing an application causes a "service is unavailable" error.</span></span>
> 
> <span data-ttu-id="e5e58-357">**Solución alternativa**</span><span class="sxs-lookup"><span data-stu-id="e5e58-357">**Workaround**</span></span>  
> <span data-ttu-id="e5e58-358">En WebMatrix, agregue una barra diagonal inversa (\) al final del nombre del servidor en el **configuración de publicación** ventana y, a continuación, publicar la aplicación de nuevo.</span><span class="sxs-lookup"><span data-stu-id="e5e58-358">In WebMatrix, add a backslash (\) to the end of the server name in the **Publish Settings** window and then publish the application again.</span></span>


#### <a name="issue-moodle-website-layout-and-links-are-broken-after-publishing"></a><span data-ttu-id="e5e58-359">Problema: Vínculos y el diseño del sitio Web de Moodle se interrumpen después de la publicación</span><span class="sxs-lookup"><span data-stu-id="e5e58-359">Issue: Moodle website layout and links are broken after publishing</span></span>

> <span data-ttu-id="e5e58-360">Después de publicar una aplicación de Moodle, la aplicación no funciona correctamente.</span><span class="sxs-lookup"><span data-stu-id="e5e58-360">After you publish a Moodle application, the application does not work correctly.</span></span>
> 
> <span data-ttu-id="e5e58-361">**Solución alternativa**</span><span class="sxs-lookup"><span data-stu-id="e5e58-361">**Workaround**</span></span>  
> <span data-ttu-id="e5e58-362">En WebMatrix, agregue una barra diagonal (/) al final de la **nombre del sitio** campo el **configuración de publicación** ventana y, a continuación, publicar la aplicación de nuevo.</span><span class="sxs-lookup"><span data-stu-id="e5e58-362">In WebMatrix, add a slash (/) to the end of the **Site Name** field in the **Publish Settings** window and then publish the application again.</span></span>


#### <a name="issue-publishing-nopcommerce-fails-with-a-database-error"></a><span data-ttu-id="e5e58-363">Problema: Publicación de nopCommerce se produce un error de base de datos</span><span class="sxs-lookup"><span data-stu-id="e5e58-363">Issue: Publishing nopCommerce fails with a database error</span></span>

> <span data-ttu-id="e5e58-364">Publicación nopCommerce se produce un error y notifica un error de base de datos, como "insertar en la instrucción nop\_tabla del registro de error."</span><span class="sxs-lookup"><span data-stu-id="e5e58-364">Publishing nopCommerce fails and reports a database error like "Insert into the nop\_log table failed."</span></span>
> 
> <span data-ttu-id="e5e58-365">**Solución alternativa**</span><span class="sxs-lookup"><span data-stu-id="e5e58-365">**Workaround**</span></span>  
> 
> 1. <span data-ttu-id="e5e58-366">En WebMatrix, haga clic en **ejecutar** para iniciar nopCommerce localmente.</span><span class="sxs-lookup"><span data-stu-id="e5e58-366">In WebMatrix, click **Run** to launch nopCommerce locally.</span></span>
> 2. <span data-ttu-id="e5e58-367">Inicie sesión en la página de administración.</span><span class="sxs-lookup"><span data-stu-id="e5e58-367">Log into the administration page.</span></span>
> 3. <span data-ttu-id="e5e58-368">Haga clic en el **sistema** menú.</span><span class="sxs-lookup"><span data-stu-id="e5e58-368">Click the **System** menu.</span></span>
> 4. <span data-ttu-id="e5e58-369">Haga clic en el **registro** opción.</span><span class="sxs-lookup"><span data-stu-id="e5e58-369">Click the **Log** option.</span></span>
> 5. <span data-ttu-id="e5e58-370">Haga clic en el **Vaciar registro** botón.</span><span class="sxs-lookup"><span data-stu-id="e5e58-370">Click the **Clear Log** button.</span></span>
> 6. <span data-ttu-id="e5e58-371">Vuelva a publicar nopCommerce.</span><span class="sxs-lookup"><span data-stu-id="e5e58-371">Publish nopCommerce again.</span></span>


#### <a name="issue-silverstripe-cms-displays-a-http-500-php-fcgi-error-when-you-download-a-published-site"></a><span data-ttu-id="e5e58-372">Problema: Silverstripe CMS muestra un "Error FCGI PHP HTTP 500" al descargar un sitio publicado</span><span class="sxs-lookup"><span data-stu-id="e5e58-372">Issue: Silverstripe CMS displays a "HTTP 500 PHP FCGI Error" when you download a published site</span></span>

> <span data-ttu-id="e5e58-373">**Solución alternativa**</span><span class="sxs-lookup"><span data-stu-id="e5e58-373">**Workaround**</span></span>  
> <span data-ttu-id="e5e58-374">Tras hacer clic en **Descargar sitio publicado**, omitir `silverstripe-cache/manifest_main` en **vista previa de publicación**.</span><span class="sxs-lookup"><span data-stu-id="e5e58-374">After you click **Download published site**, skip `silverstripe-cache/manifest_main` in **Publish Preview**.</span></span> <span data-ttu-id="e5e58-375">Este archivo se usa para fines de almacenamiento en caché y es específico de cada equipo.</span><span class="sxs-lookup"><span data-stu-id="e5e58-375">This file is used for caching purposes and is specific to each computer.</span></span>


#### <a name="issue-subtext-displays-server-error-in--application-when-you-download-a-published-site"></a><span data-ttu-id="e5e58-376">Problema: Subtext muestra "Error de servidor en la aplicación '/'" cuando se descarga un sitio publicado</span><span class="sxs-lookup"><span data-stu-id="e5e58-376">Issue: Subtext displays "Server Error in '/' Application" when you download a published site</span></span>

> <span data-ttu-id="e5e58-377">**Solución alternativa**</span><span class="sxs-lookup"><span data-stu-id="e5e58-377">**Workaround**</span></span>  
> <span data-ttu-id="e5e58-378">Abra la carpeta del sitio *web.config* de archivo y reemplace el identificador de usuario y contraseña en la cadena de conexión de base de datos con las credenciales de administrador de SQL Server (las credenciales "sa").</span><span class="sxs-lookup"><span data-stu-id="e5e58-378">Open the site's *web.config* file and replace the user ID and password in the database connection string with the SQL Server administrator credentials (the "sa" credentials).</span></span>
> 
> <span data-ttu-id="e5e58-379">Como alternativa, siga estos pasos para conceder a la cuenta de usuario que ha iniciado sesión con `db_owner` permisos:</span><span class="sxs-lookup"><span data-stu-id="e5e58-379">Alternatively, follow these steps in order to give the user account you are logged in with `db_owner` permissions:</span></span>
> 
> 1. <span data-ttu-id="e5e58-380">Instalar SQL Server Management Studio mediante el instalador de plataforma Web.</span><span class="sxs-lookup"><span data-stu-id="e5e58-380">Install SQL Server Management Studio using the Web Platform Installer.</span></span>
> 2. <span data-ttu-id="e5e58-381">Conéctese a la instancia local de SQL Server Express (de forma predeterminada, `.\SQLEXPRESS`).</span><span class="sxs-lookup"><span data-stu-id="e5e58-381">Connect to the local SQL Server Express instance (by default, `.\SQLEXPRESS`).</span></span>
> 3. <span data-ttu-id="e5e58-382">Haga clic en **bases de datos** &gt; *[localSubtextDatabase]* &gt; **seguridad** &gt; **usuarios** &gt; *[localSubtextUser*] (valor predeterminado es `subtextuser`], con el botón secundario y haga clic en **propiedades**.</span><span class="sxs-lookup"><span data-stu-id="e5e58-382">Click **Databases** &gt; *[localSubtextDatabase]* &gt; **Security** &gt; **Users** &gt; *[localSubtextUser*] (default is `subtextuser`], right-click, and click **Properties**.</span></span>
> 4. <span data-ttu-id="e5e58-383">Seleccione **db\_propietario** en la sección de la pertenencia al rol.</span><span class="sxs-lookup"><span data-stu-id="e5e58-383">Select **db\_owner** in the role membership section.</span></span>


#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a><span data-ttu-id="e5e58-384">Problema: Sitio podría no funcionar después de publicar si el campo "Dirección URL de destino" no es el prefijo http:// o https://</span><span class="sxs-lookup"><span data-stu-id="e5e58-384">Issue: Site might not work after publishing if the "Destination URL" field is not prefixed with http:// or https://</span></span>

> <span data-ttu-id="e5e58-385">En el **configuración de publicación** cuadro de diálogo, si la dirección URL de destino no comienza con `http://` o `https://`, el sitio no funcionen después de la implementación.</span><span class="sxs-lookup"><span data-stu-id="e5e58-385">In the **Publishing Settings** dialog box, if the destination URL does not begin with `http://` or `https://`, the site might not work after deployment.</span></span>
> 
> <span data-ttu-id="e5e58-386">**Solución alternativa**</span><span class="sxs-lookup"><span data-stu-id="e5e58-386">**Workaround**</span></span>  
> <span data-ttu-id="e5e58-387">Asegúrese de que antes de publicar un sitio, la dirección URL de destino en el **configuración de publicación** inicia el cuadro de diálogo con `http://` o `https://`.</span><span class="sxs-lookup"><span data-stu-id="e5e58-387">Make sure that before you publish a site, the destination URL in the **Publish Settings** dialog box starts with `http://` or `https://`.</span></span>


#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a><span data-ttu-id="e5e58-388">Problema: Publicar una base de datos MySQL produce el error "no se pudo publicar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="e5e58-388">Issue: Publishing a MySQL database fails with the error "Failed to publish the database.</span></span> <span data-ttu-id="e5e58-389">Esto puede ocurrir si la base de datos remoto no puede ejecutar la secuencia de comandos."</span><span class="sxs-lookup"><span data-stu-id="e5e58-389">This can happen if the remote database cannot run the script."</span></span>

> <span data-ttu-id="e5e58-390">El error puede producirse por una serie de motivos.</span><span class="sxs-lookup"><span data-stu-id="e5e58-390">The error can occur for a number of reasons.</span></span> <span data-ttu-id="e5e58-391">Una razón que ve este error es si la secuencia de comandos de base de datos contiene un carácter de comillas simples (') y el juego de caracteres de destino MySQL la base de datos predeterminado no es UTF-8.</span><span class="sxs-lookup"><span data-stu-id="e5e58-391">One reason you can see this error is if the database script contains a single quotation character (') and the destination MySQL database's default character set is not to UTF-8.</span></span>
> 
> <span data-ttu-id="e5e58-392">**Solución alternativa**</span><span class="sxs-lookup"><span data-stu-id="e5e58-392">**Workaround**</span></span>  
> <span data-ttu-id="e5e58-393">Establece el carácter predeterminado establecido para la base de datos MySQL remota en UTF-8.</span><span class="sxs-lookup"><span data-stu-id="e5e58-393">Set the default character set for the remote MySQL database to UTF-8.</span></span>


#### <a name="issue-some-links-are-not-visible-in-dotnetnuke-after-publishing-or-downloading-the-site"></a><span data-ttu-id="e5e58-394">Problema: Algunos vínculos no están visibles en DotNetNuke tras publicar o descargar del sitio</span><span class="sxs-lookup"><span data-stu-id="e5e58-394">Issue: Some links are not visible in DotNetNuke after publishing or downloading the site</span></span>

> <span data-ttu-id="e5e58-395">Si publicar o descargar un sitio de DotNetNuke, es posible que deba borrar la memoria caché para obtener los nuevos vínculos que aparecen en el sitio.</span><span class="sxs-lookup"><span data-stu-id="e5e58-395">If you publish or download a DotNetNuke site, you might need to clear the cache to get the new links to appear on the site.</span></span>
> 
> <span data-ttu-id="e5e58-396">**Solución alternativa**</span><span class="sxs-lookup"><span data-stu-id="e5e58-396">**Workaround**</span></span>
> 
> 1. <span data-ttu-id="e5e58-397">Inicie sesión como "Host".</span><span class="sxs-lookup"><span data-stu-id="e5e58-397">Log in as "Host".</span></span>
> 2. <span data-ttu-id="e5e58-398">Vaya al menú de host y seleccione **configuración de Host**.</span><span class="sxs-lookup"><span data-stu-id="e5e58-398">Go to the host menu and select **Host Settings**.</span></span>
> 3. <span data-ttu-id="e5e58-399">Desplácese hacia abajo y en **configuración avanzada**, expanda **configuración de rendimiento**.</span><span class="sxs-lookup"><span data-stu-id="e5e58-399">Scroll down and under **Advanced Settings**, expand **Performance Settings**.</span></span>
> 4. <span data-ttu-id="e5e58-400">Haga clic en el **Borrar caché** vínculo para las páginas.</span><span class="sxs-lookup"><span data-stu-id="e5e58-400">Click the **Clear Cache** link for pages.</span></span>
> 5. <span data-ttu-id="e5e58-401">Vaya a la parte inferior de la página y reiniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e5e58-401">Go to the bottom of the page and restart the application.</span></span>


#### <a name="issue-some-links-in-atomsite-are-broken-after-you-download-a-published-site"></a><span data-ttu-id="e5e58-402">Problema: Algunos vínculos de AtomSite se interrumpen después de descargar un sitio publicado</span><span class="sxs-lookup"><span data-stu-id="e5e58-402">Issue: Some links in AtomSite are broken after you download a published site</span></span>

> <span data-ttu-id="e5e58-403">**Solución alternativa**</span><span class="sxs-lookup"><span data-stu-id="e5e58-403">**Workaround**</span></span>  
> <span data-ttu-id="e5e58-404">En el *service.config* archivo, *users.config* archivo y todos los *.xml* archivos, reemplace la cadena de dirección URL (por ejemplo, `http://myhost.com/atomsite`) con locales (por ejemplo, `http://localhost:1239`).</span><span class="sxs-lookup"><span data-stu-id="e5e58-404">In the *service.config* file, *users.config* file, and all *.xml* files, replace the URL string (for example, `http://myhost.com/atomsite`) with the local one (for example, `http://localhost:1239`).</span></span>


#### <a name="issue-mysql-based-applications-like-wordpress-fail-to-publish-and-report-a-database-error"></a><span data-ttu-id="e5e58-405">Problema: Aplicaciones basadas en MySQL como WordPress no se pueden publicar y notificar un error de base de datos</span><span class="sxs-lookup"><span data-stu-id="e5e58-405">Issue: MySQL-based applications like WordPress fail to publish and report a database error</span></span>

> <span data-ttu-id="e5e58-406">De forma predeterminada, WebMatrix instala MySQL con el juego de caracteres UTF-8.</span><span class="sxs-lookup"><span data-stu-id="e5e58-406">By default, WebMatrix installs MySQL with the UTF-8 character set.</span></span> <span data-ttu-id="e5e58-407">Si instala MySQL en su propio y el juego de caracteres no es UTF-8 (por ejemplo, es Latin1), podría producir un error en el proceso de publicación para las bases de datos.</span><span class="sxs-lookup"><span data-stu-id="e5e58-407">If you install MySQL on your own, and the character set is not UTF-8 (for example, it is Latin1), the publish process for databases might fail.</span></span>
> 
> <span data-ttu-id="e5e58-408">**Solución alternativa**</span><span class="sxs-lookup"><span data-stu-id="e5e58-408">**Workaround**</span></span>
> 
> 1. <span data-ttu-id="e5e58-409">Cambiar el juego de caracteres de MySQL a UTF-8.</span><span class="sxs-lookup"><span data-stu-id="e5e58-409">Change the character set for MySQL to UTF-8.</span></span> <span data-ttu-id="e5e58-410">(Para obtener más información, consulte [intercalación y juego de caracteres de servidor](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) en el sitio Web de MySQL.)</span><span class="sxs-lookup"><span data-stu-id="e5e58-410">(For details, see [Server Character Set and Collation](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) on the MySQL website.)</span></span>
> 2. <span data-ttu-id="e5e58-411">Vuelva a instalar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e5e58-411">Reinstall the application.</span></span>
> 3. <span data-ttu-id="e5e58-412">Vuelva a publicar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="e5e58-412">Republish the application.</span></span>


#### <a name="issue-download-published-site-fails-for-applications-that-have-browser-based-setup"></a><span data-ttu-id="e5e58-413">Problema: "Sitio publicado de descarga" se produce un error para las aplicaciones que tienen el programa de instalación basada en explorador</span><span class="sxs-lookup"><span data-stu-id="e5e58-413">Issue: "Download published site" fails for applications that have browser-based setup</span></span>

> <span data-ttu-id="e5e58-414">Algunas aplicaciones (por ejemplo, Kentico CMS) requieren que se inicie en el explorador con el fin de realizar la configuración posterior a la instalación como la creación de una base de datos.</span><span class="sxs-lookup"><span data-stu-id="e5e58-414">Some applications (for example, Kentico CMS) require you to launch them in the browser in order to perform post-installation setup such as creating a database.</span></span> <span data-ttu-id="e5e58-415">Si publica una aplicación similar al siguiente sin completar la instalación basada en explorador, intenta descargar el mismo sitio desde un servidor remoto generará un error.</span><span class="sxs-lookup"><span data-stu-id="e5e58-415">If you publish an application like this without completing the browser-based setup, attempting to download the same site from a remote server will fail.</span></span>
> 
> <span data-ttu-id="e5e58-416">**Solución alternativa**</span><span class="sxs-lookup"><span data-stu-id="e5e58-416">**Workaround**</span></span>  
> <span data-ttu-id="e5e58-417">Finalizar la instalación basada en explorador antes de publicar el sitio.</span><span class="sxs-lookup"><span data-stu-id="e5e58-417">Finish browser-based setup before publishing the site.</span></span>


#### <a name="issue-download-published-site-fails-with-a-database-error-for-dotnetnuke-and-kooboo-cms"></a><span data-ttu-id="e5e58-418">Problema: "Sitio publicado de descarga" se produce un error de base de datos de DotNetNuke y Kooboo CMS</span><span class="sxs-lookup"><span data-stu-id="e5e58-418">Issue: "Download published site" fails with a database error for DotNetNuke and Kooboo CMS</span></span>

> <span data-ttu-id="e5e58-419">Si se intenta descargar una aplicación de un servidor y tener credenciales de administrador en la cadena de conexión de base de datos en el **configuración de publicación** cuadro de diálogo, es posible que vea el siguiente error en el registro de la publicación:</span><span class="sxs-lookup"><span data-stu-id="e5e58-419">If you try to download an application from a server and you have administrator credentials in the database connection string in the **Publish Settings** dialog, you might see the following error in the publish log:</span></span>
> 
> [!code-console[Main](overview/samples/sample9.cmd)]
> 
> <span data-ttu-id="e5e58-420">**Solución alternativa**</span><span class="sxs-lookup"><span data-stu-id="e5e58-420">**Workaround**</span></span>  
> <span data-ttu-id="e5e58-421">Si es posible, vuelva a publicar el sitio (o publicarlo) con las credenciales sin privilegios de administrador para la base de datos.</span><span class="sxs-lookup"><span data-stu-id="e5e58-421">If practical, republish the site (or have it published) using non-administrator credentials for the database.</span></span>


<a id="More_Info"></a>

## <a name="for-more-information"></a><span data-ttu-id="e5e58-422">Para obtener más información</span><span class="sxs-lookup"><span data-stu-id="e5e58-422">For More Information</span></span>

<span data-ttu-id="e5e58-423">Para obtener más información acerca de WebMatrix 1.0, consulte los siguientes sitios Web:</span><span class="sxs-lookup"><span data-stu-id="e5e58-423">For more information about WebMatrix 1.0, see the following websites:</span></span>

- [<span data-ttu-id="e5e58-424">IIS.net</span><span class="sxs-lookup"><span data-stu-id="e5e58-424">IIS.net</span></span>](http://iis.net/)
- [<span data-ttu-id="e5e58-425">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e5e58-425">ASP.NET</span></span>](https://asp.net/webmatrix)
- [<span data-ttu-id="e5e58-426">Microsoft.com/web</span><span class="sxs-lookup"><span data-stu-id="e5e58-426">Microsoft.com/web</span></span>](https://www.microsoft.com/web)

<span data-ttu-id="e5e58-427">© 2011 Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="e5e58-427">© 2011 Microsoft Corporation.</span></span> <span data-ttu-id="e5e58-428">Reservados todos los derechos.</span><span class="sxs-lookup"><span data-stu-id="e5e58-428">All Rights Reserved.</span></span> <span data-ttu-id="e5e58-429">[Términos de uso](https://msdn.microsoft.cos/cc300389.aspx).</span><span class="sxs-lookup"><span data-stu-id="e5e58-429">[Terms of Use](https://msdn.microsoft.cos/cc300389.aspx).</span></span>
